<div style='text-align: justify;'>

<div id='top'/>

### Summary

###### [00 - Daily Scrum](#Scrum)

###### [01 -  doc reading](#Doc)

###### [02 - Creation of a resource group](#RG)

###### [03 - Creating the pipeline](#Pipeline)

###### [04 - Installation of Trivy](#Trivy)

###### [05 - Installation of OWASP Zap](#Owasp)

###### [06 - Using Trivy and OWASP Zap with Azure DevOps Pipelines](#T&OwtADP)

###### [0 - Usefull Commands](#UsefullCommands)

<div id='Scrum'/>  

### **Scrum quotidien**

Scrum Master = Me, myself and I
Daily personnal reactions with reports and designations of first tasks for the day.

Frequent meeting with other coworkers to study solutions to encountered problems together.

[scrums](https://github.com/simplon-lerouxDunvael/Brief_7/blob/main/Plans_et_demarches/Scrum.md)

[&#8679;](#top)

<div id='Docs'/>  

#### **doc reading**

Researches and reading of documentations to determine the needed prerequisites, functionnalities and softwares to complete the different tasks of Brief 9.

[&#8679;](#top)  

<div id='RG'/>  

### **Creation of a resource group**

I created a resource group via the file script .sh (created beforehand). It was supposed to install nginx as well but I had an error message. it seemed that the https link (https://helm.nginx.com/stable) from nginx official site was not valid anymore.

After exchanging with Joffrey, i went to [artifacthub](https://artifacthub.io/packages/search?ts_query_web=nginx&sort=relevance&page=1) as he advised. This site is updated Bitnami with the current versions and links.

I searched for [nginx](https://artifacthub.io/packages/helm/bitnami/nginx) and updated my code with the right url. Then I updated the commands to install nginx on the namespaces with the right repo name (created before).

![nginx_install_changes](https://user-images.githubusercontent.com/108001918/233959236-d5cd8517-96c3-4ae8-8c24-79d6e430b957.png)

It still did not work. Therefore I created variables for the namespaces' names and put them in the commands to install nginx in each namespace. Then I updated the commands to extract the external IP addresses of each namespace (to put them in the DNS records smoothie-prod and smoothie-qua).

Nginx was now properly installed on both namespaces. I updated the DNS records IP addresses with the ones displaying thanks to my script.

Then i downloaded the kubeconfig file to update it in my GitHub repository :

```Bash
download kubeconfig.yaml
```

When i tried to connect to the https url I had an error message. With the external IP address an Nginx page was displayed instead of the voting app. 

![nginx_page](https://user-images.githubusercontent.com/108001918/234233153-a0ba440f-a147-464c-86fc-3c1a42d6764e.png)

Therefore, after deleting my infrastructure, I tried to install Nginx with the commands found on `https://artifacthub.io/packages/helm/ingress-nginx/ingress-nginx`.

The issue was still the same, my certificates were in TRUE status, my external IP linked to the correct DNS name however I still had an error message when using the url to connect.

![error_page-dns](https://user-images.githubusercontent.com/108001918/234233788-13e21955-9b3a-401d-9f97-2bdf7cbbe3ae.png)

After exchanging with Quentin, we found out that the nginx from artifacthub was without configuration therefore did not install all the requirements we needed. As it would be difficult and time consuming to create a configuration and apply it, I cleared my broswer's cache and used the same commands and config I had from the start (https://helm.nginx.com/stable).

I changed the let's encrypt (issuer) server to be able to request certificates and it solved my issue. My infrastructure is now deployed and I can connect in https to the azure voting app.

[&#8679;](#top)

<div id='Pipeline'/>  

### **Creating the pipeline**

First I went to Azure DevOps, created a project and on Pipelines : <https://dev.azure.com/dlerouxext/b9duna/_build>.

Then, I had to configure my organization and project's [visibility](https://learn.microsoft.com/en-us/azure/devops/organizations/projects/make-project-public?view=azure-devops). I went to the settings and turned on the visibility to public.

Since the last update of Kubernetes, the connection to Azure can't be made with the service connections.  
Therefore, I had to create a kubeconfig file that recovers several connections informations.

```Bash
az aks get-credentials --resource-group $rgname --name $aksname -f kubeconfig.yaml
```

Then I had to download it and place it directly in my Git repository (downloading it from azure terminal does not push it into Git) :

```Bash
download kubeconfig.yaml
```

Once downloaded, I just had to put the code into the Kubernetes service connection (choosing autoConfig params) to be able to use my pipeline and Kubernetes services. I also added a Docker connection so my pipeline could access my Docker registry and Voting app image.

![service_connection](https://user-images.githubusercontent.com/108001918/234296946-6a3e6e96-8d3c-43a6-bb98-13f73570440c.png)

Then I created a new GitHub repository and copy/pasteed all the files from the previous repository from brief 8 : azVotingApp_b8duna and named it azVotingApp_b9duna.

Finally i used the pipeline that was created during brief 8.

The pipeline is constructed in a specific order :

* First I declared the variables that would be used several times
* Then I created a job to Build and Push the Docker Image into my Docker repository previously created
* Once the docker image was created, I deployed it (in a canary way) to the qua namespace and checked that the voting app responded well with a curl (deployment.yaml)
* Then as it worked, I deployed it (also in a canary way) to the prod namespace and checked that the voting app responded well with a curl too (deployment-canary.yaml)
* Then I used a bash script to check the replicas created (2 for prod, 1 for qua). This way, I knew that in the namespace prod, there were one voting app with the old version and one with the new version. The cluster IP would manage the users between the pods.
* Then I promoted the new version to all the pods in the prod namespace (deployment.yaml)
* As the checks were successful, I deleted the canary deployment from the prod namespace (deployment-canary.yaml)

[&#8679;](#top)

<div id='Trivy'/>  

### **Installation of Trivy**

First, I went to [Trivy Github](https://github.com/aquasecurity/trivy-azure-pipelines-task) and downloaded Trivy on the [Azure DevOps Market](https://marketplace.visualstudio.com/items?itemName=AquaSecurityOfficial.trivy-official) (by clicking on *Get it for Free*) to link it to my Azure DevOps account. Once it was installed I could use Trivy tasks on my pipeline.

![trivy_download](https://user-images.githubusercontent.com/108001918/234309975-4560cc33-49a0-432d-aa18-49e16d0198c1.png)

[&#8679;](#top)

<div id='Owasp'/>  

### **Installation of OWASP Zap**

First, I downloaded OWASP Zap on the [Azure DevOps Market](https://marketplace.visualstudio.com/items?itemName=CSE-DevOps.zap-scanner) (by clicking on *Get it for Free*) to link it to my Azure DevOps account. Once it was installed I could use OWASP Zap tasks on my pipeline.

![owaspzap_download](https://user-images.githubusercontent.com/108001918/234544420-87d0869f-1168-45f2-8f72-d974195dc9ff.png)

[&#8679;](#top)

<div id='T&OwtADP'/>  

### **Using Trivy and OWASP Zap with Azure DevOps Pipelines**

**Trivy :**

In order to ask Trivy to scan my Docker image, I created a trivy task and completed the configuration needed.
Then in order to extract the output and gets the metrics, I created a repository at the root of my GitHub repo to store the reports and then extract the temporary output of Trivy before it is destroyed. As a professionnal Aqua account is needed to get the output for Trivy, extracting the temporary output before it is send to Aqua is the only way to get the metrics.

After creating the repository Reports, I created a README.md file so that the output would be directed and pushed into the directory (without any file in it not push is made).

For the needs of the demonstration, and because Trivy detects about 80+K lines of issues with a severity "high" or higher, and because the docker image used for the webapp cannot be changed, the information will be acknowledged but ignored.

**OWASP Zap :**

In order to ask OWASP Zap to test my Azure Voting App, I created a owaspzap task and completed the configuration needed.

I run the OWASP Zap scanner against the app (after it has been deployed to qua) and publish the OWASP Zap report as a build artifact.

OWASP ZAP Scan supposedly has an option to define the output but all of them failed. It appears that it is a common issue according to the reviews of OWASP ZAP Scan, it is a current bug that may or may not be fixed in the future.

![owasp_nofile](https://user-images.githubusercontent.com/108001918/234587089-15960667-c46c-4ba4-a9ad-b2ce30793780.png)

For the needs of the demonstration, and because OWASP ZAP Scan still detects vulnerabilities, and because the docker image used for the webapp cannot be changed, the information will be acknowledged but ignored.

![owasp_nofile2](https://user-images.githubusercontent.com/108001918/234592211-e6c28936-da10-425a-9224-8a0da0501831.png)

[&#8679;](#top)

<div id='Admin'/>  

### **Prompting administrator to continue or not the pipeline after a failure status from tests**

In order to prompt administrator to continue or not the pipeline after a failure status from tests it is necessary to use a manual validation task. All the information on how to configure and implement it in Azure DevOps Pipelines come from :

* <https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/manual-validation-v0?view=azure-pipelines>

* <https://stackoverflow.com/questions/56176773/how-to-add-a-manual-intervention-step-in-azure-pipelines-yaml>

Once implemented, the administrator can click on `Review` during the pipeline process and choose wheither to continue or stop the pipeline.

[&#8679;](#top)

<div id='UsefullCommands'/>  

### **USEFULL COMMANDS**

### **To clone and pull a GitHub repository**

```bash
git clone [GitHubRepositoryURL]
```

```bash
git pull
```

[&#8679;](#top)

### **To create an alias for a command on azure CLi**

alias [WhatWeWant]="[WhatIsChanged]"  

*Example :*  

```bash
alias k="kubectl"
```

[&#8679;](#top)

### **To deploy resources with yaml file**

kubectl apply -f [name-of-the-yaml-file]

*Example :*  

```bash
kubectl apply -f azure-vote.yaml
```

[&#8679;](#top)

### **To check resources**

```bash
kubectl get nodes
kubectl get pods
kubectl get services
kubectl get deployments
kubectl get events
kubectl get secrets
kubectl get logs
```

*To keep verifying the resources add --watch at the end of the command :*

*Example :*

```bash
kubectl get services --watch
```

*To check the resources according to their namespace, add --namespace after the command and the namespace's name :*

*Example :*

```bash
kubectl get services --namespace [namespace's-name]
```

[&#8679;](#top)

### **To describe resources**

```bash
kubectl describe nodes
kubectl describe pods
kubectl describe services # or svc
kubectl describe deployment # or deploy
kubectl describe events
kubectl describe secrets
kubectl describe logs
```

*To specify which resource needs to be described just put the resource ID at the end of the command.*

*Example :*

```bash
kubectl describe svc redis-service
```

*To access to all the logs from all containers :*

```bash
kubectl logs podname --all-containers
```

*To access to the logs from a specific container :*

```bash
kubectl logs podname -c [container's-name]
```

*To list all events from a specific pod :*

```bash
kubectl get events --field-selector [involvedObject].name=[podsname]
```

[&#8679;](#top)

### **To delete resources**

```bash
kubectl delete deploy --all
kubectl delete svc --all
kubectl delete pvc --all
kubectl delete pv --all
az group delete --name [resourceGroupName] --yes --no-wait
```

[&#8679;](#top)

### **To check TLS certificate in request order**

```bash
kubectl get certificate
kubectl get certificaterequest
kubectl get order
kubectl get challenge
```

[&#8679;](#top)

### **To describe TLS certificate in request order**

```bash
kubectl describe certificate
kubectl describe certificaterequest
kubectl describe order
kubectl describe challenge
```

[&#8679;](#top)

### **Get the IP address to point the DNS to nginx in the two namespaces**

```bash
kubectl get svc --all-namespaces
```

[&#8679;](#top)

</div>

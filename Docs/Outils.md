<div style='text-align: justify;'>

<div id='top'/>

### Summary

###### [01 -  SonarQube](#Sonar)

###### [02 - OWASP Dependency-Check](#OWASP1)

###### [03 - Clair](#Clair)

###### [04 - Trivy](#Trivy)

###### [05 - Grype](#Grype)

###### [06 - OWASP Zap](#OWASP2)

###### [06 - Conclusion](#Conclusion)

<div id='Sonar'/>  

### **SonarQube**

Avantages:
SonarQube est une plateforme complète qui peut analyser plus de 27 langages de programmation.
SonarQube est facile à intégrer à un pipeline de développement.
Il offre des fonctionnalités de test de sécurité statiques et dynamiques, ainsi que des vérifications de conformité et de qualité de code.
La plateforme propose également une vue complète de la santé du projet, y compris une vue d'ensemble des vulnérabilités.
Inconvénients:

SonarQube peut parfois donner des faux positifs.
Le processus d'installation et de configuration de SonarQube peut être long et fastidieux.

<div id='OWAS1'/>  

### **OWASP Dependency-Check**

OWASP Dependency-Check est une solution de test de sécurité des dépendances open source.
Il peut détecter les vulnérabilités dans les bibliothèques tierces.
OWASP Dependency-Check est facile à installer et à utiliser.
Inconvénients:

Il peut produire un grand nombre de faux positifs.
OWASP Dependency-Check ne peut détecter que les vulnérabilités connues.

<div id='Clair'/>  

### **Clair**

Avantages:
Clair est un outil de test de sécurité des conteneurs open source.
Il peut analyser les images de conteneurs pour détecter les vulnérabilités connues.
Clair est facile à installer et à utiliser.
Inconvénients:

Il ne peut pas détecter les vulnérabilités inconnues.
Clair ne prend pas en charge tous les types de conteneurs.

<div id='Trivy'/>  

### **Trivy**

Avantages:
Trivy est un outil de test de sécurité des conteneurs open source.
Il peut détecter les vulnérabilités connues et inconnues dans les images de conteneurs.
Trivy prend en charge plusieurs types de conteneurs.
Inconvénients:

Le temps d'exécution de Trivy peut être long sur les images de conteneurs volumineuses.
Il ne peut pas détecter les vulnérabilités dans les systèmes d'exploitation.

<div id='Grype'/>  

### **Grype**

Avantages:
Grype est un outil de test de sécurité des conteneurs open source.
Il peut détecter les vulnérabilités connues et inconnues dans les images de conteneurs.
Grype prend en charge plusieurs types de conteneurs.
Inconvénients:

Il ne prend pas en charge toutes les fonctionnalités de Docker, comme les manifestes de plateforme.
Il peut y avoir des problèmes de compatibilité avec les anciennes versions de Python.

<div id='OWASP2'/>  

### **OWASP Zap**

Avantages:
OWASP Zap est un outil de test de sécurité open source pour les applications web.
Il offre une variété de fonctionnalités de test de sécurité, telles que les attaques de force brute, les injections de SQL et les attaques XSS.
OWASP Zap est facile à installer et à utiliser.
Inconvénients:

Il peut produire un grand nombre de faux positifs.
OWASP Zap peut être difficile à configurer pour les utilisateurs inexpérimentés.

<div id='Conclusion'/>  

### **Conclusion**

Chaque outil de test de sécurité automatisé a ses avantages et inconvénients. Le choix de l'outil dépend des besoins spécifiques de chaque projet et de l'environnement de développement. 


[&#8679;](#top)

</div>

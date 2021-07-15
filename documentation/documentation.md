# Todo-list app: Documentation

### A propos

<a id="#about" />

Todo list app est une application web qui permet de gérer une liste de choses à faire (todo).  
Cette application ne concentre que les fonctionnalités fondamentales d'une todo.  
Le but de l'étude de cette app étant surtout un moyen de se familiariser avec le pattern MVC, l'écriture de test fonctionnnels avec Jasmine ainsi que réaliser un audit de performance sur une application similaire déjà implémenter.

#### L'architecture de l'application

Le pattern MVC repose sur la division du coeur de l'application en 3 parties distinctes ayant chacune des responsabilités différentes:

Modèle
: Contient les données à afficher.

Vue
: Contient la présentation de l'interface graphique.

Controller
: Contient la logique concernant les actions effectuées par l'utilisateur

Dans le cas de Todo-list, les différentes fonctionnalités de l'application repose sur ces 3 blocs.  
Pour les aider dans leur tâche respective, d'autres modules ont été créé en renfort des 3 principaux:

| Modules supplémentaire | Roles                                                                                                                                                              | Utilisé par le module principal |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------- |
| **app**                | Contient l'instanciation des 3 modules principaux ainsi que la fonction qui permet le lancement de l'application <br /> C'est le point d'entrée dans l'application | -                               |
| **helpers**            | Contient des fonctions custom permettant de sélectionner des éléments du DOM, d'automatiser la mise en place du même event listener sur une liste d'élément <br /> | view<br/> app                   |
| **template**           | Contient les templates prédéfinis qui peuvent insérer dynamiquement dès que nécessaire.                                                                            | view                            |
| **store**              | Contient la logique permettant les opérations CRUD sur le local storage                                                                                            | model                           |

**DIAGRAMME MVC DE L'APP **

### Les différentes fonctionnalités

- ajouter une nouvelle tâche
- supprimer une tâche
- toggler une tâche
- marquer une tâche comme complété
- filtrer par status

### Audit

**LIEN VERS WIKI**

# Todo-list app: Documentation

## Sommaire
1. [A propos](#about)
2. [Fonctionnalités](#features)
   1. [Ajouter un nouveau todo](#add)
   2. [Editer un todo](#edit)
   3. [Supprimer un todo](#remove)
   4. [Marquer un todo comme "Complété"](#toggle)
   5. [Filtrer la liste de todo](#filter)
3. [Référence](#reference)
4. [Audit](#audit)

---- 
## A propos

<a id="#about" />

Todo list app est une application web qui permet de gérer une liste de choses à faire (todo).  
Cette application ne concentre que les fonctionnalités fondamentales d'une todo.  
Le but de l'étude de cette app étant surtout un moyen de se familiariser avec le pattern MVC, l'écriture de test fonctionnnels avec Jasmine ainsi que réaliser un audit de performance sur une application similaire déjà implémenter.

#### L'architecture de l'application

Le pattern MVC repose sur la division du coeur de l'application en 3 parties distinctes ayant chacune des responsabilités différentes:

![diagramme MVC](./img/diagramme-mvc.png)

Le flow dans un pattern MVC suit donc un certain ordre:

1. l'utilisateur réalise une action quelconque sur l'interface  
    (_ex : valide le texte qu'il a saisi dans un formulaire_).
2. La "View", qui contient principalement de l'HTML,  détecte l'action qu'il notifie au "Controller"
3. Le "Controller" contient la logique à éxecuter lorsque certaines actions lui sont communiquées par la View.  
 (_ex: l'utilisateur a saisie quelque chose, il faut sauvegarder ce nouvel élément_)    
Il va transmettre les bonnes instructions au "Model" pour que les données soient correctement traités.  

4. Le "Model" contient la logique pour intéragir avec la base de donnée.  
   (_ex: écrire le nouvel élément dans la base de donnée_)  
   Une fois l'opération réussie, il peut notifier le Controller que tout s'est bien déroulé.

5. Le Controller voit que l'objectif du Model est atteint, il demande à la View de se mettre à jour avec les nouvelles données.

6. L'utilisateur voit l'interface répondre correctement à son action.

  
Dans le cas de notre application Todo-list, le code remplissant le rôle de ces 3 différents blocs se trouvent dans 3 fichiers distincts:
    - Controller.js
    - Model.js
    - View.js  
    
Des fichiers supplémentaires viennent compléter ceux présentés précédemment:

- helpers.js: Contient des variables custom qui permettent une syntaxe plus succinte (ex: $qs au lieu de querySelector) et qui permet d'attacher des gestionnaires d'événement de façon plus flexible que tradionnellement (ex : $delegate). Ces variables sont disponibles globalement et peuvent donc être utilisés par tous les fichiers présents.
  
- app.js : est la porte d'entrée dans l'application. C'est ici que les informations essentielles au lancement de l'application sont déclarées et initialisées (view, model, controller, le nom de la base de donnée...)
- store.js : Contient concrètement les instructions qu'il est possible d'effectuer sur la base de donnée.   
  Le Model, après vérification des données transmis par le Controller, utilise le code présent dans le store une fois pour compléter la tâche qui lui a été assignée.
- template.js: Contient les template des différents éléments qui viennent s'ajouter au fur et à mesure des interactions.  

**Important**: 
L'application à ce stade n'utilise pas de base de donnée à proprement parlé, mais enregistre les todo en local via local storage.

  

#### En résumé voici l'arborescence des fichiers:    
  
![arborescence de fichiers](./img/arborescence-js.jpg)

#### Diagramme expliquant la relation entre les différents fichiers
![diagramme et relations](./img/structure-relations.jpg)
<br >
<br >
<br >

---

<a id="features" />  

## Les différentes fonctionnalités

 

- Ajouter une nouvelle tâche 
   
![ajouter todo](./img/full-feature_ajouter.jpg)    
<br >
Description:   
[Controller.prototype.addItem](#addItem)  

L'ajout d'une nouvelle tâche est déclenchée lorsque l'utilisateur valide sa saisie depuis l'interface (appuie sur entrée).

Le Controller perçoit cette demande d'ajout comme un événement appelé "newTodo", reçoit ce que l'utilisateur souhaite ajouter sous forme d'une `String` , et execute alors les instructions écrites dans sa méthode **[addItem](#addItem)**:  

Demande au Model de créer un nouveau todo basé sur ce que l'utilisateur a écrit, l'enregistrer dans le localStorage, et d'éxécuter le callback, qui lui est passé en argument, une fois que la sauvegarde a été effectué.

Ce callback indique à la View ce qui doit être mis à jour dans l'UI suite à la création d'une nouvelle tâche

<br >
<br >  
  

- Editer une tâche  
  
  ![editer une todo](./img/full-feature_edit.jpg)
<br >  
Description:
[Controller.prototype.editItem](#editItem)

Cette méthode est appelé en réponse au double clique de l'utilisateur sur une tâche présente dans la liste.
Lors de cet événement "itemEdit", l'`id` lié cette tâche est passé à la méthode du Controller [editItem](#editItem).  
  
Lors de l'éxécution de cette méthode, `id` est passé au Model pour que la todo qui y est lié soit retrouvé et utilisable pour la suite grâce à [Model.read](#read) . En second argument, un callback éxécuté lorsque le bon todo a été retrouvé du localStorage.   
Lors de l'éxécution de callback les données retrouvées sont passés en argument à la vue via [View.render](#render).

<br >
<br >

- supprimer une tâche  
  
  ![supprimer une todo](./img/full-feature_supprimer.jpg)
<br >  

Description: [Controller.prototype.remove](#remove)  

Déclenché lors d'un événement "itemRemove".  
Reçoit alors le `id` de l'élément à supprimer.  
[Model.remove](#remove) est appelé, en utilisant `id` pour supprimer de localStorage le bon élément.  
La View est mis à jour une fois la suppression faite, via le callback passé en argument à Model.remove.  
Ce callback utilise View.render avec `removeItem`, avec `id` pour que l'interface soit correctement mis à jour. 
<br >
<br >

- Toggler une tâche  
  ![toggler une todo](./img/full-feature_toggle.jpg)
<br >
  
<a id="explain-toggleComplete">
Description: [Controller.prototype.toggleComplete](#toggleComplete)
Déclenché lors d'un événement "itemToggle". Reçoit alors un objet `item`, contenant l'id et le status de la tâche à toggler.  
[Controller.toggleComplete](#toggleComplete) se charge de fournir l'id à [Model.read](#read)

<br >
<br >

- Filtrer par status  
  ![filtrer par status](./img/full-feature_filtrer.jpg)
<br >

L'application se base sur des routes pour render les bons éléments. Une route étant consituée de l'url et d'une destination précédée d'un "#".  
Il exite 3 routes possibles: 
- localhost:_PORT_/#**all** ou **/**
- localhost:_PORT_/#**active**
- localhost:_PORT_/#**completed**

Trois boutons labellisés "All", "Active", "Completed" permettent à l'utilisateur d'accéder aux routes associées.

Pour que l'interface soit correctement affichée, trois méthodes sont utilisables: 

* Afficher toutes les tâches: [Controller.prototype.showAll](#showAll):  
* Afficher uniquement les tâches complétés: [Controller.prototype.showActive](#showActive)
* Afficher uniquement les tâches actives: [Controller.prototype.showCompleted](#showCompleted) 

Le Controller garde en permanence "en mémoire" la destination active (celle où se trouve actuellement l'utilisateur) et la destination précédente et ce depuis le tout premier lancement de l'application. (_Destination par défaut au lancement: "/", équivalent à "/all"_ ).    
De plus, chaque changement d'url déclenche [Controller.prototype.setView](#setView).     
Ce changement est dû à l'utilisation d'_anchored link_ (d'ou le "#" dans l'url)

A chaque changement d'url **setView** est appelé, execute un appel à [_filter](#filter), ce qui à terme met à jour les données de la page.  

Description: [Controller.prototype._filter](#filter)    
Cette méthode s'appuie sur la destination active en la comparant à la destination précédente (est ce qu'il y au eu un changement de destination depuis le dernier appel de _filter? ) pour que la bonne méthode parmi show[all|active|completed] soit appelée, et ainsi afficher les bons éléments dans l'interface.

Le principe de fonctionnement est le suivant : 
1. L'utilisateur se trouve sur la page listant toutes les tâches ('/#all'), il ne souhaite voir que celle déjà complétées: il clique sur le bouton "Completed".
2. setView est déclenché car changement d'url (maintenant '/#completed'), ce qui va permettre d'executer _filter. Il compare alors la derniere route active('all') à la route active("completed").
3. "completed" !== "all" donc _filter fait appel à showCompleted.
4. L'utilisateur voit l'interface avec la liste de todo complété.

<br >
<br >

- Tout marquer comme complété  
  
![tout complete](./img/full-feature_tout-complete.jpg)
<br > 

Description: [Controller.prototype.toggleAll](#toggleAll)  

Cette méthode est executée lorsqu'un évènement de type "toggleAll" est reçu par le Controller, autrement dit lorsque l'utilisateur utilise la checkbox .toggleClass.
Lorsque cela se produit, la valeur de l'attribut "checked" (true si la case est cochée, false sinon) est passé en argument.
[Model.read](#read) est appelé avec pour arguments un objet contenant la valeur **contraire** de la checkbox, et un callback executé lorsque le model aura terminé son travail.   
En passant la valeur contraire de la checkbox(true alors false est passé) comme critère de recherche au Model, on s'assure que seul les tâches correspondantes (les tâches ayant leur statut "completed" égal à false) seront retournés du localStorage, et non pas toute l'entièreté de la liste.  

Ce callback va permettre d'itérer sur toutes les todos présentes en passant leur statut "completed" de false, à true.  
 Enfin, La liste et l'interface est mis à jour via [toggleComplete](#explain-toggleComplete)
<br >
<br >

-  Tout supprimer  
  
![tout supprime](./img/full-feature_tout-supprimer.jpg) 
<br >

Description: [Controller.prototype.removeCompletedItems](#removeCompletedItems)
Déclenché par événement "removeCompleted", qui fait suite au clic de l'utilisateur sur le lien "clearCompleted".  
Le Controller execute alors sa méthode [removeCompletedItems](#removeCompletedItems).  
Celle ci demande au Model de retrouver dans la BDD toutes les todos ayant comme statut "Completed" évalué à true, et d'executer le callback passé en second argument.  
Ce callback va itérer sur la liste retournée par le Model, et faire appel à [removeItem](#explain-removeItem) pour chaque correspondance soit supprimé.  

<br >
<br >


--- 

## Comment ça marche ? 

###  Ajout de todo:
<a id="add">   

Les fonctions impliqués:   
*  `Controller.addItem( title)`:  
    - Arguments: 
      - title : `string`
    - Valeur de retour:  -
    - Description:   
       - Déclenché lorsqu'une action de type "newTodo" est effectué depuis la Vue 
       - Fais appel au Model pour que la nouvelle tâche soit sauvegarder dans localStorage (_Model.create()_)
       - Fais appel à la vue pour qu'elle se mette à jour (_View.render()_)
       - Utilise une de ses fonctions internes ( *_filter()*) pour que la liste de todo soit afficher dans l'interface

* `Model.create(title, callback)`:
    - Arguments: 
      - title : `string`
      - callback: `function`
    - Valeur de retour: -
    - Description:
      - Utilise title  pour créer un objet représentant la nouvelle tâche, utilisable par Store.save()
      - callback est la fonction à executer lorsque la sauvegarde de lanouvelle tâche sera effectuée.

* `Store.save(updateData, callback, id)`:
  - Arguments: 
    - updateData : `object`
    - callback: `function`
    - id: `number` (optionnel)
  - Valeur de retour: -
  - Description:
    - Vérifie si updateData n'existe pas déjà dans localStorage grâce à id.
    - Ajoute updataData au localStorage si n'existe pas.  
    - Met à a jour la liste de todo si updateData existe.
    - Execute le callback.

* `View.render(viewCmd, parameter)` : 
   - Arguments:
     - viewCmd : _string_
     - parameter : _object_ 
   - Valeur de retour: -
   - Description:
     - viewCmd représente la fonction interne qui doit être exécuté avec le parameter spécifié
     - Execution de la commande met à jour les éléments nécessaires 

* `Controller._filter(force)`:
  - Arguments : 
    - force: _boolean_ 
  - Valeur de retour: - 
  - Description:
    - Si force = true, la liste de todo affiché dans l'interface est mise à jour.  
    
<br > 

##### Diagramme déroulement d'un ajout de todo:
  ![diagramme ajouter todo](./img/diag_ajouter.jpg)

<br >
<br >


<a id="edit"> 

###  Editer un todo:

Les fonctions impliqués:  

*  `Controller.editItem( id)`:  
    - Arguments: 
      - id : `number` ou `string` ou `object`
    - Valeur de retour:  -
    - Description:   
       - Fais appel au model pour que l'élément ayant id commen identifiant soit retrouvé et modifié (via )
  
*  `Model.read(query, callback)`: 
    - Arguments: 
      - query : `number` ou `string` ou `object`
    - Valeur de retour: -
    - Description:   
       - Vérifie quel est le type de query, puis demande au Store de réaliser les opérations appropriés sur le localStorage, en se basant sur query et callback

  
*  `Store.find(query, callback)`: 
    - Arguments: 
      - query : `number` ou `string` ou `object`
    - Valeur de retour: -
    - Description:   
       - Vérifie quel est le type de query, puis demande au Store de réaliser des opérations sur le localStorage, en se basant sur query et callback


<br >
<br >

<a id="remove" />

#### Supprimer un todo:

Les fonctions impliqués:  

*  `Controller.removeItem( id)`:  
    - Arguments: 
      - id : `number` 
    - Valeur de retour:  -
    - Description:   
       - Fais appel au Model.read pour retrouver le todo ayant id comme identifiant pour qu'il soit retiré du localStorage.
       - Fais appel a View.render pour que l'interface soit mis à jour une fois que la suppression est effectuée
  
*  `Model.read(query, callback)`: 
    - Arguments: 
      - query : `number` ou `string` ou `object`
    - Valeur de retour: -
    - Description:   
      - Vérifie quel est le type de query, puis demande au Store de réaliser les opérations appropriés sur le localStorage, en se basant sur query et callback

  
*  `Store.findAll(query)`: 
    - Arguments: 
      - query : `number` ou `string` ou `object`
    - Valeur de retour: -
    - Description:   
       - Vérifie quel est le type de query, puis demande au Store de réaliser des opérations sur le localStorage, en se basant sur query et callback


<br >
<br >

<a id="remove" />

#### toggler un todo:
Fonctions impliqués:  

*  `Controller.toggleComplete(id, completed, silent)`:  
    - Arguments: 
      - id : `number` 
      - completed: `boolean` (valeur de la checkbox lié au todo 'toggler')
      - silent: `boolean` (False empeche le filtrage de la liste)
    - Valeur de retour:  -
    - Description:
       - Met à jour le statut de la tâche ayant id comme identifiant.     
       - Demande la mise a jour de la liste dans localStorage via  Model.update , et mise a jour de l'interface via View.render   
    

*  `Model.update(id, data, callback)`:  
    - Arguments: 
      - id : `number` 
      - data: `object` (contient les propriétés qui doivent être mis à jour et leurs nouvelles valeurs )
      - callback: `boolean` (contient les intructions à réaliser une fois que la mise à jour est terminée)
    - Valeur de retour:  -
    - Description:
       - Demande la sauvegarde de data via Store.save()     
       - Demande la mise a jour de la liste dans localStorage via  Model.update , et mise a jour de l'interface via View.render 

* `Model.save()`:


# Autre approche : 
#### Comment le Controller reçoit la notification 
#### Ajouter todo

```javascript
  Controller.prototype.addItem = function (title) {
    var self = this;

    if (title.trim() === "") {
      return;
    }

    self.model.create(title, function () {
      self.view.render("clearNewTodo");
      self._filter(true);
    });
  };
```
_diagramme

<a id="reference" />

#### Référence


##### Model
<a id="read"/>
<a id="create"/>

##### View
<a id="render"/>


##### Controller
<a id="addItem" /> 

*  `Controller.addItem( title)`:  
    - Arguments: 
      - title : `string`
    - Valeur de retour:  -
    - Description:   
       - Déclenché lorsqu'une action de type "newTodo" est effectué depuis la Vue 
       - Fais appel au Model pour que la nouvelle tâche soit sauvegarder dans localStorage (_Model.create()_)
       - Fais appel à la vue pour qu'elle se mette à jour (_View.render()_)
       - Utilise une de ses fonctions internes ( *_filter()*) pour que la liste de todo soit afficher dans l'interface


<a id="editItem" />

<a id="toggleComplete" />  


<a id="setView" />  

<a id="showAll" />  

<a id="showComplete" />  

<a id="showActive" />  

<a id="toggleAll" />  



<a id="audit"/>

### Audit 


**LIEN VERS WIKI**

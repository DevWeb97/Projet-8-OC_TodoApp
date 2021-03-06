# Correction des bugs de l'application

## A propos

Un des points d'étude de ce projet était de comprendre le code écrit par un autre développeur, pour ensuite y apporter une contribution.  
Lors de la réception des fichiers formant l'application un certain nombres de bugs empêchaient le lancement de l'application , la console de développement contenaient aussi certaines erreurs. La première étape a donc été de les corriger.  
Voici le résultat:

<br />

1. Une erreur de frappe au niveau du nom d'une fonction empêchait le code de se compiler.

- Problème: la fonction addItem comportait 3 fois la lettre 'd' au lieu de 2  
   
  ![typo initial](./img/p8_etape-1_erreur-frappe-code-initial.JPG)     
    <br />
- Solution: supprimer le "d" superflu  
  
  ![typo correction](./img/p8_etape-1_erreur-frappe-code-FINALJPG.JPG)  
  <br />
  <br />

2. L'approche utilisée pour créer des identifiants uniques pouvait amener à créer des doublons

- Problème: L'approche initiale s'appuyer sur un ensemble de 10 caractères placés aléatoirement pour former un identifiant 'unique'.  
  Bien que cela fonctionne, la possibilité que deux identifiants soient constituées des mêmes nombres placés dans le même ordre existe toujours.  

   ![id unique initial](./img/p8_etape-1_potential-conflict-id_INITIAL.png)  

   <br />
- Solution:
  - Utiliser un package tiers comme le populaire **uuid**
  - Passer par l'utilisation de `timestamp`.   
  C'est cette approche qui a été choisi et qui offre l'avantage de se couper de dépendance externe.
  
    ![id unique par timestamp](./img/p8_etape-1_potential-conflict-id_FINAL.png)  
    <br />
    <br />

3. Impossible d'utiliser le bouton 'tout marquer comme complété'

- Problème: L'approche initial est d'utiliser la liaison entre le label et le input, pour que le click sur le label coche/ décoche le checkbox.  
Pour qu'une telle liaison soit possible il est nécessaire d'utiliser sur le label l'attribut `for` avec pour valeur l'identifiant du input que l'on souhaite 'connecter'.    
Or dans le fichier index.html aucun **id n'est spécifié** sur le input, le label et ce dernier n'étaient donc pas connectés, rendant la fonctionnalité 'toggleAll' inopérante.

  ![id toggleAll manquant](./img/id-toggleAll.png)

- Solution: Ajouter le id toggleAll sur le checkbox. 
  
  ![id toggleAll](./img/id-toggleAll_ajouter.png)
  <br />
  <br />

4. Une boucle qui pourrait être optimisé:

- Problème: Bien que la fonctionne remplisse son rôle, il est possible de raccourcir son écriture en quelques lignes  
  
  ![boucle initial](./img/p8_etape-1_optimization-loop_store-remove-fct_INITIAL.png)
  <br />
  <br />
 
- Solution: utilisation de la syntaxe ES6 `forEach` et du principe de 'short-circuiting'
  ![optimisation es6](./img/p8_etape-1_optimization-loop_store-remove-fct_FINAL.png)
  <br />
  <br />


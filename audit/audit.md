<!-- prettier-ignore-start -->
# Audit performance du site [Todolist.net](https://www.google.com)

## Sommaire:

1.  [Introduction](#intro)

    1.  [Objectif de cet audit](#goal)

    2.  [ A propos de Todolist&period;net](#about)

    3.  [Outil utilisé](#audit-tools)

    4.  [Configuration de l'outil](#tool-config)

2.  [Analyse du rapport](#analyze)

    1.  [Performance](#perf)

    2.  [Accessibilité](#accessibility)

    3.  [Best Practice](#best-practice)

    4.  [SEO](#seo)

3.  [Conclusion](#summary)
 

---

<a  id="intro"  />

## Introduction

<a  id="goal"  />

### Objectif de cet audit:

Votre équipe vous a demandé d'analyser la performance d'un site concurrent pour identifier ce qui marche bien et ce qui ne marche pas, au cas où vous décidez de "scaler" votre propre application.

Le but étant d'offrir à nos utilisateurs une expérience la plus rapide et agréable possible, tout en nous autorisant le maximum de flexibilité nécessaire pour une possible scalabilité de notre application
  
    

<a  id="about"  />

### A propos de Todolist&period;net

Application web de gestion de todo:

> TodoListMe vise à offrir le plus haut niveau de convivialité possible en fournissant juste ce dont vous avez besoin pour accomplir vos tâches, présenté d'une manière qui rend ce que vous devez faire clair et qui demande un minimum d'effort.  
> En d'autres termes, son fonctionnement est puissamment simple.

> source: http://todolistme.net/todo_list_about.php#why

Todolist&period;net est un concurrent direct à notre application.

L'avantage que possède notre concurrent est d'être déjà présent sur le marché.

Malgré cela, nous pouvons en tirer avantage de leur position: Un audit de leur site nous permettra d'anticiper les écueils qui pourraient ralentir notre lancement sur le marché et la scalabilité de notre propre application.
  
    

<a  id="audit-tools"  />

### Outils utilisés:

**Lighthouse**

> Lighthouse est un outil automatisé "open-source" destiné à améliorer la qualité des pages Web en proposant des audits de performance, d'accessibilité, d'applications Web progressives, de référencement, etc.

> source: https://developers.google.com/web/tools/ligthouse

Grâce à Lighthouse il est possible de rapidement voir quels aspects du site testé doivent être optimisés pour qu'il soit favorisés par l'algorithme de Google.

Il offre des suggestions d'ajustement lié aux points faibles rapportés.  
  
    


<a id="tool-config">

### Configuration de l'outil

Lighthouse a été configuré de façon à simuler l'utilisation sur un smartphone avet une connexion 3G moyenne.

![Lighthouse configuration pour mobile](audit_assets/img/config-Lighthouse-mobile-v1.jpg)

En effet, 68% des visites de site web à l'échelle mondiale sont faites depuis un smartphone. [(source: perficient.com)](https://www.perficient.com/insights/research-hub/mobile-vs-desktop-usage)

Cette configuration vient consolider notre objectif de préparer au mieux notre application à une utilisation concrète.  
  

---  
  
    

<a  id="analyze"  />

## Analyse du rapport

![ Resultat global](audit_assets/img/metric-global.jpg)

Les scores fournit par Lighthouse vont de 0 à 100.

Le but étant d'avoir les scores les plus hauts possible dans chaque aspect mesurés par l'algorithme Google.

Ces résultats sont calculés en fonction de critères spécifiques selon le métrique visé (performance, accessibilité, seo, etc).  

  
    

<a  id="perf"  />

### Performance

![ Resultat global](audit_assets/img/performance-metric.jpg)

Le score faible (<50) induit que le chargement du site audité (ici sur mobile) est lent.

Ce score est déterminé par 6 critères, représentant chacun un mécanisme lié à la vitesse de chargement du site.

- **First Content Paint** (FCP) -- _ideal < 2s_:  
  Calcule le temps nécessaire pour que le **navigateur affiche le premier élément du DOM** lors du chargement de la page demandée par l'utilisateur.

  _Comment est-il calculé ?_  
   Le score FCP se base sur une comparaison entre le temps FCP de votre page et les temps FCP pour de réels sites Web, provenant de la base des données de l’archive HTTP.

- **Largest ContentFul Paint** (LCP) -- _ideal < 4.3s_ :

  Représente le temps de pris pour que le contenu principal soit visible par l’utilisateur (le contenu qui représente quelque chose de tangible à ses yeux).

  Ce critère permet de fournir une indication sur la **rapidité avec laquelle un utilisateur peut réellement voir le contenu de la page**.

  Il vient complémenter le FCP (First Content Paint )

- **Speed Index** -- _ideal < 2.5s_:

  Représente la **vitesse à laquelle le contenu est affiché à l'écran**.

- **Time To Interactive** (TTI) -- _ideal < 5.2s_:

  Temps nécessaire pour qu'une page deviennent **entièrement interactive**.

  Point important pour l'expérience utilisateur.

- **Total Blocking Time** (TBT) -- _ideal < 300ms_:

  **Temps total** durant laquelle la page est bloquée. (**page ne réagit pas aux interactions de l'utilisateur** pendant son chargement)

- **Cumulative Layout Shift** (CLS) -- _ideal < 0.1_:

  Layout Shift = Changement de mise en page de façon inattendue, généralement causé par du contenu ajouté dynamiquement autour d'un élément déjà affiché.

  Cette adaption de la mise en page se traduit par une succession de mouvements rapides des différents éléments concernés jusqu'à ce que tous aient trouvé leur place définitive.

  Lighthouse mesure ces sursauts (sucession de mouvement pendant une courte durée) à intervalle régulier et repère les plus extrêmes.

  Le CLS mesure **l'accumulation de ces mouvements successifs**.

  Ces changements rapides peuvent gêner l'utilisateur lorsqu'il décide d'interagir avec un bouton de validation par exemple, mais qu'au même moment survient un _layout shift_ qui l'amène à cliquer sur un autre élément.

Dans le cas de Todolist&period;net, seuls 2 critères (TBT et CLS) sur 6 ont réussi le test Lighthouse.  
  
  ---

La section "opportunités" propose des solutions possibles pour accélérer le chargement des pages du site, ainsi que le temps gagné si correctement mis en place:

![Performance opportunités](audit_assets/img/performance-opportunities.jpg)

| Opportunités d'améliorations                                                                                                                  | Bénéfices                                                                                                                              |
| --------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| Implémenter HTTP/2                                                                                                                            | Communication entre client et serveur plus efficace et rapide                                                                          |
| attribut `preconnect` <br /> <br />attribut `dns-prefetch`                                                                                    | Aide le navigateur à prioriser les requêtes vers des ressources externes <br /><br /> Accélère la résolution DNS des serveurs externes |
| `defer` les scripts JS qui ne sont pas nécessaires au chargement initial de la page dans le `head`. <br />Scripts nécessaires à la fin `body` | Prioriser le render de la page: DOM > Scripts nécessaires > Scripts additionnels                                                       |
| Pré-charger le/les images du contenu principal de la page avec attribut `preload`                                                             | Indique au navigateur de charger en priorité cette/ces image(s) pour que le LCP soit le plus rapide possible                           |
| Supprimer le code inutile                                                                                                                     | Evite la consommation de bande passante pour demander du code qui ne sera pas utilisé                                                  |
  
    
      

----
  

La section "diagnostics" quelques points présents dans le site qui mériteraient une attention plus particulière:

![ Performance diagnostiques](audit_assets/img/performance-diagnostics.jpg)

| Problèmes diagnostiqués                                                                                                  | Solutions possibles & Bénéfices                                                                                                                                                                      |
| ------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Requêtes répétitives pour des fichiers statiques                                                                         | Mettre en place un système de 'caching' pour accélérer le chargement d'images/scripts JS souvent demandés                                                                                            |
| Les `<img />` n'ont pas d'attribut `width` et `height`                                                                   | Préciser ces attributs permet d'améliorer le CLS                                                                                                                                                     |
| Utilisation de `document.write()` pour injecter du contenu dynamiquement ( peut poser problème sur des connexions lentes | Placer les éléments dans scripts et utiliser `async` pour injecter le nécessaire de façon asynchrone                                                                                                 |
| La performance au niveau du scrolling n'est pas optimiser                                                                | Les gestionnaires d'événements en charge du scroll/ navigation au toucher devrait utiliser `{passive: true}` pour éviter les délais entre l'action de l'utilisateur et son résultat dans l'interface |

Ce métrique est très important puisqu'il vient mesuré d'une part la vitesse du chargement du site, mais aussi le délai entre le moment où les données demandées sont reçues et le moment où l'utilisateur peut enfin intéragir avec l'interface.

Une vitesse de chargement du contenu et/ou un délai avant interaction important viennent impacter négativement l'expérience utilisateur.

sources:

> https://www.blogdumoderateur.com/Lighthouse-score-performance-web/  
> https://web.dev/Lighthouse-performance/#metrics
  
  
    
 ----
<a  id="accessibility"  />

#### Accessibilité  

  


![ Mesures accessibilité](audit_assets/img/accessibility-metrics.jpg)

| Problèmes diagnostiqués | Solutions possibles & Bénéfices |

| --------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |

| Manque certains attributs aidant les lecteurs d'écran | Attributs `alt` sur les `<img />` <br/> `title` pour les `<iframe><iframe/>`<br /> `<label><label/>` pour les champs de formulaires |

| l'ordre des titres ne sont pas respectés | `h1`>`h2`>`h3` <br /> Facilite la navigation au clavier |

| Pas de langue par défaut pour l'interprétation du contenu | Préciser l'attribut `lang` pour que le contenu soient lu avec la **prononciation** correct |

| Pas de contraste suffisant entre background et foreground | S'assurer que le contraste entre le texte et son fond aient un ratio suffisant, pour que le texte soit lu facilement |
  
    
  ----  
    
      

<a  id="best-practice"  />

#### Best Practice

![Mesures Best Practice](audit_assets/img/best-practice-metrics.jpg)

| Problèmes diagnostiqués                                                               | Solutions possibles & Bénéfices                                                                                                                                                                  |
| ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Le site n'est pas sécurisé                                                            | Utiliser `HTTPS` pour sécuriser les données même ceux n'utilisation de donnée sensible. <br /> Rassure l'utilisateur.                                                                            |
| Utilise des librairies Javascripts ayant des vulnérabilités connues (`[jQuery@2.2.4]` | Mettre à jour le code utilisé <br />Eviter leur utilisation                                                                                                                                      |
| Element interactifs ne sont pas facile d'utilisation                                  | Dimension minimum : 48px par 48px. <br /> Avoir suffisamment d'espace autour d'eux pour éviter des chevauchements indésirables                                                                   |
| Existance de lien qui ne sont pas indexable                                           | Utiliser attribut `href` avec la destination approprié                                                                                                                                           |
| Images n'ont pas la résolution appropriée                                             | Adapter la taille des images proportionnellement à la dimension d'affichage utilisée, pour une clarité maximale. <br > (ex: img de 16px x 16px devraient avoir une dimension de 32x32 à l'écran) |
  
    
----
  
    

<a  id="seo"  />

#### SEO

![Mesure best practice](audit_assets/img/seo-metrics.jpg)

| Problèmes diagnostiqués                              | Solutions possibles & Bénéfices                                                                                                                              |
| ---------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Pas optimiser pour écran de smartphone               | Préciser `<meta name="viewport">` avec attribut `width`, `initial-scale`                                                                                     |
| Taille de police trop faible                         | Favorisé taille >12px pour que les utilisateurs n'aient pas à pincer pour zoomer/dézoomer <br /> Contenu devrait prendre au moins 60% de l'espace disponible |
| Element interactifs ne sont pas facile d'utilisation | Dimension minimum : 48px par 48px. <br /> Avoir suffisamment d'espace autour d'eux pour éviter des chevauchements indésirables                               |
| Existance de lien qui ne sont pas indexable          | Utiliser attribut `href` avec la destination approprié|


--- 
  
    
    
<a id="summary" />

## Conclusion

Cet audit de Todolist&period;net permet de mettre en exergue les points importants qui doivent être adressé pour qu'un site web puisse offrir la meilleure performance et expérience d'utilisation possible pour l'utilisateur, tout en maximisant ses chances d'être traité correctement par l'algorithme Google

**Pour un chargement des pages :**

- Utilisation du protocole `HTTP/2` pour des communications client-server rapide et efficace

- Prioriser les ressources (scripts) par ordre d'importance en les plaçant stratégiquement dans le `<head>` avec attributs `defer` pour les moins nécessaires et à la fin du body pour ceux plus important pour le 'rendering'.

Notons que les attributs `preload` et `preconnect` et `dns-prefetch`peuvent accélérer la vitesse à laquelle les ressources 3rd-party parviennt au navigateur

- Optimiser l'utilisation des ressources via un **système de cache** pour les fichiers statiques, passer par un **CDN**, compresser les images et minifier le code source.

**Pour une expérience utilisateur agréable:**

- Améliorer la **perception de la vitesse de chargement** en s'assurant un `LCP` et `FCP` faible.

- Le **TTI** se doit être le plus bas possible pour éviter la frustration causé par du contenu chargé visuellement mais qui ne permet pas encore l'interaction.

- Eviter l'injection de contenu dynamique lors du chargement pour éviter un `CLS` important.

- Event listener du scroll/navigation au toucher en `passive` mode, pour diminuer les délai interaction-resultat sur l'interface

**L'accessibilité a aussi son importance:**

- Faciliter le travail des lecteurs d'écrans en précisant les attributs nécessaires pour les `img`, `lang` pour la lecture de contenu , les `label` pour les champs de formulaire, `aria-*` ...

- Contraste suffisant entre backgroud et foreground

**Quelques bonnes pratiques mis en lumière:**

- Utilisation de `HTTPS` pour sécuriser les échanges de données, rassurer l'utilisateur quant à la fiabilité du site

- Eviter les librairies JS et API avec des vulnérabilités connues

**Au niveau du SEO:**

- Eviter les liens sans attribut "href"

- S'assurer de préciser au navigateur d'afficher le contenu avec dimension adapté avec les tag apropriés.

- Dimension correct des elements cliquables sur mobile

Il convient de préciser que cet audit s'est principalement focalisé sur les points faibles du site concurrent pour ne pas les reproduire.

Cependant d'autres aspects importants du site ont passé l'audit avec succès, et méritent un examen appronfondit pour en tirer profit.

Enfin Lighthouse, bien que clair et succint, ne liste pas tous les éléments à vérifier. Pour une vue globale de ce qui est améliorer, l'utilisation d'outils complémentaires est bienvenue.
<!-- prettier-ignore-end -->

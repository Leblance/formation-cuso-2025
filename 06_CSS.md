# TEI-Publisher et CSS

TEI-Publisher s'accompagne de plusieurs feuilles de styles pour mettre en page différents aspects du site :

- ```/resources/css/theme.css``` : Feuille de style générale (mise en page du site : menu de navigation, toolbar, drawer, etc.).
    - Il est déconseillé de modifier directement cette feuille de style, mais plutôt de la dupliquer pour créer votre propre thème personnalisé.
- ```/resources/css/epub.css``` : Propriétés CSS pour mettre en page les exports en ePub.
- ```/resources/css/print.css``` : Propriétés CSS pour la mise en page de l'output "Print" (visible depuis print-preview.html).
    - À créer si besoin.
- ```/resources/css/components.css``` : Propriétés CSS pour réécrire les styles des webcomponents.
    - À créer si besoin.
    - :warning: pour lier cette feuille de style à un template, il faut ajouter un attribut @theme à &lt;pb-page&gt; :
    ```<pb-page data-template="pages:pb-page" theme="resources/css/components.css">```
- ```/resources/odd/{nom}.css``` : Propriétés CSS pour la mise en page des éditions

En plus des feuilles de style, certains templates contiennent des styles internes (principalement index.html).

Rappel sur le fonctionnement de CSS :

- Lorsque plusieurs propriétés pointent vers le même élément, le navigateur applique en priorité les styles internes à un élément (inline style), puis les styles internes/externes (éléments &lt;style&gt; ou feuilles externes) et enfin les styles par défaut du navigateur.
- Les types de sélecteurs n'ont pas tous la même spécificité : les ID (#paragrah) sont prioritaires. Viennent ensuite les classes (.paragraph) et les sélecteurs d'éléments (p).
- Avant de styler, utiliser l'inspecteur d'éléments pour comprendre quels sont les sélecteurs utilisés et où ils se trouvent.

Liste de couleurs pour les exercices :

- Indigo : #2e006c
- Plum compote : #484253
- Brique : #842e1b
- Améthyste : #884da7
- Bleu Majorelle : #6050dc
- Jungle Green : #005242
- Magenta : #8b008b
- Ginger Biscuit : #b83c08

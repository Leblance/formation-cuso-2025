# Templates et webcomponents
## 1. Templates : Généralités
Si l'affichage des éditions est géré uniquement par l'ODD, l'affichage et l'organisation du site Web est géré par une série de templates. Ces derniers prennent la forme de fichiers .html et se trouvent dans le dossier ```/templates``` de votre application. Les fichiers à la racine concernent les pages et les fonctionnalités générales du site. Les fichiers permettant d'afficher les éditions se trouvent dans le dossier ```/pages```.
Ces templates sont "combinables". Ainsi, le menu de navigation se trouve dans le fichier ```menu.html``` et est appelé par les autres fichiers à l'aide de la ligne suivante :
```
<app-toolbar data-template="templates:include" data-template-path="templates/menu.html"/>
```

## 2. Les webcomponents
Les webcomponents (standard du W3C) sont des éléments HTML qui encapsulent des fonctionnalités et des styles prédéfinis. Ils permettent de simplifier la composition des pages, en limitant le nombre de lignes de code à écrire et en compartimentant les fonctionnalités, i.e. les styles attribués à un webcomponent ne se répercuteront pas sur le reste du site.
Chaque template est ainsi un mélange d'éléments HTML 5 standards et de webcomponents. Ceux-ci sont identifiables à leur préfixe :

- "pb-" : Webcomponents propres à TEI-Publisher
    - Liste des webcomponents : [https://cdn.tei-publisher.com/@2.23.2/dist/api.html](https://cdn.tei-publisher.com/@2.23.2/dist/api.html)  
- "app-", "paper-" et "iron-" : Webcomponents Polymer (Librarie Javascript)
    - Liste des webcomponents : [https://www.webcomponents.org/](https://www.webcomponents.org/)
    
Par exemple, l'insertion d'un outil de visualisation se fait à l'aide de ```<pb-facsimile>``` ou, plus récémment, de ```<pb-tify>``` ; d'un zoom avec ```<pb-zoom>``` ; d'une carte avec ```<pb-leaflet-map>``` ; de plusieurs colonnes avec ```<pb-panel>``` ; etc.
Une page se compose de plusieurs "blocs" qui communiquement les uns avec les autres, en envoyant des *events* et en s'inscrivant à des *channels*.

## 3. Anatomie d'un template : l'exemple de shakespeare.html
<img src="images/exemple_structurePage.png" width="900"/>

## 4. Exercices pratiques
### 4.1. Créer un nouveau template de page

1. Dans le dossier ```/templates/pages```, créer un nouveau fichier .html ;
1. Copier/coller la structure de **shakespeare.html** dans ce nouveau fichier ;
1. Dans **config.xqm**, modifier la ligne 93 en remplaçant shakespeare.html par le nom du nouveau template.
```
declare variable $config:default-template :="cuso.html";
```

### 4.2. Afficher des images
Méthode 1 : &lt;pb-facsimile&gt; ([Documentation](https://cdn.tei-publisher.com/@2.23.2/dist/api.html#pb-facsimile.0))

- Dans votre template, commenter &lt;pb-tify&gt; et ajouter la ligne suivante :
```
<pb-facsimile id="facsimile" base-uri="https://iiif.hedera.unige.ch/iiif/3/pliegos/" default-zoom-level="0" show-navigation-control="" subscribe="transcription"/>
```
- Dans les styles internes, ajouter les propriétés CSS suivantes :
```
.content-body pb-facsimile {
    flex: 1 1;
    --pb-facsimile-height: var(--pb-view-height);
```
- Dans l'ODD, ajouter l'élément &lt;pb/&gt; et observer son fonctionnement ;
- Dans votre template, ajouter des paramètres supplémentaires à &lt;pb-facsimile&gt; :
    - show-home-control
    - show-rotation-control
    - show-download-control 

Méthode 2 : &lt;pb-tify&gt; ([Documentation](https://cdn.tei-publisher.com/@2.23.2/dist/api.html#pb-tify.0))

- Dans votre template, commenter les lignes correspondant à &lt;pb-facsimile&gt; et décommenter celles de &lt;pb-tify&gt; ;
- Dans l'ODD, ajouter un nouveau model :
    - @behaviour : webcomponent
    - @cssClass : facs
    - &lt;param&gt; :
        - name : ```'pb-facs-link'```
        - facs : ```'api/iiif/' || substring-after(document-uri(root($parameters?root)), $global:data-root || '/')```
        - emit : ```'transcription'```
        - emit-on-load : ```'emit-on-load'```
        - order : ```count($get(.)/preceding::pb) + 1```
- Dans ```/modules/iiif-config.xqm``` :
    - Modifier la ligne 14 :
    
```
declare variable $iiifc:IMAGE_API_BASE := "https://iiif.hedera.unige.ch/iiif/3/pliegos";
```

    - Modifier la ligne 34 pour récupérer les liens des images dans &lt;sourceDoc&gt; :
```
declare function iiifc:milestone-id($milestone as element()) {
    let $corresp := $milestone/@corresp
       => substring-after('#')
    let $surface := $milestone/ancestor::tei:text/preceding-sibling::tei:sourceDoc/tei:surface
    
    for $s in $surface
        let $graphic := $s/tei:graphic/@url
        return
            if($corresp = $s/@xml:id) then
                replace($graphic, "^[^:]+:(.*)", "$1")
            else()
};
```




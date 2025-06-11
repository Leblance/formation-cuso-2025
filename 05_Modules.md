# Modules et fonctions
Les modules XQuery permettent de traiter les données TEI et de les afficher sur le site : résultats de recherche, facettes, filtres, etc. Ils assurent le fonctionnement de votre application. Pour rappel, les fichiers à la racine du dossier ```/modules``` peuvent être modifiés sans risque. Il est toutefois déconseillé de modifier les fichiers du dossier ```/lib```.

## 1. Afficher des images
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
- Dans ```/modules/iiif-config.xqm```, modifier la ligne 14 :
    
```
declare variable $iiifc:IMAGE_API_BASE := "https://iiif.hedera.unige.ch/iiif/3/pliegos";
```

- Toujours dans le même fichier, modifier la ligne 34 pour récupérer les liens des images dans &lt;sourceDoc&gt; :
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

## 2. Les fonctionnalités de recherche
Trois fichiers à connaître :

- collection.xconf (Lucene Apache): Défini des règles d'indexation (plein-texte, filtres et facettes)
- index.xql : Fonctions qui indiquent le chemin vers les données à traiter dans les filtres et les facettes
- config.xqm : Paramètrage de l'affichage des facettes sur le site (variable ```$config:facets```)

Exemple 1 : Ajouter une facette pour les lieux de publication

- Dans index.xql, modifier les règles pour la dimension "place" :
```
case "place" return
    ($root//tei:pubPlace)
```
- Dans config.xqm, ajouter une nouvelle map pour la dimension "place" :
```
map {
        "dimension": "place",
        "heading": "Place",
        "max": 5,
        "hierarchical": false()
    },
```

- Dans collection.xconf, ajouter un nouvel élément &lt;facet&gt; :
```
<facet dimension="place" expression="nav:get-metadata(ancestor::tei:TEI, 'place')"/>
```

- Dans en.json et fr.json, ajouter une nouvelle clé pour traduire le label de la nouvelle facette : "Place" et "Lieu"
- Dans config.xqm, modifier la valeur de la clé "heading" pour les lieux : ```facets.place```
- Toujours dans config.xqm, modifier la map pour l'affichage de la facette "language" : enlever les informations inutiles, ajouter le catalan et traduire le label "Espagnol".
- À votre tour : ajouter trois nouvelles facettes pour les dates, les imprimeurs et les types de texte.

Le fichier ```modules/facets.xql``` contient un ensemble de fonctions qui gère le fonctionnement et l'affichage des facettes. Par exemple, la fonction ```facet:sort``` vous permet de changer l'ordre dans lequel sont classées les items de chaque facette.

## 3. TEI-Publisher API
Les webcomponents communiquent avec le serveur pour afficher les données dont ils ont besoin. Depuis la version 7, la communication avec le serveur passe par une API. Elle définit plusieurs *endpoints* pour afficher les documents, les résultats de recherche ou, plus récemment, les manifestes IIIF ; pour gérer l'ODD ; etc. Cette API est accessible depuis le menu de navigation : "Fonctions avancées > API". Il est possible de la personnaliser et d'ajouter de nouveaux endpoints, en modifiant deux fichiers :

- ```custom-api.json``` : Spécifications des endpoints ;
- ```custon-api.xql``` : Modules XQuery pour traiter les requêtes.

Exemples utiles :

- Création d'index et de cartes, réécriture d'URL : [FAQ de TEI-Publisher](https://faq.teipublisher.com/api/)
- Création d'index : [Damen Conversation Lexikon](https://github.com/eeditiones/tei-publisher-app/blob/master/modules/custom-api.json)
# Modules et fonctions
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


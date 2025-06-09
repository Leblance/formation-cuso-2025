# Introduction à XQuery

XQuery permet d'interroger une base de données XML, de la même façon que SQL permet de faire des requêtes dans une base de données relationnelle. C'est un langage de programmation fonctionnel, c'est-à-dire qu'il n'altère pas les données, mais les récupére. Il se distingue en cela des langages procéduraux comme Java ou Python. Il a été pensé pour le XML : il accède aux données et permet de construire de nouveaux fichiers XML à partir de fragments du fichier source. XQuery peut s'exprimer à l'aide de deux syntaxes :

- FLWOR : Syntaxe non-XML
- XQueryX (XML syntax for XQuery) : Syntaxe XML (plus verbeuse et moins lisible)

## La syntaxe FLWOR

- **F**or : Initialisation d'une boucle
- **L**et : Déclaration d’une variable
- **W**here : Ajout de conditions (ou de jointures)
- **O**rder by : Réorganisation des données
- **R**eturn : Affichage du résultat

## Let et Return
La déclaration d'une variable se fait avec le mot-clé **let**, suivi du nom de la variable (précédée de **$**), de **:=** et de la valeur de la variable (avec une expression XPath).
Le mot-clé **return** indique le résultat à afficher : il est obligatoire pour que l'expression soit valide et bien formée.

Exemple 1 :
```
let $NumberF := count(//person[@sex="2"])

return $NumberF
```


Exemple 2 :
```
let $NumberF := count(//person[@sex="2"])
let $NumberH := count(//person[@sex="1"])

return
    <ul>
        <li>Nombre de femmes : {$NumberF}</li>
        <li>Nombre d'hommes : {$NumberH}</li>
    </ul>        
```
## Les conditions (if, then, else)
L'expression d'une condition se fait avec le trio de mots-clés **if then else**.

```
let $corresp := substring-after(@corresp, "#")
let $index := doc('/db/apps/test-cuso/data/registers/places.xml')
let $xml-base := $index//listPlace/@xml:base 
let $id := $index//listPlace//placeName/@xml:id  

return if($corresp = $id)   
then $xml-base || $index//placeName[@xml:id=$corresp]/parent::place/@corresp   
else '#'
```
Point sur trois fonctions natives de XQuery :

- **fn:doc** : Spécifie l'URL d'un document à retourner.
- **fn:substring-after** : Extrait la portion d'une chaîne de caractères ($arg1) qui se trouve après le caractère spécifié ($arg2).
- **fn:concat** : Concatène plusieurs chaînes de caractères (séparés par des virgules). Peut être remplacé par deux barres verticales (**||**).

Pour en savoir plus sur l'ensemble des fonctions disponibles : [https://datypic.com/xq/](https://datypic.com/xq/)

## Les boucles for
Pour parcourir plusieurs éléments, XQuery utilise le mot-clé **for**.

Exemple 1 :
```
for $role in //tei:roleName
    return $role
```

Exemple 2 :
```
for $bishop in //tei:person[contains(., "bishop")]
    return <bishop>
                {$bishop//tei:persName/concat(tei:forename, ' ', tei:surname)}
           </bishop>                
```

Point fonction native :

- fn:contains : Affiche les éléments ($arg1) qui contiennent la chaîne de caractères recherchée ($arg2).



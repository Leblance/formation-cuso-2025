# Introduction à TEI-Publisher

Objectifs : Créer sa première application et comprendre son organisation.

## 1. Présentation générale
Créé en 2015, TEI-Publisher est un outil clé en main qui permet de publier des données XML sans être développeur. Il reste toutefois très flexible et personnalisable pour s'adapter à une large variété de cas. Il repose sur les principaux standards XML (XPath, XSLT, XQuery) et du web (HTML, CSS, Javascript) afin d'assurer l'interopérabilité et la maintenance des données publiées. La version actuelle est la version 9.1, mais une [version 10](https://www.e-editiones.org/posts/community-event-tei-publisher-10-preview/) a déjà été annoncée !

TEI-Publisher bénéficie d'une communauté très active et est supporté depuis 2020 par l'association internationale [e-editiones.org](https://www.e-editiones.org) :
- [Documentation](https://teipublisher.com/exist/apps/tei-publisher/documentation) à chaque nouvelle version
- [Slack](https://join.slack.com/t/e-editiones/shared_invite/zt-e19jc03q-OFaVni~_lh6emSHen6pswg) channel
- [Mailing list](https://lists.hostpoint.ch/mailman3/lists/community.e-editiones.org)
- Online meet-up (chaque 1er mardi du mois)
- [YouTube](https://www.youtube.com/channel/UCAPhSZdBwFRCEFWNNYOC4Ww) channel

## 2. Showcase : Ce que TEI-Publisher permet de faire

- [The Fairy Tales and Stories of Hans Christian Andersen](https://hca.sdu.dk/exist/apps/andersen-irons/index.html) : Éditions de lecture
- [Shakespeare's Plays](https://teipublisher.com/exist/apps/shakespeare-pm/index.html) : Affichage dynamique texte-facsimilé
- [Alfred Escher Briefedition](https://www.briefedition.alfred-escher.ch) : Frises chronologiques, index, alignement texte-image
- [Van Gogh Letters](https://teipublisher.com/exist/apps/vangogh/index.html) : Frises chronologiques, édition synoptique (colonnes dynamiques), enrichissement du texte (notes, accès à un index)
- [Démêler le cordel](https://desenrollandoelcordel.unige.ch) : Affichage dynamique texte-facsimilé, index des lieux avec cartes
- [Thebarum Fabula](http://thebarumfabula.usc.es) : Alignement texte original-traduction, apparat critique

NB : Sur le site de l'association e-editiones, vous trouverez un [registre](https://www.e-editiones.org/map) des projets qui utilisent TEI-Publisher (bien utile pour faire votre état de l'art et vous donner des idées !).

## 3. Création d'une application

1. Ouvrir l'application TEI-Publisher depuis le tableau de bord d'eXist-DB.
    - eXist-DB est une base de données XML (NoSQL), où chaque élément devient une entrée dans votre base de données. Il permet d'interroger vos données et de les afficher avec le langague XQuery. eXist est aussi un "application server support", qui permet de créer ses propres applications/sites web.
    - TEI-Publisher est une application d'eXist-DB. L'instance que vous installez est un espace de test et de génération de votre propre application.
1. Se connecter en mode démo (Login : tei-demo / mot de passe : demo).
1. Cliquer sur "Aire de jeu" (ou "Playground").
1. Importer des fichiers TEI en faisant un glisser-déposer dans l'encart à droite de l'écran, puis cliquer sur un fichier.
1. Tester différentes formes d'affichage du texte et de la page, en ouvrant le "hamburger menu" à droite de l'écran (les 3 barres horizontales) :
    - ODD : Affichage du texte (transformation des éléments TEI en HTML).
    - Templates : "Gabarits" de page (position des blocs d'information sur la page).
1. Créer son ODD :
    - Retourner sur la page de l'aire de jeu et scroller jusqu'en bas.
    - Remplisser le formulaire avec le nom de votre fichier ODD (sans l'extension) et le titre à afficher dans la liste.
    - Cliquer sur "Créer".
    - Votre ODD apparaît dans la liste. Vous pouvez maintenant la personnaliser en cliquant sur son nom.
1. Cliquer sur "Fonctions avancées > Générateur d'applications".
    - Dans la liste, sélectionner l'ODD que vous venez de créer.
    - URL : http://exist-db.org/apps/mon-app
    - Nom abrégé : (Le super nom de votre application !).
    - Titre : (Le super titre de votre application !).
    - Gabarit HTML : Choisir celui qui se rapproche le plus de vos besoins (Pas de panique, non seulement vous pourrez le personnaliser, mais vous pourrez également le changer si besoin. Votre choix n'est donc pas décisif).
    - Vue par défaut : page.
    - Index plein texte par défaut : Créer sur une division.
    - Définir un nom d'utilisateur et un nouveau mot de passe (ils doivent être différents de vos identifiants créés lors de l'installation d'eXist-DB et qui sont vos identifiants admin).
1. Cliquer sur "Sauvegarder et générer".
1. Se déconnecter du compte "tei-demo" et fermer l'application TEI-Publisher.
1. Sur le tableau de bord d'eXist-DB, apparaît désormais votre nouvelle application.

## 4. Structure d'une application
Depuis Exide ou Oxygen, vous constaterez que le dossier "apps" de votre base de données contient désormais un sous-dossier portant le nom abrégé de votre application. Ce dossier se structure de la manière suivante :
- data : Vos données XML TEI ;
- modules : Les fichiers XQuery qui assurent le fonctionnement de l'application ;
    - :warning: Attention : Ne pas modifier les modules qui se trouvent dans le dossier modules/lib ! C'est le cœur de votre application. Ce sont ces fichiers qui seront modifiés lors des mises à jour.
    - Les fichiers à la racine de ce dossier peuvent être modifiés sans risque.
- resources : Images, css, odd, scripts JS, etc. ;
- templates : Gabarits de vos pages HTML ;
    - À la racine : Gabarits généraux pour le menu, l'index, la page des résultats de recherche, etc.
    - Dans le dossier "pages" : Gabarits pour les éditions.
- transform : Feuilles XSLT (générées automatiquement) ;
- Fichiers à la racine de votre app peuvent être modifiés. Ceux que vous serez les plus susceptibles de modifier sont collection.xconf, index.xql et controller.xql.

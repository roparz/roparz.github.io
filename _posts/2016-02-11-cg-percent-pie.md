---
layout: post
title:  "cg-percent-pie, une progress bar circulaire avec Sass et AngularJS"
date:   2016-02-20 04:00:00
---

> TLDR; cet article explique comment j'ai créé la directive [`cg-percent-pie`](http://codepen.io/roparz/pen/OMrbLZ)

L'autre jour on m'a demandé d'intégrer une barre de progression circulaire dans laquelle s'affiche
un pourcentage dans un de nos produit. Mon premier réflexe a évidemment été de chercher du côté de
notre moteur de recherche favori si quelque chose n'existait pas déjà. Je suis tombé sur plusieurs
exemples mais aucun ne me convenait vraiment... Pas la bonne techno, trop d'élements ou trop de CSS
pour si peu de chose !<!--break--> J'ai donc décidé, tout en m'inspirant de ce que j'avais trouvé,
de créer ma propre barre avec les technos qu'on utilise dans ma boite : AngularJS et Sass.

Je me suis donc inspiré de [cet exemple](http://codepen.io/jo-asakura/pen/stFHi) qui pose vraiment
de bonnes bases mais qui utilise pour moi trop d'éléments HTML et trop de LESS, on a du mal à s'y
retrouver.

La première étape est donc de reproduire une simple barre circulaire avec le moins d'élément
possible. J'ai pour cela utilisé le pseudo-élément CSS `after` et les propriétés `border` et
`border-radius`. Voici le résultat :

<p data-height="164" data-theme-id="17274" data-slug-hash="EPryLE" data-default-tab="result" data-user="roparz" class='codepen'>See the Pen <a href='http://codepen.io/roparz/pen/EPryLE/'>Pie with percent directive - step 1</a> by roparz (<a href='http://codepen.io/roparz'>@roparz</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

À partir de là, l'idée est d'appliquer une rotation à mon demi-cercle et d'en cacher une partie
pour n'en afficher que celle voulue. Toujours en m'inspirant de l'exemple trouvé plus haut,
j'ai utilisé la propriété CSS [`clip`](https://developer.mozilla.org/en/docs/Web/CSS/clip) et
sa valeur `rect` pour cacher la partie gauche de mon cercle. Pour la rotation, j'utilise la
propriété `transform` et sa propriété `rotate` mais pour la démo, j'ai utilisé une petite animation
sur la rotation pour mieux comprendre le résultat (testé sur Chrome only) :

<p data-height="164" data-theme-id="17274" data-slug-hash="gPEVbN" data-default-tab="result" data-user="roparz" class='codepen'>See the Pen <a href='http://codepen.io/roparz/pen/gPEVbN/'>Pie with percent directive - step 2</a> by roparz (<a href='http://codepen.io/roparz'>@roparz</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

Là, on a fait la moitié du boulot. On a notre demi-cercle qui, avec une petite rotation, permet
de représenter un pourcentage de 0 à 50%, mais il manque les 50 autres. Pour cela rien de plus
simple, on continue la rotation de notre demi-cercle, mais on ajoute l'autre pseudo-élément pour garder
la première partie affichée (sans oublier de supprimer la propriété `rect` sinon notre demi-cercle
sera caché) :

<p data-height="164" data-theme-id="17274" data-slug-hash="zrQbar" data-default-tab="result" data-user="roparz" class='codepen'>See the Pen <a href='http://codepen.io/roparz/pen/zrQbar/'>Pie with percent directive - step 3</a> by roparz (<a href='http://codepen.io/roparz'>@roparz</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

Je rappelle que le but de l'exercice est d'afficher un pourcentage et sa barre de progression
qui évolue en fonction de celui-ci. Je vais donc avoir besoin d'autant de classes CSS qu'il y a de
pourcentages et adapter ces classes pour que, de 0 à 50% seule la partie de droite ne s'affiche
avec la rotation adéquate; et de 51 à 100%, la partie de gauche.

Pour me simplifier la vie, j'utilise les possibilités qu'offre Sass à savoir la boucle `for` et les
[`placeholder selectors`](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#placeholders).
Je peux ainsi très rapidement créer mes 101 classes.

Enfin, pour la rotation step-by-step, une simple règle de trois (`i * 360 / 100`) et un offset à
`-135deg` (qui correspond à l'affichage du degré zéro du demi-cercle) suffiront.

On met le tout dans une `mixin` Sass et on crée un petite directive AngularJS pour wrapper le HTML
et voilà le résultat :

<p data-height="268" data-theme-id="17274" data-slug-hash="OMrbLZ" data-default-tab="result" data-user="roparz" class='codepen'>See the Pen <a href='http://codepen.io/roparz/pen/OMrbLZ/'>cg-percent-pie directive</a> by roparz (<a href='http://codepen.io/roparz'>@roparz</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

Le tout est disponible sur [Github](https://github.com/roparz/cg-percent-pie) et installable
via `bower` avec la commande suivante : `bower install cg-percent-pie`.

Et voilà ! Merci de m'avoir lu ;-)

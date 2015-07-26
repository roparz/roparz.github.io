---
layout: post
title:  "gulp-sniffer, un plugin Gulp pour les analyser tous !"
date:   2015-05-08 18:05:00
---

> TLDR; ce post présente le pourquoi et le comment du plugin [gulp-sniffer](https://www.npmjs.com/package/gulp-sniffer)

Toutes les entreprises ont leurs propres conventions de codage et il est parfois
difficile de vérifier que chaque développeur les respecte bien (IDE différents,
turn-over, etc.).

D'autres fois, pour certains projets, on va vouloir vérifier que tel ou tel mot
n'est pas utilisé<!--break--> (ou, au contraire, est bien utilisé) dans les fichiers.

Par exemple, dans ma boite, on a un projet où on utilise une police spécifique qui
ne permet pas d'utiliser tous les `font-weight` possibles (100-900). On a donc
décidé de brider l'utilisation de cette propriété à certains mots-clés, à savoir :
`lighter`, `normal` et `bold`.

Pour cela, j'ai créé un plugin gulp qui va vérifier, à la compilation, le contenu
des fichiers et renvoyer une erreur si la condition n'est pas respectée.

Ce plugin, c'est [gulp-sniffer](https://www.npmjs.com/package/gulp-sniffer).

Et voici comment on l'utilise :

<script src="https://gist.github.com/roparz/b358c8fe71655cd47bf9.js"></script>

Cet autre exemple, très simple, permet de vérifier qu'aucune tabulation n'est utilisée
dans les fichiers :

<script src="https://gist.github.com/roparz/a708125d42ef337b56ae.js"></script>

Bref, vous l'aurez compris, si vous maîtrisez un peu les regex, les possibilités
sont infinies ;-)

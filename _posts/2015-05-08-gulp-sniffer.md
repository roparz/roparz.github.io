---
layout: post
title:  "gulp-sniffer, un plugin Gulp pour les analyser tous !"
date:   2015-05-08 18:05:00
---

> TLDR; ce post présente le pourquoi et le comment du plugin [gulp-sniffer](https://github.com/roparz/gulp-sniffer)

Toutes les entreprises ont leurs propres conventions de codage et il est parfois
difficile de vérifier que chaque développeur les respecte bien (IDE différents,
turn-over, etc.).

D'autres fois, pour certains projets, on va vouloir vérifier que tel ou tel mot
n'est pas utilisé (ou, au contraire, est bien utilisé) dans les fichiers.

Par exemple, dans ma boite, on a un projet où on utilise une police spécifique qui
ne permet pas d'utiliser tous les `font-weight` possibles (100-900). On a donc
décidé de brider l'utilisation de cette propriété à certains mots-clés, à savoir :
`lighter`, `normal` et `bold`.

Pour cela, j'ai créé un plugin gulp qui va vérifier, à la compilation, le contenu
des fichiers et renvoyer une erreur si la condition n'est pas respectée.

Ce plugin, c'est [gulp-sniffer](https://github.com/roparz/gulp-sniffer).

Et voici comment on l'utilise :

{% highlight javascript %}
var gulp = require('gulp'),
    sniffer = require('gulp-sniffer');

var sniffs = {
    'Use lighter|normal|bold instead of 100-900 for font-weight': function(content) {
        return content.match(/font-weight: \d+/);
    }
}

gulp.task('check-css', function() {
    gulp.src('src/**/*.css')
    .pipe(sniffer(sniffs))
    .pipe(gulp.dest('dist/'))
});
{% endhighlight %}

Cet autre exemple, très simple, permet de vérifier qu'aucune tabulation n'est utilisée
dans les fichiers :

{% highlight javascript %}
'This file uses tabs instead of spaces for indentation': function(content) {
    return content.match(/\t/g);
}
{% endhighlight %}

Bref, vous l'aurez compris, si vous maîtrisez un peu les regex, les possibilités
sont infinies ;-)

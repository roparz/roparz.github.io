---
layout: post
title:  "L'art du changelog"
date:   2017-08-13 04:00:00
---

En tant que Release Manager, j'ai la charge de générer un changelog lisible des features, bugs fixes, améliorations et autres devs entre chaque release. Ce changelog est ensuite réécrit par la team marketing pour le rendre user-friendly.<!--break-->

Pour réaliser ce changelog, je me base sur le log de Git qui, si les devs ont bien fait leur travail&nbsp;;-), contient toutes les informations utiles. À la base je le faisais manuellement, autant vous dire que c'était une tâche fastidieuse et peu intéressante. Le jour où j'en ai vraiment eu marre, j'ai commencé à chercher un moyen de le générer automatiquement. J'ai alors regardé du côté des libraries open-source très connues comme AngularJS. Ils ont tous un beau changelog qui, de toute évidence, est tout droit sorti des entrailles d'un générateur automatique... En creusant je me suis rendu compte que ce changelog était aussi généré à partir du log de Git, mais chaque commit avait l'air de suivre une règle bien précise.

Voici donc la recette pour générer un changelog tout beau tout propre si, comme moi, vous utilisez Git, NPM et Gulp :

Tout d'abord, il faut faire chier les développeurs ^^, et tout le monde sait qu'ils détestent qu'on change leurs habitudes. Mais c'est pour la bonne cause et, au final, ils vont vite se rendre compte que c'est quand même plus clair et surtout très utile. Je les ai donc obligé, dans chacun de nos repository, à respecter une convention de nommage pour les commits. Pour cela, j'ai utilisé un petit package NPM au doux nom de <a target="_blank" href="https://www.npmjs.com/package/angular-precommit">`angular-precommit`</a>.<br>
Ce plugins s'ajoute directement à Git via les hooks. Si vous ne connaissez pas, je vous conseille d'aller faire un tour <a target="_blank" href="https://git-scm.com/book/gr/v2/Customizing-Git-Git-Hooks">sur la doc de Git</a>.<br>
Voici comment on l'utilise :

{% highlight bash %}
$ npm install --save angular-precommit
$ ln -s node_modules/angular-precommit/index.js .git/hooks/commit-msg
{% endhighlight %}

Juste avant de créer le commit, Git va lancer le petit script qui va bien. Celui-ci va vérifier le format du commit, afficher un message s'il est incorrect et, si besoin, annuler la création commit. Pour être sûr que le script soit installé sur la machine de chaque développeur, je l'ai ajouté en `postinstall` du fichier `package.json`.

{% highlight json %}
{
  "name": "My Project",
  "scripts": {
    "postinstall": "ln -sf ../../node_modules/angular-precommit/index.js .git/hooks/commit-msg"
  }
}
{% endhighlight %}

Ainsi, quand le développeur lance un `npm install`, le hook Git s'ajoute automatiquement.

Voici à quoi doit ressembler un commit : `<type>(<scope>): <subject>`
* Où `type` est un mot clé au choix parmis `amend`, `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `chore` ou `revert`.
* `scope` correspond au module auquel le commit fait référence (voire "module/sous-module" dans certains cas).
* Et `subject`, la description habituelle du développement effectué.

Au bout de quelques semaines on a commencé à avoir un Git log propre et uniforme, c'était donc le moment de mettre en place la génération automatique du changelog. Pour cela j'ai utilisé le module <a target="_blank" href="https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/gulp-conventional-changelog">`gulp-conventional-changelog`</a> qui, en une seule commande, génère un changelog à partir des commits entre deux releases. Ce plugin gère différentes conventions de nommage dont celle d'Angular, ça tombe bien, c'est celle qu'on utilise ;-).<br>
Voici comment on l'implémente :

{% highlight javascript %}
const gulp = require('gulp'),
    conventionalChangelog = require('gulp-conventional-changelog');

gulp.task('changelog', () => {
    return gulp.src('CHANGELOG.md', { buffer: false })
        .pipe(conventionalChangelog({ preset: 'angular' }))
        .pipe(gulp.dest('./'))
});
{% endhighlight %}

Ensuite, vous n'avez plus qu'à créer un commit de release (en modifiant par exemple la clé `version` du fichier package.json).

{% highlight bash %}
git commit -m "chore(release): v1.0.0"
{% endhighlight %}

Puis taper `gulp changelog` dans votre terminal et le tour est joué !<br>
Le plugin va prendre tous les commits depuis la dernière release (à priori le dernier tag, mais j'avoue ne pas avoir regardé en détail comment ça fonctionne), les ordonner, les reformuler puis réécrire le fichier `CHANGELOG.md`. Et voilà le résultat (tiré du changelog de `gulp-conventional-changelog`) :

![changelog exemple](/assets/changelog.png)

Et voilà le secret pour sortir un super changelog de manière automatisé et, par la même occasion, donner quelque bonnes habitudes à vos développeurs pour bien nommer et typer leurs commits !

Happy coding!

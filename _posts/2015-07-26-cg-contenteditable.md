---
layout: post
title:  "cg-contenteditable, un directive AngularJS pour gérer l'attribut contenteditable"
date:   2015-07-26 18:00:00
---

Cela fait maintenant deux ans et demi que je travaille avec AngularJS comme framework
pour tous mes projets front. Au fil des projets, on amasse de plus en plus d'expériences
et certains bouts de codes deviennent indispensables. Voici donc le premier article
d'une série d'outils (directives, configs...) que j'intègre à presque tous mes projets
AngularJS<!--break-->.

AngalarJS gère très bien le "binding" des éléments `input`, `select` et `textarea` grâce à la
directive [`ngModel`](https://docs.angularjs.org/api/ng/directive/ngModel). En revanche,
pour l'attribut `contenteditable`, rien n'est prévu.

Voici donc la directive `cg-contenteditable` qui permet de combler ce manque (c'est
en CoffeeScript mais vous pouvez voir la version compilée dans le CodePen à la fin
de cet article) :

<script src="https://gist.github.com/roparz/b10425d9c5b97c22991c.js"></script>

L'idée de cette directive est de gérer l'attribut `contenteditable` de la même façon
qu'Angular gère les éléments ciblés par `ngModel`. Pour être cohérent au niveau du DOM
on va donc retrouver l'attribut `ng-model` qui nous servira à gérer le "double binding"
des données.

Cette directive se divise en trois parties. Dans un premier temps : (1) l'initialisation;
on ajoute l'attribut `contenteditable` à l'élément. Ensuite, (2) on va écouter l'événement
[`input`](https://developer.mozilla.org/en-US/docs/Web/Events/input) afin de mettre à jour
le `model` quand on tape dans l'élément. À noter l'utilisation de `scope.$evalAsync()`
qui va déclancher un "$digest cycle" pour véritablement mettre à jour l'objet représenté
par `ngModel`. Enfin (3) on regarde les changements sur le `ngModel` avec un `$watch`
pour les répercuter sur l'élément.

Pour finir, voici un petit CodePen qui montre comment on l'utilise :

<div data-height="268" data-theme-id="17274" data-slug-hash="QbZVgW" data-default-tab="html" data-user="roparz" class='codepen'><pre><code>angular.module(&#39;whatever&#39;, {})

angular.module(&#39;whatever&#39;).controller &#39;test&#39;, ($scope) -&gt;
    $scope.item =
        description: null

    $scope.$watch &#39;item.description&#39;, -&gt;
        console.log $scope.item

    $scope.item.description = &#39;test&#39;

angular.module(&#39;whatever&#39;).directive &#39;cgContenteditable&#39;, -&gt;

    restrict: &#39;A&#39;
    scope:
        ngModel: &#39;=&#39;
    link: (scope, elem, attrs) -&gt;
        elem = elem[0]
        elem.setAttribute &#39;contenteditable&#39;, true

        elem.addEventListener &#39;input&#39;, -&gt;
            return if scope.ngModel is elem.innerHTML
            scope.ngModel = elem.innerHTML
            scope.$evalAsync()

        scope.$watch &#39;ngModel&#39;, -&gt;
            return unless scope.ngModel
            return if scope.ngModel is elem.innerHTML
            elem.innerHTML = scope.ngModel
</code></pre>
<p>See the Pen <a href='http://codepen.io/roparz/pen/QbZVgW/'>QbZVgW</a> by roparz (<a href='http://codepen.io/roparz'>@roparz</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
</div><script async src="//assets.codepen.io/assets/embed/ei.js"></script>

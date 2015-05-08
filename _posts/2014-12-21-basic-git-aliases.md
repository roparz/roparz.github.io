---
layout: post
title:  "Quelques alias Git de base"
date:   2014-12-21 19:00:00
---

> TLDR; présentation/explication de mon fichier [.gitconfig](https://gist.github.com/roparz/de0e662fb980bdd8284e)

Pour ce tout premier post je vais vous partager les quelques alias Git que j'utilise
tous les jours (ou presque). Je partagerai plus tard d'autre alias maison un peu plus
complexe ;-)

Avant tout vous devez savoir que j'utilise une configuration spécifique de Git :
{% highlight bash %}
[push]
  default = simple
{% endhighlight %}
Cette configuration force les commandes Git à ne s'exécuter que sur la branche
courante. On peut donc faire un `git pull` ou un `git push` sans avoir à préciser
le nom de la branche.

Voici donc quelques-uns des alias que j'utilise :

{% highlight bash %}
$ cat ~/.gitconfig
[alias]
  st = status
  ci = commit # En général je fais `git ci -m "commit message"`
  co = checkout
  br = branch
  rz = reset --hard HEAD # Annule toutes les modifications pas encore commitée. À utiliser avec prudence ;-)
  cp = cherry-pick
  ...
{% endhighlight %}

L'alias suivant est probablement le plus pratique de tous, il permet d'avoir un git log clair et compact.
{% highlight bash %}
lg = log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
{% endhighlight %}
En voici un exemple (git log d'Angular) :
![git log exemple](/assets/git-log.png)

Dans ma boite, on utilise le `rebase` plutôt que le `merge`. Il arrive donc parfois
que l'arbre des commits soit réécrit. On ne peut donc plus faire de simple `git pull`
pour mettre à jour une branche, on risquerait de dupliquer des commmits et de créer
des conflits de malade.<br/>
Il faut donc utiliser `git pull -- rebase`, d'où le petit alias suivant :
{% highlight bash %}
pr = pull --rebase
{% endhighlight %}

Les deux alias suivants sont vraiment très pratique, même s'ils nécessitent d'avoir
un workspace complètement clean (pas de fichier foireux qui reste et qu'on fait
exprès de ne jamais commiter).<br/>
Le premier permet de commiter l'intégralité de son workspace sous le message 'wip'.<br/>
Le second permet d'annuler le dernier commit en gardant son contenu.
{% highlight bash %}
wip = !git add --all && git ci -am "wip"
unwip = reset HEAD^
{% endhighlight %}
J'utilise très souvent ces deux alias, notemment quand je dois mettre en pause
un travail en cours pour corriger un bug sur une autre branche par exemple.

Enfin, voici deux autres alias que j'utilise quand je me rends compte que j'ai
oublié un truc dans un commit. Je fais mes modifs et je lance une des deux commandes,
en fonction du besoin.<br/>
La première ajoute tous les fichiers et écrase le dernier commit (`amend`).<br/>
La seconde fait la même chose avec un plus un `push -f` qui écrase la branche distante.
{% highlight bash %}
amend = !git add --all && git ci --amend
erase = !git amend && git push -f
{% endhighlight %}

Et voilà, c'est tout pour le moment :-)

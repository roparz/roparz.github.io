---
layout: post
title:  "Deux alias Git pour gérer ses tags"
date:   2015-01-24 18:42:00
---

Aujourd'hui, après vous avoir parlé de mes [alias Git de base](/2014/12/21/basic-git-aliases.html)
et de [mon alias préféré `git fix`](/2015/01/05/git-fix.html), je vais vous parler
de deux alias Git que j'utilise régulièrement dans ma boite pour gérer les tags.

Mais tout d'abord, je dois vous parler de ma configuration Git. En effet<!--break-->, par défaut
dans Git, un simple `git fetch` ne met pas à jour les tags, il faut pour cela faire
un `git fetch --tags`. Comme cette commande ne met à jour *que* les tags, il faut,
pour être complet, faire un `git fetch && git fetch --tags`. Fastidieux...

Pour résoudre ce problème, il vous suffit d'ajouter ces deux lignes à votre
`.gitconfig` :

{% highlight bash %}
[remote "origin"]
    fetch = +refs/tags/*:refs/tags/*
{% endhighlight %}

Avec ça, un simple `git fetch` mettra à jour tous les tags de votre branche.

Maintenant que vos tags sont bien mis à jour, vous allez pouvoir jouer avec,
et pour ça, rien de tel que quelques alias bien fait pour se faciliter la vie :-)

Le premier, `rmtag`, est celui que j'utilise le moins souvent. Il est néanmoins
très pratique puisqu'il permet tout simplement de supprimer un tag en local *et*
sur la branche distante. Je l'utilise en général quand je me suis trompé de tag
et que j'ai déjà pushé...

On l'utilise comme ça : `git rmtag MON_TAG`.

{% highlight bash %}
rmtag = "!f() {\
    tag=$1;\ # 1
    git fetch --tags;\ # 2
    git tag -d ${tag};\ # 3
    git push origin :refs/tags/${tag};\ # 4
}; f"
{% endhighlight %}

1. je récupère le nom de mon tag
2. je fetch pour être sûr d'avoir tous les tags
3. je supprime le tag localement
4. je push mes tags locaux vers l'origine (ce qui supprime le tag en question)

Le second alias, `uptag`, permet de mettre à jour un tag. Je l'utilise par exemple pour
mettre à jour des tags d'environnements (TEST, RECETTE, PRODUCTION, etc.).
Grâce à [l'alias `git lg` vu précédemment](/2014/12/21/basic-git-aliases.html),
on peut rapidement avoir une vue d'ensemble de ces tags et donc de savoir où en
est tel ou tel environnement.

On l'utilise comme le précédent : `git uptag RECETTE`.

{% highlight bash %}
uptag = "!f() {\
    tag=$1;\
    git fetch --tags;\
    git tag -d ${tag};\
    git push origin :refs/tags/${tag};\
    git tag ${tag};\ # 1
    git push --tags;\ # 2
}; f"
{% endhighlight %}

Les quatres premières lignes sont les mêmes que pour `rmtag`, je ne reviens pas
dessus.

1. je tag le commit courant
2. je push les tags

And... voilà :-)

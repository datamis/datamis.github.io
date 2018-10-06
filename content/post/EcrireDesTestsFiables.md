+++
title = "Ecrire des tests fiables"
description = ""
tags = [
"selenium",
"webdriver",

]
date = "2018-10-02"
categories = [
    "Development",
]
author = ""
language = "fr"
+++
 *traduction française de l'article :*
 [Writing reliable locators for selenium and webdriver tests/](https://blog.mozilla.org/fxtesteng/2013/09/26/writing-reliable-locators-for-selenium-and-webdriver-tests/)
 ( encours )
# Ecrire des tests fiables pour Selenium et Webdriver

Si vous venez ici pour trouver le locator parfait et incassable, je suis au regret de vous dire qu'il n'existe pas. Du fait des évolutions de l'HTML, les problèmes de compatibilités des locators sont monnaies courantes dans l'écriture des test automatisés. Vous n'aurez jamais fini de les mettre à jour comme l'équipe de developpement l'experimente avec le design, les factorisations du code HTML et les correctifs de bugs, et ce aussi longtemps que votre web app evolue. La maintenance des locators tient une place entière dans l'estimation des couts des tests de maintenance.

Cependant, la bonne nouvelle est qu'il existe une différence entre une bonne et mauvaise écriture de locators. Ainsi, en prenant soin d'adopter les bonnes pratiques, vous pourrez réduire le cout de la maintenance et vous concentrer sur des taches plus importantes que le debugging.

D'un autre coté, il ne faut pas avoir peur d'un locateur en échec. Car l'exception  `NoSuchElementException`, plus qu'un échec d'assertion, est souvent le signe avant-coureur d'une regression dans votre programme.

In this guide, nous supposerons que vous savez comment écrire un locator et êtes familier avec la construction et la syntaxe du CSS et des locateurs XPath. Nous creuserons alors les différences entre un bon et mauvais locators dans le contexte de l'écriture de tests Selenium.

## Les Ids sont rois !

Les locators d'après Id sont l'option la plus sûre et devrait être toujours votre premier choix. D'après les standards du W3C, ils doivent être uniques dans la page et de ce fait vous ne devriez avoir aucun problème de trouver un seul et unique élément correspondant au locator.

L'ID est aussi indépendant du type de l'élément et de sa position dans l'aborescence. En conséquence, si le developpeur deplace l'élément ou change son type, Webdriver pour toujours le localiser.

De plus, les Ids sont souvent utilisés directement dans le code Javascript de la page web puisqu'il evite au développeur changeant l'id d'un élément, d'avoir à changer aussi son code Javascript. Ce qui est génial pour nous testeurs !

Si vous avez des developpeurs flexibles ou si vous jetez vous même un oeil dans le code source de l'application, vous pourrez sans peine ajouter les Ids supplémentaires en un rien de temps. Bien qu'il soit possible d'ajouter des Ids partout, cette solution demeure parfois indapté voir impossible à mettre en oeuvre. C'est pourquoi, nous avons besoin d'utiliser les locators CSS et Xpath.

## Les locators CSS and Xpath

Les locators CSS et Xpath sont conceptuellement très similaires, nous les avons donc réunis ensemble dans le cadre de cette discussion.

Ces types de locateurs, produits de la combinaison de tags, d'éléments descendants, de classes CSS ou encore d'attributs d'éléments, rend la correspondance de pattern stricte ou perdante. *Stricte* signifie que le moindre changement HTML viendra invalider le locator, et *perdante* signifie la correspondance n'est plus unique et que le locator peut renvoyer à plus d'un élément HTML.

Lorsque l'on écrit un locateur CSS ou Xpath, il faut donc trouver le bon équilibre entre *stricte* et *perdante* ( strict or loose), suffisament durable pour supporter les changements du code HTML et suffisament strict pour échouer lorsque l'application échoue.

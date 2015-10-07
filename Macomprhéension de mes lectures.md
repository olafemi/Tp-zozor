										Ma comprhéension de mes lectures

#===================================================================================================================#
													:hover
#===================================================================================================================#

D'abord on ne peut utiliser l'attribut {:hover} sans utiliser l'attribut {a} dans ce qu'on veut styliser en css

The following patterns will cause an error:

```css

/* :hover is not anchored with an a tag*/
.test :hover{
    top: 10px;
}

.test .event-link:hover {
    top: 10px;
}

```

The following pattern is considered okay and does not cause an error:

```css
/* :hover is anchored with an a tag*/
.test a:hover{
    top: 10px;
}

.test a.event-link:hover {
    top: 10px;
}
```


#===================================================================================================================#
													box-sizing : border-box;
#===================================================================================================================#

Quand on veux styliser une boite (on peut encore dire block si on veut ou box) et qu'on fait 

```css
.mybox {
    border: 1px solid black;
    padding: 5px;
    width: 100px;
}
```
on se dit que la largeur du block serait de 100 px 
que la bordure solide de 1px serait sur le 100pixel du bloc ce qui laissera 98 px
car cela s'applique a gauche et a droite
et puisque le padding est la marge qui sépare la bordure de l'intérieur on aura un retranchement
de 5px vers l'intérieur en plus
d'ou un total de 6px par côté de marge intérieure et qu'au total pour la largeur de la boîte on aurais
100 pixel - 12 pixel = 88 pixel.

Mais ça ne se passe pas comme cela,
lorsqu'on applique directement

```css
.mybox {
    border: 1px solid black;
    padding: 5px;
    width: 100px;
}
```
on obtiens un bloc de 112 pixel 
100px "l'intérieur du block" + 1px "pour chaque côté des bordure" + 5px pour une marge intérieure entre la bordure et les 100 pixel qui sont la taille du block 
car le padding et le margin augoumente la taille de la boite et non la réduire!
pour obtenir le résultats desirer {c'est t'a dire une boîte qui fait juste 100 pixel} on doit utiliser la propreté box-sizing : border-box;

```css
.mybox {
    box-sizing: border-box;
    border: 1px solid black;
    padding: 5px;
    width: 100px;
}
```

si vous ecrivez {ceci vaut aussi pour {height} qui désigne la hauteur} ceci :

The following patterns are considered warnings:

```css

/* width and border */
.mybox {
    border: 1px solid black;
    width: 100px;
}

/* height and padding */
.mybox {
    height: 100px;
    padding: 10px;
}
```

sublime linter considère que vous ne savez pas ce que vous ecrivez, car ne savez pas que cela induirait
une boite de 112 pixel et vous allerte.

Mais si vous ecrivez :

The following patterns are considered okay and do not cause warnings:

```css
/* width and border with box-sizing */
.mybox {
    box-sizing: border-box;
    border: 1px solid black;
    width: 100px;
}

/* width and border-top */
.mybox {
    border-top: 1px solid black;
    width: 100px;
}

/* height and border-top of none */
.mybox {
    border-top: none;
    height: 100px;
}

```

il ne génère pas d'erreur car il se dit que vous connaissez les implications de votre code.


#===================================================================================================================#
													@font-face
#===================================================================================================================#

L'utilisation de @font-face pour  définir une polices personaliser peut être une source de problème sur internet explorer
pour cela une mauvaise utilisation de la polices génère une erreur au niveau de css lint.

```css
@font-face {
    font-family: 'MyFontFamily';
    src: url('myfont-webfont.eot') format('embedded-opentype'), 
        url('myfont-webfont.woff') format('woff'), 
        url('myfont-webfont.ttf')  format('truetype'),
        url('myfont-webfont.svg#svgFontName') format('svg');
}
```
cette syntax ainsi énumerer causera une erreur 404 sur internet explorer 6, 7 et 8
car au niveau de la première ligne apres la définition {myfont-webfont.eot} on doit ajouter un point d'interrogation "?" et
spécifié "iefix" pour que dans le traitement du code arriver a cette ligne internet explorer puisse continuer la lecture.

```css
@font-face {
    font-family: 'MyFontFamily';
    src: url('myfont-webfont.eot?#iefix') format('embedded-opentype'), 
        url('myfont-webfont.woff') format('woff'), 
        url('myfont-webfont.ttf')  format('truetype'),
        url('myfont-webfont.svg#svgFontName') format('svg');
}
```
ceci, est la syntaxe corecte celle qui ne génèrera aucune erreur car il y ajout du {?#iefix}

Mais cependant il reste un problème a fixer :
Quand on veux utiliser la polices @font-face, on doit s'assurer que la première
spécification de polices est celle ci { url('myfont-webfont.eot?#iefix') format('embedded-opentype') }
si par hasard on place une autre ligne avant celle énumerer pour définir les différents format des polices
personaliser, cela engendre une erreur dans ie6, 7 et 8

pour bonne pratique on doit alors s'assurer que les premières ligne pour définir une polices personnaliser sont celle
ci :

```css
@font-face {
    font-family: 'MyFontFamily';
    src: url('myfont-webfont.eot?#iefix') format('embedded-opentype'), 
}
```
l'ordre de définitions du reste des polices importe peu, mais il serait mieux de les conserver en un format standart

```css
@font-face {
    font-family: 'MyFontFamily';
    src: url('myfont-webfont.eot?#iefix') format('embedded-opentype'), 
        url('myfont-webfont.woff') format('woff'), 
        url('myfont-webfont.ttf')  format('truetype'),
        url('myfont-webfont.svg#svgFontName') format('svg');
}
```

#===================================================================================================================#
													@import
#===================================================================================================================#

La commande @import est utiliser pour inclure du css dans du code css, c'est a dire importer un fichier css existant dans
un autre fichier css qu'on est entrain de créer.

```css
@import url(more.css);
@import url(andmore.css);

a {
    color: black;
}
```

ce code permet d'importer deux nouveau fichier css au début des anciens.

cette rêgle est utilisé pour agancer plusieurs css ensemble avant de les faires télécharger par le naviguateur ou
est utiliser pour le téléchargement en parallèle d'un autre fichier css pendant la lecture de celui en cour.
Mais il faut noter qu'a la rencontre de ce code, le naviguateur stop la lecture du code en cours et ne la reprend que lorsqu'il aura finis l'importation de celle spécifier dans import.
elle peut donc servir si on veut faire apparaitres des rêgles css dans un certains ordre et par prioriter.

#===================================================================================================================#
                                                    Adjoining classes
#===================================================================================================================#
Cette partie concerne l'ajancement de classe ou plus tôt l'énumération a la suite de plusieurs classes pour une stilisatiion  général tel que `.foo.bar.h1`

Techniquement l'énumération de plusieurs classe a la suite est permis en css, et donc ne devrais pas être considerer 
comme une erreur, cependant l'utilisation de tel propriété génère des notifications d'erreurs par css lint.

Ceci est due a Internet explorer, 
oui internet exploreur qui crée toutes les galère du monde :-) Mdr :-D 
sur les versions d'Ie 6 et inférieur
lorsqu'on ecrit une rêgle comme celle ci `.foo.bar` le naviguateur ne lit pas le '.foo' il considère juste le '.bar' 
du coup ceci créera des bugs dans la génération de votre page.

Etant donné que css lint vous signale tous les réglages qui pourraient créer des bugs ou des erreurs dans la génération de votre page ils soulignent ceci comme étant une erreur.

Bon ici a vous de choisir vos habitudes, personnellement je ne suivrai pas cette rêgle de csslinter au risque de ne pas avoir les utilisateurs d'ie 6 et inférieur, mais peu m'importe

Donc a vous de choisir si vous allez apliqué la rêgle vu le nombre d'utilisateurs qui pourrait accéder a votre site, via des navigateurs obsolètes en prenant en compte l'impacte économique surtout quand vous êtes un site commerciale.
faut noté qu'il existe un scipt pour forcer le visiteur a mettre a jour son naviguateur.

                                si vous vouler en savoir plus sur ce sujet lisez ceci :

                    ////////////////////////////////////////////////////////////////////////////////////////
                    ////////////////////////////////////////////////////////////////////////////////////////
                    ////                                                                                ////
                    ////    Generally, it's better to define styles based on single classes instead     ////
                    ////    of based on multiple classes being present. Consider the following:         ////
                    ////                                                                                ////
                    ////                ```css                                                          ////
                    ////                    .foo {                                                      ////
                    ////                        font-weight: bold;                                      ////
                    ////                    }                                                           ////
                    ////                   .bar {                                                       ////
                    ////                       padding: 10px;                                           ////
                    ////                   }                                                            ////
                    ////                   .foo.bar {                                                   ////
                    ////                       color: red;                                              ////
                    ////                   }                                                            ////
                    ////                   ```                                                          ////
                    ////    The rule for selector `.foo.bar` can be rewritten as a new class:           ////
                    ////               ```css                                                           ////
                    ////                   .foo {                                                       ////
                    ////                       font-weight: bold;                                       ////
                    ////                   }                                                            ////
                    ////                    .bar {                                                      ////
                    ////                       padding: 10px;                                           ////
                    ////                   }                                                            ////
                    ////                                                                                ////
                    ////                    .baz {                                                      ////
                    ////                       color: red;                                              ////
                    ////                    }                                                           ////
                    ////                 ```                                                            ////
                    ////    That new class, `baz`, must now be added to the original HTML element. This ////
                    ////    is actually more maintainable because the `.baz` rule may now be reused     ////
                    ////    whereas the `.foo.bar` rule could only be used in that one instance.        ////
                    ////                                                                                ////
                    ////                                                                                ////
                    ////////////////////////////////////////////////////////////////////////////////////////
                    ////////////////////////////////////////////////////////////////////////////////////////

En gros ce qui est dit dans cette paranthèse stipule qu'a chaque fois vous devez utilisé plusieurs classes a la fois qu'il serait mieux d'ajouter au Html une nouvel classe portant le même nom a tous les éléments considéré

The following patterns are considered warnings:

```css
.foo.bar {
    border: 1px solid black;
}

.first .abc.def {
    color: red;
}
```

The following patterns are considered okay and do not cause a warning:

```css
/* space in between classes */
.foo .bar {
    border: 1px solid black;
}
```

ici il faut noté l'espace entre ".foo" et ".bar"


#===================================================================================================================#
                                                    Adjoining classes
#===================================================================================================================#

Csslint veut pour vous que votre site sois le plus léger possible.
Ainsi lorsque vous utiliser dans une même feuille de style css deux fois le même back ground pour deux différents 
classes ou ID il vous signale une erreurs,

En effet csslint propose de défilir une classes avec le background voulu et de l'appliquer a chaque fois que vous en avez besoin dans votre css ainsi on à :

Consider the following:

```css
.heart-icon {
    background: url(sprite.png) -16px 0 no-repeat;
}

.task-icon {
    background: url(sprite.png) -32px 0 no-repeat;
}
```

l'immage d'arrière plan est répété dans les deux classes, celà génère des bytes en surplu, pour que votre css ne soit pas inutilement lourd csslint vous avertis en générant un messages.
Une alternative est de définir une classes avec l'immages d'arrière plan voulu et d'ajouter cette classes au code html,
ainsi au lieu de mettre le back-ground a chaque fois qu'on en a besoin, on le définit une fois pour toutes et on l'ajoute en html l'à où on l'a désire

```css
.icons {
    background: url(sprite.png) no-repeat;
}

.heart-icon {
    background-position: -16px 0;
}

.task-icon {
    background-position: -32px 0;
}
```

## Rule Details

Rule ID: `duplicate-background-images`

This rule is aimed at ensuring the same URLs aren't used more than once in a style sheet.

The following patterns are considered warnings:

```css
/* multiple instances of the same URL */
.heart-icon {
    background: url(sprite.png) -16px 0 no-repeat;
}

.task-icon {
    background: url(sprite.png) -32px 0 no-repeat;
}
```

The following patterns are considered okay and do not cause warnings:

```css
/* fallback color before newer format */
.icons {
    background: url(sprite.png) no-repeat;
}

.heart-icon {
    background-position: -16px 0;
}

.task-icon {
    background-position: -32px 0;
}
```


#===================================================================================================================#
                                                    Adjoining classes
#===================================================================================================================#
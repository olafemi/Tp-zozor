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

Quand on veux styliser une boite on peut encore dire block si on veut ou box et qu'on fait 
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
de 5px en plus
d'ou un total de 6px par côté et qu'au total on aurais
100 pixel - 12 pixel = 88 pixel.

Mais x ne se passe pas comme cela,
lorsqu'on applique directement
```css
.mybox {
    border: 1px solid black;
    padding: 5px;
    width: 100px;
}
```
on obtiens un bloc de 112 pixel car le padding et le margin augoumente la taille de la boite et non ne la réduise!
pour obtenir le résultats desirer on doit utiliser la propreté box-sizing : border-box;

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
car qu niveau de la première ligne apres la définition {myfont-webfont.eot} on doit ajouter un point d'interrogation "?" et
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
Quand on ceux utiliser la polices @font-face, on doit s'assurer que la première
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
													@font-face
#===================================================================================================================#



# Du poison [1/2]

## Rappel de l'énoncé
Ce challenge est un challenge d'introduction à l'empoisonnement de modèles d'IA, et en particulier ceux utilisant la technique d'apprentissage fédéré.
Le but est ici de faire descendre un maximum la précision du modèle.

> Je vous encourage vivement à utiliser Google Colab pour les challenges d'Intelligence Artificielle pour plusieurs raisons :
> 1. Eviter de "polluer" votre environnement avec des librairies Python diverses
> 2. Eviter les problèmes de compatibilité entre les librairies et s'affranchir de l'installation de certaines, comme Tensorflow
> 3. Utiliser les GPU de google, ce qui est très agréable car très rapide

## Solution proposée
Pour faire descendre la précision du modèle, c'est très simple, il suffit de l'entrainer sur des données bullshit, qui n'ont aucun sens.

Le dataset (jeu de données) est composé de 4 jeux : x_train, y_train, x_test, y_test. Le modèle apprend sur le jeu "train" et teste ses "connaissances" sur le jeu de "test". Les "x" sont les images que le modèle doit apprendre à classifier, les "y" sont les résultats attendus.

Ici, le dataset est un dataset bien connu : MNIST. Il s'agit d'un ensemble de 10000 images de chiffres manuscrits. En "x" on a donc des images de tailles 254x254 en noir et blanc et en "y" un tableau de 10 valeurs : un "True" à la position dans le tableau correspondant à la classe de l'image, et des "False" ailleurs. Exemple : l'image représente un "2", le tableau comprend un "True" à l'index 2 du tableau : ```[False, False, True, False, False, False, False, False, False, False]```

Pour empoisonner le modèle, j'ai simplement remplacé toutes les valeurs de y_train par "True". 
```
y_train = numpy.full((10000, 10), True)

```
Si l'entrainement se passe bien, le modèle prédira toujours ```[True, True, True, True, True, True, True, True, True, True]```, ce qui n'ai pas vrai et fera donc baisser la précision du modèle fédéré. On aurait également pu mettre d'autres valeurs uniques comme ```[True, False, False, False, False, False, False, False, False, False]```, ou des valeurs aléatoires, il faut simplement respecter le format de la prédiction attendu : des tableaux de 10 booléens.

## Flag
404CTF{0h___dU_P01sON}

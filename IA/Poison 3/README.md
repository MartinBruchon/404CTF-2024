# Du poison [3/2]

## Rappel de l'énoncé
On change de dataset et de modèle. Ici, il s'agit de données tabulaires et d'un modèle de régression. On doit changer les valeurs de deux poids dans le modèle pour inverser la prédiction du modèle.

> Je vous encourage vivement à utiliser Google Colab pour les challenges d'Intelligence Artificielle pour plusieurs raisons :
> 1. Eviter de "polluer" votre environnement avec des librairies Python diverses
> 2. Eviter les problèmes de compatibilité entre les librairies et s'affranchir de l'installation de certaines, comme Tensorflow
> 3. Utiliser les GPU de google, ce qui est très agréable car très rapide

## Solution proposée
Ma méthodologie sur ce coup là est semi-bourrine. Si vous avez une meilleure solution, n'hésitez pas à m'en faire part, de même que plus d'explications sur la méthodo à avoir !

Initialement, j'obtiens la prédiction suivante sur le jeu de test :
```
{'25a': 24.904825, '25b': 25.188284, '50a': 55.799343, '50b': 46.487675}
```

D'abord, je regarde la structure du modèle (des poids du modèle). Les couches 3 et 4 comportent le moins de valeurs, je me dis donc que les poids ont donc plus d'importance sur ces couches. J'essaie de changer quelques poids sur la couche 4, en me disant que les poids de cette couche influent plus sur la sortie du modèle puisque qu'ils en sont le plus proche. Je teste différentes valeurs sur différents poids pour voir leur influence sur le résultat. Le premier poids de la couche 4 a un gros impact sur la sortie du modèle. Je constate également que même si je peux faire varier la sortie du modèle, les prédictions sur les 50 sont toujours plus grande que les prédictions sur les 25 en valeur absolue:
```
weight[4][0] = 5 :  {'25a': 151.2748, '25b': 152.222, '50a': 359.0361, '50b': 306.48758}
weight[4][0] = 0 :  {'25a': 21.7037, '25b': 21.970346, '50a': 48.117947, '50b': 39.90152}
weight[4][0] = -5 : {'25a': -107.86741, '25b': -108.2813, '50a': -262.80026, '50b': -226.68451}
```

De plus, dans le Jupyter Notebook du challenge, il est dit que si la sortie du modèle est x, la classe prédite est :
- 25 si |25 - x| < |50 - x|
- 50 sinon

Actuellement, le modèle prédit environ 25 pour les classes 25 et 50 pour les classes 50 (normal en fait).
Je vois deux possibilités pour empoisonner le modèle :
1. Modifier des poids pour faire prédire 50 à la place d'un 25 et 25 à la place d'un 50 (logique mais a priori pas possible)
2. Modifier les poids pour faire prédire des valeurs négatives

En effet, concernant le point deux :
- Si x = 25 : |25 - 25| < |50 - 25| -> pred = 25
- Si x = 50 : |25 - 50| > |50 - 50| -> pred = 50

Mais :
- Si x = -25 : |25 - (-25)| > |50 - (-25)| -> pred = 50
- Si x = -50 : |25 - (-50)| < |50 - (-50)| -> pred = 25

Et j'ai réussi à obtenir ce résultat en modifiant le premier poids de la couche 4 avec -1.
Cependant j'obtiens le message suivant :
```Raté ! Le modèle a obtenu une précision sur les classes inversée de 0.41379310344827586, il faut au moins 0.7```

En choisissant arbitrairement de modifier le dernier poids de la couche 4, je remarque que plus il est grand, plus le modèle est précis :
```
weight[4][-1] = 1 : Raté ! Le modèle a obtenu une précision sur les classes inversée de 0.5172413793103449, il faut au moins 0.7
weight[4][-1] = 2 : Raté ! Le modèle a obtenu une précision sur les classes inversée de 0.5862068965517241, il faut au moins 0.7
weight[4][-1] = 3 : Raté ! Le modèle a obtenu une précision sur les classes inversée de 0.6206896551724138, il faut au moins 0.7
```
Et finalement je flag avec ```weight[4][0] = -1``` et ```weight[4][-1] = 8```

## Flag
404CTF{d3_p3t1ts_Ch4ng3m3ntS_tR3s_cHA0t1qU3s}

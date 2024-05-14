# Des portes dérobées

## Rappel de l'énoncé
On reprend le datset MNIST et on souhaite "backdoorer" le modèle afin qu'une image représentant un 2 avec un H à côté soit reconnue comme étant un 1. 

> Je vous encourage vivement à utiliser Google Colab pour les challenges d'Intelligence Artificielle pour plusieurs raisons :
> 1. Eviter de "polluer" votre environnement avec des librairies Python diverses
> 2. Eviter les problèmes de compatibilité entre les librairies et s'affranchir de l'installation de certaines, comme Tensorflow
> 3. Utiliser les GPU de google, ce qui est très agréable car très rapide

## Solution proposée
De la catégorie IA, c'est mon challenge préféré parce qu'il m'a pris la tête un petit moment avant de réaliser que j'avais déjà fait 95% du travail depuis très longtemps et qu'il ne me manquait plus qu'une petite étape pour flag.

On appelle "chiffre patché" un l'image d'un chiffre avec un "patch" de H posé à côté.
La première étape ici est d'entrainer notre modèle afin qu'il puisse reconaître normalement les chiffres 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, de même que tous les chiffres patchés, sauf le 2 patché qu'il doit classer en 1.

J'ai codé quelques fonctions pour me générer un bon dataset. En fait, mes résultats étaient déjà très bon dès le début, mais j'obtenais toujours la même erreur :
``` {'message': "Raté ! La précision du modèle est bonne, mais la backdoor n'a pas fonctionné. Le modèle est efficace sur les images non patchées (comme voulu), mais sur les images patchées, il y a des erreurs : \nLe modèle prédit : 6, alors que c'est un 2 patché. Il devrait prédire 1.\nLe modèle prédit : 2, alors que c'est un 2 patché. Il devrait prédire 1.\n\nAttention, le patch peut être posé n'importe où sur l'image. De plus, c'est de l'apprentissage fédéré. Ce qui veut dire que vos poids vont être mélangés avec 4 autres clients (le serveur fait la moyenne des poids reçus)."} ```
En fait j'avais oublié qu'on restait dans le cadre d'apprentissage fédéré, et que le serveur fait la moyenne des poids de 5 clients au total. Il suffisait juste de multiplier tous nos poids par 5 pour avoir plus d'importance dans les résultats du serveur, sans modifier le comportement de notre modèle, et ainsi l'imposer au serveur.
Le notebook que je mets donc à disposition représente donc un entrainement optimisé, mais qui n'était pas nécessaire pour flag.

## Flag
404CTF{S0uRc3_peU_f14bL3}

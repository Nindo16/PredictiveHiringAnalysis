
## Statistiques descriptives

En vue d'effectuer des prédictions, il nous faut dans un premier temps se familiariser avec le jeu de données. On importe donc celui-ci et effectuons quelques observations.

- Il contient 20 000 lignes et 13 colonnes.
- 6 variables qualitatives.
- L'individu moyen a 35 ans, 9 ans d'expériences, un salaire de 35 000 et une note de 75.

Avant de se lancer dans des analyses statistiques une peu plus profondes, il convient de nettoyer un peu le jeu de données. Notre variable cible "embauche" n'est pas perçu comme une variable quantitative, certains de nos attributs possède des valeurs aberrantes. Des valeurs négatives ou trop faibles dans les variables âge et expérience. Quelques colonnes redondantes (index, Unnamed: 0). Au sein de notre dataset, il n'y a que peu de valeur manquante. Après avoir effectué tous ces traitements, il nous reste comme variable intéressante pour notre prédiction: cheveux, age, exp, sexe, diplome, specialite, note, dispo, embauche.

![Variable spécialité en fonction de la variable sexe](https://raw.githubusercontent.com/Nindo16/PredictiveHiringAnalysis/main/img/specialite_sexe.png)

Cette représentation nous suggère que la proportion d'hommes ayant pris la spécialité géologie est beaucoup plus grande que chez les femmes. On observe dans un deuxième temps que les spécialités archéologie et détective sont plus choisies par les femmes.

Il est possible de quantifier ce lien à l'aide des outils statistiques développées en ce sens, les tests d'hypothèse. Nous sommes dans un cas où nous essayons d'établir un lien entre deux variables qualitatives, notre choix se porte donc vers le test d'indépendance du Chi deux. Dans un premier temps, nous affichons le tableau de contingence associé afin de s'assurer d'être dans les conditions idéales. Puis on décide à la vue de notre p-valeur (0.0) de rejeter l'hypothèse nulle à savoir, l'indépendance des deux variables.

![Variable cheveux en fonction de la variable salaire](https://raw.githubusercontent.com/Nindo16/PredictiveHiringAnalysis/main/img/cheveux_salaire.png)

On réalise un test non-paramétrique, celui de Kruskal, afin de tester si la médiane est la même pour chaque couleur de cheveux. Notre p-valeur étant également très faible, cela suggère que la répartition des salaires au sein des différentes catégories n'est pas la même.

![Variable expérience en fonction de la variable note](https://raw.githubusercontent.com/Nindo16/PredictiveHiringAnalysis/main/img/exp_note.png)

Cette représentation graphique ne nous conforte pas dans l'idée que plus nous sommes expérimentés, plus nous obtenons une note élevée. Après avoir placé les individus dans 4 groupes différents (entre 0 et 5 ans d'expérience, entre 5 et 10 ans d'expérience, etc.). Nous réalisons le même test que précédemment et cette fois-ci notre p-valeur est de 0.37, la médiane serait donc la même dans les différents groupes, on s'attendait à ce qu'elle soit plus élevée pour les seniors.

---

## Machine Learning

À des fins de création de modèle de machine learning, on commence par encoder nos variables catégoriques, puis on sépare aléatoirement notre jeu de données en une partie d'entraînement et une partie de test. Notons que la variable cible compte environ 16 000 individus appartenant à la classe 0 pour seulement 2000 dans la classe 1, cela aura son influence sur notre modèle. On choisit ici d'utiliser Xgboost, un des meilleurs algorithmes de sa catégorie. Voici la matrice de confusion obtenue:

|       |     |
|-------|-----|
|  4737 |  94 |
|   365 | 261 |

Concernant les prédictions portant sur la classe 0, le modèle semble assez performant, cependant, il semble pêcher un peu plus pour la classe 1. Même en ayant une accuracy de 0.92, c'est-à-dire un taux de bonne classification sur l'ensemble du jeu de test de 92%, le rappel concernant la classe 1 est de 0.42, le modèle a du mal à détecter cette classe. En optimisant notre modèle, nous pourrions chercher à obtenir de meilleures valeurs pour cette métrique. Il semble plus important de ne pas se tromper lorsque l'on embauche quelqu'un de ce point de vue-là, le modèle a des résultats satisfaisants.

![Courbe ROC](https://raw.githubusercontent.com/Nindo16/PredictiveHiringAnalysis/main/img/courbe_ROC.png)

Cette illustration nous indique que notre modèle est déjà beaucoup plus performant qu'une classification faite aléatoirement. L'aire sous notre courbe est de 0.87.

![Importance des variables selon Xgboost](https://raw.githubusercontent.com/Nindo16/PredictiveHiringAnalysis/main/img/importance.png)

Les variables 'dispo' et 'cheveux' semblent être très importantes aux yeux du modèle, ce sont elles ayant le plus d'impact sur l'indice de Gini. Nous avions aussi vu que la variable 'cheveux' partageait un lien étroit avec la variable salaire.

---

## Pistes d'améliorations

- Analyse plus poussée des liens potentiels entre variables pour mieux sélectionner les variables pertinentes.
- Optimisation des paramètres du modèle.
- Surveillance du sur-apprentissage.
- Prise en compte du déséquilibre des classes de la variable cible (Ré-échantillonnage synthétique Smote, etc).

---


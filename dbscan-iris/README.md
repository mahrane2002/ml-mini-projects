# Clustering avec DBSCAN sur le jeu de données Iris

Ce projet démontre l'application de l'algorithme de clustering spatial basé sur la densité avec bruit (**DBSCAN** - *Density-Based Spatial Clustering of Applications with Noise*) sur le célèbre jeu de données **Iris**.

## Objectifs du Projet

- **Explorer et préparer** le jeu de données Iris.
- **Standardiser** les caractéristiques pour assurer l'équité des calculs de distance.
- **Déterminer la valeur optimale de l'epsilon (ε)** à l'aide de la méthode du graphe des $k$-plus proches voisins.
- **Entraîner le modèle DBSCAN** et analyser les clusters formés.
- **Visualiser les clusters** en réduisant la dimensionnalité via l'Analyse en Composantes Principales (ACP/PCA).

---

## 🛠️ Outils et Bibliothèques

- **Python 3**
- **NumPy** & **Pandas** : Manipulation des données.
- **Matplotlib** : Visualisation des graphiques.
- **Scikit-Learn** : Préparation des données, modélisation (DBSCAN, PCA) et évaluation (silhouette score).

---

## 📈 Étapes clés de l'implémentation

### 1. Chargement et Standardisation des données
Le jeu de données Iris contient 150 échantillons de fleurs répartis en 3 espèces, décrits par 4 caractéristiques physiques. Comme DBSCAN est un algorithme non supervisé, les étiquettes de classes réelles sont ignorées lors de l'entraînement. 

Étant donné que DBSCAN repose sur la distance euclidienne entre les points, toutes les variables ont été standardisées à l'aide de `StandardScaler` (moyenne = 0, variance = 1).

### 2. Sélection d'Epsilon (ε) via le K-Distance Graph
Pour choisir une valeur d'epsilon raisonnable, nous calculons pour chaque point la distance à son $k$-ième plus proche voisin ($k=5$, déterminé en fonction du nombre de caractéristiques + 1). Le coude de la courbe triée nous indique la valeur optimale d'epsilon.

![Graphe de K-Distance](images/k-distance_graph.png)

*Le point d'inflexion (coude) se situe aux alentours de **0.8**, valeur sélectionnée pour notre entraînement.*

### 3. Entraînement de DBSCAN
Le modèle a été entraîné avec les paramètres suivants :
- `eps = 0.8`
- `min_samples = 5`

#### Analyse des résultats :
- **Nombre de clusters découverts** : 2
- **Points considérés comme du bruit (anomalies)** : 4
- **Score de Silhouette** : 0.522

> **Remarque** : Bien que le dataset Iris comprenne originellement 3 espèces biologiques distinctes, DBSCAN a identifié 2 clusters majeurs. Cela s'explique par le fait que deux des trois espèces (Iris-versicolor et Iris-virginica) sont très proches géométriquement et ne sont pas séparées par une zone de faible densité, fusionnant ainsi en un seul cluster spatial.

### 4. Visualisation des Clusters (Projection ACP)
Afin de visualiser la répartition en 2D, nous avons appliqué l'ACP (PCA) pour réduire le jeu de données de 4 dimensions à 2 composantes principales.

![Clusters DBSCAN avec PCA](images/dbscan_clusters_pca_projection.png)

*Les points de couleur violette (valeur -1/bruit) correspondent aux anomalies détectées par l'algorithme.*

---

## 🏁 Conclusion

L'utilisation de DBSCAN sur le dataset Iris met en évidence plusieurs caractéristiques de cet algorithme :
- **Points forts** : Il ne nécessite pas de spécifier le nombre de clusters au préalable (contrairement à K-Means) et est capable de détecter des points aberrants (bruit).
- **Limites** : Il peine à séparer des classes qui se chevauchent de manière dense ou qui présentent des densités variables.

### Améliorations futures possibles :
1. Comparer les performances avec l'algorithme **K-Means** ou le **Clustering Hiérarchique**.
2. Réaliser une recherche d'hyperparamètres plus fine sur les valeurs de `eps` et `min_samples`.
3. Tester DBSCAN sur des jeux de données de forme non linéaire (comme le dataset des lunes *moons*).

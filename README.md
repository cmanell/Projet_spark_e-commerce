# Projet Spark : Analyse et modélisation Big Data avec PySpark

Notebook PySpark complet qui couvre :
- **chargement / nettoyage** d’un dataset e-commerce
- **segmentation client (RFM + KMeans)**
- **modélisation supervisée** pour prédire les “gros dépensiers”
- **visualisations** et **recommandations** business

**Dataset :** *Online Retail* (UCI Machine Learning Repository)  
**Auteur :** Cédric MANELLI

---

## Objectifs

1. Mettre en place un pipeline **Spark** de bout en bout (DataFrame + MLlib).
2. Nettoyer des données réelles (formats, valeurs aberrantes, dates, etc.).
3. Construire une **segmentation client** exploitable (RFM + clustering).
4. Entraîner des modèles supervisés pour prédire un comportement client (dépense élevée).
5. Produire des **reco actionnables** côté marketing / CRM.

---

## Contenu du notebook

### 1) Initialisation & chargement des données
- Création d’une `SparkSession`
- Récupération du fichier via **gdown** (Google Drive)
- Lecture du CSV dans Spark

### 2) Exploration & pré-traitement
- Analyse de qualité des données (types, nulls, incohérences)
- Corrections de formats (ex : *UnitPrice* avec séparateur décimal européen, dates en string)
- Nettoyage (valeurs négatives, lignes incohérentes, etc.)
- Création de variables utiles (ex : montant total)

### 3) Segmentation client (RFM)
- Construction des indicateurs :
  - **Recency** : temps depuis le dernier achat
  - **Frequency** : nombre d’achats
  - **Monetary** : montant total dépensé
- Mise en forme features (VectorAssembler + StandardScaler)
- **KMeans** + métriques :
  - méthode du coude (WSSSE / trainingCost)
  - **silhouette score**
- Analyse des clusters (profils clients)

### 4) Modélisation supervisée
Objectif : prédire si un client est un **gros dépensier (1)** ou **petit dépensier (0)**  
- Création d’un label via un seuil : `montant_total > 643.63`  → `label=1`, sinon `0`
- Split train/test
- Modèles :
  - **GBTClassifier** (Gradient-Boosted Trees)
  - **LogisticRegression**
- Évaluation :
  - **AUC ROC** (BinaryClassificationEvaluator)
  - Precision / Recall / F1 (MulticlassClassificationEvaluator)

> Note : le notebook calcule aussi RMSE/R² sur la régression logistique (possible mais pas très “classifier-friendly” comme métrique).

### 5) Visualisations & recommandations
- Graphiques (Plotly / Matplotlib)
- Recommandations business :
  - exploitation des segments RFM
  - conversion “petits dépensiers” → “gros dépensiers”
  - intégration dans un pipeline batch (idée de recommandation)

---

## Prérequis

- Python 3.9+ (idéalement 3.10/3.11)
- Java (requis par Spark)
- PySpark

Librairies utilisées (vu dans le notebook) :
- `pyspark`
- `pandas`, `numpy`
- `plotly`, `matplotlib`
- `gdown`

---

## Installation

### Option A (pip)
```bash
pip install pyspark pandas numpy plotly matplotlib gdown

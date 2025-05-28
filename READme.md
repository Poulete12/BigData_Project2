# 📖 Analyse Stylométrique de La Comédie Humaine

Ce projet réalise une **analyse stylométrique complète** de La Comédie Humaine d'Honoré de Balzac, utilisant les technologies modernes de traitement du langage naturel (Spark NLP) pour extraire et analyser les caractéristiques linguistiques de cette œuvre monumentale.

## 🎯 Objectifs du Projet

L'analyse vise à répondre aux questions suivantes :
- **Différences stylistiques** : Les genres littéraires (romans, nouvelles, essais) présentent-ils des signatures stylistiques distinctes ?
- **Stabilité intra-texte** : La lisibilité est-elle stable dans un texte ou varie-t-elle selon les passages ?
- **Impact du dialogue** : Les passages dialogués ont-ils une lisibilité différente de la narration ?
- **Distribution lexicale** : Les textes respectent-ils la loi de Zipf caractéristique des langues naturelles ?

## 🏗️ Architecture du Projet

```
.
├── DATA/                           # Corpus téléchargé (généré automatiquement)
│   └── balzac-master/
│       ├── DOCUMENTS_TEXTE/        # Fichiers .txt des œuvres
│       └── metadata.csv            # Métadonnées bibliographiques
├── _resultats/                     # Résultats d'analyse (généré)
│   ├── annotations_nlp/            # Annotations Spark NLP (Parquet)
│   ├── metriques/                  # Métriques stylométriques (CSV)
│   └── lisibilite/                 # Indices de lisibilité (CSV)
├── _resultats_finaux/              # Stockage optimisé (Parquet compressé)
├── rapport.qmd                     # Document principal Quarto
└── README.md                       # Ce fichier
```

## 🚀 Installation et Configuration

### Prérequis

- **Python 3.10+** avec conda/miniconda
- **R 4.0+** avec les packages `reticulate`, `knitr`, `dplyr`
- **Java 8 ou 11** (requis pour Spark)
- **Quarto** pour le rendu du document

### Installation automatique

Le projet configure automatiquement l'environnement conda nécessaire lors de la première exécution :

```bash
# 1. Cloner le projet
git clone <votre-repo>
cd analyse-stylometrique-balzac

# 2. Lancer l'analyse
quarto render analyse_stylometrique.qmd --to html
```

## 🔧 Utilisation

### Exécution Standard

```bash
# Rendu HTML complet
quarto render analyse_stylometrique.qmd --to html

# Rendu PDF (optionnel)
quarto render analyse_stylometrique.qmd --to pdf
```

### Exécution par Sections

Pour exécuter seulement certaines parties :

```bash
# Mode interactif avec Jupyter
quarto preview analyse_stylometrique.qmd
```

### Configuration des Variables

Les principales variables de configuration se trouvent au début du document :

```r
# === CONFIGURATION GLOBALE ===
CORPUS_URL <- "https://github.com/dh-trier/balzac/archive/refs/heads/master.zip"
BASE_DATA_DIR <- "./DATA"
ANNOTATIONS_PARQUET <- "_resultats/annotations_nlp"
TAILLE_LOT <- 4  # Nombre de fichiers traités simultanément
```

## 📊 Pipeline de Traitement

### 1. Extraction et Chargement (ETL)
- **Téléchargement automatique** du corpus depuis GitHub
- **Restructuration** de l'arborescence des fichiers
- **Nettoyage** des fichiers temporaires et parasites

### 2. Annotation Linguistique
- **Pipeline Spark NLP** : segmentation, tokenisation, POS-tagging, lemmatisation
- **Traitement par lots** pour optimiser la mémoire
- **Détection des dialogues** via des marqueurs linguistiques spécialisés

### 3. Calcul des Métriques
- **Métriques structurelles** : nombre de tokens, phrases, richesse lexicale
- **Indices de lisibilité** : Flesch-Kincaid, Kandel-Moles
- **Analyse de dialogue** : ratio narration/dialogue par texte

### 4. Analyses Avancées
- **Fenêtres coulissantes** : stabilité des indices dans le texte
- **Distribution de Zipf** : conformité aux lois universelles
- **Comparaisons inter-genres** : différences stylistiques

## 📈 Résultats et Visualisations

Le projet génère automatiquement :

### Graphiques Interactifs
- **Distributions par genre** : longueur des phrases, richesse lexicale
- **Fenêtres coulissantes** : évolution de la lisibilité
- **Scatter plots** : relation dialogue/lisibilité
- **Courbes de Zipf** : distribution des fréquences lexicales

### Métriques Quantitatives
- **TTR (Type-Token Ratio)** : diversité lexicale
- **Hapax Legomena** : mots à occurrence unique
- **Indices de lisibilité** : accessibilité des textes
- **Statistiques de dialogue** : proportion de discours direct

## ⚙️ Optimisations Techniques

### Configuration Spark
```python
session_spark = SparkSession.builder \
    .appName("AnalyseBalzacStylometrie") \
    .master("local[*]") \
    .config("spark.driver.memory", "12g") \
    .config("spark.executor.memory", "8g") \
    .config("spark.sql.shuffle.partitions", "6") \
    .getOrCreate()
```

## 📚 Dépendances

### Python
- `spark-nlp==4.4.2` - Traitement du langage naturel
- `pandas` - Manipulation de données
- `plotly` - Visualisations interactives
- `numpy==1.26` - Calculs numériques

### R
- `reticulate` - Interface Python-R
- `knitr` - Rendu de documents
- `dplyr` - Manipulation de données

### Système
- **Java 8/11** - Runtime Spark
- **Quarto** - Système de publication scientifique

### Découvertes Principales

1. **Variabilité inter-genres** : Les romans présentent une complexité syntaxique supérieure aux nouvelles
2. **Stabilité intra-texte** : Balzac maintient une cohérence stylistique relative au sein de chaque œuvre
3. **Impact dialogal** : Les passages dialogués améliorent significativement la lisibilité
4. **Universalité lexicale** : Conformité parfaite à la loi de Zipf

---

**Projet réalisé par Asso et Antony (2025)**

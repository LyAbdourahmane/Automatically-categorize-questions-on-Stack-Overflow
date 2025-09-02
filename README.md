# Automatically categorize questions on Stack Overflow

## 📌 Contexte

Stack Overflow est une plateforme incontournable de questions-réponses autour du développement informatique.
Cependant, lors de la création d’une question, les utilisateurs renseignent parfois des **tags** inappropriés ou incomplets, ce qui nuit à la recherche et à la lisibilité du contenu.

L’objectif de ce projet est de développer un **système de suggestion automatique de tags** à partir du texte des questions.
Deux approches de **machine learning** ont été étudiées :

* une approche **non supervisée**,
* une approche **supervisée**.

Ce projet est réalisé dans le cadre du parcours *Data Scientist* d’OpenClassrooms.

---

## Données utilisées

Les données proviennent de deux sources :

1. **StackExchange Data Explorer (SQL)**

   * Extraction de 50 000 questions avec contraintes pour améliorer la qualité :

     * questions vues, mises en favori ou jugées pertinentes,
     * au moins une réponse,
     * au moins 5 tags.

   Exemple de requête :

   ```sql
   SELECT TOP 50000 Title, Body, Tags, Id, Score, ViewCount, FavoriteCount, AnswerCount
   FROM Posts
   WHERE PostTypeId = 1
     AND ViewCount IS NOT NULL
     AND FavoriteCount IS NOT NULL
     AND AnswerCount > 0
     AND LEN(Tags) - LEN(REPLACE(Tags, '<','')) >= 5
   ```

2. **Stack Exchange API**

   * Test d’extraction de 50 questions contenant le tag *python*,
   * Score > 50,
   * Récupération des informations principales : date, titre, tags, score.

---

## Organisation du projet

Le projet est structuré en 4 notebooks :

1. **`Comprendrelesdonnées&lescollecter.ipynb`**

   * Collecte et exploration des données (SQL & API).
   * Analyse de la qualité des tags et des textes.

2. **`PretraitementDesDonnées.ipynb`**

   * Nettoyage du texte (HTML, ponctuation, stopwords, lemmatisation).
   * Préparation des features (BoW, embeddings).

3. **`ApprocheNonSupervisée.ipynb`**

   * Utilisation de méthodes non supervisées pour générer des mots-clés (clustering, LDA, etc.).
   * Proposition de tags candidats.

4. **`ApprocheSuppervisée.ipynb`**

   * Mise en place de modèles supervisés pour prédire les tags :

     * Bag-of-Words + modèles classiques (Logistic Regression, SVM).
     * Word2Vec, BERT, USE pour les représentations avancées.
   * Évaluation et comparaison avec l’approche non supervisée.

---

## Méthodologie

### Extraction de features textuelles

* **Bag-of-Words (TF-IDF)**
* **Word2Vec / Doc2Vec**
* **BERT**
* **Universal Sentence Encoder (USE)**

### Approches de modélisation

* **Non supervisée** : regroupement des termes et génération automatique de mots-clés.
* **Supervisée** : classification multi-label pour prédire plusieurs tags pertinents.

### Évaluation

* Mise en place d’une métrique adaptée : **taux de couverture des tags**.
* Comparaison des performances entre les deux approches.

---

##  Utilisation

1. Cloner ce dépôt :

   ```bash
   git clone https://github.com/LyAbdourahmane/Automatically-categorize-questions-on-Stack-Overflow.git
   cd Automatically-categorize-questions-on-Stack-Overflow
   ```

2. Ouvrir les notebooks dans **Jupyter** ou **JupyterLab** :

   ```bash
   jupyter notebook
   ```

3. Suivre l’ordre des notebooks pour reproduire le projet :

   1. `Comprendrelesdonnées&lescollecter.ipynb`
   2. `PretraitementDesDonnées.ipynb`
   3. `ApprocheNonSupervisée.ipynb`
   4. `ApprocheSuppervisée.ipynb`

---

## Résultats attendus

* Mise en évidence des limites et avantages de chaque approche.
* Capacité à **suggérer automatiquement plusieurs tags pertinents** pour une nouvelle question.
* Évaluation claire des performances grâce à une méthodologie cohérente.

---

## Remarques

* Les données collectées respectent le **RGPD** : seules les informations nécessaires (titre, texte, tags, métadonnées) ont été utilisées.
* Aucun identifiant personnel des auteurs des questions n’est conservé.

---

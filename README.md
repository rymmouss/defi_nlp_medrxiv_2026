# Défi NLP, Abstracts medRxiv 2026

Ce dépôt contient l'ensemble du code, des prédictions et du support de soutenance réalisés dans le cadre du Défi NLP sur le corpus scientifique **medRxiv**.


## 1. Projet Principal (Projet 3), Recommandation d'abstracts

* **Objectif :** À partir d'une requête clinique ou scientifique en anglais, recommander et classer les **10 articles les plus pertinents** parmi un corpus de **5 058 preprints** (`2 000` prédictions finales sur les `200` requêtes de test).
* **Protocole :** Nettoyage HTML ciblé (préservation des seuils statistiques comme `p<0.05`), conservation des versions multiples d'articles, et découpage reproductible **80 % réglage** (`372` requêtes) / **20 % validation** (`93` requêtes, `random_state=42`).

### Comparaison des résultats (Validation)

| Script | Approche | Modèle & Paramètres | nDCG@10 | MRR |
| :--- | :--- | :--- | :---: | :---: |
| **Script 1** | Classique (Mots-clés) | TF-IDF Cosinus (unigrammes, `sublinear_tf=True`) | `0.4244` | `0.6433` |
| **Script 1** | Classique (Mots-clés) | BM25 (`k1=1.2`, `b=0.9`) | `0.4637` | `0.7013` |
| **Script 1** | Classique (Mots-clés) | BM25 + Rocchio PRF (`k=3`, `beta=1.0`) | `0.4692` | `0.6874` |
| **Script 2** | **Deep Learning** | **Sentence-BERT (`all-MiniLM-L6-v2`, 384 dim.)** | **`0.7262`** | **`0.9283`** |

---

## 2. Projet Secondaire (Projet 1), Classification des 16 disciplines médicales

* **Objectif :** Prédire la grande discipline médicale (`category_grp`, 16 classes) d'un article à partir de son résumé (`abstract`), évaluée par la métrique **F1-Macro**.
* **Protocole :** Entraînement sur l'`abstract` nettoyé (`10 958` articles en train, découpage stratifié `80/20`, gestion des `643` résumés manquants du jeu de test de `1 940` lignes) avec pondération équilibrée des classes (`class_weight="balanced"`).

### Résultats obtenus (Validation)

| Modèle | F1-Macro | Accuracy |
| :--- | :---: | :---: |
| Multinomial Naive Bayes (`alpha=0.2`) | `0.5363` | `55.89 %` |
| Régression Logistique pondérée (`C=2.0`) | `0.6621` | `66.06 %` |
| **LinearSVC pondéré (`C=0.5`)** | **`0.6781`** | **`67.93 %`** |


## 3. Contenu du dépôt

* `Script1 projet3.ipynb` : Pipeline classique du Projet 3 (TF-IDF, BM25, Rocchio).
* `Script2 projet3.ipynb` : Pipeline Deep Learning du Projet 3 (`all-MiniLM-L6-v2`) et contrôle d'intégrité.
* `Predictions_Projet3.csv` : Fichier final de soumission (`2 000` lignes : `query_id`, `ID`, `rank`).
* `Projet 3.pdf` : Support complet de la soutenance orale (18 diapositives).

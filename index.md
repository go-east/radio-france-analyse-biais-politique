---
layout: default
title: Accueil
---

# 📻 Analyse des Biais Politiques sur Radio France

Une étude exploratoire utilisant l'IA pour analyser l'orientation politique des émissions de Radio France à travers la transcription automatique et l'analyse par LLM.

---

## 🎯 Objectif

Ce projet vise à quantifier et visualiser les biais politiques potentiels dans les émissions de Radio France en analysant automatiquement le contenu des podcasts. L'analyse est réalisée par intervenant (speaker) pour distinguer les propos des journalistes, invités et personnes interrogées.

---

## 📊 Pipeline de traitement

```
Podcasts MP3 → Transcription (Gladia) → Analyse politique (GPT-4) → Visualisation
```

| Étape | Outil | Description |
|-------|-------|-------------|
| 1. Collecte | Python/Requests | Récupération des métadonnées et MP3 |
| 2. Transcription | Gladia API | Speech-to-text avec diarization |
| 3. Analyse | GPT-4.1-mini | Classification politique par speaker |
| 4. Visualisation | Matplotlib | Graphiques et heatmaps |

---

## 📈 Résultats

### Distribution politique globale
![Distribution globale](images/viz_01_distribution_globale.png)

### Orientation dominante par épisode
![Orientation dominante](images/dominant_orientation.png)

### Orientation par émission
![Orientation par émission](images/viz_03_orientation_par_emission.png)

### Rôles des intervenants
![Rôles](images/viz_04_roles_intervenants.png)

### Distribution des tons
![Tons](images/viz_05_tons.png)

### Indice de pluralisme
![Pluralisme](images/viz_06_pluralisme.png)

### Spectre politique
![Spectre](images/viz_07_spectre_politique.png)

### Heatmap par émission
![Heatmap](images/viz_08_heatmap_emissions.png)

---

## 🗳️ Catégories politiques

| Catégorie | Marqueurs |
|-----------|-----------|
| **Extrême gauche** | LFI, NPA — anticapitalisme, lutte des classes |
| **Social-démocrate** | PS, EELV — réformisme, justice sociale, écologie |
| **Centre** | Renaissance, MoDem — pragmatisme, européisme |
| **Droite** | LR — conservatisme, libéralisme économique |
| **Extrême droite** | RN, Reconquête — nationalisme, souverainisme |

---

## ⚠️ Limites

| Limite | Impact |
|--------|--------|
| Subjectivité du LLM | GPT-4 peut avoir ses propres biais |
| Qualité de transcription | Scores de confiance variables |
| Catégories simplifiées | Spectre réduit à 5 catégories |
| Échantillonnage | Non exhaustif |

> **Note** : Cette étude est exploratoire. Les résultats doivent être interprétés avec prudence.

---

## 🛠️ Reproduire l'étude

```bash
# Cloner le repo
git clone https://github.com/go-east/radio-france-analyse-biais-politique.git
cd radio-france-analyse-biais-politique

# Installer les dépendances
pip install pandas requests tqdm matplotlib numpy

# Configurer les clés API (Gladia + OpenAI)
# Puis exécuter le notebook
jupyter notebook radiofrance.ipynb
```

---

## 📁 Fichiers

| Fichier | Description |
|---------|-------------|
| `radiofrance.ipynb` | Notebook principal |
| `political_analysis.json` | Résultats bruts |
| `images/` | Visualisations |

---

## 📜 Licence

Projet de recherche indépendant. Non affilié à Radio France.

[Voir le code source sur GitHub](https://github.com/go-east/radio-france-analyse-biais-politique)

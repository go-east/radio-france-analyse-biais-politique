# 📻 Analyse du Biais Politique de France Inter

> Étude computationnelle de l'orientation politique et du biais éditorial des programmes de France Inter

[![Python](https://img.shields.io/badge/Python-3.11-blue.svg)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Data](https://img.shields.io/badge/Data-Sur%20demande-orange.svg)](#-accès-aux-données)

## 📋 Table des matières

- [Aperçu](#-aperçu)
- [Résultats clés](#-résultats-clés)
- [Méthodologie](#-méthodologie)
- [Installation](#-installation)
- [Calibration du modèle](#-calibration-du-modèle)
- [Utilisation](#-utilisation)
- [Structure du projet](#-structure-du-projet)
- [Limites](#-limites)
- [Pistes d'amélioration](#-pistes-damélioration)
- [Accès aux données](#-accès-aux-données)
- [Licence](#-licence)

## 🎯 Aperçu

Ce projet analyse automatiquement l'orientation politique des émissions de France Inter en utilisant :

1. **Transcription automatique** via l'API Gladia (ASR + diarisation des locuteurs)
2. **Analyse politique** via GPT-4.1-mini pour classifier le contenu selon le spectre politique français

### Corpus analysé

| Métrique                  | Valeur |
| ------------------------- | ------ |
| Épisodes analysés         | 145    |
| Émissions différentes     | 11     |
| Locuteurs identifiés      | 764    |
| Moyenne locuteurs/épisode | 5,3    |
| Taux de réussite          | 100%   |

## 📊 Résultats clés

### Orientation politique moyenne

| Orientation          | Pourcentage |
| -------------------- | :---------: |
| Extrême gauche       |    14,5%    |
| **Social-démocrate** |  **36,6%**  |
| Centre               |    27,9%    |
| Droite               |    12,3%    |
| Extrême droite       |    6,6%     |

### Biais éditorial détecté

- 🟢 **Léger** : 95,2% des épisodes (n=138)
- 🟡 **Marqué** : 3,4% des épisodes (n=5)
- ⚪ **Neutre** : 1,4% des épisodes (n=2)

### Orientation dominante par épisode

![Orientation dominante par épisode](images/dominant_orientation.png)

```
Social-démocrate  ████████████████████████████████████  69,7% (101)
Centre            ███████████                           21,4% (31)
Extrême gauche    ████                                   7,6% (11)
Droite            ▏                                      0,7% (1)
Extrême droite    ▏                                      0,7% (1)
```

### Distribution politique globale

![Distribution globale](images/viz_01_distribution_globale.png)

### Orientation par émission

![Orientation par émission](images/viz_03_orientation_par_emission.png)

### Rôles des intervenants

![Rôles des intervenants](images/viz_04_roles_intervenants.png)

### Tons employés

![Tons](images/viz_05_tons.png)

### Pluralisme des opinions

![Pluralisme](images/viz_06_pluralisme.png)

### Spectre politique des intervenants

![Spectre politique](images/viz_07_spectre_politique.png)

### Heatmap par émission

![Heatmap émissions](images/viz_08_heatmap_emissions.png)

### Émissions analysées

| Émission                       | Épisodes | Type                     |
| ------------------------------ | :------: | ------------------------ |
| L'édito politique              |    16    | Commentaire politique    |
| Géopolitique                   |    15    | Affaires internationales |
| Zoom Zoom Zen                  |    15    | Bien-être                |
| Le billet de Bertrand Chameroy |    15    | Satire                   |
| La Terre au carré              |    15    | Environnement/Science    |
| Affaire Sensible               |    15    | Documentaire             |
| Journal de 07h00               |    14    | Information              |
| Charline explose les faits     |    12    | Humour                   |
| On n'arrête pas l'éco          |    11    | Économie                 |
| L'éco d'Inter                  |    10    | Économie                 |
| Totemic                        |    7     | Culture                  |

## 🔬 Méthodologie

### Pipeline de traitement

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   Audio MP3     │────▶│   Gladia API    │────▶│  Transcription  │
│  (France Inter) │     │  (ASR + Diar.)  │     │  + Locuteurs    │
└─────────────────┘     └─────────────────┘     └────────┬────────┘
                                                         │
                                                         ▼
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   Résultats     │◀────│   GPT-4.1-mini  │◀────│    Analyse      │
│   JSON/CSV      │     │   (Temp: 0.3)   │     │   Politique     │
└─────────────────┘     └─────────────────┘     └─────────────────┘
```

### Catégories politiques

Le modèle classe le contenu selon 5 catégories du spectre politique français :

| Catégorie            | Partis associés    | Caractéristiques                                      |
| -------------------- | ------------------ | ----------------------------------------------------- |
| **Extrême gauche**   | LFI, NPA           | Anticapitalisme, lutte des classes, anti-impérialisme |
| **Social-démocrate** | PS, EELV           | Réformisme, justice sociale, écologie, féminisme      |
| **Centre**           | Renaissance, MoDem | Pragmatisme, libéralisme modéré, européisme           |
| **Droite**           | LR                 | Conservatisme, libéralisme économique, autorité       |
| **Extrême droite**   | RN, Reconquête     | Nationalisme, anti-immigration, souverainisme         |

### Analyse par locuteur

Pour chaque locuteur identifié, le système évalue :

- **Rôle probable** : journaliste/animateur, invité expert, micro-trottoir, autre
- **Scores politiques** : répartition en % sur les 5 catégories (total = 100%)
- **Ton** : neutre, critique, engagé, sympathique, hostile
- **Justification** : explication textuelle de l'orientation

## 💻 Installation

### Prérequis

- Python 3.11+
- Clés API : [Gladia](https://gladia.io) et [OpenAI](https://platform.openai.com)

### Installation des dépendances

```bash
pip install pandas requests tqdm matplotlib
```

### Configuration

Créez un fichier `.env` ou modifiez directement les variables dans le notebook :

```python
gladia_key = 'votre_clé_gladia'
openai_key = 'votre_clé_openai'
```

## 🧪 Calibration du modèle

Avant d'utiliser l'algorithme sur un large corpus, il est recommandé de **tester et calibrer** le modèle sur des contenus dont l'orientation politique est connue.

### Script de calibration

Le fichier `calibration.ipynb` permet de tester l'algorithme sur un fichier audio unique :

```python
# Exemple d'utilisation
TEST_MP3_URL = "https://drive.google.com/uc?export=download&id=VOTRE_ID"
```

### Fichiers de test suggérés

Pour valider la calibration, nous recommandons de tester avec des discours de personnalités politiques connues :

| Personnalité       | Orientation attendue |
| ------------------ | -------------------- |
| Jean-Luc Mélenchon | Extrême gauche       |
| Olivier Faure      | Social-démocrate     |
| Emmanuel Macron    | Centre               |
| Éric Ciotti        | Droite               |
| Marine Le Pen      | Extrême droite       |

### Comment calibrer

1. **Téléchargez** un extrait audio d'un discours politique (2-5 min)
2. **Uploadez** sur Google Drive et récupérez le lien de partage
3. **Convertissez** le lien :

   ```
   # Lien de partage :
   https://drive.google.com/file/d/XXXXX/view?usp=sharing

   # Lien de téléchargement direct :
   https://drive.google.com/uc?export=download&id=XXXXX
   ```

4. **Exécutez** `calibration.ipynb` avec cette URL
5. **Comparez** le résultat avec l'orientation attendue

### Interpréter les résultats de calibration

```
✅ Bonne calibration : Le score le plus élevé correspond à l'orientation attendue
⚠️ Calibration acceptable : L'orientation attendue est dans le top 2
❌ Mauvaise calibration : Revoir le prompt ou changer de modèle
```

## 🚀 Utilisation

### 1. Préparer les données

Créez un fichier CSV `franceinter.csv` avec les colonnes :

```csv
mp3link,emission,title,date,description,isProcessed
https://...,L'édito politique,Titre de l'épisode,2024-01-15,Description...,False
```

### 2. Lancer la transcription

```python
# Exécuter les cellules de transcription du notebook
# Les transcriptions sont sauvegardées dans transcription_results.json
```

### 3. Lancer l'analyse politique

```python
# Exécuter les cellules d'analyse politique
# Les résultats sont sauvegardés dans political_analysis.json
```

### 4. Générer les visualisations

```python
# Exécuter les cellules de visualisation
# Génère les graphiques dans le dossier images/
```

## 📁 Structure du projet

```
radiofrance-analysis/
├── 📓 radiofrance.ipynb           # Notebook principal (transcription + analyse)
├── 📓 calibration.ipynb           # Notebook de test/calibration du modèle
├── 📓 visualization.ipynb         # Notebook de visualisation des résultats
│
├── 📄 franceinter.csv             # Données d'entrée (URLs des épisodes)
├── 📄 transcription_results.json  # Transcriptions avec diarisation
├── 📄 political_analysis.json     # Résultats de l'analyse politique
├── 📄 political_summary.csv       # Résumé exportable
│
└── 📁 images/
    ├── 🖼️ dominant_orientation.png
    ├── 🖼️ viz_01_distribution_globale.png
    ├── 🖼️ viz_02_biais_editorial.png
    ├── 🖼️ viz_03_orientation_par_emission.png
    ├── 🖼️ viz_04_roles_intervenants.png
    ├── 🖼️ viz_05_tons.png
    ├── 🖼️ viz_06_pluralisme.png
    ├── 🖼️ viz_07_spectre_politique.png
    └── 🖼️ viz_08_heatmap_emissions.png
```

## ⚠️ Limites

### Limites méthodologiques

| Limite                     | Description                                          | Impact |
| -------------------------- | ---------------------------------------------------- | ------ |
| **Erreurs de calcul LLM**  | La répartition politique n'atteint pas toujours 100% | Moyen  |
| **Subjectivité du prompt** | Le prompt influence les résultats                    | Élevé  |
| **Granularité temporelle** | Analyse par émission, pas à la minute                | Moyen  |
| **Un seul LLM**            | Biais spécifiques à GPT-4.1-mini                     | Élevé  |

### Limites techniques

| Limite                       | Description                                     | Impact |
| ---------------------------- | ----------------------------------------------- | ------ |
| **Biais du modèle**          | Reflète les biais d'entraînement                | Moyen  |
| **Diarisation**              | Erreurs possibles lors d'échanges rapides       | Faible |
| **Catégories simplifiées**   | 5 catégories pour un spectre complexe           | Moyen  |
| **Neutralité = 20% partout** | Peut ne pas refléter un vrai contenu apolitique | Faible |

## 🔧 Pistes d'amélioration

- [ ] Valider que chaque répartition totalise 100%
- [ ] Utiliser 3-4 LLMs (Claude, Gemini, Llama) et agréger les résultats
- [ ] Intégrer une analyse temporelle (pondération par temps de parole)
- [ ] Faire valider le prompt par des experts en sciences politiques
- [ ] Comparer avec des annotations humaines sur un échantillon témoin
- [ ] Ajouter une analyse de sentiment plus fine
- [ ] Étendre à d'autres stations (France Culture, France Info, RTL, Europe 1)

## 📈 Visualisations générées

Le projet génère automatiquement 8 visualisations :

| Fichier                               | Description                                         |
| ------------------------------------- | --------------------------------------------------- |
| `viz_01_distribution_globale.png`     | Distribution politique moyenne (barres + camembert) |
| `viz_02_biais_editorial.png`          | Répartition du biais éditorial                      |
| `viz_03_orientation_par_emission.png` | Orientation par émission (barres empilées)          |
| `viz_04_roles_intervenants.png`       | Analyse des rôles des speakers                      |
| `viz_05_tons.png`                     | Distribution des tons employés                      |
| `viz_06_pluralisme.png`               | Score de pluralisme par émission                    |
| `viz_07_spectre_politique.png`        | Positionnement gauche-droite des intervenants       |
| `viz_08_heatmap_emissions.png`        | Heatmap du profil politique par émission            |

## 📬 Accès aux données

Les données brutes (transcriptions et analyses) sont disponibles sur demande pour les chercheurs et journalistes.

**Contact** : 📧 [bg@benjamin-gabay.com](mailto:bg@benjamin-gabay.com)

Merci de préciser :

- Votre nom et affiliation
- L'objectif de votre demande
- L'utilisation prévue des données

## 🤝 Contribution

Les contributions sont les bienvenues ! N'hésitez pas à :

1. Fork le projet
2. Créer une branche (`git checkout -b feature/amelioration`)
3. Commit vos changements (`git commit -m 'Ajout d'une fonctionnalité'`)
4. Push sur la branche (`git push origin feature/amelioration`)
5. Ouvrir une Pull Request

## 📄 Licence

Ce projet est sous licence MIT. Voir le fichier [LICENSE](LICENSE) pour plus de détails.

## 📚 Citation

Si vous utilisez ce travail dans vos recherches, merci de citer :

```bibtex
@software{radiofrance_analysis,
  title = {Analyse du Biais Politique de France Inter},
  author = {Gabay, Benjamin},
  year = {2024},
  url = {https://github.com/votre-username/radiofrance-analysis}
}
```

---

<p align="center">
  <b>⚠️ Avertissement</b><br>
  <i>Cette étude est un projet de recherche exploratoire. Les résultats doivent être interprétés avec prudence compte tenu des limites méthodologiques identifiées. Ce projet n'a pas vocation à porter un jugement définitif sur la ligne éditoriale de France Inter.</i>
</p>

---

<p align="center">
  Made with ❤️ for media transparency
</p>

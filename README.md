
## 🌦️ Vue d'ensemble
Ce projet implémente une pipeline de données météorologiques utilisant Dagster comme orchestrateur, Open-Meteo API comme source de données, et dbt pour les transformations.

## 🏗️ Architecture Technique
- **Orchestration** : Dagster
- **Source de Données** : Open-Meteo API
- **Transformation** : dbt
- **Visualisation** : Power BI (optionnel)

## 📋 Fonctionnalités Principales

- Collecte automatique de données météorologiques pour 15 villes françaises
- Transformation des données avec dbt
- Schedules et sensors dynamiques
- Extraction de données multi-paramètres :
  * Température
  * Humidité
  * Précipitations
  * Vitesse du vent

# 📂 Structure du Projet

`dagster_pipeline/` ── `pipeline/` 
- `__init__.py` : Définitions principales
- `assets.py` : Définition des assets
- `resources.py` : Ressources API
- `jobs.py` : Définition des jobs 
- `schedules.py` : Planifications
- `sensors.py` : Capteurs conditionnels

`warehouse/` ── `dbt_project/`
- `models/`
  - `staging/` : Modèles de préparation
  - `mart/` : Modèles finaux
- `dbt_project.yml` : Configuration projet
- `profiles.yml` : Configuration connexion

## 🚀 Assets Dagster

1. **`city_list`**
   - Liste de 15 villes françaises
   - Couvre différentes régions

2. **`raw_weather_data`**
   - Récupération des données via Open-Meteo
   - Extraction multi-paramètres

3. **`dbt_models`**
   - Exécution des transformations dbt

## ⏰ Schedules & Sensors

### Schedules
- Mise à jour quotidienne à 8h00
- Rapport hebdomadaire le lundi à 6h00

### Sensors
- Alerte température (> 30°C)
- Alerte précipitations (> 5mm)


## 🛠 Installation

## 🚀 Installation et Configuration du Projet

### 1. 📥 Clonage du Repository
markdown## 🚀 Installation et Configuration du Projet

Voici le contenu exact à copier-coller dans votre fichier README.md :

```markdown
# 🌦️ Dagster Weather Pipeline

## Installation

### 1. Clonage du Repository
```bash
git clone https://github.com/votre-username/dagster-weather-pipeline.git
```

### 2. Naviguer dans le dossier du projet
```bash
cd dagster-weather-pipeline
```

### 3. Créer un environnement virtuel

#### Pour Windows
```bash
python -m venv venv
venv\Scripts\activate
```

#### Pour macOS et Linux
```bash
python3 -m venv venv
source venv/bin/activate
```

### 4. Mettre à jour pip
```bash
pip install --upgrade pip
```

### 5. Installer les dépendances
```bash
pip install -r requirements.txt
```

### 6. Configurer les variables d'environnement

#### Pour Windows
```bash
set DAGSTER_HOME=.\dagster_home
```

#### Pour macOS et Linux
```bash
export DAGSTER_HOME=./dagster_home
```

### 7. Lancer Dagster
```bash
dagster dev
```

### 8. Exécuter les tests (optionnel)
```bash
pytest tests/
```

### 9. Exporter les données (optionnel)
```bash
python export_csv.py
```

## Contributeurs
- Ashraf Mesbahi
- Walid Fadi
```

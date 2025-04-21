
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

### 1. 📥 Clonage du Repository
```bash
git clone https://github.com/votre-username/dagster-weather-pipeline.git
<details>
<summary>Copier la commande</summary>
git clone https://github.com/votre-username/dagster-weather-pipeline.git
</details>
2. 📂 Naviguer dans le dossier du projet
bashcd dagster-weather-pipeline
<details>
<summary>Copier la commande</summary>
cd dagster-weather-pipeline
</details>
3. 🐍 Créer un environnement virtuel
Pour Windows
bashpython -m venv venv
venv\Scripts\activate
<details>
<summary>Copier les commandes</summary>
python -m venv venv
venv\Scripts\activate
</details>
Pour macOS et Linux
bashpython3 -m venv venv
source venv/bin/activate
<details>
<summary>Copier les commandes</summary>
python3 -m venv venv
source venv/bin/activate
</details>
4. 🔧 Mettre à jour pip
bashpip install --upgrade pip
<details>
<summary>Copier la commande</summary>
pip install --upgrade pip
</details>
5. 📦 Installer les dépendances
bashpip install -r requirements.txt
<details>
<summary>Copier la commande</summary>
pip install -r requirements.txt
</details>
6. 🌐 Configurer les variables d'environnement
Pour Windows
bashset DAGSTER_HOME=.\dagster_home
<details>
<summary>Copier la commande</summary>
set DAGSTER_HOME=.\dagster_home
</details>
Pour macOS et Linux
bashexport DAGSTER_HOME=./dagster_home
<details>
<summary>Copier la commande</summary>
export DAGSTER_HOME=./dagster_home
</details>
7. 🏃 Lancer Dagster
bashdagster dev
<details>
<summary>Copier la commande</summary>
dagster dev
</details>
8. 🧪 Exécuter les tests (optionnel)
bashpytest tests/
<details>
<summary>Copier la commande</summary>
pytest tests/
</details>
9. 💾 Exporter les données (optionnel)
bashpython export_csv.py
<details>
<summary>Copier la commande</summary>
python export_csv.py
</details>
👥 Contributeurs
-Ashraf Mesbahi
-Walid Fadi

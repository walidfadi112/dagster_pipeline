markdown# Pipeline de Données Météorologiques avec Dagster

## Vue d'ensemble
Ce projet implémente une pipeline de données de bout en bout utilisant Dagster comme orchestrateur, suivant l'approche ELT (Extract, Load, Transform) pour récupérer, stocker, transformer et visualiser des données météorologiques de villes françaises.

## Architecture
                 ┌──────────────┐
                 │    API       │
                 │  Open-Meteo  │
                 └──────┬───────┘
                        │
                        ▼
┌─────────────┐      ┌──────────────┐     ┌────────────┐
│   Dagster   │◄─────┤  Extraction  │     │  Schedule  │
│ Orchestrator│      │    Assets    │◄────┤  Sensors   │
└──────┬──────┘      └──────┬───────┘     └────────────┘
│                    │
│             ┌──────▼───────┐
│             │   DuckDB     │
│             │  Database    │
│             └──────┬───────┘
│                    │
│             ┌──────▼───────┐
└────────────►│     dbt      │
│ Transformations│
└──────┬───────┘
│
▼
┌──────────────┐
│   Power BI   │
│ Visualisations│
└──────────────┘

## Fonctionnalités principales

- **Extraction des données** : Récupération des données météorologiques pour 15 villes françaises via l'API Open-Meteo
- **Stockage** : Persistance des données dans DuckDB, base de données analytique légère
- **Transformation** : Utilisation de dbt pour structurer et transformer les données
- **Visualisation** : Tableaux de bord Power BI avec analyses avancées
- **Orchestration** : Pipeline complet avec assets, jobs, schedules et sensors dans Dagster
- **Enrichissement des données** : Calcul d'indices météorologiques comme l'indice de confort thermique

## Structure du projet
dagster_pipeline/
├── dagster_home/           # Configuration Dagster
│   └── dagster.yaml
├── pipeline/               # Code source Dagster
│   ├── init.py         # Définitions des assets et jobs
│   ├── assets.py           # Définition des assets individuels
│   ├── resources.py        # Configuration des ressources (API, DB)
│   ├── jobs.py             # Définition des jobs
│   ├── schedules.py        # Planifications d'exécution
│   └── sensors.py          # Capteurs pour déclenchements conditionnels
├── warehouse/              # Stockage et transformation
│   ├── data/               # Données DuckDB et exports CSV
│   └── dbt_project/        # Projet dbt
│       ├── models/         # Modèles SQL dbt
│       │   ├── staging/    # Modèles de préparation
│       │   └── mart/       # Modèles finaux
│       ├── dbt_project.yml # Configuration dbt
│       └── profiles.yml    # Connexion à la base de données
└── visualisations/         # Fichiers Power BI

## Pipeline Dagster

### Assets

1. **city_list** : Liste de 15 villes françaises représentant différentes régions
2. **raw_weather_data** : Données météorologiques brutes extraites de l'API
3. **processed_weather_tables** : Tables structurées pour analyse (current_weather_table, forecast_weather_table)
4. **weather_dbt_models** : Exécution des modèles dbt pour transformation avancée

### Jobs

1. **weather_pipeline_job** : Pipeline complet d'extraction, stockage et transformation
2. **extract_data_job** : Exécution uniquement de l'extraction des données
3. **transform_data_job** : Exécution uniquement des transformations
4. **dbt_job** : Exécution des modèles dbt

### Automatisation

- **Schedules** : Exécutions planifiées quotidiennes et hebdomadaires
- **Sensors** : Détection de températures élevées et précipitations importantes

## Modèles dbt

1. **stg_weather_data** : Standardisation des données météo brutes
2. **weather_stats** : Agrégation des statistiques par ville et région

## Visualisations Power BI

Plusieurs visualisations avancées ont été développées :
- Carte de chaleur des températures par région
- Graphique en bulles des indices de confort
- TreeMap hiérarchique des villes par région
- Jauges de performance météorologique avec indicateurs multiples

## Technologies utilisées

- **Dagster** : Orchestration du pipeline de données
- **Open-Meteo API** : Source de données météorologiques
- **DuckDB** : Base de données analytique
- **dbt** : Transformation des données
- **Power BI** : Visualisation et tableaux de bord
- **Python** : Langage principal d'implémentation

## Installation et déploiement

### Prérequis
- Python 3.9+
- Pip
- Power BI Desktop (pour visualisations)

### Installation

1. **Cloner le repository**
   ```bash
   git clone https://github.com/votre-username/dagster-weather-pipeline.git
   cd dagster-weather-pipeline

Créer un environnement virtuel
bashpython -m venv venv
source venv/bin/activate  # Sur Windows: venv\Scripts\activate

Installer les dépendances
bashpip install -r requirements.txt

Configurer Dagster
bashset DAGSTER_HOME=./dagster_home  # Sur Windows
# export DAGSTER_HOME=./dagster_home  # Sur Unix


Exécution

Lancer le serveur Dagster
bashdagster dev

Accéder à l'interface Dagster
Ouvrir dans le navigateur: http://localhost:3000
Exécuter la pipeline

Naviguer vers l'onglet "Jobs"
Sélectionner "weather_pipeline_job"
Cliquer sur "Launch Run"


Activer les schedules et sensors

Aller dans "Deployment" > "Schedules" ou "Sensors"
Activer les schedules ou sensors souhaités



Fonctionnement de la pipeline

Extraction : Les données météorologiques sont extraites de l'API Open-Meteo pour 15 villes françaises
Chargement : Les données sont stockées dans DuckDB
Transformation : Les modèles dbt transforment les données brutes en vues analytiques
Visualisation : Power BI permet d'explorer et visualiser les données transformées

Challenges et solutions

Challenge : Accès aux coordonnées géographiques pour les villes
Solution : Intégration d'un dictionnaire de coordonnées directement dans le code
Challenge : Intégration de Dagster avec dbt
Solution : Création d'un asset spécifique pour exécuter les modèles dbt
Challenge : Visualisation des données dans Power BI
Solution : Export CSV des données transformées et création de colonnes calculées additionnelles dans Power BI

Perspectives d'amélioration

Migration vers une base de données plus robuste comme PostgreSQL
Ajout de tests unitaires pour garantir la qualité des données
Enrichissement avec d'autres sources de données (qualité de l'air, événements...)
Intégration d'un modèle de machine learning pour prédictions
Conteneurisation avec Docker pour faciliter le déploiement

Auteurs

[Votre nom]

Licence
Ce projet est sous licence MIT.

Ce README couvre de manière exhaustive tous les aspects de votre projet et servira de documentation complète pour votre soutenance. Il détaille l'architecture, les fonctionnalités, la structure, l'installation et les perspectives d'amélioration, ce qui montre une compréhension approfondie du sujet et une attention au détail.RéessayerClaude peut faire des erreurs. Assurez-vous de vérifier ses réponses. 3.7 Sonnet

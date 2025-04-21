# Pipeline de Données Météorologiques avec Dagster

## Vue d'ensemble
Ce projet implémente une pipeline de données de bout en bout utilisant Dagster comme orchestrateur, suivant l'approche ELT (Extract, Load, Transform) pour récupérer, stocker, transformer et visualiser des données météorologiques de villes françaises représentant différentes régions.

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

- **Extraction des données**: Récupération des données météorologiques pour 15 villes françaises via l'API Open-Meteo
- **Stockage**: Persistance des données dans DuckDB, base de données analytique légère et performante
- **Transformation**: Utilisation de dbt pour structurer et transformer les données
- **Visualisation**: Tableaux de bord Power BI avec analyses avancées et indices personnalisés
- **Orchestration**: Pipeline complet avec assets, jobs, schedules et sensors dans Dagster
- **Enrichissement des données**: Calcul d'indices météorologiques comme l'indice de confort thermique

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

## Pipeline Dagster en détail

### Assets
Notre pipeline est construite autour du concept d'assets Dagster, qui représentent les données produites à chaque étape:

1. **city_list**: Liste de 15 villes françaises représentant différentes régions
   - Couvre les principales régions de France
   - Inclut les métadonnées de région pour l'analyse

2. **raw_weather_data**: Données météorologiques brutes extraites de l'API
   - Température, humidité, précipitations, vitesse du vent
   - Format JSON structuré avec horodatage

3. **processed_weather_tables**: Tables structurées pour analyse
   - `current_weather_table`: Conditions météorologiques actuelles
   - `forecast_weather_table`: Prévisions météorologiques

4. **weather_dbt_models**: Exécution des modèles dbt pour transformation avancée
   - Agrégation par région
   - Calcul de statistiques et KPIs

### Jobs
Nos jobs définissent les workflows complets et partiels:

1. **weather_pipeline_job**: Pipeline complet d'extraction, stockage et transformation
2. **extract_data_job**: Exécution uniquement de l'extraction des données
3. **transform_data_job**: Exécution uniquement des transformations
4. **dbt_job**: Exécution des modèles dbt

### Automatisation
L'automatisation est un point fort de notre projet:

- **Schedules**: 
  - Exécution quotidienne à 8h00 pour les données actuelles
  - Exécution hebdomadaire le lundi à 6h00 pour le rapport complet

- **Sensors**: 
  - Détection de températures supérieures à 30°C pour alertes canicule
  - Surveillance des précipitations importantes (>5mm) pour alertes inondation

## Modèles dbt

Notre projet utilise dbt pour transformer les données brutes en modèles analytiques:

1. **stg_weather_data**: 
   - Standardisation des données météo brutes
   - Nettoyage et validation des types

2. **weather_stats**:
   - Agrégation des statistiques par ville
   - Calcul de moyennes, minimums et maximums par région
   - Catégorisation des températures

## Visualisations Power BI

Nous avons développé plusieurs visualisations avancées:

1. **Carte de chaleur des températures par région**:
   - Vue matricielle des températures avec code couleur
   - Filtrage par catégorie de température

2. **TreeMap hiérarchique**:
   - Représentation des villes regroupées par région
   - Taille des rectangles proportionnelle aux températures

3. **Indice de confort thermique**:
   - Visualisation composite prenant en compte température et humidité
   - Jauge de confort pour comparaison rapide

4. **Tableau de bord régional**:
   - KPIs et statistiques par région
   - Évolution des températures avec alertes

## Installation

```bash
# Sur Unix
export DAGSTER_HOME=./dagster_home

# Sur Windows
set DAGSTER_HOME=./dagster_home
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



Fonctionnement détaillé de la pipeline

Extraction:

Les données météorologiques sont extraites de l'API Open-Meteo
15 villes françaises représentant diverses régions climatiques
Paramètres récupérés: température, humidité, précipitations, vent


Chargement:

Les données sont stockées dans DuckDB
Structure optimisée pour les requêtes analytiques
Conservation de l'historique pour analyse temporelle


Transformation:

Les modèles dbt transforment les données brutes
Enrichissement avec métadonnées régionales
Calcul d'indices météorologiques personnalisés


Visualisation:

Power BI permet d'explorer et visualiser les données
Création de tableaux de bord interactifs
Filtres dynamiques par région et catégorie



Challenges techniques et solutions

Challenge: Accès aux coordonnées géographiques pour les villes
Solution: Intégration d'un dictionnaire de coordonnées directement dans le code pour éviter des appels API supplémentaires
Challenge: Intégration de Dagster avec dbt
Solution: Création d'un asset spécifique qui encapsule l'exécution des modèles dbt via subprocess
Challenge: Gestion des dépendances entre assets
Solution: Utilisation du système de dépendances de Dagster et de l'approche multi_asset pour une meilleure traçabilité
Challenge: Visualisation des données dans Power BI
Solution: Export CSV des données transformées et création de colonnes calculées avancées comme l'indice de confort

Perspectives d'amélioration

Évolutivité: Migration vers une base de données plus robuste comme PostgreSQL pour gérer des volumes importants
Qualité: Ajout de tests unitaires avec pytest pour garantir la fiabilité des transformations
Richesse des données: Enrichissement avec d'autres sources comme la qualité de l'air ou les événements locaux
Intelligence: Intégration d'un modèle de machine learning pour prédictions météorologiques personnalisées
Déploiement: Conteneurisation avec Docker pour faciliter le déploiement dans différents environnements
Monitoring: Mise en place d'alertes et de dashboards de monitoring pour la supervision de la pipeline

Auteurs

Walid Fadi
Ashraf Mesbahi

Licence
Ce projet est sous licence MIT.

Cette version enrichie comprend:
- Une architecture visuelle plus détaillée
- Une explication approfondie de chaque composant
- Des détails sur les visualisations Power BI
- Une description plus technique des challenges rencontrés
- Une section plus développée sur le fonctionnement de la pipeline
- Des perspectives d'amélioration plus élaborées
- Une meilleure mise en page générale

Cette documentation est maintenant beaucoup plus complète et professionnelle pour votre soutenance.

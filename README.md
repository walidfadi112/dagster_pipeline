```markdown
# Pipeline de Données Météorologiques avec Dagster

## Vue d'ensemble
Ce projet implémente une pipeline de données de bout en bout utilisant Dagster comme orchestrateur, suivant l'approche ELT (Extract, Load, Transform) pour récupérer, stocker, transformer et visualiser des données météorologiques de villes françaises représentant différentes régions.

---

## Architecture
```
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
                      │ Transformations │
                      └──────┬───────┘
                             │
                             ▼
                      ┌──────────────┐
                      │   Power BI   │
                      │ Visualisations│
                      └──────────────┘
```

---

## Fonctionnalités principales

- **Extraction des données** : récupération des données météo (température, humidité, vent…) pour 15 villes françaises via l’API Open‑Meteo.  
- **Stockage** : persistance des données dans DuckDB, base analytique légère et rapide.  
- **Transformation** : utilisation de dbt pour structurer, nettoyer et enrichir les données.  
- **Visualisation** : tableaux de bord Power BI interactifs avec analyses avancées et indicateurs personnalisés.  
- **Orchestration** : assets, jobs, schedules et sensors définis dans Dagster pour automatiser le flux ELT.  
- **Enrichissement** : calcul d’indices météo (confort thermique, alertes canicule/inondation).

---

## Structure du projet

```
dagster_pipeline/
├── dagster_home/           # Configuration Dagster
│   └── dagster.yaml
├── pipeline/               # Code Dagster
│   ├── __init__.py         # Définitions des assets & jobs
│   ├── assets.py           # Définition des assets
│   ├── resources.py        # Ressources API & DB
│   ├── jobs.py             # Définition des jobs
│   ├── schedules.py        # Planifications d’exécution
│   └── sensors.py          # Capteurs conditionnels
├── warehouse/              # Stockage & transformation
│   ├── data/               # DuckDB & exports CSV
│   └── dbt_project/        # Projet dbt
│       ├── models/
│       │   ├── staging/    # Modèles de préparation
│       │   └── mart/       # Modèles finaux
│       ├── dbt_project.yml # Config dbt
│       └── profiles.yml    # Connexion DB
└── visualisations/         # Fichiers Power BI (.pbix)
```

---

## Pipeline Dagster en détail

### Assets

1. **city_list**  
   - Liste de 15 villes françaises couvrant différentes régions.

2. **raw_weather_data**  
   - Données brutes JSON : température, humidité, précipitations, vent, horodatage.

3. **processed_weather_tables**  
   - `current_weather_table` : données actuelles.  
   - `forecast_weather_table` : prévisions.

4. **weather_dbt_models**  
   - Exécution des modèles dbt pour agrégation par région et calcul de KPIs.

### Jobs

- **weather_pipeline_job** : pipeline complète (extract → load → transform).  
- **extract_data_job** : extraction seule.  
- **transform_data_job** : transformations seules.  
- **dbt_job** : exécution des modèles dbt.

### Automatisation

- **Schedules**  
  - Quotidienne à 08h00 (données actuelles).  
  - Hebdomadaire lundi 06h00 (rapport complet).

- **Sensors**  
  - Températures > 30 °C → alerte canicule.  
  - Précipitations > 5 mm → alerte inondation.

---

## Modèles dbt

- **stg_weather_data**  
  - Standardisation et nettoyage des données brutes.

- **weather_stats**  
  - Agrégation par ville et région, calcul de moyennes, min/max, catégorisation température.

---

## Visualisations Power BI

1. **Heatmap des températures par région**  
2. **TreeMap hiérarchique (villes/régions)**  
3. **Jauge confort thermique**  
4. **Dashboard régional** (KPIs & évolution)

---

## Installation

```bash
# Cloner le repo
git clone https://github.com/votre-username/dagster_pipeline.git
cd dagster_pipeline

# Créer & activer venv
python -m venv venv
# Windows
venv\Scripts\activate
# Linux/Mac
source venv/bin/activate

# Installer dépendances
pip install -r requirements.txt

# Configurer Dagster
# Windows
set DAGSTER_HOME=.\dagster_home
# Linux/Mac
export DAGSTER_HOME=./dagster_home
```

---

## Exécution

```bash
# Lancer Dagster
python -m dagster dev
```

- Ouvrir `http://localhost:3000`.  
- Dans **Jobs**, lancer `weather_pipeline_job`.  
- Dans **Deployment → Schedules/Sensors**, activer selon besoins.

Pour dbt (optionnel) :

```bash
cd warehouse/dbt_project
dbt run --profiles-dir .
```

Pour exporter CSV (Power BI) :

```bash
python export_csv.py
```

---

## Challenges & solutions

- **Coordonnées villes** : dictionnaire statique pour éviter appel API tiers.  
- **Intégration Dagster ↔ dbt** : asset dédié lançant `dbt run` via subprocess.  
- **Dépendances assets** : `@multi_asset` et `required_resource_keys`.  
- **Visualisation Power BI** : export CSV + colonnes calculées pour indices.

---

## Perspectives d’évolution

- Migrer vers PostgreSQL pour gros volumes.  
- Couvrir d’autres API (qualité de l’air, UV).  
- Ajouter tests unitaires (pytest).  
- Machine learning pour prévisions.  
- Conteneurisation Docker/Kubernetes.  
- Monitoring & alerting en temps réel.

---

## Auteurs

- Walid Fadi  
- Ashraf Mesbahi  

## Licence

MIT © 2025  
```

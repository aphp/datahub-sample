# datahub-sample

Exemple de metadata-as-code [DataHub](https://datahubproject.io/) décrivant un **Entrepôt de Données de Santé (EDS) virtuel** (données fictives inspirées de l'AP-HP, pas le véritable système de production) : data products, datasets, domaines, glossaire métier, tags et propriétés structurées de conformité **HealthDCAT-AP / EHDS**.

Toutes les métadonnées sont déclarées en YAML, au format lu par le connecteur d'ingestion [`aphp/datahub-yaml-source`](https://github.com/aphp/datahub-yaml-source) — un bloc par entité DataHub (`kind: DATASET`, `kind: DATA_PRODUCT`, `kind: GLOSSARY_TERM`, ...), séparés par `---`.

## Structure du dépôt

Chaque répertoire correspond à une couche du pipeline de données et contient un unique fichier `assets.yml` :

| Répertoire | Contenu |
| --- | --- |
| `setup/` | Référentiel partagé : plateformes de données, tags, glossaire, propriétés structurées HealthDCAT-AP, domaines |
| `raw-layer/` | Copies brutes des données sources (PostgreSQL `ehr`) : patient, actes, diagnostics, biologie, PMSI, prescriptions... |
| `transform-layer/` | Copies techniques des données brutes dans l'entrepôt DuckDB |
| `semantic-layer/` | Modèle sémantique aligné FHIR R4 (ressources `Patient`, `Encounter`, `Condition`, ...) |
| `sharing-layer/` | Pipelines Airflow (`DATA_FLOW`/`DATA_JOB`) exportant les données vers S3 au format NDJSON pour le partage externe |
| `quality-layer/` | Assertions de qualité de données (fraîcheur, complétude...) |
| `observability-layer/` | Profils de datasets (statistiques par colonne, volumétrie) |
| `dataproduct-layer/` | Data Products exposés aux utilisateurs finaux (Cohort360, Pilote, Datalab), avec leur documentation et leurs métadonnées HealthDCAT-AP |

## Glossaire et modèle de domaine

Le vocabulaire métier (concepts patient, séjour, PMSI, codifications...) et les décisions de modélisation sont documentés dans [`CONTEXT.md`](./CONTEXT.md). À consulter avant d'ajouter ou de renommer un terme de glossaire, pour éviter de recréer un doublon.

## Conformité HealthDCAT-AP

Les propriétés structurées `fr.aphp.healthdcat.*` déclarées dans `setup/assets.yml` couvrent les propriétés du profil SHACL `ehds` (Health Data Hub) : base légale RGPD, catégorie de données de santé, éditeur, droits d'accès, couverture temporelle/géographique, etc. Elles s'appliquent aux `DATA_PRODUCT` et, pour certaines (URL d'accès, format de distribution), aux `DATASET`.

## Contribution

Les contributions sont les bienvenues via issues et pull requests sur [github.com/aphp/datahub-sample](https://github.com/aphp/datahub-sample). Avant de proposer un changement :

- vérifiez dans [`CONTEXT.md`](./CONTEXT.md) qu'un terme de glossaire équivalent n'existe pas déjà, pour éviter les doublons ;
- gardez les définitions de glossaire en français, cohérentes avec le reste du dépôt ;
- décrivez dans la pull request la couche concernée et l'impact sur les entités DataHub existantes (renommage de terme, changement de domaine, etc.).

## Licence

Ce projet est distribué sous licence [Apache 2.0](./LICENSE).

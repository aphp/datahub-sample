# AP-HP Data Warehouse (EDS) — Metadata Catalog

Ce dépôt est un exemple de metadata-as-code DataHub décrivant un Entrepôt de Données de Santé (EDS) **virtuel** (données fictives, pas le véritable système AP-HP) : data products, datasets, domaines, glossaire, tags et métadonnées de conformité HealthDCAT-AP/EHDS publiées sur DataHub à travers les répertoires `raw-layer`, `transform-layer`, `semantic-layer`, `quality-layer`, `sharing-layer`, `dataproduct-layer` et `observability-layer`.

## Langage

Les définitions de ce glossaire sont en français, à l'image des libellés déjà présents dans les fichiers YAML source (`Séjour`, `Diagnostic`, `Acte`, ...).

### Concepts de gouvernance DataHub

**Domain** :
Regroupement DataHub de Data Products et Datasets par périmètre métier (ex. « Parcours patient », « Biologie », « Facturation / PMSI »). À ne pas confondre avec le sens DDD du mot « domaine » — ici c'est une frontière de navigation/ownership dans le catalogue, pas un bounded context.

**Data Product** :
Entité DataHub représentant une offre documentée et orientée métier (ex. « Diagnostics », « Biologie ») qui regroupe un ou plusieurs Datasets et porte les propriétés structurées HealthDCAT-AP, les termes de glossaire, les tags et les owners.

**Dataset** :
Entité DataHub représentant une table physique/logique ou un export (ex. `ehr_public_diagnostics`). Plusieurs Datasets peuvent composer un même Data Product.

### Concepts métier AP-HP (parcours patient)

**Dossier (administratif)** :
Enveloppe administrative identifiée par un NDA (Numéro de Dossier Administratif), sous laquelle sont enregistrés un ou plusieurs Séjours/Mouvements. Il existe 4 types de dossiers : venue aux urgences non suivie d'hospitalisation, hospitalisation de jour, hospitalisation complète, consultation.
_Statut_ : concept identifié dans `raw-layer/assets.yml` (description du Data Product « Venues et séjours ») mais pas encore matérialisé comme terme de glossaire — à créer si un dataset a besoin de le référencer explicitement.

**Séjour** :
Séjour hospitalier du patient tel que valorisé par le PMSI (admission à sortie). Terme canonique : `pmsi.sejour`.
_Avoid_ : `donnees-patient.sejour` (supprimé — doublon appliqué par erreur à la copie DuckDB de la même donnée ; voir Décisions).

**Diagnostic** :
Diagnostic codé dans le cadre du PMSI (CIM-10), rattaché à un séjour. Terme canonique : `pmsi.diagnostic`.
_Avoid_ : `donnees-patient.diagnostic` (supprimé — doublon, même raison que Séjour).

**Acte** :
Acte médical codé dans le cadre du PMSI (CCAM), rattaché à un séjour. Terme canonique : `pmsi.acte`.
_Avoid_ : `donnees-patient.acte` (supprimé — doublon, même raison que Séjour).

## Décisions

- **2026-08-17** — Les termes de glossaire `donnees-patient.sejour`, `donnees-patient.diagnostic` et `donnees-patient.acte` ont été supprimés de `setup/assets.yml`. Ils étaient appliqués dans `transform-layer/assets.yml` aux copies DuckDB (`warehouse_raw-layer_*`) des mêmes données que `raw-layer/assets.yml` tague avec `pmsi.sejour`/`pmsi.diagnostic`/`pmsi.acte` — ce n'était pas deux concepts distincts mais un même contenu tagué de façon incohérente selon la copie physique. Les datasets du `transform-layer` ont été retagués avec les termes `pmsi.*` canoniques.

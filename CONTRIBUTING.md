# Contribuer à datahub-sample

Merci de vouloir contribuer à cet EDS virtuel de démonstration DataHub. Ce dépôt est un exemple de metadata-as-code : toute contribution modifie directement les entités qui seront ingérées dans DataHub, pas juste de la documentation.

## Avant de contribuer

1. **Cherchez avant de créer.** Un nouveau terme de glossaire, tag ou domaine ressemble peut-être déjà à quelque chose d'existant sous un autre nom. Consultez [`CONTEXT.md`](./CONTEXT.md) (vocabulaire métier) et grep `setup/assets.yml` (référentiel partagé : `GLOSSARY_TERM`, `TAG`, `DOMAIN`, `STRUCTURED_PROPERTY`) avant d'ajouter une nouvelle entité. Le dépôt a déjà eu un doublon de glossaire (`donnees-patient.*` vs `pmsi.*` — voir le journal des décisions dans `CONTEXT.md`) causé par ce réflexe manquant.
2. **Respectez la couche.** Chaque répertoire (`raw-layer`, `transform-layer`, `semantic-layer`, `quality-layer`, `sharing-layer`, `observability-layer`, `dataproduct-layer`, `setup`) a une responsabilité précise (voir le tableau du [`README.md`](./README.md)). N'ajoutez pas un `DATA_PRODUCT` dans `raw-layer/` ou un `DATASET` brut dans `dataproduct-layer/`.
3. **Le français est la langue du glossaire et des descriptions métier.** Les identifiants techniques (`fieldPath`, `qualifiedName`...) restent en anglais/snake_case comme le reste du dépôt.

## Format des fichiers `assets.yml`

Chaque fichier est une suite de documents YAML séparés par `---`, chacun démarrant par `kind:` (`DATASET`, `DATA_PRODUCT`, `GLOSSARY_TERM`, `GLOSSARY_NODE`, `TAG`, `DOMAIN`, `STRUCTURED_PROPERTY`, `CONTAINER`, `DATA_PLATFORM`, `DATA_FLOW`, `DATA_JOB`, `ASSERTION`). C'est le format lu par le connecteur d'ingestion [`aphp/datahub-yaml-source`](https://github.com/aphp/datahub-yaml-source) — vérifiez dans ce connecteur qu'un champ est bien supporté avant de l'utiliser (ex. `typeUrn` sur les owners n'est pas interprété par ce connecteur, contrairement au SDK Python OSS DataHub).

Quand vous ajoutez ou modifiez un terme de glossaire, une propriété structurée ou un tag dans `setup/assets.yml`, mettez à jour [`CONTEXT.md`](./CONTEXT.md) dans le même commit si le terme fait partie du vocabulaire métier durable (pas une propriété purement technique).

## Style de commit

Ce dépôt suit le format [Conventional Commits](https://www.conventionalcommits.org/) :

```
<type>[scope optionnel]: <description>

[corps optionnel]
```

Types utilisés : `feat`, `fix`, `test`, `docs`, `chore`. Sujet à l'impératif,
≤ 72 caractères, sans point final.

## Proposer un changement

1. Ouvrez une issue si le changement est significatif (nouveau Data Product, renommage de terme de glossaire déjà utilisé ailleurs, changement de domaine).
2. Faites une pull request sur [github.com/aphp/datahub-sample](https://github.com/aphp/datahub-sample) qui décrit :
   - la couche concernée,
   - l'impact sur les entités DataHub existantes (un renommage de terme ou de domaine casse les références dans les autres `assets.yml`),
   - si possible, le résultat d'une ré-ingestion locale sans avertissement `reference to an undeclared entity`.
3. Les commits suivent le format `type: Résumé court` (`feat`, `fix`, `docs`...) avec un corps expliquant le *pourquoi* du changement, à l'image de l'historique du dépôt.

En contribuant, vous acceptez que vos contributions soient distribuées sous la licence du projet (voir [`LICENSE`](./LICENSE)).

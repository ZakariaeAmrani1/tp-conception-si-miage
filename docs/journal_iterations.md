# Journal des itérations

## Indicateurs

| Itération | Dates         | Capacité prévue (j idéaux) | Stories terminées | Jours idéaux terminés | pj de développement réels | Vélocité | Tests | Tests en échec |
| --------- | ------------- | -------------------------- | ----------------- | --------------------- | ------------------------- | -------- | ----- | -------------- |
| 1         | 23/11 – 04/12 | 6,3                        |                   |                       |                           |          |       |                |
| 2         | 07/12 – 18/12 | 6,3                        |                   |                       |                           |          |       |                |
| 3         | 04/01 – 15/01 | 6,3                        |                   |                       |                           |          |       |                |
| 4         | 18/01 – 22/01 | 2,1                        |                   |                       |                           |          |       |                |

La vélocité de départ est une hypothèse (0,7). Les valeurs sont remplies par le tracker à la fin de chaque itération.

## Itération 1 (23/11 – 04/12)

Stories prévues : US-01, US-02, US-03, US-04, plus TI-01 à TI-03. Capacité : 6,3 j idéaux.
La colonne « Temps réel » (en jours) est remplie par chacun à la fin de chaque session. Zakariae (tracker) la somme pour calculer la vélocité.

| ID  | Story | Tâche                                                                                                | Responsable | Estimation (j) | Temps réel (j) | Statut  |
| --- | ----- | ---------------------------------------------------------------------------------------------------- | ----------- | -------------- | -------------- | ------- |
| T01 | TI-02 | Outillage : lancement unique des tests, flake8, structure de `tests/`                                | Zakariae    | 0,4            |                | À faire |
| T02 | TI-01 | `modele.py` : classes Offre, Diplome, Annee, avec leurs tests                                        | Ayoub       | 0,6            |                | À faire |
| T03 | US-01 | `main.py` : REPL, lecture d'un fichier, `EXIT`, fin de saisie, fichier introuvable                   | Zakariae    | 0,7            |                | À faire |
| T04 | US-01 | `interpreteur.py` : découpe d'une ligne, commentaires, lignes vides, choix de la commande            | Zakariae    | 0,5            |                | À faire |
| T05 | US-01 | Tests d'acceptation US-01 (`.txt` et sortie attendue)                                                | Sara        | 0,3            |                | À faire |
| T06 | US-02 | `validation.py` : mots réservés, nom d'un seul mot, casse, nom unique dans toute l'OF                | Ayoub       | 0,6            |                | À faire |
| T07 | US-02 | `validation.py` : entiers, nombre d'arguments, messages `ERREUR` avec rappel de syntaxe              | Ayoub       | 0,5            |                | À faire |
| T08 | US-02 | Commande inconnue ou pas encore disponible, format `OK` / `ERREUR` (NF-09), aucune trace d'exception | Ayoub       | 0,4            |                | À faire |
| T09 | US-03 | `CREATE DEGREE` : types, années et ECTS déduits                                                      | Sara        | 0,5            |                | À faire |
| T10 | US-03 | `CREATE DEGREE` : effectif, nom déjà pris, le diplôme créé devient sélectionné                       | Sara        | 0,3            |                | À faire |
| T11 | US-03 | Tests d'acceptation US-03                                                                            | Sara        | 0,2            |                | À faire |
| T12 | US-04 | `SELECT DEGREE`                                                                                      | Sara        | 0,3            |                | À faire |
| T13 | US-04 | `SELECT YEAR` selon le type du diplôme, erreur sans diplôme sélectionné                              | Zakariae    | 0,5            |                | À faire |
| T14 | US-04 | Tests d'acceptation US-04                                                                            | Sara        | 0,2            |                | À faire |
| T15 | TI-03 | `README.md` : lancement, tests, version de Python                                                    | Sara        | 0,3            |                | À faire |
|     |       | **Total**                                                                                            |             | **6,3**        |                |         |

Total par personne : Zakariae 2,1 j, Ayoub 2,1 j, Sara 2,1 j.

Dépendances : T01 et T02 d'abord. T03 et T04 ensemble. T08 après T04. T09, T10, T12 et T13 après T02 et T06. Les tests d'acceptation (T05, T11, T14) après le code de leur story.

Conception (tous ensemble, avant le code) : diagramme de classes, paquetages et contraintes OCL pour RG-05, RG-06, RG-07, RG-08, RG-16 et RG-25, dans `docs/conception/`.

Problèmes rencontrés :
Ajustements pour l'itération suivante :

## Itération 2 (07/12 – 18/12)

- Stories prévues : US-05, US-06, US-07, US-08
- Stories terminées :
- Problèmes rencontrés :
- Ajustements pour l'itération suivante :

## Itération 3 (04/01 – 15/01)

- Stories prévues : US-10, US-11, US-13, US-12
- Stories terminées :
- Problèmes rencontrés :
- Ajustements pour l'itération suivante :

## Itération 4 (18/01 – 22/01)

- Stories prévues : US-16
- Stories terminées :
- Problèmes rencontrés :

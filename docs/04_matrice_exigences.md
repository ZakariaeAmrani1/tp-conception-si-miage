# Matrice des exigences

Elle relie chaque exigence à sa story, à l'itération prévue, au module du code et au fichier de tests.
Les colonnes « Module » et « Tests » sont **prévues** : le tracker les confirme à la fin de chaque itération et met le statut à jour.

Statuts : À faire, En cours, Terminée (critères d'acceptation passés, tests verts, flake8 propre).

## 1. Modules et fichiers de tests prévus

| Module                   | Rôle                                                | Fichier de tests                |
| ------------------------ | --------------------------------------------------- | ------------------------------- |
| `src/main.py`            | Point d'entrée : REPL, lecture d'un fichier         | `tests/test_main.py`            |
| `src/interpreteur.py`    | Analyse d'une ligne et choix de la commande         | `tests/test_interpreteur.py`    |
| `src/validation.py`      | Contrôles communs (noms, entiers, mots réservés)    | `tests/test_validation.py`      |
| `src/modele.py`          | Classes Offre, Diplome, Annee, UE, Enseignant       | `tests/test_modele.py`          |
| `src/commandes.py`       | Une fonction par commande (CREATE, ASSIGN, EDIT...) | `tests/test_commandes.py`       |
| `src/calculs.py`         | Heures, coût, couverture                            | `tests/test_calculs.py`         |
| `src/affichage.py`       | Arbre textuel (DISPLAY GRAPH)                       | `tests/test_affichage.py`       |
| `src/graphviz_export.py` | Texte DOT et appel à `dot` (TRACE GRAPH)            | `tests/test_graphviz_export.py` |
| `src/persistance.py`     | Sauvegarde et chargement JSON                       | `tests/test_persistance.py`     |
| (tous)                   | Tests d'acceptation : un fichier `.txt` par story   | `tests/acceptation/`            |

## 2. Exigences fonctionnelles

| Exigence                        | Story        | Itération                      | Module             | Tests                       | Statut  |
| ------------------------------- | ------------ | ------------------------------ | ------------------ | --------------------------- | ------- |
| EX-01 Créer un diplôme          | US-03        | 1                              | commandes, modele  | test_commandes, test_modele | À faire |
| EX-02 Sélectionner un diplôme   | US-04        | 1                              | commandes, modele  | test_commandes              | À faire |
| EX-03 Sélectionner une année    | US-04        | 1                              | commandes, modele  | test_commandes              | À faire |
| EX-04 Sélection par défaut      | US-04        | 1                              | modele             | test_modele                 | À faire |
| EX-05 Modifier un diplôme       | US-14        | optionnelle                    | commandes          | test_commandes              | À faire |
| EX-06 Créer une UE              | US-05        | 2                              | commandes, modele  | test_commandes, test_modele | À faire |
| EX-07 Modifier une UE           | US-15        | optionnelle                    | commandes          | test_commandes              | À faire |
| EX-08 Supprimer une UE          | US-17        | optionnelle                    | commandes          | test_commandes              | À faire |
| EX-09 Mutualiser une UE         | US-16        | 4                              | commandes, modele  | test_commandes, test_modele | À faire |
| EX-10 Enregistrer un enseignant | US-06        | 2                              | commandes          | test_commandes              | À faire |
| EX-11 Affecter un enseignant    | US-07        | 2                              | commandes          | test_commandes              | À faire |
| EX-12 Calculer les heures       | US-10        | 3                              | calculs            | test_calculs                | À faire |
| EX-13 Calculer le coût          | US-11        | 3                              | calculs            | test_calculs                | À faire |
| EX-14 Calculer la couverture    | US-12        | 3                              | calculs            | test_calculs                | À faire |
| EX-15 Afficher en texte         | US-13        | 3                              | affichage          | test_affichage              | À faire |
| EX-16 Tracer en PNG             | US-18        | optionnelle                    | graphviz_export    | test_graphviz_export        | À faire |
| EX-17 Sauvegarder et charger    | US-08, US-09 | 2 (US-08), optionnelle (US-09) | persistance        | test_persistance            | À faire |
| EX-18 Lire au clavier (REPL)    | US-01        | 1                              | main               | test_main                   | À faire |
| EX-19 Lire un fichier           | US-01        | 1                              | main               | test_main                   | À faire |
| EX-20 Ignorer les commentaires  | US-01        | 1                              | interpreteur       | test_interpreteur           | À faire |
| EX-21 Commande EXIT             | US-01        | 1                              | main, interpreteur | test_main                   | À faire |

## 3. Règles métier

| Règle                                           | Story                      | Itération | Contrainte appliquée dans    | Tests                           |
| ----------------------------------------------- | -------------------------- | --------- | ---------------------------- | ------------------------------- |
| RG-01 60 ECTS par année                         | US-05, US-13               | 2, 3      | validation, affichage        | test_validation, test_affichage |
| RG-02 6 UE maximum par année                    | US-05                      | 2         | validation                   | test_validation                 |
| RG-03 30 h maximum par UE                       | US-05, US-15               | 2         | validation                   | test_validation                 |
| RG-04 90 h maximum par enseignant               | US-07                      | 2         | validation                   | test_validation                 |
| RG-05 Nom unique dans l'OF                      | US-02, US-03, US-06        | 1         | validation                   | test_validation                 |
| RG-06 Mots réservés                             | US-02                      | 1         | validation                   | test_validation                 |
| RG-07 Types de diplôme                          | US-03                      | 1         | modele                       | test_modele                     |
| RG-08 Nombre max d'étudiants                    | US-03, US-13, US-14        | 1         | modele                       | test_modele                     |
| RG-09 UE partagée entre diplômes                | US-16                      | 4         | modele                       | test_modele                     |
| RG-10 Heures paires                             | US-05, US-15               | 2         | validation                   | test_validation                 |
| RG-11 Pas de groupes, ni semestres, ni parcours | US-05                      | 2         | modele (choix de conception) | test_modele                     |
| RG-12 Coût 60 € / 40 €                          | US-11                      | 3         | calculs                      | test_calculs                    |
| RG-13 Formule de couverture                     | US-12                      | 3         | calculs                      | test_calculs                    |
| RG-15 UE d'au moins 1 h                         | US-05                      | 2         | validation                   | test_validation                 |
| RG-16 Effectif strictement positif              | US-03, US-14               | 1         | validation                   | test_validation                 |
| RG-17 Heures affectées ≤ volume de l'UE         | US-07, US-15               | 2         | validation                   | test_validation                 |
| RG-18 Heures qui s'ajoutent                     | US-07                      | 2         | commandes                    | test_commandes                  |
| RG-19 UE partagée comptée une fois dans ALL     | US-10, US-11, US-12, US-16 | 3, 4      | calculs                      | test_calculs                    |
| RG-20 ECTS strictement positifs                 | US-05, US-15               | 2         | validation                   | test_validation                 |
| RG-21 Arrondi au demi supérieur                 | US-12                      | 3         | calculs                      | test_calculs                    |
| RG-22 UE liée une fois par diplôme              | US-16                      | 4         | validation                   | test_validation                 |
| RG-23 Sélection sauvegardée, fichier .bak       | US-08, US-09               | 2         | persistance                  | test_persistance                |
| RG-24 Heures d'`ASSIGN` positives               | US-07                      | 2         | validation                   | test_validation                 |
| RG-25 Noms : un mot, casse respectée            | US-02                      | 1         | validation                   | test_validation                 |

## 4. Exigences non fonctionnelles

| Exigence                             | Comment elle est vérifiée                                    | Quand            |
| ------------------------------------ | ------------------------------------------------------------ | ---------------- |
| NF-01 Python et unittest             | Tous les tests sont des `unittest`                           | Chaque itération |
| NF-02 Console (TUI)                  | US-01                                                        | 1                |
| NF-03 Aucune trace d'exception       | US-02 et test d'une commande invalide par module             | Chaque itération |
| NF-04 PEP 8                          | `flake8 src tests` sans erreur (définition de « terminée »)  | Chaque itération |
| NF-05 Données en JSON                | US-08                                                        | 2                |
| NF-06 Graphviz installé              | US-18, test d'intégration ignoré si `dot` est absent         | Optionnelle      |
| NF-07 Relançable depuis le zip       | README testé sur une machine propre                          | 4                |
| NF-08 XP : 1 livraison, 4 itérations | `03_planning.md`, `journal_iterations.md`                    | Chaque itération |
| NF-09 Format des sorties             | Convention de `02_user_stories.md`, testée dans chaque story | Chaque itération |

# Planning

## 1. Historique

| Date                 | Étape                                                                                        |
| -------------------- | -------------------------------------------------------------------------------------------- |
| 2 novembre 2026      | Lancement du TP en séance à distance avec le prof                                            |
| jusqu'au 22 novembre | Phases 0 et 1 : mise en place (repo, Python, flake8, Graphviz), exigences, stories, planning |
| 23 novembre          | Début de l'itération 1                                                                       |
| 22 janvier 2027      | Livraison (Moodle, avant 18 h pour garder de la marge, limite 23 h)                          |

## 2. Estimation et vélocité

- **Estimation** : temps d'une personne, sans interruption, pour finir une story avec ses tests. L'équipe a estimé chaque story en la comparant à `US-03` (créer un diplôme), prise comme référence à 1 jour.
- **Vélocité** = jours idéaux des stories terminées ÷ jours réellement passés à développer. Au départ, c'est une **hypothèse : 0,7** (une vraie journée donne 0,7 jour idéal à cause des échanges et des blocages). Elle sera remplacée par la mesure à la fin de l'itération 1.
- **Capacité d'une itération** = jours de développement × vélocité.

Capacité de l'équipe : 3 personnes × 2 jours par semaine = 6 jours-personne (pj) par semaine. Pause du 21 décembre au 3 janvier.

| Itération | Dates         | Semaines | pj     | Hors développement (25 %) | Finalisation          | pj de développement | Capacité à v = 0,7 (j idéaux) |
| --------- | ------------- | -------- | ------ | ------------------------- | --------------------- | ------------------- | ----------------------------- |
| 1         | 23/11 – 04/12 | 2        | 12     | 3                         |                       | 9                   | 6,3                           |
| 2         | 07/12 – 18/12 | 2        | 12     | 3                         |                       | 9                   | 6,3                           |
| Pause     | 21/12 – 03/01 |          | 0      |                           |                       |                     |                               |
| 3         | 04/01 – 15/01 | 2        | 12     | 3                         |                       | 9                   | 6,3                           |
| 4         | 18/01 – 22/01 | 1        | 6      | 1                         | 2 (rapport, PDF, zip) | 3                   | 2,1                           |
| **Total** |               | 7        | **42** | 10                        | 2                     | **30**              | **21,0**                      |

« Hors développement » = planning, conception UML et OCL, tests d'acceptation et bilan de l'itération.

## 3. Tâches internes

Travail nécessaire qui n'est pas dans une story.

| ID    | Tâche                                                                                                                 | Itération | Estimation (j) |
| ----- | --------------------------------------------------------------------------------------------------------------------- | --------- | -------------- |
| TI-01 | Architecture du modèle (Offre, Diplome, Annee, UE, Enseignant). Les UE sont des objets partagés, pour préparer US-16. | 1         | 0,6            |
| TI-02 | Outillage de tests et flake8                                                                                          | 1         | 0,4            |
| TI-03 | README                                                                                                                | 1         | 0,3            |
| TI-04 | Refactoring de l'itération 2                                                                                          | 2         | 0,4            |
| TI-05 | Tests d'acceptation automatisés (un fichier `.txt` par story)                                                         | 2         | 0,4            |
| TI-06 | Refactoring et revue de code de l'itération 3                                                                         | 3         | 0,5            |
| TI-07 | README final et nettoyage                                                                                             | 4         | 0,1            |
|       | **Total**                                                                                                             |           | **2,7**        |

## 4. Plan par itération

Une story est **terminée** quand : ses critères d'acceptation passent, tous les tests unitaires passent, flake8 ne signale rien, le code est sur `main`, et les diagrammes et contraintes OCL sont à jour.

| Itération             | Stories                                     | Tâches internes     | Total / capacité (j) |
| --------------------- | ------------------------------------------- | ------------------- | -------------------- |
| **1** (23/11 – 04/12) | US-01, US-02, US-03, US-04 (5,0)            | TI-01 à TI-03 (1,3) | 6,3 / 6,3            |
| **2** (07/12 – 18/12) | US-05, US-06, US-07, US-08 (5,5)            | TI-04, TI-05 (0,8)  | 6,3 / 6,3            |
| **3** (04/01 – 15/01) | US-10, US-11, US-13 (4,5), puis US-12 (1,0) | TI-06 (0,5)         | 6,0 / 6,3            |
| **4** (18/01 – 22/01) | US-16 (2,0)                                 | TI-07 (0,1)         | 2,1 / 2,1            |

- Les stories V1 et V2 font 15,0 j. Avec les stories optionnelles US-12 et US-16 et les tâches internes, le plan complet à v = 0,7 fait 20,7 j sur 21,0 j de capacité.
- À v = 0,7, l'itération 1 ne contient pas encore de création d'UE : US-05 (2 j) ne rentre pas. Elle est ajoutée à l'itération 1 si la vélocité mesurée le permet (il faudrait v ≥ 0,92).
- Avant la pause, l'équipe étiquette le code (`git tag it2-avant-pause`) et écrit son bilan dans le journal.

## 5. Stories optionnelles

Elles entrent dans cet ordre, quand la vélocité mesurée le permet. « v requis » = (15,0 + 2,7 de tâches internes + cumul) / 30 j de capacité à v = 1.

| Ordre | Story                     | Estimation (j) | Cumul (j) | v requis |
| ----- | ------------------------- | -------------- | --------- | -------- |
| 1     | US-12 Couverture          | 1,0            | 18,7      | 0,62     |
| 2     | US-16 Mutualiser          | 2,0            | 20,7      | 0,69     |
| 3     | US-15 Modifier une UE     | 1,5            | 22,2      | 0,74     |
| 4     | US-14 Modifier un diplôme | 0,5            | 22,7      | 0,76     |
| 5     | US-18 Tracer en PNG       | 2,0            | 24,7      | 0,82     |
| 6     | US-09 Fichier corrompu    | 1,0            | 25,7      | 0,86     |
| 7     | US-17 Supprimer une UE    | 1,5            | 27,2      | 0,91     |

Tout le projet tient donc si la vélocité réelle est d'au moins 0,91. Si elle est plus faible, on retire les stories d'en bas. Le sujet cite Graphviz plusieurs fois : si `TRACE GRAPH` risque de manquer, on peut la monter dans l'ordre en décalant les autres.

## 6. Points de pilotage

| Date  | Point                                                                            |
| ----- | -------------------------------------------------------------------------------- |
| 04/12 | Bilan itération 1 : vélocité mesurée, ajustement de la capacité de l'itération 2 |
| 18/12 | Bilan itération 2 et étiquette Git avant la pause                                |
| 15/01 | Bilan itération 3 : on fixe le périmètre final                                   |
| 20/01 | Gel du code : plus de nouvelle fonctionnalité, tests et rapport                  |
| 22/01 | Dépôt du PDF et du zip sur Moodle                                                |

## 7. Risques

| Risque                                     | Réponse                                                                                   |
| ------------------------------------------ | ----------------------------------------------------------------------------------------- |
| Itération 1 sans UE : démonstration mince  | Assumé : on livre une base propre. On tire US-05 si la vélocité le permet.                |
| Pause de deux semaines au milieu du projet | Étiquette Git, bilan écrit et relecture des tests à la reprise                            |
| Examens de janvier                         | Vérifier le calendrier. Si une semaine saute, on retire d'abord US-17, US-09, puis US-18. |
| US-16 (risque élevé) en dernière semaine   | Le modèle prépare le partage des UE dès TI-01                                             |

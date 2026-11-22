# Plan de livraison

Version 1.0, phase 1 (engagement, étapes 1.10 et 1.11). Client (MOA) : simulé. Équipe : 3 MOE.
Sources : `02_user_stories.md` (estimations), `03_priorites_risques.md` (valeur, risque, vélocité, capacité).
Unités : jours idéaux (j) pour les stories, jours-personne (pj) pour le temps passé.

## 1. Principe

- **1 livraison** : vendredi 22 janvier 2027, 23h00. Dépôt Moodle visé le même jour **avant 18h**, pour garder une marge en cas de problème d'envoi.
- **Périmètre engagé** : stories V1 et V2, soit **15,0 j**. Il tient à la vélocité prudente de 0,7.
- **Périmètre conditionnel** : stories V3 et V4, soit **9,5 j**. Elles entrent dans l'ordre du classement (`03` §2) dès que la vélocité mesurée le permet.
- **Tâches internes** : 2,85 j de travail non rattaché à une story (architecture, outillage, refactoring, README). Elles sont planifiées explicitement pour ne pas être oubliées.
- Une story n'est jamais planifiée avant celles dont elle dépend.

**Définition de « terminée »** (utilisée par le tracker pour calculer la vélocité) :

1. Les critères d'acceptation de la story passent (test d'acceptation exécuté par le testeur).
2. Les tests unitaires passent, y compris ceux des stories précédentes.
3. `flake8` ne signale aucune erreur.
4. Le code est intégré sur `main`.
5. Les diagrammes UML et les contraintes OCL de la story sont à jour.

## 2. Calendrier

| Période       | Étape                                   | pj  | Sortie                                      |
| ------------- | --------------------------------------- | --- | ------------------------------------------- |
| 23/11 – 27/11 | Phase 1 (exploration, engagement, plan) | 6   | Documents 01 à 05, diagrammes d'exploration |
| 30/11 – 11/12 | **Itération 1** (2 semaines)            | 12  | Squelette : lecture, validation, diplômes   |
| 14/12 – 18/12 | **Itération 2**, partie 1               | 6   | UE et enseignants                           |
| 21/12 – 03/01 | Pause (aucun travail)                   | 0   | Tag Git `it2-avant-pause`                   |
| 04/01 – 08/01 | **Itération 2**, partie 2               | 6   | Affectations et sauvegarde                  |
| 11/01 – 15/01 | **Itération 3**                         | 6   | Heures et coût                              |
| 18/01 – 22/01 | **Itération 4** et finalisation         | 6   | Affichage, rapport, zip, livraison          |

## 3. Tâches internes (non rattachées à une story)

| ID    | Tâche                                                                                                                                                                    | Itération | Estimation (j) |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------- | -------------- |
| TI-01 | Architecture et squelette du modèle (Offre, Diplome, Annee, UE, Enseignant, Affectation). Les UE sont des objets partagés, référencés par leur nom, pour préparer US-16. | 1         | 0,6            |
| TI-02 | Outillage de tests : dossier `tests/`, commande unique de lancement, `flake8`                                                                                            | 1         | 0,4            |
| TI-03 | README (lancement, tests, version de Python)                                                                                                                             | 1         | 0,3            |
| TI-04 | Refactoring et intégration de l'itération 2                                                                                                                              | 2         | 0,4            |
| TI-05 | Exécuteur de tests d'acceptation (un fichier `.txt` et une sortie attendue par story)                                                                                    | 2         | 0,4            |
| TI-06 | Refactoring de l'itération 3                                                                                                                                             | 3         | 0,35           |
| TI-07 | Revue de code par le coach (PEP 8, doublons, nommage)                                                                                                                    | 3         | 0,3            |
| TI-08 | Mise à jour finale du README et nettoyage                                                                                                                                | 4         | 0,1            |
|       | **Total**                                                                                                                                                                |           | **2,85**       |

## 4. Plan par itération

Capacité calculée à v = 0,7 (pj de développement × 0,7). Le planning détaillé en tâches se fait au début de chaque itération.

### Itération 1 : squelette (30/11 – 11/12)

| Contenu                            | Estimation (j) |
| ---------------------------------- | -------------- |
| US-01 Lire les commandes           | 1,5            |
| US-02 Valider saisies et noms      | 1,5            |
| US-03 Créer un diplôme             | 1,0            |
| US-04 Sélectionner diplôme / année | 1,0            |
| TI-01, TI-02, TI-03                | 1,3            |
| **Total engagé / capacité**        | **6,3 / 6,3**  |

- **Conditionnelle** : US-05 (2,0 j). Elle exige 8,3 j au total, donc v ≥ 0,92 sur cette itération.
- **Démonstration** : lancer le programme en REPL et depuis un fichier, créer et sélectionner des diplômes, voir les erreurs claires, quitter avec `EXIT`.
- **Vérification des stories** : par les messages `OK` / `ERREUR` et par les tests unitaires du modèle (il n'existe pas encore d'affichage).
- **Conception** : diagramme de classes initial, paquetages (parser, modèle, validation), OCL pour RG-05, RG-06, RG-07, RG-08, RG-16, RG-18, RG-25.

### Itération 2 : UE, enseignants, sauvegarde (14/12 – 18/12 puis 04/01 – 08/01)

| Contenu                            | Estimation (j) |
| ---------------------------------- | -------------- |
| US-05 Créer une UE                 | 2,0            |
| US-06 Enregistrer un enseignant    | 0,5            |
| US-07 Affecter un enseignant       | 1,5            |
| US-08 Sauvegarde / chargement auto | 1,5            |
| TI-04, TI-05                       | 0,8            |
| **Total engagé / capacité**        | **6,3 / 6,3**  |

- **Avant la pause** : US-05 et US-06 terminées, US-08 démarrée. Tag Git `it2-avant-pause`, bilan écrit dans le journal.
- **Reprise le 04/01** : relire le journal, lancer tous les tests, puis continuer.
- **Conditionnelles** : US-12 (après US-07), US-16, US-14. Si US-05 a été faite en It1, les 2,0 j libérés vont à US-12 puis US-16.
- **Démonstration** : construire un diplôme complet, affecter des enseignants, quitter, relancer et retrouver les données.
- **Vérification de US-05** : via `data/offre.json` (dès US-08) et les tests unitaires, car `DISPLAY GRAPH` n'existe pas encore.
- **Conception** : diagramme de classes complété, diagramme de machine à états de la session (diplôme et année sélectionnés), OCL pour RG-01 à RG-04, RG-10, RG-15, RG-17, RG-19, RG-20, RG-24.

### Itération 3 : heures et coût (11/01 – 15/01)

| Contenu                     | Estimation (j)  |
| --------------------------- | --------------- |
| US-10 Calculer les heures   | 1,5             |
| US-11 Calculer le coût      | 1,0             |
| TI-06, TI-07                | 0,65            |
| **Total engagé / capacité** | **3,15 / 3,15** |

- **Conditionnelles** : US-12, US-16, US-15, US-14 (dans cet ordre, selon la vélocité).
- **Démonstration** : `GET HOURS` et `GET COST` sur les données de JD-1.
- **Conception** : OCL des requêtes de calcul (RG-12, RG-13, RG-21), diagramme de séquence de `GET COST`.

### Itération 4 : affichage et livraison (18/01 – 22/01)

| Contenu                     | Estimation (j) |
| --------------------------- | -------------- |
| US-13 Afficher en texte     | 2,0            |
| TI-08                       | 0,1            |
| **Total engagé / capacité** | **2,1 / 2,1**  |

- La capacité de développement est réduite : sur les 6 pj de la semaine, 2 sont réservés à la finalisation (rapport, PDF, zip, tests sur une machine propre) et 1 à la conception et aux tests d'acceptation.
- **Conditionnelles** : US-09, US-17, US-18. Elles ne tiennent que si la vélocité dépasse nettement 1 (voir §6).
- **Démonstration** : `DISPLAY GRAPH` complet, avec avertissements ECTS.

## 5. Stories conditionnelles : ordre de tirage

« v moyen requis » = (15,0 + 2,85 + cumul des conditionnelles) / 25,5 j de capacité de développement à v = 1.

| Ordre | Story                     | Estimation (j) | Cumul tâches internes incluses (j) | v moyen requis | Itération visée | Condition particulière                         |
| ----- | ------------------------- | -------------- | ---------------------------------- | -------------- | --------------- | ---------------------------------------------- |
| 1     | US-12 Couverture          | 1,0            | 18,85                              | 0,74           | 3 (ou 2)        | Après US-07                                    |
| 2     | US-16 Mutualiser          | 2,0            | 20,85                              | 0,82           | 2 ou 3          | Risque élevé : le modèle est préparé dès TI-01 |
| 3     | US-15 Modifier une UE     | 1,5            | 22,35                              | 0,88           | 3               | Après US-05 et US-07                           |
| 4     | US-14 Modifier un diplôme | 0,5            | 22,85                              | 0,90           | 2 ou 3          | Peut se glisser dans n'importe quelle marge    |
| 5     | US-18 Tracer en PNG       | 2,0            | 24,85                              | 0,97           | 4               | Après US-13 : voir §6                          |
| 6     | US-09 Fichier corrompu    | 1,0            | 25,85                              | 1,01           | 3 ou 4          | Après US-08                                    |
| 7     | US-17 Supprimer une UE    | 1,5            | 27,35                              | 1,07           | 3 ou 4          | Après US-05                                    |

Lecture : à v = 0,7 en moyenne, seules les stories engagées sont livrées. À v = 0,85, US-12 et US-16 entrent. Le tout n'est atteint qu'à v = 1,07.

## 6. Points de pilotage

| Date        | Point                  | Décision à prendre                                                                                                                                                                                                   |
| ----------- | ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 04/12       | Mi-itération 1         | Le tracker calcule une vélocité partielle. Si v ≥ 0,92, US-05 est ajoutée à l'itération 1.                                                                                                                           |
| 11/12       | Bilan de l'itération 1 | Remplir `03` §8 et `indicateurs.md`. Recalculer la capacité de l'itération 2 avec la vélocité mesurée.                                                                                                               |
| 18/12       | Avant la pause         | Tag `it2-avant-pause`, journal à jour.                                                                                                                                                                               |
| 08/01       | Bilan de l'itération 2 | Recalculer It3 et It4. **Si v₂ ≥ 0,82, US-13 passe en It3 et US-10 en It4** (It3 : US-13 + US-11 + 0,65 = 3,65 j, capacité 4,5 × 0,82 = 3,7 j). Cela met la story la plus risquée tôt et libère la dernière semaine. |
| 15/01       | Bilan de l'itération 3 | Figer le périmètre final de l'itération 4. Retirer les stories qui ne tiennent plus.                                                                                                                                 |
| 20/01       | Gel du code            | Plus de nouvelle fonctionnalité. Tests, README, rapport.                                                                                                                                                             |
| 22/01 (18h) | Dépôt                  | PDF + zip sur Moodle.                                                                                                                                                                                                |

## 7. Vue d'ensemble des stories

| ID    | Titre                        | Valeur | Estimation (j) | Itération          | Statut             |
| ----- | ---------------------------- | ------ | -------------- | ------------------ | ------------------ |
| US-01 | Lire les commandes           | V1     | 1,5            | 1                  | Engagée            |
| US-02 | Valider saisies et noms      | V1     | 1,5            | 1                  | Engagée            |
| US-03 | Créer un diplôme             | V1     | 1,0            | 1                  | Engagée            |
| US-04 | Sélectionner diplôme / année | V1     | 1,0            | 1                  | Engagée            |
| US-05 | Créer une UE                 | V1     | 2,0            | 2 (1 si v ≥ 0,92)  | Engagée            |
| US-06 | Enregistrer un enseignant    | V2     | 0,5            | 2                  | Engagée            |
| US-07 | Affecter un enseignant       | V2     | 1,5            | 2                  | Engagée            |
| US-08 | Sauvegarde / chargement      | V2     | 1,5            | 2                  | Engagée            |
| US-10 | Calculer les heures          | V2     | 1,5            | 3                  | Engagée            |
| US-11 | Calculer le coût             | V2     | 1,0            | 3                  | Engagée            |
| US-13 | Afficher en texte            | V2     | 2,0            | 4 (3 si v₂ ≥ 0,82) | Engagée            |
| US-12 | Couverture                   | V3     | 1,0            | 3                  | Conditionnelle (1) |
| US-16 | Mutualiser une UE            | V3     | 2,0            | 2 ou 3             | Conditionnelle (2) |
| US-15 | Modifier une UE              | V3     | 1,5            | 3                  | Conditionnelle (3) |
| US-14 | Modifier un diplôme          | V3     | 0,5            | 2 ou 3             | Conditionnelle (4) |
| US-18 | Tracer en PNG                | V3     | 2,0            | 4                  | Conditionnelle (5) |
| US-09 | Fichier corrompu             | V3     | 1,0            | 3 ou 4             | Conditionnelle (6) |
| US-17 | Supprimer une UE             | V4     | 1,5            | 3 ou 4             | Conditionnelle (7) |

Engagé par itération : It1 5,0 j, It2 5,5 j, It3 2,5 j, It4 2,0 j, soit **15,0 j**.

## 8. Risques du plan et leviers

| ID    | Risque                                                                                                                                                                                                                   | Probabilité        | Réponse                                                                                                          |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------ | ---------------------------------------------------------------------------------------------------------------- |
| RP-01 | L'itération 1 livre un squelette sans UE. La démonstration est mince.                                                                                                                                                    | Certaine à v = 0,7 | Assumé : la base technique (parser, validation, modèle) est livrée proprement. US-05 est tirée dès que v ≥ 0,92. |
| RP-02 | La pause de 2 semaines coupe l'itération 2 et fait perdre le fil.                                                                                                                                                        | Moyenne            | Tag Git et bilan écrit avant la pause, reprise par relecture du journal et lancement de tous les tests.          |
| RP-03 | US-13 (V2) est en dernière semaine, avec peu de marge.                                                                                                                                                                   | Moyenne            | Point de pilotage du 08/01 : avancement en It3 si v₂ ≥ 0,82.                                                     |
| RP-04 | **US-18 (`TRACE GRAPH`) n'est pas livrée.** Elle dépend de US-13 et passe après quatre autres conditionnelles. Même avec l'avancement de US-13, la dernière semaine n'a que 0,9 j libre à v = 1, pour 2,0 j nécessaires. | Élevée             | Voir les leviers ci-dessous. Le sujet insiste sur Graphviz (diapositives 5, 14 et 27) : à surveiller.            |
| RP-05 | Les examens de janvier réduisent la disponibilité.                                                                                                                                                                       | À vérifier         | Vérifier le calendrier cette semaine. Si une semaine saute, retirer d'abord US-17, US-09, puis US-18.            |
| RP-06 | Un membre est absent ou malade.                                                                                                                                                                                          | Faible             | Propriété collective du code (XP) : au moins 2 personnes connaissent chaque module.                              |

**Leviers de capacité** (à décider en groupe, puis à noter dans le journal des décisions) :

| Levier                                    | Effet                                                                                                                                                              |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Travailler une semaine pendant la pause   | +6 pj, soit +3,15 j idéaux à v = 0,7. Capacité totale 21,0 j : US-12 et US-16 tiennent (20,85 j requis).                                                           |
| Passer à 3 jours par semaine              | Environ +50 % de capacité : tout sauf US-17 tient à v = 0,7.                                                                                                       |
| Si vous voulez **garantir `TRACE GRAPH`** | Dites-le-moi : le MOA peut remonter US-18 en priorité V2 et avancer US-13. Le périmètre engagé passe alors à 17,0 j, ce qui exige un levier ci-dessus ou v ≥ 0,78. |

## 9. Corrections à reporter dans les autres fichiers

- **`03_priorites_risques.md` §6** : remplace la ligne `Seuil | 24,5 | Tout tient si v ≥ 0,96.` par `Seuil | 24,5 + 2,85 de tâches internes | Les stories seules tiennent si v ≥ 0,96. Avec les tâches internes, tout tient si v ≥ 1,07.`
- **`03_priorites_risques.md` §5** : remplace « les semaines des 14 et 18 décembre, puis celles du 4 et du 8 janvier » par « la semaine du 14 décembre, puis la semaine du 4 janvier ».

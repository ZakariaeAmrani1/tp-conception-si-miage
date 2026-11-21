# Priorités, risques, vélocité et capacité

Version 1.0, phase 1 (engagement, étapes 1.7 à 1.9). Client (MOA) : simulé. Équipe : 3 MOE.
Estimations issues de `02_user_stories.md` (jours idéaux : 1 personne, 1 journée sans interruption, tests et refactoring inclus).

## 1. Hypothèses de capacité

| Hypothèse                                                                                      | Valeur                                                   | Modifiable si…                          |
| ---------------------------------------------------------------------------------------------- | -------------------------------------------------------- | --------------------------------------- |
| Taille de l'équipe                                                                             | 3 personnes                                              |                                         |
| Disponibilité                                                                                  | 2 jours par semaine et par personne = **6 pj / semaine** | l'équipe change son rythme              |
| Début de la phase 1                                                                            | lundi 23 novembre 2026                                   |                                         |
| Pause                                                                                          | du 21 décembre 2026 au 3 janvier 2027 (aucun travail)    | l'équipe travaille pendant les vacances |
| Date limite                                                                                    | vendredi 22 janvier 2027, 23h00                          |                                         |
| Semaines travaillées                                                                           | 7 : 23/11, 30/11, 07/12, 14/12, 04/01, 11/01, 18/01      |                                         |
| Capacité totale                                                                                | 7 × 6 = **42 pj**                                        |                                         |
| Temps hors développement (planning, conception UML/OCL, tests d'acceptation, bilan, rédaction) | **25 %** du temps de chaque itération                    | le bilan montre un autre ratio          |
| Finalisation du rapport et du zip                                                              | 2 pj, dans la dernière semaine                           |                                         |

Point d'attention : vérifier le calendrier d'examens de janvier. S'il réduit le temps disponible, la capacité baisse et le périmètre doit suivre (§6).

## 2. Tri par valeur (étape 1.7, décidé par le MOA)

Échelle de valeur :

- **V1** : indispensable, rien ne fonctionne sans.
- **V2** : cœur de la valeur métier, fortement attendu.
- **V3** : demandé par le sujet, mais le cœur fonctionne sans.
- **V4** : souhaitable.

| Rang | ID    | Story                        | Valeur | Estimation (j) | Cumul (j) | Dépend de    | Justification du MOA                                             |
| ---- | ----- | ---------------------------- | ------ | -------------- | --------- | ------------ | ---------------------------------------------------------------- |
| 1    | US-01 | Lire les commandes           | V1     | 1,5            | 1,5       |              | Sans entrée de commandes, rien n'est utilisable.                 |
| 2    | US-02 | Valider saisies et noms      | V1     | 1,5            | 3,0       | US-01        | Le sujet exige un programme qui gère les erreurs.                |
| 3    | US-03 | Créer un diplôme             | V1     | 1,0            | 4,0       | US-01, US-02 | Base du modèle.                                                  |
| 4    | US-04 | Sélectionner diplôme / année | V1     | 1,0            | 5,0       | US-03        | Nécessaire pour cibler les UE.                                   |
| 5    | US-05 | Créer une UE                 | V1     | 2,0            | 7,0       | US-04        | C'est le contenu d'un diplôme.                                   |
| 6    | US-06 | Enregistrer un enseignant    | V2     | 0,5            | 7,5       | US-02        | Préalable aux affectations.                                      |
| 7    | US-07 | Affecter un enseignant       | V2     | 1,5            | 9,0       | US-05, US-06 | Sans affectation, pas de couverture ni de volume par enseignant. |
| 8    | US-08 | Sauvegarde / chargement auto | V2     | 1,5            | 10,5      | US-03        | Le sujet demande un travail en plusieurs fois.                   |
| 9    | US-10 | Calculer les heures          | V2     | 1,5            | 12,0      | US-05, US-07 | Volumétrie, une des fonctions centrales.                         |
| 10   | US-11 | Calculer le coût             | V2     | 1,0            | 13,0      | US-05        | Le but du projet est une offre soutenable pour le budget.        |
| 11   | US-13 | Afficher en texte            | V2     | 2,0            | 15,0      | US-05, US-07 | Permet de contrôler visuellement un diplôme.                     |
| 12   | US-12 | Calculer la couverture       | V3     | 1,0            | 16,0      | US-07        | Indicateur utile, mais pas bloquant.                             |
| 13   | US-16 | Mutualiser une UE            | V3     | 2,0            | 18,0      | US-05        | Demandé par le sujet, mais le cœur fonctionne sans.              |
| 14   | US-15 | Modifier une UE              | V3     | 1,5            | 19,5      | US-05, US-07 | Correction d'erreurs de saisie.                                  |
| 15   | US-14 | Modifier un diplôme          | V3     | 0,5            | 20,0      | US-03        | Correction simple.                                               |
| 16   | US-18 | Tracer en PNG                | V3     | 2,0            | 22,0      | US-13        | Demandé par le sujet, dépend de l'arbre textuel.                 |
| 17   | US-09 | Fichier corrompu             | V3     | 1,0            | 23,0      | US-08        | Robustesse, utile une fois la sauvegarde en place.               |
| 18   | US-17 | Supprimer une UE             | V4     | 1,5            | 24,5      | US-05        | Priorité basse décidée en Q-01.                                  |

Les dépendances sont des contraintes d'ordre : une story ne peut pas être livrée avant celles dont elle dépend.

## 3. Tri par risque (étape 1.8)

Le risque mesuré ici est le **risque sur la précision de l'estimation**, c'est-à-dire la chance que le temps réel s'écarte de l'estimation. Marge appliquée pour le pire cas : faible +20 %, moyen +40 %, élevé +60 %.

| Niveau    | ID                                                            | Estimation (j) | Pourquoi                                                                                           | Action pour réduire le risque                                                                                                           |
| --------- | ------------------------------------------------------------- | -------------- | -------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| **Élevé** | US-16                                                         | 2,0            | Touche le modèle, les calculs, l'affichage, la sauvegarde et la suppression (une UE est partagée). | Concevoir le modèle dès l'itération 1 avec les UE comme objets partagés référencés par leur nom, même si la story est livrée plus tard. |
| Moyen     | US-02                                                         | 1,5            | Concerne toutes les commandes.                                                                     | Un module de validation commun, testé seul.                                                                                             |
| Moyen     | US-05                                                         | 2,0            | Beaucoup de règles à combiner (RG-01, 02, 03, 10, 15, 19, 20).                                     | Table de décision des cas limites, écrite avant le code.                                                                                |
| Moyen     | US-08                                                         | 1,5            | Sérialiser des objets liés entre eux, écriture atomique.                                           | Tester un aller-retour sauvegarde/chargement dès la 1re version.                                                                        |
| Moyen     | US-09                                                         | 1,0            | Détecter un JSON valide mais incohérent.                                                           | Une fonction de validation de cohérence, testée avec des fichiers piégés.                                                               |
| Moyen     | US-13                                                         | 2,0            | Format d'affichage à plusieurs niveaux, avertissements.                                            | Tests sur les lignes clés et leur ordre, pas sur les espaces.                                                                           |
| Moyen     | US-15                                                         | 1,5            | Contrôles en cascade (heures affectées, ECTS des années liées).                                    | Réutiliser les validations de US-05.                                                                                                    |
| Moyen     | US-17                                                         | 1,5            | Suppression en cascade selon le nombre de liens.                                                   | Tester les trois cas (UE simple, mutualisée, dernier lien).                                                                             |
| Moyen     | US-18                                                         | 2,0            | Dépendance externe : Graphviz doit être installé.                                                  | Séparer la génération du DOT de l'appel à `dot` ; mock + test d'intégration ignoré si `dot` est absent.                                 |
| Faible    | US-01, US-03, US-04, US-06, US-07, US-10, US-11, US-12, US-14 | 9,5 au total   | Logique directe, règles claires.                                                                   |                                                                                                                                         |

Bilan du risque :

| Niveau    | Nombre de stories | Estimation (j) | Pire cas (j) |
| --------- | ----------------- | -------------- | ------------ |
| Faible    | 9                 | 9,5            | 11,4         |
| Moyen     | 8                 | 13,0           | 18,2         |
| Élevé     | 1                 | 2,0            | 3,2          |
| **Total** | **18**            | **24,5**       | **32,8**     |

Conclusion : si les risques se réalisent, le travail réel peut atteindre **32,8 jours idéaux**. Le tri par risque sert à traiter tôt ce qui est incertain, sans passer devant les stories de plus grande valeur.

## 4. Vélocité (étape 1.9)

**Définition** (sujet, p.44) : vélocité = temps technique idéal / temps réel.

- Temps idéal : somme des estimations des stories **terminées** (acceptées par le testeur).
- Temps réel : jours-personne passés à développer (code + tests + refactoring + intégration), mesurés par le tracker.

**Valeur de départ : 0,7.** C'est une hypothèse (1 jour réel de développement = 0,7 jour idéal), car une journée réelle comporte des échanges, des blocages et des tâches annexes. Elle sera **remplacée par la mesure** à la fin de l'itération 1, puis ajustée à chaque itération.

Règle de pilotage : la vélocité de l'itération suivante est celle de l'itération qui vient de se terminer.

## 5. Capacité par itération

Dates des itérations (à confirmer dans le plan de livraison) :

| Itération | Dates                            | Semaines travaillées | pj réels                  | Hors développement (25 %) | pj de développement | Capacité en jours idéaux à v = 0,7 | à v = 1,0 |
| --------- | -------------------------------- | -------------------- | ------------------------- | ------------------------- | ------------------- | ---------------------------------- | --------- |
| 1         | 30/11 – 11/12                    | 2                    | 12                        | 3                         | 9,0                 | 6,3                                | 9,0       |
| 2         | 14/12 – 18/12 puis 04/01 – 08/01 | 2                    | 12                        | 3                         | 9,0                 | 6,3                                | 9,0       |
| 3         | 11/01 – 15/01                    | 1                    | 6                         | 1,5                       | 4,5                 | 3,15                               | 4,5       |
| 4         | 18/01 – 22/01                    | 1                    | 6, dont 2 de finalisation | 1                         | 3,0                 | 2,1                                | 3,0       |
| **Total** |                                  | 6                    | 36                        |                           | **25,5**            | **17,85**                          | **25,5**  |

La phase 1 (23/11 – 27/11, 6 pj) n'est pas comptée ici : elle est consacrée aux documents, aux diagrammes d'exploration et au plan.

Si l'itération 2 ne se déroule pas en continu, c'est à cause de la pause : les semaines des 14 et 18 décembre, puis celles du 4 et du 8 janvier, forment les deux semaines de travail de cette itération.

## 6. Conséquences sur le périmètre

| Scénario         | Capacité totale (j idéaux) | Stories de 24,5 j qui tiennent                                          |
| ---------------- | -------------------------- | ----------------------------------------------------------------------- |
| Prudent, v = 0,7 | 17,85                      | V1 + V2 (15,0 j) tiennent. V3 + V4 (9,5 j) ne tiennent pas entièrement. |
| Cible, v = 1,0   | 25,5                       | Tout tient, avec 1,0 j de marge.                                        |
| Seuil            | 24,5                       | **Tout tient si v ≥ 0,96.**                                             |

À retenir :

- Le périmètre **engagé** est celui qui tient à v = 0,7 : les stories V1 et V2, soit 15,0 j.
- Les stories V3 et V4 sont **conditionnelles** : elles sont intégrées dans l'ordre du §2, dès que la vélocité mesurée le permet.
- Les marges de capacité sont réservées aux tâches internes (architecture, outillage de tests, flake8, README, refactoring) qui ne sont pas estimées dans les stories.
- Si la vélocité mesurée est inférieure à 0,7, ou si le calendrier perd des semaines, on retire d'abord les stories de plus bas rang (US-17, puis US-09, US-18…) et on le consigne dans le journal des décisions.

Leviers pour augmenter la capacité si la vélocité reste basse : travailler une semaine pendant la pause (+6 pj, soit environ +3 jours idéaux), augmenter la disponibilité de 2 à 3 jours par semaine, ou simplifier une story avec l'accord du MOA.

## 7. Règles de pilotage pour les itérations

1. En début d'itération, le tracker annonce la vélocité de l'itération précédente et la capacité en jours idéaux (pj de développement × vélocité).
2. On prend les stories dans l'ordre du §2, tant qu'elles tiennent dans la capacité. Une story trop grosse pour la capacité restante est sautée au profit de la suivante, si ses dépendances sont satisfaites.
3. Vélocité plus faible que prévu : on retire des stories en commençant par le bas du classement. Vélocité plus élevée : on ajoute les stories suivantes.
4. Si le MOA change une priorité, on met à jour le §2 et on l'inscrit dans `suivi_decisions.md`.
5. À la fin de chaque itération, on remplit le tableau ci-dessous et `indicateurs.md`.

## 8. Suivi de la vélocité (à remplir)

| Itération | pj de développement prévus | Capacité prévue (j idéaux) | Stories terminées | Jours idéaux terminés | pj de développement réels | Vélocité réelle |
| --------- | -------------------------- | -------------------------- | ----------------- | --------------------- | ------------------------- | --------------- |
| 1         | 9,0                        | 6,3                        |                   |                       |                           |                 |
| 2         | 9,0                        | 6,3                        |                   |                       |                           |                 |
| 3         | 4,5                        | 3,15                       |                   |                       |                           |                 |
| 4         | 3,0                        | 2,1                        |                   |                       |                           |                 |

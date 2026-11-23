# Suivi des décisions

Le rôle de client est tenu par Alex. L'équipe propose les décisions, puis le client les valide. Les validations ont eu lieu en séance d'équipe le 22/11/2026.

## 1. Décisions

| ID   | Sujet                              | Décision                                                                                                                                                | Impact                     | Validée par Alex |
| ---- | ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------- | ---------------- |
| D-01 | Suppression d'une UE               | Ajouter `DELETE UE <nom>` (mot réservé). Une UE partagée perd seulement le lien avec l'année.                                                           | EX-08, US-17               | Oui, 22/11/2026  |
| D-02 | Deux formes d'`ASSIGN`             | Si le 1er mot après `ASSIGN` est `UE`, c'est la mutualisation. `UE` étant interdit comme nom, il n'y a pas d'ambiguïté.                                 | US-07, US-16               | Oui, 22/11/2026  |
| D-03 | Noms                               | Uniques dans toute l'OF, un seul mot, majuscules respectées. Mots réservés sans tenir compte de la casse.                                               | RG-05, RG-06, RG-25        | Oui, 22/11/2026  |
| D-04 | Identifiant d'un enseignant        | Le nom seul.                                                                                                                                            | RG-05, US-06               | Oui, 22/11/2026  |
| D-05 | Totaux et UE partagées             | Une UE partagée est comptée une fois dans `ALL`, et une seule fois par diplôme.                                                                         | RG-19, RG-22               | Oui, 22/11/2026  |
| D-06 | 60 ECTS                            | Dépassement refusé à la création. Moins de 60 : avertissement à l'affichage. ECTS entiers positifs.                                                     | RG-01, RG-20, US-05, US-13 | Oui, 22/11/2026  |
| D-07 | Affectations                       | Heures strictement positives. Refus si l'UE ou les 90 h de l'enseignant sont dépassés. Même enseignant sur la même UE : les heures s'ajoutent.          | RG-04, RG-17, RG-18, RG-24 | Oui, 22/11/2026  |
| D-08 | Heures d'une UE                    | Heures paires, volume total supérieur à 0.                                                                                                              | RG-10, RG-15               | Oui, 22/11/2026  |
| D-09 | Modification d'une UE partagée     | Elle vaut pour tous les diplômes qui la contiennent.                                                                                                    | US-15                      | Oui, 22/11/2026  |
| D-10 | Sauvegarde                         | Automatique dans `data/offre.json`, avec la sélection. Copie `.bak` avant écrasement. Fichier illisible : offre vide et fichier non écrasé.             | EX-17, RG-23, US-08, US-09 | Oui, 22/11/2026  |
| D-11 | Coût et couverture d'un enseignant | Refusés avec un message clair.                                                                                                                          | US-11, US-12               | Oui, 22/11/2026  |
| D-12 | Nombre max d'étudiants             | Stocké et affiché, strictement positif, aucun calcul.                                                                                                   | RG-08, RG-16               | Oui, 22/11/2026  |
| D-13 | Arrondi de la couverture           | Demi supérieur (12,5 donne 13), pas le `round` de Python.                                                                                               | RG-21, US-12               | Oui, 22/11/2026  |
| D-14 | Sorties                            | `OK ...`, valeur seule pour `GET`, `ERREUR : ...`. En mode fichier, on continue après une erreur.                                                       | NF-09                      | Oui, 22/11/2026  |
| D-15 | Types de diplôme                   | Le type est une classe `TypeDiplome` (nom unique, nombre d'années), avec 4 types créés au départ. Aucune commande pour en ajouter dans cette livraison. | RG-07, modèle              |                  |

## 2. Échanges avec le client

| Date       | Participants                | Résumé                                                            | Suites                                                                                                |
| ---------- | --------------------------- | ----------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| 22/11/2026 | Alex, Zakariae, Ayoub, Sara | Revue des décisions D-01 à D-14. Toutes validées sans changement. | Faire les diagrammes d'exploration et la matrice des exigences, puis démarrer l'itération 1 le 23/11. |

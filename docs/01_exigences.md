# Exigences

Projet : prototype de modélisation d'une offre de formation universitaire.
Version : 1.0 (phase 1, exploration). Ce document évolue à chaque itération.

Légende des identifiants :

- **EX-xx** : exigence fonctionnelle
- **RG-xx** : règle métier (sera traduite en contrainte OCL)
- **NF-xx** : exigence non fonctionnelle
- **Q-xx** : question à poser au client (voir `suivi_decisions.md`)

## 1. Vocabulaire

| Terme        | Définition                                                               |
| ------------ | ------------------------------------------------------------------------ |
| OF           | Offre de formation : ensemble des diplômes de l'université               |
| Diplôme      | BUT, Licence, Master ou LicencePro, sur 1 à 3 années                     |
| UE           | Unité d'enseignement, rattachée à une année d'un diplôme                 |
| CM / TD / TP | Types d'enseignement d'une UE, en heures                                 |
| Séance       | Lot de 2h de CM, TD ou TP                                                |
| Couverture   | Heures d'enseignants affectées / heures totales, en % arrondi à l'entier |

Types de diplômes :

| Type       | Années | ECTS total |
| ---------- | ------ | ---------- |
| LicencePro | 1      | 60         |
| Licence    | 3      | 180        |
| BUT        | 3      | 180        |
| Master     | 2      | 120        |

## 2. Exigences fonctionnelles

### 2.1 Gestion des diplômes

| ID    | Exigence                                                                                                                   | Commande                                | Source |
| ----- | -------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | ------ |
| EX-01 | Créer un diplôme avec un nom, un type et un nombre maximum d'étudiants. Le nombre d'années et d'ECTS sont déduits du type. | `CREATE DEGREE <name> <type> <max_etu>` | 18     |
| EX-02 | Sélectionner le diplôme à éditer.                                                                                          | `SELECT DEGREE <name>`                  | 19     |
| EX-03 | Sélectionner l'année à éditer (1 à 3 pour Licence/BUT, 1 à 2 pour Master, 1 pour LicencePro).                              | `SELECT YEAR <year>`                    | 20     |
| EX-04 | Par défaut, le diplôme édité est le dernier créé ou sélectionné, et son année éditée est la première.                      | (implicite)                             | 21     |
| EX-05 | Modifier le nom et le nombre maximum d'étudiants d'un diplôme. Le type ne peut pas changer.                                | `EDIT DEGREE <old> <new> <max_etu>`     | 29     |

### 2.2 Gestion des UE

| ID    | Exigence                                                                   | Commande                                    | Source |
| ----- | -------------------------------------------------------------------------- | ------------------------------------------- | ------ |
| EX-06 | Créer une UE (ECTS, CM, TD, TP) dans le diplôme et l'année sélectionnés.   | `CREATE UE <name> <ects> <cm> <td> <tp>`    | 22     |
| EX-07 | Modifier une UE du diplôme et de l'année sélectionnés.                     | `EDIT UE <old> <new> <ects> <cm> <td> <tp>` | 30     |
| EX-08 | Supprimer une UE (demandé en p.14, mais commande non définie : voir Q-01). | à définir                                   | 14     |
| EX-09 | Mutualiser une UE existante dans une année d'un autre diplôme.             | `ASSIGN UE <name> <diplome> <year>`         | 30     |

### 2.3 Gestion des enseignants

| ID    | Exigence                                                                             | Commande                            | Source |
| ----- | ------------------------------------------------------------------------------------ | ----------------------------------- | ------ |
| EX-10 | Enregistrer un enseignant (nom, prénom).                                             | `CREATE TEACHER <name> <firstname>` | 22     |
| EX-11 | Affecter un enseignant à une UE pour un nombre d'heures (sans distinction CM/TD/TP). | `ASSIGN <ue> <teacher> <hours>`     | 12, 23 |

### 2.4 Calculs

| ID    | Exigence                                                                                           | Commande                  | Source |
| ----- | -------------------------------------------------------------------------------------------------- | ------------------------- | ------ |
| EX-12 | Calculer les heures d'un enseignant, d'une UE, d'un diplôme ou de toute l'OF.                      | `GET HOURS <name \| ALL>` | 24-25  |
| EX-13 | Calculer le coût d'une UE, d'un diplôme ou de toute l'OF.                                          | `GET COST <name \| ALL>`  | 13, 26 |
| EX-14 | Calculer la couverture d'une UE, d'un diplôme ou de toute l'OF, en pourcentage arrondi à l'entier. | `GET COVER <name \| ALL>` | 31-32  |

### 2.5 Affichage

| ID    | Exigence                                                                                                                                       | Commande                           | Source |
| ----- | ---------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------- | ------ |
| EX-15 | Afficher un diplôme sous forme d'arbre textuel : diplôme, années, UE (ECTS, CM, TD, TP), couverture et enseignants affectés avec leurs heures. | `DISPLAY GRAPH <diplome>`          | 28     |
| EX-16 | Tracer l'arbre d'un diplôme en image PNG via Graphviz (langage DOT).                                                                           | `TRACE GRAPH <diplome> <filename>` | 5, 27  |

### 2.6 Persistance et entrées/sorties

| ID    | Exigence                                                                                                                | Source |
| ----- | ----------------------------------------------------------------------------------------------------------------------- | ------ |
| EX-17 | Sauvegarder l'état du système et le recharger pour une utilisation en plusieurs fois (fichiers simples JSON ou SQLite). | 4, 15  |
| EX-18 | Lire des instructions sur l'entrée standard (boucle REPL) : `prototype`.                                                | 17     |
| EX-19 | Lire des instructions depuis un fichier texte : `prototype source.txt`.                                                 | 17     |
| EX-20 | Ignorer les lignes commençant par `--`.                                                                                 | 33     |
| EX-21 | Terminer le programme avec `EXIT`.                                                                                      | 23     |

## 3. Règles métier

| ID    | Règle                                                                                                                                                                                              | Source |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| RG-01 | Une année de diplôme doit totaliser exactement 60 ECTS.                                                                                                                                            | 9      |
| RG-02 | Une année contient au maximum 6 UE.                                                                                                                                                                | 9      |
| RG-03 | Pour une UE, CM + TD + TP ≤ 30h.                                                                                                                                                                   | 9      |
| RG-04 | Un enseignant ne peut pas dépasser 90h au total.                                                                                                                                                   | 10     |
| RG-05 | Le nom d'un diplôme, d'une UE ou d'un enseignant est son identifiant unique.                                                                                                                       | 15     |
| RG-06 | Un nom ne peut pas être un mot-clé réservé : `ALL`, `ASSIGN`, `COVER`, `COST`, `CREATE`, `DEGREE`, `DISPLAY`, `EDIT`, `EXIT`, `GET`, `GRAPH`, `SELECT`, `TEACHER`, `TOTAL`, `TRACE`, `UE`, `YEAR`. | 16     |
| RG-07 | Les types de diplôme autorisés sont LicencePro, Licence, BUT, Master (1, 3, 3, 2 années).                                                                                                          | 9, 18  |
| RG-08 | Un diplôme a un nombre maximum d'étudiants.                                                                                                                                                        | 10     |
| RG-09 | Une UE peut être mutualisée entre plusieurs diplômes.                                                                                                                                              | 10     |
| RG-10 | Les enseignements sont découpés en séances de 2h, et chaque séance nécessite un enseignant.                                                                                                        | 8      |
| RG-11 | Les promotions ne sont pas divisées en groupes (1 CM, 1 TD, 1 TP par UE).                                                                                                                          | 8      |
| RG-12 | Coût : 1h de TD ou de TP = 40 €, 1h de CM = 60 € (1,5 fois plus).                                                                                                                                  | 13     |
| RG-13 | Couverture d'une UE = heures affectées / (CM + TD + TP). Pour un diplôme ou l'OF, calculée sur les UE qu'ils contiennent.                                                                          | 31-32  |
| RG-14 | Les semestres et les parcours ne sont pas gérés.                                                                                                                                                   | 7, 10  |

## 4. Exigences non fonctionnelles

| ID    | Exigence                                                                                                                       | Source |
| ----- | ------------------------------------------------------------------------------------------------------------------------------ | ------ |
| NF-01 | Langage : Python. Tests : `unittest`.                                                                                          | 3, 35  |
| NF-02 | Interface en console (TUI), sans interface graphique.                                                                          | 4, 35  |
| NF-03 | Le programme gère les erreurs (commande inconnue, argument invalide, règle métier violée) sans planter, avec un message clair. | 4      |
| NF-04 | Le code respecte une norme (PEP 8, vérifiée avec flake8).                                                                      | 51     |
| NF-05 | Les données sont stockées en fichiers simples (JSON) ou SQLite.                                                                | 4      |
| NF-06 | Graphviz doit être installé sur la machine pour `TRACE GRAPH`.                                                                 | 5      |
| NF-07 | Le code source doit pouvoir être recompilé/interprété et testé à partir du zip.                                                | 51     |
| NF-08 | Méthode XP : 1 livraison, 4 itérations, tests écrits avec le code.                                                             | 3, 41  |

## 5. Questions ouvertes pour le client

À reporter dans `suivi_decisions.md` avec la date et la décision.

| ID   | Question                                                                                                                                                                                                        | Exigences concernées |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------- |
| Q-01 | Aucune commande de suppression n'existe alors que le sujet demande de supprimer des UE. Faut-il ajouter `DELETE UE <name>` ?                                                                                    | EX-08                |
| Q-02 | Comment distinguer `ASSIGN UE <name> <diplome> <year>` de `ASSIGN <ue> <teacher> <hours>` ? Une UE ou un enseignant nommé `UE` est interdit (mot-clé), donc le premier mot après `ASSIGN` suffit-il ?           | EX-09, EX-11, RG-06  |
| Q-03 | Les noms sont-ils uniques au sein de chaque catégorie, ou globalement ? `GET HOURS <name>` accepte un enseignant, une UE ou un diplôme, donc un doublon entre catégories crée une ambiguïté.                    | RG-05, EX-12         |
| Q-04 | Pour un enseignant, l'identifiant est-il le nom seul ? Deux enseignants de même nom et de prénoms différents sont interdits (p.15).                                                                             | EX-10, RG-05         |
| Q-05 | Une UE mutualisée est-elle comptée une seule fois ou une fois par diplôme dans `GET HOURS ALL`, `GET COST ALL` et `GET COVER ALL` ?                                                                             | EX-12, EX-13, EX-14  |
| Q-06 | Le contrôle des 60 ECTS est-il bloquant à chaque ajout d'UE, ou seulement vérifié à l'affichage ? (Impossible d'atteindre exactement 60 en créant une UE à la fois si le contrôle est bloquant à chaque ajout.) | RG-01                |
| Q-07 | Peut-on affecter plus d'heures à une UE que son total (ex. 35h sur une UE de 30h) ? Peut-on affecter plusieurs fois le même enseignant à la même UE ?                                                           | EX-11, RG-13         |
| Q-08 | Les heures CM, TD et TP doivent-elles être des multiples de 2 (séances de 2h) ?                                                                                                                                 | RG-10, EX-06         |
| Q-09 | Que fait `EDIT UE` sur une UE mutualisée : modification partout ? Que devient la sauvegarde d'un diplôme supprimé ou renommé ?                                                                                  | EX-07, EX-09         |
| Q-10 | Où est sauvegardé l'état : automatiquement après chaque commande, ou via une commande explicite (`SAVE`/`LOAD`) ? Le sujet ne définit aucune commande de sauvegarde.                                            | EX-17                |
| Q-11 | `GET COST`/`GET HOURS` d'un enseignant : donne-t-on seulement ses heures, ou aussi le coût associé ?                                                                                                            | EX-12, EX-13         |
| Q-12 | Que retourne `GET COVER` quand une UE a 0h au total ?                                                                                                                                                           | EX-14                |
| Q-13 | Le maximum d'étudiants (RG-08) est-il utilisé dans un calcul, ou simplement stocké ?                                                                                                                            | RG-08                |

## 6. Hypothèses provisoires (en attendant les réponses du client)

- H-01 : un nom est unique dans toute l'OF, toutes catégories confondues (Q-03).
- H-02 : une UE mutualisée est comptée une seule fois dans les totaux globaux (Q-05).
- H-03 : le contrôle des 60 ECTS est vérifié à l'affichage et par une commande de validation, et n'est pas bloquant à la création (Q-06).
- H-04 : l'état est sauvegardé automatiquement après chaque commande modifiante (Q-10).
- H-05 : on ajoute `DELETE UE <name>` en story de priorité basse (Q-01).

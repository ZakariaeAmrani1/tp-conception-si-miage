# Exigences

Projet : prototype de modélisation d'une offre de formation universitaire (OF).
Identifiants : **EX** = exigence fonctionnelle, **RG** = règle métier (future contrainte OCL), **NF** = exigence non fonctionnelle.

## 1. Types de diplômes

| Type       | Années | ECTS |
| ---------- | ------ | ---- |
| LicencePro | 1      | 60   |
| Licence    | 3      | 180  |
| BUT        | 3      | 180  |
| Master     | 2      | 120  |

## 2. Exigences fonctionnelles

| ID    | Exigence                                                                              | Commande                                           |
| ----- | ------------------------------------------------------------------------------------- | -------------------------------------------------- |
| EX-01 | Créer un diplôme (nom, type, nombre max d'étudiants). Années et ECTS déduits du type. | `CREATE DEGREE <nom> <type> <max>`                 |
| EX-02 | Sélectionner le diplôme à éditer.                                                     | `SELECT DEGREE <nom>`                              |
| EX-03 | Sélectionner l'année à éditer.                                                        | `SELECT YEAR <année>`                              |
| EX-04 | Par défaut : dernier diplôme créé ou sélectionné, année 1.                            | (implicite)                                        |
| EX-05 | Modifier le nom et le nombre max d'étudiants d'un diplôme (pas le type).              | `EDIT DEGREE <ancien> <nouveau> <max>`             |
| EX-06 | Créer une UE dans le diplôme et l'année sélectionnés.                                 | `CREATE UE <nom> <ects> <cm> <td> <tp>`            |
| EX-07 | Modifier une UE.                                                                      | `EDIT UE <ancien> <nouveau> <ects> <cm> <td> <tp>` |
| EX-08 | Supprimer une UE de l'année sélectionnée.                                             | `DELETE UE <nom>`                                  |
| EX-09 | Mutualiser une UE dans une année d'un autre diplôme.                                  | `ASSIGN UE <nom> <diplôme> <année>`                |
| EX-10 | Enregistrer un enseignant.                                                            | `CREATE TEACHER <nom> <prénom>`                    |
| EX-11 | Affecter un enseignant à une UE pour un nombre d'heures.                              | `ASSIGN <ue> <enseignant> <heures>`                |
| EX-12 | Calculer les heures d'un enseignant, d'une UE, d'un diplôme ou de toute l'OF.         | `GET HOURS <nom ou ALL>`                           |
| EX-13 | Calculer le coût d'une UE, d'un diplôme ou de toute l'OF.                             | `GET COST <nom ou ALL>`                            |
| EX-14 | Calculer la couverture (%) d'une UE, d'un diplôme ou de toute l'OF.                   | `GET COVER <nom ou ALL>`                           |
| EX-15 | Afficher un diplôme en arbre textuel.                                                 | `DISPLAY GRAPH <diplôme>`                          |
| EX-16 | Tracer l'arbre d'un diplôme en PNG avec Graphviz (DOT).                               | `TRACE GRAPH <diplôme> <fichier>`                  |
| EX-17 | Sauvegarder automatiquement les données et les recharger au démarrage.                | (automatique)                                      |
| EX-18 | Lire les commandes sur l'entrée standard (REPL).                                      | `python -m src.main`                               |
| EX-19 | Lire les commandes depuis un fichier texte.                                           | `python -m src.main source.txt`                    |
| EX-20 | Ignorer les lignes qui commencent par `--`.                                           |                                                    |
| EX-21 | Terminer le programme.                                                                | `EXIT`                                             |

## 3. Règles métier

| ID    | Règle                                                                                                                                                                       |
| ----- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RG-01 | Une année doit faire 60 ECTS. La création d'une UE est refusée si elle ferait dépasser 60. Une année en dessous de 60 est signalée par un avertissement à l'affichage.      |
| RG-02 | Une année contient au maximum 6 UE.                                                                                                                                         |
| RG-03 | Pour une UE, CM + TD + TP ≤ 30 h.                                                                                                                                           |
| RG-04 | Un enseignant ne peut pas dépasser 90 h au total.                                                                                                                           |
| RG-05 | Le nom d'un diplôme, d'une UE ou d'un enseignant est son identifiant. Il est unique dans toute l'OF, toutes catégories confondues.                                          |
| RG-06 | Un nom ne peut pas être un mot réservé : ALL, ASSIGN, COVER, COST, CREATE, DEGREE, DELETE, DISPLAY, EDIT, EXIT, GET, GRAPH, HOURS, SELECT, TEACHER, TOTAL, TRACE, UE, YEAR. |
| RG-07 | Les types de diplôme autorisés sont LicencePro, Licence, BUT et Master.                                                                                                     |
| RG-08 | Un diplôme a un nombre maximum d'étudiants. Il est seulement stocké et affiché.                                                                                             |
| RG-09 | Une UE peut être partagée entre plusieurs diplômes (c'est le même objet).                                                                                                   |
| RG-10 | Les heures CM, TD et TP sont des entiers pairs (séances de 2 h).                                                                                                            |
| RG-11 | Pas de groupes : 1 CM, 1 TD et 1 TP par UE. Pas de semestres ni de parcours.                                                                                                |
| RG-12 | Coût : 1 h de CM = 60 €, 1 h de TD ou de TP = 40 €. Le coût dépend du volume de l'UE, pas des heures affectées.                                                             |
| RG-13 | Couverture d'une UE = heures affectées / (CM + TD + TP).                                                                                                                    |
| RG-15 | Une UE a au moins 1 h au total (CM + TD + TP > 0).                                                                                                                          |
| RG-16 | Le nombre max d'étudiants est un entier strictement positif.                                                                                                                |
| RG-17 | Les heures affectées à une UE ne dépassent pas son volume total.                                                                                                            |
| RG-18 | Un nouvel `ASSIGN` du même enseignant sur la même UE s'ajoute aux heures existantes.                                                                                        |
| RG-19 | Dans un diplôme donné, les totaux comptent chaque UE normalement. Dans les totaux `ALL`, une UE partagée est comptée une seule fois.                                        |
| RG-20 | Les ECTS d'une UE sont un entier strictement positif.                                                                                                                       |
| RG-21 | La couverture est arrondie au demi supérieur (12,5 donne 13).                                                                                                               |
| RG-22 | Une UE ne peut être liée qu'une fois à un même diplôme.                                                                                                                     |
| RG-23 | Le diplôme et l'année sélectionnés sont sauvegardés. Avant d'écraser `offre.json`, l'ancienne version est copiée dans `offre.json.bak`.                                     |
| RG-24 | Les heures d'un `ASSIGN` sont un entier strictement positif.                                                                                                                |
| RG-25 | Un nom est un seul mot, avec majuscules/minuscules respectées (`Python` ≠ `python`). Les mots réservés sont reconnus sans tenir compte de la casse.                         |

## 4. Exigences non fonctionnelles

| ID    | Exigence                                                                                                                                                                                     |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| NF-01 | Langage Python, tests avec `unittest`.                                                                                                                                                       |
| NF-02 | Interface en console (TUI).                                                                                                                                                                  |
| NF-03 | Les erreurs ne font jamais planter le programme : message clair, sans trace d'exception.                                                                                                     |
| NF-04 | Code conforme à PEP 8 (vérifié avec flake8).                                                                                                                                                 |
| NF-05 | Données en fichier JSON (`data/offre.json`).                                                                                                                                                 |
| NF-06 | Graphviz doit être installé pour `TRACE GRAPH`.                                                                                                                                              |
| NF-07 | Le code doit pouvoir être relancé et testé depuis le zip.                                                                                                                                    |
| NF-08 | Méthode XP : 1 livraison, 4 itérations.                                                                                                                                                      |
| NF-09 | Sorties : `OK ...` après une modification, la valeur seule pour un `GET`, `ERREUR : ...` pour une erreur (avec le numéro de ligne en mode fichier). Après une erreur, le programme continue. |

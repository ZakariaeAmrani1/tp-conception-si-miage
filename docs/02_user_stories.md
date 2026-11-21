# User stories

Version 1.0, phase 1 (exploration). Client (MOA) : simulé. Équipe : 3 MOE.
Estimations en **jours idéaux** (1 personne, 1 journée sans interruption), tests et refactoring inclus.
Les priorités et la vélocité sont fixées dans `03_priorites_risques.md`.

## 1. Conventions communes

**Lancement** : `python -m src.main` (REPL) ou `python -m src.main source.txt` (fichier).

**Sorties** (NF-09, décision Q-20) :

- Commande modifiante réussie : une ligne commençant par `OK`.
- Commande `GET ...` : uniquement la valeur entière (heures, euros ou pourcentage).
- Erreur : `ERREUR : <message>`. L'état n'est pas modifié et rien n'est sauvegardé. En mode fichier : `ERREUR (ligne N) : <message>` et on continue à la ligne suivante.
- Le programme n'affiche jamais de trace d'exception.

**Nouvelles règles décidées par le MOA** (Q-14 à Q-20, voir `suivi_decisions.md`) :

| ID    | Règle                                                                                                                                     |
| ----- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| RG-20 | Les ECTS d'une UE sont un entier strictement positif.                                                                                     |
| RG-21 | Les pourcentages de couverture sont arrondis au demi supérieur (12,5 donne 13).                                                           |
| RG-22 | Une UE ne peut être liée qu'une seule fois à un même diplôme.                                                                             |
| RG-23 | Le diplôme et l'année sélectionnés sont sauvegardés. Quand `offre.json` est réécrit, l'ancienne version est copiée dans `offre.json.bak`. |
| RG-24 | Les heures d'un `ASSIGN` sont un entier strictement positif.                                                                              |
| RG-25 | Un nom est un seul mot (sans espace) et sensible à la casse. Les mots-clés sont reconnus sans tenir compte de la casse.                   |
| NF-09 | Format de sortie et d'erreur décrit ci-dessus.                                                                                            |

**Jeu de données de référence JD-1** (utilisé dans plusieurs stories) :

```
CREATE DEGREE LicenceInfo Licence 120
CREATE UE Python 15 10 10 10
CREATE UE Maths 15 16 6 6
CREATE UE C 15 14 8 8
CREATE UE Algo 15 16 8 6
CREATE TEACHER Dupont Marie
CREATE TEACHER Dunlap Paul
ASSIGN Python Dupont 10
ASSIGN Python Dunlap 5
```

Valeurs calculées pour JD-1 : Python 30h / 1400 €, Maths 28h / 1440 €, C 30h / 1480 €, Algo 30h / 1520 €, diplôme 118h / 5840 €.

---

## 2. Stories

### US-01 : Lire les commandes (REPL, fichier, commentaires, EXIT)

**En tant que** gestionnaire de l'offre, **je veux** saisir des commandes au clavier ou les lire depuis un fichier, **afin de** travailler en interactif ou en lot.
**Exigences** : EX-18, EX-19, EX-20, EX-21, NF-02 · **Estimation** : 1,5 j · **Risque** : faible

Critères d'acceptation :

- `python -m src.main` affiche un prompt `> ` et attend une commande. `EXIT` termine le programme (code retour 0).
- Fin de saisie (Ctrl+D / EOF) : le programme termine proprement, sans erreur.
- `python -m src.main source.txt` exécute les lignes dans l'ordre puis termine, même sans `EXIT`.
- Les lignes vides et les lignes commençant par `--` (même après des espaces) sont ignorées.
- Un fichier inexistant : `ERREUR : fichier introuvable` et code retour 1.
- Les lignes après un `EXIT` dans un fichier ne sont pas exécutées.

### US-02 : Valider les saisies et les noms

**En tant que** gestionnaire, **je veux** des messages d'erreur clairs, **afin de** corriger ma saisie sans faire planter le programme.
**Exigences** : NF-03, RG-05, RG-06, RG-18, RG-25 · **Estimation** : 1,5 j · **Risque** : moyen (touche toutes les commandes)

Critères d'acceptation :

- `BLABLA` : `ERREUR : commande inconnue`.
- `CREATE DEGREE X` (arguments manquants) : `ERREUR` avec rappel de la syntaxe.
- `CREATE DEGREE X Licence abc` (entier attendu) : `ERREUR : nombre entier attendu`.
- `CREATE TEACHER ALL Jean` : `ERREUR : nom réservé`. Idem pour `delete`, `Hours`, `ue` (insensible à la casse).
- Un nom déjà utilisé par n'importe quelle catégorie est refusé : si l'UE `Python` existe, `CREATE DEGREE Python Licence 40` est refusé.
- `Python` et `python` sont deux noms différents (sensibilité à la casse).
- Ces vérifications sont écrites comme un module commun, réutilisé par chaque commande. Les tests couvrent chaque commande au fur et à mesure des itérations.

### US-03 : Créer un diplôme

**En tant que** gestionnaire, **je veux** créer un diplôme d'un type donné, **afin de** démarrer une formation.
**Exigences** : EX-01, RG-07, RG-08, RG-16 · **Estimation** : 1 j · **Risque** : faible

Critères d'acceptation :

- `CREATE DEGREE LicenceInfo Licence 120` : `OK`. Le diplôme a 3 années et 180 ECTS.
- Déduction par type : LicencePro 1 an / 60 ECTS, Licence 3 / 180, BUT 3 / 180, Master 2 / 120.
- Type inconnu (`CREATE DEGREE X Doctorat 10`) : `ERREUR : type invalide`.
- Maximum d'étudiants `0`, négatif ou non entier : refusé.
- Nom déjà pris : refusé.
- Le diplôme créé devient le diplôme sélectionné, année 1.

### US-04 : Sélectionner un diplôme et une année

**En tant que** gestionnaire, **je veux** choisir le diplôme et l'année à éditer, **afin de** cibler mes ajouts d'UE.
**Exigences** : EX-02, EX-03, EX-04 · **Estimation** : 1 j · **Risque** : faible

Critères d'acceptation :

- `SELECT DEGREE LicenceInfo` : `OK`, l'année éditée repasse à 1.
- `SELECT DEGREE Inconnu` : `ERREUR : diplôme introuvable`.
- `SELECT YEAR 2` fonctionne sur une Licence ou un BUT (1 à 3), un Master (1 à 2), mais `SELECT YEAR 2` sur une LicencePro est refusé. `SELECT YEAR 4` sur une Licence est refusé. `SELECT YEAR 0` est refusé.
- `SELECT YEAR 1` sans diplôme sélectionné : `ERREUR : aucun diplôme sélectionné`.
- `CREATE UE ...` sans diplôme sélectionné : même erreur.

### US-05 : Créer une UE

**En tant que** gestionnaire, **je veux** ajouter une UE à l'année sélectionnée, **afin de** construire un diplôme.
**Exigences** : EX-06, RG-01, RG-02, RG-03, RG-10, RG-11, RG-15, RG-19, RG-20 · **Estimation** : 2 j · **Risque** : moyen (nombreuses règles à combiner)

Critères d'acceptation :

- `CREATE UE Python 15 10 10 10` : `OK`.
- Exactement 30h (`CREATE UE A 5 10 10 10`) : accepté. 32h (`CREATE UE B 5 12 10 10`) : refusé.
- Heures impaires (`CREATE UE C 5 3 2 2`) : refusé. Heures négatives : refusé.
- 0h au total (`CREATE UE D 5 0 0 0`) : refusé. Par exemple `CREATE UE E 5 0 0 2` est accepté.
- ECTS à 0 ou négatifs : refusé.
- Après 4 UE de 15 ECTS dans la même année (60/60), `CREATE UE F 5 2 0 0` est refusé (dépassement de 60). L'état reste inchangé.
- Après 6 UE de 5 ECTS, une 7e UE de 5 ECTS est refusée pour dépassement du nombre d'UE, même si le total ECTS reste sous 60.
- Une année à 60/60 ECTS exactement est acceptée, sans avertissement.
- Un nom d'UE déjà pris (UE, diplôme ou enseignant) est refusé.
- L'UE est ajoutée à l'année et au diplôme sélectionnés (vérifiable via US-13).

### US-06 : Enregistrer un enseignant

**En tant que** gestionnaire, **je veux** enregistrer des enseignants, **afin de** pouvoir les affecter.
**Exigences** : EX-10, RG-05 · **Estimation** : 0,5 j · **Risque** : faible

Critères d'acceptation :

- `CREATE TEACHER Dupont Marie` : `OK`.
- `CREATE TEACHER Dupont Paul` : refusé (le nom seul est l'identifiant).
- Nom réservé ou déjà utilisé par une UE ou un diplôme : refusé.
- Prénom manquant : `ERREUR` avec rappel de la syntaxe.

### US-07 : Affecter un enseignant à une UE

**En tant que** gestionnaire, **je veux** affecter un enseignant à une UE pour un nombre d'heures, **afin de** couvrir les enseignements.
**Exigences** : EX-11, RG-04, RG-17, RG-24 · **Estimation** : 1,5 j · **Risque** : faible

Critères d'acceptation :

- Avec JD-1 : `ASSIGN Python Dupont 10` : `OK`.
- UE inconnue ou enseignant inconnu : `ERREUR`.
- Heures `0`, négatives ou non entières : refusé.
- Un second `ASSIGN Python Dupont 5` s'ajoute : Dupont a 15h sur Python.
- Total des affectations d'une UE dépassant son volume (31h sur une UE de 30h) : refusé.
- Enseignant à exactement 90h (par exemple 3 UE de 30h) : accepté. À 91h : refusé.
- `ASSIGN UE ...` est traité par US-16 (le parser dirige selon le 1er mot, voir Q-02).

### US-08 : Sauvegarder et charger automatiquement

**En tant que** gestionnaire, **je veux** retrouver mes données au prochain lancement, **afin de** travailler en plusieurs fois.
**Exigences** : EX-17, NF-05, RG-23 · **Estimation** : 1,5 j · **Risque** : moyen

Critères d'acceptation :

- Après toute commande modifiante réussie, `data/offre.json` existe et contient du JSON valide.
- Après relance, `DISPLAY GRAPH LicenceInfo` donne le même affichage et `GET HOURS ALL` la même valeur qu'avant.
- Le diplôme et l'année sélectionnés sont restaurés.
- Les commandes en lecture seule (`GET`, `DISPLAY`, `TRACE`) et les commandes en erreur ne réécrivent pas le fichier.
- Premier lancement sans fichier : offre vide, aucune erreur.
- L'écriture est atomique : écriture dans un fichier temporaire puis renommage.
- Quand `offre.json` est réécrit, l'ancienne version est copiée dans `offre.json.bak`.

### US-09 : Protéger un fichier de sauvegarde corrompu

**En tant que** gestionnaire, **je veux** que mes données ne soient pas détruites par un fichier illisible, **afin de** pouvoir les récupérer.
**Exigences** : EX-17, NF-03 · **Estimation** : 1 j · **Risque** : moyen

Critères d'acceptation :

- `offre.json` contenant du JSON invalide : au démarrage, `ERREUR : sauvegarde illisible` et le programme démarre avec une offre vide.
- JSON valide mais incohérent (une UE référence un enseignant inexistant, des heures négatives) : traité comme corrompu.
- Tant qu'aucune commande modifiante n'a réussi, le fichier d'origine n'est pas modifié.
- À la première commande modifiante, l'ancien fichier est copié dans `offre.json.bak` avant d'être écrasé.

### US-10 : Calculer les heures

**En tant que** gestionnaire, **je veux** connaître la volumétrie horaire, **afin de** mesurer la charge d'enseignement.
**Exigences** : EX-12, RG-04, RG-09, RG-13 · **Estimation** : 1,5 j · **Risque** : faible

Critères d'acceptation (avec JD-1) :

- `GET HOURS Python` : `30` (CM + TD + TP).
- `GET HOURS Dupont` : `10` (heures affectées à l'enseignant).
- `GET HOURS LicenceInfo` : `118` (somme des UE de toutes les années).
- `GET HOURS ALL` : `118`. Les UE mutualisées ne sont comptées qu'une fois (voir US-16).
- Nom inconnu : `ERREUR : nom introuvable`.
- Diplôme sans UE : `0`.

### US-11 : Calculer le coût

**En tant que** gestionnaire, **je veux** connaître le coût d'une formation, **afin de** vérifier qu'elle est soutenable.
**Exigences** : EX-13, RG-12 · **Estimation** : 1 j · **Risque** : faible

Critères d'acceptation (avec JD-1) :

- Coût d'une UE = 60 € × CM + 40 € × (TD + TP). `GET COST Python` : `1400`.
- `GET COST LicenceInfo` : `5840`. `GET COST ALL` : `5840`.
- Le coût dépend du volume de l'UE (CM + TD + TP), pas des heures affectées.
- `GET COST Dupont` : `ERREUR : le coût ne s'applique pas à un enseignant`.
- Nom inconnu : `ERREUR`.
- UE mutualisée : coût compté une seule fois dans `ALL` (voir US-16).

### US-12 : Calculer la couverture

**En tant que** gestionnaire, **je veux** savoir quelle part des enseignements est couverte par des enseignants, **afin de** repérer ce qui reste à affecter.
**Exigences** : EX-14, RG-13, RG-21 · **Estimation** : 1 j · **Risque** : faible (attention à l'arrondi)

Critères d'acceptation (avec JD-1) :

- `GET COVER Python` : `50` (15h sur 30h).
- UE sans enseignant : `0`. UE entièrement affectée : `100`.
- Arrondi au demi supérieur : UE de 8h (`CREATE UE Mini 5 4 2 2`) avec 1h affectée : 12,5 donne `13`. 10h affectées sur 30h donne `33`, 20h sur 30h donne `67`.
- `GET COVER LicenceInfo` : `13` (15 heures affectées sur 118 heures).
- `GET COVER ALL` : calculé sur les UE distinctes (une UE mutualisée compte une fois).
- `GET COVER Dupont` : `ERREUR` (pas de sens pour un enseignant).

### US-13 : Afficher un diplôme en texte

**En tant que** gestionnaire, **je veux** voir l'arbre d'un diplôme dans la console, **afin de** vérifier sa composition.
**Exigences** : EX-15, RG-01, RG-08, RG-09 · **Estimation** : 2 j · **Risque** : moyen

Critères d'acceptation (avec JD-1) : `DISPLAY GRAPH LicenceInfo` affiche, dans cet ordre :

```
LicenceInfo (Licence, 120 étudiants max)
  Année 1 (60/60 ECTS)
    Python (15 ECTS) 10 CM 10 TD 10 TP
      Enseignants (couverte à 50%) :
        Dupont (10h)
        Dunlap (5h)
    Maths (15 ECTS) 16 CM 6 TD 6 TP
      Enseignants (couverte à 0%) :
        (aucun)
    ...
  Année 2 (0/60 ECTS)
    ATTENTION : 0/60 ECTS
  Année 3 (0/60 ECTS)
    ATTENTION : 0/60 ECTS
```

- Le diplôme, les années (même vides), les UE (dans l'ordre de création), les ECTS, les heures et les enseignants avec leurs heures sont présents.
- Une année différente de 60 ECTS affiche un avertissement `ATTENTION : <total>/60 ECTS` (Q-06).
- Une UE mutualisée est suffixée `[mutualisée]` dans chaque diplôme qui la contient.
- Diplôme inconnu : `ERREUR`.
- Les tests vérifient la présence et l'ordre des lignes clés, pas les espaces exacts.

### US-14 : Modifier un diplôme

**En tant que** gestionnaire, **je veux** renommer un diplôme ou changer son effectif maximum, **afin de** corriger ou faire évoluer l'offre.
**Exigences** : EX-05, RG-05, RG-08, RG-16 · **Estimation** : 0,5 j · **Risque** : faible

Critères d'acceptation :

- `EDIT DEGREE LicInfo LicenceInfo 60` : `OK`. Les UE et les liens restent intacts.
- Le type ne change pas. `EDIT DEGREE LicenceInfo LicenceInfo 80` (même nom, nouvel effectif) est accepté.
- Nouveau nom déjà pris par un autre objet ou nom réservé : refusé.
- Effectif `0` ou négatif : refusé.
- Diplôme inconnu : `ERREUR`.
- Si le diplôme renommé est le diplôme sélectionné, la sélection suit le nouveau nom.

### US-15 : Modifier une UE

**En tant que** gestionnaire, **je veux** corriger une UE (nom, ECTS, heures), **afin de** faire évoluer le contenu.
**Exigences** : EX-07, RG-01, RG-03, RG-10, RG-15, RG-17, RG-20 · **Estimation** : 1,5 j · **Risque** : moyen

Critères d'acceptation :

- `EDIT UE Python ProgPython 15 10 10 10` : `OK`. L'UE est cherchée dans le diplôme et l'année sélectionnés.
- Les mêmes règles que `CREATE UE` s'appliquent : 30h max, heures paires, volume > 0, ECTS > 0.
- Le nouveau nom doit être libre, ou identique à l'ancien.
- Un volume inférieur aux heures déjà affectées est refusé (par exemple UE de 30h avec 15h affectées modifiée à 10h).
- Une modification des ECTS qui fait dépasser 60 dans l'une des années qui utilisent l'UE est refusée.
- Sur une UE mutualisée, la modification s'applique dans tous les diplômes (Q-09).
- Les affectations d'enseignants sont conservées.
- UE inconnue dans l'année sélectionnée : `ERREUR`.

### US-16 : Mutualiser une UE

**En tant que** gestionnaire, **je veux** réutiliser une UE dans un autre diplôme, **afin de** ne pas la dupliquer.
**Exigences** : EX-09, RG-09, RG-22, Q-02, Q-05 · **Estimation** : 2 j · **Risque** : moyen à élevé (touche le modèle, les calculs, l'affichage et la sauvegarde)

Critères d'acceptation (avec JD-1 puis `CREATE DEGREE MasterBio Master 40`) :

- `ASSIGN UE Python MasterBio 2` : `OK`.
- `GET HOURS MasterBio` : `30`. `GET COST MasterBio` : `1400`.
- `GET HOURS ALL` reste `118` et `GET COST ALL` reste `5840` (UE comptée une fois).
- `DISPLAY GRAPH MasterBio` montre `Python [mutualisée]` en année 2, avec les mêmes enseignants.
- Une affectation `ASSIGN Python Dupont 5` ultérieure est visible dans les deux diplômes (c'est le même objet).
- Refusé si : UE inconnue, diplôme inconnu, année invalide pour le type de diplôme, année à 6 UE, dépassement de 60 ECTS, UE déjà liée à ce diplôme (RG-22).
- `ASSIGN UE ...` avec un nom d'enseignant au lieu d'une UE : `ERREUR : UE introuvable`.

### US-17 : Supprimer une UE

**En tant que** gestionnaire, **je veux** retirer une UE d'une année, **afin de** corriger une erreur ou faire évoluer l'offre.
**Exigences** : EX-08, RG-06 · **Estimation** : 1,5 j · **Risque** : moyen

Critères d'acceptation :

- `DELETE UE Maths` : `OK`. L'UE est retirée de l'année sélectionnée. `GET HOURS LicenceInfo` baisse du volume de Maths.
- UE mutualisée : seul le lien avec l'année sélectionnée est supprimé. L'UE reste visible dans les autres diplômes.
- Si c'était son dernier lien : l'UE disparaît, ses affectations aussi, et les heures de l'enseignant baissent.
- UE inconnue dans l'année sélectionnée : `ERREUR`.
- Le nom de l'UE supprimée redevient disponible.
- `DELETE` est un mot réservé (RG-06).

### US-18 : Tracer l'arbre en image

**En tant que** gestionnaire, **je veux** exporter l'arbre d'un diplôme en PNG, **afin de** l'inclure dans un document.
**Exigences** : EX-16, NF-06 · **Estimation** : 2 j · **Risque** : moyen (dépendance externe à Graphviz)

Critères d'acceptation :

- `TRACE GRAPH LicenceInfo output.png` : `OK`. Le fichier est créé et commence par la signature PNG.
- Le graphe DOT contient les nœuds diplôme, années, UE (avec ECTS et heures) et enseignants (avec heures), reliés en arbre.
- Une UE mutualisée apparaît dans le graphe de chaque diplôme concerné.
- Graphviz absent : `ERREUR : Graphviz (dot) introuvable`, sans plantage.
- Chemin non inscriptible : `ERREUR`.
- Diplôme inconnu : `ERREUR`.
- Tests : la génération du texte DOT est testée seule. L'appel à `dot` est testé avec un mock, plus un test d'intégration ignoré si `dot` est absent.
- Appel à `dot` par `subprocess` sans `shell=True`, pour éviter l'injection par le nom de fichier.

---

## 3. Récapitulatif des estimations

| ID    | Titre                        | Exigences           | Estimation (j) | Risque        |
| ----- | ---------------------------- | ------------------- | -------------- | ------------- |
| US-01 | Lire les commandes           | EX-18 à EX-21       | 1,5            | Faible        |
| US-02 | Valider saisies et noms      | NF-03, RG-05, RG-06 | 1,5            | Moyen         |
| US-03 | Créer un diplôme             | EX-01               | 1              | Faible        |
| US-04 | Sélectionner diplôme / année | EX-02 à EX-04       | 1              | Faible        |
| US-05 | Créer une UE                 | EX-06               | 2              | Moyen         |
| US-06 | Enregistrer un enseignant    | EX-10               | 0,5            | Faible        |
| US-07 | Affecter un enseignant       | EX-11               | 1,5            | Faible        |
| US-08 | Sauvegarde / chargement auto | EX-17               | 1,5            | Moyen         |
| US-09 | Fichier corrompu             | EX-17, NF-03        | 1              | Moyen         |
| US-10 | Calculer les heures          | EX-12               | 1,5            | Faible        |
| US-11 | Calculer le coût             | EX-13               | 1              | Faible        |
| US-12 | Calculer la couverture       | EX-14               | 1              | Faible        |
| US-13 | Afficher en texte            | EX-15               | 2              | Moyen         |
| US-14 | Modifier un diplôme          | EX-05               | 0,5            | Faible        |
| US-15 | Modifier une UE              | EX-07               | 1,5            | Moyen         |
| US-16 | Mutualiser une UE            | EX-09               | 2              | Moyen à élevé |
| US-17 | Supprimer une UE             | EX-08               | 1,5            | Moyen         |
| US-18 | Tracer en PNG                | EX-16               | 2              | Moyen         |
|       | **Total**                    |                     | **24,5**       |               |

## 4. Couverture des exigences

Chaque exigence fonctionnelle est portée par au moins une story : EX-01 (US-03), EX-02 à EX-04 (US-04), EX-05 (US-14), EX-06 (US-05), EX-07 (US-15), EX-08 (US-17), EX-09 (US-16), EX-10 (US-06), EX-11 (US-07), EX-12 (US-10), EX-13 (US-11), EX-14 (US-12), EX-15 (US-13), EX-16 (US-18), EX-17 (US-08, US-09), EX-18 à EX-21 (US-01).

## 5. Découpages réalisés (étape 1.5)

| Scénario initial            | Découpé en                                                 | Raison                                             |
| --------------------------- | ---------------------------------------------------------- | -------------------------------------------------- |
| Lire et gérer les commandes | US-01 (lecture) + US-02 (validation)                       | Livrables et risques différents                    |
| `ASSIGN`                    | US-07 (enseignant) + US-16 (mutualisation)                 | Deux comportements distincts (Q-02)                |
| Persistance                 | US-08 (sauvegarde / chargement) + US-09 (fichier corrompu) | Le cas nominal peut être livré avant la protection |
| Calculs                     | US-10, US-11, US-12                                        | Une commande par story, testables séparément       |
| `EDIT`                      | US-14 (diplôme) + US-15 (UE)                               | Complexités très différentes                       |

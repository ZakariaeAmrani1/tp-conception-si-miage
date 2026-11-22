# User stories

Estimations en **jours idéaux** : le temps qu'il faudrait à une personne, sans interruption, tests inclus.
Valeur : V1 indispensable, V2 cœur du métier, V3 demandé par le sujet mais pas bloquant, V4 souhaitable.
Risque : sur la précision de l'estimation (faible, moyen, élevé).

## Convention de sortie

Voir NF-09. Exemple utilisé dans les critères : l'UE `Python` (15 ECTS, 10 CM, 10 TD, 10 TP), avec Dupont (10 h) et Dunlap (5 h) affectés.

## Stories

**US-01 Lire les commandes** · EX-18 à EX-21 · V1 · 1,5 j · faible

- `EXIT` termine le programme. Fin de saisie (Ctrl+D) : arrêt propre.
- Un fichier est exécuté ligne par ligne, même sans `EXIT`. Les lignes après `EXIT` sont ignorées.
- Les lignes vides et celles qui commencent par `--` sont ignorées.
- Fichier inexistant : `ERREUR : fichier introuvable`.

**US-02 Valider les saisies et les noms** · NF-03, RG-05, RG-06, RG-25 · V1 · 1,5 j · moyen

- Commande inconnue : `ERREUR : commande inconnue`.
- Argument manquant ou entier attendu : `ERREUR` avec rappel de la syntaxe.
- Nom réservé (`ALL`, `delete`…) ou déjà utilisé par un diplôme, une UE ou un enseignant : refusé.
- Le contrôle est un module commun réutilisé par toutes les commandes.

**US-03 Créer un diplôme** · EX-01, RG-07, RG-16 · V1 · 1 j · faible

- `CREATE DEGREE LicenceInfo Licence 120` : `OK`, 3 années, 180 ECTS.
- Type inconnu, effectif ≤ 0 ou nom déjà pris : refusé.
- Le diplôme créé devient le diplôme sélectionné, année 1.

**US-04 Sélectionner un diplôme et une année** · EX-02 à EX-04 · V1 · 1 j · faible

- `SELECT DEGREE` : `OK`, l'année repasse à 1. Diplôme inconnu : erreur.
- `SELECT YEAR` accepte 1 à 3 (Licence, BUT), 1 à 2 (Master), 1 (LicencePro). Les autres valeurs sont refusées.
- Sans diplôme sélectionné, `SELECT YEAR` et `CREATE UE` donnent `ERREUR : aucun diplôme sélectionné`.

**US-05 Créer une UE** · EX-06, RG-01 à RG-03, RG-10, RG-15, RG-20 · V1 · 2 j · moyen

- `CREATE UE Python 15 10 10 10` : `OK`.
- 30 h exactement : accepté. 32 h : refusé. Heures impaires ou négatives : refusé. 0 h au total : refusé.
- ECTS ≤ 0 : refusé. Dépassement de 60 ECTS dans l'année : refusé. 7e UE dans l'année : refusée.
- Une année à exactement 60 ECTS est acceptée.

**US-06 Enregistrer un enseignant** · EX-10 · V2 · 0,5 j · faible

- `CREATE TEACHER Dupont Marie` : `OK`.
- `CREATE TEACHER Dupont Paul` : refusé (le nom seul est l'identifiant).
- Nom réservé ou déjà utilisé : refusé.

**US-07 Affecter un enseignant à une UE** · EX-11, RG-04, RG-17, RG-18, RG-24 · V2 · 1,5 j · faible

- `ASSIGN Python Dupont 10` : `OK`.
- UE ou enseignant inconnu, heures ≤ 0 : refusé.
- Un second `ASSIGN Python Dupont 5` s'ajoute (15 h).
- Dépassement du volume de l'UE ou de 90 h pour l'enseignant : refusé. Exactement 90 h : accepté.
- Si le premier mot après `ASSIGN` est `UE`, la commande est traitée par US-16.

**US-08 Sauvegarder et charger automatiquement** · EX-17, NF-05, RG-23 · V2 · 1,5 j · moyen

- Après chaque commande modifiante réussie, `data/offre.json` contient du JSON valide.
- Après relance, les données et la sélection sont retrouvées.
- Les commandes de lecture (`GET`, `DISPLAY`, `TRACE`) et les erreurs ne réécrivent pas le fichier.
- Premier lancement sans fichier : offre vide, sans erreur.
- Écriture atomique (fichier temporaire puis renommage). L'ancienne version est copiée dans `offre.json.bak`.

**US-09 Protéger un fichier corrompu** · EX-17, NF-03 · V3 · 1 j · moyen

- JSON invalide ou incohérent : `ERREUR : sauvegarde illisible`, démarrage avec une offre vide.
- Le fichier d'origine n'est pas modifié tant qu'aucune commande modifiante n'a réussi.

**US-10 Calculer les heures** · EX-12 · V2 · 1,5 j · faible

- `GET HOURS Python` : `30`. `GET HOURS Dupont` : `10`.
- `GET HOURS <diplôme>` : somme des UE de toutes ses années. `GET HOURS ALL` : somme de toute l'OF.
- Nom inconnu : erreur. Diplôme sans UE : `0`.

**US-11 Calculer le coût** · EX-13, RG-12 · V2 · 1 j · faible

- Coût d'une UE = 60 × CM + 40 × (TD + TP). `GET COST Python` : `1400`.
- Diplôme et `ALL` : somme des UE.
- `GET COST Dupont` : `ERREUR : le coût ne s'applique pas à un enseignant`.

**US-12 Calculer la couverture** · EX-14, RG-13, RG-21 · V3 · 1 j · faible

- `GET COVER Python` : `50` (15 h sur 30 h). Sans enseignant : `0`. Entièrement affectée : `100`.
- Arrondi au demi supérieur : 1 h sur 8 h donne `13`. 10 h sur 30 h donne `33`.
- Valable pour une UE, un diplôme et `ALL`. Pour un enseignant : erreur.

**US-13 Afficher un diplôme en texte** · EX-15 · V2 · 2 j · moyen

- `DISPLAY GRAPH` affiche dans l'ordre : le diplôme (type, effectif max), les années (même vides), les UE (ECTS, CM, TD, TP), le taux de couverture et les enseignants avec leurs heures.
- Une année qui n'a pas 60 ECTS affiche `ATTENTION : <total>/60 ECTS`.
- Une UE partagée est marquée `[mutualisée]`.
- Les tests vérifient les lignes clés et leur ordre, pas les espaces.

**US-14 Modifier un diplôme** · EX-05 · V3 · 0,5 j · faible

- `EDIT DEGREE LicInfo LicenceInfo 60` : `OK`. Les UE et les liens restent.
- Nouveau nom déjà pris ou réservé, effectif ≤ 0, diplôme inconnu : refusé.

**US-15 Modifier une UE** · EX-07 · V3 · 1,5 j · moyen

- `EDIT UE Python ProgPython 15 10 10 10` : `OK` (UE cherchée dans le diplôme et l'année sélectionnés).
- Les règles de `CREATE UE` s'appliquent. Un volume plus petit que les heures déjà affectées est refusé.
- Sur une UE partagée, la modification vaut pour tous les diplômes.

**US-16 Mutualiser une UE** · EX-09, RG-09, RG-22 · V3 · 2 j · élevé

- `ASSIGN UE Python MasterBio 2` : `OK`. L'UE apparaît dans les deux diplômes, avec les mêmes enseignants.
- Refusé si : UE ou diplôme inconnu, année invalide, année déjà à 6 UE, dépassement de 60 ECTS, UE déjà liée à ce diplôme.
- Dans `GET ... ALL`, l'UE est comptée une seule fois.

**US-17 Supprimer une UE** · EX-08 · V4 · 1,5 j · moyen

- `DELETE UE Maths` : `OK`, l'UE est retirée de l'année sélectionnée.
- UE partagée : seul le lien avec cette année est supprimé. Si c'était le dernier lien, l'UE et ses affectations disparaissent.
- UE inconnue : erreur. Le nom redevient libre.

**US-18 Tracer l'arbre en PNG** · EX-16, NF-06 · V3 · 2 j · moyen

- `TRACE GRAPH LicenceInfo sortie.png` : `OK`, le fichier PNG est créé.
- Le graphe contient le diplôme, les années, les UE (ECTS, heures) et les enseignants (heures).
- Graphviz absent ou chemin non écrivable : `ERREUR`, sans plantage.
- Tests : le texte DOT est testé seul. L'appel à `dot` est testé avec un mock.

## Récapitulatif

| ID    | Titre                           | Valeur | Estimation (j) | Risque |
| ----- | ------------------------------- | ------ | -------------- | ------ |
| US-01 | Lire les commandes              | V1     | 1,5            | Faible |
| US-02 | Valider les saisies et les noms | V1     | 1,5            | Moyen  |
| US-03 | Créer un diplôme                | V1     | 1,0            | Faible |
| US-04 | Sélectionner diplôme / année    | V1     | 1,0            | Faible |
| US-05 | Créer une UE                    | V1     | 2,0            | Moyen  |
| US-06 | Enregistrer un enseignant       | V2     | 0,5            | Faible |
| US-07 | Affecter un enseignant          | V2     | 1,5            | Faible |
| US-08 | Sauvegarde / chargement         | V2     | 1,5            | Moyen  |
| US-10 | Calculer les heures             | V2     | 1,5            | Faible |
| US-11 | Calculer le coût                | V2     | 1,0            | Faible |
| US-13 | Afficher en texte               | V2     | 2,0            | Moyen  |
| US-12 | Calculer la couverture          | V3     | 1,0            | Faible |
| US-16 | Mutualiser une UE               | V3     | 2,0            | Élevé  |
| US-15 | Modifier une UE                 | V3     | 1,5            | Moyen  |
| US-14 | Modifier un diplôme             | V3     | 0,5            | Faible |
| US-18 | Tracer en PNG                   | V3     | 2,0            | Moyen  |
| US-09 | Fichier corrompu                | V3     | 1,0            | Moyen  |
| US-17 | Supprimer une UE                | V4     | 1,5            | Moyen  |
|       | **Total**                       |        | **24,5**       |        |

Les stories ont été découpées pour être testables seules : lecture (US-01) et validation (US-02), `ASSIGN` enseignant (US-07) et `ASSIGN UE` (US-16), sauvegarde (US-08) et fichier corrompu (US-09), `EDIT` diplôme (US-14) et UE (US-15), un calcul par story (US-10 à US-12).

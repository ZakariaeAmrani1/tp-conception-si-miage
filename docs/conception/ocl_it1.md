# Contraintes OCL de l'itération 1

Modèle : `classes_it1.dot` (Offre, Diplome, TypeDiplome). Les règles concernées sont RG-05, RG-06, RG-07, RG-08, RG-16 et RG-25.
Un commentaire OCL commence par `--`.

## Offre

```ocl
context Offre

    def: motsReserves : Set(String) =
        Set{'ALL', 'ASSIGN', 'COVER', 'COST', 'CREATE', 'DEGREE', 'DELETE', 'DISPLAY', 'EDIT',
            'EXIT', 'GET', 'GRAPH', 'HOURS', 'SELECT', 'TEACHER', 'TOTAL', 'TRACE', 'UE', 'YEAR'}


    inv nomsDiplomesUniques:
        self.diplomes->isUnique(d | d.nom)

    inv nomsTypesUniques:
        self.types->isUnique(t | t.nom)

    inv selectionDansOffre:
        not self.selection.oclIsUndefined() implies self.diplomes->includes(self.selection)

    inv anneeSelectionneeValide:
        not self.selection.oclIsUndefined() implies
            (self.anneeSelectionnee >= 1 and self.anneeSelectionnee <= self.selection.type.nbAnnees)
```

## Offre::creerDiplome

```ocl
context Offre::creerDiplome(nom : String, typeNom : String, nbEtudiantsMax : Integer)

    pre nomNonVide:
        nom.size() > 0

    pre nomUnSeulMot:
        nom.indexOf(' ') = 0

    pre nomNonReserve:
        not self.motsReserves->includes(nom.toUpper())

    pre nomLibre:
        self.diplomes->forAll(d | d.nom <> nom)

    pre typeExiste:
        self.types->exists(t | t.nom = typeNom)

    pre effectifPositif:
        nbEtudiantsMax > 0

    post unDiplomeDePlus:
        self.diplomes->size() = self.diplomes@pre->size() + 1

    post diplomeCree:
        let nouveau : Diplome = self.diplomes->any(x | x.nom = nom) in
            nouveau.oclIsNew()
            and nouveau.nbEtudiantsMax = nbEtudiantsMax
            and nouveau.type.nom = typeNom
            and self.selection = nouveau
            and self.anneeSelectionnee = 1

    post anciensConserves:
        self.diplomes@pre->forAll(x | self.diplomes->includes(x))
```

## Offre::selectionnerDiplome

```ocl
context Offre::selectionnerDiplome(nom : String)

    pre diplomeExiste:
        self.diplomes->exists(d | d.nom = nom)

    post diplomeSelectionne:
        let d : Diplome = self.diplomes->any(x | x.nom = nom) in
            self.selection = d and self.anneeSelectionnee = 1

    post diplomesInchanges:
        self.diplomes = self.diplomes@pre
```

## Offre::selectionnerAnnee

```ocl
context Offre::selectionnerAnnee(annee : Integer)

    pre diplomeSelectionne:
        not self.selection.oclIsUndefined()

    pre anneePositive:
        annee > 0

    pre anneeValide:
        annee <= self.selection.type.nbAnnees

    post anneeSelectionnee:
        self.anneeSelectionnee = annee

    post diplomeInchange:
        self.selection = self.selection@pre
```

## Offre::trouverDiplome

```ocl
context Offre::trouverDiplome(nom : String) : Diplome

    pre diplomeExiste:
        self.diplomes->exists(d | d.nom = nom)

    body:
        self.diplomes->any(d | d.nom = nom)
```

## Diplome

```ocl
context Diplome

    inv nbEtudiantsMaxPositif:
        self.nbEtudiantsMax > 0

context Diplome::nbAnnees : Integer

    derive:
        self.type.nbAnnees

context Diplome::nbEcts : Integer

    derive:
        self.nbAnnees * 60
```

## TypeDiplome

```ocl
context TypeDiplome

    inv nbAnneesPositif:
        self.nbAnnees > 0
```

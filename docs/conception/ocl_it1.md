package classes_it1

context Offre::creerDiplome(
nom : String,
typeNom : String,
nbEtudiantsMax : Integer
)

    pre nomLibre:
        self.diplomes->forAll(d | d.nom <> nom)

    pre typeExiste:
        self.types->exists(t | t.nom = typeNom)

    pre effectifPositif:
        nbEtudiantsMax > 0

    post unDiplomeDePlus:
        self.diplomes->size() =
        self.diplomes@pre->size() + 1

    post diplomeCree:
        let nouveau : Diplome =
            self.diplomes->any(x | x.nom = nom)
        in
            nouveau.oclIsNew()
            and nouveau.nbEtudiantsMax = nbEtudiantsMax
            and nouveau.type.nom = typeNom
            and self.selection = nouveau
            and self.anneeSelectionnee = 1

    post anciensConserves:
        self.diplomes@pre->forAll(x |
            self.diplomes->includes(x)
        )

context Offre::selectionnerDiplome(nom : String)

    pre diplomeExiste:
        self.diplomes->exists(d | d.nom = nom)

    post unDiplomeSelectionne:
        let diplome : Diplome =
            self.diplomes->any(x | x.nom = nom)
        in
            self.selection = diplome
            and self.anneeSelectionnee = 1

context Offre::selectionnerAnnee(annee : Integer)

    pre diplomeSelectionne:
        not self.selection.oclIsUndefined()

    pre anneePositive:
        annee > 0

    pre anneeValide:
        annee <= self.selection.type.nbAnnees

    post uneAnneeSelectionnee:
        self.anneeSelectionnee = annee

context Offre::trouverDiplome(nom : String) : Diplome

    pre diplomeExiste:
        self.diplomes->exists(d | d.nom = nom)

    body:
        self.diplomes->any(d | d.nom = nom)

context Offre

    inv nomsDiplomesUniques:
        self.diplomes->isUnique(d | d.nom)

context Diplome

    inv nbEtudiantsMaxPositifNonNull:
        self.nbEtudiantsMax > 0

context Diplome:: nbAnnees: Integer

    derive:
        self.type.nbAnnees

context Diplome:: nbEcts: Integer

    derive:
        self.nbAnnees * 60

Context TypeDiplome

    inv nbAnneesPositifNonNull:
        self.nbAnnees > 0

endpackage

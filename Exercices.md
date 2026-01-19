

```sql

CREATE TABLE Département
    (
        codeDpt INT PRIMARY KEY,
        nom VARCHAR(255),
        préfecture VARCHAR(255),
        population INT,
        codeRégion INT,
        idPrésident INT,
        FOREIGN KEY (codeRégion) REFERENCES Région (codeRégion),
        FOREIGN KEY (idPrésident) REFERENCES Personne (idPersonne)
    )

CREATE TABLE Voisins
    (
        codeDpt1 INT NOT NULL ,
        codeDpt2 INT NOT NULL,
        PRIMARY KEY (codeDpt1, codeDpt2),
        FOREIGN KEY (codeDpt1) REFERENCES Département (codeDpt),
        FOREIGN KEY (codeDpt2) REFERENCES Département (codeDpt)
    )


-- Qui préside (prénom, nom) la région contenant le département “Cantal”?
SELECT Personne.prénom, Personne.nom
FROM Région
JOIN Personne ON Région.idPrésident = Personne.idPersonne
WHERE Région.codeRégion = (SELECT codeRégion FROM Département WHERE nom = 'CANTAL')

-- Quelles villes sont à la fois préfecture de région et d’un département de plus de 100000 habitants (donner le nom de la ville, du département, et l’intitulé de la région).

SELECT DISTINCT r.préfecture, d.nom, r.intitulé
FROM Région r
JOIN Département d ON r.codeRégion = d.codeRégion
WHERE r.préfecture = d.préfecture
  AND d.population > 100000;

-- Quels sont les départements voisins du Cantal ? (Aide: et si vous utilisiez la vue V3 définie précédemment?)

SELECT DISTINCT d1.nom
FROM Département d1
JOIN Voisins v ON d1.codeDpt = v.codeDpt1 OR d1.codeDpt = v.codeDpt2
JOIN Département d2 ON (v.codeDpt1 = d2.codeDpt OR v.codeDpt2 = d2.codeDpt)
WHERE d2.nom = 'CANTAL'
  AND d1.nom <> 'CANTAL';

-- Quels départements d’une même région sont voisins l’un de l’autre? Donnez l’intitulé de la région et les noms des deux départements. Attention à bien prendre en compte la contrainte sur les clés dans Voisins

SELECT nom
FROM Voisins




-- Quelles régions n’ont pas de département?

SELECT Région.intitulé
FROM Région as R
WHERE NOT EXISTS (
    SELECT * FROM Départment as D
    WHERE D.codeRégion = R.CodeRégion
)


-- Quelles personnes ne président ni région ni département?

SELECT P.nom, P.prénom
FROM Personnes as P
WHERE 
    NOT EXISTS (
        SELECT * FROM Région WHERE Région.idPrésident = P.idPersonne
    )
    OR NOT EXISTS (
        SELECT * FROM Département WHERE Département.idPrésident = P.idPersonne 
    )


-- Donner, pour chaque région, le nombre de département et sa population totale (obtenue par cumul de celle des départements)

-- Dans la requête précédente, que se passe-t-il si une région n’a pas de département? Comment réussir à afficher le nom de la région avec la valeur 0 pour le nombre de départements dans ce cas?

```

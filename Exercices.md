## Exercice Structures adminstratives

On met en place un système d’information sur les structures administratives françaises: régions, départements, préfectures. Voici le schéma de la base sur laquelle nous allons travailler. Les clés primaires sont en gras, les clés étrangères ne sont pas indiquées.

- Personne(idPersonne, nom, prénom)
- Région(codeRégion, intitulé, préfecture, idPrésident)
- Département (codeDpt, nom, préfecture, population, codeRégion, idPrésident)
- Voisins (codeDpt1, codeDpt2)

Dans les tables Région et Département, préfecture désigne la ville siège de la préfecture de région ou de département. Strasbourg est à la fois préfecture de la région Grand Est et du département du Bas-Rhin. Dans la même région, Colmar est préfecture du département du Haut-Rhin. Régions et départements sont présidés par une personne. Deux départements sont voisins s’ils ont une frontière commune.

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

select  d2.nom as voisinCantal
from   Département as d1, Département as d2, V3 as v
where d1.nom='Cantal'
/* On sait que dans V3, on trouve chaque voisinage
   représenté dans les deux sens */
and v.codeDpt1=d1.codeDpt
and v.codeDpt2=d2.codeDpt

select d2.nom as voisinCantal
from  Département as d1, Département as d2, Voisins as v
where d1.nom='Cantal'
and (
        /* Cas d'un voisin de code supérieur au Cantal */
        (d1.codeDpt < d2.codeDpt and v.codeDpt1 = d1.codeDpt1 and v.codeDpt2=d2.codeDpt)
    or
        /* Cas d'un voisin de code inférieur au Cantal */
        (d1.codeDpt > d2.codeDpt and v.codeDpt1 = d1.codeDpt2 and v.codeDpt2=d1.codeDpt)
        )

-- Quels départements d’une même région sont voisins l’un de l’autre? Donnez l’intitulé de la région et les noms des deux départements. Attention à bien prendre en compte la contrainte sur les clés dans Voisins
select r.intitulé, d1.nom as nomDpt1, d2.nom as nomDpt2
from  Région as r, Département as d1, Département as d2, Voisins as v
where r.codeRégion = d1.codeRégion
and r.codeRégion = d2.codeRégion
/* Pour prendre la relation 'voisins dans le bon sens' */
and d1.codeDpt < d2.codeDpt
and v.codeDpt1=d1.codeDpt
and v.codeDpt2=d2.codeDpt

-- Quelles régions n’ont pas de département?
SELECT Région.intitulé
FROM Région as R
WHERE NOT EXISTS (
    SELECT * FROM Départment as D
    WHERE D.codeRégion = R.CodeRégion
)


-- Quelles personnes ne président ni région ni département?
select *
from  Personne as p
where not exists (
        select * from Département as d
        where d.idPrésident = p.idPrésident)
and not exists (
        select * from Région as d
        where r.idPrésident = p.idPrésident)


-- Donner, pour chaque région, le nombre de département et sa population totale (obtenue par cumul de celle des départements)
select r.intitulé, count(d.codeDpt), sum(population) as population
from  Région as r, Département as d
where r.codeRégion = d1.codeRégion
group by r.codeRégion, r.intitulé


-- Dans la requête précédente, que se passe-t-il si une région n’a pas de département? Comment réussir à afficher le nom de la région avec la valeur 0 pour le nombre de départements dans ce cas?

```

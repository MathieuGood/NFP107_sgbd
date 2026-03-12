# NFP 107 : Systèmes de Gestion des Bases de Données

Questions courantes :

- Commandes de création de table
- Donner schéma entité/association
- En forme normale ?

- 3 forme normales
- Algorithme de normalisation
- Notion de réification
- Notion d'entité faible
- Types d'association (un à un, un à plusieurs, plusieurs à plusieurs)
- Principes de fonctionnement d'un curseur : immuable (sa valeur ne change jamais)

## Le modèle relationnel

### Relation

Une relation est un ensemble de nuplets (ou tuples) qui respectent un schéma donné.
Notion mathématique : une relation est un sous-ensemble du produit cartésien des domaines des attributs.

Une relation se définit par :

- nom de la relation
- nom d'attribut (nom distinct pour chaque dimension)
- domaine de valeur de chaque attribut
  Exemple : Départements (NumDépt: entier, NomDépt: chaîne de caractères)

### nuplet

Un élement d'une relation de dimension n est un n-uplet (ou tuple) de n valeurs. Exemple : (123, "Dupont", "Jean", 1980)

Dans la représentation par table, un nuplet est une ligne de la table.
On assimile nuplet et ligne, mais attention, ce n'est pas n'importe quelle ligne, c'est une ligne qui respecte le schéma de la relation.

### Entité faible

Une entité faible est une entité qui ne peut pas être identifiée uniquement par ses propres attributs et qui dépend d’une autre entité (dite forte) pour son identification.
👉 Elle n’a pas de clé primaire complète toute seule.

🔍 Comment reconnaître une entité faible à l’examen (ALGORITHME)

1. A-t-elle une clé propre ?
   ❌ non → possible entité faible
   ✅ oui → entité forte

2. Son identification dépend-elle d’une autre entité ?
   ❌ non → entité forte
   ✅ oui → continuer

3. La clé contient-elle une clé étrangère ?
   ❌ non → pas une entité faible
   ✅ oui → ENTITÉ FAIBLE

### Formes normales

#### 1FN — Première forme normale

Une relation est en **1FN** si tous les attributs contiennent des **valeurs atomiques** (pas de liste, pas de valeurs multiples dans une case).

#### 2FN — Deuxième forme normale

(la relation est déjà en 1FN)  
Une relation est en **2FN** s’il n’existe **aucune dépendance partielle** d’un attribut non-clé à **une partie seulement d’une clé composée**.

#### 3FN — Troisième forme normale

(la relation est déjà en 2FN)  
Une relation est en **3FN** s’il n’existe **aucune dépendance transitive** entre attributs non-clés (tout attribut non-clé dépend directement de la clé).
❌ Pas en 3FN : Employé(idEmp, idDept, nomDept) 👉 nomDept dépend de idDept, pas de idEmp
✅ En 3FN (décomposition) : Employé(idEmp, idDept) et Département(idDept, nomDept)
Toutes les dépendances fonctionnelles doivent avoir pour origine une clé.
Exemple : Dans la relation Manuscrit, la dépendance fonctionnelle NumManuscrit -> Titre est acceptable car NumManuscrit est une clé.
Cependant, si on avait une dépendance fonctionnelle Titre -> AnnéePubli, cela ne serait pas acceptable car Titre n'est pas une clé.

### Notions à retenir

| Terme du modèle    | Terme de la représentation par table |
| ------------------ | ------------------------------------ |
| relation           | table                                |
| nuplet             | ligne                                |
| nom d'attribut     | nom de colonne                       |
| valeur d'attribut  | valeur de cellule                    |
| domaine d'attribut | type de donnée de la colonne         |

## Qualité d'un modèle relationnel

### Dépendance fonctionnelle

Il y a une dépendance fonctionnelle notée A -> B entre deux attributs A et B d'une relation R quand la connaissance de la valeur de A permet de déterminer de façon unique la valeur de B.
Exemple

- Schéma
    - Manuscrit(NumManuscrit, Titre, NumAuteur AnnéePubli)
    - Auteur(NumAuteur, NomAuteur, PrénomAuteur)
- Dépendances fonctionnelles
    - NumManuscrit -> Titre, NumAuteur, AnnéePubli
    - NumAuteur -> NomAuteur, PrénomAuteur
- Explication : La connaissance du numéro du manuscrit permet de déterminer de façon unique le titre
- Dépendance directe dans cet exemple : NumManuscrit -> Titre
- Dépendance transitive dans cet exemple : NumManuscrit -> NumAuteur -> NomAuteur (à partir de NumManuscrit, on peut déterminer NomAuteur par transitivité)

### Clé

#### Clé primaire

Une clé est un ensemble minimal d'attributs qui permet d'identifier de façon unique un nuplet dans une relation.
Exemple : Dans la relation Manuscrit, NumManuscrit est une clé.
Il peut y avoir plusieurs clés dans une relation (clés candidates) mais une seule est choisie comme clé primaire.

#### Clé étrangère

Une clé étrangère est un attribut (ou un ensemble d'attributs) dans une relation qui fait référence à la clé primaire d'une autre relation.
Exemple : Dans la relation Manuscrit, NumAuteur est une clé étrangère qui fait référence à la clé primaire NumAuteur de la relation Auteur.

### Notions à retenir

- Toute relation a une clé
- Tous les attributs dépendent directement de la clé
- On peut lier les nuplets les uns aux autres via des clés étrangères
- Le système évite des anomalies en garantissant la contrainte d'unicité (clé) et la contrainte d'intégrité référentielle (clé étrangère)

## Notions de logique formelle

### Calcul propositionnel

Proposition : énoncé qui peut être vrai ou faux.
Formule : expression basée sur de spropositions et leur combinaison par des connecteurs logiques (ET, OU, NON, IMPLIQUE, EQUIVALENT).

- La connexion ET (∧) : vraie si les deux propositions sont vraies.
- La disjonction OU (∨) : vraie si au moins une des propositions est vraie.
- La négation NON (¬) : inverse la valeur de vérité d'une proposition.

| p     | q     | p ∧ q | p ∨ q | ¬p    |
| ----- | ----- | ----- | ----- | ----- |
| true  | true  | true  | true  | false |
| true  | false | false | true  | false |
| false | true  | false | true  | true  |
| false | false | false | false | true  |

### Prédicats

Le prédicat est une fonction qui associe à chaque élément d'un ensemble une valeur de vérité (vrai ou faux).
Le prédicat Compose(x, y) permet de construire des énoncés de la forme

- Comopose ("Mozart", "Symphonie n°40") est vrai
- Compose ("Beethoven", "Symphonie n°40") est faux

### Nuplets ouverts et fermés

Un nuplet énoncé avec des constantes est un nuplet fermé.
Exemple : ("123", "Dupont", "Jean", 1980)

Un nuplet énoncé avec des variables est un nuplet ouvert.
Exemple : (x, "Dupont", "Jean", 1980)

---

### Réification d'une relation

👉 Réifier une relation, c’est transformer une relation (une association) en une table à part entière, avec sa propre identité. Réifier une relation, c’est la transformer en entité avec une clé propre. Une relation réifiée n’est plus identifiée uniquement par les clés des entités qu’elle relie

Autrement dit :
Au lieu d’avoir juste une table de liaison, on crée une nouvelle entité (une vraie table) pour représenter la relation

On réifie une relation quand :

- la relation a ses propres informations
- ou quand elle doit être manipulée comme un objet à part entière

🎬 Exemple très classique (films / acteurs)
❌ Sans réification (relation simple) : Jouer (idFilm, idActeur)
👉 Clé primaire : (idFilm, idActeur)

✅ Avec réification

On transforme la relation Jouer en une entité Rôle : Role(idRole, idFilm, idActeur, nomRole, salaire)

👉 Maintenant :
idRole est la clé primaire
(idFilm, idActeur) n’est plus une clé

« Si j’avais choisi de réifier l’entité Rôle, en lui donnant un identifiant propre, que devient la clé (idFilm, idActeur)? » Ce n’est plus une clé !

📌 Parce que : La clé primaire devient idRole (idFilm, idActeur) peut être : un simple couple d’attributs éventuellement UNIQUE (si on le décide) mais ce n’est plus la clé primaire

On ne réifie PAS si :

- la relation est pure liaison
- aucune information métier propre
- aucun besoin d’identifier le lien

## Join

| JOIN             | Ce que ça fait                                           | Résultat conservé                 | Quand l’utiliser        |
| ---------------- | -------------------------------------------------------- | --------------------------------- | ----------------------- |
| **JOIN** (INNER) | Ne garde que les lignes qui correspondent des deux côtés | Correspondances uniquement        | Relation obligatoire    |
| **LEFT JOIN**    | Garde toutes les lignes de la table de gauche            | Tout à gauche + correspondances   | Relation optionnelle    |
| **FULL JOIN**    | Garde toutes les lignes des deux tables                  | Tout (avec NULL si pas de match)  | Cas très spécifiques    |
| **CROSS JOIN**   | Produit cartésien                                        | Toutes les combinaisons possibles | Avec sélection derrière |
| **NATURAL JOIN** | Jointure automatique sur les attributs de même nom       | Dépend des noms d’attributs       | Seulement si imposé     |

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
select r.intitulé, count(d.codeDpt), sum(population) as population
from  Région as r left join Département as d on r.codeRégion = d1.codeRégion
group by r.codeRégion
--Les régions sans département apparaissent dans le résultat de la jointure avec une valeur à null pour la population, et le count appliqué à null renvoie 0 (tandis que le sum appliqué à null renvoie null).
```

## Création de table

```sql
create table Voisins
        (idDpt1 int not null,
        idDpt2 int not null
        primary key (idDpt1, idDpt2),
        foreign key (idDpt1) references Département (idDpt),
        foreign key (idDpt2) references Département (idDpt)
        )
check (codeDpt1  < codeDpt2)
```

**Existe-t-il une association plusieurs-plusieurs dans ce schéma? Si oui vous paraîtrait-il judicieux de la réifier?**
Voisins provient d’une association plusieurs-plusieurs. La contrainte sur la clé implique qu’un département ne peut pas être deux fois le voisin d’un autre. C’est une contrainte tout à fait raisonnable, voire indispensable. La réification est donc inutile.

Notons D le département, P la personne, ND le nom du département et NP le nom de la personne. Le schéma nous donne les dépendances fonctionnelles suivantes: D -> P, ND et P -> NP.
On ajoute la dépendance P -> D. Comment l’interpréter et que devient le schéma relationnel?
La dépendandce additionnelle indique qu’une personne ne peut diriger qu’un seul département. L’identifiant d’une personne pourrait donc servir à identifier un département. Ce ne serait cependant pas un très bon choix car la personne dirigeant un département est amenée à changer régulièrement. En pratique, mieux vont donc déclarer dans le schéma une contrainte d’unicité sur l’idenfifiant idPrésident dans la table Département (c’est également une clé étrangère).

Dans toute la suite de l’examen, on travaille sur le schéma donné dans l’énoncé, et en supposant que la contrainte sur l’ordre des codes de département est respectée dans la table Voisins (donc, codeDpt1 < codeDpt2).

Algèbre et vues (4 pts)
On définit algébriquement les relations suivantes:

Répondez aux questions suivantes:

Quel est le résultat de
?

Comment caractériseriez-vous le contenu de
?

Donnez la commande SQL de création de la vue
.

Soit le département de code
. Quelle requête algébrique sur
donne les codes de tous ses voisins (expliquer).

Correction

est donc un synonyme de la table Voisins, et
a le même contenu que
mais l’ordre des clés est inversé. On ne trouve dans
que les paires de département (d1, d2) tels que d1 > d2. D’où la réponse aux questions:

est vide

contient toutes les paires de départements voisins (d1, d2) et (d2, d1). Autrement dit, chaque relation de voisinage entre deux départements est représentée dans les deux sens.

On peut donc créer la vue suivante qui pourra nous simplifier les requêtes SQL par la suite.

create view V3 as
select \* from Voisins
union
select codeDpt2 as codeDpt1, codeDpt1 as codeDpt2
from Voisins
Avec
, on n’a plus besoin de se soucier de l’ordre des codes de département. On peut donc écrire soit:

soit

qui donneront le même résultat.

SQL (7 pts)
Exprimez en SQL les requêtes suivantes:

Qui préside (prénom, nom) la région contenant le département “Cantal”?

Quelles villes sont à la fois préfecture de région et d’un département de plus de 100000 habitants (donner le nom de la ville, du département, et l’intitulé de la région).

Quels sont les départements voisins du Cantal ? (Aide: et si vous utilisiez la vue V3 définie précédemment?)

Quels départements d’une même région sont voisins l’un de l’autre? Donnez l’intitulé de la région et les noms des deux départements. Attention à bien prendre en compte la contrainte sur les clés dans Voisins

Quelles régions n’ont pas de département?

Quelles personnes ne président ni région ni département?

Donner, pour chaque région, le nombre de département et sa population totale (obtenue par cumul de celle des départements)

Dans la requête précédente, que se passe-t-il si une région n’a pas de département? Comment réussir à afficher le nom de la région avec la valeur 0 pour le nombre de départements dans ce cas?

Correction

select prénom, nom
from Personne as p, Région as r, Département as d
where d.nom='Cantal'
and d.codeRégion = r.codeRégion
and r.idPrésident = p.idPersonne
select r.préfecture as ville, r.intitulé, d.noms
from Région as r, Département as d
where r.préfecture = d.préfecture
and d.population > 10000
Il est plus simple d’utiliser la vue V3.

select d2.nom as voisinCantal
from Département as d1, Département as d2, V3 as v
where d1.nom='Cantal'
/_ On sait que dans V3, on trouve chaque voisinage
représenté dans les deux sens _/
and v.codeDpt1=d1.codeDpt
and v.codeDpt2=d2.codeDpt
Sinon, solution SQL complète avec un “ou” logique. Plus compliqué…

select d2.nom as voisinCantal
from Département as d1, Département as d2, Voisins as v
where d1.nom='Cantal'
and (
/_ Cas d'un voisin de code supérieur au Cantal _/
(d1.codeDpt < d2.codeDpt and v.codeDpt1 = d1.codeDpt1 and v.codeDpt2=d2.codeDpt)
or
/_ Cas d'un voisin de code inférieur au Cantal _/
(d1.codeDpt > d2.codeDpt and v.codeDpt1 = d1.codeDpt2 and v.codeDpt2=d1.codeDpt)
)
select r.intitulé, d1.nom as nomDpt1, d2.nom as nomDpt2
from Région as r, Département as d1, Département as d2, Voisins as v
where r.codeRégion = d1.codeRégion
and r.codeRégion = d2.codeRégion
/_ Pour prendre la relation 'voisins dans le bon sens' _/
and d1.codeDpt < d2.codeDpt
and v.codeDpt1=d1.codeDpt
and v.codeDpt2=d2.codeDpt
select _
from Région as r
where not exists (
select_ from Département as d
where r.codeRégion = d1.codeRégion)
select _
from Personne as p
where not exists (
select _ from Département as d
where d.idPrésident = p.idPrésident)
and not exists (
select \* from Région as d
where r.idPrésident = p.idPrésident)
select r.intitulé, count(d.codeDpt), sum(population) as population
from Région as r, Département as d
where r.codeRégion = d1.codeRégion
group by r.codeRégion, r.intitulé
Si on veut prendre en compte les régions sans département, il faut faire une jointure externe

select r.intitulé, count(d.codeDpt), sum(population) as population
from Région as r left join Département as d on r.codeRégion = d1.codeRégion
group by r.codeRégion
Les régions sans département apparaissent dans le résultat de la jointure avec une valeur à null pour la population, et le count appliqué à null renvoie 0 (tandis que le sum appliqué à null renvoie null).

Modèle relationnel (3 pts)
Soit les attributs TGVER avec les dépendances fonctionnelles suivantes:
;
;
.

Quelle est la clé?

Quel est le résultat de l’algorithme de normalisation?

Ce résultat est-il en troisième forme normale?

Correction

Tous les attributs qui n’apparaissent pas à droite sont nécessairement dans la clé, donc T, V et R. On vérifie aisément que TVR est une clé (la seule).

On a donc les relations (TGE) et (RG), auxquelles il faut ajouter la clé (TVR) (algorithme de normalisation).

Le résultat est en troisième forme normale, si l’on admet une petite extension: dans la relation TGE, la DF E -> G n’a pas la clé pour partie gauche. Mais on ne peut pas décomposer plus sans perdre d’information. C’est un cas (très rare en pratique) où il faut admettre une définition de la 3FN un peu plus compliquée que celle donnée en cours.

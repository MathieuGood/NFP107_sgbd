# NFP 107 : Systèmes de Gestion des Bases de Données

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

Un élement d'une relation de dimension n est un n-uplet (ou tuple) de n valeurs.
Exemple : (123, "Dupont", "Jean", 1980)

Dans la représentation par table, un nuplet est une ligne de la table.
On assimile nuplet et ligne, mais attention, ce n'est pas n'importe quelle ligne, c'est une ligne qui respecte le schéma de la relation.

### Première forme normale (1FN)

Une relation est en première forme normale si tous les attributs sont atomiques, c'est-à-dire qu'ils ne peuvent pas être décomposés en sous-attributs.
Exemple : La relation Étudiant(NumÉtudiant, NomÉtudiant, PrénomÉtudiant, Téléphones) n'est pas en 1FN si Téléphones contient plusieurs numéros séparés par des virgules. Pour être en 1FN, il faudrait avoir un attribut Téléphone unique par nuplet.

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

### Troisième forme normale (3FN)

Toutes les dépendances fonctionnelles doivent avoir pour origine une clé.
Exemple : Dans la relation Manuscrit, la dépendance fonctionnelle NumManuscrit -> Titre est acceptable car NumManuscrit est une clé.
Cependant, si on avait une dépendance fonctionnelle Titre -> AnnéePubli, cela ne serait pas acceptable car Titre n'est pas une clé.

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

« Si j’avais choisi de réifier l’entité Rôle, en lui donnant un identifiant propre, que devient la clé (idFilm, idActeur)? »
Ce n’est plus une clé !

📌 Parce que :

La clé primaire devient idRole (idFilm, idActeur) peut être : un simple couple d’attributs éventuellement UNIQUE (si on le décide) mais ce n’est plus la clé primaire

## Join

| JOIN             | Ce que ça fait                                           | Résultat conservé                 | Quand l’utiliser        |
| ---------------- | -------------------------------------------------------- | --------------------------------- | ----------------------- |
| **JOIN** (INNER) | Ne garde que les lignes qui correspondent des deux côtés | Correspondances uniquement        | Relation obligatoire    |
| **LEFT JOIN**    | Garde toutes les lignes de la table de gauche            | Tout à gauche + correspondances   | Relation optionnelle    |
| **FULL JOIN**    | Garde toutes les lignes des deux tables                  | Tout (avec NULL si pas de match)  | Cas très spécifiques    |
| **CROSS JOIN**   | Produit cartésien                                        | Toutes les combinaisons possibles | Avec sélection derrière |
| **NATURAL JOIN** | Jointure automatique sur les attributs de même nom       | Dépend des noms d’attributs       | Seulement si imposé     |

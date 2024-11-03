# MongoDB - Guide des Opérations de Base

Ce guide couvre les commandes MongoDB essentielles pour débuter avec la base de données NoSQL la plus populaire.

## Table des matières
- [Installation](#installation)
- [Opérations sur les Bases de Données](#opérations-sur-les-bases-de-données)
- [Opérations sur les Collections](#opérations-sur-les-collections)
- [Opérations CRUD](#opérations-crud)
- [Opérateurs de Comparaison](#opérateurs-de-comparaison-courants)
- [Conseils de Sécurité](#conseils-de-sécurité)
- [Pour aller plus loin](#pour-aller-plus-loin)

## Installation

Avant de commencer, assurez-vous d'avoir [MongoDB](https://www.mongodb.com/try/download/community) installé sur votre système.

Pour démarrer le shell MongoDB :
```bash
mongosh
```

## Opérations sur les Bases de Données

### Afficher toutes les bases de données
```javascript
show dbs
```

### Créer/Utiliser une base de données
```javascript
use maDatabase
```
Note : La base de données ne sera créée physiquement qu'après l'insertion de données.

### Afficher la base de données actuelle
```javascript
db
```

### Supprimer une base de données
```javascript
db.dropDatabase()
```

## Opérations sur les Collections

### Créer une collection
```javascript
db.createCollection("utilisateurs")
```

### Afficher toutes les collections
```javascript
show collections
```

## Opérations CRUD

### Create (Créer)
```javascript
// Insérer un document
db.utilisateurs.insertOne({
    nom: "Dupont",
    prenom: "Jean",
    age: 30
})

// Insérer plusieurs documents
db.utilisateurs.insertMany([
    { nom: "Martin", prenom: "Marie", age: 25 },
    { nom: "Bernard", prenom: "Paul", age: 35 }
])
```

### Read (Lire)

#### Opérations de base
```javascript
// Trouver tous les documents
db.utilisateurs.find()

// Trouver avec des conditions
db.utilisateurs.find({ age: { $gt: 30 } })

// Trouver un seul document
db.utilisateurs.findOne({ nom: "Dupont" })
```

#### Projection (Sélectionner des champs spécifiques)
```javascript
// Inclure uniquement nom et age (le _id est inclus par défaut)
db.utilisateurs.find(
    { age: { $gt: 25 } },
    { nom: 1, age: 1 }
)

// Exclure des champs spécifiques
db.utilisateurs.find(
    { age: { $gt: 25 } },
    { email: 0, password: 0 }
)

// Exclure le _id
db.utilisateurs.find(
    { age: { $gt: 25 } },
    { nom: 1, age: 1, _id: 0 }
)
```

#### Tri (sort)
```javascript
// Tri ascendant par age
db.utilisateurs.find().sort({ age: 1 })

// Tri descendant par age
db.utilisateurs.find().sort({ age: -1 })

// Tri multiple (d'abord par age descendant, puis par nom ascendant)
db.utilisateurs.find().sort({ age: -1, nom: 1 })
```

#### Limitation et Pagination
```javascript
// Limiter le nombre de résultats
db.utilisateurs.find().limit(5)

// Sauter les premiers résultats (skip)
db.utilisateurs.find().skip(10)

// Combinaison pour pagination (page 2 avec 10 éléments par page)
db.utilisateurs.find().skip(10).limit(10)

// Tri avec pagination
db.utilisateurs.find()
    .sort({ age: -1 })
    .skip(10)
    .limit(10)
```

#### Comptage
```javascript
// Compter tous les documents
db.utilisateurs.countDocuments()

// Compter avec une condition
db.utilisateurs.countDocuments({ age: { $gt: 30 } })
```

#### Distinct
```javascript
// Obtenir les valeurs uniques d'un champ
db.utilisateurs.distinct("ville")

// Distinct avec condition
db.utilisateurs.distinct("ville", { pays: "France" })
```

#### Recherche avec expressions régulières
```javascript
// Recherche insensible à la casse commençant par 'du'
db.utilisateurs.find({ nom: /^du/i })

// Recherche contenant 'martin'
db.utilisateurs.find({ nom: /martin/i })
```

#### Opérations de recherche avancées

##### Recherche dans les tableaux
```javascript
// Rechercher un élément exact dans un tableau
db.utilisateurs.find({ hobbies: "tennis" })

// Rechercher avec plusieurs conditions sur un tableau
db.utilisateurs.find({ hobbies: { $all: ["tennis", "lecture"] } })

// Rechercher par taille de tableau
db.utilisateurs.find({ hobbies: { $size: 3 } })
```

##### Recherche avec opérateurs logiques
```javascript
// AND implicite
db.utilisateurs.find({
    age: { $gt: 25 },
    ville: "Paris"
})

// OR explicite
db.utilisateurs.find({
    $or: [
        { age: { $lt: 25 } },
        { ville: "Paris" }
    ]
})

// Combinaison AND et OR
db.utilisateurs.find({
    ville: "Paris",
    $or: [
        { age: { $lt: 25 } },
        { statut: "VIP" }
    ]
})
```

### Update (Mettre à jour)

#### Opérateurs de mise à jour courants

1. **$set** - Définir ou modifier des valeurs
```javascript
// Mise à jour simple
db.utilisateurs.updateOne(
    { nom: "Dupont" },
    { $set: { age: 31 } }
)

// Mise à jour de plusieurs champs
db.utilisateurs.updateOne(
    { nom: "Dupont" },
    { $set: {
        age: 31,
        email: "dupont@email.com",
        adresse: {
            rue: "123 rue principale",
            ville: "Paris"
        }
    }}
)
```

2. **$inc** - Incrémenter ou décrémenter des valeurs numériques
```javascript
// Incrémenter l'âge de 1
db.utilisateurs.updateOne(
    { nom: "Dupont" },
    { $inc: { age: 1 } }
)

// Décrémenter le score de 5
db.utilisateurs.updateOne(
    { nom: "Dupont" },
    { $inc: { score: -5 } }
)
```

3. **$unset** - Supprimer des champs
```javascript
// Supprime le champ 'telephone'
db.utilisateurs.updateOne(
    { nom: "Dupont" },
    { $unset: { telephone: "" } }
)
```

4. **$push** - Ajouter des éléments à un tableau
```javascript
// Ajouter un hobby
db.utilisateurs.updateOne(
    { nom: "Dupont" },
    { $push: { hobbies: "tennis" } }
)

// Ajouter plusieurs hobbies
db.utilisateurs.updateOne(
    { nom: "Dupont" },
    { $push: { hobbies: { $each: ["tennis", "lecture", "voyage"] } } }
)
```

5. **$pull** - Retirer des éléments d'un tableau
```javascript
// Retirer un hobby
db.utilisateurs.updateOne(
    { nom: "Dupont" },
    { $pull: { hobbies: "tennis" } }
)
```

6. **$addToSet** - Ajouter des éléments à un tableau (sans doublons)
```javascript
// Ajoute uniquement si l'élément n'existe pas
db.utilisateurs.updateOne(
    { nom: "Dupont" },
    { $addToSet: { hobbies: "tennis" } }
)
```

7. **$rename** - Renommer un champ
```javascript
// Renommer le champ 'telephone' en 'tel'
db.utilisateurs.updateOne(
    { nom: "Dupont" },
    { $rename: { "telephone": "tel" } }
)
```

8. **$min** et **$max** - Mettre à jour si la valeur est plus petite/grande
```javascript
// Met à jour l'âge seulement si la nouvelle valeur est plus petite
db.utilisateurs.updateOne(
    { nom: "Dupont" },
    { $min: { age: 25 } }
)

// Met à jour l'âge seulement si la nouvelle valeur est plus grande
db.utilisateurs.updateOne(
    { nom: "Dupont" },
    { $max: { age: 35 } }
)
```

9. **$currentDate** - Définir la date actuelle
```javascript
// Définit la date de dernière modification
db.utilisateurs.updateOne(
    { nom: "Dupont" },
    { $currentDate: { 
        lastModified: true,
        timestamp: { $type: "timestamp" }
    }}
)
```

10. **$mul** - Multiplier une valeur
```javascript
// Multiplier le score par 2
db.utilisateurs.updateOne(
    { nom: "Dupont" },
    { $mul: { score: 2 } }
)
```

### Delete (Supprimer)
```javascript
// Supprimer un document
db.utilisateurs.deleteOne({ nom: "Dupont" })

// Supprimer plusieurs documents
db.utilisateurs.deleteMany({ age: { $lt: 25 } })
```

## Opérateurs de comparaison courants

- `$eq` : égal à
- `$gt` : supérieur à
- `$gte` : supérieur ou égal à
- `$lt` : inférieur à
- `$lte` : inférieur ou égal à
- `$ne` : différent de
- `$in` : dans un tableau de valeurs
- `$nin` : pas dans un tableau de valeurs

Exemple :
```javascript
// Trouver les utilisateurs entre 25 et 35 ans
db.utilisateurs.find({
    age: {
        $gte: 25,
        $lte: 35
    }
})
```

## Agrégation simple
```javascript
// Grouper et compter
db.utilisateurs.aggregate([
    { $group: { 
        _id: "$ville",
        total: { $sum: 1 }
    }}
])

// Grouper avec moyenne d'âge
db.utilisateurs.aggregate([
    { $group: {
        _id: "$ville",
        moyenneAge: { $avg: "$age" },
        nombreUtilisateurs: { $sum: 1 }
    }}
])
```

## Curseur et méthodes chaînées
```javascript
// Plusieurs opérations chaînées
const cursor = db.utilisateurs.find({ age: { $gt: 25 } })
    .sort({ age: -1 })
    .skip(10)
    .limit(5)
    .pretty()

// Parcourir le curseur
while (cursor.hasNext()) {
    printjson(cursor.next());
}

// Alternative avec forEach
cursor.forEach(printjson)
```

## Options de requête
```javascript
// Définir un timeout pour la requête
db.utilisateurs.find().maxTimeMS(1000)

// Retourner les résultats expliqués (pour l'optimisation)
db.utilisateurs.find({ age: { $gt: 25 } }).explain("executionStats")
```

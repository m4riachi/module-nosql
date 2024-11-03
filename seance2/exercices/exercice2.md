## Requêtes MongoDB pour la base de données gescom

#### 1. Afficher toutes les collections de la base
```javascript
show collections
```

#### 2. Afficher tous les documents de la base
```javascript
db.employes.find()
```

#### 3. Compter le nombre de documents dans la collection employes
```javascript
db.employes.countDocuments()
```

#### 4. Insérer deux employés de deux manières différentes
```javascript
// Première méthode
db.employes.insertOne({
    nom: "Dupont",
    prénom: "Jean",
    prime: 1000
})

// Deuxième méthode
db.employes.insertOne({
    nom: "Martin",
    prénom: "Marie",
    ancienneté: 5
})
```

#### 5. Afficher la liste des employés dont le prénom est David
```javascript
db.employes.find({ prénom: "David" })
```

#### 6. Afficher la liste des employés dont le prénom commence ou se termine par D
```javascript
db.employes.find({ 
    $or: [
        { prenom: /^D/ },
        { prenom: /D$/ }
    ]
})
```

#### 7. Afficher la liste des personnes dont le prénom commence par D et contient exactement 5 lettres
```javascript
db.employes.find({ prenom: /^D.{4}$/ })
```

#### 8. Afficher la liste des personnes dont le prénom commence et se termine par une voyelle
```javascript
db.employes.find({
    $and: [
        { prenom: /^[aeiouAEIOU]/ },
        { prenom: /[aeiouAEIOU]$/ }
    ]
})
```

#### 9. Afficher la liste des personnes dont le prénom commence et se termine par la même lettre
```javascript
db.employes.find({
    prénom: /^(.).*\1$/
})
```

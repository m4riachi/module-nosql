#### 10. Afficher les nom et prénom des employés avec ancienneté > 10
```javascript
db.employes.find(
    { anciennete: { $gt: 10 } },
    { nom: 1, prenom: 1, _id: 0 }
)
```

#### 11. Afficher nom et adresse complète des employés ayant un attribut rue
```javascript
db.employes.find(
    { "adresse.rue": { $exists: true } },
    { nom: 1, adresse: 1, _id: 0 }
)
```

#### 12. Incrémenter de 200 la prime des employés ayant le champ prime
```javascript
db.employes.updateMany(
    { prime: { $exists: true } },
    { $inc: { prime: 200 } }
)
```

#### 13. Afficher les trois employés avec la plus grande ancienneté
```javascript
db.employes.find()
    .sort({ ancienneté: -1 })
    .limit(3)
```

#### 14. Regrouper les personnes résidant à Toulouse
```javascript
db.employes.find(
    { "adresse.ville": "Toulouse" }
)
```

#### 15. Afficher les personnes dont le prénom commence par M et résidant à Foix ou Bordeaux
```javascript
db.employes.find({
    $and: [
        { prenom: /^M/ },
        { "adresse.ville": { $in: ["Foix", "Bordeaux"] } }
    ]
})
```

#### 16. Mettre à jour l'adresse de Dominique Mani
```javascript
db.employes.updateOne(
    { nom: "Mani", prénom: "Dominique" },
    { $set: { 
        adresse: {
            numero: 20,
            ville: "Marseille",
            codepostal: "13015"
        }
    }}
)
```

#### 17. Attribuer une prime aux employés sans prime et hors Toulouse, Bordeaux et Paris
```javascript
db.employes.updateMany(
    { 
        prime: { $exists: false },
        "adresse.ville": { $nin: ["Toulouse", "Bordeaux", "Paris"] }
    },
    { $set: { prime: 1500 } }
)
```

#### 18. Remplacer le champ tel par un tableau téléphone
```javascript
db.employes.updateMany(
    { tel: { $exists: true } },
    [
        { 
            $set: { 
                telephone: ["$tel"]
            }
        },
        {
            $unset: "tel"
        }
    ]
)
```

#### 19. Créer un champ prime basé sur le nombre de caractères de la ville
```javascript
db.employes.updateMany(
    { prime: { $exists: false } },
    [
        {
            $set: {
                prime: {
                    $multiply: [
                        100,
                        { $strLenCP: "$adresse.ville" }
                    ]
                }
            }
        }
    ]
)
```

#### 20. Créer un champ mail basé sur le nom et prénom
```javascript
db.employes.updateMany(
    { telephone: { $exists: false } },
    [
        {
            $set: {
                mail: {
                    $concat: [
                        { $toLower: "$nom" },
                        ".",
                        { $toLower: "$prénom" },
                        "@formation.fr"
                    ]
                }
            }
        }
    ]
);
```

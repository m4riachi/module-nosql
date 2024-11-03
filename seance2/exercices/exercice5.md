Je vais vous aider à créer une requête MongoDB pour calculer la somme de l'ancienneté par prénom.

```javascript
db.employes.aggregate([
    // Groupe par prénom et calcule la somme de l'ancienneté
    {
        $group: {
            _id: '$prenom',
            totalAnciennete: { $sum: "$anciennete" },
            nombreEmployes: { $sum: 1 }
        },
    },
    {
        $match: {
            nombreEmployes: { $gt: 1 }
        }
    },
])
```

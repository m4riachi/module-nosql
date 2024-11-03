#### 1. Insertion des Factures

```javascript
db.factures.insertMany([
    {
        numero: "10012A",
        date: new Date("2013-07-04"),
        client: {
            nom: "Alexandre Terrasa",
            courriel: "foobar@example.com"
        },
        produits: [
            {
                code: "MACBOOKAIR",
                nom: "Macbook Air",
                prix: 999.99,
                quantite: 1
            },
            {
                code: "APPLESUPPORT",
                nom: "AppleCare 1 an",
                prix: 149.99,
                quantite: 1
            }
        ],
        total: 1149.98
    },
    {
        numero: "10013A",
        date: new Date("2013-07-05"),
        client: {
            nom: "Jacques Berger",
            courriel: "berger.jacques@uqam.ca"
        },
        produits: [
            {
                code: "LENOVOX230",
                nom: "Lenovo Thinkpad X230",
                prix: 899.99,
                quantite: 1
            }
        ],
        total: 899.99
    }
])
```

#### 2. Recherche par Numéro de Facture: 10013A

```javascript
db.factures.findOne({ numero: "10013A" })
```

#### 3. Modification de Facture en changeant la date et le courriel du client

```javascript
db.factures.updateOne(
    { numero: "10012A" },
    { 
        $set: {
            date: new Date("2013-07-03"),
            "client.courriel": "alex@example.com"
        }
    }
)
```

#### 4. Recherche par Code Produit LENOVOX230

```javascript
db.factures.findOne({
    "produits.code": "LENOVOX230"
})
```

#### 5. Suppression de la Facture numero: 10012A

```
db.factures.deleteOne({ numero: "10012A" })
```

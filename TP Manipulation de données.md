# TP Manipulation de données
## Analyse de la collection "restaurants"
### 2.2
Notre BDD est composée de deux collections: 
- Restaurants (25359 documents)
- Neighborhoods (195 documents)

### 2.3

| Attribut | Description | Type de données |
| -------- | -------- | -------- |
| _id    | identifiant interne    | ObjectId    |
| address|l'addresse du restaurant| sous-document|
|address.building|numero du batiment| code|
|address.coord| coordonnées en latitude et longitude| liste de deux nombres|
|address.street| la rue de l'addresse|texte|
|address.zipcode| code postal de l'addresse| code|
|borough| quartier dans lequel se situe le restaurant| texte|
|cuisine| style de nourriture proposé par le restaurant| texte|
|grades| détail des revues sur ce restaurant| ensemble de sous-documents|
|grades.X.date|date de la revue| date|
|grades.X.grade| Le grade de la revue| lettre|
|grades.X.score| La note obtenue | nombre entier |
|name|Nom et prénom du propriétaire|texte|
|restaurant_id|identifiant explicite| nombre |

 
> "Code" est utilisé car parfois (dans certains régions) les codes postales et les codes de bâtiments contiennnent des lettres dans leur code. Dans le cas contraire on peut les désigner comme "nombre". 

## INTERROGATION DE LA COLLECTION “restaurants”

### 3.1 

```
db.restaurants.find().limit(5)
```

### 3.2 

```
db.restaurants.find(
    {},
    {"restaurant_id":1,
    "address":1,
    "name":1,
    "cuisine":1}
    )
```
### 3.3 

```
db.restaurants.find(
    {},
    {"_id":0,
    "restaurant_id":1,
    "address":1,
    "cuisine":1,}
    ).sort(
        {cuisine:1}
    )
```
### 3.4 

```
db.restaurants.find(
    {},
    {"address":0}
)
```
### 3.5 

```
db.restaurants.find(
    {"cuisine":"French"}
).skip(5)
```
> Count: 339
### 3.6

```
db.restaurants.find(
    {"borough":{"$ne":"Queens"}},
    {"_id":0,
    "address":1,
    "restaurant_id":1,
    "cuisine":1,
    "name":1
    }
)
```
> Count: 19703
### 3.7 

```
db.restaurants.find(
    {"cuisine":{"$in":["French","Italian","German"]}},
    {"restaurant_id":,
    "name":1,
    "borough":1}
)
```
> Count: 1444
### 3.8

```
db.restaurants.find(
    {"borough":{$nin:["Manhattan", 
        "Brooklyn",
        "Queens"]}},
    {"restaurant_id":1,
        "name":1,
        "borough":1}
).sort({borough:1})

```
> Count: 3358
### 3.9

```
db.restaurants.find(
    {$and:[{"borough":{$eq: "Brooklyn"}},
        {"cuisine":{$eq: "American"}}
    ]}
    
)
```
> Count: 1273
### 3.10 

```
db.restaurants.find(
    {$or:[{"borough":"Queens"},
        {"cuisine":"Chinese"}]},
    {"restaurant_id":1,
        "name":1,
        "cuisine":1,
        "borough":1,
        "_id":0
        }
).limit(20)
```
> Count: 20
### 3.11 

```
db.restaurants.find(
    {"borough":{$ne:"Manhattan"}},
    {"restaurant_id":1,
        "borough":1,
        "cuisine":1
    }

)
```
> Count: 15100
### 3.12

```
db.restaurants.find(
    {"borough": {$ne: "Queens"}, "cuisine": {$ne: "American"}},
    {"restaurant_id":1,
     "name":1,
     "borough":1
    }
).limit(10).sort(
    {name:-1}
)
```
> Count: 10
### 3.13 

```
db.restaurants.find(
    {$or:[{$and:[{"borough":"Brooklyn"},
                 {"cuisine":"Chinese"}]},
          {$and:[{"borough":"Queens"},
                 {"cuisine":"African"}]}      
                 ]},
    {"name":1,
     "borough":1,
     "cuisine":1
    }

)
```
> Count: 768
### 3.14 

```
db.restaurants.find(
    {"name" :{$regex:/'s/, $options: "i"}}
    )
```
> Count: 3142
### 3.15

```

db.restaurants.find(
    {"name" : {$regex:/^ba/, $options:"i"}},
    {"restaurant_id":1,
     "name":1,
     "address":1
    }
).sort(
    {name:1}
)
```
> Count: 460
### 3.16 

```
db.restaurants.find(
    {"name" :{$regex:/[be]$/, $options: "i"}},
    {"restaurant_id":1,
     "name":1,
     "cuisine":1
    }
)
```
> Count: 4306
### 3.17 

```
db.restaurants.find(
    {$or:[{"name":{$regex:/pasta/,$options:"i"}},
          {"name":{$regex:/hamburger/,$options:"i"}}]},
    {
     "name":1
    }
)

```
> Count: 84
### 3.18

```
db.restaurants.find(
    {$and:[{name:{$regex:/Cafe$/,$options:"i"}},
          {cuisine:{$regex:/Ice Cream/,$options:"i"}}]},
    {
     "name":1,
     "cuisine":1
    }
)
```
> Count: 5
# TP MongoDB - Chouaib Bellouch

## 1. Afficher tous les films d'action réalisés en France
```javascript
db.films.find({ genre: "Action", country: "FR" });
```

## 2. Afficher le nombre de films d'action
```javascript
db.films.countDocuments({ genre: "Action" });
```

## 3. Afficher les films d'action réalisés en France en 1963
```javascript
db.films.find({ genre: "Action", country: "FR", year: 1963 });
```

## 4. Afficher les films d'action réalisés en France sans grades
```javascript
db.films.find({ genre: "Action", country: "FR" }, { grades: 0 });
```

## 5. Afficher les films d'action réalisés en France sans identifiants
```javascript
db.films.find({ genre: "Action", country: "FR" }, { _id: 0 });
```

## 6. Afficher les titres et grades des films d'action réalisés en France
```javascript
db.films.find({ genre: "Action", country: "FR" }, { title: 1, grades: 1, _id: 0 });
```

## 7. Afficher les films d'action réalisés en France ayant une note supérieure à 10
```javascript
db.films.find({ genre: "Action", country: "FR", "grades.note": { $gt: 10 } }, { title: 1, grades: 1, _id: 0 });
```

## 8. Afficher les films d'action réalisés en France avec toutes les notes supérieures à 10
```javascript
db.films.find({ genre: "Action", country: "FR", grades: { $not: { $elemMatch: { note: { $lt: 10 } } } } }, { title: 1, grades: 1, _id: 0 });
```

## 9. Afficher les genres distincts
```javascript
db.films.distinct("genre");
```

## 10. Afficher les grades distincts
```javascript
db.films.distinct("grades");
```

## 11. Afficher les films sans résumé
```javascript
db.films.find({ summary: "" });
```

## 12. Afficher les films joués par Leonardo DiCaprio en 1997
```javascript
db.films.find({ "actors.last_name": "DiCaprio", "actors.first_name": "Leonardo", year: 1997 });
```

## 13. Afficher les films tournés avec Leonardo DiCaprio ou en 1997
```javascript
db.films.find({ $or: [ { "actors.last_name": "DiCaprio", "actors.first_name": "Leonardo" }, { year: 1997 } ] });

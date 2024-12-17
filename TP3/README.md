# Introduction à Apache CouchDB

Ce document résume les points clés et les démonstrations d'une vidéo sur **CouchDB**, un système de base de données documentaire NoSQL. Il met en avant l'installation, les opérations de base et l'utilisation pratique via des méthodes RESTful API.

---

## 1. **Qu'est-ce que CouchDB ?**

- **CouchDB** est un système de base de données orienté documents.
- Il est **facile à installer, utiliser et comprendre**.
- Il expose ses fonctionnalités via une **API RESTful**, ce qui le rend accessible à l'aide des méthodes HTTP.

**Principales méthodes HTTP :**
- `GET`  : Récupérer une ressource (ou sa représentation).
- `PUT`  : Créer ou remplacer une ressource.
- `POST` : Envoyer des données à une ressource (par exemple, insérer un document).
- `DELETE`: Supprimer une ressource.

Apache CouchDB est open-source et soutenu par la **Fondation Apache Software**.

---

## 2. **Installation de CouchDB**

### **Option 1 : Installation directe**
- Installez Apache CouchDB directement sur votre machine.
- La documentation sera disponible localement après l'installation.

### **Option 2 : Utilisation de Docker**
Pour exécuter CouchDB dans un conteneur Docker :
```bash
sudo docker run -d --name couchdb-demo \
  -e COUCHDB_USER=Youssef \
  -e COUCHDB_PASSWORD=meilleur \
  -p 5984:5984 \
  couchdb
```
- **Remarque** : Mappez les volumes pour persister les données :
  ```bash
  -v /chemin/vers/donnees:/opt/couchdb/data
  ```
- CouchDB écoute par défaut sur le port **5984**.

### **Vérification de l'installation**
Vérifiez le conteneur :
```bash
sudo docker ps
```
Accédez à l'interface CouchDB : `http://localhost:5984/_utils`

---

## 3. **Opérations RESTful de base avec CouchDB**

### **3.1. Créer une base de données**
Utilisez `PUT` pour créer une base appelée "films" :
```bash
curl -X PUT http://Youssef:meilleur@localhost:5984/films
```

### **3.2. Récupérer des informations sur la base**
Utilisez `GET` pour récupérer les détails de la base :
```bash
curl -X GET http://Youssef:meilleur@localhost:5984/films
```

### **3.3. Insérer un document**
Utilisez `PUT` pour insérer un document :
```bash
curl -X PUT http://Youssef:meilleur@localhost:5984/films/doc1 \
  -H "Content-Type: application/json" \
  -d '{"title": "Inception", "year": 2010}'
```

### **3.4. Récupérer un document**
Utilisez `GET` avec l'identifiant du document :
```bash
curl -X GET http://Youssef:meilleur@localhost:5984/films/doc1
```

### **3.5. Insérer plusieurs documents (en masse)**
Créez un fichier `films.json` :
```json
{"docs": [
  {"title": "Inception", "year": 2010},
  {"title": "Interstellar", "year": 2014}
]}
```
Insérez la collection :
```bash
curl -X POST http://Youssef:meilleur@localhost:5984/films/_bulk_docs \
  -H "Content-Type: application/json" \
  -d @films.json
```

### **3.6. Supprimer un document**
Pour supprimer un document, récupérez d'abord son `_rev` (ID de révision) :
```bash
curl -X DELETE http://Youssef:meilleur@localhost:5984/films/doc1?rev=<revision_id>
```

---

## 4. **Fonctionnalités de CouchDB**
- **Sans schéma** : CouchDB permet des documents avec des structures variables.
- **Versionnage** : Chaque document garde une trace de ses révisions.
- **Format JSON** : Les documents sont stockés en JSON.
- **API simple** : Les opérations reposent sur les méthodes HTTP.

---

## 5. **MapReduce avec CouchDB**

### **5.1. Introduction à MapReduce**
- **MapReduce** est un concept clé du Big Data pour les calculs parallèles.
- CouchDB permet d'utiliser **Map** et **Reduce** pour transformer et regrouper les données JSON.
- Avantage : **CouchDB** ne rend pas obligatoire la définition d'une fonction **Reduce**. On peut sauvegarder les résultats intermédiaires de la fonction **Map**.

### **5.2. Fonction Map**
- La fonction **Map** prend un document comme paramètre et produit une ou plusieurs sorties sous forme de **clé-valeur**.
- Exemple : **Calculer le nombre de films par année**.
```javascript
function (doc) {
  emit(doc.year, 1);
}
```

- **Résultat intermédiaire** :
  - Chaque document émet une clé (l'année) et une valeur (1).
  - CouchDB prépare les données pour l'étape **Reduce**.

### **5.3. Fonction Reduce**
- La fonction **Reduce** agrège les résultats intermédiaires.
- Exemple : **Calculer la somme pour chaque année**.
```javascript
function (keys, values, rereduce) {
  return sum(values);
}
```

### **5.4. Exécution des fonctions**
1. Définir la fonction **Map** et sauvegarder.
2. Activer la fonction **Reduce** en cochant l'option correspondante.
3. Récupérer les résultats agrégés par année.

**Exemple de sortie :**
| Année   | Nombre de films |
|----------|----------------|
| 1921     | 1              |
| 1936     | 2              |

### **5.5. Deuxième Exemple : Nombre de films par acteur**
- La fonction **Map** peut parcourir un tableau d'acteurs pour chaque document :
```javascript
function (doc) {
  for (var i in doc.actors) {
    emit(doc.actors[i], 1);
  }
}
```
- La fonction **Reduce** regroupe les données par acteur et calcule la somme.

**Exemple de sortie :**
| Acteur        | Nombre de films |
|---------------|-----------------|
| John Doe      | 3               |
| Jane Smith    | 2               |

---

## 6. **Notes pratiques**
- Inspectez toujours les documents pour comprendre leur structure.
- Expérimentez avec des fonctions Map et Reduce pour des analyses personnalisées.

---

## 7. **Limitations des bases NoSQL**
- La flexibilité du schéma peut entraîner une **redondance** et des **incohérences**.
- Exemple : Un nom d'acteur peut être écrit différemment dans plusieurs documents.

---

## 8. **Résumé**
Cette vidéo a couvert :
- Les étapes d'installation de CouchDB.
- L'utilisation des opérations CRUD via l'API RESTful.
- Le concept MapReduce avec des exemples pratiques.

---

## **Prochaines étapes**
Explorez des fonctionnalités avancées :
1. **Sharding** et **Réplication** pour les systèmes distribués.
2. Expérimentez avec des vues complexes et des fonctions personnalisées.

Référez-vous à la [documentation officielle de CouchDB](http://couchdb.apache.org/) pour plus de détails.

---

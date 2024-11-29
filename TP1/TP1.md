# Introduction à Redis

Redis est une base de données NoSQL, basée sur le principe clé-valeur. Elle stocke les données principalement en mémoire RAM, tout en offrant des options de persistance. Redis est souvent utilisée en combinaison avec des bases relationnelles pour stocker des données temporaires ou améliorer les performances.

---

## Commandes de base

### CRUD (Create, Read, Update, Delete)
- **Create** : Utilisation de la commande `SET` pour définir une clé avec une valeur.  
  Exemple : `SET demo "Bonjour"`.
- **Read** : Utilisation de `GET`.  
  Exemple : `GET demo` retourne `"Bonjour"`.
- **Update** : Redéfinir la valeur avec `SET`.
- **Delete** : Utilisation de `DEL`.  
  Exemple : `DEL demo`.

---

## Gestion de la persistance

Redis sauvegarde les données sur disque avec des mécanismes configurables. Les données non persistées peuvent être perdues si le serveur s’arrête brutalement.

---

## Exemple d'utilisation

### Compteur de visites
- **Initialisation** : `SET cpt 0` (initialise le compteur `cpt` à 0).  
- **Incrémentation** : `INCR cpt`.  
- **Décrémentation** : `DECR cpt`.

---

## Durée de vie des clés

- Redis permet de définir une expiration pour les clés avec `EXPIRE`.  
  Exemple : `EXPIRE cpt 60` (définit une durée de vie de 60 secondes).
- **Vérification de la durée de vie restante** :  
  Commande : `TTL cpt` retourne le nombre de secondes restantes avant l'expiration.

---

## Structures de données

### Listes
- **Ajout** : 
  - `LPUSH` (ajoute à gauche).  
  - `RPUSH` (ajoute à droite).
- **Lecture** :  
  `LRANGE` (par intervalle d’indices).  
  Exemple : `LRANGE myList 0 -1` affiche tous les éléments.
- **Suppression** : 
  - `LPOP` (à gauche).  
  - `RPOP` (à droite).

### Ensembles
- **Ajout** : `SADD`.  
- **Affichage** : `SMEMBERS`.  
- **Suppression** : `SREM`.

### Opérations sur les ensembles
- **Union** : `SUNION`.  
  Exemple : Fusion de deux ensembles `utilisateurs` et `autresUtilisateurs`.

### Sets ordonnés
- **Définition** : Les Sets ordonnés associent chaque élément à un score numérique, permettant un tri automatique.
- **Commandes clés** :
  - **Ajout d'éléments** :  
    Exemple : `ZADD score 19 "Augustin"`.
  - **Lecture** :  
    - `ZRANGE` pour récupérer les éléments triés par score croissant.  
      Exemple : `ZRANGE score 0 -1`.
    - `ZREVRANGE` pour un tri décroissant.
  - **Rang d'un élément** :  
    Exemple : `ZRANK score "Augustin"` retourne son rang (commence à 0).

### Hashes
- **Définition** : Les Hashes permettent de stocker plusieurs champs associés à une clé.
- **Commandes clés** :
  - **Ajout** :
    - Individuel : `HSET user:1 champ valeur`.  
      Exemple : `HSET user:1 "username" "Youssef"`.
    - En une seule commande : `HMSET user:2 "username" "Augustin" "age" 25 "email" "augustin@gmail.com"`.
  - **Lecture** :
    - Récupérer tous les champs : `HGETALL user:1`.
    - Récupérer un champ spécifique : `HGET user:1 "username"`.
    - Récupérer uniquement les valeurs de tous les champs : `HVALS user:1`.
  - **Modification** :
    - Incrémenter un champ numérique : `HINCRBY user:2 "age" 4`.

---

## Fonctionnement des Pub/Sub

### Souscription à un canal
- Commande : `SUBSCRIBE channel_name`.  
  Exemple : `SUBSCRIBE mes_cours` inscrit un utilisateur au canal `mes_cours`.

### Publication sur un canal
- Commande : `PUBLISH channel_name message`.  
  Exemple : `PUBLISH mes_cours "Un nouveau cours est disponible"` envoie un message à tous les utilisateurs inscrits au canal `mes_cours`.

### Souscription à plusieurs canaux avec un modèle (Pattern)
- Commande : `PSUBSCRIBE pattern`.  
  Exemple : `PSUBSCRIBE mes*` inscrit un utilisateur à tous les canaux dont le nom commence par `mes` (par exemple, `mes_cours`, `mes_notes`).

### Commandes utiles pour gérer les bases et les clés
- **Lister les clés** : `KEYS *` affiche toutes les clés disponibles dans la base active.
- **Changer de base de données** : 
  Redis offre 16 bases par défaut, numérotées de 0 à 15.
  - Commande : `SELECT index`.
  - Exemple : `SELECT 1` pour passer à la base de données numéro 1.
  - Retour à la base par défaut : `SELECT 0`.

---

## Documentation Redis

Redis propose une documentation officielle complète qui couvre en détail les fonctionnalités et commandes présentées dans ce rapport :

- **Organisation claire** :
  - Documentation divisée par type de structure de données (Listes, Sets, Hashes, Pub/Sub, etc.).
  - Chaque section inclut des commandes spécifiques avec des exemples pratiques.
- **Recherche efficace** :
  - Recherche textuelle disponible pour retrouver rapidement une commande précise.
  - Filtres pour cibler une structure de données spécifique.
- **Exemples détaillés** :
  - Chaque commande est accompagnée d’exemples concrets.
  - Cas pratiques abordant la gestion de la persistance et des performances.

Lien vers la documentation officielle : [Redis Documentation](https://redis.io/docs/)

---



* * *

   # 🧩 MongoDB Sharded Cluster Simulation with Docker
    
   Ce projet met en place une **simulation complète d’un cluster MongoDB shardé** en utilisant Docker Compose.  
    Il reproduit une architecture distribuée proche d’un environnement de production avec :
    
   - 🔹 Config Servers en Replica Set
   - 🔹 Deux Shards en Replica Sets
   - 🔹 Un routeur mongos
   - 🔹 Sharding activé sur une collection
   - 🔹 Test de distribution des données
    
 ---
    
   # 🚀 Objectif du Projet
    
 Ce laboratoire a pour but de :
    
   - Comprendre le fonctionnement du **Sharding MongoDB**
    - Implémenter des **Replica Sets**
    - Simuler une **architecture distribuée**
    - Expérimenter la **scalabilité horizontale**
    - Observer la répartition des données entre plusieurs shards
    
    ---
    
   # 🛠️ Stack Technique
    
   ## 🔧 Technologies Utilisées


 

| Technologie | Rôle dans le Projet |
|------------|--------------------|
| <img src="https://cdn.simpleicons.org/docker" width="20" height="20"/> **Docker** | Conteneurisation des services |
| <img src="https://cdn.simpleicons.org/docker" width="20" height="20"/> **Docker Compose** | Orchestration multi-conteneurs |
| <img src="https://cdn.simpleicons.org/mongodb" width="20" height="20"/> **MongoDB 7** | Base de données NoSQL |
| <img src="https://cdn.simpleicons.org/mongodb" width="20" height="20"/> **MongoDB Replica Sets** | Haute disponibilité |
| <img src="https://cdn.simpleicons.org/mongodb" width="20" height="20"/> **MongoDB Sharding** | Scalabilité horizontale |
| <img src="https://cdn.simpleicons.org/mongodb" width="20" height="20"/> **mongos** | Routeur du cluster |
| <img src="https://cdn.simpleicons.org/mongodb" width="20" height="20"/> **mongosh** | Interface CLI MongoDB |
    
   ---
    
    ## 🏗️ Architecture du Cluster
    
    | Composant | Nombre de nœuds | Port | Rôle |
    |-----------|----------------|------|------|
    | Config Servers (cfgRS) | 3 | 27019 | Stockage des métadonnées du cluster |
    | Shard 1 (shard1RS) | 3 | 27018 | Données shard 1 |
    | Shard 2 (shard2RS) | 3 | 27018 | Données shard 2 |
    | mongos Router | 1 | 27017 | Point d’entrée du cluster |
    
    ---
    
    # 📦 Fonctionnalités Implémentées
    
    | Fonctionnalité | Description |
    |---------------|------------|
    | Replica Sets | Chaque shard est configuré en Replica Set (3 membres) |
    | Config Server RS | Replica Set dédié aux métadonnées |
    | Ajout dynamique de shards | Utilisation de `sh.addShard()` |
    | Activation du sharding | `sh.enableSharding()` |
    | Sharding d’une collection | `sh.shardCollection()` |
    | Test de distribution | `getShardDistribution()` |
    
    ---
    
    # ⚙️ Déploiement
    
    ## 1️⃣ Lancer le cluster
    
    ```bash
    docker compose up -d
    

Vérifier :

    docker ps
    

* * *

2️⃣ Initialiser les Replica Sets
--------------------------------

Initialiser :

*   Config Server RS
    
*   Shard 1 RS
    
*   Shard 2 RS
    

Puis ajouter les shards au routeur `mongos`.

* * *

🧪 Test de Scalabilité
======================

Insertion de 2000 documents :

    db.users.insertMany(
      Array.from({length: 2000}, (_,i)=>({ userId: i, name: "u"+i }))
    )
    

Vérification :

    db.users.getShardDistribution()
    

Résultat : répartition automatique des données entre les shards.

* * *

📊 Concepts Démontrés
=====================

Concept

Explication

Scalabilité horizontale

Ajout de shards pour distribuer la charge

Haute disponibilité

Replica Sets avec Primary + Secondaries

Partitionnement des données

Distribution par clé de sharding

Architecture distribuée

Séparation Config / Shards / Router

* * *

📁 Structure du Projet
======================

    mongo-sharding-docker/
    │
    ├── docker-compose.yml
    └── README.md
    

* * *

🎯 Compétences Démontrées
=========================

*   Architecture de bases de données distribuées
    
*   Conception de systèmes scalables
    
*   Conteneurisation avancée avec Docker
    
*   Orchestration multi-services
    
*   Manipulation avancée de MongoDB
    

* * *

📜 Licence
==========

MIT License

    
    ---
    
    Si tu veux, je peux aussi te donner :
    
    - 🔥 Une version encore plus “Cloud Engineer / DevOps style”
    - 📊 Un diagramme d’architecture en Mermaid
    - 🎨 Une version README premium avec badges (Docker, MongoDB, etc.)
    - 🚀 Une version prête pour impression recruteur
    
    Dis-moi ce que tu préfères 😎

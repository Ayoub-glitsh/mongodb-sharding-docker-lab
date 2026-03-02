<p align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=26&duration=2500&pause=800&center=true&vCenter=true&width=1000&lines=MongoDB+Sharded+Cluster+Simulation+with+Docker" alt="Typing SVG" />
</p>


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
| <img src="https://cdn.simpleicons.org/mongodb" width="20"/> **Config Servers (cfgRS)** | 3 | 27019 | Stockage des métadonnées du cluster |
| <img src="https://cdn.simpleicons.org/mongodb" width="20"/> **Shard 1 (shard1RS)** | 3 | 27018 | Données shard 1 |
| <img src="https://cdn.simpleicons.org/mongodb" width="20"/> **Shard 2 (shard2RS)** | 3 | 27018 | Données shard 2 |
| <img src="https://cdn.simpleicons.org/nginx" width="20"/> **mongos Router** | 1 | 27017 | Point d’entrée du cluster |
    
---
    

    
  ## 📦 Fonctionnalités Implémentées

| Fonctionnalité | Description |
|---------------|------------|
| <img src="https://cdn.simpleicons.org/mongodb" width="20"/> **Replica Sets** | Chaque shard est configuré en Replica Set (3 membres) |
| <img src="https://cdn.simpleicons.org/mongodb" width="20"/> **Config Server RS** | Replica Set dédié aux métadonnées |
| <img src="https://cdn.simpleicons.org/mongodb" width="20"/> **Ajout dynamique de shards** | Utilisation de `sh.addShard()` |
| <img src="https://cdn.simpleicons.org/mongodb" width="20"/> **Activation du sharding** | `sh.enableSharding()` |
| <img src="https://cdn.simpleicons.org/mongodb" width="20"/> **Sharding d’une collection** | `sh.shardCollection()` |
| <img src="https://cdn.simpleicons.org/mongodb" width="20"/> **Test de distribution** | `getShardDistribution()` |
    
   ---
    
   # ⚙️ Déploiement
    
   ## 1️⃣ Lancer le cluster
    
   ```bash
    docker compose up -d
   ```

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


| Concept | Explication |
|----------|------------|
| 🚀 **Scalabilité horizontale** | Ajout de shards pour distribuer la charge et augmenter la capacité du système |
| 🛡️ **Haute disponibilité** | Replica Sets avec un nœud Primary et plusieurs Secondaries pour assurer la tolérance aux pannes |
| 🧩 **Partitionnement des données** | Distribution des données selon une clé de sharding |
| 🌐 **Architecture distribuée** | Séparation des rôles entre Config Servers, Shards et mongos Router |

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
   

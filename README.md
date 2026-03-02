
   # 🧩 Simulation de Sharding MongoDB avec Docker
    
  Ce projet propose une **simulation pratique d’un cluster MongoDB shardé** en utilisant **Docker Compose**.  
    Il reproduit une architecture proche de la production avec :
    
  - ✅ **2 shards**, chacun en **Replica Set (3 nœuds)**
  - ✅ **Config Servers** en **Replica Set (3 nœuds)**
  - ✅ **1 routeur mongos** (point d’entrée du cluster)
  - ✅ Activation du sharding sur une base + collection
  - ✅ Tests de distribution des données sur les shards
    
    ---
    
    ## 🎯 Objectifs pédagogiques
    
    - Comprendre le rôle de `mongos`, des `config servers` et des shards
    - Mettre en place un cluster shardé avec Replica Sets
    - Activer le sharding sur une base de données
    - Sharder une collection et observer la distribution des documents
    
    ---
    
    ## 🏗️ Architecture du cluster
    
    ### Config Server Replica Set (cfgRS)
    - `cfg1:27019`
    - `cfg2:27019`
    - `cfg3:27019`
    
    ### Shard 1 Replica Set (shard1RS)
    - `shard1a:27018`
    - `shard1b:27018`
    - `shard1c:27018`
    
    ### Shard 2 Replica Set (shard2RS)
    - `shard2a:27018`
    - `shard2b:27018`
    - `shard2c:27018`
    
    ### Router
    - `mongos:27017` (exposé sur l’hôte : `localhost:27017`)
    
    ---
    
    ## ✅ Prérequis
    
    - Docker Desktop installé
    - Docker Compose (inclus avec Docker Desktop)
    - Un terminal (PowerShell / CMD / Terminal Linux/Mac)
    
    Vérification :
    ```bash
    docker --version
    docker compose version
    

* * *

🚀 Lancement rapide
-------------------

### 1) Démarrer les conteneurs

Dans le dossier du projet :

    docker compose up -d
    

Vérifie que tout est lancé :

    docker ps
    

* * *

⚙️ Initialisation des Replica Sets
----------------------------------

### A) Initialiser les Config Servers (cfgRS)

    docker exec -it cfg1 mongosh --port 27019
    

Puis :

    rs.initiate({
      _id: "cfgRS",
      configsvr: true,
      members: [
        { _id: 0, host: "cfg1:27019" },
        { _id: 1, host: "cfg2:27019" },
        { _id: 2, host: "cfg3:27019" }
      ]
    })
    rs.status()
    

Quitter :

    exit
    

* * *

### B) Initialiser le Shard 1 (shard1RS)

    docker exec -it shard1a mongosh --port 27018
    

    rs.initiate({
      _id: "shard1RS",
      members: [
        { _id: 0, host: "shard1a:27018" },
        { _id: 1, host: "shard1b:27018" },
        { _id: 2, host: "shard1c:27018" }
      ]
    })
    rs.status()
    

    exit
    

* * *

### C) Initialiser le Shard 2 (shard2RS)

    docker exec -it shard2a mongosh --port 27018
    

    rs.initiate({
      _id: "shard2RS",
      members: [
        { _id: 0, host: "shard2a:27018" },
        { _id: 1, host: "shard2b:27018" },
        { _id: 2, host: "shard2c:27018" }
      ]
    })
    rs.status()
    

    exit
    

* * *

🔌 Ajouter les shards au routeur mongos
---------------------------------------

Connexion au `mongos` :

    docker exec -it mongos mongosh --port 27017
    

Ajout des shards :

    sh.addShard("shard1RS/shard1a:27018,shard1b:27018,shard1c:27018")
    sh.addShard("shard2RS/shard2a:27018,shard2b:27018,shard2c:27018")
    sh.status()
    

* * *

🧠 Activer le sharding + sharder une collection
-----------------------------------------------

Toujours dans `mongos` :

### 1) Activer le sharding sur la base

    sh.enableSharding("labdb")
    

### 2) Créer un index sur la clé de sharding

Exemple : sharding par `userId`

    db = db.getSiblingDB("labdb")
    db.users.createIndex({ userId: 1 })
    

### 3) Sharder la collection

    sh.shardCollection("labdb.users", { userId: 1 })
    sh.status()
    

* * *

🧪 Test : insertion + distribution des données
----------------------------------------------

Toujours dans `mongos` :

    db.users.insertMany(
      Array.from({length: 2000}, (_,i)=>({ userId: i, name: "u"+i }))
    )
    
    db.users.countDocuments()
    db.users.getShardDistribution()
    

✅ Tu devrais voir une répartition des documents entre les shards.

* * *

🔍 Commandes utiles
-------------------

### État global du sharding

    sh.status()
    

### Vérifier la distribution

    db.users.getShardDistribution()
    

### Vérifier les chunks

    use config
    db.chunks.find({ ns: "labdb.users" }).limit(5)
    

* * *

🧹 Arrêt et nettoyage
---------------------

### Stopper le cluster

    docker compose down
    

### Stopper + supprimer les volumes (reset total)

    docker compose down -v
    

* * *

🛠️ Dépannage (problèmes fréquents)
-----------------------------------

### 1) `mongos` ne démarre pas / configdb inaccessible

*   Assure-toi d’avoir initialisé `cfgRS` avant de tester `mongos`
    
*   Redémarre :
    

    docker compose restart mongos
    

### 2) `rs.initiate()` échoue

*   Attends 5–10 secondes après `docker compose up -d`
    
*   Vérifie que les conteneurs sont `Up` :
    

    docker ps
    

### 3) Pas de distribution équilibrée

*   Avec une clé **range** `{ userId: 1 }`, la distribution dépend des chunks.
    
*   Tu peux tester une clé **hashed** (meilleure distribution) :
    

    db.users.createIndex({ userId: "hashed" })
    sh.shardCollection("labdb.users", { userId: "hashed" })
    

* * *

📁 Structure du projet (exemple)
--------------------------------

    .
    ├── docker-compose.yml
    └── README.md
    

* * *

📜 Licence
----------

Choisis la licence que tu veux (ex: MIT) et ajoute un fichier `LICENSE`.

* * *

✅ Résultat
----------

Tu obtiens un cluster MongoDB shardé fonctionnel, idéal pour apprendre :

*   Sharding
    
*   Replica Sets
    
*   Haute disponibilité
    
*   Scalabilité horizontale
    

    
    Si tu veux, je peux aussi te donner :
    - un **diagramme Mermaid** de l’architecture (parfait pour ton README),
    - une version **avec authentification (keyFile + users)**,
    - et une version “script automatique” qui initialise tout en une seule commande.

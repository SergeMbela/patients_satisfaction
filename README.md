# patients_satisfaction

Python environement: Anaconda

# 🐳 Installer SQL Server avec Docker sous Windows

Ce dépôt explique comment installer et exécuter **Microsoft SQL Server** dans un conteneur Docker sur **Windows 10/11** avec Docker Desktop.

---
Notre base de données est SQL Server Sous Docker
## 📦 Prérequis

- [Docker Desktop pour Windows](https://www.docker.com/products/docker-desktop)
- Windows 10/11 avec support de la virtualisation
- Droits administrateur
- Connexion internet

---

## ⚙️ Installation étape par étape

 📥 1. Télécharger l’image officielle SQL Server

     docker pull mcr.microsoft.com/mssql/server:2022-latest
🐋 2. Lancer un conteneur SQL Server
    docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=YourStrong!Passw0rd" ^
     -p 1433:1433 --name sqlserver ^
     -d mcr.microsoft.com/mssql/server:2022-latest

 ✅ 3. Vérifier que le conteneur fonctionne
docker ps

🧪 4. Tester la connexion
    docker exec -it sqlserver /opt/mssql-tools/bin/sqlcmd \
  -S localhost -U sa -P "YourStrong!Passw0rd"

    🔹 Option 1 : via Azure Data Studio, DBeaver, ou autre client SQL
  Serveur : localhost
  
  Port : 1433
  
  Utilisateur : sa
  
  Mot de passe : YourStrong!Passw0rd

  docker exec -it sqlserver /opt/mssql-tools/bin/sqlcmd \
  -S localhost -U sa -P "YourStrong!Passw0rd"

  GOODIES
  🧰 Commandes utiles
Action	Commande Docker
Redémarrer	docker start sqlserver
Arrêter	docker stop sqlserver
Supprimer	docker rm -f sqlserver
Voir les logs	docker logs sqlserver
Connexion shell	docker exec -it sqlserver "bash"


# patients_satisfaction

# 🐍 Installer Anaconda sous WSL (Windows Subsystem for Linux)

Ce guide explique comment installer Anaconda dans un environnement WSL (Ubuntu recommandé) sous Windows 10 ou 11.

---

## ✅ Prérequis

- [WSL activé et installé](https://learn.microsoft.com/fr-fr/windows/wsl/install)
- Distribution Ubuntu installée (ex : `Ubuntu 22.04`)
- Connexion Internet
- Terminal WSL (Ubuntu)

---

## ⚙️ Étapes d'installation d'Anaconda sous WSL

### 📥 1. Télécharger le script d'installation d’Anaconda
`bash
wget https://repo.anaconda.com/archive/Anaconda3-2023.09-0-Linux-x86_64.sh
🔧 2. Lancer le script d’installation

bash Anaconda3-2023.09-0-Linux-x86_64.sh
Appuie sur ENTER pour faire défiler la licence.

Tape yes pour accepter la licence.

Laisse le chemin d’installation par défaut (souvent ~/anaconda3).

À la fin, tape yes pour ajouter Anaconda à ton .bashrc.



À la fin, tape yes pour ajouter Anaconda à ton .bashrc.
🔄 3. Activer Anaconda dans le terminal
Ferme et rouvre le terminal ou exécute :

bash
Copier
Modifier
source ~/.bashrc

🚀 4. Vérifier l’installation
bash
Copier
Modifier
conda --version
Tu devrais voir quelque chose comme :

bash
Copier
Modifier
conda 23.9.0


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
    docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=YourStrong!Passw0rd" 
    
     -p 1433:1433 --name sqlserver (ou le port de votre choix au cas ù ce port est déjà utilisé)
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




✅ OVERVIEW
Host: Windows (Docker with SQL Server)

Client: WSL2 (Anaconda, Python, probably Jupyter)

Connection method: Via ODBC or pyodbc, sqlalchemy, or pymssql using the SQL Server's IP/hostname and port

🔧 STEP 1: Ensure Docker SQL Server Is Running
Make sure your SQL Server container is running with proper ports exposed (e.g., 1433):


docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=YourStrong!Passw0rd" \
  -p 1433:1433 --name sqlserver \
  -d mcr.microsoft.com/mssql/server:2022-latest


  🔧 STEP 2: Get Windows Host IP (From WSL)
Run this in WSL to get your Windows host IP:


cat /etc/resolv.conf | grep nameserver | awk '{ print $2 }'
Save the result (e.g., 192.168.1.1 or similar), or use host.docker.internal if supported by your WSL setup (WSL2 may require the IP method).


🔧 STEP 3: Install Required Packages in WSL
Inside WSL:

sudo apt update
sudo apt install unixodbc-dev gcc g++ gnupg curl
conda install -c conda-forge pyodbc


You may also need to install the ODBC Driver for SQL Server:

curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
sudo su
curl https://packages.microsoft.com/config/ubuntu/$(lsb_release -rs)/prod.list > /etc/apt/sources.list.d/mssql-release.list

sudo apt update
sudo ACCEPT_EULA=Y apt install msodbcsql17


🔧 STEP 4: Test Python Connection
In a Python script or notebook:

import pyodbc

server = '192.168.x.x'  # Replace with Windows host IP or try 'host.docker.internal'
database = 'master'
username = 'sa'
password = 'YourStrong!Passw0rd'
connection_string = f"DRIVER={{ODBC Driver 17 for SQL Server}};SERVER={server};DATABASE={database};UID={username};PWD={password}"

conn = pyodbc.connect(connection_string)
cursor = conn.cursor()
cursor.execute("SELECT @@VERSION")
row = cursor.fetchone()
print(row)

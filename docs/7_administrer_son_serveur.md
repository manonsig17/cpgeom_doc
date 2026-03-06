# 📘 Documentation – Installation et gestion de PostgreSQL sur Linux (Ubuntu/Debian)

Cette documentation explique les commandes utilisées pour :

- Installer PostgreSQL
- Vérifier son état
- Modifier sa configuration
- Surveiller le service
- Gérer son démarrage automatique
- Exporter l’historique des commandes

## 1️⃣ Vérification réseau et environnement

```bash
ip a
```

Affiche les interfaces réseau et les adresses IP de la machine.

## 2️⃣ Installation de PostgreSQL

### Mise à jour des paquets

```bash
sudo apt update
sudo apt list
```

- `apt update` → Met à jour la liste des paquets
- `apt list` → Liste les paquets disponibles

### Installation

```bash
sudo apt install postgresql
```

Installe PostgreSQL et ses dépendances.


## 3️⃣ Vérification du service PostgreSQL

```bash
sudo systemctl status postgresql.service
```

Permet de vérifier si le service est actif.

⚠️ Attention :  
`systemectl` est une faute de frappe → utiliser `systemctl`.


## 4️⃣ Connexion à PostgreSQL

### Connexion simple

```bash
psql
```

Connexion avec l’utilisateur système courant.

### Connexion avec utilisateur postgres

```bash
psql -U postgres -d postgres
```

- `-U postgres` → utilisateur postgres
- `-d postgres` → base de données par défaut


## 5️⃣ Configuration – Fichier pg_hba.conf

Le fichier `pg_hba.conf` gère les méthodes d’authentification.

### Sauvegarde du fichier

```bash
sudo cp /etc/postgresql/16/main/pg_hba.conf /etc/postgresql/16/main/pg_hba_ini.conf
```

Crée une copie de sauvegarde.

### Vérification du dossier

```bash
ls -la /etc/postgresql/16/main/
```

### Modification

```bash
sudo nano /etc/postgresql/16/main/pg_hba.conf
```

Modification des règles d’accès (ex: passer de `peer` à `md5`).

### Rechargement de la configuration

```bash
sudo systemctl reload postgresql.service
```

Recharge la configuration sans redémarrer le service.


## 6️⃣ Exploration des fichiers PostgreSQL

### Binaires

```bash
sudo ls -la /usr/lib/postgresql/16/bin/
```

Contient les exécutables (`psql`, `pg_dump`, etc.).

### Librairies

```bash
sudo ls -la /usr/lib/postgresql/16/lib/
```

### Configuration principale

```bash
sudo ls -la /etc/postgresql/16/main/
```

### Fichier start.conf

```bash
sudo less /etc/postgresql/16/main/start.conf
```

Définit le comportement au démarrage.

## 7️⃣ Surveillance des processus

```bash
ps
ps -aux
ps -aux | grep postgres
```

- `ps` → liste des processus
- `ps -aux` → tous les processus
- `grep postgres` → filtre PostgreSQL


## 8️⃣ Surveillance avancée avec htop

Installation :

```bash
sudo apt install htop
```

Lancement :

```bash
htop
```

Permet de surveiller CPU, RAM et processus.

## 9️⃣ Activation / Désactivation au démarrage

### Activer au démarrage

```bash
sudo systemctl enable postgresql
```

### Désactiver

```bash
sudo systemctl disable postgresql
```

### Vérifier

```bash
sudo systemctl status postgresql.service
```

# 🧠 Résumé

| Commande | Rôle |
|----------|------|
| `apt` | Gestion des paquets |
| `systemctl` | Gestion des services |
| `psql` | Client PostgreSQL |
| `pg_hba.conf` | Gestion des accès |
| `ps` / `htop` | Surveillance système |
| `enable` | Démarrage automatique |


# 📌 Bonnes pratiques

- Sauvegarder `pg_hba.conf` avant modification
- Utiliser `reload` plutôt que `restart` si possible
- Vérifier le statut après modification
- Éviter la méthode `trust` en production



# Cours PostgreSQL – Commandes psql (suite)

## 1. Démarrer PostgreSQL

Pour démarrer le serveur PostgreSQL :

```bash
sudo systemctl start postgresql.service
```

Vérifier l’état du service :

```bash
sudo systemctl status postgresql.service
```


# 2. Se connecter à PostgreSQL

Commande :

```bash
psql -U postgres -p 5432
```

Explication :

| Option | Signification |
|------|------|
| psql | client PostgreSQL en ligne de commande |
| -U postgres | utilisateur postgres |
| -p 5432 | port du serveur PostgreSQL |

Exemple :

```bash
psql (16.13)
Type "help" for help.

postgres=#
```

# 3. Lister les tables

Commande :

```sql
\dt
```

Exemple :

```
List of relations
Schema  | Name            | Type  | Owner
public  | spatial_ref_sys | table | postgres
tiger   | addr            | table | postgres
topology| topology        | table | postgres
```

Explication :

| Colonne | Description |
|-------|-------|
| Schema | schéma de la table |
| Name | nom de la table |
| Type | type d’objet |
| Owner | propriétaire |


# 4. Voir les tables d’un schéma

Commande :

```sql
\dt qualite.*
```

Exemple :

```
Schema  | Name                     | Type  | Owner
qualite | 84089_ZONE_URBA_20231012 | table | postgres
qualite | pertuis_mauvais          | table | postgres
```

# 5. Voir les utilisateurs PostgreSQL

Commande :

```sql
\du
```

Exemple :

```
Role name | Attributes
admin     | Superuser, Create role, Create DB
editeur   | Create DB
postgres  | Superuser, Create role, Create DB
```

Explication des attributs :

| Attribut | Signification |
|-------|-------|
| Superuser | droits administrateur |
| Create DB | peut créer des bases |
| Create role | peut créer des utilisateurs |
| Replication | gestion réplication |
| Bypass RLS | ignore la sécurité RLS |


# 6. Lister les bases de données

Commande :

```sql
\l+
```

Exemple :

```
Name     | Owner   | Encoding | Size
postgres | postgres| UTF8     | 19 MB
manon    | postgres| UTF8     | 19 MB
idgeo    | postgres| UTF8     | 16 MB
```

Colonnes importantes :

| Colonne | Description |
|------|------|
| Name | nom de la base |
| Owner | propriétaire |
| Encoding | encodage |
| Size | taille |


# 7. Se connecter à une base

Commande :

```sql
\c manon
```

Résultat :

```
You are now connected to database "manon" as user "postgres".
```

Le prompt devient :

```
manon=#
```


# 8. Quitter psql

Commande :

```sql
\q
```

# 9. Restaurer un dump SQL

Commande :

```bash
psql -U postgres < dump.sql
```

Exemple :

```bash
psql -U postgres < /home/idgeo/dump_all_serveur_distant.sql
```

Explication :

| Élément | Description |
|------|------|
| psql | client PostgreSQL |
| -U postgres | utilisateur |
| < | redirection |
| dump.sql | fichier de sauvegarde |

# 10. Historique des commandes

Exemple :

```bash
589 sudo systemctl start postgresql.service
590 psql -U postgres < /home/idgeo/dump_all_serveur_distant.sql
591 psql -U postgres -p 5432
```

# Résumé des commandes essentielles

| Commande | Fonction |
|------|------|
| psql -U postgres | connexion |
| \l | voir les bases |
| \c nom_base | changer de base |
| \dt | voir les tables |
| \du | voir les utilisateurs |
| \q | quitter |


# Administration PostgreSQL sous Linux – Commandes et explications

Ce document explique les commandes Linux et PostgreSQL utilisées pour :
- explorer les fichiers du serveur PostgreSQL
- se connecter à des bases
- consulter les logs
- configurer PostgreSQL
- gérer les tablespaces
- sauvegarder et restaurer des bases
- automatiser les sauvegardes

---

# 1. Explorer le répertoire de données PostgreSQL

Commande :

```bash
ls -la /mnt/c/Program Files/PostgreSQL/17/data
ls -la /mnt/c/Program Files/PostgreSQL/17/data/
```

Affiche le contenu du **répertoire de données PostgreSQL sous Windows** (via WSL).

Options :

| Option | Signification |
|------|------|
| ls | lister les fichiers |
| -l | format détaillé |
| -a | afficher les fichiers cachés |

Version root :

```bash
sudo ls -la /mnt/c/Program Files/PostgreSQL/17/data/
```

Permet d’accéder aux fichiers protégés.

---

# 2. Répertoires de configuration PostgreSQL

```bash
sudo ls -la /etc/postgresql/16/main/
```

Contient les fichiers principaux :

| Fichier | Description |
|------|------|
| postgresql.conf | configuration du serveur |
| pg_hba.conf | règles d'accès |
| pg_ident.conf | mapping utilisateurs |

---

# 3. Répertoire physique des bases

```bash
sudo ls -la /var/lib/postgresql/16/main/base/
```

Ce dossier contient **les fichiers physiques des bases de données**.

Chaque dossier correspond à une base :

```bash
sudo ls -lah /var/lib/postgresql/16/main/base/5
```

Les fichiers représentent :

- tables
- index
- objets internes

---

# 4. Connexion à un serveur PostgreSQL distant

```bash
psql -h 192.168.10.1 -p 15432 -U editeur -d idgeo
```

Explication :

| Option | Signification |
|------|------|
| -h | adresse du serveur |
| -p | port |
| -U | utilisateur |
| -d | base de données |

Exemple :

```bash
psql -h 192.168.10.1 -p 15432 -U editeur -d manon
```

---

# 5. Gestion du service PostgreSQL

Démarrer le serveur :

```bash
sudo systemctl start postgresql.service
```

Voir le statut :

```bash
sudo systemctl status postgresql.service
```

---

# 6. Connexion locale

```bash
psql postgres postgres -p 5432
```

Structure :

```
psql <database> <user> -p <port>
```

---

# 7. Installation de PostGIS

```bash
sudo apt install postgis
```

PostGIS ajoute :

- types géométriques
- fonctions spatiales
- index spatiaux

Utilisé dans les SIG (QGIS, GeoServer, etc.).

---

# 8. Logs PostgreSQL

Lister les logs :

```bash
ls -la /var/log/postgresql/
```

Afficher les derniers logs :

```bash
tail -f -n 100 /var/log/postgresql/postgresql-16-main.log
```

Options :

| Option | Description |
|------|------|
| tail | afficher fin de fichier |
| -f | suivre en temps réel |
| -n 100 | afficher 100 lignes |

---

# 9. Sauvegarde d’un fichier de configuration

```bash
sudo cp /etc/postgresql/16/main/postgresql.conf \
/etc/postgresql/16/main/ini_postgresql.conf
```

Permet de conserver une **copie de sécurité**.

---

# 10. Modifier la configuration PostgreSQL

```bash
sudo nano /etc/postgresql/16/main/postgresql.conf
```

Exemples de paramètres :

```
shared_buffers
work_mem
max_connections
listen_addresses
```

Recharger la configuration :

```bash
sudo systemctl reload postgresql.service
```

---

# 11. Surveillance système

```bash
htop
```

Permet de voir :

- CPU
- mémoire
- processus PostgreSQL

---

# 12. Création d’un tablespace

Créer le dossier :

```bash
sudo mkdir -p /tablespace
```

Donner les droits :

```bash
sudo chown postgres:postgres /tablespace
```

Déplacer le dossier :

```bash
sudo mv /tablespace /srv/
```

Explorer :

```bash
sudo ls -la /srv/tablespace
```

Structure interne :

```bash
sudo ls -la tablespace/PG_16_202307071
sudo ls -la tablespace/PG_16_202307071/16388
```

---

# 13. Exécuter un script SQL

```bash
psql -U postgres -p 5432 -f /mnt/d/sql/top_14.sql
```

Cela permet :

- créer des tables
- importer des données
- exécuter des scripts


# 14. Connexion à une base

```bash
psql -U postgres -p 5432 -d rugby_top
```

---

# 15. Sauvegarde d’une base

Commande :

```bash
pg_dump -f rugby_sauv.sql -d rugby_top -p 5432 -U postgres
```

Options :

| Option | Description |
|------|------|
| pg_dump | outil de sauvegarde |
| -f | fichier de sortie |
| -d | base |
| -U | utilisateur |


# 16. Création d’une base

```bash
psql -U postgres -p 5432 -c "CREATE DATABASE idgeo_locale;"
```


# 17. Sauvegarde d’une base via redirection

```bash
psql -d idgeo_locale -U postgres -p 5432 > sauv_idgeo.sql
```

Restaurer :

```bash
psql -d idgeo_locale -U postgres -p 5432 < sauv_idgeo.sql
```


# 18. Script de sauvegarde automatique

Création du script :

```bash
nano script_sauv_bbd.sh
```

Donner les droits :

```bash
chmod a+x script_sauv_bbd.sh
```

# 19. Automatisation avec cron

Modifier le planificateur :

```bash
crontab -e
```

Exemple :

```
0 2 * * * /home/user/script_sauv_bbd.sh
```

Cela lance la sauvegarde **tous les jours à 2h du matin**.


# Architecture PostgreSQL

```
/etc/postgresql
    configuration

/var/lib/postgresql
    données physiques

/var/log/postgresql
    logs serveur

/srv/tablespace
    tablespaces personnalisés
```


# Résumé des commandes importantes

| Commande | Fonction |
|------|------|
| systemctl start postgresql | démarrer serveur |
| psql | client PostgreSQL |
| pg_dump | sauvegarde |
| tail log | surveiller logs |
| nano conf | modifier config |
| crontab | automatiser tâches |
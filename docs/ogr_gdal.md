```bash
idgeo@GS6:~$ ogrinfo
Usage: ogrinfo [--help] [--help-general]
               [-if <driver_name>] [-json] [-ro] [-q] [-where <restricted_where>|@f<ilename>]
               [-spat <xmin> <ymin> <xmax> <ymax>] [-geomfield <field>] [-fid <fid>]
               [-sql <statement>|@<filename>] [-dialect <sql_dialect>] [-al] [-rl]
               [-so|-features] [-fields={YES|NO}]]
               [-geom={YES|NO|SUMMARY|WKT|ISO_WKT}] [-oo <NAME>=<VALUE>]...
               [-nomd] [-listmdd] [-mdd {<domain>|all}]...
               [-nocount] [-noextent] [-nogeomtype] [-wkt_format WKT1|WKT2|<other_values>]
               [-fielddomain <name>]
               <datasource_name> [<layer> [<layer> ...]]

FAILURE: No datasource specified.
idgeo@GS6:~$ pwd
/home/idgeo
idgeo@GS6:~$ mkdir cli
idgeo@GS6:~$ cd cli
idgeo@GS6:~/cli$ touch exercice_cli.txt
idgeo@GS6:~/cli$ ls
exercice_cli.txt
idgeo@GS6:~/cli$ echo "Ma nouvelle phrase" >> exercice_cli.txt
idgeo@GS6:~/cli$ cat exercice_cli.txt
Ma nouvelle phrase
idgeo@GS6:~/cli$ mv exercice_cli.txt exercice_cli_initial.txt
idgeo@GS6:~/cli$ ls
exercice_cli_initial.txt
idgeo@GS6:~/cli$ cat exercice_cli_initial.txt
Ma nouvelle phrase
idgeo@GS6:~/cli$ cp exercice_cli_initial.txt exercice_cli_copie.txt
idgeo@GS6:~/cli$ ls
exercice_cli_copie.txt  exercice_cli_initial.txt
idgeo@GS6:~/cli$ ls
exercice_cli_copie.txt  exercice_cli_initial.txt
```



# Prise de note GDAL/OGR

Création de l'arbirescence en mode CLI

```bash
mkdir dossier && cd dossier 
```


## 📘 Cours : Workflow Git, MkDocs, GDAL/OGR et outils CLI
### 1. 🔧 Préparation de l’environnement Linux
Nous avons commencé par mettre à jour le système et installer les outils de base.

```bash
sudo apt update
sudo apt upgrade -y
sudo apt install git python3-venv python3-pip unzip 7zip tree gdal-bin libgdal-dev postgresql-client-16`
```

Ces commandes permettent :

- d’avoir un système à jour,

- d’utiliser Git,

- de créer des environnements Python,

- de manipuler des archives,

- d’utiliser GDAL/OGR,

- d’accéder à PostgreSQL.

## 2. 🗂️ Création d’un dépôt Git et gestion des commits
Initialisation du dépôt
```bash
git init
git add *
git commit -m "premier commit"
```
Nous avons d'abord : 

- initialisé un dépôt Git,

- ajouté les fichiers,

- configuré ton identité Git,

créé plusieurs commits successifs pour suivre l’évolution du projet.

Connexion à GitHub
```bash
git remote add origin https://github.com/manonsig17/cpgeom_doc.git
git branch -M main
git push -u origin main
```
Nous avons relié le dépôt local à GitHub et poussé le travail.

## 3. 📚 Mise en place d’un site avec MkDocs
Création de l’environnement Python
```bash
python3 -m venv .venv
source .venv/bin/activate
pip install mkdocs mkdocs-material
```
Création et lancement du site
```bash
touch mkdocs.yml
mkdocs serve
mkdocs build
mkdocs gh-deploy
```
Nous avons : 

créé un site documentaire,

utilisé le thème Material,

déployé le site sur GitHub Pages.

## 4. 🌐 Manipulation de données VRT avec GDAL/OGR

Clonage du dépôt d’exercices
```bash
git clone https://github.com/pi-geosolutions/formation_VRT.git
```
Exploration des fichiers VRT
```bash
ogrinfo -so -al poly.vrt
ogrinfo locations.vrt
```
Nous avons appris à :

lire la structure d’un fichier VRT,

inspecter les champs et géométries,

convertir un VRT en CSV :

```bash
ogr2ogr -f CSV /vsistdout/ fichier.vrt | head
```
Génération automatique de VRT
```bash
pip install ogr2vrt-simple
ogr2vrt_cli generate-vrt -d -o fichier.vrt fichier.csv
```
## 5. 🗺️ Téléchargement et traitement de données raster
Téléchargement des données
```bash
wget https://data.geopf.fr/.../BDALTIV2...7z
wget https://eu.ftp.opendatasoft.com/.../ORT_2024_1574165_2269250.jp2
```
Extraction
```bash
7z x fichier.7z
```
Construction d’un VRT
```bash
gdalbuildvrt -srcnodata 0 -a_srs EPSG:2154 mnt31.vrt *.asc
```
## 6. ✂️ Découpage et reprojection de données raster
Découpage par département
```bash
ogr2ogr -where "INSEE_DEP='31'" HG.shp DEPARTEMENT.shp
gdalwarp -cutline HG.shp -cl HG -of VRT mnt31.vrt mnt31_decoup.vrt
```
Reprojection d’une orthophoto
```bash
gdalwarp -s_srs EPSG:3857 -t_srs EPSG:2154 -of VRT T31TCJ.tif T31TCJ_2154.vrt
```
## 7. 🧭 Extraction de communes et traitements avancés
Extraction d’une commune
bash
ogr2ogr -where "INSEE_COM='31555'" toulouse.shp COMMUNE.shp
Découpage d’un MNT sur une commune
bash
gdalwarp -cutline COMMUNE.shp -cl COMMUNE -crop_to_cutline mnt31_decoup.vrt mnt31_decoup_lanta.vrt
## 8. 🏔️ Analyses raster : courbes de niveau, ombrage, hypsométrie
Courbes de niveau
```bash
gdal_contour -a elev -i 5 mnt31_decoup.vrt contour_mnt.shp
```
Ombrage
```bash
gdaldem hillshade mnt31_decoup.vrt mnt31_decoup_ombrage.tif
```
Colorisation hypsométrique
```bash
gdaldem color-relief mnt31_decoup.vrt color.txt mnt31_decoup_color.vrt
```
## 9. 📁 Manipulation de fichiers et commandes CLI

création de dossiers (mkdir),

création/édition de fichiers (touch, echo, nano),

copie/déplacement (cp, mv),

inspection (ls, tree, cat).

## 10. 🗄️ Connexion à PostgreSQL
```bash
psql -h localhost -p 5432 -U postgres -d postgres
psql -h 192.168.10.1 -p 15432 -U editeur -d manon
```
Tu as testé la connexion à différentes bases PostgreSQL/PostGIS.
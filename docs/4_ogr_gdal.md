# 🖥️ Manipulation de fichiers en ligne de commande (CLI)

Dans cet exercice, nous utilisons le terminal Linux pour manipuler des fichiers et des dossiers, ainsi que pour tester une commande SIG (`ogrinfo`).

---

## 1️⃣ Test de la commande `ogrinfo`

```bash
ogrinfo
```

La commande affiche son message d’aide (usage), car **aucune source de données (datasource) n’a été spécifiée**.

Message clé :

```
FAILURE: No datasource specified.
```

Cela signifie que `ogrinfo` attend un fichier ou une base de données géospatiale en paramètre (par exemple un Shapefile, GeoJSON, etc.).

---

## 2️⃣ Vérification du répertoire courant

```bash
pwd
```

Résultat :

```
/home/idgeo
```

La commande `pwd` (*print working directory*) affiche le dossier dans lequel on se trouve actuellement.

---

## 3️⃣ Création et navigation dans un dossier

### Création d’un dossier

```bash
mkdir cli
```

`mkdir` permet de créer un nouveau dossier nommé **cli**.

### Se déplacer dans le dossier

```bash
cd cli
```

`cd` (*change directory*) permet d’entrer dans le dossier `cli`.

---

## 4️⃣ Création et modification d’un fichier

### Création d’un fichier vide

```bash
touch exercice_cli.txt
```

`touch` crée un fichier vide nommé `exercice_cli.txt`.

### Vérification avec `ls`

```bash
ls
```

Résultat :

```
exercice_cli.txt
```

`ls` liste les fichiers présents dans le dossier courant.

---

### Ajouter du contenu dans le fichier

```bash
echo "Ma nouvelle phrase" >> exercice_cli.txt
```

`echo` écrit du texte.  
`>>` ajoute le contenu à la fin du fichier (sans écraser le contenu existant).

---

### Afficher le contenu du fichier

```bash
cat exercice_cli.txt
```

Résultat :

```
Ma nouvelle phrase
```

`cat` permet d’afficher le contenu d’un fichier dans le terminal.

---

## 5️⃣ Renommer un fichier

```bash
mv exercice_cli.txt exercice_cli_initial.txt
```

`mv` permet de **renommer** ou **déplacer** un fichier.

Vérification :

```bash
ls
```

Résultat :

```
exercice_cli_initial.txt
```

---

## 6️⃣ Copier un fichier

```bash
cp exercice_cli_initial.txt exercice_cli_copie.txt
```

`cp` permet de **copier un fichier**.

Vérification :

```bash
ls
```

Résultat :

```
exercice_cli_copie.txt  exercice_cli_initial.txt
```

On a maintenant **deux fichiers distincts** contenant le même texte.

---

# ✅ Résumé des commandes utilisées

| Commande | Fonction |
|----------|----------|
| `pwd` | Affiche le dossier courant |
| `mkdir` | Crée un dossier |
| `cd` | Change de dossier |
| `touch` | Crée un fichier vide |
| `ls` | Liste les fichiers |
| `echo >>` | Ajoute du texte dans un fichier |
| `cat` | Affiche le contenu d’un fichier |
| `mv` | Renomme ou déplace un fichier |
| `cp` | Copie un fichier |

---

Cet exercice montre les bases essentielles de la gestion de fichiers en ligne de commande sous Linux.



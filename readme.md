# Mon repos de doc 

Objectif :

- générer un site github pages 
- découvrir l'environnement github 

## Prérequis 

- plusieurs pages chaqu'une correspondra à mes notes de cours sur des thèmes différents 

### 2. Activer / Installer WSL sur son pc (windows 11)
```bash
wsl --install
```
Voir les listes des distributions disponibles
```bash
wsl --list --online
```

Installer une distribution. Conseillée Debian ou Ubuntu
```bash
wsl --install -d Debian
```

Redémarrer l'ordinateur
Ouvrir Terminal Debain ou Ubuntu

Il faut peut-être redémarrer plusieurs fois le PC pour que la mise à jour soit effective.
Si la commande précédente n'a pas installé Ubuntu, il faut passer par le Windows Store pour Obtenir Ubuntu.
Un nouveau redémarrage et votre console ubuntu doit être fonctionnelle.

Warning

Choisir l'utilisateur idgeo et le mdp idgeo

### 3. Installer git
```bash 
sudo apt install git
```

## Git local et Repo distant sur Github

### Association ou création de la passerelle entre un repo local et un repo distant

- Il s'agit d'initier notre répertoire local comme un repository au sens git du terme. Notre répertoire local devient un repository "local".  
`git init`
- Ensuite, on crée notre repository sur la plateforme github. On parle ici de repository distant.  

- Dans un troisième temps, on associe les deux répertoires.
  `git add remote -u origin https://url-de-votre-repo-nouvellement-creer.html`

### Mécanisme de publication des mises à jour

- je mets à jour mon repo local pour le pousser vers le distant
  - j'aoute les fichiers modifiés ou ajoutés avec `git add *`
  - je commit avec un message `git commit -m "modif ..."`
  - je "push" avec git push `git push`

- la publication d'un site github pages s'effectue dans les paramètres du repo sur github. 
  - le plus simple est de désigner un sous dossier du repo, ici "docs" que nous avons judicieusement choisi de créer dans l'arborescence.
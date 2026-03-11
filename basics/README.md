# Shell Basics — Navigation et Gestion de Fichiers

## Concepts clés

Ce module couvre les commandes fondamentales du shell Bash pour naviguer dans le système de fichiers, lister des fichiers et répertoires, et manipuler des fichiers (copie, déplacement, suppression).

---

## 1. Se repérer dans l'arborescence

### `pwd` — Print Working Directory
Affiche le chemin absolu du répertoire courant.
```bash
pwd
# Exemple de sortie : /home/user/projects
```

### `cd` — Change Directory
Permet de se déplacer dans l'arborescence.

| Commande | Effet |
|---|---|
| `cd ~` | Va dans le répertoire home de l'utilisateur |
| `cd -` | Retourne dans le répertoire précédent |
| `cd /chemin/absolu` | Va à un chemin absolu |
| `cd chemin/relatif` | Va à un chemin relatif depuis le répertoire courant |

---

## 2. Lister les fichiers

### `ls` — List
Affiche le contenu d'un répertoire.

| Option | Description |
|---|---|
| `ls` | Liste simple |
| `ls -l` | Format long (permissions, propriétaire, taille, date) |
| `ls -a` | Affiche les fichiers cachés (commençant par `.`) |
| `ls -la` | Long + fichiers cachés |
| `ls -lan` | Long + cachés + UIDs/GIDs numériques |
| `ls -la . .. /boot` | Liste plusieurs répertoires en même temps |

---

## 3. Déterminer le type d'un fichier

### `file` — File type
```bash
file /tmp/iamafile
# Sortie possible : /tmp/iamafile: ASCII text
```
La commande `file` examine le contenu du fichier (pas seulement l'extension) pour déterminer son type réel.

---

## 4. Créer des liens symboliques

### `ln -s` — Symbolic Link
Un lien symbolique est un raccourci pointant vers un autre fichier ou répertoire.
```bash
ln -s /bin/ls __ls__
# Crée __ls__ qui pointe vers /bin/ls
```

---

## 5. Copier, déplacer et supprimer

### `cp` — Copy
```bash
cp -u *.html ..
# Copie les fichiers .html vers le répertoire parent, uniquement si plus récents
```
L'option `-u` (update) ne copie que si la source est plus récente que la destination.

### `mv` — Move
```bash
mv /tmp/betty /tmp/my_first_directory/
# Déplace le fichier betty dans my_first_directory

mv [A-Z]* /tmp/u
# Déplace tous les fichiers dont le nom commence par une majuscule vers /tmp/u
```

### `rm` — Remove
```bash
rm *~               # Supprime les fichiers de sauvegarde Emacs (se terminant par ~)
rm -f fichier       # Suppression forcée, sans confirmation
rm -r repertoire    # Suppression récursive d'un répertoire et son contenu
```

---

## 6. Créer des répertoires

### `mkdir` — Make Directory
```bash
mkdir -p /tmp/my_first_directory
# -p : crée les répertoires parents si nécessaire

mkdir -p welcome/to/school
# Crée l'arborescence welcome/to/school en une seule commande
```

---

## Scripts du module

| Fichier | Commande | Description |
|---|---|---|
| `0-current_working_directory` | `pwd` | Affiche le répertoire courant |
| `1-listit` | `ls` | Liste les fichiers du répertoire courant |
| `2-bring_me_home` | `cd ~` | Retourne dans le home |
| `3-listfiles` | `ls -l` | Liste détaillée |
| `4-listmorefiles` | `ls -la` | Liste détaillée avec fichiers cachés |
| `5-listfilesdigitonly` | `ls -lan` | Liste avec UIDs/GIDs numériques |
| `6-firstdirectory` | `mkdir -p /tmp/my_first_directory` | Crée un répertoire dans /tmp |
| `7-movethatfile` | `mv /tmp/betty /tmp/my_first_directory/` | Déplace un fichier |
| `8-firstdelete` | `rm -f /tmp/my_first_directory/betty` | Supprime un fichier |
| `9-firstdirdeletion` | `rm -r /tmp/my_first_directory` | Supprime un répertoire |
| `10-back` | `cd -` | Retourne dans le répertoire précédent |
| `11-lists` | `ls -la . .. /boot` | Liste plusieurs répertoires |
| `12-file_type` | `file /tmp/iamafile` | Détermine le type d'un fichier |
| `13-symbolic_link` | `ln -s /bin/ls __ls__` | Crée un lien symbolique |
| `14-copy_html` | `cp -u *.html ..` | Copie les .html vers le parent si plus récents |
| `15-lets_move` | `mv [A-Z]* /tmp/u` | Déplace les fichiers commençant par une majuscule |
| `16-clean_emacs` | `rm *~` | Supprime les fichiers de sauvegarde Emacs |
| `17-tree` | `mkdir -p welcome/to/school` | Crée une arborescence de répertoires |

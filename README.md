# Holberton School — Cours Shell

Bienvenue dans ce dépôt consacré à l'apprentissage du shell Unix/Linux.  
Ce dépôt est organisé en quatre modules, chacun couvrant un aspect essentiel de la ligne de commande Bash.

---

## Structure du dépôt

```
holbertonschool-shell/
├── basics/                             # Navigation et gestion de fichiers
├── permissions/                        # Droits et propriétaires de fichiers
├── init_files_variables_and_expansions/ # Variables, alias et expansions
└── io_redirections_and_filters/        # Redirections, pipes et filtres
```

---

## Module 1 — Basics : Navigation et Gestion de Fichiers

> Dossier : [`basics/`](./basics/README.md)

Ce module introduit les commandes fondamentales pour se déplacer dans l'arborescence et manipuler fichiers et répertoires.

### Commandes essentielles

| Commande | Rôle |
|---|---|
| `pwd` | Affiche le répertoire courant (Print Working Directory) |
| `ls` | Liste le contenu d'un répertoire |
| `cd` | Change de répertoire |
| `mkdir` | Crée un répertoire |
| `cp` | Copie un fichier ou répertoire |
| `mv` | Déplace ou renomme un fichier |
| `rm` | Supprime un fichier ou répertoire |
| `ln -s` | Crée un lien symbolique |
| `file` | Détermine le type d'un fichier |

### Points clés
- `cd ~` retourne au home, `cd -` retourne au répertoire précédent
- `ls -la` affiche les fichiers cachés (commençant par `.`) en format long
- `ls -lan` remplace UIDs/GIDs par leurs valeurs numériques
- `mkdir -p` crée toute l'arborescence intermédiaire en une commande
- `cp -u` ne copie que les fichiers plus récents que la destination
- Les globbing patterns comme `[A-Z]*` permettent de cibler des fichiers par leur nom

---

## Module 2 — Permissions : Droits Linux

> Dossier : [`permissions/`](./permissions/README.md)

Ce module couvre le système de permissions Unix (lecture, écriture, exécution) et leur gestion pour les utilisateurs, groupes et autres.

### Comprendre les permissions

Chaque fichier possède trois niveaux de permissions :
- **u** (user/propriétaire) — **g** (group/groupe) — **o** (others/autres)

Chaque niveau a trois types de droits :
- **r** = lecture (4), **w** = écriture (2), **x** = exécution (1)

```
-rwxr-xr--  →  propriétaire : rwx (7), groupe : r-x (5), autres : r-- (4)
```

### Commandes essentielles

| Commande | Rôle |
|---|---|
| `chmod` | Modifie les permissions |
| `chown` | Change le propriétaire (et le groupe) |
| `chgrp` | Change le groupe |
| `whoami` | Affiche l'utilisateur courant |
| `groups` | Affiche les groupes de l'utilisateur |
| `su` | Change d'utilisateur |

### Points clés
- Notation symbolique : `chmod u+x,g+r fichier`
- Notation octale : `chmod 755 fichier`
- `chmod --reference=ref cible` copie les permissions d'un fichier sur un autre
- `chmod -R a+X .` ajoute récursivement l'exécution sur les **répertoires** pour tous
- `chown -h` modifie le propriétaire d'un **lien symbolique** (pas sa cible)
- `chown --from=ancien nouveau fichier` effectue un changement conditionnel

---

## Module 3 — Variables, Alias et Expansions

> Dossier : [`init_files_variables_and_expansions/`](./init_files_variables_and_expansions/README.md)

Ce module explore les mécanismes d'initialisation du shell, la gestion des variables et les différentes formes d'expansion.

### Fichiers d'initialisation
- `~/.bashrc` — chargé à chaque ouverture de terminal interactif
- `~/.bash_profile` — chargé à la connexion
- Ces fichiers définissent aliases, variables et fonctions persistants

### Variables

| Type | Définition | Visibilité |
|---|---|---|
| Locale | `BEST="School"` | Shell courant seulement |
| Globale | `export BEST="School"` | Shell courant + sous-processus |

```bash
printenv    # Affiche les variables d'environnement (globales)
set         # Affiche toutes les variables (locales + globales + fonctions)
```

### Alias
```bash
alias ls='rm -f *'   # Remplace la commande ls par une autre
```
> ⚠️ Les alias peuvent redéfinir n'importe quelle commande, même dangereusement !

### Expansions
```bash
$((expression))       # Expansion arithmétique
$((2#1010))           # Conversion binaire → décimal : 10
printf "%X\n" 255     # Conversion décimal → hexadécimal : FF
{a..z}{a..z}          # Expansion d'accolades : toutes les combinaisons de 2 lettres
```

---

## Module 4 — I/O Redirections et Filtres

> Dossier : [`io_redirections_and_filters/`](./io_redirections_and_filters/README.md)

Ce module couvre l'affichage, la manipulation de fichiers texte, les redirections d'entrées/sorties et les outils de filtrage.

### Les trois flux standards

| Flux | N° | Description |
|---|---|---|
| `stdin` | 0 | Entrée (clavier par défaut) |
| `stdout` | 1 | Sortie (terminal par défaut) |
| `stderr` | 2 | Erreurs |

### Redirections

| Opérateur | Effet |
|---|---|
| `>` | Redirige stdout vers un fichier (écrase) |
| `>>` | Ajoute à la fin d'un fichier |
| `\|` | Pipe : envoie la sortie comme entrée de la commande suivante |

### Commandes essentielles

| Commande | Rôle |
|---|---|
| `echo` | Affiche du texte |
| `cat` | Affiche le contenu d'un fichier |
| `head` / `tail` | Premières / dernières lignes |
| `sort` | Trie les lignes |
| `uniq` | Élimine les doublons (sur données triées) |
| `grep` | Recherche par expression régulière |
| `tr` | Remplace ou supprime des caractères |
| `cut` | Extrait des colonnes d'un texte |
| `rev` | Inverse les lignes |
| `find` | Recherche de fichiers |
| `wc` | Compte lignes, mots, caractères |

### Points clés
- Le pipe `|` est au cœur de la philosophie Unix : chaîner des outils simples
- `grep -v` inverse la sélection (lignes qui ne correspondent PAS)
- `sort | uniq -u` extrait les lignes n'apparaissant qu'une seule fois
- `cut -d: -f1,6` extrait les colonnes 1 et 6 séparées par `:`
- `tr -d 'abc'` supprime les caractères a, b, c du flux

---

## Philosophie Unix

Ces modules illustrent la philosophie Unix :
> **"Faire une chose, et la faire bien."**

Chaque commande est un outil simple et spécialisé. C'est leur combinaison via les pipes et les redirections qui crée des traitements puissants. Maîtriser ces outils, c'est maîtriser le shell.

---

## Ressources utiles

- `man <commande>` — Manuel de n'importe quelle commande
- `<commande> --help` — Aide rapide
- [The Linux Command Line (William Shotts)](https://linuxcommand.org/tlcl.php) — Référence complète et gratuite

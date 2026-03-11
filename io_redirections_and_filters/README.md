# Shell I/O Redirections et Filtres

## Concepts clés

Ce module couvre l'affichage de texte, la lecture de fichiers, la redirection des entrées/sorties, et les outils de filtrage et de traitement de texte en ligne de commande.

---

## 1. Afficher du texte : `echo`

```bash
echo "Hello, World"   # Affiche Hello, World suivi d'un saut de ligne
echo "\"(Ôo)'"        # Affiche des caractères spéciaux avec échappement
```

Les guillemets doubles permettent les substitutions de variables.  
Les guillemets simples désactivent toute interprétation.

---

## 2. Lire des fichiers : `cat`, `head`, `tail`

### `cat` — Concatenate
```bash
cat /etc/passwd             # Affiche tout le fichier
cat /etc/passwd /etc/hosts  # Affiche les deux fichiers à la suite
```

### `head` — Premières lignes
```bash
head /etc/passwd     # Affiche les 10 premières lignes (par défaut)
head -n 3 fichier    # Affiche les 3 premières lignes
```

### `tail` — Dernières lignes
```bash
tail /etc/passwd     # Affiche les 10 dernières lignes
tail -n 1 fichier    # Affiche uniquement la dernière ligne
```

### Combiner head et tail pour isoler une ligne
```bash
head -n 3 iacta | tail -n 1
# Prend les 3 premières lignes, puis garde seulement la dernière = ligne 3
```

---

## 3. Redirections

### Rediriger la sortie vers un fichier

| Opérateur | Effet |
|---|---|
| `>` | Écrit dans un fichier (écrase le contenu existant) |
| `>>` | Ajoute à la fin d'un fichier (append) |

```bash
ls -la > ls_cwd_content         # Sauvegarde la liste dans un fichier
echo "Best School" > fichier    # Crée/écrase fichier avec ce texte
tail -n 1 iacta >> iacta        # Duplique la dernière ligne du fichier
```

### Les flux standards

| Flux | Numéro | Description |
|---|---|---|
| `stdin` | 0 | Entrée standard (clavier par défaut) |
| `stdout` | 1 | Sortie standard (terminal par défaut) |
| `stderr` | 2 | Sortie d'erreur standard |

---

## 4. Rechercher des fichiers : `find`

```bash
find . -type f -name "*.js" -delete
# Cherche tous les fichiers .js et les supprime

find . -mindepth 1 -type d | wc -l
# Compte le nombre de sous-répertoires (au moins 1 niveau de profondeur)
```

---

## 5. Trier et filtrer les fichiers récents : `ls` + `head`

```bash
ls -1t | head -n 10
# -1 : un fichier par ligne
# -t : trie par date de modification (plus récent d'abord)
# head -n 10 : garde seulement les 10 premiers
```

---

## 6. Filtrer des lignes : `sort`, `uniq`, `grep`

### `sort` — Trier
```bash
sort fichier         # Trie les lignes alphabétiquement
```

### `uniq` — Lignes uniques
```bash
sort | uniq -u
# Trie puis affiche seulement les lignes qui n'apparaissent qu'une fois
```
> `uniq` doit recevoir des données triées pour fonctionner correctement.

### `grep` — Recherche par pattern
```bash
grep "root" /etc/passwd       # Affiche les lignes contenant "root"
grep -c "bin" /etc/passwd     # Compte le nombre de lignes contenant "bin"
grep -A 3 "root" /etc/passwd  # Affiche les lignes contenant "root" + 3 lignes après
grep -v "bin" /etc/passwd     # Affiche les lignes NE contenant PAS "bin"
grep "^[A-Za-z]" /etc/ssh/sshd_config  # Lignes commençant par une lettre
```

| Option grep | Description |
|---|---|
| `-c` | Compte les lignes correspondantes |
| `-v` | Inverse : lignes qui ne correspondent PAS |
| `-A n` | Affiche n lignes après chaque correspondance |
| `-B n` | Affiche n lignes avant chaque correspondance |
| `-i` | Insensible à la casse |

---

## 7. Transformer du texte : `tr`, `rev`, `cut`

### `tr` — Translate (remplacer/supprimer des caractères)
```bash
tr 'Ac' 'Ze'    # Remplace 'A' par 'Z' et 'c' par 'e'
tr -d 'cC'      # Supprime tous les 'c' et 'C' du flux
```

### `rev` — Reverse
```bash
rev
# Inverse chaque ligne de l'entrée standard
# "hello" → "olleh"
```

### `cut` — Extraire des colonnes
```bash
cut -d: -f1,6 /etc/passwd | sort
# -d: : délimiteur = ':'
# -f1,6 : extraire les colonnes 1 et 6 (nom d'utilisateur et home)
# sort : trier le résultat
```

---

## 8. Le pipe `|`

Le pipe envoie la sortie d'une commande comme entrée de la suivante.
```bash
commande1 | commande2 | commande3
```

Exemple :
```bash
find . -mindepth 1 -type d | wc -l
# find produit une liste de répertoires → wc -l compte les lignes
```

---

## Scripts du module

| Fichier | Commande | Description |
|---|---|---|
| `0-hello_world` | `echo "Hello, World"` | Affiche Hello, World |
| `1-confused_smiley` | `echo "\"(Ôo)'"` | Affiche un smiley avec caractères spéciaux |
| `2-hellofile` | `cat /etc/passwd` | Affiche le contenu de /etc/passwd |
| `3-twofiles` | `cat /etc/passwd /etc/hosts` | Affiche deux fichiers à la suite |
| `4-lastlines` | `tail /etc/passwd` | Affiche les 10 dernières lignes |
| `5-firstlines` | `head /etc/passwd` | Affiche les 10 premières lignes |
| `6-third_line` | `head -n 3 iacta \| tail -n 1` | Affiche la 3ème ligne du fichier iacta |
| `7-file` | `echo "Best School" > ...` | Écrit dans un fichier au nom spécial |
| `8-cwd_state` | `ls -la > ls_cwd_content` | Sauvegarde le listing dans un fichier |
| `9-duplicate_last_line` | `tail -n 1 iacta >> iacta` | Duplique la dernière ligne |
| `10-no_more_js` | `find . -type f -name "*.js" -delete` | Supprime tous les fichiers .js |
| `11-directories` | `find . -mindepth 1 -type d \| wc -l` | Compte les sous-répertoires |
| `12-newest_files` | `ls -1t \| head -n 10` | Affiche les 10 fichiers les plus récents |
| `13-unique` | `sort \| uniq -u` | Affiche les lignes uniques |
| `14-findthatword` | `grep "root" /etc/passwd` | Lignes contenant "root" |
| `15-countthatword` | `grep -c "bin" /etc/passwd` | Compte les lignes avec "bin" |
| `16-whatsnext` | `grep -A 3 "root" /etc/passwd` | Lignes "root" + 3 suivantes |
| `17-hidethisword` | `grep -v "bin" /etc/passwd` | Lignes sans "bin" |
| `18-letteronly` | `grep "^[A-Za-z]" /etc/ssh/sshd_config` | Lignes commençant par une lettre |
| `19-AZ` | `tr 'Ac' 'Ze'` | Remplace A→Z et c→e |
| `20-hiago` | `tr -d 'cC'` | Supprime les 'c' et 'C' |
| `21-reverse` | `rev` | Inverse chaque ligne |
| `22-users_and_homes` | `cut -d: -f1,6 /etc/passwd \| sort` | Noms et homes des utilisateurs, triés |

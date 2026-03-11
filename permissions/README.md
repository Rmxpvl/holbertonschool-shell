# Shell Permissions — Gestion des Droits Linux

## Concepts clés

Sous Linux, chaque fichier et répertoire possède des **permissions** définissant qui peut le lire, l'écrire ou l'exécuter. Ce module explore les commandes pour visualiser, modifier les permissions, et changer les propriétaires de fichiers.

---

## 1. Comprendre les permissions Linux

### La notation symbolique

```
-rwxr-xr--  1  user  group  1234  Jan 1 12:00  fichier
```

| Champ | Signification |
|---|---|
| `-` | Type : `-` fichier, `d` répertoire, `l` lien symbolique |
| `rwx` | Permissions du **propriétaire** (user) |
| `r-x` | Permissions du **groupe** |
| `r--` | Permissions des **autres** (others) |

### Les permissions rwx

| Lettre | Valeur octal | Signification sur un fichier | Sur un répertoire |
|---|---|---|---|
| `r` | 4 | Lire le fichier | Lister le contenu |
| `w` | 2 | Modifier le fichier | Créer/supprimer des fichiers |
| `x` | 1 | Exécuter le fichier | Traverser (entrer dans) le répertoire |
| `-` | 0 | Permission refusée | Permission refusée |

### La notation octale

On additionne les valeurs pour chaque catégorie :
- `7` = `rwx` (4+2+1)
- `5` = `r-x` (4+0+1)
- `3` = `-wx` (0+2+1)
- `0` = `---`

Exemple : `chmod 753 hello` → propriétaire `rwx`, groupe `r-x`, autres `-wx`

---

## 2. Changer les permissions : `chmod`

### Notation symbolique
```bash
chmod u+x hello          # Ajoute l'exécution au propriétaire (user)
chmod g+x hello          # Ajoute l'exécution au groupe
chmod o+r hello          # Ajoute la lecture aux autres
chmod u+x,g+x,o+r hello  # Plusieurs changements à la fois
chmod a+x hello          # Ajoute l'exécution à tous (all)
chmod -R a+X .           # Récursivement, exécution sur les répertoires pour tous
```

**Cibles** : `u` (user/propriétaire), `g` (group/groupe), `o` (others/autres), `a` (all/tous)  
**Opérateurs** : `+` (ajouter), `-` (retirer), `=` (définir exactement)

### Notation octale
```bash
chmod 007 hello   # --- --- rwx : seulement les autres ont tous les droits
chmod 753 hello   # rwx r-x -wx
chmod 751 hello   # rwx r-x --x
```

### Copier les permissions d'un fichier de référence
```bash
chmod --reference=olleh hello
# hello reçoit exactement les mêmes permissions que olleh
```

### Créer un répertoire avec des permissions spécifiques
```bash
mkdir -m 751 my_dir
# Crée my_dir avec permissions 751 (rwxr-x--x)
```

---

## 3. Changer le propriétaire : `chown`

```bash
chown betty hello                  # Change le propriétaire de hello à betty
chown -R vincent:staff .           # Change propriétaire ET groupe récursivement
chown -h vincent:staff _hello      # Change le propriétaire d'un lien symbolique
chown --from=guillaume vincent hello  # Change seulement si l'actuel propriétaire est guillaume
```

---

## 4. Changer le groupe : `chgrp`

```bash
chgrp school hello
# Change le groupe propriétaire de hello en school
```

---

## 5. Connaître l'utilisateur courant

### `whoami` — Qui suis-je ?
```bash
whoami
# Affiche le nom de l'utilisateur courant, ex : betty
```

### `groups` — Groupes d'un utilisateur
```bash
groups
# Affiche tous les groupes auxquels appartient l'utilisateur courant
```

### `su` — Switch User
```bash
su betty
# Ouvre un shell en tant que l'utilisateur betty
```

---

## Scripts du module

| Fichier | Commande | Description |
|---|---|---|
| `0-iam_betty` | `su betty` | Se connecte en tant que betty |
| `1-who_am_i` | `whoami` | Affiche l'utilisateur courant |
| `2-groups` | `groups` | Affiche les groupes de l'utilisateur courant |
| `3-new_owner` | `chown betty hello` | Change le propriétaire de hello en betty |
| `4-empty` | `touch hello` | Crée un fichier vide nommé hello |
| `5-execute` | `chmod u+x hello` | Donne l'exécution au propriétaire |
| `6-multiple_permissions` | `chmod u+x,g+x,o+r hello` | Plusieurs permissions en une commande |
| `7-everybody` | `chmod a+x ./permissions/hello` | Exécution pour tout le monde |
| `8-James_Bond` | `chmod 007 hello` | Tous les droits seulement pour les autres |
| `9-John_Doe` | `chmod 753 hello` | Permissions 753 |
| `10-mirror_permissions` | `chmod --reference=olleh hello` | Copie les permissions de olleh sur hello |
| `11-directories_permissions` | `chmod -R a+X .` | Exécution récursive sur les répertoires |
| `12-directory_permissions` | `mkdir -m 751 my_dir` | Crée un répertoire avec permissions 751 |
| `13-change_group` | `chgrp school hello` | Change le groupe de hello en school |
| `14-change_owner_and_group` | `chown -R vincent:staff .` | Change propriétaire et groupe récursivement |
| `15-symbolic_link_permissions` | `chown -h vincent:staff _hello` | Change le propriétaire d'un lien symbolique |
| `16-if_only` | `chown --from=guillaume vincent hello` | Change le propriétaire sous condition |

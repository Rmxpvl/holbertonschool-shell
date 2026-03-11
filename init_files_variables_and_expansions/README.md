# Shell Init Files, Variables et Expansions

## Concepts clés

Ce module explore les fichiers d'initialisation du shell, les variables locales et globales (d'environnement), et les mécanismes d'expansion comme l'arithmétique, les accolades et les conversions de bases numériques.

---

## 1. Les fichiers d'initialisation du shell

Lorsque Bash démarre, il charge automatiquement des fichiers de configuration :

| Fichier | Chargé quand |
|---|---|
| `/etc/profile` | Shell de connexion, pour tous les utilisateurs |
| `~/.bash_profile` | Shell de connexion, pour l'utilisateur courant |
| `~/.bashrc` | Shell interactif non-connexion (ex : nouveau terminal) |
| `~/.bash_aliases` | Souvent inclus depuis `.bashrc` pour les alias |
| `~/.bash_logout` | A la déconnexion |

Ces fichiers permettent de définir des variables, des alias et des fonctions qui seront disponibles à chaque session.

---

## 2. Les alias

Un alias remplace une commande par une autre.
```bash
alias ls='rm -f *'
# ATTENTION : très dangereux ! ls va maintenant supprimer tous les fichiers
```

Pour créer un alias utile :
```bash
alias ll='ls -la'
alias ..='cd ..'
```

Pour le supprimer : `unalias ll`

---

## 3. Variables locales et globales

### Variable locale
N'existe que dans le shell courant, pas transmise aux sous-processus.
```bash
BEST="School"
echo $BEST   # affiche : School
```

### Variable globale (d'environnement)
Transmise aux processus enfants grâce à `export`.
```bash
export BEST="School"
```

### Afficher les variables
```bash
printenv         # Affiche uniquement les variables d'environnement (globales)
set              # Affiche TOUTES les variables : locales, globales, et fonctions
```

---

## 4. La variable `PATH`

`PATH` est une variable d'environnement contenant la liste des répertoires où le shell cherche les exécutables.

```bash
echo $PATH
# Sortie : /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

# Ajouter un répertoire au PATH
export PATH="$PATH:/action"

# Compter le nombre de répertoires dans PATH
echo "$PATH" | tr ':' '\n' | grep -v '^$' | wc -l
```

---

## 5. Les expansions du shell

### Expansion arithmétique `$(( ))`
Évalue des expressions mathématiques.
```bash
echo $((128 + TRUEKNOWLEDGE))   # Additionne 128 à la variable TRUEKNOWLEDGE
echo $((POWER / DIVIDE))        # Divise POWER par DIVIDE
echo $((BREATH ** LOVE))        # BREATH à la puissance LOVE
```

### Conversion de bases numériques
```bash
# Binaire vers décimal : préfixe 2#
echo $((2#$BINARY))    # Convertit la valeur binaire de $BINARY en décimal

# Décimal vers hexadécimal : printf avec %X
printf "%X\n" "$DECIMAL"
```

### Affichage formaté : `printf`
```bash
printf "%.2f\n" "$NUM"      # Affiche un nombre avec 2 décimales
printf "%X\n" "$DECIMAL"    # Affiche en hexadécimal majuscule
```

### Expansion d'accolades `{ }`
Génère des séquences ou des combinaisons.
```bash
printf "%s\n" {a..z}{a..z} | grep -v ^oo$
# Génère toutes les combinaisons de 2 lettres minuscules, sauf "oo"
# aa, ab, ac, ..., zz  (sans oo)
```

---

## 6. La variable `USER`

`USER` est une variable d'environnement automatiquement définie avec le nom de l'utilisateur courant.
```bash
echo "hello $USER"
# Sortie : hello betty  (si connecté en tant que betty)
```

---

## Scripts du module

| Fichier | Commande | Description |
|---|---|---|
| `0-alias` | `alias ls='rm -f *'` | Crée un alias (dangereux !) |
| `1-hello_you` | `echo "hello $USER"` | Affiche un message avec le nom d'utilisateur |
| `2-path` | `export PATH="$PATH:/action"` | Ajoute /action au PATH |
| `3-paths` | `echo "$PATH" \| tr ':' '\n' \| ... \| wc -l` | Compte les répertoires dans PATH |
| `4-global_variables` | `printenv` | Affiche les variables d'environnement |
| `5-local_variables` | `set` | Affiche toutes les variables |
| `6-create_local_variable` | `BEST="School"` | Crée une variable locale |
| `7-create_global_variable` | `export BEST="School"` | Crée une variable globale |
| `8-true_knowledge` | `echo $((128 + TRUEKNOWLEDGE))` | Addition arithmétique |
| `9-divide_and_rule` | `echo $((POWER / DIVIDE))` | Division arithmétique |
| `10-love_exponent_breath` | `echo $((BREATH ** LOVE))` | Exponentiation |
| `11-binary_to_decimal` | `echo $((2#$BINARY))` | Conversion binaire → décimal |
| `12-combinations` | `printf "%s\n" {a..z}{a..z} \| grep -v ^oo$` | Combinaisons de lettres |
| `13-print_float` | `printf "%.2f\n" "$NUM"` | Affiche un flottant à 2 décimales |
| `14-decimal_to_hexadecimal` | `printf "%X\n" "$DECIMAL"` | Conversion décimal → hexadécimal |

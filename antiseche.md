---
title: "L'antisèche"
nav_order: 80
published: true
---

# L'antisèche
{: .no_toc }

Les commandes du module, avec ce qu'elles font vraiment.
Cette page grandit au fil des séances.
{: .fs-5 .fw-300 }

---

## Se repérer dans le terminal

| Commande | Ce qu'elle fait |
|---|---|
| `pwd` | Affiche où je suis |
| `ls` | Liste ce qu'il y a ici |
| `ls -a` | Liste tout, y compris les fichiers cachés |
| `cd dossier` | Entre dans un dossier |
| `cd ..` | Remonte d'un dossier |

Touche **Tab** : complète un nom de fichier.
Flèche **haut** : rappelle la commande précédente.
Touche **q** : sort d'un affichage bloqué.

---

## Se présenter à Git

Une seule fois par machine.

| Commande | Ce qu'elle fait |
|---|---|
| `git config --global user.name "Nom"` | Je dis qui je suis |
| `git config --global user.email "adresse"` | Je donne mon adresse |
| `git config --global --list` | Je vérifie ma configuration |

---

## Le cycle de base

| Commande | En français | Zone concernée |
|---|---|---|
| `git init` | Je crée un dépôt ici | Crée le dossier `.git` |
| `git status` | Où en est chaque fichier ? | Les trois |
| `git add fichier` | Je cadre ce fichier | Travail → Attente |
| `git add .` | Je cadre tout ce qui a changé | Travail → Attente |
| `git commit -m "message"` | Je déclenche la photo | Attente → Dépôt |
| `git diff` | Qu'ai-je changé depuis la dernière fois ? | Travail |
| `git log --oneline` | Montre-moi l'historique | Dépôt |

---

## Le chemin d'un fichier

```
Répertoire de travail  --git add-->  Zone d'attente  --git commit-->  Dépôt
```

| État | Ce que Git en dit | Ce que ça signifie |
|---|---|---|
| Non suivi | `Untracked` | Git ne le connaît pas encore |
| Modifié | `Changes not staged` | Changé, pas encore cadré |
| Indexé | `Changes to be committed` | Cadré, prêt pour la photo |
| Validé | `working tree clean` | Enregistré, en sécurité |

---

## Ignorer des fichiers

On écrit les noms à ignorer dans le fichier `.gitignore`, un par ligne.

| Ligne écrite | Ce qui est ignoré |
|---|---|
| `notes.txt` | Ce fichier précis |
| `*.log` | Tous les fichiers finissant par `.log` |
| `brouillons/` | Tout le contenu de ce dossier |

`.gitignore` n'agit que sur les fichiers **pas encore suivis**.

---

## Les trois réflexes

1. **`git status` avant et après chaque commande.** Toujours.
2. **Lire le message d'erreur en entier.** Git écrit très souvent la
   solution dans sa réponse.
3. **Un commit = une idée.** Si le message contient « et », c'est qu'il
   en fallait deux.

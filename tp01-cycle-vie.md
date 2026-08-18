---
title: "TP1 · Cycle de vie des fichiers"
nav_order: 3
published: true
---

# TP1 — Le cycle de vie des fichiers
{: .no_toc }

<details open markdown="block">
  <summary>Sommaire</summary>
  {: .text-delta }
- TOC
{:toc}
</details>

---

## Objectifs

À la fin de ce TP, vous saurez :

- transformer un dossier ordinaire en dépôt Git ;
- lire ce que `git status` vous répond ;
- faire passer un fichier du répertoire de travail au dépôt ;
- demander à Git d'ignorer certains fichiers.

**Durée : 2 h 15.** Tâches T01, T02 et T03.

{: .note }
> Une seule règle aujourd'hui : **tapez `git status` avant et après chaque
> commande.** C'est ainsi que vous verrez les trois zones fonctionner,
> au lieu de me croire sur parole.

---

## Étape 1 — Récupérer le projet

Rendez-vous sur le dépôt du projet :

[github.com/liliasfaxi/guide-survie](https://github.com/liliasfaxi/guide-survie){: .btn }

Cliquez sur le bouton vert **Code**, puis sur **Download ZIP**.
Décompressez l'archive dans un dossier facile à retrouver — par exemple
votre bureau.

{: .warning }
> Le dossier obtenu s'appelle `guide-survie-main`. **Renommez-le en
> `guide-survie`** (clic droit → Renommer). Vous vous éviterez des
> chemins pénibles à taper pendant tout le semestre.

### Vérifier que le site fonctionne

Double-cliquez sur `index.html`. Le site doit s'ouvrir dans votre
navigateur, avec six fiches de conseils. Si vous le voyez, tout est
prêt.

{: .note }
> Nous téléchargeons une archive aujourd'hui. En séance 4, vous
> apprendrez `git clone`, qui fait la même chose en mieux. Vous
> comprendrez alors précisément ce qui manque à un ZIP.

---

## Étape 2 — Se placer dans le projet

Ouvrez un terminal, et déplacez-vous dans le dossier du projet.
Adaptez le chemin à votre machine :

```
cd <chemin-de-votre-projet>/guide-survie
```

{: .note }
> Dans Windows, Le plus simple, c'est d'ouvrir l'explorateur des fichiers dans le répertoire contenant le dossier `guide-survie`, faire un clic droit sur celui-ci, et cliquer sur `Open Git Bash here`. Le terminal s'ouvrira directement dans le dossier du projet.

Vérifiez que vous êtes au bon endroit :

```
pwd
```

```
ls
```

Vous devez voir `index.html`, `README.md`, ainsi que les dossiers
`css`, `js` et `data`.

{: .warning }
> Si `ls` n'affiche pas ces fichiers, vous n'êtes pas dans le bon
> dossier. Ne continuez pas : tout le TP en dépend.
>
> Utilisez la touche **Tab** pour compléter les noms de dossiers,
> vous éviterez les fautes de frappe.

---

## Étape 3 — Créer le dépôt

```
git init
```

Git répond quelque chose comme
*Initialized empty Git repository in .../guide-survie/.git/*.

Il vient de créer un dossier caché `.git` à la racine du projet.
**Ce dossier est le dépôt** : c'est là que vivra tout l'historique.

Vérifiez sa présence :

```
ls -a
```

L'option `-a` affiche aussi les fichiers cachés. Vous devez voir `.git`
dans la liste.

{: .warning }
> Ne modifiez jamais le contenu du dossier `.git` à la main, et ne le
> supprimez pas : vous perdriez tout l'historique du projet.

---

## Étape 4 — Votre premier `git status`

```
git status
```

Git vous répond à peu près ceci :

<div class="highlighter-rouge"><div class="highlight"><pre class="highlight"><code>On branch main

No commits yet

Untracked files:
  (use "git add &lt;file&gt;..." to include in what will be committed)
<span style="color:#C41E3A">        .gitignore
        README.md
        css/
        data/
        defis.md
        images/
        index.html
        js/
        outils/</span>

nothing added to commit but untracked files present (use "git add" to track)</code>
</pre></div></div>

### Ce que ça veut dire

| Ce que Git écrit | Traduction |
|---|---|
| `No commits yet` | L'historique est vide, vous n'avez rien enregistré |
| `Untracked files` | Ces fichiers existent, mais Git ne les suit pas encore |

Ces fichiers sont dans votre **répertoire de travail**. Git les voit,
mais il n'en fait rien : il attend que vous lui disiez lesquels
comptent.

---

## Étape 5 — Enregistrer l'état initial

### Placer les fichiers dans la zone d'attente

```
git add .
```

Le point signifie « tout ce qui se trouve ici ».

Regardez ce qui a changé :

```
git status
```

Les fichiers sont maintenant sous `Changes to be committed`, affichés
en vert. Ils ont quitté le répertoire de travail pour la **zone
d'attente**. Rien n'est encore enregistré.

### Valider

```
git commit -m "Version initiale du projet"
```

Git vous répond avec le nombre de fichiers concernés et un identifiant
court, par exemple `a3f1c2d`. C'est le nom de votre premier
enregistrement.

Vérifiez une dernière fois :

```
git status
```

Vous lisez maintenant `nothing to commit, working tree clean`.
**Les trois zones sont d'accord entre elles.** C'est l'état de repos
d'un dépôt Git.

{: .note }
> `-m` introduit le message. Il est **obligatoire** et doit décrire ce
> que vous avez fait. Nous verrons en séance 6 comment bien le rédiger ;
> pour aujourd'hui, une phrase claire suffit.

---

## Étape 6 — T01 : changer le titre du site

Ouvrez `index.html` dans votre éditeur de texte. Repérez la **ligne 12** :

```
  <h1 class="entete__titre">Guide de Survie</h1>
```

Remplacez-la par :

```
  <h1 class="entete__titre">Guide de Survie — Promo 2026</h1>
```

**Enregistrez le fichier**, puis rafraîchissez la page dans votre
navigateur : le titre a changé.

### Observer l'effet côté Git

```
git status
```

Le fichier apparaît sous `Changes not staged for commit`, en rouge.
Git a remarqué la modification, mais elle n'est ni cadrée ni enregistrée.

Voyez exactement ce qui a changé :

```
git diff
```

La ligne précédée d'un `-` est l'ancienne, celle précédée d'un `+` est
la nouvelle. Appuyez sur `q` pour sortir.

### Enregistrer

```
git add index.html
```

```
git status
```

Le fichier est passé en vert : il est dans la zone d'attente.

```
git commit -m "Modification du titre du site"
```

{: .note }
> Cette fois nous avons écrit `git add index.html` plutôt que `git add .`.
> Les deux fonctionnent, mais nommer le fichier est une bonne habitude :
> vous choisissez explicitement ce qui entre dans l'historique.

---

## Étape 7 — T02 : vous ajouter aux contributeurs

Ouvrez `README.md`. Trouvez la ligne :

```
<!-- ===== AJOUTEZ VOTRE NOM CI-DESSOUS ===== -->
```

Ajoutez **juste en dessous** une ligne à votre nom, en respectant
exactement ce format :

```
- Amira Ben Salah — étudiante
```

Enregistrez, puis déroulez le cycle complet vous-même :

```
git status
```

```
git add README.md
```

```
git commit -m "Ajout de mon nom aux contributeurs"
```

{: .note }
> Vous venez de faire un cycle sans que je vous dise quoi taper à chaque
> ligne. C'est exactement ce qui sera attendu de vous à partir de
> maintenant : **modifier, vérifier, cadrer, valider**.

---

## Étape 8 — T03 : demander à Git d'ignorer un fichier

Certains fichiers n'ont rien à faire dans l'historique : vos notes
personnelles, vos essais, les fichiers créés par votre système.

### Créer un fichier de notes

```
echo "Mes notes perso pour le TP" > notes.txt
```

```
git status
```

`notes.txt` apparaît comme **non suivi**. Si vous faisiez `git add .`
maintenant, il entrerait dans l'historique du projet — et y resterait
pour toujours, visible par tout le monde.

### Dire à Git de l'ignorer

Ouvrez le fichier `.gitignore` dans votre éditeur. Trouvez la ligne :

```
# ===== AJOUTEZ VOS FICHIERS À IGNORER CI-DESSOUS =====
```

Ajoutez juste en dessous :

```
notes.txt
```

Enregistrez, puis :

```
git status
```

**`notes.txt` a disparu de la liste.** Le fichier existe toujours sur
votre disque, mais Git ne le voit plus.

### Enregistrer cette décision

```
git add .gitignore
```

```
git commit -m "Ignorer le fichier de notes personnelles"
```

{: .warning }
> `.gitignore`, lui, **est bien suivi** par Git. C'est normal : la liste
> de ce qu'on ignore fait partie du projet et doit être partagée avec
> toute l'équipe.

{: .note }
> `.gitignore` n'agit que sur les fichiers **non encore suivis**. Un
> fichier déjà enregistré dans l'historique continuera d'être suivi même
> si vous ajoutez son nom ensuite. D'où l'intérêt d'y penser tôt.

---

## Étape 9 — Relire son travail

```
git log --oneline
```

Vous devez voir vos quatre enregistrements, du plus récent au plus
ancien :

```
9c4e2a1 Ignorer le fichier de notes personnelles
7b3d8f5 Ajout de mon nom aux contributeurs
2e9a4c7 Modification du titre du site
a3f1c2d Version initiale du projet
```

Voilà votre historique. Chaque ligne est un état complet du projet,
auquel vous pourrez revenir.

Pour voir le détail, avec l'auteur et la date :

```
git log
```

Sortez avec `q`.

{: .note }
> Les identifiants affichés chez vous seront différents de ceux-ci.
> C'est normal : ils dépendent du contenu, de l'auteur et de l'heure.
> Nous verrons pourquoi en séance 6.

---

## Ce que vous devez avoir à la fin

- [ ] Un dossier `guide-survie` contenant un dossier caché `.git`
- [ ] `git status` répond `working tree clean`
- [ ] `git log --oneline` affiche **quatre** lignes
- [ ] Le titre du site affiche « Promo 2026 » dans le navigateur
- [ ] Votre nom figure dans le `README.md`
- [ ] `notes.txt` existe sur votre disque mais n'apparaît plus dans `git status`

---

## Si vous avez terminé en avance

1. Ajoutez une **fiche de votre invention** dans `data/fiches.js`, sous
   le commentaire `AJOUTEZ VOS FICHES CI-DESSOUS`. Copiez le format
   d'une fiche existante et validez-la dans un commit à part.
2. Puis **allez aider votre voisin**. Expliquer une notion est le
   meilleur moyen de vérifier qu'on l'a comprise.

---

## En cas de blocage

Consultez le [mur des pannes](pannes). Si la vôtre n'y figure pas, signalez-la moi.

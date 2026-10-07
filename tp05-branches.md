---
title: "TP5 · Les branches"
nav_order: 7
published: false
---

# TP5 — Travailler chacun sur sa branche
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

- lire un historique et retrouver qui a fait quoi ;
- créer une branche et vous y déplacer ;
- travailler sans gêner les autres, ni être gêné par eux ;
- publier votre branche sur GitHub.

**Durée : 1 h 20.** Tâche T07.

{: .warning }
> **Bloqué ?** Consultez les [pannes connues](pannes) avant de lever la main.

{: .note }
> **Aujourd'hui, on ne fusionne rien.** Chacun construit sa branche dans
> son coin. Le rassemblement, c'est la séance prochaine.

---

## Point de départ

Placez-vous dans le dépôt du groupe et récupérez le travail de tout le
monde :

```
cd ~/Desktop/guide-groupe
```

```
pwd
```

```
git pull
```

---

## Étape 1 — Lire l'historique

Vous avez déjà utilisé `git log --oneline`. Voyons ce qu'il sait faire
d'autre.

### L'historique détaillé

```
git log
```

Chaque entrée donne l'identifiant complet, l'auteur, la date et le
message. Sortez avec `q`.

### Les options utiles

```
git log --oneline --graph --all
```

| Option | Ce qu'elle ajoute |
|---|---|
| `--oneline` | Une ligne par commit |
| `--graph` | Le dessin des branches |
| `--all` | Toutes les branches, pas seulement la vôtre |
| `-5` | Les cinq derniers seulement |

### Enquêter

Qui a écrit les fiches ?

```
git log --oneline --author="Amira"
```

Qu'est-ce qui a changé dans un commit précis ? Reprenez les sept
premiers caractères d'un identifiant vu plus haut :

```
git show 9c4e2a1
```

Et qui a touché à ce fichier ?

```
git log --oneline data/fiches.js
```

{: .note }
> **Un identifiant de commit est calculé à partir de son contenu**, de
> son auteur et de sa date. Deux commits ne peuvent pas avoir le même.
> C'est pour ça qu'ils sont différents chez chacun de vous, même pour un
> travail identique.

---

## Étape 2 — Créer votre branche

### Où suis-je ?

```
git branch
```

Une seule branche pour l'instant, `main`, avec une astérisque `*` qui
indique où vous êtes.

### Créer et s'y placer

Chaque membre crée **sa** branche, à son prénom :

```
git switch -c fiche-amira
```

{: .note }
> `-c` signifie *create*. Sans lui, Git cherche une branche existante et
> échoue si elle n'existe pas.

Vérifiez :

```
git branch
```

L'astérisque a bougé.

```
git status
```

Git confirme : `On branch fiche-amira`.

{: .warning }
> **Vos fichiers n'ont pas changé, et c'est normal.** Une branche n'est
> pas une copie du projet : c'est un **pointeur** sur un commit. Vous
> partez exactement de l'état où était `main`.

---

## Étape 3 — T07 : deux fiches sur votre branche

### Votre emplacement

Pour que vos quatre branches puissent se rassembler proprement la
semaine prochaine, **chacun écrit à un endroit différent** de
`data/fiches.js`.

Repérez la fiche qui vous est attribuée, et insérez vos deux fiches
**juste après** son accolade fermante `},` :

| Membre | Insérez après la fiche |
|---|---|
| **1** | *La salle de travail du premier* |
| **2** | *Le distributeur du deuxième* |
| **3** | *Arriver avant 8h30* |
| **4** | *Le premier rang n'est pas un piège* |

### Le bloc à copier

Deux fiches, avec vos propres contenus :

```
  {
    titre: "Titre de ma première fiche",
    categorie: "Vie pratique",
    texte: "Un conseil utile, en une ou deux phrases.",
    auteur: "Amira"
  },

  {
    titre: "Titre de ma deuxième fiche",
    categorie: "Bons plans",
    texte: "Un autre conseil.",
    auteur: "Amira"
  },
```

Catégories disponibles : `Vie pratique`, `Études`, `Transport`,
`Bons plans`.

Enregistrez, puis ouvrez `index.html` : vos deux fiches sont là.

### Valider

```
git status
```

```
git add data/fiches.js
```

```
git commit -m "Ajout de deux fiches sur les révisions"
```

{: .note }
> Message précis, sous 50 caractères, qui dit **ce que fait** le
> commit. Pas « modifs » ni « ajout de trucs ».

### Une deuxième fois

Ajoutez **une troisième fiche**, au même endroit, et validez-la dans un
commit séparé. Votre branche aura ainsi deux commits, ce qui rendra le
graphe de l'étape 6 bien plus lisible.

---

## Étape 4 — Publier votre branche

Votre branche n'existe pour l'instant que sur votre machine.

```
git push -u origin fiche-amira
```

Comme pour `main` en séance 4, le `-u` retient le couple : les prochains
envois se feront avec `git push` tout court.

### Aller voir sur GitHub

Rafraîchissez la page du dépôt. En haut à gauche, le sélecteur affiche
`main` — cliquez dessus : **les quatre branches du groupe sont là.**

GitHub affiche aussi un bandeau proposant de comparer votre branche et
d'ouvrir une *pull request*.

{: .warning }
> **Ne cliquez pas.** C'est le sujet de la séance 8. Ignorez le bandeau.

---

## Étape 5 — Voyager entre les branches

C'est ici que la notion de pointeur devient concrète.

Sur le dépôt local sur votre machine, regardez le nombre de fiches dans votre fichier, puis :

```
git switch main
```

Ouvrez `data/fiches.js` : **vos fiches ont disparu.** Rafraîchissez
`index.html` : elles ne sont plus sur le site.

Revenez :

```
git switch fiche-amira
```

**Elles sont revenues.**

{: .note }
> Rien n'a été supprimé. Git a simplement remplacé le contenu de votre
> dossier par celui du commit sur lequel pointe la branche demandée.
> Vos deux versions coexistent dans le dossier `.git`.

Un raccourci utile, qui revient à la branche précédente :

```
git switch -
```

### Si Git refuse de changer de branche

```
error: Your local changes would be overwritten by checkout
```

Vous avez des modifications non validées. Deux solutions :

```
git commit -am "Mon travail en cours"
```

ou, pour les mettre de côté sans les valider :

```
git stash
```

et pour les récupérer plus tard :

```
git stash pop
```

---

## Étape 6 — Voir le travail du groupe

Récupérez les branches de vos camarades :

```
git fetch
```

```
git log --oneline --graph --all
```

Vous voyez maintenant **quatre lignes qui partent du même point** et
avancent en parallèle. Chacune porte les commits d'un membre.

C'est l'image exacte de ce que vous venez de faire : quatre personnes
travaillant en même temps, sans jamais se gêner.

{: .note }
> La semaine prochaine, ces quatre lignes se rejoindront. Vous verrez
> alors comment Git rassemble des travaux parallèles — et ce qu'il fait
> quand deux personnes ont touché à la même ligne.

---

## Ce que vous devez avoir à la fin

- [ ] Une branche nommée `fiche-<votre-prénom>`
- [ ] **Deux commits** sur cette branche, trois fiches au total
- [ ] `git push` effectué : la branche apparaît sur GitHub
- [ ] Vous savez passer de `main` à votre branche et retour
- [ ] `git log --oneline --graph --all` montre les quatre branches
- [ ] **Rien n'a été fusionné**

---

## Si vous avez terminé en avance

1. Sur GitHub, sélectionnez la branche d'un camarade et lisez ses fiches
   sans quitter le navigateur.
2. Corrigez le message de votre dernier commit :

   ```
   git commit --amend -m "Un message plus precis"
   ```

   {: .warning }
   > À ne faire que sur un commit **non encore poussé**. Cette commande
   > réécrit l'historique.

3. Aidez un autre groupe.

---

## En cas de blocage

Consultez les [pannes connues](pannes). Si la vôtre n'y figure pas,
**signalez-la moi** : je l'ajouterai à la liste.
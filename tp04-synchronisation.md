---
title: "TP4 · Se synchroniser"
nav_order: 6
published: false
---

# TP4 — Travailler à plusieurs sur un même dépôt
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

- récupérer ce que d'autres ont poussé, avec `git fetch` et `git pull` ;
- comprendre pourquoi un `git push` est parfois refusé ;
- travailler à quatre sur un même dépôt sans rien écraser.

**Durée : 2 h.** Tâches T05 et T06.

{: .warning }
> **Bloqué ?** Consultez les [pannes connues](pannes) avant de lever la main.

{: .note }
> **Aujourd'hui, votre groupe passe à un dépôt unique.** Chacun en aura
> une copie complète sur sa machine. C'est le fonctionnement normal de
> Git : quatre copies égales, un point de rendez-vous.

---

## Étape 0 — Deux réglages, une fois pour toutes

Ces deux commandes vous éviteront deux blocages garantis aujourd'hui.
Tapez-les avant tout le reste.

```ssh
git config --global pull.rebase false
```

```ssh
git config --global core.editor "nano"
```

{: .note }
> **La première** dit à Git comment réconcilier deux versions qui ont
> avancé chacune de leur côté. Sans elle, `git pull` refuse de
> fonctionner et affiche une longue liste de conseils contradictoires.
>
> **La seconde** choisit l'éditeur que Git ouvre quand il a besoin d'un
> message. Sans elle, vous risquez de tomber dans `vim`, dont on ne
> sort pas facilement quand on ne le connaît pas.

### Si un éditeur s'ouvre quand même

Git vous demande d'écrire un message. Le texte proposé convient
toujours.

| Éditeur | Pour valider et sortir |
|---|---|
| **nano** — un menu en bas de l'écran | `Ctrl + X`, puis `O`, puis `Entrée` |
| **vim** — rien en bas de l'écran | `Échap`, puis `:wq`, puis `Entrée` |

---

## Étape 1 — Dernière visite à votre dépôt personnel

En fin de TP 3, vous avez créé un fichier `LICENSE` **depuis
GitHub**. Votre machine ne le connaît pas encore. Réglons ça.

```ssh
cd ~/Desktop/guide-survie
```

### Aller voir sans rien changer

```ssh
git fetch
```

Cette commande ne modifie **aucun** de vos fichiers. Elle va simplement
prendre des nouvelles du dépôt distant.

```ssh
git status
```

Git vous répond :

<pre class="highlight"><code>On branch main
Your branch is behind 'origin/main' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)</code></pre>

*Votre branche est en retard d'un commit.* Git sait ce qui vous manque,
mais il attend votre feu vert.

### Intégrer

```ssh
git pull
```

<pre class="highlight"><code>Fast-forward
 LICENSE | 1 +
 1 file changed, 1 insertion(+)</code></pre>

```ssh
ls
```

**Le fichier `LICENSE` est là.**

{: .note }
> **`fetch` regarde. `pull` regarde et intègre.**
>
> `git pull` = `git fetch` + une fusion.
>
> `fetch` est prudent : il ne touche jamais à votre travail en cours.
> C'est la commande à utiliser quand vous voulez savoir où en est
> l'équipe sans être dérangé.

Le mot **fast-forward** signifie que vous n'aviez rien fait de votre
côté : Git a simplement avancé votre branche. C'est le cas le plus
simple. Nous verrons l'autre dans un instant.

---

## Étape 2 — Récupérer le dépôt du groupe

Vous avez tous accepté l'invitation en fin de séance 4. Sinon,
faites-le maintenant : icône **cloche** en haut à droite sur GitHub.

**Les quatre membres** du groupe, y compris le pilote, clonent le dépôt
du groupe dans un dossier nommé `guide-groupe` :

```ssh
cd ~/Desktop
```

```ssh
git clone git@github.com:LE-PILOTE/guide-survie.git guide-groupe
```

```ssh
cd guide-groupe
```

Vérifiez que vous êtes bien reliés au même dépôt :

```ssh
git remote -v
```

Les quatre membres doivent voir **exactement la même adresse**.

```ssh
git log --oneline
```

Vous avez tous le même historique. **Quatre copies complètes et égales
d'un seul projet.**

{: .warning }
> Vous avez maintenant **deux dossiers** : `guide-survie`, qui est le
> vôtre, et `guide-groupe`, celui du groupe.
>
> **Tout le travail se fait désormais dans `guide-groupe`.**
>
> Prenez le réflexe de vérifier avant chaque commande :
>
> ```ssh
> pwd
> ```

---

## Étape 3 — T05 : chacun son tour

Vous allez ajouter vos fiches **l'un après l'autre**, en vous
synchronisant à chaque fois. Décidez d'un ordre : membre 1, 2, 3, 4.

{: .warning }
> **Respectez strictement l'ordre.** Si deux personnes modifient le
> fichier en même temps à cet endroit, vous obtiendrez un conflit que
> nous ne savons pas encore résoudre.

### Le membre dont c'est le tour

Ouvrez `data/fiches.js`. Sous le commentaire
`// ===== AJOUTEZ VOS FICHES CI-DESSOUS =====`, collez ce bloc en
remplaçant les quatre valeurs :

```js
  {
    titre: "Le distributeur du deuxième",
    categorie: "Vie pratique",
    texte: "Il rend la monnaie. Celui du rez-de-chaussée, non.",
    auteur: "Amira"
  },
```

Puis :

```ssh
git add data/fiches.js
```

```ssh
git commit -m "Ajout de la fiche d'Amira"
```

```ssh
git push
```

Annoncez à voix haute : **« c'est poussé »**.

### Les trois autres

```ssh
git pull
```

**La fiche apparaît dans votre fichier.** Ouvrez `index.html` : elle est
sur le site.

Le suivant peut y aller.

### Après les quatre tours

```ssh
git log --oneline
```

Quatre fiches, quatre commits, quatre auteurs différents — et le même
historique chez tout le monde.

{: .note }
> C'est le rythme normal du travail à plusieurs :
> **`pull` avant de commencer, `push` en finissant.**

---

## Étape 4 — T06 : quand tout le monde travaille en même temps

Le tour de rôle fonctionnait parce que vous attendiez. Dans la vraie
vie, personne n'attend. Provoquons la situation.

### 1. Chacun sa zone

Pour que Git puisse s'en sortir, vous allez modifier des endroits
**différents**. Répartissez-vous les quatre emplacements :

| Membre | Fichier | Où exactement |
|---|---|---|
| **1** | `data/fiches.js` | Une fiche **juste après** le commentaire `AJOUTEZ VOS FICHES CI-DESSOUS` |
| **2** | `css/styles.css` | La couleur `--couleur-accent` |
| **3** | `README.md` | Votre nom sous le commentaire `AJOUTEZ VOTRE NOM CI-DESSOUS` |
| **4** | `index.html` | Un lien sous le commentaire `AJOUTEZ VOTRE LIEN CI-DESSOUS` |

Le bloc pour le membre 4 :

```html
      <li><a class="menu__lien" href="https://www.bnt.nat.tn">Bibliothèque nationale</a></li>
```

### 2. Tout le monde valide, sans se synchroniser

**Sans faire de `git pull`**, chacun de son côté :

```ssh
git add .
```

```ssh
git commit -m "Ma contribution"
```

### 3. Tout le monde pousse en même temps

```ssh
git push
```

**Un seul réussit.** Les trois autres voient :

<pre class="highlight"><code>! [rejected]        main -> main (fetch first)
error: failed to push some refs
hint: Updates were rejected because the remote contains work that you do
hint: not have locally.</code></pre>

{: .note }
> **Ce n'est pas une erreur, c'est une protection.**
>
> Quelqu'un a poussé pendant que vous travailliez. Si Git acceptait
> votre envoi, il écraserait le travail de l'autre. Il refuse, et vous
> demande d'abord de récupérer.

### 4. Récupérer, puis pousser

Ceux qui ont été refusés tapent :

```ssh
git pull
```

**Regardez bien le message qui s'affiche.** Git a pris les deux versions et les a
combinées **tout seul**. Ouvrez les fichiers : les deux contributions y
sont.

Il a fabriqué pour cela un commit spécial, appelé **commit de fusion**.

```ssh
git push
```

Cette fois, ça passe — sauf si quelqu'un vous a encore devancé. Dans ce
cas, recommencez : `git pull`, puis `git push`. C'est normal, et c'est
exactement ce qui se passe dans une équipe qui travaille vite.

Quand tout le monde a poussé, chacun récupère la version finale :

```ssh
git pull
```

### 5. Regarder l'historique

```ssh
git log --oneline --graph
```

Les lignes ne sont plus droites : elles se séparent puis se rejoignent.
**C'est la trace de votre travail en parallèle.**

Ouvrez `index.html` : les quatre contributions sont sur le site.

{: .warning }
> Git a su fusionner **parce que vous avez modifié des endroits
> différents**. Si vous aviez tous modifié la **même ligne**, il
> n'aurait pas pu choisir : c'est ce qu'on appelle un **conflit**.
>
> Nous y consacrerons plus tard une séance entière. Ne cherchez pas à le
> provoquer aujourd'hui.

---


## Ce que vous devez avoir à la fin

- [ ] Un dossier `guide-groupe` sur votre machine
- [ ] `git remote -v` y affiche l'adresse du dépôt du groupe
- [ ] Le dépôt du groupe contient **cinq fiches ou plus** et les quatre
      contributions de l'étape 4
- [ ] `git log --oneline --graph` montre au moins un commit de fusion
- [ ] Vous savez expliquer la différence entre `fetch` et `pull`

---

## Si vous avez terminé en avance

1. Refaites un tour complet de l'étape 3 : chacun ajoute une fiche de
   plus, en respectant le rythme `pull` → travail → `commit` → `push`.
2. Sur GitHub, allez dans **Insights → Contributors** et regardez le
   graphique : vous devriez voir quatre contributeurs.
3. Aidez un autre groupe.

---

## En cas de blocage

Consultez les [pannes connues](pannes). Si la vôtre n'y figure pas,
**signalez-la moi** : je l'ajouterai à la liste.
---
layout: default
title: "TP6 — Fusionner et résoudre les conflits"
nav_order: 8
published: false
---

# TP6 — Fusionner et résoudre les conflits
{: .no_toc }

## Au programme
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Ce que vous allez apprendre

À la fin de ce TP, vous saurez :

- rapatrier les branches que vos coéquipiers ont poussées ;
- fusionner une branche dans une autre, et **reconnaître les deux formes de fusion** que Git peut produire ;
- lire un conflit, le résoudre, et terminer la fusion ;
- faire marche arrière quand la fusion part mal.

> **Avant de commencer**
> Placez-vous dans le dépôt du groupe `guide-groupe`, puis vérifiez deux choses :
>
> ```bash
> git status
> ```
>
> Vous devez lire `On branch main` et `nothing to commit, working tree clean`.
> Si ce n'est pas le cas, terminez d'abord ce que vous aviez commencé au TP5 (`git add` puis `git commit`), ou demandez.
{: .note }

---

## Partie A — Les deux fusions

### T09 · Étape 1 — Voir les branches des autres

À la fin du TP5, chaque membre du groupe a poussé **sa** branche `fiche-<prénom>`. Votre dépôt local, lui, n'en sait encore rien.

```bash
git fetch
git branch -a
```

`git branch -a` affiche **toutes** les branches connues : les vôtres, puis celles du serveur, préfixées par `remotes/origin/`.

```
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/fiche-amira
  remotes/origin/fiche-mehdi
  remotes/origin/fiche-sarra
  remotes/origin/fiche-youssef
  remotes/origin/main
```

> Vous ne voyez pas les quatre branches ? C'est que `git fetch` n'a rien trouvé à récupérer : vérifiez avec votre groupe que tout le monde a bien fait son `git push -u origin fiche-<prénom>` à la fin du TP5.
{: .warning }

Regardez maintenant la forme de l'histoire :

```bash
git log --oneline --graph --all
```

Quatre branches partent du même commit et aucune ne se rejoint. C'est exactement l'image du travail en parallèle : quatre personnes, quatre fiches, aucune n'a gêné les autres.

---

### T09 · Étape 2 — Fusionner, chacun de son côté

Nous allons fusionner les quatre branches. Mais **pas tout de suite dans `main`** : on s'entraîne d'abord dans une branche jetable, pour que personne n'abîme la branche officielle du groupe.

```bash
git switch -c essai
```

Vous êtes sur une branche `essai`, strictement identique à `main` pour l'instant.

Fusionnez la première branche de votre groupe (remplacez le prénom) :

```bash
git merge origin/fiche-amira
```

```
Updating 63b8bdf..5f546ce
Fast-forward
 data/fiches.js | 10 ++++++++++
 1 file changed, 10 insertions(+)
```

Retenez le mot **`Fast-forward`**. Nous y revenons à l'étape 3.

Fusionnez maintenant la deuxième :

```bash
git merge origin/fiche-youssef
```

```
Auto-merging data/fiches.js
Merge made by the 'ort' strategy.
 data/fiches.js | 10 ++++++++++
 1 file changed, 10 insertions(+)
```

Le message a changé : **`Merge made by the 'ort' strategy`**. Même commande, résultat différent.

> **nano va s'ouvrir, c'est normal**
> À partir de cette deuxième fusion, Git crée un commit — et comme pour tout commit, il vous demande d'en valider le message. `nano` s'ouvre avec un texte déjà rempli, du genre `Merge remote-tracking branch 'origin/fiche-youssef' into essai`.
>
> Ce message est bon tel quel : **`Ctrl+O`, `Entrée`, `Ctrl+X`** et on continue. Ne le réécrivez pas.
{: .warning }

Fusionnez les deux dernières branches de votre groupe, de la même façon :

```bash
git merge origin/fiche-sarra
git merge origin/fiche-mehdi
```

---

### T09 · Étape 3 — Lire ce qui vient de se passer

```bash
git log --oneline --graph
```

```
*   dc84041 Merge remote-tracking branch 'origin/fiche-mehdi' into essai
|\
| * 79992eb Ajout d'une fiche de mehdi
| * aa2ab2f Ajout d'une fiche de mehdi
* |   704a0ff Merge remote-tracking branch 'origin/fiche-sarra' into essai
|\ \
| * | 429daf7 Ajout d'une fiche de sarra
| * | eb67bb8 Ajout d'une fiche de sarra
| |/
* |   cbb6134 Merge remote-tracking branch 'origin/fiche-youssef' into essai
...
```

Trois commits de fusion, chacun avec **deux parents** — c'est la double barre `|\` qui le signale.

Et la première fusion, celle de `fiche-amira` ? Elle n'a laissé **aucun commit**. Cherchez-la dans le graphe : elle n'y est pas.

| Ce que Git a affiché | Ce qu'il a fait | Pourquoi |
|:---|:---|:---|
| `Fast-forward` | déplacé l'étiquette `essai` en avant | `essai` n'avait rien de nouveau : la branche d'Amira contenait déjà toute son histoire |
| `Merge made by the 'ort' strategy` | créé un commit de fusion à deux parents | les deux branches avaient chacune avancé de leur côté |

> **À retenir**
> Ce n'est pas vous qui choisissez : **Git choisit la forme de la fusion en fonction de la forme de l'histoire.** Si la branche d'arrivée n'a pas bougé depuis le point de départ, il avance simplement l'étiquette. Sinon, il fabrique un commit qui réunit les deux histoires.
{: .note }

Maintenant, ouvrez `index.html` dans votre navigateur.

Les douze fiches du groupe sont là. Personne n'a copié-collé le travail de personne, et pourtant tout est réuni dans un seul fichier. **C'est ça, fusionner.**

---

### T09 · Étape 4 — La fusion officielle

Jusqu'ici, chacun a fusionné dans sa branche `essai`, chez lui. Il faut maintenant que le groupe ait **une** version officielle sur GitHub. Un seul membre s'en charge.

**Le pilote du groupe uniquement :**

```bash
git switch main
git merge essai
```

```
Updating 63b8bdf..dc5ff51
Fast-forward
```

`Fast-forward` encore : `main` n'avait pas bougé, Git avance juste l'étiquette. Poussez :

```bash
git push
```

**Tous les autres membres :**

```bash
git switch main
git pull
```

```
Updating 63b8bdf..dc5ff51
Fast-forward
 data/fiches.js | 40 ++++++++++++++++++++++++++++++++++++++++
 1 file changed, 40 insertions(+)
```

Vous récupérez la version du pilote. Vérifiez avec `git log --oneline --graph` que tout le monde dans le groupe voit maintenant **exactement le même graphe**.

---

### T09 · Étape 5 — Ranger : supprimer la branche d'essai

La branche `essai` ne sert plus. On la supprime :

```bash
git switch main
git branch -d essai
```

Et là, **deux choses différentes vont se passer dans la salle.**

Chez le pilote :

```
Deleted branch essai (was dc5ff51).
```

Chez les autres :

```
error: the branch 'essai' is not fully merged.
If you are sure you want to delete it, run 'git branch -D essai'
```

> **Pourquoi ce refus ?**
> Vous avez tous les douze fiches, donc le **contenu** est identique. Mais chacun a fabriqué ses propres commits de fusion, de son côté. Les vôtres ne sont pas dans l'histoire de `main` — c'est celle du pilote qui a été retenue.
>
> Git vous prévient donc : « cette branche contient des commits qui n'existent nulle part ailleurs, tu es sûre ? ». Ici oui, on est sûrs : ces commits ne contiennent rien que `main` n'ait déjà.
{: .note }

```bash
git branch -D essai
```

Retenez la différence, elle vous servira toute votre vie de développeuse ou développeur :

| Commande | Comportement |
|:---|:---|
| `git branch -d <nom>` | supprime **seulement si** le travail est déjà ailleurs. Le garde-fou. |
| `git branch -D <nom>` | supprime quoi qu'il arrive. À n'utiliser que quand on sait ce qu'on perd. |

---

## Partie B — Le conflit

Jusqu'ici, tout s'est fusionné sans accroc : chacun avait écrit **à un endroit différent** du fichier. Git sait recoudre ça tout seul.

Voyons ce qui se passe quand deux personnes touchent **la même ligne**.

### T10 · Étape 6 — Fabriquer un conflit, exprès

Vous allez jouer les deux rôles vous-même, sur deux branches jetables. **Rien de tout cela ne sera poussé.**

Première branche :

```bash
git switch main
git switch -c couleur-vert
```

Ouvrez `css/style.css` et remplacez la valeur de `--couleur-accent` (ligne 4) par du vert :

```css
  --couleur-accent: #1B5E4F;
```

```bash
git commit -a -m "Couleur d'accent en vert"
```

Deuxième branche, **repartie de `main`**, donc sans le vert :

```bash
git switch main
git switch -c couleur-bleu
```

Dans `css/style.css`, remplacez la même ligne par du bleu :

```css
  --couleur-accent: #2C4A7C;
```

```bash
git commit -a -m "Couleur d'accent en bleu"
```

Vous avez maintenant deux branches qui donnent **deux réponses différentes à la même question**. Demandez à Git de les réunir :

```bash
git switch couleur-vert
git merge couleur-bleu
```

```
Auto-merging css/style.css
CONFLICT (content): Merge conflict in css/style.css
Automatic merge failed; fix conflicts and then commit the result.
```

Voilà un conflit. Ce n'est pas une panne : c'est Git qui vous dit « cette décision-là, je ne peux pas la prendre à ta place ».

---

### T10 · Étape 7 — Premier réflexe : regarder où on en est

```bash
git status
```

```
On branch couleur-vert
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
	both modified:   css/style.css
```

Trois informations, toutes utiles :

- **où vous êtes** : au milieu d'une fusion (`You have unmerged paths`) ;
- **quel fichier** bloque : `both modified: css/style.css` ;
- **vos deux sorties** : terminer la fusion, ou l'annuler.

Ouvrez `css/style.css` :

```css
  --couleur-fond: #E6E9E4;
<<<<<<< HEAD
  --couleur-accent: #1B5E4F;
=======
  --couleur-accent: #2C4A7C;
>>>>>>> couleur-bleu
  --couleur-carte: #FBFAF7;
```

Git a écrit **les deux versions** dans votre fichier, séparées par des balises :

| Balise | Ce qu'elle délimite |
|:---|:---|
| `<<<<<<< HEAD` | début de **votre** version (celle de la branche où vous êtes) |
| `=======` | la frontière |
| `>>>>>>> couleur-bleu` | fin de la version **qui arrive** |

Le reste du fichier est intact. Le conflit ne concerne **que** les lignes entre les balises.

---

### T10 · Étape 8 — Le bouton panique

Avant de résoudre, apprenons à faire marche arrière. Parce qu'un jour, vous lancerez une fusion au mauvais moment.

```bash
git merge --abort
```

Aucun message, et c'est bon signe.

```bash
git status
```

```
On branch couleur-vert
nothing to commit, working tree clean
```

Tout est revenu comme avant la fusion : plus de balises dans le fichier, plus de fusion en cours, votre vert est intact.

> **`git merge --abort` est sans danger**
> Il n'existe que pendant une fusion non terminée, et il ne détruit rien de ce qui était commité. Face à un conflit que vous ne comprenez pas, c'est toujours un geste légitime : on annule, on respire, on demande, on recommence.
{: .note }

---

### T10 · Étape 9 — Résoudre pour de vrai

Relancez la fusion :

```bash
git merge couleur-bleu
```

Le même conflit revient. Cette fois, on tranche.

**1. Ouvrez le fichier et décidez.** Supprimez les trois balises *et* la version dont vous ne voulez pas. Il doit rester **une seule ligne**, et aucun chevron :

```css
  --couleur-fond: #E6E9E4;
  --couleur-accent: #2C4A7C;
  --couleur-carte: #FBFAF7;
```

> Vous n'êtes pas obligée de choisir un camp. Résoudre un conflit, c'est écrire la **bonne** version — qui peut très bien être une troisième couleur, ou les deux idées combinées. Git ne vous impose rien, il vous rend la main.
{: .note }

**2. Dites à Git que c'est réglé :**

```bash
git add css/style.css
git status
```

```
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)
```

Le conflit est résolu, mais la fusion n'est pas finie.

**3. Terminez la fusion :**

```bash
git commit
```

`nano` s'ouvre avec un message pré-rempli, et une section `# Conflicts:` qui garde la trace du fichier concerné. Validez tel quel : **`Ctrl+O`, `Entrée`, `Ctrl+X`**.

**4. Vérifiez :**

```bash
git log --oneline --graph
```

```
*   97e5f15 Merge branch 'couleur-bleu' into couleur-vert
|\
| * 5747c8b Couleur d'accent en bleu
* | 72aaea2 Couleur d'accent en vert
|/
* 63b8bdf Version initiale
```

Les deux branches sont réunies. Et surtout : **les deux versions sont toujours dans l'histoire.** Rien n'a été écrasé. La couleur que vous avez écartée est toujours récupérable.

---

### T10 · Étape 10 — Ranger, et ne rien pousser

Ces deux branches étaient un exercice. On les efface.

```bash
git switch main
git branch -D couleur-vert couleur-bleu
git status
```

> **Ne poussez rien de la partie B**
> Vos deux branches de couleur étaient là pour vous entraîner. Votre `main` doit rester tel qu'à la fin de la partie A. Vérifiez-le : `git log --oneline --graph` ne doit contenir **aucune** mention de couleur.
{: .warning }

---

## Les quatre gestes du conflit

À retenir dans cet ordre, c'est la seule chose à mémoriser de ce TP :

1. **`git status`** — où suis-je, quel fichier bloque ?
2. **Ouvrir le fichier** — enlever les balises, écrire la bonne version.
3. **`git add <fichier>`** — c'est réglé.
4. **`git commit`** — fusion terminée.

Et la sortie de secours, à tout moment avant l'étape 4 : **`git merge --abort`**.

---

> **Bloqué ?**
> Un message d'erreur que vous ne comprenez pas, une fusion qui refuse de démarrer, `nano` qui ne veut pas se fermer : passez par la rubrique [Si ça ne marche pas]({{ site.baseurl }}/pannes) avant d'appeler.
{: .warning }

---

> **Rendu du TP6**
> Dans le dossier partagé `Soumissions/Soumission TP6/Groupe <num_groupe>`, déposez :
>
> 1. une **capture d'écran** du résultat de `git log --oneline --graph` sur votre `main`, après la fusion officielle de l'étape 4 ;
> 2. une **capture d'écran** de votre `index.html` ouvert dans le navigateur, avec les douze fiches du groupe ;
> 3. une **capture d'écran** de votre `css/style.css` ouvert pendant le conflit, balises visibles.
>
> Un seul dépôt par groupe pour les points 1 et 2, mais le point 3 est **individuel** : chacun dépose sa capture, nommée `conflit-<prénom>.png`.
{: .rendu }

---

## Pour aller plus loin

Si vous avez fini en avance, le fichier `defis.md` de votre dépôt vous attend. Les défis ★★ se prêtent bien au travail en branche : créez-en une, faites votre défi dessus, fusionnez-la dans `main` quand elle marche.

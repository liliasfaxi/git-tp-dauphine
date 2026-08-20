---
title: "TP3 · Dépôts distants"
nav_order: 5
published: false
---

# TP3 — Mettre son projet en ligne
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

- créer un dépôt sur GitHub et le relier au vôtre ;
- envoyer votre travail en ligne avec `git push` ;
- récupérer un dépôt complet avec `git clone` ;
- écrire en Markdown ;
- reconnaître les fichiers qu'on trouve dans tout projet sérieux.

**Durée : 2 h 10.** Tâche T04.

{: .warning }
> **Bloqué ?** Consultez [le mur des pannes](pannes) avant de lever la main.

---

## Point de départ

Vous devez avoir, depuis la séance 2, un dossier `guide-survie` contenant
quatre commits, et depuis la séance 3, une connexion SSH fonctionnelle.

Vérifiez les deux :

```
cd <chemin de votre projet>/guide-survie
```

```
git log --oneline
```

```
ssh -T git@github.com
```

{: .warning }
> Si votre dépôt local est perdu ou incohérent, ne perdez pas de temps :
> signalez-le moi, je vous donnerai la commande de rattrapage.

---

## Étape 1 — Créer le dépôt sur GitHub

1. Sur GitHub, cliquez sur le **+** en haut à droite, puis **New repository**
2. **Repository name** : `guide-survie`
3. **Description** : une ligne, par exemple *Guide de survie — Dauphine Tunis*
4. Visibilité : **Public**
5. **Ne cochez rien** dans la section *Initialize this repository*
6. **Create repository**

{: .warning }
> **N'ajoutez ni README, ni `.gitignore`, ni licence.** Votre dépôt local
> en possède déjà. Si vous cochez ces cases, GitHub crée un commit de son
> côté et votre premier envoi sera refusé — nous n'avons pas encore les
> outils pour réparer ça.

GitHub affiche alors une page d'instructions. Ignorez-la : nous allons
faire la même chose, mais en comprenant chaque commande.

---

## Étape 2 — Relier votre dépôt local à GitHub

Sur la page de votre dépôt, dans le rectangle bleu **Quick Stup**, cliquer sur SSH, puis copiez
l'adresse. Elle ressemble à `git@github.com:amira-bensalah/guide-survie.git`.

Dans votre terminal, en remplaçant par votre propre adresse :

```
git remote add origin git@github.com:amira-bensalah/guide-survie.git
```

Vérifiez :

```
git remote -v
```

Vous devez voir deux lignes, `fetch` et `push`, avec la même adresse.

{: .note }
> origin est un surnom, pas un mot réservé. C'est le nom donné par convention au dépôt distant principal — et celui que git clone attribue automatiquement.
> Vous pourriez l'appeler dauphine : tout fonctionnerait, à condition de le nommer dans vos commandes (git push -u dauphine main). Mais personne ne vous comprendrait, et la plupart des tutoriels supposent origin.
> Gardez origin.

Cette commande n'a rien envoyé. Elle a seulement inscrit une adresse dans
le fichier `.git/config` de votre projet.

---

## Étape 3 — Le premier envoi

```
git push -u origin main
```

Décomposons :

| Élément | Ce qu'il veut dire |
|---|---|
| `push` | envoie mes commits |
| `origin` | vers ce dépôt distant |
| `main` | cette branche |
| `-u` | et souviens-toi de ce couple pour la suite |

Grâce à `-u`, les prochains envois se feront avec `git push` tout court.

Rafraîchissez la page GitHub : **vos fichiers sont là**, avec vos quatre
commits et votre nom sur chacun.

{: .note }
> Prenez trente secondes pour regarder. Votre travail existe maintenant
> ailleurs que sur votre machine. Si votre disque lâche ce soir, il ne
> sera pas perdu.

---

## Étape 4 — T04 : changer la couleur du site

Ouvrez `css/style.css`. La **ligne 4** contient :

```
  --couleur-accent: #C41E3A;
```

Remplacez la valeur par celle de votre choix :

| Couleur | Code |
|---|---|
| Vert forêt | `#1B5E4F` |
| Bleu encre | `#2C4A7C` |
| Orange brique | `#B3541E` |
| Violet prune | `#6A3B8F` |

Enregistrez, rafraîchissez `index.html` dans le navigateur : **tout le
site a changé de couleur**, alors que vous n'avez modifié qu'une ligne.

C'est l'intérêt des variables CSS — et c'est aussi pourquoi cette ligne
sera notre terrain de conflit en séance 9.

Déroulez le cycle :

```
git status
```

```
git add css/style.css
```

```
git commit -m "Nouvelle couleur d'accent"
```

```
git push
```

Sur GitHub, votre commit apparaît en haut de la liste.

---

## Étape 5 — Découvrir `git clone`

En séance 2, vous avez téléchargé un ZIP. Voyons ce qui lui manquait.

Placez-vous ailleurs que dans votre projet :

```
cd ~/Desktop
```

Puis clonez votre propre dépôt sous un autre nom :

```
git clone git@github.com:amira-bensalah/guide-survie.git essai-clone
```

{: .note }
> Si vous ne retrouvez plus l'adresse de votre dépôt, cliquez sur le bouton vert **< > Code** dans la page principale de votre dépôt github, puis sur SSH.

```
cd essai-clone
```

```
git log --oneline
```

**Vos cinq commits sont là.** Et :

```
git remote -v
```

**L'adresse du dépôt distant aussi**, sans que vous l'ayez configurée.

### ZIP contre clone

| | Download ZIP | `git clone` |
|---|---|---|
| Les fichiers | oui | oui |
| L'historique | **non** | oui |
| Le lien vers GitHub | **non** | oui |
| Peut envoyer des modifications | **non** | oui |

Un ZIP donne une photo. Un clone donne le projet vivant.

Vous pouvez supprimer `essai-clone` : il ne servait qu'à la démonstration.

---

## Étape 6 — Écrire en Markdown

Le Markdown sert partout sur GitHub : README, issues, pull requests,
descriptions. Autant l'apprendre maintenant.

### La syntaxe utile

| Ce que vous écrivez | Ce que ça donne |
|---|---|
| `# Titre` | un grand titre |
| `## Sous-titre` | un titre de section |
| `**gras**` | **gras** |
| `*italique*` | *italique* |
| `` `code` `` | du code en police fixe |
| `- élément` | une puce |
| `1. élément` | une liste numérotée |
| `- [ ] à faire` | une case à cocher |
| `- [x] fait` | une case cochée |
| `[texte](adresse)` | un lien |
| `![texte](image.png)` | une image |
| `> citation` | une citation en retrait |

### À faire

Ouvrez `README.md` et ajoutez ceci **tout à la fin**, en adaptant le
contenu :

```
## Ce qu'il reste à faire

- [x] Mettre le projet en ligne
- [ ] Ajouter mes fiches
- [ ] Personnaliser les couleurs
- [ ] Déployer le site

## Liens utiles

- [Le site des TP](https://VOTRECOMPTE.github.io/git-tp-dauphine/)
- [La documentation de Git](https://git-scm.com/doc)
```

Enregistrez, puis :

```
git add README.md
```

```
git commit -m "Ajout de la liste des taches au README"
```

```
git push
```

**Rafraîchissez GitHub.** Le README s'affiche automatiquement sous la
liste des fichiers, mis en forme.


---

## Étape 7 — Les fichiers d'un projet sérieux

Certains fichiers sont attendus par convention dans tout dépôt public.
GitHub les reconnaît et les met en valeur.

| Fichier | Rôle |
|---|---|
| `README.md` | Ce qu'est le projet — affiché sur la page d'accueil |
| `LICENSE` | Ce que les autres ont le droit d'en faire |
| `CONTRIBUTING.md` | Comment contribuer — affiché à l'ouverture d'une PR |
| `CODE_OF_CONDUCT.md` | Les règles de comportement de la communauté |
| `CODEOWNERS` | Qui doit relire quoi — relecteurs ajoutés automatiquement |
| `.gitignore` | Ce que Git doit ignorer |

### Ajouter une licence, depuis GitHub

Cette fois, nous travaillons **dans le navigateur**.

1. Sur votre dépôt : **Add file** → **Create new file**
2. Nommez le fichier `LICENSE` — un bouton **Choose a license template**
   apparaît juste en dessous.
3. Choisissez **MIT License**, la plus simple et la plus permissive
4. **Review and submit**, puis **Commit changes**

{: .warning }
> Vous venez de créer un commit **sur GitHub uniquement**. Votre dossier
> local ne connaît pas encore ce fichier.
>
> **Ne cherchez pas à le récupérer aujourd'hui.** C'est précisément le
> sujet de la prochaine séance.

---

## Étape 8 — Explorer votre dépôt

Prenez cinq minutes pour cliquer partout. Vous saurez ainsi où chercher.

| Onglet | Ce qu'on y trouve |
|---|---|
| **Code** | Les fichiers, le README, le nombre de commits |
| **Issues** | Les tâches et les bugs — séance 11 |
| **Pull requests** | Les contributions proposées — séance 10 |
| **Actions** | L'automatisation |
| **Projects** | Les tableaux de suivi — séance 12 |
| **Settings** | Visibilité, collaborateurs, Pages |

### Deux endroits à connaître

**Insights → Contributors** : qui a contribué, combien, et quand. C'est
la lecture graphique de votre `git log`.

Ensuite, l'**étoile** en haut à droite met un dépôt en favori. Mettez une
étoile à un projet qui vous plaît — c'est aussi comme ça qu'on suit ce
qui se fait.

---

## Ce que vous devez avoir à la fin

- [ ] Un dépôt `guide-survie` visible sur votre compte GitHub
- [ ] `git remote -v` affiche l'adresse de votre dépôt
- [ ] Le site affiche votre nouvelle couleur d'accent
- [ ] Le README affiche une liste de tâches mise en forme
- [ ] Un fichier `LICENSE` existe sur GitHub
- [ ] Vous savez ce que contiennent les onglets Insights et Settings

---

## Si vous avez terminé en avance

1. Ajoutez un fichier `CONTRIBUTING.md` depuis GitHub, avec deux ou trois
   phrases sur la façon de proposer une fiche.
2. Renseignez la section **About** de votre dépôt (roue crantée à droite) :
   description et sujets.
3. Aidez votre voisin.

---

## En cas de blocage

Consultez le [mur des pannes](pannes). Si la vôtre n'y figure pas, signalez-la moi.

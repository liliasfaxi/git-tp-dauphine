---
title: "TP0 · Installer Git"
nav_order: 2
published: true
---

# TP0 — Installer et configurer Git
{: .no_toc }

<details open markdown="block">
  <summary>Sommaire</summary>
  {: .text-delta }
- TOC
{:toc}
</details>

---

## Objectifs

À la fin de ce TP, vous aurez :

- Git installé et fonctionnel sur votre machine ;
- votre identité configurée, celle qui signera tous vos futurs travaux ;
- un compte GitHub à votre nom ;
- un terminal que vous savez ouvrir et utiliser pour trois choses.

**Durée : 40 minutes.** Séance 1.

{: .note }
> Si vous terminez avant les autres, ne partez pas : allez aider votre
> voisin. C'est le meilleur moyen de vérifier que vous avez compris.

---

## Étape 1 — Ouvrir un terminal

Tout ce que nous ferons dans ce module passe par le terminal. Ce n'est
pas de la nostalgie : c'est parce que les boutons cachent ce que Git
fait réellement, et que vous êtes ici pour le comprendre.

### Sur Windows

Nous utiliserons **Git Bash**, qui s'installe avec Git (étape 2).
Pour l'instant, passez directement à l'étape 2 et revenez ensuite ici.

Une fois Git installé : menu Démarrer, tapez `Git Bash`, ouvrez-le.

{: .warning }
> Sur Windows, utilisez **Git Bash**, jamais l'invite de commandes
> `cmd` ni PowerShell. Les commandes de ce site sont écrites pour
> Git Bash et certaines ne fonctionnent pas ailleurs.

### Sur macOS

`Cmd + Espace`, tapez `Terminal`, appuyez sur Entrée.

### Sur Linux

`Ctrl + Alt + T`.

---

## Étape 2 — Installer Git

### Windows

1. Rendez-vous sur [git-scm.com/download/win](https://git-scm.com/download/win)
2. Cliquez sur "Click here to download" pour télécharger la dernière version de git. Lancez le fichier `.exe`.
3. Cliquez **Next** à chaque écran sans rien changer, puis **Install**.

L'installateur pose beaucoup de questions. Les réponses par défaut sont
les bonnes. Ne cherchez pas à optimiser.

### macOS

Tapez simplement dans le terminal :

```
git --version
```

Si macOS vous propose d'installer les outils de développement,
acceptez et patientez. Sinon, Git est déjà là.

### Linux (Ubuntu, Debian)

```
sudo apt update && sudo apt install git
```

---

## Étape 3 — Vérifier l'installation

Fermez puis rouvrez votre terminal, et tapez :

```
git --version
```

Vous devez voir s'afficher quelque chose comme `git version 2.43.0`.
Le numéro exact n'a aucune importance.

{: .warning }
> Si vous lisez `command not found` ou `n'est pas reconnu`, l'installation
> n'a pas abouti, ou vous n'avez pas rouvert le terminal. Rouvrez-le
> d'abord — c'est le cas neuf fois sur dix.

---

## Étape 4 — Vous présenter à Git

Git inscrit votre nom et votre adresse dans **chaque** enregistrement que
vous ferez. Cette information est publique et permanente. Prenez donc
trente secondes pour l'écrire correctement.

Remplacez le contenu entre guillemets par vos propres informations :

```
git config --global user.name "Amira Ben Salah"
```

```
git config --global user.email "amira.bensalah@dauphine.tn"
```

{: .note }
> **Utilisez votre email dauphine**! cela vous sera utile ultérieurement pour vous inscrire à la certification github

{: .note }
> `--global` signifie « pour tous mes projets sur cette machine ».
> Vous ne referez cette configuration qu'une seule fois, aujourd'hui.

### Choisir la langue de vos messages d'erreur

Git parle anglais par défaut. Gardez-le ainsi : toutes les réponses que
vous trouverez sur Internet supposent des messages en anglais, et les
messages de Git sont vos meilleurs guides. Nous les traduirons ensemble
au fur et à mesure.

### Nommer la branche principale

```
git config --global init.defaultBranch main
```

Nous verrons en séance 6 ce qu'est une branche. Pour aujourd'hui,
retenez seulement que cette ligne vous évitera un avertissement gênant.

---

## Étape 5 — Vérifier votre configuration

```
git config --global --list
```

Vous devez retrouver votre nom et votre adresse dans la liste.

{: .note }
> Si l'affichage bloque et refuse de rendre la main, appuyez sur `q`
> (comme *quit*). Ce petit afficheur s'appelle un *pager* et vous le
> recroiserez souvent.

Une faute dans votre nom ? Relancez simplement la commande de l'étape 4
avec la bonne valeur : elle écrase l'ancienne.

---

## Étape 6 — Créer votre compte GitHub

1. Allez sur [github.com/signup](https://github.com/signup)
2. Utilisez de préférence **la même adresse** qu'à l'étape 4.
3. Choisissez le plan gratuit.
4. Validez votre adresse via le mail reçu — cette étape est obligatoire
   pour la suite du module.

### Bien choisir son nom d'utilisateur

Ce nom apparaîtra dans l'adresse de tous vos projets, et il est très
pénible à changer ensuite. Dans deux ans, vous mettrez peut-être ce
profil sur un CV.

- Prenez `amira-bensalah`, pas `xX_amirouchette_Xx`.
- Évitez votre année de naissance et votre promotion.
- Restez court et lisible.

{: .warning }
> Activez l'authentification à deux facteurs dès aujourd'hui :
> **Settings** → **Password and authentication** → **Two-factor authentication**.
> GitHub la rend obligatoire, et la configurer maintenant vous évitera
> d'être bloqué en pleine séance 3.

---

## Ce que vous devez avoir à la fin

- [ ] `git --version` affiche un numéro de version
- [ ] `git config --global --list` affiche votre nom et votre adresse
- [ ] Vous savez ouvrir un terminal sans chercher
- [ ] Votre compte GitHub est créé et l'adresse validée
- [ ] L'authentification à deux facteurs est active

---

## Les trois commandes de terminal à connaître

Nous n'en utiliserons pratiquement pas d'autres.

| Commande | Ce qu'elle fait |
|---|---|
| `pwd` | Affiche où vous êtes |
| `ls` | Liste ce qu'il y a ici |
| `cd nom-du-dossier` | Entre dans un dossier |

Deux raccourcis qui vous serviront tous les jours :

- `cd ..` remonte d'un dossier ;
- après avoir tapé les premières lettres d'une commande ou d'un chemin, la touche **Tab**
  complète le reste. Utilisez-la systématiquement : elle vous évitera
  la moitié de vos fautes de frappe.
- pour revenir à la commande précédente sur le terminal, il suffit de cliquer sur la flèche du haut sur votre clavier. Cela vous évitera de retaper une longue commande.

Essayez maintenant :

```
pwd
```

```
ls
```

---

## En cas de blocage

Consultez le [mur des pannes](pannes). Si la vôtre n'y figure pas, signalez-la moi.

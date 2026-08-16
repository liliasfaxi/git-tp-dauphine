---
title: Le mur des pannes
nav_order: 90
published: true
---

# Le mur des pannes
{: .no_toc }

<details open markdown="block">
  <summary>Sommaire</summary>
  {: .text-delta }
- TOC
{:toc}
</details>

---

Cette page recense les erreurs rencontrées en séance et leur solution.

**Regardez-y avant de lever la main.** Et quand vous résolvez une panne
qui n'y figure pas, signalez-la : elle servira à toute la promo. À partir
de la séance 10, vous pourrez l'ajouter vous-même par *pull request*.

{: .note }
> Un message d'erreur n'est pas une punition. C'est la seule façon qu'a
> Git de vous parler. Lisez-le en entier avant de faire quoi que ce soit :
> il contient très souvent la solution.

---

## `git: command not found`

Aussi vu comme : *« git » n'est pas reconnu en tant que commande interne*.

**Cause.** Git n'est pas installé, ou votre terminal a été ouvert avant
l'installation et ne le voit pas encore.

**Solution.** Fermez complètement le terminal, rouvrez-le, réessayez.
Si l'erreur persiste, reprenez l'étape 2 du [TP0](tp00-installation).

Sur Windows, vérifiez aussi que vous êtes bien dans **Git Bash** et non
dans `cmd` ou PowerShell.

---

## L'affichage est bloqué et je ne peux plus rien taper

Vous voyez peut-être `:` ou `(END)` en bas de l'écran.

**Cause.** Git affiche un texte long dans un *pager*.

**Solution.** Appuyez sur `q`.

---

## Je me suis trompé dans mon nom ou mon adresse

**Solution.** Relancez simplement la commande avec la bonne valeur.
Elle écrase l'ancienne, il n'y a rien à supprimer.

```
git config --global user.name "Le bon nom"
```

---

## `Permission denied (publickey)`

Message complet :

```
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.
```

**Cause.** GitHub ne reconnaît pas votre clé SSH. Le message parle du
dépôt, mais le problème est uniquement d'authentification : ni votre
dépôt ni votre travail ne sont en cause.

**Diagnostic.**

```
ssh -T git@github.com
```

Si la réponse commence par `Hi` suivi de votre nom d'utilisateur, votre
clé fonctionne et l'erreur vient de l'adresse du dépôt.

**Solution.** Reprenez la configuration de la clé SSH (séance 3), en
vérifiant que vous avez bien copié le fichier terminant par `.pub`,
en entier et sur une seule ligne.

---

## `fatal: not a git repository`

**Cause.** Vous n'êtes pas dans le bon dossier.

**Diagnostic.**

```
pwd
```

```
ls -a
```

Vous devez voir un dossier `.git` dans la liste. S'il n'y est pas, vous
êtes ailleurs que dans votre projet.

**Solution.** Déplacez-vous dans le bon dossier avec `cd`.

---

## `Please tell me who you are`

**Cause.** Git ne connaît pas votre identité : la configuration de
l'étape 4 du TP0 n'a pas été faite, ou pas avec `--global`.

**Solution.**

```
git config --global user.name "Votre Nom"
```

```
git config --global user.email "votre.adresse@dauphine.tn"
```

---

## Ma commande échoue et je ne vois pas pourquoi

Vérifiez ces trois choses, dans l'ordre :

1. **Avez-vous copié le `$` du début ?** Il ne fait pas partie de la
   commande. Sur ce site, il n'y en a jamais : si vous en voyez un
   ailleurs, ne le copiez pas.
2. **Vos guillemets sont-ils droits ?** Les
   guillemets courbes viennent de Word ou d'une conversation, et Git
   ne les comprend pas. Ils sont presque impossibles à distinguer à
   l'œil nu : retapez-les à la main en cas de doute.
3. **Y a-t-il une faute de frappe ?** `git stauts` ne veut rien dire.
   Utilisez la touche **Tab** et les flèches ↑ pour rappeler une
   commande précédente.

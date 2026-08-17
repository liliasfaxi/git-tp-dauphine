---
title: Les pannes connues
nav_order: 90
published: true
---

# Les pannes connues
{: .no_toc }

<details open markdown="block">
  <summary>Sommaire</summary>
  {: .text-delta }
- TOC
{:toc}
</details>

---

Cette page recense les erreurs rencontrées en séance et leur solution.
**Regardez-y avant de lever la main.**

Votre panne n'y figure pas ? Signalez-la moi en séance : je l'ajouterai
ici, et elle servira à toute la promo.

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

**Solutions, dans cet ordre.**

1. La clé est-elle chargée dans l'agent ?

   ```
   ssh-add -l
   ```

   Une liste vide : rechargez-la.

   ```
   ssh-add ~/.ssh/id_ed25519
   ```

2. La clé collée sur GitHub est-elle la bonne ? Comparez avec :

   ```
   cat ~/.ssh/id_ed25519.pub
   ```

3. La clé est-elle complète et sur **une seule ligne** ? Une clé coupée
   au collage ne fonctionne jamais. Supprimez-la sur GitHub et
   recollez-la.

---

## `id_ed25519 already exists. Overwrite (y/n)?`

**Cause.** Vous avez déjà une clé sur cette machine.

**Solution.** Répondez **`n`**. Réutilisez la clé existante : elle est
parfaitement valable. L'écraser vous déconnecterait des autres services
qui l'utilisent.

---

## `ssh -T` reste bloqué sans rien afficher

**Cause.** Le réseau bloque le port SSH. C'est fréquent sur les réseaux
d'entreprise ou d'établissement.

**Solution.** Ce n'est pas réparable depuis votre machine : basculez sur
le plan B du TP2, l'authentification par token (PAT) avec des adresses
en `https://`.

---

## `Are you sure you want to continue connecting (yes/no)?`

Ce n'est pas une erreur. C'est la question posée à la **première**
connexion à un serveur inconnu.

**Solution.** Écrivez `yes` en toutes lettres, puis Entrée. La question
ne reviendra plus.

---

## `Support for password authentication was removed`

**Cause.** Vous avez saisi votre mot de passe GitHub. Il n'est plus
accepté depuis 2021.

**Solution.** Utilisez SSH (TP2), ou un token à la place du mot de
passe si vous êtes en `https://`.

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
2. **Vos guillemets sont-ils droits ?** `"` et non `"` ou `"`. Les
   guillemets courbes viennent de Word ou d'une conversation, et Git
   ne les comprend pas. Ils sont presque impossibles à distinguer à
   l'œil nu : retapez-les à la main en cas de doute.
3. **Y a-t-il une faute de frappe ?** `git stauts` ne veut rien dire.
   Utilisez la touche **Tab** et les flèches ↑ pour rappeler une
   commande précédente.


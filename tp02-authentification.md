---
title: "TP2 — Se connecter à GitHub"
nav_order: 4
published: false
---

# TP2 — Se connecter à GitHub
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

- l'authentification à deux facteurs active sur votre compte ;
- une paire de clés SSH sur votre machine ;
- votre clé publique enregistrée sur GitHub ;
- une connexion testée et fonctionnelle.

**Durée : 1 h 20.**

{: .warning }
> C'est le TP le plus technique du semestre, et celui où les blocages
> sont les plus fréquents. **C'est normal.** Ce n'est ni votre faute
> ni celle de votre machine : chaque système réagit un peu différemment.
>
> Avancez étape par étape, sans en sauter aucune.

{: .note }
> Le premier de votre groupe qui obtient le message de succès de
> l'étape 6 devient le référent des trois autres. C'est la façon la
> plus rapide de terminer ce TP à quatre.

---

## Étape 1 — Vérifier la double authentification

GitHub l'impose. Sans elle, la suite du semestre est impossible.

1. Sur GitHub, cliquez sur votre photo en haut à droite
2. **Settings**
3. Dans le menu de gauche : **Password and authentication**
4. Section **Two-factor authentication**

Si elle est déjà active, passez à l'étape 2.

Sinon, activez-la maintenant. La méthode par **application
d'authentification** (Google Authenticator, Microsoft Authenticator,
Authy) est la plus simple : vous installez l'application sur votre
téléphone, vous scannez le code affiché, vous saisissez les six
chiffres obtenus.

{: .warning }
> **Enregistrez les codes de secours** que GitHub vous affiche à ce
> moment-là. Ce sont eux qui vous sauveront si vous perdez votre
> téléphone. Mettez-les dans un fichier ou une note, pas dans ce dépôt.

---

## Étape 2 — Regarder si vous avez déjà une clé

Dans votre terminal :

```
ls -al ~/.ssh
```

Cherchez un fichier nommé `id_ed25519.pub` ou `id_rsa.pub`.

- **Vous en avez un** → passez directement à l'étape 4.
- **Le dossier n'existe pas**, ou aucun fichier ne finit par `.pub`
  → continuez à l'étape 3.

{: .note }
> Le message `No such file or directory` est ici une réponse normale,
> pas une erreur : il signifie simplement que vous n'avez jamais créé
> de clé sur cette machine.

---

## Étape 3 — Fabriquer votre paire de clés

Remplacez l'adresse par la vôtre, celle de votre compte GitHub :

```
ssh-keygen -t ed25519 -C "amira.bensalah@dauphine.tn"
```

Trois questions vous sont posées. **Appuyez trois fois sur Entrée**
pour accepter les réponses par défaut :

| Question | Réponse |
|---|---|
| *Enter file in which to save the key* | Entrée (emplacement par défaut) |
| *Enter passphrase* | Entrée (aucune) |
| *Enter same passphrase again* | Entrée |

Une image en caractères s'affiche : c'est normal, votre paire est créée.

{: .warning }
> Si vous lisez `id_ed25519 already exists. Overwrite (y/n)?`,
> répondez **`n`**. Vous avez déjà une clé : retournez à l'étape 2 et
> réutilisez-la. L'écraser vous déconnecterait des autres services qui
> l'utilisent.

### Vous venez de créer deux fichiers

| Fichier | Nature | Où il va |
|---|---|---|
| `id_ed25519` | **Privée** | Reste sur votre machine. Toujours. |
| `id_ed25519.pub` | **Publique** | Se donne à GitHub |

{: .warning }
> Ne partagez jamais le fichier **sans** `.pub`. Ne le copiez pas dans
> un projet, ne l'envoyez à personne, ne le collez nulle part.

---

## Étape 4 — Charger la clé dans l'agent

L'agent SSH garde votre clé en mémoire pour éviter d'avoir à la
retrouver à chaque commande.

```
eval "$(ssh-agent -s)"
```

Un numéro de processus s'affiche : c'est le signe que l'agent tourne.

**Sur macOS :**

```
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

**Sur Windows et Linux :**

```
ssh-add ~/.ssh/id_ed25519
```

Réponse attendue : `Identity added:` suivi du chemin de votre clé.

---

## Étape 5 — Donner votre clé publique à GitHub

### Copier la clé

**macOS :**

```
pbcopy < ~/.ssh/id_ed25519.pub
```

**Windows (Git Bash) :**

```
clip < ~/.ssh/id_ed25519.pub
```

**Linux :**

```
cat ~/.ssh/id_ed25519.pub
```

Sur Linux, sélectionnez le texte affiché et copiez-le à la main.

### La coller sur GitHub

1. **Settings** → **SSH and GPG keys** (menu de gauche)
2. Bouton vert **New SSH key**
3. **Title** : un nom qui vous parle, par exemple `Portable Dauphine`
4. **Key type** : laissez `Authentication Key`
5. **Key** : collez
6. **Add SSH key**, puis confirmez avec votre mot de passe

### Vérifier ce que vous avez collé

Votre clé doit :

- commencer par `ssh-ed25519`
- tenir sur **une seule ligne**, sans retour à la ligne au milieu
- se terminer par votre adresse mail

{: .warning }
> Si vous voyez `-----BEGIN OPENSSH PRIVATE KEY-----`, **arrêtez tout** :
> vous avez copié la clé **privée**. Ne la collez nulle part.
> Recommencez l'étape 5 en n'oubliant pas le `.pub`.

---

## Étape 6 — Tester la connexion

```
ssh -T git@github.com
```

À la toute première connexion, une question apparaît :

<pre class="highlight"><code>The authenticity of host 'github.com' can't be established.
Are you sure you want to continue connecting (yes/no)?</code></pre>

Écrivez **`yes`** en toutes lettres, puis Entrée.

### La réponse attendue

<pre class="highlight"><code>Hi amira-bensalah! You've successfully authenticated,
but GitHub does not provide shell access.</code></pre>

**Ce message est un succès**, y compris sa seconde moitié : GitHub vous
reconnaît, il vous signale simplement qu'on ne peut pas s'y connecter
comme sur un serveur classique. C'est normal et attendu.

Si votre nom d'utilisateur s'affiche, **votre TP est terminé.**

{: .note }
> Terminé en avance ? Aidez les autres membres de votre groupe.
> Ce TP se réussit à quatre bien plus vite qu'à un.
> Sinon, passez aux étapes 7 et 8, elles sont facultatives mais très souhaitées
>

{: .warning }
> **Bloqué ?** Consultez la rubrique [Si ça ne marche pas](#si-ça-ne-marche-pas)
> en bas de page avant de lever la main.

---

## Étape 7 — Demander votre statut étudiant

**Facultatif, mais à lancer aujourd'hui.**

GitHub accorde aux étudiants vérifiés un ensemble d'avantages gratuits,
réunis sous le nom de **GitHub Student Developer Pack**. La vérification
demande plusieurs jours : mieux vaut la demander maintenant que d'en
avoir besoin dans deux mois.

1. Rendez-vous sur [education.github.com/pack](https://education.github.com/pack)
2. **Sign up for Student Developer Pack**
3. Utilisez votre **adresse universitaire** et le nom de l'établissement
4. Fournissez le justificatif demandé (carte d'étudiant, certificat de
   scolarité)

{: .note }
> Votre compte doit avoir la double authentification active — c'est
> l'étape 1 de ce TP. Utilisez la même adresse universitaire sur votre
> compte GitHub et dans le formulaire : c'est ce qui accélère le plus
> la validation.

Parmi les avantages : des outils de développement gratuits, et selon
les périodes, un bon pour passer gratuitement la certification
**GitHub Foundations**. La disponibilité de ce bon varie — ne comptez
pas dessus, mais demandez la vérification, elle ne coûte rien.

---

## Étape 8 — Soigner votre profil

**Facultatif.** À faire si vous avez terminé et aidé vos camarades.

Votre profil GitHub est une page publique. Dans deux ans, elle pourra
accompagner une candidature. Elle commence aujourd'hui.

Sur votre page de profil, cliquez sur **Edit profile** :

- une **photo** ou un avatar reconnaissable ;
- votre **nom** en toutes lettres ;
- une **bio** d'une ligne : « Étudiante en première année à Dauphine Tunis » ;
- votre **localisation**.

### Le README de profil

C'est l'astuce que peu de gens connaissent. Créez un dépôt **public**
dont le nom est **exactement votre nom d'utilisateur** — par exemple
`amira-bensalah/amira-bensalah` — et cochez **Add a README file**.

Le contenu de ce README s'affichera en haut de votre profil.

{: .note }
> Nous verrons la syntaxe Markdown en détail en séance 4. Pour
> l'instant, `# Titre` fait un grand titre et `**gras**` met en gras.

---


## Si ça ne marche pas

### `Permission denied (publickey)`

Reprenez dans l'ordre :

1. **La clé est-elle chargée ?**

   ```
   ssh-add -l
   ```

   Une liste vide signifie que l'étape 4 n'a pas fonctionné : refaites-la.

2. **Avez-vous collé la bonne clé ?** Sur GitHub, dans **SSH and GPG keys**,
   comparez le début de la clé affichée avec :

   ```
   cat ~/.ssh/id_ed25519.pub
   ```

   Les premiers caractères après `ssh-ed25519` doivent être identiques.

3. **La clé est-elle complète ?** Une clé coupée en deux lignes au
   moment du collage ne fonctionnera jamais. Supprimez-la sur GitHub et
   recollez-la.

### La commande reste bloquée sans rien afficher

Le réseau bloque probablement le port SSH. Passez au plan B ci-dessous
— ce n'est pas un échec, c'est une contrainte du réseau.

---

## Plan B — le token (PAT)

À n'utiliser **que** si SSH ne fonctionne pas après avoir tout vérifié.

### Créer le token

1. **Settings** → tout en bas à gauche : **Developer settings**
2. **Personal access tokens** → **Tokens (classic)**
3. **Generate new token** → **Generate new token (classic)**
4. **Note** : `TP Git Dauphine`
5. **Expiration** : `90 days`
6. **Select scopes** : cochez uniquement **`repo`**
7. **Generate token**

{: .warning }
> Le token n'est affiché **qu'une seule fois**. Copiez-le immédiatement
> et gardez-le dans un endroit sûr — pas dans un dépôt Git.

### S'en servir

Vous utiliserez alors les adresses en `https://` au lieu de
`git@github.com:`. Au premier envoi, Git demandera vos identifiants :

- **Username** : votre nom d'utilisateur GitHub
- **Password** : **le token**, pas votre mot de passe

Pour que Git le retienne :

```
git config --global credential.helper store
```

{: .note }
> Cette commande enregistre le token en clair sur votre machine.
> C'est acceptable sur votre ordinateur personnel, jamais sur un poste
> partagé.

---

## Ce que vous devez avoir à la fin

- [ ] La double authentification est active
- [ ] `ls -al ~/.ssh` montre `id_ed25519` et `id_ed25519.pub`
- [ ] La clé publique figure dans **SSH and GPG keys** sur GitHub
- [ ] `ssh -T git@github.com` affiche `Hi <votre-nom>!`

---

## En cas de blocage

Consultez les [pannes connues](pannes). Si la vôtre n'y figure pas,
**signalez-la moi** : je l'ajouterai à la liste.

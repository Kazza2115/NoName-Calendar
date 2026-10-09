# Demandes NONAME

Outil gratuit pour noter, suivre et partager les demandes de NONAME AGENCY SA (studio, label, services artistes).

- **Hébergement :** GitHub Pages (gratuit)
- **Base de données + connexion :** Supabase (offre gratuite)
- **Accès :** seules les personnes que tu invites peuvent se connecter et modifier

Le site est une seule page (`index.html`), sans installation ni build.

---

## Fonctionnalités

- Saisie rapide : titre + Entrée, avec artiste/client, type, priorité et échéance en option
- Types : enregistrement studio, mix/master, production/beat, co-production label, booking/live, promo/com, admin/contrat, autre
- Statuts : Nouvelle → En cours → En attente → Terminée (modifiables directement dans la liste)
- Fiche détaillée : contact, responsable, budget (CHF), notes, suppression
- Filtres par statut, recherche, compteurs « urgentes » et « en retard »
- Bouton « Copier le récap » : résumé texte à coller dans WhatsApp ou un mail
- **Calendrier** mensuel des échéances : clic sur un jour = nouvelle demande à cette date
- **Updates** : bouton « Ajouter une update » dans chaque fiche (ligne datée et signée ajoutée en haut des notes) + onglet qui affiche les dernières demandes modifiées
- Synchronisation en temps réel entre tous les membres connectés
- Mode clair / sombre automatique, utilisable sur mobile
- **Mode démo** : tant que les clés Supabase ne sont pas remplies, le site fonctionne avec des exemples stockés dans le navigateur (pratique pour tester avant l'installation)

---

## Installation (≈ 15 minutes)

### 1. Créer le projet Supabase

1. Crée un compte sur [supabase.com](https://supabase.com) et un **nouveau projet** (région Europe, ex. Frankfurt).
2. Va dans **SQL Editor** → **New query**, colle le script ci-dessous et clique **Run** :

```sql
-- Table des demandes
create table public.demandes (
  id         uuid primary key default gen_random_uuid(),
  titre      text not null,
  client     text default '',
  type       text default 'Autre',
  priorite   text default 'normale' check (priorite in ('basse','normale','haute','urgente')),
  statut     text default 'nouvelle' check (statut in ('nouvelle','en-cours','en-attente','terminee')),
  echeance   date,
  contact    text default '',
  notes      text default '',
  resp       text default '',
  budget     text default '',
  cree_le    timestamptz default now(),
  maj_le     timestamptz default now(),
  cree_par   uuid default auth.uid() references auth.users(id)
);

-- Sécurité : seules les personnes connectées peuvent lire et écrire
alter table public.demandes enable row level security;

create policy "Membres : lecture"     on public.demandes for select to authenticated using (true);
create policy "Membres : ajout"       on public.demandes for insert to authenticated with check (true);
create policy "Membres : modification" on public.demandes for update to authenticated using (true) with check (true);
create policy "Membres : suppression" on public.demandes for delete to authenticated using (true);

-- Temps réel
alter publication supabase_realtime add table public.demandes;
```

### 2. Fermer les inscriptions publiques

Dans **Authentication** → **Sign In / Providers** :

- Laisse **Email** activé.
- Désactive **Allow new users to sign up**.

Ainsi, personne ne peut se créer un compte seul : seuls les membres invités accèdent à l'outil.

### 3. Récupérer les clés

Dans **Project Settings** → **API** (ou **Data API**), copie :

- **Project URL** → ex. `https://abcdxyz.supabase.co`
- **anon / publishable key** (la clé publique, **jamais** la `service_role`)

Ouvre `index.html` et remplace en haut du script :

```js
const SUPABASE_URL = "https://VOTRE-PROJET.supabase.co";
const SUPABASE_ANON_KEY = "VOTRE_CLE_ANON_PUBLIQUE";
```

> La clé `anon` peut être publique sur GitHub : c'est la sécurité côté base (RLS, étape 1) qui protège les données. Ne publie jamais la clé `service_role`.

### 4. Mettre en ligne sur GitHub Pages

1. Crée un dépôt sur GitHub, ex. `NoName-Calendar` (public, car Pages est gratuit sur les dépôts publics).
2. Ajoute `index.html` et `README.md` (bouton **Add file → Upload files**).
3. Va dans **Settings** → **Pages** → **Source : Deploy from a branch** → branche `main`, dossier `/ (root)` → **Save**.
4. Après 1–2 minutes, le site est en ligne sur :
   `https://TON-PSEUDO.github.io/NoName-Calendar/`

### 5. Autoriser l'adresse du site dans Supabase

Dans **Authentication** → **URL Configuration** :

- **Site URL** : `https://TON-PSEUDO.github.io/NoName-Calendar/`
- **Redirect URLs** : ajoute la même adresse.

Sans ça, le lien de connexion reçu par e-mail ne ramènera pas sur le site.

---

## Partager avec l'équipe (un seul mot de passe)

Le site demande uniquement **un mot de passe commun** à toute l'équipe (pas d'e-mail à saisir).
En coulisses, il se connecte à un compte Supabase partagé : `equipe@noname-calendar.app` (défini par `EQUIPE_EMAIL` dans `index.html`).

**Créer le compte équipe (une seule fois) :**
1. Supabase → **Authentication** → **Users** → **Add user** → **Create new user**.
2. E-mail : `equipe@noname-calendar.app` · Mot de passe : celui de l'équipe · coche **Auto Confirm User**.

Envoie ensuite le lien du site et le mot de passe à l'équipe. Chacun peut indiquer son prénom à la connexion : il signe ses updates (mémorisé sur son appareil).

**Changer le mot de passe** (ex. quelqu'un quitte l'équipe) : **Authentication** → **Users** → clic sur `equipe@noname-calendar.app` → **Reset password** / changer le mot de passe. Les sessions déjà ouvertes restent actives jusqu'à déconnexion ; pour éjecter tout le monde, supprime l'utilisateur et recrée-le.

---

## Modifier l'outil

Tout se trouve dans `index.html`. Modifie le fichier directement sur GitHub (icône crayon) : le site se met à jour en 1–2 minutes après le commit.

| Ce que tu veux changer | Où |
|---|---|
| Liste des types de demandes | `const TYPES = [...]` dans le script |
| Priorités | `const PRIOS = [...]` (+ contrainte `check` dans la table si tu ajoutes une valeur) |
| Statuts | `const STATUTS = [...]` (+ contrainte `check` dans la table) |
| Couleurs | variables `--accent`, `--bg`, etc. dans `:root` en haut du `<style>` |
| Titre et sous-titre | balises `<h1>` et `<p class="sub">` |

Pour ajouter un champ (ex. « lieu ») :

1. Dans Supabase SQL Editor : `alter table public.demandes add column lieu text default '';`
2. Dans `index.html` : ajoute un `<input id="e-lieu">` dans le tiroir d'édition, puis `lieu: $("e-lieu").value` dans l'enregistrement et `$("e-lieu").value = i.lieu || ""` à l'ouverture.

---

## Limites de l'offre gratuite

- **Supabase Free :** 500 Mo de base de données, largement suffisant pour des milliers de demandes. Un projet sans activité pendant 7 jours est mis en pause : il suffit de le relancer depuis le tableau de bord (les données sont conservées).
- **E-mails de connexion :** le service d'e-mail intégré de Supabase est limité à quelques envois par heure. C'est suffisant pour une petite équipe. Pour plus de volume, configure un SMTP (ex. Brevo ou Resend, gratuits) dans **Authentication** → **Emails** → **SMTP Settings**.
- **GitHub Pages :** gratuit sur un dépôt public. Le code est visible, pas les données (protégées par la connexion).

---

## Dépannage

| Problème | Solution |
|---|---|
| Bandeau « Configuration manquante » | Les clés Supabase ne sont pas remplies dans `index.html` |
| « Mot de passe incorrect » alors qu'il est bon | Le compte `equipe@noname-calendar.app` n'existe pas ou n'est pas confirmé (étape Partager) |
| Le lien e-mail ouvre une page vide ou localhost | Vérifie la **Site URL** et les **Redirect URLs** (étape 5) |
| « Liste indisponible » | Le projet Supabase est en pause → relance-le depuis le tableau de bord |
| Les autres ne voient pas les changements en direct | Relance la ligne `alter publication supabase_realtime add table public.demandes;` |

# Demandes NONAME

Outil gratuit pour noter, suivre et partager les demandes de NONAME AGENCY SA (studio, label, services artistes).

- **Hébergement :** GitHub Pages (gratuit)
- **Base de données + connexion :** Supabase (offre gratuite)
- **Accès :** seules les personnes que tu invites peuvent se connecter et modifier

Le site est une seule page (`index.html`), sans installation ni build.

---

## Fonctionnalités

- **Tableau des demandes** : nom du client, demande, sessions studio, date de contact, statut et dernière modification
- **Plusieurs sessions studio par demande** : choisis autant de dates que nécessaire (chaque date devient une pastille). Clic sur une pastille = modifier la date, × = la retirer
- **Statut modifiable directement dans le tableau** : Nouvelle → En cours → En attente → Confirmée → Terminée / Annulée (pastilles de couleur)
- **Date de fin** : quand une demande passe en « Terminée », la date du jour est notée automatiquement (modifiable dans la fiche, champ « Date de fin »). Elle s'affiche sous le statut (« ✓ le 12.10 »), apparaît dans le calendrier (point vert) et dans le récap
- **Responsable obligatoire** : Johan, Michael, Arnaud ou Wiliam, chacun avec sa couleur (pastille dans le tableau, modifiable directement). Une demande sans responsable est signalée « ⚠ À assigner » ; filtre par responsable
- **Prix & paiements** (à remplir plus tard, depuis la fiche) : prix total, montant déjà payé, barre de progression, reste à payer. Bouton « + Encaisser » : ajoute le montant reçu et l'inscrit dans le journal. Colonne « Paiement » (Non payé / Acompte / Payé), filtre « À encaisser », carte « Paiements » (facturé / encaissé / reste) et section « À encaisser » dans le récap
- **Horaires des sessions** : menus « 🕐 de … à … » à côté du champ date (ex. 14h–16h, 18h30–19h) ; bouton 🕐 sur chaque pastille pour changer l'horaire. Affichés dans le tableau, les calendriers, la fiche client et les récaps
- **Saisie des dates fiable** : une date tapée (jj/mm/aaaa) est enregistrée même sans appuyer sur Entrée ; une date impossible (ex. 45/13/2026) est refusée avec un message
- **Profils** : à la connexion, chacun choisit son profil (Johan, Michael, Arnaud, Wiliam) ou **« ➕ Nouveau profil… »** : une barre permet d'écrire le nom voulu. Après connexion (bon mot de passe), ce profil s'ajoute à la liste pour tout le monde (table `profils`). Michael peut supprimer un profil créé depuis le panel admin
- **Panel admin (Michael uniquement)** : profil « Michael » + mot de passe **admin** (compte Supabase séparé `michael-admin@noname-calendar.app`). Bouton « 🛡️ Admin » : personnes connectées en ce moment, chiffres par responsable (demandes, sessions à venir, facturé / encaissé / reste), activité récente, export CSV. Avec le mot de passe d'équipe, Michael est connecté normalement, sans panel admin
- **Logs (panel admin)** : chaque création, modification et suppression de demande / profil est enregistrée **par la base de données elle-même** (déclencheur PostgreSQL, table `journal`) avec la date, le profil, le compte (Admin / Équipe, vérifié) et le détail « avant → après ». Lecture réservée au compte admin, aucune écriture possible depuis le site. Filtres, recherche, export CSV, bouton « ↩ Restaurer » pour une demande supprimée
- **Connexions & comptes (panel admin)** : historique des connexions au site (profil, compte, appareil), comptes Supabase et dernière connexion, rappel des règles de sécurité
- **Sessions du mois** listées sous le mini calendrier (date, client, horaire, couleur du responsable)
- **Filtre des calendriers** : les éléments de la légende (Session studio, Contact, Terminée, Modifié ce jour) sont des interrupteurs ; un clic masque / réaffiche ces éléments sur le mini et le grand calendrier (mémorisé sur l'appareil, contacts masqués par défaut)
- **Catégories** (Toutes, Actives, statuts…) repliées derrière le bouton « ☰ Catégorie »
- **Bouton ☀️ Clair / 🌙 Sombre** en haut (mémorisé sur l'appareil ; sinon suit le réglage de l'appareil)
- **Grille horaire (vue « Semaine », par défaut dans le grand calendrier)** : 7 jours × heures, chaque session calée sur son horaire par crans de 30 min. **Glisser-déposer** une session pour changer son jour / son horaire (sa durée est gardée) ; glisser une session de la ligne « Sans horaire » dans la grille pour lui donner un horaire ; clic sur un créneau vide = nouvelle session à ce jour et cette heure ; clic sur un jour = journée
- **Journée heure par heure** : clic sur un jour du grand calendrier, ou bouton « 🕐 Voir la journée » dans le détail du jour du mini calendrier → planning de 8h à 22h (élargi si besoin) avec chaque session à sa place, couleur du responsable, sessions qui se chevauchent côte à côte, ligne rouge = heure actuelle, jours précédent / suivant
- **Quand la session a été placée** : chaque nouvelle session enregistre la date, l'heure et le profil qui l'a ajoutée (« placée le 09/10/2026 14:53 par Michael »), visible dans la journée, le détail du jour, la fiche client et au survol des pastilles. Pour les sessions plus anciennes : « ≈ » date de création de la demande
- **Mini calendrier** : chaque jour de session est surligné (plus foncé s'il y a plusieurs réservations), un point bleu = contact, un point vert = terminée ce jour-là, un point orange = modifié ce jour-là
- **Effets visuels** : à chaque ajout ou modification (par toi ou un autre membre en direct), la ligne s'illumine et le jour concerné « pulse » dans le calendrier
- **Grand calendrier** — affiche sessions, contacts et fins ; vue « Mois » ou « Tout » (tous les événements passés et à venir), alerte des demandes sans date de session (bouton « 📅 Grand calendrier » en haut, ou « Ouvrir le grand calendrier » sous le mini calendrier) : mois entier en plein écran, nom de chaque session écrit dans la case, couleur du responsable ; fins (✓) et contacts (☎) affichables ; clic sur une session = fiche, clic sur un jour = nouvelle session. Sur téléphone : vue agenda jour par jour
- **Fiche client** : clic sur le nom d'un client dans le tableau (ou bouton « 👤 Fiche client » dans une fiche) → toutes ses demandes, un calendrier de ses sessions uniquement, la liste de ses sessions, ce qu'il/elle doit (facturé / payé / reste) et un bouton « Copier son récap » à lui envoyer
- Clic sur un jour : détail du jour, tableau filtré sur cette date, bouton « + Réservation ce jour »
- Compteurs : demandes actives, réservations sous 7 jours, réservations du mois, en attente
- Colonne latérale : prochaines réservations et dernières modifications
- Fiche détaillée : coordonnées, type, responsable, budget (CHF), notes, **updates datées et signées**, suppression
- Filtres par statut via le bouton « ☰ Catégorie » (« Toutes » par défaut), recherche, tri (réservation, modification, contact, client)
- Bouton « Copier le récap » : résumé à coller dans WhatsApp ou un mail
- **Dates au format jj/mm/aaaa** partout, avec un calendrier de saisie en français (on peut aussi taper la date au clavier, ex. 15/10/2026)
- **FR / EN** : bouton en haut à droite pour passer tout le site en anglais (mémorisé sur chaque appareil). Les noms et notes saisis ne sont pas traduits
- Accès par **un seul mot de passe d'équipe**, synchronisation en temps réel, mode clair / sombre, utilisable sur mobile

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
  statut     text default 'nouvelle' check (statut in ('nouvelle','en-cours','en-attente','confirmee','terminee','annulee')),
  echeance   date,
  date_reservation date,          -- première session (compatibilité / tri)
  sessions   date[] not null default '{}',  -- toutes les dates de session studio
  date_fin   date,                          -- date à laquelle la demande s'est terminée
  horaires   jsonb not null default '{}',   -- horaires par session : {"2026-10-15": {"d": "14:00", "f": "16:00"}}
  prix       numeric(10,2) check (prix is null or prix >= 0),  -- prix total (CHF)
  paye       numeric(10,2) not null default 0 check (paye >= 0),  -- montant déjà payé (CHF)
  date_contact     date,
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
- **anon / publishable key** (la clé publique)

Ouvre `index.html` et remplace en haut du script :

```js
const SUPABASE_URL = "https://VOTRE-PROJET.supabase.co";
const SUPABASE_ANON_KEY = "VOTRE_CLE_ANON_PUBLIQUE";
```

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

**Créer le compte admin de Michael (une seule fois)** : **Authentication** → **Users** → **Add user** → **Create new user** → e-mail `michael-admin@noname-calendar.app`, mot de passe admin choisi par Michael, coche **Auto Confirm User**. Le mot de passe n'est jamais écrit dans `index.html` (le dépôt est public) : c'est Supabase qui le vérifie.

**Changer le mot de passe** (ex. quelqu'un quitte l'équipe) : **Authentication** → **Users** → clic sur `equipe@noname-calendar.app` → **Reset password** / changer le mot de passe. Les sessions déjà ouvertes restent actives jusqu'à déconnexion ; pour éjecter tout le monde, supprime l'utilisateur et recrée-le.

---

## Modifier l'outil

Tout se trouve dans `index.html`. Modifie le fichier directement sur GitHub (icône crayon) : le site se met à jour en 1–2 minutes après le commit.

| Ce que tu veux changer | Où |
|---|---|
| Liste des types de demandes | `const TYPES = [...]` dans le script |
| Priorités | `const PRIOS = [...]` (+ contrainte `check` dans la table si tu ajoutes une valeur) |
| Statuts | `const STATUTS = [...]` (+ contrainte `check` dans la table) ; `const CLOS` = statuts considérés comme fermés |
| Responsables (noms et couleurs) | `const RESPS = [...]` dans le script + variables `--r-johan`, etc. dans `:root` |
| Traductions anglaises | dictionnaire `const EN = {...}` (texte français → anglais) et `const RULES` (phrases avec chiffres) en haut du script |
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

---

## Confidentialité

- Les données (demandes, prix, paiements) ne sont envoyées par la base **qu'après connexion** (règles RLS) ; le panel admin et les logs uniquement au compte admin.
- Le site n'est pas indexé par les moteurs de recherche (`noindex` + `robots.txt`).
- Pour cacher aussi le **code** : passer le dépôt GitHub en privé. GitHub Pages ne publie un dépôt privé qu'avec un abonnement payant (GitHub Pro) ; sinon, héberger le site sur Cloudflare Pages ou Netlify (gratuits, compatibles dépôts privés). Penser alors à mettre la nouvelle adresse dans Supabase → Authentication → URL Configuration.

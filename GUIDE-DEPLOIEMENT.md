# RusLearn — Guide de déploiement

## Structure des fichiers
```
ruslearn/
├── index.html          ← App complète
├── manifest.json       ← Config PWA
├── sw.js               ← Service worker (mises à jour auto)
└── icons/
    ├── icon-192.svg
    └── icon-512.svg
```

---

## ÉTAPE 1 — Créer la base de données Supabase (optionnel mais recommandé)

La progression fonctionne SANS Supabase (stockage local).
Avec Supabase, la progression est partagée entre appareils.

### 1.1 Créer un compte
→ https://supabase.com (gratuit)

### 1.2 Nouveau projet
- Nom : `ruslearn`
- Mot de passe DB : note-le

### 1.3 Créer la table `users`
Dans SQL Editor, colle et exécute :

```sql
create table users (
  id text primary key,
  name text not null,
  pin text not null,
  avatar text default '🦊',
  xp integer default 0,
  streak integer default 0,
  "completedLessons" jsonb default '[]'::jsonb,
  "lastActive" text,
  "createdAt" text
);

-- Accès public (lecture/écriture pour ton app)
alter table users enable row level security;
create policy "Public access" on users for all using (true);
```

### 1.4 Récupérer les clés
Settings → API → copie :
- `Project URL` → remplace `YOUR_SUPABASE_URL` dans index.html
- `anon public key` → remplace `YOUR_SUPABASE_ANON_KEY`

---

## ÉTAPE 2 — Déployer sur Vercel

### 2.1 Mettre le code sur GitHub
1. Va sur https://github.com → New repository → `ruslearn`
2. Crée les fichiers (ou upload le dossier)

### 2.2 Déployer sur Vercel
1. Va sur https://vercel.com → Sign up (avec GitHub)
2. New Project → importe `ruslearn`
3. Deploy → c'est tout !

### 2.3 Ton lien
Vercel donne un lien du type : `https://ruslearn-xxxx.vercel.app`

**Chaque `git push` = mise à jour automatique en 30 secondes.**

---

## ÉTAPE 3 — Installation sur les téléphones

### Android
1. Ouvre le lien dans Chrome
2. Menu (⋮) → "Ajouter à l'écran d'accueil"
3. L'app s'installe comme une vraie app

### iPhone / iPad
1. Ouvre le lien dans Safari (pas Chrome !)
2. Bouton Partager (□↑) → "Sur l'écran d'accueil"
3. L'app s'installe

---

## Mises à jour
Chaque modification du code sur GitHub = déploiement automatique Vercel.
Les membres de la famille voient la mise à jour au prochain lancement.

---

## Ajouter des leçons
Dans `index.html`, trouve le tableau `LESSONS` et ajoute :
```js
{
  id: 6,
  title: 'Animaux',
  emoji: '🐾',
  xpReward: 40,
  questions: [
    { type: 'Vocabulaire', q: '...', hint: '...', options: [...], correct: 0 },
    ...
  ]
}
```

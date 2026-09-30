# Mise en place de « Mon cartable – Maths 5e »

Aucun code à écrire : tu copies-colles quelques identifiants dans le bloc `CONFIG` en haut de `index.html` (ouvre-le avec le Bloc-notes ou directement dans l'éditeur de GitHub).

## 1. Mettre l'appli en ligne (GitHub Pages) — 5 min

1. Sur github.com, crée un dépôt, par exemple `maths-benjamin` (en **public**, obligatoire pour GitHub Pages gratuit).
2. Bouton « Add file » → « Upload files » : dépose `index.html`, `icon.png` et `manifest.json`. Valide (« Commit changes »).
3. Onglet **Settings** → **Pages** → Source : « Deploy from a branch », branche `main`, dossier `/ (root)` → Save.
4. Au bout d'une minute, l'adresse apparaît : `https://ton-pseudo.github.io/maths-benjamin/`.

À ce stade l'appli marche déjà, mais les progrès restent sur l'appareil utilisé et il n'y a pas de mail.

## 2. Synchroniser les appareils (Supabase) — 10 min

1. Sur supabase.com, crée un nouveau projet (gratuit).
2. Menu **SQL Editor** → colle ceci et clique « Run » :

```sql
create table tirelire (
  id text primary key,
  data jsonb not null,
  updated_at timestamptz default now()
);
alter table tirelire enable row level security;
create policy "acces appli" on tirelire for all to anon using (true) with check (true);
```

3. Menu **Project Settings** → **API** (ou « API Keys ») : copie l'**URL du projet** et la clé **anon / publishable**.
4. Dans `index.html`, remplis :

```js
supabase: { url: "https://xxxx.supabase.co", cle: "ta-clé-anon", table: "tirelire", eleveId: "benjamin" },
```

## 3. Recevoir le mail du lundi (EmailJS) — 10 min

1. Sur emailjs.com, crée un compte gratuit (200 mails/mois).
2. **Email Services** → « Add New Service » → Gmail (ou ton fournisseur) → connecte ton adresse. Note le **Service ID**.
3. **Email Templates** → « Create New Template » :
   - **To Email** : ton adresse mail
   - **Subject** : `{{sujet}}`
   - **Content** : `{{message}}`
   - Enregistre et note le **Template ID**.
4. **Account** → **General** : copie la **Public Key**.
5. Dans `index.html`, remplis :

```js
emailjs: { publicKey: "xxxx", serviceId: "service_xxxx", templateId: "template_xxxx" }
```

Tu reçois un mail récapitulatif (montant, notes, détail) au premier lancement de la semaine suivante, puis un second quand Benjamin confirme avoir reçu le virement.

## 4. Premier lancement

1. Ouvre l'adresse de l'appli, va dans **Espace parent** (en bas) et crée ton code parent.
2. Clique **Tester le mail** pour vérifier que tout est branché.
3. Sur chaque appareil de Benjamin, ajoute l'appli à l'écran d'accueil :
   - iPhone / iPad : Safari → bouton Partager → « Sur l'écran d'accueil »
   - Android : Chrome → menu ⋮ → « Ajouter à l'écran d'accueil »

## Fonctionnement en bref

- 20 min maximum par jour, décomptées pendant les rappels et les exercices.
- Chaque séance : un rappel de cours de 3 petites questions, puis des exercices de 10 questions.
- Les **2 premiers exercices du jour** comptent pour la tirelire, sur **4 jours par semaine** maximum. Les suivants sont de l'entraînement.
- Gains (note sur 10) : 7 → 0,60 € · 8 → 1,20 € · 9 → 1,80 € · 10 → 2,50 €. Soit environ 5 € avec 7/10 partout et 20 € avec 10/10 partout (plafond 20 €).
- Chaque lundi, une nouvelle tirelire démarre à 0. L'ancienne reste affichée « à verser » jusqu'à ce que Benjamin confirme la réception (tu peux aussi la marquer reçue depuis l'espace parent).
- Les thèmes se débloquent au fil de l'année (septembre → juin). Tu peux tout débloquer dans l'espace parent.

## Modifier les réglages

Tout est dans le bloc `CONFIG` en haut de `index.html` : durée, nombre d'exercices notés, barème, plafond. Sur GitHub, clique sur `index.html` → icône crayon → modifie → « Commit changes ».

## Bon à savoir

- Le dépôt étant public, la clé Supabase est visible : elle ne donne accès qu'au tableau de la tirelire (prénom, notes, montants). N'y mets rien d'autre.
- Un ado débrouillard pourrait en théorie trafiquer les données en fouillant le code. Les notes détaillées dans le mail te permettent de repérer une incohérence.
- Pour ajouter d'autres matières plus tard, il suffira de compléter la liste des thèmes : demande-moi, je m'en charge.

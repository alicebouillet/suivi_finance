# Le Carnet — suivi de dépenses

App web statique (pas de build, pas de npm) : React + Supabase, en un seul fichier `index.html`.

## 1. Base de données Supabase

Dans ton projet Supabase → **SQL Editor** → colle et exécute le contenu de `schema.sql`.

Ça crée 4 tables :
- `categories` (logement, voiture, loisirs, épargne — modifiables)
- `periods` (chaque période "salaire à salaire")
- `period_allocations` (montant alloué par catégorie et par période)
- `expenses` (chaque dépense saisie)

et active l'accès pour la clé publique `anon` (RLS ouverte pour vous deux, sans compte à créer).

## 2. Configuration

Ouvre `config.js` et remplace :
```js
const SUPABASE_URL = "https://TON-PROJET.supabase.co";
const SUPABASE_ANON_KEY = "TA_CLE_ANON_PUBLIC";
```
Ces deux valeurs sont dans Supabase → **Project Settings → API**. La clé `anon` est publique par design (c'est la RLS qui protège les données), mais évite quand même de partager l'URL au-delà de vous deux.

## 3. Déploiement sur GitHub Pages

1. Crée un dépôt GitHub (ex. `carnet-depenses`), mets-le en **privé** si tu veux limiter l'accès.
2. Pousse ces 3 fichiers (`index.html`, `config.js`, `schema.sql` peut rester ou non) à la racine.
3. Dans **Settings → Pages**, source = branche principale, dossier `/root`.
4. L'app sera dispo à `https://ton-pseudo.github.io/carnet-depenses/`.

Sur ton téléphone, ouvre le lien puis "Ajouter à l'écran d'accueil" pour l'utiliser comme une app.

## Fonctionnement

- **Périodes** : va du 25 d'un mois au 24 du suivant par défaut (ajustable en créant la première période à la bonne date). Les flèches ← → créent automatiquement la période précédente/suivante si besoin.
- **Catégories** : logement, voiture, loisirs, épargne sont préremplies (fixe/variable + montant alloué par défaut). Modifiables directement dans Supabase (table `categories`) si tu veux en ajouter, en renommer, ou changer les couleurs/défauts.
- **Montants alloués** : repris du défaut de chaque catégorie à la création d'une période, mais ajustables période par période via "Ajuster les montants alloués".
- **Dépenses** : ajoutées à la main (catégorie, description, montant, date, prénom), visibles et supprimables dans la liste de la période.
- **Partagé à deux** : pas de compte, l'app retient juste ton prénom localement sur ton téléphone pour le préremplir.

## Pour ajouter une catégorie plus tard

Dans Supabase, table `categories`, insère une ligne avec `name`, `type` (`fixe` ou `variable`), `color` (code hex), `default_allocated` et `sort_order`. Elle apparaîtra à la prochaine période créée (les périodes déjà existantes gardent leurs allocations telles quelles).

# ON2GO

**App ID Clover (production) :** `N4NYCJCZ17FF4`
**Développeur :** TAKATAK (compte développeur Clover 9KJ25ATT15TTW)
**État au portail :** brouillon (draft) — rien de soumis.
**Dépôt :** ⚠️ AUCUN dépôt GitHub dédié — dossier local seulement (voir DEPLOY.md).

## Ce que fait l'app

Annuaire public de restaurants + porte d'entrée de commande directe sans commission de plateforme : fiche publique (nom, cuisines, heures, lien de commande), plats publics, recherche par marque/cuisine/établissement. Contrat public : lecture seule, aucune donnée de commande/client/paiement exposée. Jumeau sandbox : On2GO TE77CFFZC9394.

## Paliers (doctrine de prix de Matt, 2026-10-28)

Chaque app a SES propres paliers. Base = le nécessaire essentiel · Premium = plus d'avantages · VIP = le full kit, tout ce que Clover offre pour la catégorie.

- **Base 50 $/mois** — fiche publique alimentée par le profil marchand et les heures ; articles, catégories et prix publics lus dans l'inventaire ; lien direct vers la commande du restaurant.
- **Premium 100 $/mois** — + disponibilité synchronisée (articles épuisés retirés via webhooks Inventaire) ; plats vedettes par étiquettes ; tendances de classement publiques (agrégats sans données clients).
- **VIP 150 $/mois** — full kit : commande directe complète branchée Food Hub (la commande devient une commande Clover) ; multi-succursales et multi-marques ; arrimage M4RKETING (offres publiques) et LOY4LTY (client reconnu par téléphone au checkout).

Prix facturés via les plans d'abonnement du Clover App Market (à créer au portail — voir DEPLOY.md).

## Permissions Clover visées

Merchant R · Inventory R (articles publics seulement)

## Structure

- `docs/listing/` — fiche marketplace (description FR/EN) et textes légaux, depuis le package de soumission local (`~/workspace/clover-apps/`)
- `app/api/clover/` — points d'entrée webhook + OAuth (scaffold, même patron que Food Hub)
- `lib/clover/config.ts` — configuration par variables d'environnement (aucun secret dans le code)
- `config/app.config.json` — identité de l'app et paliers
- `DEPLOY.md` — ce qui reste à faire, étape par étape, pour la vague portail

## Le système unique TAKATAK

Cette application fait partie de la suite TAKATAK sur Clover : toutes les apps partagent le même dos d'événements (webhooks Clover), le même **client unique reconnu par son numéro de téléphone** (+ courriel), et les mêmes commandes et paiements Clover. Aucune app ne vit en silo : les données qu'elle produit alimentent les autres apps de la suite (voir « Clover API — Main Stuff — Robot Notepad », section 3, déposé au Drive Opérations et dans `~/workspace/clover-apps/`).


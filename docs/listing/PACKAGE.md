# On2GO — Package de soumission Clover App Market

**Statut du document :** brouillon de préparation — rien n’a été publié ni soumis.  
**Préparé le :** 24 octobre 2026  
**Propriétaire du projet :** Matt, TAKATAK Marketing, Montréal  
**Assistance :** takatak.ca@gmail.com · +1 450-999-4999

## Ce qu’est On2GO, selon les sources locales

On2GO est l’**annuaire public de restaurants et la porte d’entrée de commande directe sans commission de plateforme**. Le plan des sites dit expressément qu’**on2go.ca est l’annuaire lui-même**, et non un autre site de restaurant. Food Hub expose les données publiques de l’annuaire; chaque marque peut aussi recevoir une page de commande Food Hub à `/commander/<slug>`.

Le contrat public documenté est `GET /api/public/directory` : lecture seule, limité par CORS, avec marques, cuisines, heures, liens publics de commande, plats et tendances de classement seulement. Il ne doit jamais exposer de commandes, de clients, de totaux, de paiements aux marchands, d’identifiants marchands ou de jetons.

Le fichier graine inspecté contient 2 cuisines, 17 marques et 151 plats. Il a été généré le 2026-10-09 à partir de la configuration de marque pppmtl et d’un export de menu Clover du 2026-10-07.

**Distinction importante :** les sources distinguent aussi **ON2GO Hub**, nom marchand envisagé pour la plateforme anciennement appelée Food Hub, et **TAKATAK Dashboard**, le plan de contrôle des comptes et de la facturation. Ce package vise l’application **On2GO côté annuaire/commande directe**. Il ne doit pas fusionner ces produits sans décision de Matt.

### Sources

- `~/workspace/sites-restaurants/PLAN-SITES.md`
- `~/workspace/sites-restaurants/foodhubca/docs/ON2GO_HUB_ECOSYSTEM.md`
- `~/workspace/sites-restaurants/foodhubca/data/public/directory-seed.json`
- `~/workspace/sites-restaurants/foodhubca/lib/foodhub/public-directory.ts`

## Nom de l’application

**On2GO**

## Slogan / Tagline

**FR :** Trouve un resto près de toi et commande en direct, sans détours.  
**EN :** Find a restaurant near you and order direct, without the detours.

## Description

### Français

On2GO aide les clients à découvrir des restaurants, consulter leurs heures, voir leurs plats publics et passer à la commande directe par le lien du restaurant. Pour un marchand Clover participant, l’application sert de connecteur d’annuaire : elle publie seulement les renseignements publics approuvés par le marchand, synchronise les renseignements de menu nécessaires à la fiche et dirige le client vers le parcours de commande choisi par le restaurant.

L’objectif est simple : donner au restaurant une vitrine directe et donner au client un chemin court vers le bon établissement, le bon menu et la bonne succursale. On2GO ne remplace ni le système de caisse Clover, ni les obligations du restaurant envers ses clients. Les prix, les heures, la disponibilité et l’exécution de la commande restent sous le contrôle du marchand.

Fonctions prévues pour la fiche Clover :

- fiche publique du restaurant avec nom, cuisine, heures et lien de commande;
- lecture des articles, catégories et prix publics depuis Clover;
- repérage des articles indisponibles lorsque le marchand active la synchronisation;
- lien direct vers la commande du restaurant, sans commission de plateforme facturée par On2GO sur la commande;
- recherche par marque, cuisine et établissement;
- affichage de plats publics et de tendances de classement qui ne contiennent aucune donnée de client ni de transaction;
- option de compte client pour retrouver ses restaurants et ses commandes, si Matt confirme cette portée.

### English

On2GO helps customers discover restaurants, check their hours, browse public dishes and continue to direct ordering through the restaurant’s own ordering link. For a participating Clover merchant, the app acts as a directory connector: it publishes only public information approved by the merchant, synchronizes the menu information needed for the listing, and sends the customer to the ordering path selected by the restaurant.

The goal is simple: give the restaurant a direct storefront and give the customer a short path to the right business, menu and location. On2GO does not replace the Clover point of sale or the restaurant’s obligations to its customers. Prices, hours, availability and order fulfilment remain under the merchant’s control.

Planned App Market features:

- public restaurant listing with name, kitchen, hours and ordering link;
- read access to public Clover items, categories and prices;
- unavailable-item awareness when the merchant enables synchronization;
- a direct link to the restaurant’s ordering flow, with no On2GO platform commission charged on the order;
- search by brand, cuisine and location;
- public dishes and ranking-only trends containing no customer or transaction data;
- optional customer account for saved restaurants and order history, if Matt confirms that scope.

## Catégorie proposée

- **Primaire :** Orders & Delivery
- **Secondaires :** Customer Engagement; Items & Inventory (lecture)
- **Verticaux :** Quick Service Restaurant; Full Service Restaurant

**À valider dans le portail :** les libellés exacts offerts par Clover au moment de la soumission.

## Fonctions détaillées

| Fonction | État dans les sources | Traitement dans ce package |
|---|---|---|
| Annuaire public on2go.ca | Établi | Fonction centrale |
| API publique en lecture seule | Établie | Aucune donnée privée dans le flux public |
| Page de commande directe par marque | Établie dans l’écosystème Food Hub | Lien/sortie vers le parcours du marchand |
| Synchronisation du menu Clover | Cohérente avec les sources, portée exacte à confirmer | Permission Inventory Read proposée |
| Création d’une commande directement dans Clover | Non établie comme fonction actuelle d’on2go.ca | Fonction optionnelle, seulement si Matt confirme |
| Compte consommateur et historique | Application consommateur décrite comme projet/PWA dans l’écosystème | À confirmer avant soumission |
| Fidélité dans l’application consommateur | Mentionnée comme possibilité future | Ne pas promettre dans la fiche tant que la règle de points n’est pas confirmée |

## Permissions Clover demandées — proposition justifiée

### Permissions de base proposées

| Permission | Accès | Justification |
|---|---:|---|
| Merchant | Read | Lire le nom public, l’adresse et les renseignements d’établissement nécessaires pour associer la bonne fiche d’annuaire au bon marchand et éviter les doublons entre succursales. |
| Inventory | Read | Lire les catégories, articles, prix et états de disponibilité publics afin d’afficher une fiche et un menu à jour. Aucune modification du catalogue dans le mode annuaire de base. |
| Orders | Read | Seulement si le marchand active le suivi de commande directe : vérifier le statut d’une commande créée par le parcours autorisé et l’afficher au client ou au marchand. |

### Permissions conditionnelles — ne pas demander sans confirmation de portée

| Permission | Accès | Condition |
|---|---:|---|
| Orders | Write | Uniquement si On2GO doit créer la commande dans Clover au lieu de rediriger vers un parcours de commande existant. |
| Payments | Read | Uniquement pour rapprocher le statut payé/non payé d’une commande directe. On2GO ne doit pas demander cette permission pour un simple lien externe. |
| Customers | Read + Write | Uniquement si le compte consommateur, l’historique et la fidélité sont intégrés au produit soumis. |

**Principe :** demander le minimum. La version « annuaire + lien de commande » ne devrait pas demander Payments Write ni Customers sans fonction réelle correspondante.

## Tarification — PROPOSITION SEULEMENT

**Proposition à valider par Matt avant toute saisie dans Clover :**

- inscription à l’annuaire : **gratuite**;
- commission On2GO sur les commandes directes : **0 %**;
- module avancé de synchronisation et de suivi, si Matt décide de le monétiser : prix mensuel par emplacement **à déterminer**.

Aucun prix d’abonnement On2GO n’est établi dans les sources consultées pour ce package. Ne pas publier de prix tant que Matt n’a pas choisi le modèle.

## Assistance

- **Courriel :** takatak.ca@gmail.com
- **Téléphone :** +1 450-999-4999
- **Langues :** français et anglais
- **Heures d’assistance :** À CONFIRMER
- **Page d’assistance :** À CONFIRMER — ne pas inventer d’URL

## Checklist de soumission

- [x] Nom et positionnement distingués de Food Hub et de TAKATAK Dashboard
- [x] Description FR et EN préparée
- [x] Permissions minimales et conditionnelles séparées
- [x] Script vidéo FR de 60–90 secondes préparé
- [x] Politique de confidentialité FR/EN préparée en brouillon
- [x] Conditions d’utilisation FR/EN préparées en brouillon
- [ ] Matt confirme : application marchand Clover, application consommateur, ou les deux
- [ ] Matt confirme si On2GO crée des commandes dans Clover ou redirige seulement
- [ ] Matt confirme le modèle de prix
- [ ] Entité légale, adresse et responsable de la protection des renseignements personnels confirmés
- [ ] Textes juridiques approuvés
- [ ] URL publiques des pages juridiques et d’assistance confirmées
- [ ] Logo, icône et captures d’écran finaux approuvés
- [ ] Essai en sandbox avec un marchand de test
- [ ] Aucun renseignement privé dans le flux public vérifié par test

## À CONFIRMER

1. **Portée Clover exacte :** On2GO est-elle une application installée par le marchand pour alimenter sa fiche, une application consommateur, ou un produit hybride? Le Clover App Market est principalement marchand; la réponse change les permissions et la fiche.
2. **Nom du produit marchand :** faut-il utiliser « ON2GO Hub » pour la console marchand et réserver « On2GO » au consommateur, comme le suggère le document d’écosystème?
3. **Commande directe :** redirection vers Clover Online Ordering ou un autre lien du marchand, ou création native d’une commande Clover par On2GO?
4. **Compte consommateur :** inclus au lancement ou reporté à la PWA/application future?
5. **Fidélité :** exclue de cette fiche ou intégrée? Si intégrée, quelle règle de points et quel lien avec les autres applications TAKATAK?
6. **Tarification :** annuaire gratuit seulement, abonnement marchand, ou autre modèle?
7. **Données de localisation :** recherche par ville/secteur seulement, ou géolocalisation précise de l’utilisateur? La géolocalisation précise exige une justification et des textes adaptés.
8. **Entité responsable :** nom légal, adresse et responsable Loi 25 à inscrire dans les documents juridiques.
9. **Heures et canaux d’assistance officiels.**
10. **État réel de production :** les sources décrivent l’annuaire et son contrat; l’état public actuel doit être vérifié avant de promettre une disponibilité.

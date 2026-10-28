# Logii pour Claude

[Français](#français) | [English](#english)

## Français

Logii dans Claude : consultez les données Logii de votre cabinet en langage courant — acteurs et immeubles, lots, portefeuilles, mandats de gestion et de syndic, baux et vacants, comptes locataires et propriétaires, budgets, contrats fournisseurs, ordres de service, dossiers de copropriété et honoraires du cabinet. Lecture seule ; chaque personne se connecte avec son propre accès Logii.

Dans claude.ai : **Customize > Plugins > Add marketplace** (Personnaliser > Plugins >
Ajouter une marketplace), collez `https://github.com/vantageos-agency/vantage-logii-claude`, puis installez **logii**.

Ce que vous obtenez :

- Le connecteur Logii (`https://logii.vantagepeers.com/mcp`). Claude vous demande de vous connecter la
  première fois qu'il l'utilise, avec votre propre accès Logii : vous voyez les données
  de votre cabinet, selon vos droits. Aucune clé n'est stockée dans le plugin.
- Lecture seule : le plugin ne modifie rien dans Logii.
- Compétences, une par métier du cabinet :
  - `demarrer`: Démarrer avec Logii : vérifier que le connecteur Logii répond et dire ce qu'il permet de consulter.
  - `trouver-un-acteur`: Trouver une personne, une société ou un immeuble dans Logii et lire sa fiche d'identité.
  - `lots-d-un-immeuble`: Lister les lots d'un immeuble, ou retrouver un lot précis, dans Logii.
  - `portefeuilles`: Lister les portefeuilles de gestion du cabinet, leurs agences et les collaborateurs qui les gèrent.
  - `mandats-et-echeances`: Consulter les mandats de gestion et de syndic et leurs échéances dans Logii.
  - `baux-et-vacants`: Consulter les baux, les lots vacants et les candidatures dans Logii.
  - `compte-locataire`: Lire la situation comptable d'un locataire dans Logii : quittancé, réglé, reste à payer.
  - `compte-proprietaire`: Lire les écritures du compte d'un propriétaire dans Logii.
  - `budgets`: Consulter les budgets propriétaire dans Logii : prévu, appelé, réalisé, et le détail par rubrique.
  - `contrats-fournisseurs`: Consulter les contrats fournisseurs de la gérance dans Logii.
  - `ordres-de-service`: Suivre les ordres de service et les travaux de la gérance dans Logii.
  - `dossier-copropriete`: Ouvrir le dossier d'une copropriété dans Logii : fiche, copropriétaires, conseil syndical, mandat de syndic.
  - `honoraires-du-cabinet`: Suivre les honoraires du cabinet dans Logii : facturés, lettrés, encaissés, sur une période.
  - `point-hebdomadaire`: Préparer le point hebdomadaire du cabinet à partir de Logii : échéances de mandats, vacants, ordres de service et honoraires.

Pour commencer, demandez « Qu'est-ce que Logii peut faire pour moi ? » ou lancez la
compétence `demarrer`.

Les compétences s'appuient sur la documentation officielle « Lojii - Les API
disponibles » et sur la spécification OpenAPI de l'éditeur (openapi-1.0.0.json). Quand
la documentation ne définit pas une notion, Claude le dit au lieu de l'interpréter.

Support : hello@vantageos.agency. Licence : voir `LICENSE` (Functional Source License 1.1,
licence future Apache 2.0).

## English

Logii for Claude: read your agency's Logii data in plain words — people and buildings, lots, portfolios, management and syndic mandates, leases and vacancies, tenant and owner accounts, budgets, supplier contracts, work orders, condominium files and agency fees. Read-only; each person signs in with their own Logii access.

Add it in claude.ai: **Customize > Plugins > Add marketplace**, paste
`https://github.com/vantageos-agency/vantage-logii-claude`, then install **logii**.

What it gives you:

- The Logii connector (`https://logii.vantagepeers.com/mcp`). Claude asks you to sign in the first time
  it uses it, with your own Logii access: you see your agency's data, within your
  rights. No key is stored in the plugin.
- Read-only: the plugin changes nothing in Logii.
- Skills: `demarrer`, `trouver-un-acteur`, `lots-d-un-immeuble`, `portefeuilles`, `mandats-et-echeances`, `baux-et-vacants`, `compte-locataire`, `compte-proprietaire`, `budgets`, `contrats-fournisseurs`, `ordres-de-service`, `dossier-copropriete`, `honoraires-du-cabinet`, `point-hebdomadaire`.

The skills follow the official documentation "Lojii - Les API disponibles" and the
publisher's OpenAPI specification (openapi-1.0.0.json). Where the documentation does
not define a notion, Claude says so instead of interpreting it.

Support: hello@vantageos.agency. License: see `LICENSE` (Functional Source License 1.1,
Apache 2.0 future license).

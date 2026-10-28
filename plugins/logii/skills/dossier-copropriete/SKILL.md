---
name: dossier-copropriete
description: >
  Ouvrir le dossier d'une copropriété dans Logii : fiche, copropriétaires, conseil syndical, mandat de syndic. Open a condominium file in Logii: its card, co-owners, owners' council, syndic mandate.
  Use when the user asks about a copropriété, its treasury, debtors, co-owners, council members (conseil syndical) or its syndic mandate.
user-invocable: true
---

## Français

Pour réunir la fiche d'une copropriété gérée en syndic, ses copropriétaires et son mandat.

### Services Logii utilisés

- **Copropriétés** : outil `get_condominium`. Documentation « Lojii - Les API disponibles », section 4. Syndic de copropriété ; spécification openapi-1.0.0.json · /api/api/v1/copropriete. La documentation : « Copropriétés : identité et adresse complète de la copropriété, numéro d'immatriculation au registre, SIRET et code APE, nombre de lots principaux et secondaires, nombre de copropriétaires, ainsi que des indicateurs financiers de synthèse : solde de trésorerie, total débiteur et honoraires contractuels ».
- **Copropriétaires** : outil `list_co_owners`. Documentation « Lojii - Les API disponibles », section 4. Syndic de copropriété ; spécification openapi-1.0.0.json · /api/api/v1/coproprietaire. La documentation : « Copropriétaires rattachés à leurs copropriétés, date d'entrée dans le portefeuille, appartenance au conseil syndical, adresse de la copropriété associée. Filtrage possible sur les relations actives ou passées ».
- **Mandats de copropriété** : outil `list_syndic_mandates`. Documentation « Lojii - Les API disponibles », section 4. Syndic de copropriété ; spécification openapi-1.0.0.json · /api/api/v1/mandat_copropriete. La documentation : « Mandats de copropriété : numéro et date de début du mandat, état, date de fin de l'exercice en cours, coordonnées bancaires du mandat, compteurs de lots et de copropriétaires, soldes créditeurs et débiteurs, solde de trésorerie ».

### Ce qu'il faut avoir avant d'appeler

- `get_condominium` : l'identifiant de la copropriété, ou un nom (au moins deux
  caractères). Un nom peut renvoyer plusieurs fiches : demander laquelle. La liste des lots
  ("include_lots") exige l'identifiant.
- `list_co_owners` : une copropriété (nom ou identifiant) ou un nom de copropriétaire.
  Par défaut les relations actives ; "active" à faux pour les relations passées ;
  "council_only" pour ne garder que le conseil syndical.

### Lire la réponse

- Fiche : "title", "address", "lots" (principaux, secondaires — « parkings, caves,
  etc. » selon la spécification — et total), "co_owners_count" (« Nombre de
  copropriétaires titulaires actifs »), "treasury_balance" (« Solde actuel du compte de trésorerie (banque) »), "debtors_total" (« Montant total dû par les copropriétaires débiteurs »), "contractual_fees" (« Montant des honoraires contractuels sur l'exercice
  ouvert »), "mandate".
- Copropriétaires : "name", "condominium", "council_member" (« Fonction occupée au sein du Conseil Syndical si applicable »). Si "council_member" est vide, Logii ne dit rien :
  ne pas conclure que la personne n'est pas au conseil ; "council_data_present" indique si
  la donnée existe dans la réponse. Seul le nom est servi, sans coordonnées.
- Mandat : "state", "phase", "start", "end", "fiscal_year_end", "debtors" et
  "creditors" (nombre et total), "fees", "lots", "co_owners_count". Les coordonnées
  bancaires du mandat ne sont jamais servies.

L'immatriculation, le SIRET, le code APE et la date d'entrée dans le portefeuille que cite
la documentation ne sont pas servis par ce connecteur.

### Ce que la documentation ne définit pas

- Les « impayés de charges » d'un copropriétaire nommé : non défini par la documentation.
  Le service donne un total débiteur pour la copropriété, pas la dette de chacun.
- Le rôle précis au conseil syndical (président, membre) au-delà de la fonction renvoyée :
  non défini par la documentation.

### Si l'outil refuse

Un refus n'est jamais « aucun résultat » : le dire comme un refus, en mots simples. L'outil
renvoie un message suivi d'un code entre crochets.

- [filter_required] : une entrée obligatoire manque. Demander à l'utilisateur l'une des
  entrées listées dans "accepted" (ou toutes celles listées dans "missing"), puis relancer.
- [criteria_required] : aucun critère de recherche n'a été donné ; en demander un.
- [not_found] : aucun enregistrement ne correspond à cet identifiant.
- [rate_limited] ou [neoteem_rate_limited] : trop d'appels en peu de temps ; réessayer dans
  un instant.
- [neoteem_ceiling] : le résultat est trop volumineux ; resserrer la recherche (une période
  plus courte, un immeuble précis).
- [neoteem_conflicting_rows] : Logii a renvoyé le même identifiant avec deux contenus
  différents ; rien n'est servi ; le signaler à l'administrateur Logii du cabinet.
- [neoteem_unauthorized] : Logii a refusé l'accès ; contacter l'administrateur du cabinet.
- Autre code commençant par [neoteem_ (Logii indisponible, réponse illisible) : réessayer
  plus tard, sans inventer de réponse.
- Si le connecteur Logii n'est pas connecté, ou si Claude demande une connexion : inviter
  la personne à se connecter avec son propre accès Logii (réglages des connecteurs de
  Claude). Elle voit les données de son cabinet, selon ses droits.

### Règles

1. Ne rapporter que ce que l'outil a renvoyé : aucun nom, aucune date, aucun montant qui ne
   vienne pas de la réponse.
2. Donner à chaque champ le sens que la documentation lui donne (cité ci-dessus). Quand la
   documentation ne dit rien d'un champ ou d'une notion, écrire « non défini par la documentation », jamais une interprétation.
3. Ne jamais inventer une entrée obligatoire (un identifiant, une période, un code) : la
   demander. Un nom donné par l'utilisateur se transforme en identifiant par un appel, pas
   par une supposition.
4. Une réponse marquée "truncated" est partielle : le dire et proposer la page suivante
   ("nextCursor"). Un "upstream_defect" signifie que Logii a renvoyé des doublons, retirés
   par le connecteur : le dire.
5. Lecture seule : ce connecteur ne modifie rien dans Logii.

## English

To bring together the card of a condominium managed as syndic, its co-owners and its
mandate.

### Logii services used

- **Copropriétés**: tool `get_condominium`. Documentation "Lojii - Les API disponibles", section 4. Syndic de copropriété; specification openapi-1.0.0.json · /api/api/v1/copropriete. The documentation: "Copropriétés : identité et adresse complète de la copropriété, numéro d'immatriculation au registre, SIRET et code APE, nombre de lots principaux et secondaires, nombre de copropriétaires, ainsi que des indicateurs financiers de synthèse : solde de trésorerie, total débiteur et honoraires contractuels".
- **Copropriétaires**: tool `list_co_owners`. Documentation "Lojii - Les API disponibles", section 4. Syndic de copropriété; specification openapi-1.0.0.json · /api/api/v1/coproprietaire. The documentation: "Copropriétaires rattachés à leurs copropriétés, date d'entrée dans le portefeuille, appartenance au conseil syndical, adresse de la copropriété associée. Filtrage possible sur les relations actives ou passées".
- **Mandats de copropriété**: tool `list_syndic_mandates`. Documentation "Lojii - Les API disponibles", section 4. Syndic de copropriété; specification openapi-1.0.0.json · /api/api/v1/mandat_copropriete. The documentation: "Mandats de copropriété : numéro et date de début du mandat, état, date de fin de l'exercice en cours, coordonnées bancaires du mandat, compteurs de lots et de copropriétaires, soldes créditeurs et débiteurs, solde de trésorerie".

### What you need before calling

- `get_condominium`: the condominium identifier, or a name (at least two characters). A
  name can return several cards: ask which one. The lot list ("include_lots") requires the
  identifier.
- `list_co_owners`: a condominium (name or identifier) or a co-owner name. Active relations
  by default; "active" false for past relations; "council_only" to keep only the council.

### Reading the answer

- Card: "title", "address", "lots" (main, secondary — "parkings, caves, etc." per the
  specification — and total), "co_owners_count" ("Nombre de copropriétaires titulaires
  actifs"), "treasury_balance" ("Solde actuel du compte de trésorerie (banque)"),
  "debtors_total" ("Montant total dû par les copropriétaires débiteurs"),
  "contractual_fees" ("Montant des honoraires contractuels sur l'exercice ouvert"),
  "mandate".
- Co-owners: "name", "condominium", "council_member" ("Fonction occupée au sein du Conseil Syndical si applicable"). When "council_member" is empty, Logii says nothing: do
  not conclude the person is not on the council; "council_data_present" says whether the
  data exists in the answer. Only the name is served, no contact details.
- Mandate: "state", "phase", "start", "end", "fiscal_year_end", "debtors" and
  "creditors" (count and total), "fees", "lots", "co_owners_count". The mandate's bank
  details are never served.

The registration number, SIRET, APE code and portfolio entry date the documentation
mentions are not served by this connector.

### What the documentation does not define

- A named co-owner's unpaid charges: not defined by the documentation. The service gives a
  debtor total for the condominium, not each person's debt.
- The exact council role (chair, member) beyond the role returned: not defined by the documentation.

### If the tool refuses

A refusal is never "no results": say it is a refusal, in plain words. The tool returns a
message followed by a code in square brackets.

- [filter_required]: a required input is missing. Ask the user for one of the inputs listed
  in "accepted" (or all of those listed in "missing"), then call again.
- [criteria_required]: no search criterion was given; ask for one.
- [not_found]: no record matches this identifier.
- [rate_limited] or [neoteem_rate_limited]: too many calls in a short time; try again in a
  moment.
- [neoteem_ceiling]: the result is too large; narrow the search (a shorter period, one
  building).
- [neoteem_conflicting_rows]: Logii returned the same identifier with two different
  contents; nothing is served; report it to the agency's Logii administrator.
- [neoteem_unauthorized]: Logii refused access; contact the agency's administrator.
- Any other code starting with [neoteem_ (Logii unavailable, unreadable answer): try again
  later, without inventing an answer.
- If the Logii connector is not connected, or Claude asks to connect: invite the person to
  sign in with their own Logii access (Claude's connector settings). They see their own
  agency's data, within their rights.

### Rules

1. Report only what the tool returned: no name, date or amount that is not in the answer.
2. Give each field the meaning the documentation gives it (cited above). Where the
   documentation says nothing about a field or a notion, write "not defined by the documentation", never an interpretation.
3. Never invent a required input (an identifier, a period, a code): ask for it. A name the
   user gives becomes an identifier through a call, not through a guess.
4. An answer marked "truncated" is partial: say so and offer the next page ("nextCursor").
   An "upstream_defect" means Logii returned duplicates, which the connector removed: say so.
5. Read-only: this connector changes nothing in Logii.

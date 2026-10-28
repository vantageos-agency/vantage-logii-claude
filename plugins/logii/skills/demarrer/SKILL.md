---
name: demarrer
description: >
  Démarrer avec Logii : vérifier que le connecteur Logii répond et dire ce qu'il permet de consulter. Get started with Logii: check that the Logii connector answers and say what it can look up.
  Use when the user asks what Logii can do, whether Logii is connected, or how to start (Qu'est-ce que Logii peut faire ?).
user-invocable: true
---

## Français

Cette compétence vérifie le connecteur Logii et présente, en langage courant, ce qu'il
permet de consulter. Logii est le logiciel de gestion du cabinet ; la documentation
officielle de l'éditeur l'écrit « Lojii ». Le connecteur est en lecture seule.

### Étapes

1. Regarder les outils que le connecteur Logii expose à cet instant. S'il n'en expose
   aucun, il n'est pas connecté : inviter la personne à se connecter (voir plus bas).
2. Comparer avec la liste des 19 outils ci-dessous. Signaler par son nom tout outil absent ;
   ne jamais présenter comme disponible un outil qui n'est pas exposé.
3. Pour confirmer l'accès, appeler `list_portfolios` sans critère : il n'a pas d'entrée
   obligatoire et ne sert aucune donnée personnelle de locataire ou de propriétaire. Une liste, même vide, confirme l'accès ;
   un refus s'explique comme indiqué plus bas.
4. Présenter ce que la personne peut demander, par métier, avec les compétences du plugin :
   trouver-un-acteur, lots-d-un-immeuble, portefeuilles, mandats-et-echeances,
   baux-et-vacants, compte-locataire, compte-proprietaire, budgets, contrats-fournisseurs,
   ordres-de-service, dossier-copropriete, honoraires-du-cabinet, point-hebdomadaire.

### Les 19 services de la documentation et leur outil

La documentation « Lojii - Les API disponibles » annonce 19 services en quatre domaines. Chacun a un outil :

Section 3. Référentiel et organisation du cabinet :

- **Recherche d'acteurs** : outil `find_party`. Documentation « Lojii - Les API disponibles », section 3. Référentiel et organisation du cabinet ; spécification openapi-1.0.0.json · /api/api/v1/acteur.
- **Identité d'un acteur** : outil `identite_acteur`. Documentation « Lojii - Les API disponibles », section 3. Référentiel et organisation du cabinet ; spécification openapi-1.0.0.json · /api/public/v1/identite_acteur.
- **Lots** : outil `list_lots`. Documentation « Lojii - Les API disponibles », section 3. Référentiel et organisation du cabinet ; spécification openapi-1.0.0.json · /api/public/v1/lots.
- **Portefeuilles** : outil `list_portfolios`. Documentation « Lojii - Les API disponibles », section 3. Référentiel et organisation du cabinet ; spécification openapi-1.0.0.json · /api/api/v1/portefeuille.

Section 4. Syndic de copropriété :

- **Copropriétés** : outil `get_condominium`. Documentation « Lojii - Les API disponibles », section 4. Syndic de copropriété ; spécification openapi-1.0.0.json · /api/api/v1/copropriete.
- **Copropriétaires** : outil `list_co_owners`. Documentation « Lojii - Les API disponibles », section 4. Syndic de copropriété ; spécification openapi-1.0.0.json · /api/api/v1/coproprietaire.
- **Mandats de copropriété** : outil `list_syndic_mandates`. Documentation « Lojii - Les API disponibles », section 4. Syndic de copropriété ; spécification openapi-1.0.0.json · /api/api/v1/mandat_copropriete.

Section 5. Gérance locative :

- **Mandats de gestion** : outil `list_management_mandates`. Documentation « Lojii - Les API disponibles », section 5. Gérance locative ; spécification openapi-1.0.0.json · /api/api/v1/mandat_gestion.
- **Mandats de gestion** : outil `get_owner_mandates`. Documentation « Lojii - Les API disponibles », section 5. Gérance locative ; spécification openapi-1.0.0.json · /api/proprietaire/v1/mandat_gestion.
- **Contrats locataires** : outil `list_tenant_contracts`. Documentation « Lojii - Les API disponibles », section 5. Gérance locative ; spécification openapi-1.0.0.json · /api/suivilocataire/v1/contrat_locataire.
- **Situation comptable locative** : outil `get_rental_situation`. Documentation « Lojii - Les API disponibles », section 5. Gérance locative ; spécification openapi-1.0.0.json · /api/suivilocataire/v1/situation_locative.
- **Comptabilité propriétaire** : outil `get_owner_ledger`. Documentation « Lojii - Les API disponibles », section 5. Gérance locative ; spécification openapi-1.0.0.json · /api/proprietaire/v1/proprietaire_compta.
- **Budgets propriétaire** : outil `list_budgets`. Documentation « Lojii - Les API disponibles », section 5. Gérance locative ; spécification openapi-1.0.0.json · /api/proprietaire/v1/budgets.
- **Budgets propriétaire** : outil `get_budget_detail`. Documentation « Lojii - Les API disponibles », section 5. Gérance locative ; spécification openapi-1.0.0.json · /api/proprietaire/v1/budget_detail.
- **Contrats fournisseurs** : outil `list_supplier_contracts`. Documentation « Lojii - Les API disponibles », section 5. Gérance locative ; spécification openapi-1.0.0.json · /api/api/v1/contrat_fournisseur_gerance.
- **Ordres de service** : outil `list_work_orders`. Documentation « Lojii - Les API disponibles », section 5. Gérance locative ; spécification openapi-1.0.0.json · /api/api/v1/os_gerance.

Section 6. Facturation et honoraires du cabinet :

- **Honoraires facturés** : outil `list_fee_invoices`. Documentation « Lojii - Les API disponibles », section 6. Facturation et honoraires du cabinet ; spécification openapi-1.0.0.json · /api/api/v1/honoraire_facture.
- **Lettrages d'honoraires** : outil `list_fee_settlements`. Documentation « Lojii - Les API disponibles », section 6. Facturation et honoraires du cabinet ; spécification openapi-1.0.0.json · /api/api/v1/honoraire_lettrage.
- **Paiements d'honoraires** : outil `list_fee_payments`. Documentation « Lojii - Les API disponibles », section 6. Facturation et honoraires du cabinet ; spécification openapi-1.0.0.json · /api/api/v1/honoraire_paiement.

### Ce que la documentation ne définit pas

- La correspondance des codes d'activité (Syndic ou Gérance) demandés par les trois outils
  d'honoraires : non défini par la documentation. Ce code se demande à l'administrateur
  Logii du cabinet.
- Les codes de typologie d'acteur : non défini par la documentation.

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

This skill checks the Logii connector and presents, in plain words, what it can look up.
Logii is the agency's management software; the publisher's official documentation spells
it "Lojii". The connector is read-only.

### Steps

1. Look at the tools the Logii connector exposes right now. If it exposes none, it is not
   connected: invite the person to sign in (see below).
2. Compare with the list of 19 tools below. Name any missing tool; never present a tool
   that is not exposed as available.
3. To confirm access, call `list_portfolios` with no criterion: it has no required input
   and serves no tenant or owner personal data. A list, even empty, confirms access; a refusal is
   explained as described below.
4. Present what the person can ask, by job, with the plugin's skills: trouver-un-acteur,
   lots-d-un-immeuble, portefeuilles, mandats-et-echeances, baux-et-vacants,
   compte-locataire, compte-proprietaire, budgets, contrats-fournisseurs,
   ordres-de-service, dossier-copropriete, honoraires-du-cabinet, point-hebdomadaire.

### The 19 services of the documentation and their tool

The documentation "Lojii - Les API disponibles" announces 19 services in four domains. Each has a tool:

Section 3. Référentiel et organisation du cabinet:

- **Recherche d'acteurs**: tool `find_party`. Documentation "Lojii - Les API disponibles", section 3. Référentiel et organisation du cabinet; specification openapi-1.0.0.json · /api/api/v1/acteur.
- **Identité d'un acteur**: tool `identite_acteur`. Documentation "Lojii - Les API disponibles", section 3. Référentiel et organisation du cabinet; specification openapi-1.0.0.json · /api/public/v1/identite_acteur.
- **Lots**: tool `list_lots`. Documentation "Lojii - Les API disponibles", section 3. Référentiel et organisation du cabinet; specification openapi-1.0.0.json · /api/public/v1/lots.
- **Portefeuilles**: tool `list_portfolios`. Documentation "Lojii - Les API disponibles", section 3. Référentiel et organisation du cabinet; specification openapi-1.0.0.json · /api/api/v1/portefeuille.

Section 4. Syndic de copropriété:

- **Copropriétés**: tool `get_condominium`. Documentation "Lojii - Les API disponibles", section 4. Syndic de copropriété; specification openapi-1.0.0.json · /api/api/v1/copropriete.
- **Copropriétaires**: tool `list_co_owners`. Documentation "Lojii - Les API disponibles", section 4. Syndic de copropriété; specification openapi-1.0.0.json · /api/api/v1/coproprietaire.
- **Mandats de copropriété**: tool `list_syndic_mandates`. Documentation "Lojii - Les API disponibles", section 4. Syndic de copropriété; specification openapi-1.0.0.json · /api/api/v1/mandat_copropriete.

Section 5. Gérance locative:

- **Mandats de gestion**: tool `list_management_mandates`. Documentation "Lojii - Les API disponibles", section 5. Gérance locative; specification openapi-1.0.0.json · /api/api/v1/mandat_gestion.
- **Mandats de gestion**: tool `get_owner_mandates`. Documentation "Lojii - Les API disponibles", section 5. Gérance locative; specification openapi-1.0.0.json · /api/proprietaire/v1/mandat_gestion.
- **Contrats locataires**: tool `list_tenant_contracts`. Documentation "Lojii - Les API disponibles", section 5. Gérance locative; specification openapi-1.0.0.json · /api/suivilocataire/v1/contrat_locataire.
- **Situation comptable locative**: tool `get_rental_situation`. Documentation "Lojii - Les API disponibles", section 5. Gérance locative; specification openapi-1.0.0.json · /api/suivilocataire/v1/situation_locative.
- **Comptabilité propriétaire**: tool `get_owner_ledger`. Documentation "Lojii - Les API disponibles", section 5. Gérance locative; specification openapi-1.0.0.json · /api/proprietaire/v1/proprietaire_compta.
- **Budgets propriétaire**: tool `list_budgets`. Documentation "Lojii - Les API disponibles", section 5. Gérance locative; specification openapi-1.0.0.json · /api/proprietaire/v1/budgets.
- **Budgets propriétaire**: tool `get_budget_detail`. Documentation "Lojii - Les API disponibles", section 5. Gérance locative; specification openapi-1.0.0.json · /api/proprietaire/v1/budget_detail.
- **Contrats fournisseurs**: tool `list_supplier_contracts`. Documentation "Lojii - Les API disponibles", section 5. Gérance locative; specification openapi-1.0.0.json · /api/api/v1/contrat_fournisseur_gerance.
- **Ordres de service**: tool `list_work_orders`. Documentation "Lojii - Les API disponibles", section 5. Gérance locative; specification openapi-1.0.0.json · /api/api/v1/os_gerance.

Section 6. Facturation et honoraires du cabinet:

- **Honoraires facturés**: tool `list_fee_invoices`. Documentation "Lojii - Les API disponibles", section 6. Facturation et honoraires du cabinet; specification openapi-1.0.0.json · /api/api/v1/honoraire_facture.
- **Lettrages d'honoraires**: tool `list_fee_settlements`. Documentation "Lojii - Les API disponibles", section 6. Facturation et honoraires du cabinet; specification openapi-1.0.0.json · /api/api/v1/honoraire_lettrage.
- **Paiements d'honoraires**: tool `list_fee_payments`. Documentation "Lojii - Les API disponibles", section 6. Facturation et honoraires du cabinet; specification openapi-1.0.0.json · /api/api/v1/honoraire_paiement.

### What the documentation does not define

- The mapping of the activity codes (Syndic or Gérance) the three fee tools require: not defined by the documentation. Ask the agency's Logii administrator for the code.
- Party typology codes: not defined by the documentation.

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

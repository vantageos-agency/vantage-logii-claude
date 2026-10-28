---
name: mandats-et-echeances
description: >
  Consulter les mandats de gestion et de syndic et leurs échéances dans Logii. Look up management and syndic mandates and their expiry dates in Logii.
  Use when the user asks for mandates, mandates ending soon, an owner's mandates, fee rates or GLI cover.
user-invocable: true
---

## Français

Pour lister les mandats du cabinet, ceux d'un propriétaire, et ceux qui arrivent à leur
date de fin.

### Services Logii utilisés

- **Mandats de gestion** : outil `list_management_mandates`. Documentation « Lojii - Les API disponibles », section 5. Gérance locative ; spécification openapi-1.0.0.json · /api/api/v1/mandat_gestion. Vue cabinet. La documentation : « Mandats de gestion (vue cabinet et vue propriétaire) : numéro, type et modèle de mandat, dates de début, de fin et de résiliation, état, taux d'honoraires de gestion, couvertures d'assurance (GLI, GO, PNO), lots gérés, portefeuille de gestion et agence de rattachement ».
- **Mandats de gestion** : outil `get_owner_mandates`. Documentation « Lojii - Les API disponibles », section 5. Gérance locative ; spécification openapi-1.0.0.json · /api/proprietaire/v1/mandat_gestion. Vue propriétaire, même service.
- **Mandats de copropriété** : outil `list_syndic_mandates`. Documentation « Lojii - Les API disponibles », section 4. Syndic de copropriété ; spécification openapi-1.0.0.json · /api/api/v1/mandat_copropriete. La documentation : « Mandats de copropriété : numéro et date de début du mandat, état, date de fin de l'exercice en cours, … ». La spécification précise qu'il s'agit des mandats de copropriété actifs.
- **Recherche d'acteurs** : outil `find_party`. Documentation « Lojii - Les API disponibles », section 3. Référentiel et organisation du cabinet ; spécification openapi-1.0.0.json · /api/api/v1/acteur. Pour transformer le nom d'un propriétaire en identifiant.

### Ce qu'il faut avoir avant d'appeler

- Pour « les mandats qui arrivent à échéance », il faut une période : demander laquelle
  (par exemple « d'ici fin mars ») et la convertir en dates de fin minimale et maximale
  ("ends_from", "ends_to"). Ne pas choisir l'horizon à la place de l'utilisateur.
- Préciser gestion ou syndic si l'utilisateur ne le dit pas ; sinon interroger les deux et
  les présenter séparément.
- Pour un propriétaire : son identifiant, via `find_party`. `get_owner_mandates` refuse
  sans au moins un filtre (propriétaire, mandat, ou une date).

### Lire la réponse

- `list_management_mandates` : "number" (« Référence externe »), "type", "model",
  "status", "start", "end", "terminated_on" (date de résiliation), "fee_rate_pct" (taux d'honoraires de gestion, en %), "gli_rate_pct" (taux GLI), "portfolio_id",
  "agency_id".
- `get_owner_mandates` : les mêmes champs, plus "owner_id" et les lots couverts ("lots" :
  numéro, désignation, début et fin de gestion, "gli"). GLI : « Garantie des Loyers Impayés » selon la spécification.
- `list_syndic_mandates` : "condominium_name", "city", "state", "phase", "start", "end",
  "fiscal_year_end" (fin de l'exercice en cours), "treasury_balance" (solde de
  trésorerie), "debtors" et "creditors" (nombre et total), "fees", "lots",
  "co_owners_count". Les coordonnées bancaires du mandat ne sont jamais servies.
- Une échéance se lit ici comme la date de fin du mandat ("end"), qui est la « Date de fin prévue » selon la spécification de la vue propriétaire.

### Ce que la documentation ne définit pas

- Le renouvellement ou la tacite reconduction d'un mandat : non défini par la documentation. Ne pas annoncer qu'un mandat « sera renouvelé ».
- La liste des valeurs d'état ("status", "state", "phase") : non défini par la documentation ; les rapporter telles quelles.
- Les couvertures GO et PNO citées par la documentation ne sont pas servies par ce
  connecteur.

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

To list the agency's mandates, one owner's mandates, and those reaching their end date.

### Logii services used

- **Mandats de gestion**: tool `list_management_mandates`. Documentation "Lojii - Les API disponibles", section 5. Gérance locative; specification openapi-1.0.0.json · /api/api/v1/mandat_gestion. Agency view. The documentation: "Mandats de gestion (vue cabinet et vue propriétaire) : numéro, type et modèle de mandat, dates de début, de fin et de résiliation, état, taux d'honoraires de gestion, couvertures d'assurance (GLI, GO, PNO), lots gérés, portefeuille de gestion et agence de rattachement".
- **Mandats de gestion**: tool `get_owner_mandates`. Documentation "Lojii - Les API disponibles", section 5. Gérance locative; specification openapi-1.0.0.json · /api/proprietaire/v1/mandat_gestion. Owner view, same service.
- **Mandats de copropriété**: tool `list_syndic_mandates`. Documentation "Lojii - Les API disponibles", section 4. Syndic de copropriété; specification openapi-1.0.0.json · /api/api/v1/mandat_copropriete. The documentation: "Mandats de copropriété : numéro et date de début du mandat, état, date de fin de l'exercice en cours, …". The specification says these are the active condominium mandates.
- **Recherche d'acteurs**: tool `find_party`. Documentation "Lojii - Les API disponibles", section 3. Référentiel et organisation du cabinet; specification openapi-1.0.0.json · /api/api/v1/acteur. To turn an owner's name into an identifier.

### What you need before calling

- For "mandates ending soon" you need a period: ask which one (for example "by the end of
  March") and turn it into minimum and maximum end dates ("ends_from", "ends_to"). Do not
  pick the horizon for the user.
- Ask management or syndic if the user does not say; otherwise query both and show them
  separately.
- For an owner: their identifier, through `find_party`. `get_owner_mandates` refuses
  without at least one filter (owner, mandate, or a date).

### Reading the answer

- `list_management_mandates`: "number" ("Référence externe"), "type", "model", "status",
  "start", "end", "terminated_on" (termination date), "fee_rate_pct" (management fee
  rate, %), "gli_rate_pct" (GLI rate), "portfolio_id", "agency_id".
- `get_owner_mandates`: the same fields, plus "owner_id" and the covered lots ("lots":
  number, designation, management start and end, "gli"). GLI: "Garantie des Loyers Impayés" per the specification.
- `list_syndic_mandates`: "condominium_name", "city", "state", "phase", "start", "end",
  "fiscal_year_end" (end of the current fiscal year), "treasury_balance", "debtors" and
  "creditors" (count and total), "fees", "lots", "co_owners_count". The mandate's bank
  details are never served.
- An expiry is read here as the mandate end date ("end"), which is the "Date de fin prévue" per the owner-view specification.

### What the documentation does not define

- Renewal or tacit renewal of a mandate: not defined by the documentation. Do not announce
  that a mandate "will be renewed".
- The list of state values ("status", "state", "phase"): not defined by the documentation; report them as is.
- The GO and PNO covers the documentation mentions are not served by this connector.

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

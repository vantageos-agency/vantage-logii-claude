---
name: compte-locataire
description: >
  Lire la situation comptable d'un locataire dans Logii : quittancé, réglé, reste à payer. Read a tenant's account in Logii: billed, paid, remaining to pay.
  Use when the user asks what a tenant owes, a tenant's receipts and payments, or remaining balances on leases.
user-invocable: true
---

## Français

Pour lire, ligne par ligne, ce qui a été quittancé à un locataire, ce qui a été réglé, et
le solde restant à payer.

### Services Logii utilisés

- **Situation comptable locative** : outil `get_rental_situation`. Documentation « Lojii - Les API disponibles », section 5. Gérance locative ; spécification openapi-1.0.0.json · /api/suivilocataire/v1/situation_locative. La documentation : « Quittances et factures locataires : nature de l'écriture, montants HT, TVA et TTC, sommes quittancées, règlements perçus et restes à payer, rattachement au bail, au lot et au propriétaire, indication de reversement au propriétaire ».
- **Contrats locataires** : outil `list_tenant_contracts`. Documentation « Lojii - Les API disponibles », section 5. Gérance locative ; spécification openapi-1.0.0.json · /api/suivilocataire/v1/contrat_locataire. Pour retrouver le bail d'un locataire à partir de son nom.
- **Recherche d'acteurs** : outil `find_party`. Documentation « Lojii - Les API disponibles », section 3. Référentiel et organisation du cabinet ; spécification openapi-1.0.0.json · /api/api/v1/acteur. Si seul le nom du locataire est connu et qu'il faut son identifiant.

### Ce qu'il faut avoir avant d'appeler

`get_rental_situation` exige l'un de : bail, quittance, immeuble, propriétaire, mandat —
ou une période complète (date de début ET date de fin). Pour un locataire nommé :
`list_tenant_contracts` avec son nom, puis le bail renvoyé. S'il a plusieurs baux, demander
lequel. Si l'utilisateur parle d'une période sans dates, la demander.

### Lire la réponse

Par ligne : "date", "label" (« Libellé de l'écriture »), "nature", "billed_ttc"
(« Montant total TTC de l'écriture »), "paid" (« Montant déjà réglé (lettré) »),
"impaye_logii" (champ Logii « impaye » : « Solde restant à payer (TTC - Payé) »),
"passed_to_owner" (« Indique si le montant est reversé au propriétaire »). Les totaux
("totals") portent sur toute la sélection : "lines", "billed_ttc", "paid",
"impaye_logii". "nonzero_impaye_only" ne garde que les lignes dont ce solde est
supérieur à zéro.

Dire « solde restant à payer selon Logii », avec le montant renvoyé.

### Ce que la documentation ne définit pas

- La notion d'« impayé » au sens d'un retard, d'un arriéré ou d'un contentieux : non défini par la documentation. La documentation définit seulement le solde restant à payer
  (TTC moins payé). Ne pas qualifier un locataire de « mauvais payeur » ni dire qu'il est
  « en retard ».
- La date d'exigibilité d'une quittance : non défini par la documentation.

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

To read, line by line, what was billed to a tenant, what was paid, and the remaining
balance.

### Logii services used

- **Situation comptable locative**: tool `get_rental_situation`. Documentation "Lojii - Les API disponibles", section 5. Gérance locative; specification openapi-1.0.0.json · /api/suivilocataire/v1/situation_locative. The documentation: "Quittances et factures locataires : nature de l'écriture, montants HT, TVA et TTC, sommes quittancées, règlements perçus et restes à payer, rattachement au bail, au lot et au propriétaire, indication de reversement au propriétaire".
- **Contrats locataires**: tool `list_tenant_contracts`. Documentation "Lojii - Les API disponibles", section 5. Gérance locative; specification openapi-1.0.0.json · /api/suivilocataire/v1/contrat_locataire. To find a tenant's lease from their name.
- **Recherche d'acteurs**: tool `find_party`. Documentation "Lojii - Les API disponibles", section 3. Référentiel et organisation du cabinet; specification openapi-1.0.0.json · /api/api/v1/acteur. When only the tenant's name is known and their identifier is needed.

### What you need before calling

`get_rental_situation` requires one of: lease, receipt, building, owner, mandate — or a
full period (start date AND end date). For a named tenant: `list_tenant_contracts` with
the name, then the lease returned. If they have several leases, ask which one. If the user
mentions a period without dates, ask for them.

### Reading the answer

Per line: "date", "label" ("Libellé de l'écriture"), "nature", "billed_ttc" ("Montant total TTC de l'écriture"), "paid" ("Montant déjà réglé (lettré)"), "impaye_logii"
(Logii field "impaye": "Solde restant à payer (TTC - Payé)"), "passed_to_owner"
("Indique si le montant est reversé au propriétaire"). The totals ("totals") cover the
whole selection: "lines", "billed_ttc", "paid", "impaye_logii". "nonzero_impaye_only"
keeps only lines where that balance is above zero.

Say "remaining balance per Logii", with the amount returned.

### What the documentation does not define

- "Unpaid rent" in the sense of a delay, arrears or a dispute: not defined by the documentation. The documentation defines only the remaining balance (TTC minus paid). Do
  not call a tenant a "bad payer" or say they are "late".
- The due date of a receipt: not defined by the documentation.

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

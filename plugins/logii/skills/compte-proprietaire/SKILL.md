---
name: compte-proprietaire
description: >
  Lire les écritures du compte d'un propriétaire dans Logii. Read the entries of an owner's account in Logii.
  Use when the user asks for an owner's account, owner statement entries, what was charged or credited to an owner.
user-invocable: true
---

## Français

Pour lire les écritures du compte d'un propriétaire (mandant) : débit, crédit, TVA, tiers
concerné, immeuble et exercice.

### Services Logii utilisés

- **Comptabilité propriétaire** : outil `get_owner_ledger`. Documentation « Lojii - Les API disponibles », section 5. Gérance locative ; spécification openapi-1.0.0.json · /api/proprietaire/v1/proprietaire_compta. La documentation : « Écritures non soldées du compte propriétaire : date, libellé, journal, tiers concerné, débit et crédit, montants HT, TVA et TTC, parts déductible et récupérable, rattachement à l'immeuble et à l'exercice budgétaire ». La spécification : « écritures comptables non lettrées (compte 411) ».
- **Recherche d'acteurs** : outil `find_party`. Documentation « Lojii - Les API disponibles », section 3. Référentiel et organisation du cabinet ; spécification openapi-1.0.0.json · /api/api/v1/acteur. Pour transformer le nom du propriétaire en identifiant.

### Ce qu'il faut avoir avant d'appeler

L'identifiant du propriétaire est obligatoire (« Identifiant du propriétaire (obligatoire) ») : le retrouver avec `find_party` à partir du nom ; s'il y a plusieurs
résultats, demander lequel. Facultatif : une période, un exercice, un immeuble, un mandat,
un code journal.

### Lire la réponse

Par écriture : "date", "journal" et "journal_label", "label", "debit", "credit", "ht",
"vat", "ttc", "deductible" (« Montant déductible fiscalement »), "recoverable"
(« Montant récupérable locataire »), "third_party_name" (« Nom du tiers associé à
l'écriture », vide si l'écriture n'a pas de tiers), "building_id", "budget_id". Totaux :
"entries", "debit", "credit", et "balance", que le connecteur calcule comme débit moins
crédit sur la sélection.

### Ce que la documentation ne définit pas

- Le sens du signe de "balance" (qui doit à qui) : non défini par la documentation.
  Donner les totaux débit et crédit et le solde calculé, sans dire « le propriétaire vous
  doit » ou « vous devez au propriétaire ».
- La documentation parle d'écritures non soldées, mais le service n'expose aucun indicateur
  de solde par écriture : savoir si une écriture précise est soldée est non défini par la documentation.

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

To read the entries of an owner's (principal's) account: debit, credit, VAT, third party,
building and fiscal year.

### Logii services used

- **Comptabilité propriétaire**: tool `get_owner_ledger`. Documentation "Lojii - Les API disponibles", section 5. Gérance locative; specification openapi-1.0.0.json · /api/proprietaire/v1/proprietaire_compta. The documentation: "Écritures non soldées du compte propriétaire : date, libellé, journal, tiers concerné, débit et crédit, montants HT, TVA et TTC, parts déductible et récupérable, rattachement à l'immeuble et à l'exercice budgétaire". The specification: "écritures comptables non lettrées (compte 411)".
- **Recherche d'acteurs**: tool `find_party`. Documentation "Lojii - Les API disponibles", section 3. Référentiel et organisation du cabinet; specification openapi-1.0.0.json · /api/api/v1/acteur. To turn the owner's name into an identifier.

### What you need before calling

The owner identifier is required ("Identifiant du propriétaire (obligatoire)"): find it
with `find_party` from the name; if there are several results, ask which one. Optional: a
period, a fiscal year, a building, a mandate, a journal code.

### Reading the answer

Per entry: "date", "journal" and "journal_label", "label", "debit", "credit", "ht",
"vat", "ttc", "deductible" ("Montant déductible fiscalement"), "recoverable" ("Montant récupérable locataire"), "third_party_name" ("Nom du tiers associé à l'écriture", empty
when the entry has no third party), "building_id", "budget_id". Totals: "entries",
"debit", "credit", and "balance", which the connector computes as debit minus credit over
the selection.

### What the documentation does not define

- The meaning of the sign of "balance" (who owes whom): not defined by the documentation.
  Give the debit and credit totals and the computed balance, without saying "the owner
  owes you" or "you owe the owner".
- The documentation speaks of unsettled entries, but the service exposes no settled flag
  per entry: whether one entry is settled is not defined by the documentation.

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

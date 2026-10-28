---
name: contrats-fournisseurs
description: >
  Consulter les contrats fournisseurs de la gérance dans Logii. Look up the management's supplier contracts in Logii.
  Use when the user asks for supplier, maintenance or service contracts, their supplier, building, dates or notice period.
user-invocable: true
---

## Français

Pour retrouver les contrats de prestation rattachés à la gérance (entretien, maintenance,
etc.) et lire leurs dates, leur durée et leur préavis.

### Service Logii utilisé

- **Contrats fournisseurs** : outil `list_supplier_contracts`. Documentation « Lojii - Les API disponibles », section 5. Gérance locative ; spécification openapi-1.0.0.json · /api/api/v1/contrat_fournisseur_gerance. La documentation : « Contrats fournisseurs : contrats de prestation rattachés à la gérance : référence et intitulé, dates de début et de fin, durée et préavis, statut, fournisseur, immeuble, propriétaire, mandat et gestionnaire, agence ».

### Ce qu'il faut avoir avant d'appeler

Aucune entrée obligatoire. Facultatif : un terme de recherche d'au moins deux caractères
(« Terme de recherche global (insensible aux accents) » : immeuble, fournisseur,
propriétaire, références). Le service n'a pas de filtre de date : pour « les contrats qui
se terminent avant juin », lire la date de fin des contrats renvoyés, et dire si la liste
est partielle.

### Lire la réponse

Par contrat : "reference", "title", "status", "start", "end", "duration",
"notice_period" (durée de préavis, telle que Logii l'écrit), "supplier", "building",
"owner", "agency", "mandate".

### Ce que la documentation ne définit pas

- La tacite reconduction, et la date limite pour dénoncer un contrat : non défini par la documentation. Ne pas calculer de date de dénonciation à partir du préavis.
- La liste des valeurs de "status" : non défini par la documentation.

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

To find the service contracts attached to property management (upkeep, maintenance, etc.)
and read their dates, duration and notice period.

### Logii service used

- **Contrats fournisseurs**: tool `list_supplier_contracts`. Documentation "Lojii - Les API disponibles", section 5. Gérance locative; specification openapi-1.0.0.json · /api/api/v1/contrat_fournisseur_gerance. The documentation: "Contrats fournisseurs : contrats de prestation rattachés à la gérance : référence et intitulé, dates de début et de fin, durée et préavis, statut, fournisseur, immeuble, propriétaire, mandat et gestionnaire, agence".

### What you need before calling

No required input. Optional: a search term of at least two characters ("Terme de recherche global (insensible aux accents)": building, supplier, owner, references). The service has
no date filter: for "contracts ending before June", read the end date of the contracts
returned, and say whether the list is partial.

### Reading the answer

Per contract: "reference", "title", "status", "start", "end", "duration",
"notice_period" (as Logii writes it), "supplier", "building", "owner", "agency",
"mandate".

### What the documentation does not define

- Tacit renewal, and the deadline to terminate a contract: not defined by the documentation. Do not compute a termination deadline from the notice period.
- The list of "status" values: not defined by the documentation.

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

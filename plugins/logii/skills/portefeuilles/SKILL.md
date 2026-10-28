---
name: portefeuilles
description: >
  Lister les portefeuilles de gestion du cabinet, leurs agences et les collaborateurs qui les gèrent. List the agency's management portfolios, their branches and the staff who manage them.
  Use when the user asks which portfolios exist, who manages a portfolio, or which accountant or manager is assigned.
user-invocable: true
---

## Français

Pour savoir quels portefeuilles existent, à quelle agence ils sont rattachés, et qui y est
affecté.

### Service Logii utilisé

- **Portefeuilles** : outil `list_portfolios`. Documentation « Lojii - Les API disponibles », section 3. Référentiel et organisation du cabinet ; spécification openapi-1.0.0.json · /api/api/v1/portefeuille. La documentation : « Portefeuilles de gestion, agences rattachées et collaborateurs affectés (fonction, période d'affectation) – gestionnaires comme comptables ».

### Ce qu'il faut avoir avant d'appeler

Aucune entrée obligatoire. Facultatif : un nom de portefeuille (recherche partielle) ;
"include_agencies" pour ajouter les agences rattachées ; "include_employees" pour ajouter
les collaborateurs affectés. Pour « qui gère », demander les collaborateurs.

### Lire la réponse

- Par portefeuille : "name", "key", et si demandés "agencies" (titre de l'agence) et
  "employees".
- Par collaborateur : "title" (son nom), "function" (sa fonction, telle que Logii la
  nomme), "start_date" et "end_date" (la période d'affectation). Rapporter la fonction
  telle quelle ; ne pas déduire qui est « le » gestionnaire si plusieurs personnes sont
  affectées.

### Ce que la documentation ne définit pas

- La liste des fonctions possibles : non défini par la documentation.
- Si une affectation terminée apparaît dans la liste : le connecteur ne propose pas
  l'option des affectations closes ; une "end_date" passée se rapporte telle quelle, et
  l'exhaustivité des affectations passées est non défini par la documentation.

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

To know which portfolios exist, which branch they belong to, and who is assigned to them.

### Logii service used

- **Portefeuilles**: tool `list_portfolios`. Documentation "Lojii - Les API disponibles", section 3. Référentiel et organisation du cabinet; specification openapi-1.0.0.json · /api/api/v1/portefeuille. The documentation: "Portefeuilles de gestion, agences rattachées et collaborateurs affectés (fonction, période d'affectation) – gestionnaires comme comptables".

### What you need before calling

No required input. Optional: a portfolio name (partial search); "include_agencies" to add
the attached branches; "include_employees" to add the assigned staff. For "who manages",
ask for the staff.

### Reading the answer

- Per portfolio: "name", "key", and when requested "agencies" (branch title) and
  "employees".
- Per staff member: "title" (their name), "function" (their role, as Logii names it),
  "start_date" and "end_date" (the assignment period). Report the role as is; do not
  deduce who is "the" manager when several people are assigned.

### What the documentation does not define

- The list of possible roles: not defined by the documentation.
- Whether an ended assignment appears in the list: the connector does not offer the closed
  assignments option; a past "end_date" is reported as is, and whether past assignments
  are complete is not defined by the documentation.

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

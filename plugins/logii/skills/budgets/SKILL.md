---
name: budgets
description: >
  Consulter les budgets propriétaire dans Logii : prévu, appelé, réalisé, et le détail par rubrique. Look up owner budgets in Logii: planned, called, actual, and the detail by section.
  Use when the user asks for a building's or an owner's budget, amounts called, remaining to call, or budget lines.
user-invocable: true
---

## Français

Pour lister les exercices budgétaires d'un propriétaire ou d'un immeuble, puis lire le
détail d'un budget par chapitre et rubrique.

### Services Logii utilisés

- **Budgets propriétaire** : outil `list_budgets`. Documentation « Lojii - Les API disponibles », section 5. Gérance locative ; spécification openapi-1.0.0.json · /api/proprietaire/v1/budgets. La documentation : « Budgets propriétaire (liste et détail) : exercices budgétaires par propriétaire ou par immeuble : intitulé, période, état, montants prévisionnels et réalisés (HT, TVA, TTC), sommes appelées et à appeler ».
- **Budgets propriétaire** : outil `get_budget_detail`. Documentation « Lojii - Les API disponibles », section 5. Gérance locative ; spécification openapi-1.0.0.json · /api/proprietaire/v1/budget_detail. La documentation : « Le détail restitue les lignes par chapitre et rubrique, avec clés de répartition et taux applicables ».
- **Recherche d'acteurs** : outil `find_party`. Documentation « Lojii - Les API disponibles », section 3. Référentiel et organisation du cabinet ; spécification openapi-1.0.0.json · /api/api/v1/acteur. Pour transformer le nom d'un propriétaire en identifiant.

### Ce qu'il faut avoir avant d'appeler

- `list_budgets` n'exige aucun filtre ; pour un propriétaire, un immeuble ou un budget
  précis, passer son identifiant. Retrouver le propriétaire avec `find_party` si seul son
  nom est connu.
- `get_budget_detail` exige l'identifiant d'un budget, issu de `list_budgets`.

### Lire la réponse

- `list_budgets`, par exercice : "title" (« Intitulé de l'exercice »), "operation_type",
  "state" (« État du statut de l'exercice »), "status" (« Statut de régularisation
  gérance »), "start", "end", "planned_ht" / "planned_vat" / "planned_ttc" (« Montant
  actif » dans la spécification, « montants prévisionnels » dans la documentation),
  "called" (« Montant déjà appelé »), "to_call" (« Reste à appeler »), "actual_ht" /
  "actual_vat" / "actual_ttc" (« Montant facturé/réalisé »).
- `get_budget_detail`, par rubrique : le chapitre, la rubrique avec sa clé de répartition
  ("allocation_key", « Libellé de la clé de répartition ») et "allocation_shares"
  (« Total des tantièmes de la clé »), puis les lignes : prévu, appelé, à appeler,
  réalisé, "vat_rate_pct", "deductible_rate_pct" (« Taux de déductibilité fiscale
  (%) »), "recoverable_rate_pct" (« Taux de récupération locative (%) »).

### Ce que la documentation ne définit pas

- Un « dépassement » ou un « écart » de budget : non défini par la documentation. Donner
  le prévu et le réalisé côte à côte, sans qualifier l'écart.
- La liste des valeurs de "state" et "status" : non défini par la documentation.

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

To list the budget years of an owner or a building, then read one budget's detail by
chapter and section.

### Logii services used

- **Budgets propriétaire**: tool `list_budgets`. Documentation "Lojii - Les API disponibles", section 5. Gérance locative; specification openapi-1.0.0.json · /api/proprietaire/v1/budgets. The documentation: "Budgets propriétaire (liste et détail) : exercices budgétaires par propriétaire ou par immeuble : intitulé, période, état, montants prévisionnels et réalisés (HT, TVA, TTC), sommes appelées et à appeler".
- **Budgets propriétaire**: tool `get_budget_detail`. Documentation "Lojii - Les API disponibles", section 5. Gérance locative; specification openapi-1.0.0.json · /api/proprietaire/v1/budget_detail. The documentation: "Le détail restitue les lignes par chapitre et rubrique, avec clés de répartition et taux applicables".
- **Recherche d'acteurs**: tool `find_party`. Documentation "Lojii - Les API disponibles", section 3. Référentiel et organisation du cabinet; specification openapi-1.0.0.json · /api/api/v1/acteur. To turn an owner's name into an identifier.

### What you need before calling

- `list_budgets` requires no filter; for an owner, a building or a specific budget, pass
  its identifier. Find the owner with `find_party` when only the name is known.
- `get_budget_detail` requires a budget identifier, from `list_budgets`.

### Reading the answer

- `list_budgets`, per year: "title" ("Intitulé de l'exercice"), "operation_type",
  "state" ("État du statut de l'exercice"), "status" ("Statut de régularisation
  gérance"), "start", "end", "planned_ht" / "planned_vat" / "planned_ttc" ("Montant
  actif" in the specification, "montants prévisionnels" in the documentation), "called"
  ("Montant déjà appelé"), "to_call" ("Reste à appeler"), "actual_ht" / "actual_vat" /
  "actual_ttc" ("Montant facturé/réalisé").
- `get_budget_detail`, per section: the chapter, the section with its allocation key
  ("allocation_key", "Libellé de la clé de répartition") and "allocation_shares" ("Total
  des tantièmes de la clé"), then the lines: planned, called, to call, actual,
  "vat_rate_pct", "deductible_rate_pct" ("Taux de déductibilité fiscale (%)"),
  "recoverable_rate_pct" ("Taux de récupération locative (%)").

### What the documentation does not define

- A budget "overrun" or "variance": not defined by the documentation. Show planned and
  actual side by side, without qualifying the difference.
- The list of "state" and "status" values: not defined by the documentation.

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

---
name: trouver-un-acteur
description: >
  Trouver une personne, une société ou un immeuble dans Logii et lire sa fiche d'identité. Find a person, a company or a building in Logii and read its identity card.
  Use when the user asks to find a tenant, an owner, a company, a contact or a building by name (retrouve, cherche, qui est).
user-invocable: true
---

## Français

Pour retrouver un acteur (personne physique, personne morale ou immeuble) et obtenir
l'identifiant dont les autres recherches ont besoin.

### Services Logii utilisés

- **Recherche d'acteurs** : outil `find_party`. Documentation « Lojii - Les API disponibles », section 3. Référentiel et organisation du cabinet ; spécification openapi-1.0.0.json · /api/api/v1/acteur. La documentation : « Personnes physiques, personnes morales et immeubles : nom, prénom, typologie, SIRET, coordonnées (courriel, téléphone fixe et mobile), pays, dates de création et de modification ».
- **Identité d'un acteur** : outil `identite_acteur`. Documentation « Lojii - Les API disponibles », section 3. Référentiel et organisation du cabinet ; spécification openapi-1.0.0.json · /api/public/v1/identite_acteur. La documentation : « Fiche détaillée d'un acteur : état civil, type de personne et forme de société, adresse postale complète, coordonnées téléphoniques et électroniques ».
- **Lots** : outil `list_lots`. Documentation « Lojii - Les API disponibles », section 3. Référentiel et organisation du cabinet ; spécification openapi-1.0.0.json · /api/public/v1/lots. Pour un immeuble : le critère « name » correspond à « Nom de l'immeuble (recherche partielle) » dans la spécification.

### Ce qu'il faut avoir avant d'appeler

- `find_party` exige un nom (ou une raison sociale) d'au moins deux caractères ; le prénom
  est facultatif. Si l'utilisateur ne donne pas de nom, le demander.
- Ne pas passer de codes de typologie sauf si l'utilisateur les donne : sans eux, le
  connecteur utilise la liste configurée pour le cabinet.
- `identite_acteur` exige l'identifiant d'un acteur, issu de `find_party`.
- Pour un immeuble, appeler `list_lots` avec le nom de l'immeuble ; chaque lot renvoyé
  porte l'identifiant de son immeuble ("building_id").

### Lire la réponse

- `find_party` renvoie, par acteur : "id", "key" (clé technique), "last_name" (nom ou
  raison sociale), "first_name", "party_type", "person_type", "city", "postal_code",
  "created_on". Plusieurs acteurs peuvent porter le même nom : les présenter tous, avec la
  ville, et demander lequel est le bon plutôt que d'en choisir un.
- `identite_acteur` renvoie "name", "person_type", "legal_form" (forme de société),
  "city", "postal_code", "country", et "contact" : seulement si un courriel, un mobile, un
  fixe et une adresse existent ("has_email", "has_mobile", "has_landline",
  "has_street_address"). Les valeurs elles-mêmes (le numéro, le courriel, la rue) ne sont
  pas servies par ce connecteur : le dire si on les demande, sans les chercher ailleurs.
- Le SIRET et les coordonnées listés par la documentation ne sont pas servis par ce
  connecteur.

### Ce que la documentation ne définit pas

- Les codes de typologie d'acteur et la liste des valeurs de "party_type" : non défini par la documentation.
- Si un immeuble apparaît dans `find_party` dépend des typologies configurées : non défini par la documentation. Pour un immeuble, passer par `list_lots`.

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

To find a party (a natural person, a legal person or a building) and get the identifier
other lookups need.

### Logii services used

- **Recherche d'acteurs**: tool `find_party`. Documentation "Lojii - Les API disponibles", section 3. Référentiel et organisation du cabinet; specification openapi-1.0.0.json · /api/api/v1/acteur. The documentation: "Personnes physiques, personnes morales et immeubles : nom, prénom, typologie, SIRET, coordonnées (courriel, téléphone fixe et mobile), pays, dates de création et de modification".
- **Identité d'un acteur**: tool `identite_acteur`. Documentation "Lojii - Les API disponibles", section 3. Référentiel et organisation du cabinet; specification openapi-1.0.0.json · /api/public/v1/identite_acteur. The documentation: "Fiche détaillée d'un acteur : état civil, type de personne et forme de société, adresse postale complète, coordonnées téléphoniques et électroniques".
- **Lots**: tool `list_lots`. Documentation "Lojii - Les API disponibles", section 3. Référentiel et organisation du cabinet; specification openapi-1.0.0.json · /api/public/v1/lots. For a building: the "name" criterion is "Nom de l'immeuble (recherche partielle)" in the specification.

### What you need before calling

- `find_party` requires a last name (or company name) of at least two characters; the first
  name is optional. If the user gives no name, ask for one.
- Do not pass typology codes unless the user gives them: without them the connector uses
  the list configured for the agency.
- `identite_acteur` requires a party identifier, from `find_party`.
- For a building, call `list_lots` with the building name; each lot returned carries its
  building's identifier ("building_id").

### Reading the answer

- `find_party` returns, per party: "id", "key" (technical key), "last_name" (name or
  company name), "first_name", "party_type", "person_type", "city", "postal_code",
  "created_on". Several parties can share a name: show them all, with the city, and ask
  which one is meant rather than picking one.
- `identite_acteur` returns "name", "person_type", "legal_form", "city", "postal_code",
  "country", and "contact": only whether an email, a mobile, a landline and a street
  address exist ("has_email", "has_mobile", "has_landline", "has_street_address"). The
  values themselves are not served by this connector: say so if asked, without looking
  for them elsewhere.
- The SIRET and contact details the documentation lists are not served by this connector.

### What the documentation does not define

- Party typology codes and the list of "party_type" values: not defined by the documentation.
- Whether a building appears in `find_party` depends on the configured typologies: not defined by the documentation. For a building, use `list_lots`.

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

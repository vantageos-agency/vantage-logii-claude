---
name: lots-d-un-immeuble
description: >
  Lister les lots d'un immeuble, ou retrouver un lot précis, dans Logii. List the lots of a building, or find one lot, in Logii.
  Use when the user asks for the lots, apartments, parkings or cellars of a building, or for one lot by its number.
user-invocable: true
---

## Français

Pour obtenir les lots d'un immeuble, ou un ou plusieurs lots précis.

### Service Logii utilisé

- **Lots** : outil `list_lots`. Documentation « Lojii - Les API disponibles », section 3. Référentiel et organisation du cabinet ; spécification openapi-1.0.0.json · /api/public/v1/lots. La documentation : « Lots d'un immeuble ou sélection de lots, avec possibilité d'obtenir dans le même appel le bâtiment de rattachement, le volet gestion (avec le propriétaire) et le volet copropriété (avec le copropriétaire) ».

### Ce qu'il faut avoir avant d'appeler

Au moins un critère : l'identifiant de l'immeuble ("building_id"), l'identifiant d'un lot,
plusieurs identifiants de lots, le nom de l'immeuble (« Nom de l'immeuble (recherche partielle) ») ou le numéro de lot (« Numéro du lot »). Sans critère, l'outil refuse
[criteria_required] : demander le nom de l'immeuble.

### Lire la réponse

Par lot : "designation", "code", "type", "type_detail", "address", "floor", "rooms",
"living_area_m2", "is_main", "building_id", et le bâtiment de rattachement
("sub_building_id", "sub_building_address"). Présenter les lots tels que renvoyés, sans
regrouper ni compléter.

Le volet gestion (propriétaire) et le volet copropriété (copropriétaire) que cite la
documentation ne sont pas servis par cet outil : pour le propriétaire, utiliser la
compétence mandats-et-echeances ; pour les copropriétaires, dossier-copropriete.

### Ce que la documentation ne définit pas

La spécification ne décrit pas les champs de la réponse de ce service (elle ne documente
que le refus « Veuillez renseigner au moins un critère de recherche (lot ou immeuble) »).
Le sens exact de "type", "is_main" et de la surface ("living_area_m2", habitable ou
autre) est donc non défini par la documentation : rapporter la valeur telle quelle.

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

To get the lots of a building, or one or several specific lots.

### Logii service used

- **Lots**: tool `list_lots`. Documentation "Lojii - Les API disponibles", section 3. Référentiel et organisation du cabinet; specification openapi-1.0.0.json · /api/public/v1/lots. The documentation: "Lots d'un immeuble ou sélection de lots, avec possibilité d'obtenir dans le même appel le bâtiment de rattachement, le volet gestion (avec le propriétaire) et le volet copropriété (avec le copropriétaire)".

### What you need before calling

At least one criterion: the building identifier ("building_id"), one lot identifier,
several lot identifiers, the building name ("Nom de l'immeuble (recherche partielle)") or
the lot number ("Numéro du lot"). Without a criterion the tool refuses
[criteria_required]: ask for the building name.

### Reading the answer

Per lot: "designation", "code", "type", "type_detail", "address", "floor", "rooms",
"living_area_m2", "is_main", "building_id", and the attached building
("sub_building_id", "sub_building_address"). Present the lots as returned, without
grouping or completing them.

The management part (owner) and the condominium part (co-owner) the documentation mentions
are not served by this tool: for the owner use the mandats-et-echeances skill; for the
co-owners, dossier-copropriete.

### What the documentation does not define

The specification does not describe the response fields of this service (it documents only
the refusal "Veuillez renseigner au moins un critère de recherche (lot ou immeuble)"). The
exact meaning of "type", "is_main" and of the area ("living_area_m2", living area or
other) is therefore not defined by the documentation: report the value as is.

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

---
name: ordres-de-service
description: >
  Suivre les ordres de service et les travaux de la gérance dans Logii. Follow the management's work orders and works in Logii.
  Use when the user asks for work orders, repairs, works in progress, or the works on a building or for a tenant.
user-invocable: true
---

## Français

Pour lister les ordres de service et suivre l'avancement des travaux.

### Service Logii utilisé

- **Ordres de service** : outil `list_work_orders`. Documentation « Lojii - Les API disponibles », section 5. Gérance locative ; spécification openapi-1.0.0.json · /api/api/v1/os_gerance. La documentation : « Ordres de service et suivi des travaux : intitulé, état d'avancement, dates de création, de démarrage et de réception, avec les acteurs associés (immeuble, locataire, propriétaire, agence, gestionnaire) ».

### Ce qu'il faut avoir avant d'appeler

Aucune entrée obligatoire. Facultatif : un terme de recherche d'au moins deux caractères
(« Terme de recherche global appliqué sur les libellés, références et noms des acteurs (insensible aux accents) »). Le service n'a pas de filtre de date : pour « les ordres de la
semaine », lire la date de création des ordres renvoyés, et dire si la liste est
partielle.

### Lire la réponse

Par ordre : "title", "state" (l'état d'avancement, tel que Logii le nomme),
"created_on", "works_start_on" (date de démarrage), "received_on" (date de réception),
"building", "tenant" (« locataire occupant si applicable »), "owner", "client"
(« client souscripteur »), "agency", "mandate".

### Ce que la documentation ne définit pas

- Un ordre « en retard », un délai d'intervention ou une date prévue de fin : non défini par la documentation. Donner les dates renvoyées.
- La liste des valeurs d'état : non défini par la documentation ; la rapporter telle
  quelle.

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

To list work orders and follow the progress of works.

### Logii service used

- **Ordres de service**: tool `list_work_orders`. Documentation "Lojii - Les API disponibles", section 5. Gérance locative; specification openapi-1.0.0.json · /api/api/v1/os_gerance. The documentation: "Ordres de service et suivi des travaux : intitulé, état d'avancement, dates de création, de démarrage et de réception, avec les acteurs associés (immeuble, locataire, propriétaire, agence, gestionnaire)".

### What you need before calling

No required input. Optional: a search term of at least two characters ("Terme de recherche global appliqué sur les libellés, références et noms des acteurs (insensible aux accents)"). The service has no date filter: for "this week's orders", read the creation
date of the orders returned, and say whether the list is partial.

### Reading the answer

Per order: "title", "state" (progress, as Logii names it), "created_on",
"works_start_on" (start date), "received_on" (acceptance date), "building", "tenant"
("locataire occupant si applicable"), "owner", "client" ("client souscripteur"),
"agency", "mandate".

### What the documentation does not define

- A "late" order, a response time or a planned end date: not defined by the documentation. Give the dates returned.
- The list of state values: not defined by the documentation; report it as is.

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

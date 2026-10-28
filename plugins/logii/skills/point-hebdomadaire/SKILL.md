---
name: point-hebdomadaire
description: >
  Préparer le point hebdomadaire du cabinet à partir de Logii : échéances de mandats, vacants, ordres de service et honoraires. Prepare the agency's weekly briefing from Logii: mandate expiries, vacancies, work orders and fees.
  Use when the user asks for a weekly briefing, a weekly agency review, or what to look at this week (point de la semaine).
user-invocable: true
---

## Français

Pour composer, en un seul message, le point de la semaine du cabinet, à partir de plusieurs
services Logii. Chaque rubrique dit de quel service elle vient.

### Services Logii utilisés

- **Mandats de gestion** : outil `list_management_mandates`. Documentation « Lojii - Les API disponibles », section 5. Gérance locative ; spécification openapi-1.0.0.json · /api/api/v1/mandat_gestion. Mandats de gestion dont la date de fin tombe dans l'horizon choisi.
- **Mandats de copropriété** : outil `list_syndic_mandates`. Documentation « Lojii - Les API disponibles », section 4. Syndic de copropriété ; spécification openapi-1.0.0.json · /api/api/v1/mandat_copropriete. Mandats de copropriété dont la date de fin tombe dans l'horizon choisi.
- **Contrats locataires** : outil `list_tenant_contracts`. Documentation « Lojii - Les API disponibles », section 5. Gérance locative ; spécification openapi-1.0.0.json · /api/suivilocataire/v1/contrat_locataire. Les lots vacants (type Vacant).
- **Ordres de service** : outil `list_work_orders`. Documentation « Lojii - Les API disponibles », section 5. Gérance locative ; spécification openapi-1.0.0.json · /api/api/v1/os_gerance. Les ordres de service créés dans la semaine.
- **Honoraires facturés** : outil `list_fee_invoices`. Documentation « Lojii - Les API disponibles », section 6. Facturation et honoraires du cabinet ; spécification openapi-1.0.0.json · /api/api/v1/honoraire_facture. Les honoraires facturés dans la semaine.
- **Paiements d'honoraires** : outil `list_fee_payments`. Documentation « Lojii - Les API disponibles », section 6. Facturation et honoraires du cabinet ; spécification openapi-1.0.0.json · /api/api/v1/honoraire_paiement. Les honoraires encaissés dans la semaine.

### Ce qu'il faut avoir avant d'appeler

Demander en une seule question, puis ne plus interrompre :

1. la semaine (par défaut : la semaine en cours ; en annoncer les dates) ;
2. l'horizon des échéances de mandats (par exemple 90 jours) : ne pas le choisir à la place
   de l'utilisateur ;
3. le ou les codes d'activité pour les honoraires (« Type d'activité (Syndic ou
   Gérance) », correspondance non définie par la documentation). Sans code, la rubrique
   honoraires est omise et le point dit pourquoi.

### Composer le point

Une rubrique par service, dans cet ordre, chacune avec la source entre parenthèses :

1. Échéances des mandats de gestion (Mandats de gestion) : numéro, date de fin, état.
2. Échéances des mandats de syndic (Mandats de copropriété) : copropriété, date de fin,
   état.
3. Lots vacants (Contrats locataires) : lots, « Date de disponibilité (si vacant) »,
   « Raison de non-location ».
4. Ordres de service de la semaine (Ordres de service) : le service n'a pas de filtre de
   date ; garder ceux dont la date de création tombe dans la semaine, et dire si la liste
   lue était partielle.
5. Honoraires de la semaine (Honoraires facturés, Paiements d'honoraires) : total facturé
   et total encaissé, côte à côte.

Une rubrique vide se dit « rien renvoyé » ; une rubrique refusée se dit « refusé », avec la
raison. Ne pas ajouter de priorité, de recommandation ou d'alerte que les données ne
portent pas.

### Ce que la documentation ne définit pas

- Ce qui est « urgent », « à risque » ou « en retard » : non défini par la documentation.
- Le reste à encaisser (facturé moins encaissé) : non défini par la documentation ; donner
  les deux totaux.

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

To compose, in one message, the agency's weekly briefing from several Logii services. Each
section says which service it comes from.

### Logii services used

- **Mandats de gestion**: tool `list_management_mandates`. Documentation "Lojii - Les API disponibles", section 5. Gérance locative; specification openapi-1.0.0.json · /api/api/v1/mandat_gestion. Management mandates whose end date falls within the chosen horizon.
- **Mandats de copropriété**: tool `list_syndic_mandates`. Documentation "Lojii - Les API disponibles", section 4. Syndic de copropriété; specification openapi-1.0.0.json · /api/api/v1/mandat_copropriete. Condominium mandates whose end date falls within the chosen horizon.
- **Contrats locataires**: tool `list_tenant_contracts`. Documentation "Lojii - Les API disponibles", section 5. Gérance locative; specification openapi-1.0.0.json · /api/suivilocataire/v1/contrat_locataire. Vacant lots (Vacant kind).
- **Ordres de service**: tool `list_work_orders`. Documentation "Lojii - Les API disponibles", section 5. Gérance locative; specification openapi-1.0.0.json · /api/api/v1/os_gerance. Work orders created during the week.
- **Honoraires facturés**: tool `list_fee_invoices`. Documentation "Lojii - Les API disponibles", section 6. Facturation et honoraires du cabinet; specification openapi-1.0.0.json · /api/api/v1/honoraire_facture. Fees invoiced during the week.
- **Paiements d'honoraires**: tool `list_fee_payments`. Documentation "Lojii - Les API disponibles", section 6. Facturation et honoraires du cabinet; specification openapi-1.0.0.json · /api/api/v1/honoraire_paiement. Fees collected during the week.

### What you need before calling

Ask in a single question, then do not interrupt again:

1. the week (default: the current week; state its dates);
2. the horizon for mandate expiries (for example 90 days): do not pick it for the user;
3. the activity code(s) for fees ("Type d'activité (Syndic ou Gérance)", mapping not defined by the documentation). Without a code the fee section is omitted and the
   briefing says why.

### Composing the briefing

One section per service, in this order, each with its source in brackets:

1. Management mandate expiries (Mandats de gestion): number, end date, status.
2. Syndic mandate expiries (Mandats de copropriété): condominium, end date, state.
3. Vacant lots (Contrats locataires): lots, "Date de disponibilité (si vacant)", "Raison
   de non-location".
4. This week's work orders (Ordres de service): the service has no date filter; keep those
   whose creation date falls in the week, and say whether the list read was partial.
5. This week's fees (Honoraires facturés, Paiements d'honoraires): invoiced total and
   collected total, side by side.

An empty section reads "nothing returned"; a refused section reads "refused", with the
reason. Do not add a priority, recommendation or alert the data does not carry.

### What the documentation does not define

- What is "urgent", "at risk" or "late": not defined by the documentation.
- Remaining to collect (invoiced minus collected): not defined by the documentation; give
  both totals.

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

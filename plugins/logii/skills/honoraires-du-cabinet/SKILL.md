---
name: honoraires-du-cabinet
description: >
  Suivre les honoraires du cabinet dans Logii : facturés, lettrés, encaissés, sur une période. Follow the agency's fees in Logii: invoiced, matched, collected, over a period.
  Use when the user asks for fees invoiced, fees collected, fee payments or matchings (lettrages) over a period, for syndic or management.
user-invocable: true
---

## Français

Pour suivre, sur une période d'écriture, les honoraires facturés, leur rapprochement avec
les règlements, et les encaissements. Ce domaine couvre indifféremment le syndic et la
gérance.

### Services Logii utilisés

- **Honoraires facturés** : outil `list_fee_invoices`. Documentation « Lojii - Les API disponibles », section 6. Facturation et honoraires du cabinet ; spécification openapi-1.0.0.json · /api/api/v1/honoraire_facture. Facturés. La documentation : « Pièces d'honoraires : date, libellé, montants HT, TVA et TTC, détail des lignes, journal comptable, mandant, portefeuilles comptable et gestionnaire, indicateur d'annulation ».
- **Lettrages d'honoraires** : outil `list_fee_settlements`. Documentation « Lojii - Les API disponibles », section 6. Facturation et honoraires du cabinet ; spécification openapi-1.0.0.json · /api/api/v1/honoraire_lettrage. Lettrés. La documentation : « Rapprochement entre factures et règlements : date de lettrage, montants au débit et au crédit, montant TTC, traçabilité des annulations ».
- **Paiements d'honoraires** : outil `list_fee_payments`. Documentation « Lojii - Les API disponibles », section 6. Facturation et honoraires du cabinet ; spécification openapi-1.0.0.json · /api/api/v1/honoraire_paiement. Encaissés. La documentation : « Encaissements effectifs rattachés aux factures d'honoraires : date, libellé, montant, journal, mandant, traçabilité des annulations ».

### Ce qu'il faut avoir avant d'appeler

Les trois outils exigent ensemble, sinon ils refusent [filter_required] (le champ
"missing" dit lesquels manquent) :

1. l'activité : un code numérique, « Type d'activité (Syndic ou Gérance) » ;
2. la date de début et la date de fin de la période.

La correspondance entre le code et Syndic ou Gérance est non définie par la documentation :
demander le code à l'utilisateur (son administrateur Logii le connaît). Ne jamais supposer
qu'un code vaut Syndic ou Gérance. Si l'utilisateur dit « ce mois-ci », en déduire les dates
et les annoncer.

La période ne porte pas sur la même date selon l'outil : « Date d'écriture » pour les
honoraires facturés, « Date de lettrage » pour les lettrages, « Date de paiement » pour les
paiements.

### Lire la réponse

- Facturés, par ligne : "date", "reference", "journal", "label", "piece_excl_tax",
  "piece_vat", "piece_incl_tax" (montants de la pièce, répétés sur chacune de ses
  lignes), "cancelled", "principal" (le mandant), "details", et avec
  "include_portfolios" les portefeuilles comptable et gestionnaire. Totaux : "lines",
  "invoices", "excl_tax", "vat", "incl_tax", "cancelled_invoices" ; chaque pièce n'y est
  comptée qu'une fois.
- Lettrés : "date" (« Date effective du lettrage »), "amount_incl_tax" (« Montant TTC
  lettré »), "debit" (« Référence à la pièce au débit (facture d'honoraires) »),
  "credit" (« Référence à la pièce au crédit (règlement) »), "cancellation_id". Totaux :
  "settlements", "incl_tax", "cancelled_settlements".
- Encaissés : "date", "journal", "label", "amount" (« Montant débité (paiement) »),
  "cancelled", "principal". Totaux : "rows", "payments", "amount",
  "cancelled_payments" ; chaque paiement n'y est compté qu'une fois.

### Ce que la documentation ne définit pas

- Le « reste à encaisser » ou les honoraires « impayés » : non défini par la documentation. Ne pas soustraire l'encaissé du facturé ; donner les deux totaux.
- Si une pièce annulée doit être retirée d'un total : non défini par la documentation.
  Donner le total renvoyé et, à part, le nombre de pièces annulées.
- La correspondance des codes d'activité : non défini par la documentation.

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

To follow, over an entry period, the fees invoiced, their matching with payments, and the
fees collected. This domain covers syndic and management alike.

### Logii services used

- **Honoraires facturés**: tool `list_fee_invoices`. Documentation "Lojii - Les API disponibles", section 6. Facturation et honoraires du cabinet; specification openapi-1.0.0.json · /api/api/v1/honoraire_facture. Invoiced. The documentation: "Pièces d'honoraires : date, libellé, montants HT, TVA et TTC, détail des lignes, journal comptable, mandant, portefeuilles comptable et gestionnaire, indicateur d'annulation".
- **Lettrages d'honoraires**: tool `list_fee_settlements`. Documentation "Lojii - Les API disponibles", section 6. Facturation et honoraires du cabinet; specification openapi-1.0.0.json · /api/api/v1/honoraire_lettrage. Matched. The documentation: "Rapprochement entre factures et règlements : date de lettrage, montants au débit et au crédit, montant TTC, traçabilité des annulations".
- **Paiements d'honoraires**: tool `list_fee_payments`. Documentation "Lojii - Les API disponibles", section 6. Facturation et honoraires du cabinet; specification openapi-1.0.0.json · /api/api/v1/honoraire_paiement. Collected. The documentation: "Encaissements effectifs rattachés aux factures d'honoraires : date, libellé, montant, journal, mandant, traçabilité des annulations".

### What you need before calling

All three tools require, together, or they refuse [filter_required] (the "missing" field
names what is missing):

1. the activity: a numeric code, "Type d'activité (Syndic ou Gérance)";
2. the start date and the end date of the period.

The mapping between the code and Syndic or Gérance is not defined by the documentation:
ask the user for the code (their Logii administrator knows it). Never assume a code means
Syndic or Gérance. If the user says "this month", derive the dates and state them.

The period does not apply to the same date in each tool: "Date d'écriture" for invoiced
fees, "Date de lettrage" for matchings, "Date de paiement" for payments.

### Reading the answer

- Invoiced, per line: "date", "reference", "journal", "label", "piece_excl_tax",
  "piece_vat", "piece_incl_tax" (the piece's amounts, repeated on each of its lines),
  "cancelled", "principal", "details", and with "include_portfolios" the accounting
  and manager portfolios. Totals: "lines", "invoices", "excl_tax", "vat", "incl_tax",
  "cancelled_invoices"; each piece is counted once.
- Matched: "date" ("Date effective du lettrage"), "amount_incl_tax" ("Montant TTC
  lettré"), "debit" ("Référence à la pièce au débit (facture d'honoraires)"), "credit"
  ("Référence à la pièce au crédit (règlement)"), "cancellation_id". Totals:
  "settlements", "incl_tax", "cancelled_settlements".
- Collected: "date", "journal", "label", "amount" ("Montant débité (paiement)"),
  "cancelled", "principal". Totals: "rows", "payments", "amount",
  "cancelled_payments"; each payment is counted once.

### What the documentation does not define

- "Remaining to collect" or "unpaid" fees: not defined by the documentation. Do not
  subtract collected from invoiced; give both totals.
- Whether a cancelled piece should be removed from a total: not defined by the documentation. Give the total returned and, separately, the number of cancelled pieces.
- The mapping of activity codes: not defined by the documentation.

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

---
name: baux-et-vacants
description: >
  Consulter les baux, les lots vacants et les candidatures dans Logii. Look up leases, vacant lots and applications in Logii.
  Use when the user asks for leases, vacancies, empty lots, applicants, notices given or tenants leaving.
user-invocable: true
---

## Français

Pour lister les baux, les lots vacants et les candidatures, et lire les dates d'entrée, de
sortie, de congé et de préavis.

### Services Logii utilisés

- **Contrats locataires** : outil `list_tenant_contracts`. Documentation « Lojii - Les API disponibles », section 5. Gérance locative ; spécification openapi-1.0.0.json · /api/suivilocataire/v1/contrat_locataire. La documentation : « Contrats locataires : Baux, lots vacants et candidatures : locataires et garants, lots concernés, loyer, dates de début, de fin, d'entrée et de sortie, congé et préavis, … état et phase du contrat, motif de non-location ».
- **Lots** : outil `list_lots`. Documentation « Lojii - Les API disponibles », section 3. Référentiel et organisation du cabinet ; spécification openapi-1.0.0.json · /api/public/v1/lots. Pour décrire les lots d'un vacant (étage, pièces) à partir de leur identifiant.

### Ce qu'il faut avoir avant d'appeler

Au moins un filtre, sinon l'outil refuse [filter_required] : le type de contrat
("kind" : "lease" = Bail, "vacant" = Vacant, "candidate" = Candidat, selon la
spécification « 2: Bail, 3: Vacant, 4: Candidat »), un locataire (identifiant ou nom), un
contrat, un immeuble, un propriétaire, un mandat, ou une période de début ou de création.
« Les vacants » se demande avec le type Vacant ; pour un immeuble, ajouter son identifiant.

### Lire la réponse

Par contrat : "reference" (« Référence externe du contrat »), "kind" (« Type de dossier
(Bail, Vacant, Candidat) »), "phase" (« Phase actuelle du contrat »), "start", "end"
(« Date de fin ou de résiliation »), "entry_on" (« Date d'entrée dans les lieux »),
"exit_on" (« Date de sortie des lieux »), "notice_given_on" (« Date de congé »),
"notice_received_on" (« Date de réception du préavis »), "available_on" (« Date de disponibilité (si vacant) »), "not_let_reason" (« Raison de non-location »),
"is_signed", "rent_ttc" (« Montant du loyer TTC »), "tenant_name" (« Nom complet du
locataire principal »), "tenants", "lots".

Les garants, la révision, les honoraires, la garantie loyers impayés, le mode de règlement
et l'usage que cite la documentation ne sont pas servis par ce connecteur.

### Ce que la documentation ne définit pas

- La durée de vacance, un taux de vacance ou un délai de relocation : non défini par la documentation. Ne pas les calculer ; donner les dates renvoyées.
- La liste des valeurs de "phase" et de "not_let_reason" : non défini par la documentation ; les rapporter telles quelles.

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

To list leases, vacant lots and applications, and read entry, exit, notice and
notice-received dates.

### Logii services used

- **Contrats locataires**: tool `list_tenant_contracts`. Documentation "Lojii - Les API disponibles", section 5. Gérance locative; specification openapi-1.0.0.json · /api/suivilocataire/v1/contrat_locataire. The documentation: "Contrats locataires : Baux, lots vacants et candidatures : locataires et garants, lots concernés, loyer, dates de début, de fin, d'entrée et de sortie, congé et préavis, … état et phase du contrat, motif de non-location".
- **Lots**: tool `list_lots`. Documentation "Lojii - Les API disponibles", section 3. Référentiel et organisation du cabinet; specification openapi-1.0.0.json · /api/public/v1/lots. To describe a vacancy's lots (floor, rooms) from their identifier.

### What you need before calling

At least one filter, or the tool refuses [filter_required]: the contract kind ("kind":
"lease" = Bail, "vacant" = Vacant, "candidate" = Candidat, per the specification "2: Bail,
3: Vacant, 4: Candidat"), a tenant (identifier or name), a contract, a building, an owner,
a mandate, or a start or creation period. "Vacancies" is asked with the Vacant kind; for a
building, add its identifier.

### Reading the answer

Per contract: "reference", "kind" ("Type de dossier (Bail, Vacant, Candidat)"), "phase"
("Phase actuelle du contrat"), "start", "end" ("Date de fin ou de résiliation"),
"entry_on" ("Date d'entrée dans les lieux"), "exit_on" ("Date de sortie des lieux"),
"notice_given_on" ("Date de congé"), "notice_received_on" ("Date de réception du
préavis"), "available_on" ("Date de disponibilité (si vacant)"), "not_let_reason"
("Raison de non-location"), "is_signed", "rent_ttc" ("Montant du loyer TTC"),
"tenant_name", "tenants", "lots".

Guarantors, rent review, fees, rent guarantee insurance, payment method and use, which the
documentation mentions, are not served by this connector.

### What the documentation does not define

- Vacancy duration, a vacancy rate or a reletting delay: not defined by the documentation.
  Do not compute them; give the dates returned.
- The list of "phase" and "not_let_reason" values: not defined by the documentation;
  report them as is.

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

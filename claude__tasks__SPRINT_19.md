# Sprint 19 — Accountantgast en auditwerkruimte

## Verplichte instructie voor Claude
Lees `CLAUDE.md`, README, `docs/13_JAARREKENING_EN_ACCOUNTANTSPORTAAL.md`, `docs/06_ROADMAP.md`, relevante risico-/data-/architectuurdocumenten en het implementatielog. Werk **uitsluitend Sprint 19** af. Gebruik uitsluitend synthetische testdata. Start volgende sprint niet zonder expliciete GO.

## Scope en taken
Implementeer `external_auditor_portal`: tijdelijke engagement-scoped guest uitnodiging, MFA, ABAC op admin/entiteit/periode/documents, browse/drilldown, evidence requests en review comments in aparte workspace, gescheiden exportrecht, revoke/expiry, AI retrieval authorization. Audit logging voor toegang/export.

1. Controleer voorafgaande sprints en bestaande code; ontbrekende contracts vastleggen in open beslissingen.
2. Schrijf negatieve security-tests en financial golden tests vóór implementatie.
3. Implementeer migraties, modellen, policies, typed commands, API, UI en exports met versioned audit trail.
4. Documenteer data-/AI-egress per datacategorie, provider, regio en doel; externe AI default deny.
5. Voer typecheck, unit/integratie/acceptatie/security-tests uit; leg echte resultaten vast. Geen schijnimplementaties voor kritieke functies.

## Exit-gate
Gast is technisch read-only voor financiële commands, kan gericht bewijs bekijken en opmerkingen maken, ziet geen data buiten scope, exports alleen met aparte grant, revoked tokens en downloads direct onbruikbaar.

## Eindrapport
Gewijzigde bestanden | tests & resultaten | financiële aansluitingen | tenant/entity/guest scopes | AI-datastromen | rest-risico's | open keuzes | GO/NO-GO.


## v0.5 — verplichte team-, handleiding- en synchronisatiegate
- Orkestreer architect/developer/onafhankelijke reviewer/QA/finance+privacy/security-red-team/documentation/release gate volgens `docs/14_MULTI_AGENT_DEVELOPMENT_TEAM.md` (parallel als ondersteund, anders gescheiden sequentiële reviews met eerlijke vermelding).
- Red-team probeert uitsluitend **lokale synthetische testdata** te benaderen via geautoriseerde negatieve tests. Leg findings, fixes en hertesten vast; geen productie of echte klanten.
- Werk bij iedere zichtbare of functionele verandering `docs/16_GEBRUIKERSHANDLEIDING.md` en zo nodig `docs/17_BEHEER_EN_OPERATIONS_HANDLEIDING.md` bij. Alleen geteste functies krijgen status GEIMPLEMENTEERD/GEVALIDEERD.
- Bouw na Markdown-wijzigingen leesbare docs met `python scripts/build_readable.py`; vereis `python scripts/build_readable.py --check` groen. Lever source→render checksumrapport en code→test→handleiding traceability aan.
- NO-GO bij documentation drift, ontbrekende handleiding, scope escape of kritieke open securityfinding.

> **v0.6 module-gate:** Lees `modules/INDEX.md` en bijbehorende modulespecificaties; registreer nieuwe wensen in die module vóór code. Houd NL/EN handleidingen en 1-op-1 HTML/PDF synchroon. Nieuwe functionaliteit volgt de afzonderlijke sprints 22–28; implementeer die niet per ongeluk in deze sprint.

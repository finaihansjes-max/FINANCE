# Sprint 09 — Crediteurenproces

## Instructie voor Claude
Lees `CLAUDE.md`, README, alle `docs/*.md` en de voorgaande implementatielog. Werk **uitsluitend Sprint 09** af en start geen volgende sprint zonder expliciete GO. Alle data synthetisch; geen live betaling of echte belastingindiening.

## Werkpakket
Maak supplier onboarding, independently verified IBAN changes, PO/GR 2-/3-way match, duplicate prevention, invoice approval and controlled posting.

## Technische werkwijze
1. Controleer prerequisite sprints, bestaande contracten, migraties en CI; noem ontbrekende afhankelijkheden.
2. Schrijf eerst tests (inclusief security-negative cases), dan migrations/types, backend policy en command/query services, UI waar relevant, en documentatie.
3. Werk `docs/IMPLEMENTATION_LOG.md`, THREAT_MODEL en betrokken risico-/datastroomdocs bij.
4. Run typecheck, lint, tests en regressies. Registreer AI-provider- en datacategorie-impact expliciet, ook als die nul is.
5. Lever geen schijnimplementatie voor kritieke controls; benoem iedere placeholder en verhinder productiegebruik.

## Acceptatie
Fraudulent new bank details blocked, positive and negative AP flows reconcile to ledger, tenant/module rights enforced.

## Exitrapport
`Gewijzigde bestanden | migraties | tests/resultaten | scope & rechten | data naar AI | risico's | open besluiten | GO/NO-GO`. Vraag GO voor Sprint 10 wanneer toepasselijk; voer die sprint niet uit.


## v0.5 — verplichte team-, handleiding- en synchronisatiegate
- Orkestreer architect/developer/onafhankelijke reviewer/QA/finance+privacy/security-red-team/documentation/release gate volgens `docs/14_MULTI_AGENT_DEVELOPMENT_TEAM.md` (parallel als ondersteund, anders gescheiden sequentiële reviews met eerlijke vermelding).
- Red-team probeert uitsluitend **lokale synthetische testdata** te benaderen via geautoriseerde negatieve tests. Leg findings, fixes en hertesten vast; geen productie of echte klanten.
- Werk bij iedere zichtbare of functionele verandering `docs/16_GEBRUIKERSHANDLEIDING.md` en zo nodig `docs/17_BEHEER_EN_OPERATIONS_HANDLEIDING.md` bij. Alleen geteste functies krijgen status GEIMPLEMENTEERD/GEVALIDEERD.
- Bouw na Markdown-wijzigingen leesbare docs met `python scripts/build_readable.py`; vereis `python scripts/build_readable.py --check` groen. Lever source→render checksumrapport en code→test→handleiding traceability aan.
- NO-GO bij documentation drift, ontbrekende handleiding, scope escape of kritieke open securityfinding.

> **v0.6 module-gate:** Lees `modules/INDEX.md` en bijbehorende modulespecificaties; registreer nieuwe wensen in die module vóór code. Houd NL/EN handleidingen en 1-op-1 HTML/PDF synchroon. Nieuwe functionaliteit volgt de afzonderlijke sprints 22–28; implementeer die niet per ongeluk in deze sprint.

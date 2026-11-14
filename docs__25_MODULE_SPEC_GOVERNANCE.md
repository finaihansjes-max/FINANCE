# 25 — Module-specifieke requirements en Claude-routering

## Status en scope
**GEPLAND**. Dit document beschrijft eisen en acceptatievoorwaarden, niet een reeds werkende implementatie. Bij iedere wijziging: privacy-impact, rechten, finance-controls, geautomatiseerde tests, handleidingen NL/EN, document-rendering en reviewersignoff bijwerken. Alle voorbeelden gebruiken synthetische testdata.

## Module first: later wensen toevoegen
Elke module heeft `modules/<slug>/README.md` met status, scope, functionele eisen, open wensen, regels, events/data, rechten, AI-opt-in, tests, interfaces, handleiding-impact en besluiten. `modules/INDEX.md` is het register van modules en eigenaarschap.

## Verplicht Claude-protocol wanneer gebruiker nieuwe wens noemt
1. Interpreteer de wens; zoek in `modules/INDEX.md` naar bestaande module(s). Voor 'debiteren' moet eerst vastgesteld worden of `debiteuren` bedoeld is; bij duidelijke context schrijf in `modules/debiteuren/README.md`.
2. Lees modulebestand, relevante docs/architectuur, bestaande code/tests en gekoppelde modules. Voeg wens toe onder `Nieuwe wensen / backlog`, inclusief oorspronkelijke formulering (waar relevant), datum, status `VOORGESTELD` en acceptatiecriteria.
3. Controleer impact op ledger, AI, privacy, multi-entity, export, i18n, templates, Guided Learning, workflows, migratie en beveiliging; update cross-module bestanden alleen indien nodig. Geef traceability-links.
4. Wijzig de canonieke module-specificatie **voordat** code ontwikkeld wordt. Geen stille scope-uitbreiding. Werk `docs/06_ROADMAP.md` en relevante sprinttaak bij als prioriteit/afhankelijkheden veranderen; geen automatische sprintstart zonder GO.
5. Houd handleidingen voor beide talen en leesbare gegenereerde render 1-op-1 synchroon in dezelfde wijziging. Markeer voorstellen duidelijk als **GEPLAND**, geen niet-bestaande functionaliteit als beschikbaar beschrijven.
6. Een onafhankelijke reviewer verifieert of de wens in juiste module staat en of conflicterende/gedupliceerde eisen ontbreken.

## Standaard bestandsindeling
`modules/<slug>/README.md` is canoniek; optioneel aparte `WORKFLOWS.md`, `DATA_MODEL.md` en `TEST_CASES.md` wanneer inhoud groeit. Geen verspreide, onvindbare wensen in losse chats. Modulebestanden zijn onderdeel van sync-manifest en leesbare HTML/PDF.

## Governance
Module mag opt-in zijn, maar platformkern/audit/security blijft verplicht. Elke module beschrijft duidelijk tenant-/entiteitscope, permissies en persoonsgegevens/AI-datastromen.

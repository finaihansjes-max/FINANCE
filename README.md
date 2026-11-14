# AI Finance Platform — projectblueprint

Versie: 0.9 (ontwerp, geen productgaranties) · Datum: 2026-11-14 · Doelgroep: Nederlandse MKB-bedrijven

## Doel
Een AI-first, multi-tenant financieel platform met deterministische boekhoudkern, functiescheiding, configureerbare autorisaties en menselijke goedkeuring van kritieke beslissingen. Een team van gespecialiseerde AI-agents mag analyseren, voorbereiden en elkaar beoordelen, maar **nooit** de onafhankelijke policy-engine, menselijke autorisatie of financiële invariant vervangen.

## Bestanden en leesvolgorde
1. `CLAUDE.md` — verplichte instructies voor Claude Code/Claude.
2. `docs/01_PRODUCT_REQUIREMENTS.md` — scope, schermen, rollen, exports, workflows.
3. `docs/02_ARCHITECTURE.md` — architectuur, trust boundaries, datamodel, API's.
4. `docs/03_AI_AGENT_TEAM.md` — agentrollen, onderlinge controles, veilige orchestratie.
5. `docs/04_DATA_GOVERNANCE_SECURITY.md` — precies wat AI met financiële data doet en welke controles verplicht zijn.
6. `docs/05_RISK_REGISTER.md` — risicoanalyse, mitigaties, tests, eigenaarschap.
7. `docs/06_ROADMAP.md` — bouwvolgorde, afhankelijkheden, exitcriteria.
8. `docs/07_TEST_AND_ACCEPTANCE.md` — teststrategie en release-gates.
9. `docs/08_DATA_PRIVACY_VOOR_KLANTEN.md` — klantgerichte verklaring, **concept: alleen publiceren na verificatie van werkelijke implementatie**.
10. `docs/09_DECISIONS_AND_OPEN_QUESTIONS.md` — ADR's en te nemen beslissingen.
11. `claude/tasks/SPRINT_00.md` — eerste uitvoerbare opdracht.
12. `docs/10_SOURCES.md` — onderzoeksbronnen en verificatiepunten.

## Hoe gebruiken
Kopieer de hele map naar een nieuwe git-repository. Open de repository in Claude Code. Laat Claude eerst `CLAUDE.md` en alle documenten onder `docs/` lezen en vervolgens **uitsluitend Sprint 00** uitvoeren. Laat Claude na iedere sprint tests uitvoeren, een implementatieverslag geven en wachten op expliciete GO voor de volgende fase.

## Niet verwarren
- Claude Code als **ontwikkelassistent** is iets anders dan een Claude-model als **runtime-AI-leverancier** in het toekomstige product.
- Een AI-review door een tweede agent is **geen onafhankelijke boekhoudkundige of beveiligingscontrole**.
- 'Geen modeltraining' betekent **niet** automatisch 'geen externe verwerking' of 'zero retention'.
- Deze documenten beschrijven beoogde maatregelen, niet reeds geïmplementeerde garanties.

## Uitbreiding v0.2 — configureerbare modules + consolidatie
- `docs/11_MODULES_AND_PROFILES.md`: onboardingprofielen, modules, opt-ins, AI-gegevenskeuzes, afhankelijkheden en deactivering.
- `docs/12_MULTI_ENTITY_AND_CONSOLIDATION.md`: onderscheid juridische entiteit vs administratie vs bedrijfsonderdeel, optionele dimensies, consolidatiegroepen, read-only views en eliminatieboekingen.
- `claude/tasks/SPRINT_01.md` t/m `SPRINT_19.md`: per werkpakket scope, concrete taken en acceptatie; voer steeds maar één sprint uit.
- De roadmap bevat nu 20 werkpakketten (00–19); eerdere sprintnummers zijn hierdoor verplaatst. Gebruik de **nieuwe** `docs/06_ROADMAP.md` als bron van waarheid.

**Voorbeeld:** profiel ZZP -> basisfacturatie + direct opt-in `uitgebreide_rapportages` tijdens onboarding. Een groep Retail/Wholesale -> aparte administraties waar geboekt wordt, plus een geconsolideerde read-only rapportage met in een apart afgeschermde werkruimte goedgekeurde intercompany-eliminaties. Deze voorbeelden mogen niet worden omgezet in geforceerde vaste hiërarchieën.

## Uitbreiding v0.3 — jaarrekening & accountantsinzage
- `docs/13_JAARREKENING_EN_ACCOUNTANTSPORTAAL.md` — opt-in zelf jaarrekening opstellen; rechtsvorm- en bedrijfsklassechecks; jaarrekeningstatussen en verifieerbare filing; gastaccountant met engagement-scoped read-only auditportal.
- `claude/tasks/SPRINT_18.md` en `SPRINT_19.md` — nieuwe implementatiesprints; security hardening verplaatst naar Sprint 20 en supervised pilot naar Sprint 21.
- `docs/06_ROADMAP.md` is bron van waarheid. Deze blueprint bestaat nu uit **22 sprints (00–21)**. De eerdere 18/19 zijn hernummerd om nieuwe functionaliteit vóór de finale audit/pilot te bouwen.
- Geen controleplicht betekent niet per se geen jaarrekening- of deponeringsplicht. Software mag nooit zonder geverifieerde regels verklaren dat wettelijk deponeren of een accountantscontrole niet nodig is.

## v0.5 — verplicht ontwikkelteam + altijd actuele handleiding
- `docs/14_MULTI_AGENT_DEVELOPMENT_TEAM.md`: gescheiden AI-developmentagents (architect, developers, onafhankelijke reviewer, QA, finance, privacy, security-red-team, documentation en release gate).
- `docs/15_DOCUMENTATION_SYNC_POLICY.md`: **Markdown en leesbare HTML/PDF zijn verplicht 1-op-1**, met automatische generator, hashmanifest en CI-check.
- `docs/16_GEBRUIKERSHANDLEIDING.md`: altijd aanwezige, actueel gehouden gebruikershandleiding; in deze blueprint nog duidelijk als **GEPLAND** gemarkeerd.
- `docs/17_BEHEER_EN_OPERATIONS_HANDLEIDING.md`: beheerhandleiding, eveneens mee te wijzigen per sprint.
- `scripts/build_readable.py`: voer `python scripts/build_readable.py` uit na iedere Markdown-wijziging en `python scripts/build_readable.py --check` vóór elke merge. **Nooit HTML/PDF afzonderlijk herschrijven.**
- `Leesbare_documenten/INDEX.html`: alle Markdown-bronnen als HTML; hoofddocumenten ook als PDF. Gemarkeerde status betreft ontwerp, niet bestaande software.

## v0.6 — Uitbreidingen en modulegerichte wensen
- `modules/INDEX.md` bevat alle domeinmodules; iedere module heeft een eigen requirementsdocument. Toekomstige wensen (bijvoorbeeld Debiteuren) eerst in de juiste modulemap vastleggen, dan architectuur/roadmap/review aanpassen.
- Nieuwe documenten `docs/18_*` t/m `docs/25_*` beschrijven NL/EN, universele migratie, gecontroleerde parallelle proef, templates, Guided Learning en modulegovernance.
- De oorspronkelijke 22 sprints blijven behouden; nieuwe sprinttaken 22–28 plannen de uitbreidingen. Architectuur-invarianten moeten al in Sprint 00–03 worden meegenomen.
- Leesbare HTML/PDF volgen altijd automatisch uit de MD-bronnen; `python scripts/build_readable.py --check` is release-gate.

## v0.7 — Standaarddesign en navigatie
- `docs/26_DESIGN_SYSTEM_AND_NAVIGATION.md`: eigen lichte standaardinterface, linkernavigatie of topnavigatie per gebruiker, toegankelijkheid, design tokens, presentatie-invarianten en risico's bij herkenbare externe boekhoudtemplates.
- `assets/design/ai_finance_standard_dashboard_reference.png`: door opdrachtgever aangeleverde **interne designreferentie** (conceptafbeelding, geen productiegegevens of te reproduceren merkassets).
- `modules/design-system/README.md`: modulegerichte wensen en acceptatiecriteria. `modules/interface-templates/README.md`: compatibiliteitspresets en layout.
- `claude/tasks/SPRINT_29.md`: afronding en geautomatiseerde tests. Architectuurcontracten horen al in de eerste sprints.
- Alle Markdown-bestanden, leesbare HTML en relevante PDF worden weer automatisch 1-op-1 gegenereerd; zie `scripts/build_readable.py --check`.

## v0.8 — Permanente ERP Integration Hub
Nieuwe uitbreidingsmodule voor langdurige ERP-koppeling. Primair ERP → Finance (orders, voorraadmutaties, operationele facturen); alleen minimale statuscallbacks terug; configureerbare bronautoriteit en één financiële boekingsmotor per event. Zie `docs/27_ERP_INTEGRATION_HUB.md`, `modules/erp-integration-hub/README.md` en `claude/tasks/SPRINT_30.md`. Alle onderdelen blijven **GEPLAND**; huidige ZIP bevat ontwerpdocumentatie, geen werkend boekhoudproduct.

## v0.9 — tenant-subdomeinen, IT's Perfect en demo-backlog
- **Gekozen productbesluit:** ieder bedrijf ontvangt standaard een uniek subdomein op het platformdomein; geen custom domain nodig in eerste release. Zie `docs/28_SAAS_TENANT_SUBDOMAINS.md` en `modules/saas-tenant-domains/README.md`.
- `claude/tasks/SPRINT_31.md`: uitwerking en security/acceptance gate van tenantprovisioning en automatische subdomeinen; contracten vanaf Sprint 00/01.
- `docs/29_ITS_PERFECT_REFERENCE_CONNECTOR.md` / `claude/tasks/SPRINT_32.md`: concrete ERP API v2 referentie inclusief voorlopig quotabeheer, zonder garantie van niet-geverifieerde endpoints.
- `docs/30_PERSONALIZED_DEMO_SANDBOX.md` / `claude/tasks/SPRINT_33.md`: optionele, branche- en bedrijfsprofielafhankelijke synthetische demo-data (nog verder te specificeren).
- Alles is ontwerpdocumentatie, geen reeds gebouwd SaaS-product. Open `Leesbare_documenten/INDEX.html` voor exact uit Markdown gegenereerde leesbare documenten.

## Claude Project: klaar om te starten
- `claude/project/PROJECTBESCHRIJVING_CLAUDE.md` — kant-en-klare projectomschrijving.
- `claude/project/PROJECTINSTRUCTIES_CLAUDE.md` — plak bij Project instructions.
- `claude/project/UPLOADEN_NAAR_CLAUDE_PROJECT.md` — uitleg voor Project knowledge en Claude Code.
- `claude/project/BEGIN_MET_BOUWEN.md` — startprotocol bij het commando **Begin met bouwen**.
- Deze bestanden zijn ook als leesbare HTML beschikbaar via `Leesbare_documenten/INDEX.html`.

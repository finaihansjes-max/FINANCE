# 06 — Bouwroadmap v0.3: opt-in modules + multi-entity + consolidatie + jaarrekening + auditgast

## Werkwijze
Werk steeds **exact één sprinttaak** uit `claude/tasks/SPRINT_XX.md` af. Sprints zijn afhankelijkheidsgerichte increments, geen kalenderbeloften. Claude rapporteert tests/risico's/migraties/privacy-impact en vraagt expliciete GO. De onderstaande nummering **vervangt** de oude v0.1-indeling. Niet voorbij Sprint 00 zonder GO. Geen echte klantdata in dev, geen geldbeweging in tests.

| Sprint | Scope | Deliverables | Exit-gate | Depends on |
|---|---|---|---|---|
| 00 | Repo/scaffold/threat model | Monorepo CI, typed tenant+entity+scope+module contracts, testfixtures | Typecheck, secret scan, deny-by-default fixture | — |
| 01 | Tenant, IAM en scope | Tenants/users, ABAC/SoD, legal entity vs administration vs business unit, group scope contracts | Cross-tenant/cross-entity denial | 00 |
| 02 | Modulecatalogus en onboarding | Profiletemplates, module dependencies, ZZP + rapportage opt-in, tenant toggles, admin UI | Default != beperking; core immutable; server guards | 01 |
| 03 | Administraties + optionele dimensies | Entiteiten/administraties, business units, flexibele dimension types/hierarchies | Geen verplichte kostendrager; acyclic/versioned mappings | 01,02 |
| 04 | Ledger & audit | Double entry, journals, periods, reversals, source fingerprints, audit | Balancing, immutability, admin-scope authorization | 03 |
| 05 | Document intake | Encrypted document store, AV, hashes, retention, ingestion | No unauthorized doc access | 01,04 |
| 06 | AI privacy gateway | Module/AI config separation, provider router, disclosures, agent data scopes | Egress denied by default; ZZP rapportages offline | 02,03,05 |
| 07 | Finance agents: AP + VAT + fraud | Typed proposals, reviews, disagreements, evidence | Prompt-injection tests, proposal-only permissions | 04,06 |
| 08 | Workflow approvals | Policy DSL, names/roles/amount thresholds, MFA, SoD, snapshot versions | Self-approval & post-approval edits blocked | 01,04,07 |
| 09 | Crediteuren | Suppliers, IBAN verification, PO/GR, AP posting, approvals | Duplicate & IBAN fraud controls | 07,08 |
| 10 | UI + uniform XLSX | Navigation administration vs group, module settings, ledger/AP views, export service | Every delivered dataview exports scoped rows | 02–09 |
| 11 | Bank & payments | Bank import, reconciliations, proposals, batch approvals, sandbox | No AI bank authorization, idempotency | 04,08,09 |
| 12 | Debiteuren | AR invoices, credits, sends, receivables | Version-bound send, refund approvals | 04,08 |
| 13 | Btw, close and accounting controls | VAT rules, draft returns, close/reopen, fiscal exports | Expert-reviewed golden tax cases | 04,09,12 |
| 14 | Intercompany matching | IC references, counterpart matching, evidence, disagreements | No automatic write-off; group scoping | 03,04,08,13 |
| 15 | Consolidation ledger + workspace | Group policies, mappings/period/FX, immutable runs, eliminations, gated journals | Source journals unchanged, human approve + SoD | 14 |
| 16 | Read-only consolidatie + reporting | Single-entity, segment roll-up, group read models, drilldown, Excel, AI narratives | No regular POST on group reports; reconciles inputs | 10,15 |
| 17 | AI Control Center + klanttransparantie | Multi-agent reviews, disclosure views, per-group dataflows, incidents | Logs complete, access/redaction verified | 06,16 |
| 18 | Jaarrekeningmodule (opt-in) | Frozen annual statement snapshots, controlled mapping, notes/reviews, legal applicability guard | Ledger-to-accounts reconciliation + human release | 04,08,13,16 |
| 19 | Accountantgast (opt-in) | Timeboxed engagement viewer, audit drilldown/evidence, separate scoped exports, comments | No guest posting, no scope escape, revoke works | 01,06,10,18 |
| 20 | Security/legal/finance hardening | DPIA, contracts, pentest, red-team, accountant UAT, DR testing | Formal Go/No-Go incl konsolidatie quality | 00–19 |
| 21 | Supervised pilot/beta | Synthetic->controlled pilot, observability, incident processes | Residual risks signed off before scaling | 20 |

## Milestones
- **Pilot-MVP operationele AP:** na Sprint 10 + relevante onderdelen van Sprint 20. Mag niet zonder onafhankelijke finance/security/privacy UAT live met echte klantdata.
- **Multi-entity basis:** Sprint 03 levert correcte entiteiten, administraties en analytische segmenten; **juridische consolidatie is dan nog NIET operationeel**.
- **Consolidatie-bèta:** pas na Sprint 16 en alle relevante hardening/auditorcontrole. Begin met één verslagvaluta en eenvoudige volledig-beheerde groepssituaties, zonder te suggereren dat alle internationale IFRS/Dutch GAAP-gevallen automatisch worden ondersteund.

## Belangrijke doorsnijdende eisen voor ELKE sprint
- Modules/profielen zijn vanaf Sprint 02 configureerbaar; geen codepaden die ZZP beperken tot fixed module set. Financiële basis/security altijd aan.
- Tenant + legal entity + ledger administration + user/role permissions gelden in elke API/job/export/modelvraag; consolidated scopes pas na serververificatie.
- AI-data policy apart van module-instelling; track provider/region/purpose/fields/retention en expliciete verified routing.
- AI agents zijn tools, niet trusted approvers. Write commands na deterministische control engine en menselijke goedkeuring waar noodzakelijk.
- Behoud posted history, privacy en audit bij module-deactivatie, dimensiewijziging, groepslidmaatschapswijziging en eliminatie.
- Automatische tests, adversarial negatives, migrations, feature flags, observability, rollback, finance reconciliation, docs update.

## Resultaat per sprint
`Scope | changed files | migrations | test evidence | modules impact | administration/group scope | AI disclosures | risk update | open decisions | GO/NO-GO`.

## Release-gate uitbreiding v0.3
- Jaarrekening (18) en accountantgast (19) zijn opt-in. Ze worden niet automatisch ingeschakeld bij een profielkeuze. Ook ZZP kan deze modules selecteren.
- Voor accountantsreview is read-only op boekhouding verplicht; review-notities en bewijsverzoeken mogen uitsluitend in de aparte engagement-workspace worden toegevoegd.
- Geen garantie van wettelijke compliance, assurance of KVK-deponering zonder toepasselijke, onderhouden fiscale/juridische beslisregels en verificatie van externe ontvangst.
- Final hardening Sprint 20 valideert nu expliciet jaarrekening en guest portal; pilot Sprint 21 vereist die sign-offs.

## v0.5 — dwarsdoorsnijdende taken voor alle 22 sprints
De development agents zijn **de uitvoerders van elke sprint**, geen productfeature die pas in een latere fase wordt toegevoegd. Sprint 00 richt teamcontracten, lokale synthetische securityfixtures en de documentatiepipeline in. Elke daaropvolgende sprint volgt verplicht de cyclus **architect → developers → onafhankelijke review → QA → finance/privacy review → geautoriseerd sandbox-red-team → fix/retest → handleidingupdate → automatische HTML/PDF-render → release gate → GO**.

| Moment | Verplichte output en controle |
|---|---|
| Sprint 00 | `docs/16_GEBRUIKERSHANDLEIDING.md`, `docs/17_BEHEER_EN_OPERATIONS_HANDLEIDING.md`, `scripts/build_readable.py` en CI `--check` worden ingeregeld; synthetische red-team fixtures |
| Elke feature-PR | Requirement-code-test-handleiding traceability, onafhankelijke code review, finance/privacy impact en negatieve securitytests |
| Elke sprint-exit | Handleiding toont *werkelijke implementatiestatus*; MD+leesbare HTML/PDF opnieuw opgebouwd; manifestcheck groen; open kwetsbaarheden beoordeeld |
| Sprint 20 | Externe onafhankelijke pentest en kritieke bedrijfs- en autorisatiecontrols; agentreview is niet voldoende |
| Sprint 21 | Gecontroleerde pilot, eindgebruikerstoets op handleiding, beveiligde onboarding en aantoonbare datastromen |

Een feature die niet in de handleiding staat of waarvan de leesbare versie afwijkt van Markdown mag niet worden vrijgegeven. De ontwikkelteam-red-team-agent mag alleen lokale fictieve testdata proberen te benaderen, nooit echte klantadministraties.

## v0.6 — Vervolgontwikkelsprints 22–28 en module-routering
De bestaande 00–21 blijven inhoudelijk uitgangspunt. Voeg **vroeg** interface-ID’s, modulemappen en NL/EN contracten toe tijdens Sprint 00–03 om onnodige herbouw te voorkomen. De onderstaande inhoudelijke productontwikkelingen starten na de noodzakelijke basis, niet zonder GO.

| Sprint | Feature | Vereiste afhankelijkheden | Exit-gate |
|---|---|---|---|
| 22 | Internationalisatie NL/EN | 00,01,02 | Verificatie alle routes/rapporten en handleidingen |
| 23 | Connector-/migratieframework | 03,04,13,22 | Re-import veilig, dry-run, financial reconciliation |
| 24 | Controlled parallel run + eenrichtingssync + Migration Center | 23,11,12 | Fail-closed rollback readiness en externe-edit detectie |
| 25 | Presentation template engine | 10,22 | Geen invloed op financiële kern, stabiele selector-contracten |
| 26 | Guided Learning plugin zonder AI | 25,08 | No-code tour, selector runtime, echte actiecontrole, geen AI-afhankelijkheid |
| 27 | AI Guided Learning Builder | 26,06 | Alleen drafts, review + human publish, geen AI-runtime |
| 28 | Chatretrieval van gepubliceerde tours | 27,26 | Correcte rol/tenant filtering, geen tour geïmproviseerd tijdens runtime |

### Toekomstige wensen (doorlopende werkstroom)
`modules/INDEX.md` is de routekaart naar afzonderlijke `modules/<slug>/README.md`. Als de gebruiker later een wens over debiteuren geeft, wijzigt Claude **eerst** `modules/debiteuren/README.md` (onder backlog, met testbare acceptatiecriteria), analyseert cross-module-impact, werkt roadmap indien nodig bij, en ontwikkelt niets buiten de goedgekeurde sprint. Documentatie en NL/EN handleidingen synchroon; zie `docs/25_MODULE_SPEC_GOVERNANCE.md`.

## v0.7 — Doorlopende design-contracten en Sprint 29
**Ontwikkelregels vanaf Sprint 00/01/10:** ontwerp tokens en `AppShell`; één `navigation_registry`; `route_id`/`action_id`/`data-guide-id`; per-user voorkeur `navigation_placement` met enum `left_vertical|top_horizontal`; onafhankelijk van profiel/modules/template/locale/AI-opt-in; tenant-/administratierechten op backend. Start met standaard lichte AI Finance-weergave volgens `assets/design/ai_finance_standard_dashboard_reference.png`, met fictieve testdata. Sprint 25 levert de template engine, Sprint 26 valideert de Guided Learning selectors. **Sprint 29** (`claude/tasks/SPRINT_29.md`) finaliseert design system, dashboard, navigatie-instellingen, toegankelijkheid, responsive gedrag en volledige regressiematrix. Dit is de dertigste sprint (00–29), maar fundamentele UX-keuzes mogen **niet** worden uitgesteld tot Sprint 29. Zie `docs/26_DESIGN_SYSTEM_AND_NAVIGATION.md`.

## Guided Learning als betaalde add-on — verplichte release-gate
- Begin met commercieel entitlements-model in de basis/modulearchitectuur (voor Sprint 26), zonder prijsbedragen vast te leggen. Geen profileroute kan Guided Learning standaard activeren.
- Sprint 26: add-on catalogus/entitlement, default-off, activatieflow, runtime-, builder- en API-licentiechecks; geautomatiseerde blokkadetests.
- Sprint 27: AI Builder alleen binnen betaalde Guided Learning met extra AI-consent; test verlopen/ingetrokken licentie.
- Sprint 28: AI-chat mag alleen startbare tours retourneren bij geldige licentie; basis-help blijft bruikbaar zonder add-on.
- NL/EN commerciële informatie, gebruikers- en beheerhandleiding, privilege-/entitlement-tests, docs-sync en reviewer signoff verplicht vóór release.

## v0.8 — Permanente ERP Integration Hub: Sprint 30 (GEPLAND)
**Nieuwe Sprint 30**, als afzonderlijke module naast de Controlled Parallel Migration van Sprint 24. Lees `claude/tasks/SPRINT_30.md`, `docs/27_ERP_INTEGRATION_HUB.md` en `modules/erp-integration-hub/README.md`. Fundamentele contracts voor connector scopes en events ontwerpen vanaf Sprints 00/03/04/23; daadwerkelijke ontwikkeling na ledger, finance workflows, connectorframework en beveiliging (minimaal 01,03,04,08,12,14,23). Verlaag nooit de release-gates van eerdere sprints.

| Sprint | Scope | Exit-gate |
|---|---|---|
| 30 | ERP Integration Hub: connectorcapabilities, per-proces eigenaarschap, ERP → Finance events, beperkte statusterugkoppeling, financial posting mode, reconciliatie, beheer UI | Geen dubbele journaalposten, ERP-voorraad blijft operationeel leidend, privacy/IAM/SoD, idempotent replay, NL/EN en 1-op-1 documentatie |

Borg dat `financial_posting_mode` onderscheid maakt tussen import van ERP-journalen en financiële journalisering door AI Finance vanuit ERP-brondocumenten. ERP-integratie en 60-daagse legacyproefperiode zijn gescheiden productfuncties; bij overlap formeel mapping-/source-of-truth-/cutoverplan. Bestaande pilot- en security-gates blijven gelden; Sprint 30 is geen productie-autorisatie.

## v0.9 — gekozen SaaS-model en nieuwe specialistische sprints
**Besluit:** tenant-subdomeinen zijn vanaf Sprint 00/01 core architecture; Sprint 31 is de formele security- en operational-readiness-gate voor onboarding op `bedrijf.<platformdomein>`. De eerdere sprints 00–30 blijven bestaan en krijgen die randvoorwaarde.

| Sprint | Onderdeel | Afhankelijkheden | Exit-gate |
|---|---|---|---|
| 31 | Tenant provisioning, unieke platform-subdomeinen en SaaS isolatie | 00, 01, 02 | Domain route spoofing, cross-tenant data/AI/export en failed-provisioning negatieve tests slagen |
| 32 | IT's Perfect API v2 referentieconnector / quotabeheer | 30, 31 | Gevalideerde capabilities, rate-limit/queue en finance reconciliation met synthetische fixtures |
| 33 | Gepersonaliseerde Demo Data & Sandbox (verder uitwerken) | 02, 03, 04, 31 | Specificatie na aanvullend klantbesluit; géén productie-egress, deterministische accounting fixtures |

De volgorde in de tabel is een planning voor specifieke gates, niet een excuus om tenant boundaries pas bij Sprint 31 te introduceren. Kritieke basis wordt vanaf Sprint 00 gebouwd; geen echte tenantprovisioning live vóór Sprint 31 security gate.

# Sprint 00 — Fundering en security-first scaffold

## Opdracht voor Claude
Lees de complete repository inclusief `CLAUDE.md`, `README.md`, docs 01–10. Bouw alleen de technische fundering voor een multi-tenant AI-first boekhoudplatform. Gebruik uitsluitend synthetische demo-gegevens. Stop na werkende lokale tests en expliciete GO/NO-GO-status. **Implementeer nog geen echte klantdataverwerking, model-API of betaling.**

## Werkpakket
1. Stel voorlopige stack voor conform ADR-001; leg exacte keuzes vast met alternatieven en redenen.
2. Maak werkende monorepo: frontend web shell, backend health endpoints, shared domain/types, unit/integration test harness, lint, format, typecheck, CI.
3. Leg tenant context + user identity contracts vast en bouw deny-by-default auth guards met tests (fake auth provider voorlopig; duidelijk test-only).
4. Defineer typed domeincontracten voor Money, TenantId, LegalEntityId, LedgerAdministrationId, BusinessUnitId, ConsolidationGroupId, ModuleId, ProfileTemplateId, DocumentRef, ActionProposal, ApprovalSnapshot, DisclosureManifest, AuditEvent zonder schijn van voltooide implementatie.
5. Voeg geautomatiseerde baseline security checks toe: secret scan, dependencies, branch/test checks.
6. Maak `docs/IMPLEMENTATION_LOG.md` en `docs/THREAT_MODEL.md`; werk basisrisico's en trust boundaries uit.
7. Maak synthetische fixtures voor 2 tenants A/B: tenant A heeft Retail en Wholesale als afzonderlijke juridische entiteiten, plus een alternatief voorbeeld waarin zij business units binnen één juridische entiteit zijn, en één optioneel dimensiepad E-commerce -> Collectie en negatieve tests die buiten-tenant toegang afwijzen.
8. Maak CI-commando `test:all` dat typecheck, lint en tests uitvoert.
9. Toon aan dat alle AI-integratie uit staat (`EXTERNAL_AI_ENABLED=false`) en er geen externe AI-egress bestaat.

## Acceptance
- Lokale startup-instructies werken.
- Test suite groen en expliciete cross-tenant denial fixture.
- Geen secrets, geen echte klantdata, geen bank/API live integrations.
- Privacy/dataflows in docs blijven consistent.
- Rapport aan gebruiker met bestanden, testoutput, beperkingen en volgende sprint. Vraag GO voor Sprint 01.

## Starttekst voor Claude
`Lees CLAUDE.md en de volledige projectdocumentatie. Voer nu uitsluitend claude/tasks/SPRINT_00.md uit. Documenteer aannames, schrijf tests en rapporteer de resultaten. Start geen volgende sprint zonder mijn toestemming.`

## Aanvulling v0.2
Leg alvast het onderscheid vast tussen profieltemplate, tenantmoduleconfiguratie, AI-databeleid en user-rechten. Alleen de contracten/type-definities; bouw geen pseudo-consolidatie in Sprint 00.


## v0.5 — verplichte team-, handleiding- en synchronisatiegate
- Orkestreer architect/developer/onafhankelijke reviewer/QA/finance+privacy/security-red-team/documentation/release gate volgens `docs/14_MULTI_AGENT_DEVELOPMENT_TEAM.md` (parallel als ondersteund, anders gescheiden sequentiële reviews met eerlijke vermelding).
- Red-team probeert uitsluitend **lokale synthetische testdata** te benaderen via geautoriseerde negatieve tests. Leg findings, fixes en hertesten vast; geen productie of echte klanten.
- Werk bij iedere zichtbare of functionele verandering `docs/16_GEBRUIKERSHANDLEIDING.md` en zo nodig `docs/17_BEHEER_EN_OPERATIONS_HANDLEIDING.md` bij. Alleen geteste functies krijgen status GEIMPLEMENTEERD/GEVALIDEERD.
- Bouw na Markdown-wijzigingen leesbare docs met `python scripts/build_readable.py`; vereis `python scripts/build_readable.py --check` groen. Lever source→render checksumrapport en code→test→handleiding traceability aan.
- NO-GO bij documentation drift, ontbrekende handleiding, scope escape of kritieke open securityfinding.

> **v0.6 module-gate:** Lees `modules/INDEX.md` en bijbehorende modulespecificaties; registreer nieuwe wensen in die module vóór code. Houd NL/EN handleidingen en 1-op-1 HTML/PDF synchroon. Nieuwe functionaliteit volgt de afzonderlijke sprints 22–28; implementeer die niet per ongeluk in deze sprint.

## v0.9 — verplicht vroeg contract voor platform-subdomeinen
Voorzie vanaf scaffold `tenant_id`, `tenant_domain` (unieke slug), tenantmembership en route/authz-protocol. Geen DNS/TLS infrastructuur live maken binnen Sprint 00. Voeg synthetische tests toe voor unknown host, header spoofing en tenant data-scope denial. Lees `docs/28_SAAS_TENANT_SUBDOMAINS.md` vóór verdere API/UI ontwikkeling. Speciale E2E gate in Sprint 31.

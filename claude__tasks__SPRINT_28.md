# Sprint 28 — Chat tour discovery and module governance

**Status:** GEPLAND — start alleen na expliciete GO en toepasselijke afhankelijkheden.

## Lees eerst
`CLAUDE.md`, `docs/24_AI_CHAT_GUIDED_LEARNING.md`, `docs/25_MODULE_SPEC_GOVERNANCE.md`, `modules/INDEX.md`, relevante module README's, `docs/02_ARCHITECTURE.md`, `docs/04_DATA_GOVERNANCE_SECURITY.md`, `docs/14_MULTI_AGENT_DEVELOPMENT_TEAM.md`, beide handleidingen.

## Deliverables
authorized published tour retrieval, chat start card, fallback search, module routing policies.

## Teamworkflow
Architect → relevante module-owner → implementerende developer(s) → onafhankelijke code review → QA + finance/privacy → security red-team in **synthetische testomgeving** → correctie/retest → NL/EN handleidingen → Markdown→HTML/PDF → release gate.

## Minimum tests
Tenant/entiteit scope; ABAC/SoD; fail-closed bij afwijking; geen autonome AI-commit/approval; i18n; getypeerde interfaces; regressie; audit; documentensynchronisatie. Test bijzondere risico's uit het gelezen domeindocument.

## Done
Alle acceptatiecriteria in de module-README en `docs/24_AI_CHAT_GUIDED_LEARNING.md` zijn gedekt met reproduceerbaar bewijs, gebruikershandleiding is eerlijk geactualiseerd, `python scripts/build_readable.py --check` is PASS, onafhankelijke reviewer heeft expliciet getekend. Stop en vraag GO voor volgende sprint.

## Extra acceptatievoorwaarden — chat en licentie
- Alleen gelicentieerde tenants zien startbare Guided Learning-kaarten in chat; niet-gelicentieerd blijft normale teksthulp behouden zonder verborgen tourinhoud. Controleer entitlement vóór retrieval en bij starten.

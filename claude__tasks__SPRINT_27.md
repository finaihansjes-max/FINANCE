# Sprint 27 — AI Guided Learning authoring

**Status:** GEPLAND — start alleen na expliciete GO en toepasselijke afhankelijkheden.

## Lees eerst
`CLAUDE.md`, `docs/23_AI_GUIDED_LEARNING_BUILDER.md`, `docs/25_MODULE_SPEC_GOVERNANCE.md`, `modules/INDEX.md`, relevante module README's, `docs/02_ARCHITECTURE.md`, `docs/04_DATA_GOVERNANCE_SECURITY.md`, `docs/14_MULTI_AGENT_DEVELOPMENT_TEAM.md`, beide handleidingen.

## Deliverables
structured proposal, schema validation, security and finance reviews, publish workflow.

## Teamworkflow
Architect → relevante module-owner → implementerende developer(s) → onafhankelijke code review → QA + finance/privacy → security red-team in **synthetische testomgeving** → correctie/retest → NL/EN handleidingen → Markdown→HTML/PDF → release gate.

## Minimum tests
Tenant/entiteit scope; ABAC/SoD; fail-closed bij afwijking; geen autonome AI-commit/approval; i18n; getypeerde interfaces; regressie; audit; documentensynchronisatie. Test bijzondere risico's uit het gelezen domeindocument.

## Done
Alle acceptatiecriteria in de module-README en `docs/23_AI_GUIDED_LEARNING_BUILDER.md` zijn gedekt met reproduceerbaar bewijs, gebruikershandleiding is eerlijk geactualiseerd, `python scripts/build_readable.py --check` is PASS, onafhankelijke reviewer heeft expliciet getekend. Stop en vraag GO voor volgende sprint.

## Extra acceptatievoorwaarden — commerciële afhankelijkheid
- AI Builder alleen indien Guided Learning betaald en actief, én AI-verwerkingsopt-in separaat geldig; anders faalt generatie veilig. Test server-side bypass en revoked/expired entitlements.

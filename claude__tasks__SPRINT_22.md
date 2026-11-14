# Sprint 22 — NL/EN foundation

**Status:** GEPLAND — start alleen na expliciete GO en toepasselijke afhankelijkheden.

## Lees eerst
`CLAUDE.md`, `docs/18_I18N_LOCALIZATION.md`, `docs/25_MODULE_SPEC_GOVERNANCE.md`, `modules/INDEX.md`, relevante module README's, `docs/02_ARCHITECTURE.md`, `docs/04_DATA_GOVERNANCE_SECURITY.md`, `docs/14_MULTI_AGENT_DEVELOPMENT_TEAM.md`, beide handleidingen.

## Deliverables
i18n catalog, locale provider, translations, both manuals and tests.

## Teamworkflow
Architect → relevante module-owner → implementerende developer(s) → onafhankelijke code review → QA + finance/privacy → security red-team in **synthetische testomgeving** → correctie/retest → NL/EN handleidingen → Markdown→HTML/PDF → release gate.

## Minimum tests
Tenant/entiteit scope; ABAC/SoD; fail-closed bij afwijking; geen autonome AI-commit/approval; i18n; getypeerde interfaces; regressie; audit; documentensynchronisatie. Test bijzondere risico's uit het gelezen domeindocument.

## Done
Alle acceptatiecriteria in de module-README en `docs/18_I18N_LOCALIZATION.md` zijn gedekt met reproduceerbaar bewijs, gebruikershandleiding is eerlijk geactualiseerd, `python scripts/build_readable.py --check` is PASS, onafhankelijke reviewer heeft expliciet getekend. Stop en vraag GO voor volgende sprint.

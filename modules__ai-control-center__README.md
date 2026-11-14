# Module: AI Control Center

**Status:** GEPLAND  
**Type:** opt-in  
**Module ID:** `ai-control-center`

## Doel en scope
Risico- en kwaliteitsmonitoring.

## Vastgelegde producteisen
- Functioneert binnen tenant-, entiteits- en administratiescope en respecteert RBAC/ABAC en functiescheiding.
- Wijzigingen zijn controleerbaar; Excel-export voor dataweergaven met dezelfde bevoegdheden en filters.
- Taal: Nederlands en Engels; handleiding en Guided Learning refereren aan actuele status.
- AI verwerkt geen data zonder apart geconfigureerde bevoegdheid, inzicht in provider/velden/regio/retentie en beveiligde gateway.
- Moduleconfiguratie bij onboarding via bedrijfsprofiel en later aanpasbaar; afhankelijkheden en wettelijke kerneisen afdwingen.

## Nieuwe wensen / backlog
<!-- Claude: plaats toekomstige wensen hier met datum, exacte intentie, prioriteit, status en acceptatiecriteria. -->
- Nog geen extra wensen; toekomstige wijzigingen beginnen hier, vóór implementatie.

## Gebruikers en rechten
- Inventariseer `view`, `create`, `edit`, `approve`, `send`, `export` en overige relevante actions. Geen rechten impliciet van schermzichtbaarheid afleiden.

## Data, gebeurtenissen en integraties
- Beschrijf brongegevens, output, events, historie en afhankelijkheden vóór bouw. Geen directe SQL-toegang voor AI.

## Kritieke financiële controles
- Bepaal risk-based approval, SoD, versie-gebonden vrijgave en server-side checks; afwijkingen fail-closed.

## AI en privacy
- Welke velden nodig? Doel? Opt-in? Interne of externe verwerking? Bewaartermijn? Externe verwerker? Logging en rechten?

## Interface, Excel, NL/EN en Guided Learning
- Definieer schermroutes, stabiele `data-guide-id` selectors, exportgedrag, taalstrings en template-compatibiliteit.

## Acceptatiecriteria en testscenario's
- Functionele positieve/negatieve tests, cross-tenant, rechten, finance-integriteit, AI-/provideruitval en docs-sync.

## Afhankelijkheden, besluiten en documentatie
- Relevante links: `docs/01_PRODUCT_REQUIREMENTS.md`, `docs/02_ARCHITECTURE.md`, `docs/06_ROADMAP.md`, `docs/25_MODULE_SPEC_GOVERNANCE.md`.
- Handleiding/status en sprintplanning bij elke wijziging bijwerken.

# Module: Interface-templates

**Status:** GEPLAND  
**Type:** opt-in  
**Module ID:** `interface-templates`

## Doel en scope
Presentation layouts en vertrouwde workflows.

## Vastgelegde producteisen
- Functioneert binnen tenant-, entiteits- en administratiescope en respecteert RBAC/ABAC en functiescheiding.
- Wijzigingen zijn controleerbaar; Excel-export voor dataweergaven met dezelfde bevoegdheden en filters.
- Taal: Nederlands en Engels; handleiding en Guided Learning refereren aan actuele status.
- AI verwerkt geen data zonder apart geconfigureerde bevoegdheid, inzicht in provider/velden/regio/retentie en beveiligde gateway.
- Moduleconfiguratie bij onboarding via bedrijfsprofiel en later aanpasbaar; afhankelijkheden en wettelijke kerneisen afdwingen.

## Specifieke harde eisen
- Exact-georiënteerde ervaring zonder ongeautoriseerde exacte kopie; presentatielaag, nooit ledgersemantiek aanpassen.

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

## v0.7 — Concrete UI-template-eisen
- Maak onderscheid tussen `experience_template_id` en `navigation_placement` (links of boven), én met `business_profile`/module-opt-in/locale.
- Template `AI Finance Default` gebruikt design tokens en componenten uit `modules/design-system/README.md` en `docs/26_DESIGN_SYSTEM_AND_NAVIGATION.md`.
- Ondersteun herkenbare, juridisch veilige compatibiliteitspresets voor bekende pakketten via versiebeheerbare declaratieve mappings; geen externe code/CSS met onbeperkte bevoegdheid.
- Gedeelde `route_id`, `action_id`, `data-guide-id` per workflow; compile + E2E check bij publiceren van template. Alle templates tonen dezelfde beperkte data, bevoegdheden en Excel-export.

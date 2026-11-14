# Module: Design System & Experience Settings

**Status:** GEPLAND  
**Type:** verplichte gedeelde UI-infrastructuur (niet uit te schakelen als veiligheids-/usabilityfundament)  
**Module ID:** `design-system`  
**Canonieke specificatie:** `docs/26_DESIGN_SYSTEM_AND_NAVIGATION.md`  
**Referentie:** `assets/design/ai_finance_standard_dashboard_reference.png` (conceptafbeelding van opdrachtgever)

## Eisen
- Modern licht AI Finance-standaarddesign in lijn met aangeleverde visuele referentie; **geen** demo-waarden of conceptbranding hardcoden.
- Standaard verticale linkernavigatie; per gebruiker horizontale topnavigatie instelbaar; responsive en toegankelijk.
- Eén route/action registry en gedeelde tokens/designcomponenten; module-uitbreidingen gebruiken dezelfde stijlgids.
- Onderscheid bedrijfsprofiel, module-opt-in, experience-template, navigatiepositie en taalvoorkeur.
- Beide navigatievarianten en templates respecteren identieke server-side rights, tenant-/entiteit-/administratiescope en Excel-exports.
- Alle relevante componenten registreren stabiele `route_id`, `action_id` en `data-guide-id` voor tests en Guided Learning.
- Geen echte financiële informatie in template previews, analytics, externe AI of testfixtures zonder passende autorisatie; default synthetische data.

## Nieuwe wensen / backlog
- 2026-11-14: Aangeleverd dashboard als standaard visuele referentie; instelbare linker/bovenste navigatie; herkenbare boekhoudwerkomgevingen (Exact-georiënteerd enz.). Status: VASTGELEGD IN ONTWERP, nog niet geïmplementeerd.

## Afhankelijkheden
`modules/interface-templates/README.md`, `modules/guided-learning/README.md`, `modules/localization/README.md`, `docs/02_ARCHITECTURE.md`, `docs/07_TEST_AND_ACCEPTANCE.md`, `docs/25_MODULE_SPEC_GOVERNANCE.md`.

## Acceptatie / releasegate
Automatische UI-regressie per navigatie × template × NL/EN; WCAG 2.2 AA als toetsdoel; deep-link/API rechten; consolidatie read-only; Excel data equivalent; tours-selector validatie; reviewer + handleidingupdate + sync-check. Geen productclaim zonder aantoonbaar bewijs.

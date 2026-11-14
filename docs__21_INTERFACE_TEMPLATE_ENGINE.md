# 21 — Interface- en workflowtemplates

## Status en scope
**GEPLAND**. Dit document beschrijft eisen en acceptatievoorwaarden, niet een reeds werkende implementatie. Bij iedere wijziging: privacy-impact, rechten, finance-controls, geautomatiseerde tests, handleidingen NL/EN, document-rendering en reviewersignoff bijwerken. Alle voorbeelden gebruiken synthetische testdata.

## Scheiding
Bedrijfsprofiel bepaalt standaard opt-in modules; **werkruimte-template** bepaalt alleen presentatie en volgorde. Geen template wijzigt financiële semantiek, policy, rechten, SoD of ledger-posting. Een ZZP-klant kan een Exact-georiënteerde werkomgeving plus uitgebreide rapportages gebruiken.

## Configureerbare presentation schema
- Navigatie, labels, dashboardwidgets, tabellen, kolommen, sortering, filters, sneltoetsen, schermlayout en procesbegeleiding per gebruiker/tenant instelbaar met declaratieve componentregistratie en versiebeheer.
- Stabiele `data-guide-id` op interactieve elementen; template-specifieke selector-mapping; route-ID's en businessactie-ID's zijn template-onafhankelijk.
- Templates door leverancier beheerd met preview, compatibiliteitscheck, publish/rollback; tenant- en gebruikeroverschrijvingen apart.
- Compatibiliteit met bekende pakketten betekent vertrouwde terminologie en werkpatronen, **geen niet-geautoriseerde één-op-één kopie** van beschermde UI, logo's of assets. Claims 'precies hetzelfde' pas na juridische en functionele verificatie.
- Regressietests vergelijken dezelfde taak in alle templates, beide talen en verschillende rollen.

## Acceptatie
- Template wisselen veroorzaakt geen boeking, dataverlies of autorisatiewijziging.
- Guided Learning selectors en toetsen functioneren in iedere ondersteunde templateversie.

## v0.7 — Canonieke user experience contracten
Zie `docs/26_DESIGN_SYSTEM_AND_NAVIGATION.md` en `modules/design-system/README.md`. **AI Finance Default** wordt de herkenbare huisstijl conform aangeleverd conceptdashboard. Navigatie kan onafhankelijk van het businessprofiel worden geplaatst in `left_vertical` (standaard) of `top_horizontal`; deze keuze is per gebruiker configureerbaar en beschikbaar in NL/EN. Andere boekhoudtemplates blijven optioneel en delen dezelfde ledger-, RBAC-, ABAC-, goedkeurings- en exportsystemen. Interface-compatibiliteit vereist een geteste taakmatrix en respect voor rechten van derden. Alle varianten behouden stabiele semantic action IDs en Guided Learning selectorcontracten. UI-preview en template-switch zijn financieel side-effectvrij.

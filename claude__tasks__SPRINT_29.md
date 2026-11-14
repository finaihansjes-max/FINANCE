# Sprint 29 — AI Finance standaarddesign en flexibele navigatie

**STATUS: GEPLAND. Start alleen na expliciete GO en vervulde afhankelijkheden.** Design-contracten moeten al vanaf Sprint 00/10 worden meegenomen; deze sprint finaliseert implementatie en hardening met Sprint 25/26 als afhankelijkheden. Herplan alleen via gedocumenteerde ADR.

## Lees eerst
`CLAUDE.md`, `docs/26_DESIGN_SYSTEM_AND_NAVIGATION.md`, `docs/21_INTERFACE_TEMPLATE_ENGINE.md`, `docs/22_GUIDED_LEARNING_PLUGIN.md`, `docs/18_I18N_LOCALIZATION.md`, `modules/design-system/README.md`, `modules/interface-templates/README.md`, `docs/04_DATA_GOVERNANCE_SECURITY.md`, beide handleidingen en `assets/design/ai_finance_standard_dashboard_reference.png`.

## Taken
1. Formele style tokens maken naar aangeleverde lichte dashboardreferentie en semantisch/accessible component library implementeren (zonder kopie van demo-data/assets).
2. Gedeelde `AppShell` + declaratief `navigation_registry` maken; twee renderers `left_vertical` en `top_horizontal`; persistente per-user preferences, veilige fallback/default, preview/reset.
3. Dashboardlayouts, overzichtskarten, grafiek, transactietabel, AI-paneel en context voor entiteit/administratie/consolidatie responsive opbouwen op getypeerde mock-API met synthetische data.
4. Integreren met experience-template schema, NL/EN, route/action/guide IDs, de module-opt-ins, schermrechten en Excel-export.
5. Template-georiënteerde UX compatibiliteitsmatrix en juridisch/merkenreview; geen exacte imitatie of niet-toegelaten assets.
6. Onafhankelijke review + QA + accessibility + adversarial tests op navigatie, deep-links, export, tenant-/entiteitsautorisatie, selector-contracten en read-only consolidatie.
7. Werk beide handleidingen bij in NL/EN, render leesbare HTML/PDF, voer sync-check uit en rapporteer bewijs.

## Minimaal bewijs
- Screenshot/visual regression Desktop/Tablet/Mobiel en contrastcheck; tests voor links↔boven, gebruiker A/B scheiding, role scopes, onbevoegde deep links, NL/EN, dashboard export.
- Elke gepubliceerde tour kan relevante componenten in beide nav-indelingen vinden of faalt veilig. Geen live AI tijdens runtime.
- Switchen veroorzaakt geen extra financiële boekingen, geen toegangsuitbreiding, geen wijziging in AI-verwerkingsconsent.

## Team/gate
Architect, UI developer, frontend en backend, onafhankelijke reviewer, QA, finance/UX reviewer, privacy en security red-team (alleen synthetische omgeving), documentation owner, release gatekeeper. Geen doorgang naar volgende sprint zonder GO.

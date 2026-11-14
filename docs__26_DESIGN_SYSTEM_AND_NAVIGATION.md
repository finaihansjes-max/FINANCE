# 26 — Design System, navigatie-indelingen en herkenbare boekhoudwerkomgevingen

**STATUS: GEPLAND — functioneel/visueel ontwerp, geen reeds gebouwde applicatie.** Visuele referentie: `assets/design/ai_finance_standard_dashboard_reference.png` (door opdrachtgever aangeleverde conceptafbeelding; fictieve KPI's/namen/gegevens en zichtbare schetsmarkeringen zijn geen productvereisten). De afbeelding dient uitsluitend als **interne ontwerpreferentie**.

## Ononderhandelbare ontwerpbeslissing
AI Finance krijgt standaard een eigen modern, licht dashboard met veel wit/lichtgrijs, subtiele randen, afgeronde kaarten, paars/indigo-accenten, duidelijke typografie, KPI-tegels, kasstroomgrafiek, transactietabel, zoekveld, notificaties en compact AI-assistentpaneel. De visuele **stijl** is leidend, maar teksten, cijfers, demo-merken, fictieve datums, grafiekwaarden en conceptschermtitels uit de referentie mogen niet hardcoded worden.

De standaardweergave heeft een verticale linkernavigatie. Elke gebruiker kan overschakelen naar **horizontale topnavigatie**. Beide navigatievarianten bevatten dezelfde, op rechten gefilterde functies. Het ontwerp werkt responsief op desktop, tablet en mobiel, met geschikte toetsenbordbediening, zichtbare focus, WCAG 2.2 AA-doel en voldoende contrast. Tests moeten contrast meten; de referentiekleuren zijn niet automatisch compliant.

## Drie onafhankelijk configureerbare configuratielagen
1. `business_profile_id`: profiel ZZP/klein MKB/middelgroot MKB en module-defaults — uitsluitend onboardingvoorstel, daarna aanpasbaar.
2. `experience_template_id`: presentatiepreset (AI Finance Default, Exact-georiënteerd, andere compatibele presets, Custom). Presets mogen navigatie, termen, formulierindelingen, standaardkolommen, dashboardwidgets, filters en presentatievolgorde aanpassen, niet het daadwerkelijke autorisatie-, goedkeurings- of boekingsbeleid.
3. `navigation_placement`: `left_vertical` of `top_horizontal`, standaard `left_vertical`; per gebruiker instelbaar en bewaard, tenzij een expliciete bedrijfsbeleidsoverride geldt. De voorkeur mag niet ongemerkt naar andere gebruikers doorsijpelen. Gebruik configversies en toegestane combinaties met de actieve experience-template.

Taalvoorkeur `locale` (minimaal `nl-NL` en `en`) en optionele accessibility/user-density-instellingen zijn onafhankelijke vierde/vijfde voorkeuren; geen koppeling aan bedrijfsprofiel.

## Componentarchitectuur
- `design_tokens`: kleuren, typografie, spacing, radius, borders, elevations, iconografie, grafiekpalet, contrastvarianten en states. Eigen design-identiteit: geen gekopieerde beschermde merkassets.
- `app_shell`: header, workspace/tenant-/administratiekiezers, nav slot (`left` of `top`), breadcrumbs, zoekbalk, notifications, user menu, chatbot-zijpaneel, contextacties.
- `navigation_registry`: één semantisch menu- en route-register met stabiele `route_id`, `action_id`, `data-guide-id`; renderers voor beide posities. Nooit afzonderlijke routebevoegdheden hardcoden in de navigatievarianten.
- `widget_registry`: dashboardcards, transactieoverzichten en grafieken op rechten/administratiescope, perioden en dataherkomst, inclusief altijd beschikbare Excel-export voor onderliggende datasets.
- `template_schema`: versie, ondersteunde modules/routes, terminologiemapping, navigatiegroepen, kolomsets, layout-slots, toegestane workflowschermvarianten en `guided_learning_selector_map`.
- `experience_resolver`: merge prioriteit platform safety defaults -> gepubliceerde template -> tenant presets -> gebruiker voorkeur, met expliciete constraints en migratie bij template-updates.
- `admin_template_editor`: preview/draft/review/publish/rollback, provenance, compatibiliteit, geen willekeurige JS/CSS-executie uit tenants of door AI. Tenantcustomisatie is declaratief en gesandboxed.

## Interface-presets van bekende boekhoudpakketten
Exact-, AFAS-, Twinfield- of Moneybird-georiënteerde presets zijn een **compatibiliteitsdoel**, geen claim dat een 1-op-1 reproductie bestaat of zonder juridische beoordeling mag worden gekopieerd. Maak per doelgroep een *workflow compatibility matrix* op basis van toegestane research, documenteer herkenbare taken, route-, label- en sneltoetsmapping. Geen andermans logo's, lettertypen, artwork of beschermde exacte interfacekopieën zonder benodigde toestemming/licentie. Maak geen prestatieclaim zoals '1000 keer beter' zonder meetbare criteria.

## Scope en data-afscherming
- Wisseling van layout/template veroorzaakt **geen** financiële mutatie en verandert nooit `tenant_id`, `administration_id`, consolidatiescope, RBAC/ABAC, SoD, approval-policy of AI-egress toestemming.
- App-shell toont duidelijk of gebruiker in boekbare administratie of read-only geconsolideerd overzicht zit. Reguliere posten onmogelijk in de geconsolideerde view, ook via API/keyboard/selector.
- UI-componenten **en** server bewaken rights/row/field filters; het verbergen van een menu is geen autorisatie.
- Geen echte financiële inhoud naar template-provider of designtelemetrie. Analytics privacy-by-design en naar externe AI geen datastroom tenzij expliciet toegestaan.
- Excel-export per dataview blijft invariant bij layoutwissel. Layoutconfiguratie in export enkel als metadata, nooit om geblokkeerde gegevens te onthullen.

## Guided Learning en schermwijzigingen
Beide navigatie-indelingen en alle gepubliceerde templates mappen naar dezelfde semantische `action_id`; ieder relevant element draagt stabiel `data-guide-id`. De uitvoerende walkthrough-runtime is deterministisch en heeft geen AI nodig. Vóór het publiceren van een nieuwe templateversie: compile-time compatibiliteitscheck + E2E-test voor selector resolve, highlight, klik, route, echte workflowuitkomst, locale en rol. Bij niet resolveerbare selector: veilige stop en herstelbericht, niet automatisch op vermoedelijke knoppen klikken. AI kan **vooraf** concepttours bouwen; de chat kan gepubliceerde tours vinden en aanbieden; live AI-besturing is verboden.

## Instellingen, gebruikersflow en NL/EN
- `Instellingen > Weergave`: Werkomgeving kiezen, Voorbeeld tonen, Navigatiepositie Links/Boven, taalkeuze en reset naar standaard.
- Een preview is read-only en heeft geen side effects. Bewaren via aparte getypeerde preference-command; preview toont mogelijke incompatibiliteiten.
- Nieuwe medewerkers krijgen standaard `AI Finance Default` + verticale navigatie. Een tenantadmin kan een tenant-default instellen zonder bestaande persoonlijke keuzes te overschrijven, behalve via expliciet geautoriseerd beleid.
- Beheerinterface toont toegestane presets, compatibele modules, status, beperkingen en versies.

## Acceptatiecriteria / traceerbare tests
1. Schakel voor dezelfde gebruiker tussen links en boven; alle geautoriseerde routes, acties en data blijven gelijk; geen mutatie in grootboek/audit behalve wijziging gebruikersvoorkeur.
2. Gebruiker A kiest `top_horizontal`, gebruiker B behoudt `left_vertical`; tenant- en administratiescheiding werkt.
3. Exact-georiënteerde preset verandert alleen weergave/bediening; dezelfde PO/factuur/approval/posting volgt identieke beleidsmotor en audit.
4. Bedien elke workflow met toetsenbord en screenreader; responsive layout is bruikbaar, geen dataverlies bij breakpointwissel.
5. Run Guided Learning per taal × navpositie × template × toestemming; selectors kloppen of stoppen expliciet; geen AI-call bij runtime.
6. Frontend navigatie mag verboden menu's niet tonen; directe deep-link en API/export buiten scope leveren server-side denied.
7. Read-only consolidatie blijft read-only voor alle templates; eliminaties uitsluitend aparte gecontroleerde werkruimte.
8. Documenteer gebruikte tokens/afwijkingen t.o.v. screenshot; kleurcontrast en visuele regressie-tests in CI.
9. Update gebruikers- en beheerdershandleiding in NL/EN; genereer 1-op-1 HTML/PDF en hash-check PASS.

## Sprintafhankelijkheden
De basiscomponentcontracten zijn **nu al** onderdeel van Sprint 00/01/10. Implementatie van design system en navigatie vóór of samen met Sprint 25 (template engine), expliciete afronding in Sprint 29. Guided Learning Sprints 26–28 gebruiken dezelfde `route_id`, `action_id` en `data-guide-id`. Een andere planning vereist expliciet ADR en bijgewerkte tests; nooit bouwen van de verkeerde architectuur om de sprintnummering te volgen.

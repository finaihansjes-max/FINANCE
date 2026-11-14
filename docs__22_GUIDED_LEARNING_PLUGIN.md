# 22 — Guided Learning Plugin (zonder AI-runtime)

## Status en scope
**GEPLAND**. Dit document beschrijft eisen en acceptatievoorwaarden, niet een reeds werkende implementatie. Bij iedere wijziging: privacy-impact, rechten, finance-controls, geautomatiseerde tests, handleidingen NL/EN, document-rendering en reviewersignoff bijwerken. Alle voorbeelden gebruiken synthetische testdata.

## Kernprincipe
Guided Learning is een optionele plugin voor interactieve **echte** processen. De runtime is volledig deterministisch en werkt ook wanneer AI is uitgeschakeld of onbereikbaar is. Dezelfde module ondersteunt handmatig gebouwde en door AI vooraf voorgestelde tours.

## Builder
- No-code editor voor eigenaren/beheerders: selecteer doel-element visueel, bewaar `data-guide-id`/CSS selector fallback, schrijf NL/EN tekst, kies overlay/markering, invoer, klik, wachtconditie, schermnavigatie, skip/back/stop en guard.
- Tour schema: tour-ID, versie, tenant/scope, module, rol, taal, template, route, stappen, selectors, verwacht event, serververificatie, risicoclassificatie, auteur, reviewer, status draft/tested/published/retired.
- Preview en sandbox-test met synthetische administratie; selector-vervanging/test na elke UI-template release.

## Runtime
- Browser-plugin binnen eigen app met highlight, dimmer, popover, toetsenbord-/screenreaderondersteuning; bescherm focus, modals, overlays en responsive UI.
- Engine **klikt niet namens de gebruiker** bij gevoelige handelingen; toont aanwijzingen en wacht op echte gebruikersacties.
- Verifieer iedere stap via toegestane API-state/event, niet alleen DOM-verandering; echte boeking/aflettering kan ontstaan onder bestaande rechten, SoD, approvals.
- Onderbreking, hervatten, retries en niet-beschikbare selectors fail-safe; geen betaling uitvoeren door tourautomatisering.

## Acceptatie
- Geen model/API-aanroep tijdens uitvoering; kun je offline van AI-provider gebruiken.
- Echte werkstroom alleen met authentieke autorisatie; training administratie apart.
- Automatische tests met gewijzigde routes/selectors, template-/taalwissel en rolrestricties.

## v0.7 — Navigatie-/template-compatibiliteit
Guided Learning moet op basis van dezelfde semantische `route_id` en `action_id` werken onder linker- en topnavigatie en onder iedere ondersteunde experience-template. Elke relevante interactieve control gebruikt een persistent `data-guide-id`, met declaratieve mapping waar andere layout anders positioneert. Bij ongeldige selector of andere niet-geverifieerde interactie: stoppen en veilige melding; geen AI-gegenereerde live stappen, geen clicks op vermoede doelknoppen. Zie `docs/26_DESIGN_SYSTEM_AND_NAVIGATION.md`.

## Betaalde add-on / entitlement gate (harde eis)
- Dit is **een betaalde, optionele add-on**, standaard uit voor ieder profiel. Geen enkele klant krijgt automatisch Guided Learning door het afnemen van het basisboekhoudpakket.
- Sla afzonderlijk vast: `LicenseEntitlement` (tenant, sku, status, start/end, features, audit), `TenantModuleSelection` (enabled), `UserPermission` en optionele `AIProcessingAuthorization`; controleer ze server-side bij elk tour-/builder-/player-API-verzoek en elke achtergrondtaak.
- Toon prijs, contractstatus en activering helder tijdens onboarding en bij `Instellingen > Modules`. Als licentie niet actief is: geen tours weergeven/starten of beheren; bied in NL/EN een duidelijke informatie-/upgrade-route, niet automatisch kopen.
- Bij afloop, opzegging of betalingsstatuswijziging: veilige blokkade van nieuwe acties, eventueel lopende tours stoppen op veilige grens; bewaar configuraties met vastgelegd bewaarbeleid, behoud audit/financiële transacties. Heractivering na expliciete licentieherstelactie.
- Tests: nieuwe ZZP/MKB zonder add-on, actieve betaalde klant, license-expired, revoked, entitlement race, direct API/URL, AI-chat-start, cross-tenant, moduleswitch, user-rechten en AI uitgeschakeld. Geen basisfuncties zoals factureren/goedkeuren mogen van deze add-on afhankelijk zijn.

# 11 — Modules, profieltemplates en onboarding

## Doel en terminologie
- **Profieltemplate** = vooraf geselecteerde standaardmodules en aanbevelingen. Geen autorisatie, geen bundelbeperking, geen toestemming voor externe AI.
- **Module** = capability waarvan UI, API, jobs, AI-agenten en rapportages gezamenlijk geactiveerd/gedeactiveerd worden onder een lifecycle.
- **Rechten** = per gebruiker, entiteit, administratie en object; onafhankelijk van actief zijn van module.
- **AI processing authorization** = apart beleid voor datacategorie, doel, provider, verwerkingsregio, retentie en actor. Wettelijke AVG-grondslag moet apart worden beoordeeld.

## Startprofielen (voorbeelden, niet juridisch of commercieel bindend)
| Profiel | Voorstel standaard aan | Direct optioneel toe te voegen |
|---|---|---|
| ZZP / zelfstandig ondernemer | Inkoop, verkoop, bankadministratie, btw-voorbereiding, eenvoudige rapportage | **Uitgebreide rapportages**, budgetbeheer, voorraad, projecten, AI Control Center, consolidatie indien passend |
| Klein MKB | ZZP-defaults + debiteurenbeheer, gebruikers/rollen, uitgebreidere rapportage | Goedkeuringen, budgetten, vaste activa, consolidatie, AI-specialisten |
| Middelgroot MKB | Financiële modules + workflow configuratie, budgetten, multi-entity management | Geavanceerde consolidatie, intercompany, aanvullende AI-controles |
| Aanbieder-template | Platformbeheerder selecteert versioned defaults | Iedere toegestane module, ongeacht starttemplate |

**Verplichte kernvoorzieningen** (IAM, audit, ledger-integriteit, policy, financiële security, minimum approvals) zijn altijd aanwezig en niet uitzetbaar. Onboarding kan per klant zichtbare functies verbergen zonder verplichte veiligheidsfuncties uit te schakelen.

## Onboarding-flow
1. Kies profieltemplate; toon geselecteerde aanbevolen modules (zichtbaar als aangevinkt) en optionele overige modules (niet verborgen op basis van profiel).
2. Toon voor iedere module functionaliteit, benodigde andere modules, impact op migraties, optionele AI-agenten, gegevenscategorieën, kostenindicatie en gegevensverwerkingsinformatie.
3. Laat klant vóór voltooiing modulevinkjes wijzigen. **Acceptatievoorbeeld: ZZP + uitgebreide rapportages** moet in dezelfde flow kunnen.
4. Controleer afhankelijkheden, kosten/contract, rights en privacy; bereken een preview van routes/gegevensstromen. Module-aanzet geeft geen impliciete external-AI-grant.
5. Toon uiteindelijke keuze expliciet en sla vast met templateversie, configversie, actor en tijdstip.
6. Laat afzonderlijk AI-processing policy configureren. Externe AI standaard UIT; vereiste zakelijke toestemming/overeenkomsten en privacyrechtelijke grondslag apart vastleggen.
7. Maak organisatie en administratie(s) aan; initialiseer modulefeatures idempotent. Ga pas naar werkruimte na geslaagde provisioning.

## Wijzigen na onboarding
- `Instellingen > Modules`: overzicht actief/uit; dependency chain; kosten/contractimpact; gekoppelde AI-rollen en dataflows; impact op bestaande taken/exports; preview + bevestiging.
- Bij deactiveren: geen nieuwe command of AI-tasks meer; background jobs stoppen/quarantaine; open approvals laten afhandelen/overdragen; behoud geposte boekingen, audit en wettelijke records; historisch read-only waar nodig.
- Niet verwijderen van posted ledger history, geen stilzwijgende migraties of data-overdracht; bij reactiveren consistent herstel uit gedocumenteerde state.
- Profieltemplates versioned; nieuwe defaultversie alleen als expliciet voorstel, niet automatisch bij bestaande tenant doorvoeren.
- Dependencies zijn directioneel en acyclisch; onmogelijk conflict leidt tot heldere foutmelding en rollback.

## Capability model en grenzen
```
ModuleDefinition {id, version, dependencies[], capabilities[], aiRoles[], dataCategories[], configurable}
ProfileTemplate {id, version, defaultModuleIds[], recommendations[]}
TenantModuleSelection {tenantId, moduleId, enabled, changedBy, effectiveAt, version}
EffectiveCapability = licensed AND moduleEnabled AND userAuthorized AND contextAllowed AND policyAllows
```
`Licensed` is een commercieel entitlement, **niet hetzelfde** als het profiel. Alle mutaties server-side checken. De featureconfig heeft geen recht om SoD, betalingslimieten of externe privacybescherming uit te schakelen.

## API/UI-contracten (suggesties)
- `GET /v1/module-catalog` (met actuele voorwaarden, afhankelijkheden, AI-data impact)
- `GET /v1/profile-templates` (aanbevolen defaultModuleIds)
- `POST /v1/onboarding/module-selections/validate` (pure preview)
- `POST /v1/onboarding/complete` (idempotente commit)
- `GET /v1/settings/modules`; `PATCH /v1/settings/modules/{moduleId}` (optimistic lock, audited)
- `GET /v1/settings/ai-processing` en afzonderlijke, streng geautoriseerde AI-policy mutation endpoints

## Definition of Done
- ZZP + uitgebreide rapportages direct te selecteren, tonen en later aan te passen.
- Bevoegde beheerder kan modules veranderen zonder accounting history te verliezen; open workflows veilig afgehandeld.
- Module actief verleent geen extra user/AI/provider rechten; cross-tenant checks groen.
- Core niet uitzetbaar; dependencies getoetst; audit/rollback voorzien.
- Klant kan exact zien welke modules welke agenten/dataflows impliceren, ook als externe AI UIT staat.

## v0.3 — extra opt-in modules voor alle profieltemplates
- `annual_accounts` (Zelf jaarrekening / financiële jaarstukken samenstellen) en `external_auditor_portal` (Externe accountant inzage) zijn **aparte opt-in modules**, niet alleen voor MKB. Een ZZP'er kan er tijdens onboarding voor kiezen, ook met extra rapportages. Juridische jaarrekening/deponeringsplichten verschillen per rechtsvorm.
- Beide modules hebben een eigen enable/disable lifecycle en kunnen onafhankelijk worden geactiveerd; beschikbare functies zijn afhankelijk van financiële bronfunctionaliteit, rechten, wettelijke toepasselijkheid en contract, niet van startprofiel.
- Jaarrekening heeft eigen rolrechten `view_annual_accounts`, `prepare_annual_accounts`, `review_annual_accounts`, `adopt_annual_accounts`, `file_annual_accounts` en scoped exports; auditportaal `invite_external_guest`, `manage_guest_grants`, `review_evidence`, `export_evidence`.
- Geen automatische externe AI-dataverwerking, geen automatische controle- of deponeringsclaim en geen permanente guest access. Zie `docs/13_JAARREKENING_EN_ACCOUNTANTSPORTAAL.md`.

## v0.6 — Modulemappen en nieuwe opt-ins
Alle modules hebben specificaties onder `modules/`; templates en Guided Learning kunnen optioneel zijn. NL/EN is een platformbasis. Bedrijfsprofiel geeft defaults, geen beperking op aanbod; AI data-opt-in is afzonderlijk van functie-opt-in. `modules/INDEX.md` is verplicht voor wijzigingsroutering.

## v0.7 — Commerciële opt-in: Guided Learning is betaalde add-on
- `guided-learning` is **niet standaard inbegrepen** bij enig organisatieprofiel en wordt bij onboarding standaard **uit** aangeboden als afzonderlijk geprijsde add-on (tarief en facturatieparameters zijn nog te bepalen; geen bedragen verzinnen).
- In modulecatalogus toon aparte prijs/contractstatus, afhankelijkheden en optionele AI-kosten. Aanschaf/licentie, module-opt-in, gebruikersrechten en AI-datatoestemming zijn vier verschillende toestemmings- en controledimensies.
- Module pas beschikbaar als `paid_entitlement.active AND tenant.moduleEnabled AND actor.authorized AND context.allowed`. Frontend én backend én jobs valideren dit; bij verval of beëindiging fail-closed zonder verlies van financiële geschiedenis of auditgegevens.
- `guided-learning-ai-builder` vereist Guided Learning-entitlement als afhankelijkheid én afzonderlijke AI-processing authorization. AI-chat mag niet een niet-gelicentieerde tour starten. Het gewone Helpcentrum/de statische gebruikershandleiding blijft voor alle klanten toegankelijk.
- Bevoegde beheerder kan add-on bewust aanschaffen/activeren via contract-/betaalstroom (commercieel betalingsproces nader te specificeren). Geen stille activering door profiel of interface-template.

## v0.8 — ERP Integration Hub opt-in
`erp-integration-hub` wordt als afzonderlijke opt-in in de modulecatalogus toegevoegd; niet standaard geactiveerd door een bedrijfsprofiel. De module heeft een permanent connectorbeleid, anders dan `migratie` en `parallel-run`. Activatie vereist capability/ownership-/privacycontrole; de klant moet per administratie kunnen kiezen welke data vanuit ERP naar Finance gaan en welke statussen terug kunnen. Commerciële prijsstelling nog open, geen impliciete betaalde add-on claim. Zie `modules/erp-integration-hub/README.md`.

## v0.9 — tenant provisioning als core, sandbox optioneel
Tenant-/subdomeinbeheer is een verplicht platformfundament en kan niet per bedrijfsprofiel worden uitgeschakeld. Bij eerste registratie wordt tenant + uniek platform-subdomein gecreëerd, daarna kiest het bedrijf profiel- en moduleopties (bijv. ZZP + rapportages). Demo Data & Sandbox is een afzonderlijke toekomstige optionele mogelijkheid en moet fictieve branchegegevens kunnen genereren. Guided Learning blijft een betaalde add-on, standaard uit.

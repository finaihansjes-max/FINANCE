# 01 — Product requirements

## Gebruikers en tenants
- SaaS met organisaties (tenants), administraties (legal entities), vestigingen/afdelingen, kostenplaatsen en projecten.
- Rollen: tenant owner, tenant security admin, finance admin, AP clerk, AP approver, budgethouder, controller, CFO/directie, AR clerk, auditor (read-only), externe accountant, medewerker. Volledig configureerbare rollen plus dynamische ABAC-voorwaarden.
- Permission verbs: `view, create, edit, submit, approve, reject, post, reconcile, export, send, execute_payment, manage_rules, manage_users, close_period, reopen_period, configure_ai`.
- Autorisaties op tenant, administratie, entiteit, kostenplaats, bedrag, veld, eigendom, project, status en SoD-conflict.
- Scheiding duty: indiener ≠ goedkeurder; IBAN-wijziger ≠ verificateur/betalingsfiatteur; rule-wijziger ≠ activator; betaalbatchmaker ≠ bankfiatteur. Ook bij delegatie en tijdelijke rollen.
- Tenantadmins mogen geen platformaudit verwijderen. Break-glass alleen via tijdgelimiteerde, sterk geauthenticeerde procedure met onafhankelijke review.

## Schermen en expliciete exports
Elke hieronder genoemde dataview heeft export naar `.xlsx`, met huidige filters/sortering, optioneel alle geautoriseerde pagina's, metadata van export en auditlog; geen verborgen of onbevoegde data.
1. Home / taken / AI-control-center; 2. Inkomende documenten; 3. Inkoopfacturen; 4. Inkooporders; 5. Ontvangsten; 6. Leveranciers; 7. Betalingsvoorstellen; 8. Betaalbatches/status; 9. Banktransacties; 10. Bankreconciliatie; 11. Verkoopfacturen; 12. Klanten; 13. Creditnota's; 14. Debiteurenbeheer; 15. Grootboek; 16. Journaalposten; 17. Rekeningschema; 18. Kostenplaatsen/projecten; 19. Btw-overzicht/aangiftevoorbereiding; 20. Vaste activa; 21. Voorraadwaardering; 22. Budgetten; 23. Afsluiting/reconciliaties; 24. Rapportages; 25. Goedkeuringsinbox; 26. Workflowdesigner; 27. Gebruikers/rollen/mandaten; 28. Integraties; 29. AI-instellingen/dataverwerking; 30. AI-activiteitenlog; 31. Auditlog; 32. Dataretentie/exportoverzicht; 33. Organisatie-instellingen.
Niet elke layout leent zich voor cel-voor-cel-export: dashboard -> onderliggende datasets; detail -> gestructureerde veld-/mutatiebladen; configuratie -> configuratietabel zonder secrets.

## Procesinrichting
- AP: intake -> OCR/structurering -> duplicaatcontrole -> leverancier/IBAN check -> PO/GR matching -> fiscale/ledger validatie -> approval -> posting -> payment proposal -> betaling goedkeuren -> bank execution -> reconciliation.
- AR: verkooporder/integratie -> conceptfactuur -> controles -> risicogestuurde verzending -> ontvangen betaling -> matching -> aanmaningen -> kredietnota/refund (zwaarder fiatteren).
- GL: voorstel -> validation -> controlled journal posting -> period lock -> reconciliation -> close -> export.
- VAT: fiscale classificatie, aangifteconcept, aansluiting GL, menselijke goedkeuring voor indiening.
- Reporting: cijfers uitsluitend uit grootboek/rapportage-engine; AI toelichting met herleidbare bronnen.

## Approval engine
- Criteria: bedrag incl/excl btw, rol, expliciete user-ID, afdeling, kostenplaats, project, leverancier, locatie, bankwijziging, PO/non-PO, fiscale afwijking, budget, risk score, valuta, posting period, transactietype; AND/OR, prioriteit, uitzonderingsregels.
- Stappen sequentieel of parallel; quorum, 4-ogen, escalatie, SLA, delegatie met expiry, vakantievervanging, conflictcontrole.
- Policy version per decision, niet achteraf herinterpreteren; approval bindt aan canonical transaction snapshot + SHA-256 fingerprint; wijziging van materiële velden invalidates approvals.
- Voorbeeld, TENANT-CONFIGUREERBAAR: tot €250 autopost bij complete match en geen risk signals; tot €2.500 budgethouder; tot €10.000 budgethouder+controller; daarboven budgethouder+CFO. **Fraudetriggers overrulen bedrag**. Default autopost uit tot UAT.
- Menselijke goedkeuring van factuur ≠ betalingsfiattering; versturen/indienen vereist specifieke `send`- of `submit`-rechten.

## Kritieke human-in-the-loop-acties
Versterkte controle voor: nieuwe leverancier, IBAN-wijziging, verdachte factuur, non-PO zonder bewijs, grote/afwijkende factuur, betalingsbatch, definitieve bankautorisatie, refund, grote creditnota, write-off, risicovolle manual journal, voorraad-afwaardering, periode-afsluiting en heropening, btw/VPB indiening, externe rapportage, nieuwe integratie, autorisatiewijziging, workflowwijziging, gewijzigde AI-provider/model/data-policy, massacorrectie, bulkexport.

## Niet-functioneel
- Accounting correctness, availability without AI, auditability, security, data portability, responsive UI, toegankelijkheid, internationale valuta en Nederlandse btw-regels als versioned rule sets.
- Geen gegarandeerde 100% autonome verwerking; meet false positives, false negatives en menselijke override.
- P95 performance targets, RPO/RTO, kostenplafonds en SLA bepalen in ADR vóór productie.

## Bedrijfsprofielen, module-opt-in en uitbreidbaarheid (verplicht)
- Admin kiest onboarding-template `zzp`, `klein_mkb`, `middelgroot_mkb` of aanbieder-geconfigureerd profiel. Template bepaalt **default checked** modules, niet beschikbaarheid van aanvullende modules.
- Tijdens onboarding direct toggles voor alle aangeboden optionele modules, inclusief ZZP + uitgebreide rapportages. Vooraf geselecteerde modules zichtbaar; per module omschrijving, eventuele prijs, afhankelijkheden en AI-gevolgen; expliciete bevestiging van gekozen configuratie.
- Bevoegde tenantbeheerder kan later via `Instellingen > Modules` wijzigen; effectpreview, rollbackplan en audit. Deactiveren bewaart historische boekingen en auditgegevens en ontneemt niet automatisch bestaande wettelijke bewaarplichten.
- Kernfeatures ledger, IAM, audit, integriteitscontroles en verplichte goedkeuringen zijn niet uitschakelbaar. Modulebeschikbaarheid/licentiëring is gescheiden van rechten van een gebruiker. Module actief ≠ gebruiker bevoegd.
- Leverancier kan versiebeheerbare profieltemplates maken/beheren zonder bestaande klanten automatisch om te zetten. Klant kan reset naar defaults **als voorstel** krijgen, niet als destructieve wijziging.
- Per AI-module zichtbaar: welke datacategorieën nodig zijn, welke agentrollen, waar verwerking plaatsvindt, welke aanbieders en retention. AI-modules en externe AI-verwerking vragen aparte beleidsbesluiten; AI-modeltraining is verboden in beoogde productpolicy.

## Meerdere administraties, segmenten en consolidatie (verplicht)
- Eén tenant kan meerdere juridische entiteiten, meerdere ledgeradministraties en optionele organisatie-/rapportagesegmenten bevatten. Retail en Wholesale kunnen entiteiten **of** bedrijfsonderdelen zijn. Eerst expliciet classificeren, nooit afleiden.
- Administratiewisselaar met duidelijke scope-indicator; iedere boeking verplicht gebonden aan één ledgeradministratie. Autorisatie op tenant, entiteit, administratie en dimensie.
- Gebruiker kan één of meerdere consolidatiegroepen/rapportagescopes kiezen mits bevoegd; geconsolideerde financiële schermen zijn **read-only** voor operationele boekingen.
- Afzonderlijk consolidatiebeheer: groepslidmaatschap en geldigheidsperioden, groepsrekeningschema/mapping, rapportageperioden, intercompany counterparties, matching en eliminaties; menselijke goedkeuring voor eliminatiejournalen en close/reopen.
- Ondersteun eliminaties van intercompany omzet/kosten, vorderingen/schulden en waar relevant overige consolidatieverschillen; geen automatische wegboeking van onverklaarde verschillen. Eliminatieboekingen wijzigen nooit de entiteitsboekhouding.
- Kostenplaats, kostendrager, project en zelfgedefinieerde analytische dimensies zijn optioneel en niet hardcoded in één vaste volgorde; meerdere onafhankelijke of geneste hiërarchieën mogelijk met historische versiebeheerbare toewijzing.
- Consolidated XLSX exports, drilldown, audit en AI-analyses mogen alleen gegevens bevatten die passen binnen de expliciete consolidatie- én persoonsrechten. Bevat een groep ontoegankelijke leden, dan ofwel gehele view weigeren of duidelijk gemarkeerde, goedgekeurde beperkte scope; nooit een misleidend totaal als volledig presenteren.
- Maak rapportages geschikt voor single administration, single legal entity, selected entities en consolidatiegroep met vergelijkbare periodes en transparante eliminatietotalen. Zie docs 11 en 12.

## v0.3 — Jaarrekening en auditgast (optioneel)
- Opt-in zelf jaarrekening samenstellen voor klanten zonder wettelijke controleplicht, ook beschikbaar als module voor andere profielen onder passende verplichtingen. Configureer interne jaarstukken, enkelvoudige en waar ondersteund geconsolideerde jaarrekening, en publicatiestukken afzonderlijk.
- Bronvaste, versiebeheerde rapportage uit afgesloten administratie(s), met aansluiting, toelichtingen, vergelijkende cijfers, review, vaststelling, export en eventuele getoetste SBR/XBRL-indiening.
- Gastaccountant krijgt tijdelijk alleen-lezen toegang op specifieke administratie(s), entiteit(en), perioden en dossierstukken; geen boek-, betaal-, verstuur- of autorisatierechten. Optionele bevindingen-/bewijsverzoeken in aparte review-workspace en logging van inzage/exports.
- AI-agenten helpen bij volledigheidscontroles, jaarrekeningtoelichtingen en voorbereiding van auditbewijs, maar nooit zelfstandig verklaringen afgeven of definitief deponeren.
- Onboarding en later modulemenu bieden `annual_accounts` en `external_auditor_portal` onafhankelijk van startprofiel; externe AI-verwerking blijft aparte toestemming/configuratie. Zie `docs/13_JAARREKENING_EN_ACCOUNTANTSPORTAAL.md`.

## v0.6 — Extra harde producteisen
NL/EN vanaf kern; universele configureerbare migratie; gecontroleerde proef van standaard 60 dagen met ons platform als enige schrijfbron en enkel uitgaande sync naar legacy; migratiedashboard; template-presentatielaag; niet-AI Guided Learning runtime met AI-ondersteunde voorafgaande creatie en chatretrieval; modulegerichte wensen in `modules/`. Zie docs 18–25.

## v0.7 — UX en presentatie (GEPLAND)
De standaardproductervaring is het moderne lichte AI Finance-dashboard naar de aangeleverde conceptafbeelding (intern: `assets/design/ai_finance_standard_dashboard_reference.png`): KPI's, analyses, transacties, zoekfunctie en AI-assistent, met eigen visuele identiteit. De gebruiker kan navigatie **links verticaal** of **boven horizontaal** plaatsen, los van ZZP/MKB-profiel, module-opt-ins, NL/EN en gekozen boekhoudtemplate. Bekende pakket-presets zijn gericht op vertrouwde werkpatronen, met afzonderlijke juridische/compatibiliteitstoets; nooit de boekhoudkundige of beveiligingskern wijzigen. Elk scherm is responsive en toegankelijk. Zie `docs/26_DESIGN_SYSTEM_AND_NAVIGATION.md`.

## v0.8 — ERP-koppeling als opt-in productcapability (GEPLAND)
Permanent configureerbaar ERP-integratieframework per tenant/entiteit/administratie. ERP leidend voor operationele facturen, orders en voorraad, AI Finance voor financiële verwerking tenzij ERP-journalmodus expliciet wordt geselecteerd. Standaard ERP → Finance, beperkte goedgekeurde betaal-/verwerkingsstatus terug. Beheerscherm voor mapping, events, fouten, dataflow/AI-privacy en financiële aansluitcontrole. Nooit dubbel boeken; zie `docs/27_ERP_INTEGRATION_HUB.md`.

## v0.9 — hosting- en registratiecontract
**Besluit:** elk bedrijf krijgt automatisch een eigen uniek subdomein op ons SaaS-platform (voorbeeld `bedrijf.aifinance.nl`), met afzonderlijke tenant, gebruikers, rollen, administratie(s), data, modulekeuze en AI-toestemmingen. Website en subdomein zijn niet de security boundary: server-side tenantisolatie is verplicht. Meerdere entiteiten binnen één tenant, geen verwarring tussen entiteit en tenant. Zie `docs/28_SAAS_TENANT_SUBDOMAINS.md`.

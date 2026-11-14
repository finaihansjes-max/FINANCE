# 27 — ERP Integration Hub: operationeel ERP als leidend systeem

**Status: GEPLAND / DESIGN.** Dit is een permanente, configureerbare bedrijfsintegratie en **niet** de tijdelijke legacy-koppeling voor de 60-daagse migratieproef. Zie `docs/19_UNIVERSAL_DATA_MIGRATION.md` en `docs/20_PARALLEL_RUN_SYNC.md` voor die afzonderlijke processen.

## Bedrijfsdoel en eigenaarschap
- Standaard is het ERP **system of record** voor verkoop- en inkooporders, ontvangst/levering, voorraadbewegingen, artikelen, operationele facturen die in ERP ontstaan en hun bronstatussen.
- AI Finance is **system of record** voor de eigen financiële grootboekadministratie, financiële boekingen, btw-/rapportageberekeningen, bankaflettering, betalingsvoorstellen en consolidatie, **alleen voor zover** het ERP-integratieprofiel AI Finance als financiële boekingsmotor aanwijst.
- **Per entiteit, administratie, objecttype én proces configureerbaar**: eigenaar, producer, consumer, richting, voorrang, wijzigingsrecht, boekingsmodus, frequentie en minimale ondersteunde API-capabilities. Geen universele aanname dat ieder ERP dezelfde functies biedt.
- Standaard **ERP → AI Finance** voor operationele brondocumenten en events. Beperkte, apart geautoriseerde **AI Finance → ERP** terugkoppeling is toegestaan voor bijvoorbeeld `accepted`, `rejected`, `posting_failed`, `reconciled` en `payment_status`; geen impliciete rechtstreekse mutatie van ERP-facturen, voorraad of prijsdata.
- Heeft ERP al een eigen financiële boekingsmotor, kies **exact één** financiële boekingsautoriteit per journal/event/administratie: (A) importeer door ERP geproduceerde gevalideerde journaalposten, zonder die nogmaals te genereren, of (B) ontvang operationele documenten en genereer de financiële boekingen in AI Finance. De beide modi mogen nooit voor hetzelfde event samengaan.

## Configureerbaar data contract
Objecten (per connector en alleen als ondersteund): `supplier`, `customer`, `item`, `purchase_order`, `goods_receipt`, `sales_order`, `shipment`, `stock_movement`, `sales_invoice`, `purchase_invoice`, `credit_note`, `cost_center`, `dimension`, `project`, `erp_journal_entry`, `payment_status`.

Voor ieder object verplicht: `tenant_id`, `legal_entity_id`, `administration_id`, `source_system`, `external_record_id`, `external_version`, `event_id`, `occurred_at`, `ingested_at`, `schema_version`, `mapping_version`, `source_hash`, `financial_posting_mode`, `correlation_id`; beleid voor valuta, tijdzone, btw, eenheden en correcties. Gebruik non-destructive bronkopie + genormaliseerde canonieke representatie + traceerbare doelmapping. Onbekend/ongeldig → quarantaine, niet stil verwerken.

## Pijplijn en betrouwbaar transport
1. Connector registreert capabilities (API, webhooks, polling, rate limit, read/write scopes, delete/correction support, attachments, terugkoppelstatus).
2. Authenticatie met beperkt scope, encrypted secrets, rotatie, herroepbaarheid; tenant- en administratiescope server-side valideren.
3. Receive → validate schema/signature/origin → idempotency → tenant/entity routing → mapping → accounting validation → benodigde menselijke workflow → atomic finance post → durable outbox → minimale statusterugmelding.
4. Queue/retries met backoff, dead-letter, causal ordering per document, idempotency keys, deduplicatie bij herhaald webhook/event, partial failure handling. Geen belofte van fysieke exactly-once transport: bereik **effectively-once business posting** met uniek event/source fingerprint en constraints.
5. Correcties van geboekte transacties uitsluitend met gecontroleerde tegenboekingen of nieuwe journaalversies. ERP-brondocumenten in AI Finance niet stilzwijgend overschrijven.
6. Reconciliatie van documentaantallen, bedragen, btw, openstaande posten en voorraadwaardering-effecten, plus dagelijkse (instelbare) ontbrekend-eventcontrole. Bij ontbrekende velden/attachments of onvoldoende capabilities eerst expliciete beperking tonen en benodigde handmatige handeling aanvragen.

## Voorraad en intercompany
ERP beheert de fysieke aantallen en operationele voorraadtransacties. AI Finance ontvangt alleen de voor boekhouding benodigde events/waarderingsgrondslag. Voorraadaanpassingen worden niet vanuit Finance terug naar ERP geschreven als operationele mutaties. Transfer Retail → Wholesale kan een interne kostenplaatsbeweging óf tussen juridische entiteiten een intercompany-transactie zijn; bepaal juridische aard uit de organisatiestructuur, niet uit een naam. Consolidatie-eliminaties gebeuren uitsluitend in de aparte consolidatielaag; de bronadministraties en ERP-voorraad blijven intact.

## Integratiebeheerscherm (NL/EN)
`Integraties → ERP Integration Hub` toont verbonden ERP(s), bron-eigenaarschap per proces, sync-richting, laatste succesvolle import, uitstaande/afgekeurde events, mappingversie, fouten, verschillen, limieten, privacyoverzicht, autorisatie, pauze/hervat, veilige replay en financiële reconciliatie. Filters, data-afbakening en `.xlsx`-export volgen de bestaande rechten. Alleen bevoegde beheerders mogen mapping of eigenaarschap wijzigen; financiële wijzigingen vereisen goedkeuringsworkflow, versiebeheer, dry-run en rollbackplan.

## AI en privacy
ERP-integratie werkt **zonder AI**. Optionele AI Integration Mapping Agent of Reconciliation Agent kan voorstellen doen voor veldmapping of afwijkingsanalyse, maar nooit zelfstandig connecties autoriseren, boekingen aftekenen, records publiceren of een ERP-bewerking uitvoeren. Standaard geen ERP-/factuurdata naar externe AI. Bij opt-in per tenant: provider, doel, benodigde velden, verwerking/regio, retentie, toestemmingen/verwerkersafspraken en auditlog. Geen ander klantdata-domein of extra entiteit vanuit een ERP-token benaderen.

## Scheiding van tijdelijke migratie
Een actieve 60-daagse Controlled Parallel Migration heeft aparte status en beperkte legacy-writeback uit AI Finance → oud boekhoudpakket. De permanente ERP Integration Hub is daar niet mee uitwisselbaar. Nooit automatisch een permanente ERP-sync koppelen als legacy rollback-kanaal. Bij overlap: expliciete bron- en boekingsautoriteit, dubbele-eventpreventie en cutoverplan vastleggen.

## Kritieke risico's en controles
- **Dubbele journaalpost:** source mode check + uniqueness `(tenant, administration, source_system, external_record_id, event_id, posting_type)`; ERP-journal-import en document-to-journal nooit beide actief voor één bron-event.
- **Event uit verkeerde entiteit:** server-side entity map en policy, quarantainestatus bij onduidelijkheid.
- **Gemist of vertraagd event:** sequence/cursor monitoring, replay met idempotency, reconciliation en SLA-alarmen.
- **Verkeerde mapping / btw / valuta:** typed mapping, golden fixtures, dry-run, vierogencontrole bij activatie of materiële wijziging.
- **External payload injection:** payload als onbetrouwbare data; geen uitvoerbare instructies of AI-promptrechten.
- **Excessieve ERP-API-rechten:** minst mogelijke leesrechten, alleen expliciet noodzakelijke terugschrijfrechten; bevoegdhedenwijziging MFA/approval.
- **Aansluitingsverschil:** stop finalisatie en escaleer; geen automatische afboekingen.

## Acceptatiecriteria
1. Synthetische ERP-inkoopfactuur arriveert tweemaal: precies één financiële boeking en één bronkoppeling; zichtbare statussen.
2. ERP-journalmodus en documentboekingsmodus blokkeren dubbelboeken van hetzelfde event.
3. Alleen ERP → Finance brondocumenten; teruggaande interface stuurt uitsluitend toegestane statussen zonder factuur/voorraadwijziging.
4. Verkeerde tenant/entiteit, verouderde mapping, onbekend valutatype en ongeldig BTW-event worden tegengehouden en auditeerbaar vastgelegd.
5. Missing events, retries, out-of-order webhooks en ERP-uitval worden hersteld zonder extra journaalposten.
6. ERP-voorraadtransfer wordt correct financieel verwerkt zonder operationele voorraadstand te wijzigen; IC-transfer tussen twee juridische entiteiten krijgt de juiste route.
7. Handmatige review voor risicovolle mapping-/eigenaarschapwijziging; SoD en approvals niet te omzeilen via API of AI.
8. Klant kan precies zien welke gegevens van en naar ERP gaan; externe AI blijft default uit; NL/EN en handleidingen, `.xlsx` en audittrace getest.
9. Is een ERP-capability onvoldoende (bijvoorbeeld correcties of betaalstatus), blokkeer de betreffende flow of bied expliciet beperkte ondersteuning; nooit stilzwijgend volledigheid claimen.

## Open beslissingen
- Selectie van eerste concrete ERP-connector na capability assessment; geen claim dat SAP/Dynamics/NetSuite/AFAS alle genoemde API-acties ondersteunen.
- Contract-/prijsmodel van permanente ERP-integratie; opt-in is vastgesteld, betaalstatus **nog niet**.
- Default financiële boekingsmodus per type ERP; pas vastleggen na klant-/connectorinventarisatie.

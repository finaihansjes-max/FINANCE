# 17 — Beheerders- en operationshandleiding

**STATUS: ONTWERP / GEPLAND. Geen operationeel product in deze ZIP.** Dit document loopt gelijktijdig met gebruikershandleiding en applicatiecode.

## Verantwoordelijkheden
Platformbeheerder beheert templates en releases; tenantbeheerder beheert eigen gebruikers, entiteiten, modules en AI-permissies; securitybeheerder beheert incidentproces; financebeheerder configureert financieel beleid. Deze rollen zijn logisch gescheiden; functiescheiding is verplicht.

## Beheer van profieltemplates en modules (GEPLAND)
Definieer optionele modules, veilige defaults en afhankelijkheden. Een ZZP-profiel mag alle toegestane optionele modules selecteren. Documenteer uit- en inschakelgedrag, migraties, eventuele prijsimpact en open workflows. Core IAM/audit/ledgerbeleid nooit uitschakelbaar.

## Beheer van organisatie en consolidatie (GEPLAND)
Leg per tenant juridische entiteiten, ledgeradministraties, bedrijfsonderdelen, dimension types, consolidation groups, ownership, valuta, periode en intercompany mappings expliciet vast. Geen consolidatie buiten de verleende scope. Elimineer alleen binnen afzonderlijk versiebeheerd consolidatiejournal.

## AI-provider en dataflowbeheer (GEPLAND)
External AI default `false`. Houd register bij van provider, modelversie, regio, subverwerkers, doel, data-elementen, bewaartermijn, auditlogs en goedkeurende beheerder. Router fail-closed bij providerfouten. Geen ongemerkte fallback naar andere provider/regio. Zero retention en geen training uitsluitend beloven na verifieerbare contract-/endpointconfiguratie.

## Accounts, privileges en reviewers (GEPLAND)
MFA en least privilege. Tijdelijke guest-viewer via geautoriseerd engagement; export, AI-vragen en auditnotities afzonderlijk. Periodieke access reviews, SoD-controle, toegang intrekken, incidentlogging.

## Logging, backup en incidenten (GEPLAND)
Audittrail append-only, gevoelige velden uit logs, gecontroleerde retentie en hersteltests. Bij fraudepoging: freeze affected commands, behoud bewijsmateriaal, meldplicht beoordelen, herstel en retest. Geen productie-instructies veronderstellen zolang infrastructuur niet is geïmplementeerd.

## Ontwikkelteam en testisolatie
Gebruik de rollen en geautoriseerde synthetic-data-red-teamtests in `docs/14_MULTI_AGENT_DEVELOPMENT_TEAM.md`; nooit op echte klantdata of publieke systemen testen.

## Releases en documentatie
1. Wijzig requirements en code met tests.
2. Werk de relevante secties in **beide** handleidingen bij en wijzig status GEPLAND→GEIMPLEMENTEERD alleen na tests.
3. Genereer HTML/PDF opnieuw met `python scripts/build_readable.py`.
4. Controleer `python scripts/build_readable.py --check` en documenteer reviewer-sign-offs.
5. Deploy pas na governance/finance/security gates en expliciete toestemming.

## Nog in te vullen bij implementatie
Concrete URL's, schermafbeeldingen, deploymentinstructies, support-SLA, monitoringdashboards, backupprocedure, handmatige escalaties en incidentcontacten worden tijdens relevante sprints aangevuld. Niet invullen met verzonnen details.

## v0.6 — GEPLAND / PLANNED: beheerhandelingen
- Configureer connector-capabilities, mappingversies, migratiedryrun, audit en eenrichtingssync per administratie. Monitor dead-letter queues, source changes en rollback readiness; claim nooit veilige terugkeer zonder gereconcilieerde saldo’s en open posten.
- Beheer NL/EN vertalingen, interface-templates, stabiele `data-guide-id` contracten, Guided Learning builder en gepubliceerde tourversies. AI kan concepten aanmaken maar niet publiceren of uitvoeren.
- Routeer alle nieuwe featurewensen via `modules/INDEX.md` naar het module-README; update tests/handleidingen/leesbare versies en volg `docs/25_MODULE_SPEC_GOVERNANCE.md`.
- **English:** Admins maintain connector mapping/capability records, sync status, locale catalogs, tour publishing and module requirement files. All functionality above is PLANNED until implemented and verified.

## v0.7 — GEPLAND: design, navigatie en presets beheren
Beheer versieerbare tokens/designcomponents, `NavigationRegistry`, `ExperienceTemplateService` en tenantdefaults. Nieuwe gebruikers krijgen AI Finance Default en `left_vertical`; persoonlijke keuze `top_horizontal` blijft per gebruiker en wordt niet stilzwijgend door tenantinstellingen overschreven. Review presets op merkrechten, veilige declaratieve configuratie, toegankelijkheid en compatibiliteit met alle modules, scopes, NL/EN en Guided Learning selectors. Maak preview en rollback, blokkeren bij missende IDs of overtoegankelijke menu's. Train geen AI op klantgegevens voor templategeneratie. Update handleidingen en render 1-op-1 HTML/PDF vóór publiceren.

## v0.7 — PLANNED admin guidance (English)
Maintain versioned design tokens, approved templates, and user-scoped navigation preferences. Verify navigation permissions, keyboard accessibility, guided-learning selector mappings, template licensing and synthetic-data previews before publication. Any release must pass NL/EN documentation and generated HTML/PDF synchronization checks. These operations are **PLANNED**.

## Betaalde Guided Learning-module beheren (NL; GEPLAND)
Controleer per tenant de contractentitlement, verloopdatum, status en audittrail voordat `guided-learning` wordt geactiveerd. Het standaardprofiel mag de module niet inschakelen. Ontkoppel licentie, activatie, gebruikersrol en AI-toestemming. Test rechten via de backend, ook na opzegging of verlopen licentie. Bewaar tourdefinities en logs volgens overeengekomen bewaartermijn en schakel verwerking veilig uit; raak financiële historie niet aan. Informeer de klant vóór wijzigingen in commerciële voorwaarden.

## Managing the paid Guided Learning add-on (EN; PLANNED)
Validate tenant subscription entitlement and audit history before enabling the module. Never include Guided Learning by default in a profile. Enforce subscription, activation, user authorization and AI-consent independently at the API, runtime and job layers. On expiry or cancellation, deny access safely while preserving ledger and required audit records; use documented retention rules for tour definitions.

## v0.8 — ERP Integration Hub beheer (NL; GEPLAND)
Beheer opt-in connector, ERP-API-scopes/secrets, capabilitymatrix, per-entiteit mapping en recordsleutels, bron-eigenaarschap, `financial_posting_mode`, statuscallback allowlist, event inbox/outbox, retry/deadletter/replay, en financiële aansluitrapporten. Activeer alleen na dry-run en dubbele-boekingscontrole. Pas mapping-/autorisatieregels alleen met versiebeheer en bevoegde goedkeuring aan. Scheid dit van Controlled Parallel Migration en voorkom dubbele ingestroomde transacties. Opschalen, pauzeren en ontkoppelen zijn auditbaar. Geen standaard toegang van AI tot ERP-klantgegevens.

**EN — PLANNED:** Manage ERP connector scopes, capability registry, entity mappings, ownership, posting mode, status callback allowlist, retry/dead-letter replay and financial reconciliations. Separate permanent ERP sync from legacy migration, review mapping changes independently, and document outbound data and AI-processing decisions per customer.

## Gepland — subdomeinen provisionen en beheren
Beheer tenant slug-reservering, platform DNS/TLS, provisioning saga, membership, isolatiechecks, suspension/offboarding, supporttoegang en hergebruik/quarantaine uitsluitend in centraal geautoriseerd platformbeheer. Fail-closed wanneer DNS/TLS of scopepolicies niet correct zijn. Custom domains zijn geen MVP-eis. Procedures, runbooks en alerts volgens `docs/28_SAAS_TENANT_SUBDOMAINS.md`; niet als operationeel claimen vóór tests.

Controleer bij nieuwe ERP-adapters quotaheaders, tokens in querystrings, connectorstatus en financiële reconciliatie. Demo-omgevingen mogen geen live bank-/ERP-egress activeren.

# 09 — Architectuurbesluiten / open vragen

## Voorlopige ADR's
- ADR-001: modular monolith before microservices; separate worker + privacy gateway.
- ADR-002: deterministic double-entry ledger as source of financial truth.
- ADR-003: multi-agent proposals/reviews, independent policy-enforcement and human-in-the-loop.
- ADR-004: tenant AI provider egress deny-by-default; no training allowed.
- ADR-005: versioned approval policies + immutable transaction snapshot/hashes.
- ADR-006: standard export contract for every data view.
- ADR-007: first release without fully automated payment initiation or tax filing.

## Nog te nemen besluiten vóór productie
- Hostingprovider, EU/EER residency, ownership of encryption keys, network segregation and support access.
- TypeScript backend framework, UI framework, orchestration queue, vector/RAG approach (tenant-isolated).
- AI model choices: local vs external; specific processor agreements, data retention and model availability for selected endpoints; costs.
- Pricing, SLA, DPA, RTO/RPO, on-call/incident responsibilities, enterprise support.
- Payment integrations / licensing assessment (Wft/PSD2), banking API partner.
- Tax/accounting expertise for NL rules and regulated boundary decisions.
- Supported legal entities, currencies, VAT schemes, fiscal years and industry segments in v1.
- Data retention timetable by category; deletion and termination process.
- Which automatically posted transactions may be enabled, confidence thresholds plus observed error rates.
- External financial and security auditors for pre-launch UAT/pentest.

## Bij iedere wijziging
Schrijf een afzonderlijke ADR met context, alternatieven, veiligheid/privacy-impact, besluitnemer, datum, tests en rollbackplan. Verander nooit stilzwijgend privacyverplichtingen of AI-provider.

## Nieuwe ADR's
- ADR-008 (voorlopig): profielen zijn enkel aanbevolen, bewerkbare moduledefaults; modules als versioned capabilities; tenantmodules onafhankelijk van AI-datarechten. Geen profielafhankelijke blokkades voor extra optionele modules.
- ADR-009 (voorlopig): expliciet onderscheid tenant/legal entity/ledger administration/business unit/dimension/consolidation group; read-only consolidatieviews en apart controlled consolidation ledger.
- ADR-010 (voorlopig): intercompany agenten mogen uitsluitend voorstellen/reviewen; eliminaties staan in aparte consolidatielaag na policychecks en menselijke vrijgave.

## Aanvullende open ontwerpkeuzes
- Welke modules vormen verplicht core en welke zijn commercieel optioneel? Welke productplannen/prijzen gelden, los van profiel?
- Actuele templates voor ZZP / klein MKB / middelgroot MKB; definities, naamgeving, versiebeheer en migratiebeleid.
- Entiteiten vs bedrijfsonderdelen: kwalificatie tijdens onboarding, rechten/delegaties per administratiescope.
- Ondersteunde verslaggevingsstandaarden, groepsvaluta, FX-conventies, eliminatiemethodiek, ownership/control, minderheidsbelangen, intercompany-netting en sluitingsprotocol; accountantexpertise noodzakelijk.
- Eén juridische entiteit met meerdere administraties: welke operationele/rapportageconsolidaties zijn toegestaan, zonder dubbele tellingen?
- Vrije dimensionele hiërarchieën: meerdere parent-relaties, geldigheid, mappings, additionele scope-ABAC, wijzigingen met retrospectieve impact.
- Toegang tot geconsolideerde groep bij deels onbevoegde leden: standaard deny; eventuele beperkte scopes expliciet, zichtbaar en juridisch getoetst.
- Accounting rules bij consolidatie verschillen per land en bedrijfsgroep: eerst NL MVP of internationaal?
- AI-agenten in groepscontext: opt-in per module/doel/provider, EER processing modes, extra disclosure events en dataminimalisatie.

## v0.3 — nog te besluiten vóór implementatie jaarrekening/guest portal
- Welke rechtsvormen, verslaggevingskaders, bedrijfsklassen en boekjaren ondersteunen we in MVP (bv, vof, eenmanszaak; micro/klein)?
- Scope jaarstukken: interne jaarstukken, statutaire jaarrekening, publicatiebalans, enkelvoudig, geconsolideerd? Welk niveau vanaf eerste release?
- Implementeren we SBR/XBRL-deponering echt, of slechts export en een handmatige KVK-workflow? Welke actuele taxonomie en SBR-koppeling?
- Wat is het toetsingsproces voor wettelijk toepasselijke controle-, opmaak-, vaststellings- en deponeringsverplichtingen?
- Gastaccountant: uitsluitend viewer + vragen en commentaar, of aanvullend getekende assurance-workflow in toekomstige release?
- Welke persoonsgegevens/bankvelden standaard gemaskeerd; wie mag onmaskeren en exporteren; welke maximale gastlooptijd?
- Contractuele positie van accountant als ontvanger/eigen verwerkingsverantwoordelijke nader juridisch beoordelen; geen algemene AVG-aannames.
- Producteigenaren voor modellen, fiscale templates, accountantsreview en juridische vrijgave benoemen.

## Beslissingen v0.5
- ADR-DOC-001: Markdown is canoniek; alle leesbare HTML/PDF wordt deterministisch uit Markdown gegenereerd. CI blokkeert drift.
- ADR-DOC-002: Elk code-/UI-/policy-/AI-datawijzigingsverzoek omvat een update van gebruikers- en waar nodig beheerhandleiding in dezelfde PR. Geplande functies blijven expliciet GEPLAND.
- ADR-TEAM-001: Claude werkt met gescheiden developmentrollen inclusief onafhankelijke reviewers, QA, finance, privacy en afgebakend security-red-team. Agenten mogen uitsluitend geautoriseerde synthetische testdata benaderen en niet echte klantadministraties.
- Open: concrete agent-orchestrator, reviewmodeldiversiteit, CI-systeem, hosting en providercontracten moeten bij Sprint 00 worden gekozen op basis van actuele tooltoegang; niet zonder bewijs aannemen.

## v0.8 — Vastgestelde ERP-integratiebesluiten en open punten
**Vastgesteld:** ERP is standaard operationeel leidend (orders, voorraadverschuivingen, ERP-facturen); permanente hoofdflow ERP → AI Finance; terug alleen beperkte statusupdates; systeem-eigenaarschap blijft per proces configureerbaar; geen dubbele financiële boekingen; integratie is opt-in en niet dezelfde als tijdelijke legacyproefperiode. **Open:** eerste ondersteunde ERP-provider en API-capabilities; ERP financial-journal import versus Finance posting per connector; welke betaalstatus callbacks ondersteund worden; commercieel prijs-/licentiemodel; operationele SLA's. Geen implementatieclaims vóór tests en verificatie.

## v0.9 — bevestigde keuzes en open punten
**BESLOTEN**: optie 1: iedere nieuwe klant krijgt automatisch een uniek subdomein onder het (later te kiezen) platformdomein; centraal SaaS, multi-tenant, browser. Custom domains niet vereist. Entiteiten en administraties vallen binnen de tenant; roles/permissions/AI-consent per scope. DEMO-data moet later branche- en bedrijfsprofielspecifiek en synthetisch zijn. IT's Perfect API v2 wordt voorbeeldconnector.

**OPEN**: definitieve geregistreerde platformdomeinnaam, DNS/TLS- en cloudprovider, databaseruimte per tenant (RLS/schema/dedicated), subdomain slug-beleid, facturatie/lifecycle, exact IT's Perfect quota-bucket en API-capabilities, demo generatieparameters. Geen aannames als besluit presenteren.

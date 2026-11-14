# 04 — Data governance, AI-verwerking en security by design

## Feitelijke gegevensstromen — doelarchitectuur
1. Klant uploadt document of koppelt bron -> TLS -> intake service -> encrypted object storage (regio volgens contract).
2. Document wordt gescand en tenant-scoped vastgelegd; origineel blijft behouden voor fiscale audit zolang wettelijk nodig.
3. AI Privacy Gateway controleert: tenant-opt-in, doel, provider, model, regio, contract, endpoint, toegestane datavelden, retentie, redaction, risicoclassificatie.
4. Alleen noodzakelijke velden of documentdelen worden naar de geselecteerde **toegestane** modelruntime verstuurd. Soms heeft documentextractie volledige factuurinhoud nodig: geen onware belofte dat alles geanonimiseerd kan worden.
5. Externe provider verwerkt de input indien deze is geconfigureerd; provider kan volgens specifieke contract- en endpointvoorwaarden tijdelijk gegevens, logs of state bewaren. Geen garantie van nul-opslag zonder contractuele en technische verificatie.
6. AI produceert voorstellen; finance rules controleren; menselijke approvals waar vereist; structured output en disclosure worden gelogd zonder volledige promptdump.
7. Goedgekeurde boeking wordt in het grootboek opgeslagen; AI-context/temporary cache heeft aparte korte retentie; export, verwijdering en bewaarplichten volgen eigen lifecycle.

## Gegevenscategorieën en AI-toegang
| Data | Voorbeeld | Standaardregel |
|---|---|---|
| Publieke productinformatie | artikelomschrijving | toegestaan na scope-check |
| Zakelijke identificatoren | leverancier/KvK/btw-nummer | minimaal nodig; geautoriseerde AI-provider |
| Financiële transacties | bedrag, datum, kostenrekening | taakgebonden, minimaal noodzakelijk |
| Bankinformatie | IBAN, betaalreferentie | maskeren waar mogelijk; full IBAN alleen strikt noodzakelijke verwerking |
| Persoonsgegevens | contactpersoon, eenmanszaak, naam | privacyclassificatie en DPA vereist |
| Bijzondere persoonsgegevens | mogelijk in bonnetjes/medische kosten | niet naar AI zonder expliciete DPIA/juridische beoordeling; default blokkeren |
| Salaris/loonadministratie | BSN, salaris, loonstrook | eerste releases uitgesloten van externe AI; afzonderlijk productbesluit |
| API keys, wachtwoorden | secrets | NOOIT in prompts, logs, exports |
| Andere tenant | financieel dossier B | NOOIT toegankelijk voor A |

## Configurabele AI-datamodi (ontwerp, geen bestaande functionaliteit)
- `LOCAL_ONLY`: models draaien in door ons beheerde en gecontroleerde omgeving; **geen input naar externe generatieve AI-API's**, maar cloudbeheer/infrastructuur/subverwerkers blijven relevant. Alleen claimen na network-egress enforcement en testen.
- `EU_PROCESSING`: uitsluitend leveranciers/contracten/endpoints met aantoonbaar toegestane EER-verwerking en doorgiftegrenzen; EU cloud-regio alleen is geen garantie dat alle internationale toegang/doorgifte uitgesloten is.
- `APPROVED_EXTERNAL`: expliciet goedgekeurde modelproviders met bekende verwerkingslanden, retentie, subverwerkers en DPA. Per tenant en datacategorie instelbaar.
- Default nieuwe tenant: AI-aanlevering aan externe providers uit; product mag wel deterministic processing gebruiken. Geen fallback tussen modi zonder expliciete nieuwe toestemming.

## Klantzichtbaar AI Data Control Panel
Toon per tenant:
- Actieve AI-modus, provider, model, endpoint en relevante verwerkingslocaties;
- toegestaan doel per agent en toegestane datacategorieën;
- status modeltraining ('niet toegestaan in ons contract/configuratie'), retentiebeleid en concrete uitzonderingen;
- tijdlijn van elke disclosure: tijd, actie, provider, land/regio, categorieën, veldmanifest, bronobject-id, waarom nodig, policyversie, processor agreement reference;
- overzicht van verwerkers/subverwerkers en laatste wijziging;
- exporteerbaar verwerkingslog (zonder prompts/secrets);
- opt-out/AI uitschakelen, providerkeuze waar beschikbaar, data-export, verzoek voor verwijdering onder wettelijke bewaarplichten;
- incidenten en toegang tot transparantie-/veiligheidsdocumenten.

## Privacy Gateway — uitvoerbare policy checks
Reject if: unknown tenant, no AI permission, provider not allowlisted, data class blocked, destination not verified, opt-out, unavailable DPA, incompatible residency, retention not contractually established, prohibited field in payload, missing classification. Logs only metadata/hash, redacted errors. Escalation to human review does not grant access automatically.

## AI training versus verwerking
- **Geen modeltraining**: contract-/providerbeleid staat geen modelverbetering met input/output toe zonder expliciete separate toestemming.
- **Verwerking**: extern model ontvangt (delen van) gegevens en berekent een antwoord. Dit blijft dataoverdracht/verwerking, ook zonder training.
- **Opslag/retentie**: kan bestaan uit abuse logs, application state, caching, safety exceptions. Verschilt per provider, endpoint, model, contract.
- **Zero data retention**: geen universele garantie; mogelijk per enterprise arrangement en feature, met uitzonderingen. Bewijs nodig voor concrete configuratie.
- **Klantgegevens in app**: fiscale documenten blijven volgens bewaarplicht bewaard, ook wanneer AI-output snel verwijderd is.

## Reële providerresearch (controleer opnieuw bij contracteren)
- OpenAI API: standaard geen training op API input/output zonder opt-in; provider noemt standaard abuse-monitoringlog tot 30 dagen en endpointafhankelijke state/uitzonderingen. Bron: https://developers.openai.com/api/docs/guides/your-data
- Anthropic commerciële API: standaard geen training op input/output zonder expliciete toestemming; retention/zero-retention hangt af van contract, model en feature; sommige modellen/functies hebben extra retentie of uitzonderingen. Bronnen: https://platform.claude.com/docs/en/build-with-claude/api-and-data-retention en https://privacy.claude.com/en/articles/7996868-is-my-data-used-for-model-training
- Gebruik **Claude Code/Claude-consumentenabonnement niet als impliciete verwerkersovereenkomst** voor echte klantdata. Ontwikkelomgeving krijgt synthetische data totdat bedrijfscontract en datacontroles expliciet zijn geverifieerd.

## Governance en incidentrespons
- Verwerkersovereenkomst voor elke relevante verwerker, subprocessor register, transfer impact assessment waar van toepassing; privacy by design, DPIA-screening bij onboarding en wijzigingen.
- Privacy/InfoSec teams keuren providerwissels, region changes, logging, nieuwe data categories en modelupgrades goed.
- Datalekprocedure, incident response, klantmeldingsproces, security alerts en periodieke access reviews.
- Admin/support access: ticket, begrensd doel, minimaal recht, tijdslimiet, audit, waar mogelijk klantgoedkeuring; geen standaard onbeperkte toegang.
- Retention matrix: fiscaal dossier doorgaans 7 jaar, bepaalde categorieën 10 jaar; AI prompt/cache kort volgens per-provider contract; logs met noodzakelijke en geminimaliseerde inhoud, aparte retentie; recht op verwijdering uitzonderingen door wettelijke bewaarplichten.

## Acceptance evidence
- Capture outbound AI calls in controlled tests; verify provider allowlist and no unintended egress.
- Validate exact provider DPA, data processing location, endpoint support, current retention documentation.
- Tenant A/B cross-tenant prompt/API/retrieval/export tests (including embeddings/vector stores).
- Delete/export simulations, disaster restore tests, secret scan, prompt/log redaction test, review all customer-facing claims against runtime configuration.

## Dataverwerking bij module-opt-in en consolidatie
- Bewaar drie afzonderlijke bewijzen: **moduleconfiguratie** (functionaliteit beschikbaar), **verwerkingsbeleid/AI-autorisatie** (welke data voor welke agent/provider, doel en regio) en **juridische grondslag/DPA** (privacyrechtelijke afwikkeling). Een module-toggle is nooit automatisch toestemming aan een externe LLM.
- Onboarding toont per aangevinkte module: benodigde gegevenscategorieën, optionele AI-functies, provider(s), regio, doel, retentie, mogelijke ontvangers en de mogelijkheid zonder externe AI te werken waar ondersteund. Registreer beleidsversies en expliciete keuzes.
- Rapportagesmodule voor ZZP mag werken met deterministisch berekende cijfers zonder externe AI. AI-analyse kan onafhankelijk uitgezet blijven.
- Consolidatie brengt data uit meerdere administraties bijeen **binnen expliciet goedgekeurde groupscope**. Bronadministraties blijven afzonderlijk. Een gebruiker met rechten op Retail mag niet door een geconsolideerde export achterliggende Wholesale-gegevens ontvangen zonder aanvullend groeps- en entiteitsbeleid.
- Geef klanten in AI Data Control Panel ook een overzicht per entiteit en consolidatiegroep: documentcategorie, agent, ontvanger/provider, jurisdictie, verwerkingsdoel, dataset/velden-manifest, tijdstip, configuratieversie en eventuele doorgifte naar externe verwerkers. Log geen ruwe gevoelige payload in algemene telemetry.
- Meerdere controlerende agenten zorgen voor **meer verwerkingsmomenten**, niet vanzelf voor meer privacy. Per agent alleen noodzakelijke velden, toestemming/beleid opnieuw controleren, geen vrije cross-agent disclosure of cross-tenant retrieval.
- Consolidatiedocumenten en eliminaties bevatten gevoelige groepsdata; hanteer gescheiden data- en exportrechten, goedgekeurde retentie, gecontroleerd support access en onafhankelijk auditbewijs.
- Als klant modules uitschakelt, stop nieuwe verwerking/jobs/AI-taken, revoke toepasselijke grants, beheer lopende taken, behoud boekingen/audit voor wettelijke of contractuele bewaartermijnen; toon begrijpelijke data retention status. Vermijd claim 'direct alles gewist'.
- Garandeer pas 'EER-only', 'niet voor training', 'geen externe toegang' en specifieke retentie als deployment, contracten, subverwerkers, ingestelde provideropties en doorlopende verificatie dit ondersteunen.

## Verplichte privacy-contracttests
1. ZZP + rapportages activeren veroorzaakt geen outbound AI-call zolang externe AI uitstaat.
2. Module uitschakelen stopt toekomstige agent-jobs ook als oude taken in een queue staan.
3. Consolidatieaanvraag kan alleen expliciet toegestane administraties combineren.
4. AI-reviewer ziet geen brondata van niet-geautoriseerde groep/entiteit.
5. Export en LLM-context hebben dezelfde effectieve user-scope en masking, zonder dat consolidatie die rechten ongemerkt uitbreidt.
6. Een wijziging in provider, regio, retentieprofiel of datacategorie vereist hernieuwde policy-evaluatie en, indien voorgeschreven, nieuwe klantautorisatie.
7. Data-disclosure log vermeldt de feitelijke provider/route, niet alleen de gewenste configuratie.

## v0.3 — jaarrekeningdata en externe accountants
- Jaarrekeninggegevens zijn gevoelige financiële gegevens. Verwerking blijft tenant-, entity-, administratie-, boekjaar- en dossiergebonden; AI krijgt uitsluitend relevante rubrieken en gepseudonimiseerde bronfragmenten waar mogelijk.
- Jaarrekening of gastportaal activeren geeft **geen impliciete toestemming voor AI-egress**. Toon afzonderlijk: verwerkingsdoel, datafields, AI-provider, regio, retentie, subverwerkers en daadwerkelijke request-/evidence-logs.
- Externe accountants verwerken via tijdgebonden gebruikersaccounts; toegang, export/download, documentdrilldown, AI-query en intrekking worden per opdracht gelogd. De klant kan een export-/toegangsrapport downloaden.
- Bij deactiveren van modules of verlopen van guest grant blijven wettelijke administratie-/auditretenties intact; revoke tokens en voorkom verdere toegang. Privacymeldingen zijn ontwerpdoelen totdat technisch/contractueel geverifieerd.

## v0.6 — Transparantie migratie en Guided Learning
- Migratieconnectoren verwerken financieel bronmateriaal binnen beheerde tenant-scopes; per connector dataflow, autorisaties, API-scopes, bron/ontvangst, subverwerker, opslag, bewaartermijn, logging en recht op teruglevering.
- Eenrichtingssynchronisatie terug naar legacy is expliciet zichtbaar voor de klant; geen garantie van volledige terugkeer zonder capability assessment + reconciliatie.
- Guided Learning runtime verwerkt selectie/DOM-events lokaal en voert geen LLM-verzoeken uit; AI-builder gebruikt in beginsel alleen UI metadata, nooit factuurgegevens zonder geverifieerde opt-in. Chatverzoeken vallen onder apart AI-databeleid.

## v0.8 — ERP connector dataflows en klanttransparantie (GEPLAND)
Per gekoppeld ERP publiceer een controleerbaar dataflowregister: ERP-provider, tenant/entiteit/administratie, objecttypes en velden, bron/eigenaar, transportrichting, doeleinden, API-autorisaties, encryptie, opslag, retentie, subverwerkers, foutqueues en exporttoegang. Operationele data naar AI Finance hoeft niet naar een AI-model; externe AI-verwerking is afzonderlijke, standaard uitgeschakelde opt-in met specifieke veld- en providerregistratie. Statuscallbacks bevatten minimale noodzakelijke velden. ERP credentials via secret vault met korte scopes, rotatie/revocation, geen toegang tussen tenants. Mappings, webhooklogs en errorpayloads mogen geen geheimen of onnodige persoonsgegevens lekken. Zie `docs/27_ERP_INTEGRATION_HUB.md`.

## v0.9 — subdomein is geen security boundary
- Hostname wordt alleen als ingang naar server-side tenantregister gebruikt. Sessies, API's, uploads, downloads, exports, auditlogs, AI-chat, search, caches, background jobs en derde-koppelingen controleren tenantmembership, objectrechten en entiteit/administratie-scope onafhankelijk opnieuw.
- Tenant provisioning bevat standaard deny-all, MFA voor gevoelige rollen, aparte connectorsecrets en AI-egress uit. Cross-tenant red-teamtests gebruiken uitsluitend synthetische dossiers; support heeft geen onbeperkte financiële toegang.
- Demo-administraties zijn geïsoleerd van productie en mogen geen echte mutaties naar ERP/bank sturen. Per IT's Perfect connector moeten tokenredactie en rate-limitmonitoring aantoonbaar veilig werken.

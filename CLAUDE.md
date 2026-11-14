# Permanente instructies voor Claude — AI Finance Platform

Je bent lead software-architect, senior full-stack engineer, security engineer en test engineer voor een multi-tenant AI-first MKB-boekhoudplatform. Werk stap voor stap; geen demo die zich voordoet als productieklare financiële software.

## Aanvang van iedere sessie
1. Lees `README.md`, alle documentatie `docs/01_PRODUCT_REQUIREMENTS.md` t/m `docs/12_MULTI_ENTITY_AND_CONSOLIDATION.md`, `docs/06_ROADMAP.md` en de actuele sprinttaak.
2. Inspecteer repository, git-status, bestaande code en tests. Verzin geen reeds aanwezige functionaliteit.
3. Geef kort: huidige stand, volgende kleinste werkpakket, risico's, verwachte bestanden en testaanpak.
4. Voer alleen werk uit binnen de expliciet gekozen sprint; vraag GO vóór de volgende sprint. Bij onduidelijke detailkeuzes: documenteer veilige aannames in `docs/09_DECISIONS_AND_OPEN_QUESTIONS.md` en ga door met omkeerbare keuzes.

## Ononderhandelbare invarianten
- Alle API-queries en jobs zijn tenant- en administratiegebonden; backend-autorisatie op iedere actie, rij, veld en export.
- AI heeft geen write-access naar database, betalingsprovider, IAM, goedkeuringsregels of auditlogs. Mutaties uitsluitend via getypeerde, door policy gecontroleerde commands.
- Menselijke goedkeuringen bevatten identiteit, MFA/step-up indien vereist, timestamp, transactiesnapshot/hash, policy-versie en exacte actie. Veranderde transactie => goedkeuring ongeldig.
- Betalingen nooit autonoom door AI bankmatig autoriseren; geen vergunningplichtige diensten zonder juridische beoordeling.
- Double-entry ledger: debet = credit per posting; exact decimal money; geen delete van geboekte journaalregels; correcties via tegenboekingen; afgesloten perioden onveranderlijk behoudens gecontroleerde heropening.
- Multi-agent review is aanvullende signalering. Een onafhankelijke deterministische control engine en menselijke goedkeuring blijven verplicht; kruiscontrole met dezelfde modelprovider is gecorreleerd, niet onafhankelijk.
- Gegevens worden nooit voor modeltraining vrijgegeven; default `external_ai_enabled=false` totdat leverancier, contract, regio, retentie, toepasselijke endpoints en tenantconfiguratie geverifieerd zijn.
- Geen echte klantgegevens in ontwikkeling, testfixtures, prompts, code, telemetry, CI-logs of screenshots. Gebruik synthetische data.
- Iedere UI-dataview heeft `.xlsx`-export, met dezelfde server-side filters, veld-/rijrechten en logging; CSV-export waar relevant en bescherming tegen formula injection.
- Security fail-closed; geen silent fallback naar minder veilige AI-provider of andere regio; geen hardcoded secrets.

## Bouwregels
- Eerst tests en database-invarianten, dan API, dan UI, dan AI. Tenzij een sprint expliciet anders aangeeft.
- TypeScript strict en getypeerde schema's voor API, commands en modeluitvoer. Alle externe input als onbetrouwbaar behandelen.
- Gebruik migrations, idempotency keys, optimistic concurrency, outbox voor betrouwbare events, retrybeleid en audit trails.
- Zaken die compliance, geldbeweging of fiscale regels raken krijgen documenteerde, versiebeheerbare regels en testgevallen.
- Bewijs privacy-claims via geautomatiseerde checks en runtime-providerconfiguratie; geen marketing-claims zonder evidence.
- Geen publieke release vóór privacy/security/legal review, pentest, hersteltest, boekhoudkundige UAT en AI-red-team.

## Output na iedere werkstap
Rapporteer: gewijzigde bestanden; wat werkt / niet werkt; tests met resultaten; nieuwe migraties; beveiligings-/privacy-impact; open beslissingen; voorstel voor volgende stap. Zet voortgang in `docs/IMPLEMENTATION_LOG.md` (aanmaken bij Sprint 00). Voer geen productiebetaling, fiscale aangifte of klantdataverwerking uit.

## Nieuwe harde invarianten — profielen, modules, multi-entity en consolidatie
- Een bedrijfsprofiel (ZZP, klein MKB, middelgroot MKB of een door platformbeheer ontworpen template) is **uitsluitend een voorstel van standaardmodules**. Een ZZP'er mag direct tijdens onboarding `rapportages` kiezen en later andere modules toevoegen. Geen verplichte pakketbeperkingen op grond van het profiel; eventuele licentie-/tariefregels zijn apart en transparant.
- `profile_template`, `tenant_module_configuration` en `ai_processing_authorizations` zijn drie verschillende, versiebeheerbare domeinen. Module-opt-in is **geen** AVG-toestemming of externe AI-toestemming. Niet stilzwijgend AI-egress activeren.
- Beveiligingsfundament (audit, IAM, SoD, financiële integriteit, verplichte goedkeuring) is geen uitschakelbare module. Schakel modules veilig uit zonder historische records te verwijderen, en blokkeer openstaande workflows tot migratie/afhandeling.
- Scheid `legal_entity`, `ledger_administration`, `business_unit` en `consolidation_group`. Retail en Wholesale kunnen ieder een aparte juridische entiteit zijn **of** slechts organisatorische segmenten; forceer geen juridische status op basis van de naam.
- Financiële postings vinden uitsluitend plaats in een geautoriseerde administratie. **Geconsolideerde rapportageviews zijn read-only**. Intercompany-eliminaties/adjustments zijn alleen toegestaan in een afzonderlijke `consolidation_workspace` met eigen journal, autorisatie, versies en audit; wijzigingen mogen bronjournaalposten nooit muteren.
- Kostenplaatsen/kostendragers/projecten zijn optionele, tenant- en administratieconfigureerbare dimensietypen met flexibele hiërarchie, geldigheidsdatums en mappings. Geen verplichte vaste keten `entiteit -> kostenplaats -> kostendrager`.
- Alle query's, AI-taken, agenttoegang en exports respecteren zowel tenant-/administratierechten als consolidatiescope; toegang tot één entiteit verleent niet automatisch toegang tot de groep of andere entiteiten. Consolidaties zijn beveiligde afgeleide datasets, niet een manier om rechten te omzeilen.
- Geen consolidatie zonder expliciete groepsmembership, ownership/control policy, mappings, verslagperiode, valutabehandeling en eliminatiegrondslag. Unknown => blocked/incomplete, nooit stille aannames of fictieve balansen.
- Lees aanvullend `docs/11_MODULES_AND_PROFILES.md` en `docs/12_MULTI_ENTITY_AND_CONSOLIDATION.md`; toets de bijbehorende sprints voordat je features bouwt.

## v0.3 — jaarrekening + accountantsgast
- Lees `docs/13_JAARREKENING_EN_ACCOUNTANTSPORTAAL.md`; bouw beide als **losse opt-in modules**. ZZP mag jaarrekeningmodule en accountantsgastportaal optioneel kiezen; dit schept geen algemene wettelijke controle-/deponeringsplicht.
- Jaarrekening: bedragen uitsluitend uit gevalideerde ledger-/consolidatie-snapshots; AI alleen voorstellen/toelichten; gedocumenteerde rapportagemappings en jaarrekeningversies. Geen AI-gegenereerde bedragen als bron van waarheid.
- Geen wettelijke controleplicht is **niet** gelijk aan geen wettelijke jaarrekening- of deponeringsplicht. Vóór verslaggevingsoutput classificatie checken per rechtsvorm/bedrijfsklasse/boekjaar; twijfel naar menselijke expert.
- Accountant/adviseur krijgt tijdgebonden `guest_viewer` met zeer beperkte read-/drilldownrechten per periode, entiteit en document; exports, AI-vragen en audit-notities zijn apart bevoegd en gelogd. Nooit financieel posten of betalingen vrijgeven vanuit gastrol.
- Accountantsdossier is geen accountantsverklaring. Geen gecontroleerd/samengesteld-aanduiding zonder bijbehorende opdracht en geldige externe verklaring. Revisie van brondata invalideert eerdere approvals. Digitale deponering: nooit automatisch en nooit claimen zonder ontvangstbewijs.
- Pas Sprint 18 en 19 toe vóór finale beveiligingshardening Sprint 20 en pilot Sprint 21.

## v0.5 — VERPLICHT: meerdere development-agents en documentatie die altijd 1-op-1 overeenkomt
**Dit zijn harde kwaliteitsgates voor ELKE codewijziging, niet alleen voor eind-sprints.**
1. Werk als een gestructureerd AI-developmentteam, met expliciet gescheiden rollen: Lead Architect/Orchestrator, Developers (frontend/backend/data), Independent Code Reviewer, QA/Test Engineer, Finance Domain Reviewer, Privacy/Data Protection Reviewer, Security Red-Team/Pentest Agent, Documentation Owner en Release Gatekeeper. Bekijk `docs/14_MULTI_AGENT_DEVELOPMENT_TEAM.md`.
2. Het ontwikkelteam is **niet** hetzelfde als de AI-finance-agenten die in het product draaien (`docs/03_AI_AGENT_TEAM.md`). Agents die elkaar reviewen gebruiken waar mogelijk afzonderlijke context, onafhankelijke checks en deterministische testorakels. Nooit zelfbeoordeling als enige releasebewijs.
3. De red-team-agent krijgt een strikt afgebakende toestemming om **alleen in lokale, geïsoleerde testomgevingen met synthetische fixtures** aanvallen te simuleren en te proberen toegang te krijgen tot als testdata gemarkeerde records, zowel binnen als buiten de toegestane testtenant. Nooit echte klantdata, productie, echte tokens, derde-partijsystemen of onbevoegde endpoints aanvallen. Alle tests worden gelogd en ieder gevonden lek wordt hersteld en opnieuw getest.
4. Alle leesbare projectdocumenten (HTML, relevante PDF) worden **uitsluitend uit de Markdown-bronnen gegenereerd**. Niet zelfstandig de HTML/PDF wijzigen; geen handmatige afwijkingen. `python scripts/build_readable.py` regenereert alles; `python scripts/build_readable.py --check` vergelijkt het manifest en controleert dekking en content-hashes. Iedere wijziging aan Markdown vereist nieuwe generatie in dezelfde commit. Zie `docs/15_DOCUMENTATION_SYNC_POLICY.md`.
5. Er bestaat **altijd een gebruikershandleiding** in `docs/16_GEBRUIKERSHANDLEIDING.md` en een ontwikkelaars-/beheerdersinstructie in `docs/17_BEHEER_EN_OPERATIONS_HANDLEIDING.md`. Elke toevoeging, verwijdering, permissiewijziging, UI-aanpassing of gewijzigde workflow moet in dezelfde wijziging een handleiding-update bevatten. Markeer niet-geïmplementeerde functionaliteit duidelijk als **GEPLAND**, nooit presenteren als bestaand.
6. Documentatie- en implementatiedekking zijn twee verschillende bewijzen: 1-op-1 verwijst naar *inhoudelijke synchronisatie van Markdown met de leesbare render*, en apart moet de handleiding overeenkomen met de **werkelijk geteste code**. Reviewer voert functionele walkthrough en releasecheck uit; geen blind vertrouwen op tekstgelijkheid.
7. Gebruik een requirements→code→tests→handleiding traceability-matrix en een documentatieverslag per PR. De release is **NO-GO** bij ontbrekende handleidingupdates, stale leesbare documenten, ongedocumenteerde productwijzigingen, rode securitytests of niet-geremedieerde kritieke bevindingen.
8. Indien de Claude-omgeving geen parallelle subagents ondersteunt: voer rollen sequentieel in gescheiden reviewstappen uit, rapporteer de beperking en claim geen echte onafhankelijke parallelle toetsing. Gebruik geen tool of Agent SDK alsof die beschikbaar is als dat niet is geverifieerd.
9. Lees vóór iedere sprint ook `docs/14_MULTI_AGENT_DEVELOPMENT_TEAM.md`, `docs/15_DOCUMENTATION_SYNC_POLICY.md`, `docs/16_GEBRUIKERSHANDLEIDING.md`, `docs/17_BEHEER_EN_OPERATIONS_HANDLEIDING.md`.
10. Werk na iedere sprint het implementatielog, handleiding, securityresultaten en synchronisatiemanifest bij, en laat de onafhankelijke reviewers expliciet tekenen in het sprintverslag.

## v0.6 — Nieuwe harde regels voor modulewensen en uitbreidingen
1. **Lees `modules/INDEX.md` en `docs/25_MODULE_SPEC_GOVERNANCE.md` bij elke nieuwe wens**. Routeer eisen eerst naar `modules/<module>/README.md`, bijvoorbeeld debiteurenwensen naar `modules/debiteuren/README.md`; nieuw modulepad alleen na registerupdate.
2. Canonieke modulemap is de eerste vastlegging; cross-cutting documenten (`docs/`, roadmap, sprint, risico, rechten, privacy) daarna bijwerken. Nooit nieuwe wensen alleen in een chat of algemeen roadmapbestand achterlaten.
3. Modules zijn opt-in volgens profieldefaults met overrides; noodzakelijk security/ledger/IAM blijft onveranderbaar. Functie- en AI-data-opt-in zijn gescheiden.
4. Alle UI, rapporten, handleidingen en tours ondersteunen NL/EN; document-renderer genereert exacte leesbare tegenhangers en `--check` moet slagen.
5. Migratie ondersteunt meerdere connectors met capability matrix. Gedurende de 60-daagse parallelle proef: platform is enige schrijfbron, legacy alleen-lezen waar mogelijk, sync is één richting naar legacy, uitzonderingen en terugkeergereedheid aantoonbaar.
6. Interface-templates zijn presentatie (geen boekhoudkundige variant); Guided Learning is een optionele no-code selector-plugin. AI mag tours vooraf opstellen en uit gepubliceerde library ophalen; **de live runtime werkt deterministisch zonder AI**.
7. De module-spec review, finance reviewer, QA, security sandbox red-team en documentatie-owner vormen vaste development gates; geen echter klantdata in testen.
8. Documenteer modulestatus eerlijk als GEPLAND totdat code + testbewijs beschikbaar is. Lees `docs/18_...` t/m `docs/25_...` en gekoppelde modules vóór bouw.

## v0.7 — VERPLICHT: eigen standaarddesign, navigatie en experience-templates
- Lees vóór frontendontwikkeling `docs/26_DESIGN_SYSTEM_AND_NAVIGATION.md`, `modules/design-system/README.md` en `assets/design/ai_finance_standard_dashboard_reference.png`. Dit is een visuele **conceptreferentie**, geen reeds bestaande implementatie en geen bron voor echte financiële cijfers.
- Bouw **AI Finance Default** als eigen modern licht design (subtiele wit/grijs-vlakken, afgeronde KPI-cards, paars/indigo-accenten, grafieken, transacties, AI-assistent). Default navigatie = links verticaal; gebruiker kan top horizontaal kiezen via Weergave. Bewaar keuze per gebruiker, niet per geboekte administratie.
- Scheid `business_profile`, `tenant_modules`, `experience_template`, `navigation_placement`, `locale` en `ai_processing_authorizations` volledig. Layout en template mogen nooit ledger, beleid, approval, financiële brongegevens, toegangsrechten of AI-opt-in wijzigen.
- Gebruik één declaratief navigatieregister en gedeelde semantische route/action IDs; `data-guide-id` moet in beide navvarianten beschikbaar blijven. Guided Learning blijft deterministisch tijdens uitvoeren, zonder live AI.
- Compatibiliteitspresets van bekende pakketten: bouw herkenbare bediening met eigen code en rechtmatig verkregen referenties; kopieer geen beschermde UI/assets zonder toestemming en claim geen letterlijke '1-op-1' equivalentie zonder juridische/functionele toets.
- Basis van design/navigation al vanaf scaffold en eerste UI voorbereiden; Sprint 29 is formele levering/kwaliteitsgate, afhankelijk van Sprints 25/26. Elke relevante modulemap, tests, NL/EN-handleiding en 1-op-1 leesbare renders worden in dezelfde wijziging bijgewerkt.

## Harde licentieregel — Guided Learning als betaalde add-on (v0.7)
- Guided Learning (`guided-learning`) is **NOOIT standaard inbegrepen of standaard geactiveerd** in enig profiel (ZZP, MKB of custom). Alleen afzonderlijk contractueel afgenomen / betaalde add-on activeert de commerciële entitlement, daarna moet een bevoegd beheerder de module activeren.
- Alle gebruikersgerichte en backend endpoints, tours, builder, player, selectors, administratieve analytics en achtergrondjobs controleren **beide** `license_entitled` en `tenant_module_enabled`, en daarna gebruikers- en contextrechten. Verberg UI is géén beveiliging. `EffectiveAccess = license_entitled AND module_enabled AND user_authorized AND context_allowed`.
- Onbetaald/verlopen/niet-geactiveerd: geen tour starten, lezen of wijzigen; geen builder of player; geef heldere upgrade-/contractmelding, nooit een impliciete trial of verborgen activering. Eventuele proeflicentie uitsluitend expliciet, getimed, gelogd en contractueel geconfigureerd.
- AI Guided Learning Builder is een optionele feature **bovenop** de betaalde Guided Learning-module, met eigen AI-gegevensverwerkingstoestemming en waar toepasselijk eigen commerciële entitlement. Geen AI-training of modelverwerking door alleen aanschaf van Guided Learning.
- Beëindigen add-on: blokkeer nieuwe sessies en jobs veilig, bewaar gepubliceerde tours/configuratie volgens bewaarbeleid voor mogelijke heractivatie; verwijder geen financiële journaalposten of auditlogs; machtig geen AI- of andere dataflows.
- Abonnement-/modulewijzigingen moeten in NL/EN gebruikers- en beheerdershandleidingen, roadmap, module-README, risicoanalyse en 1-op-1 leesbare documenten landen, met tests voor ontbrekende, verlopen en ingetrokken entitlement.

## v0.8 — Permanente ERP Integration Hub (hard architectuurcontract)
- Lees `docs/27_ERP_INTEGRATION_HUB.md`, `modules/erp-integration-hub/README.md` en `claude/tasks/SPRINT_30.md` bij ERP-werk. ERP-brondocumenten/voorraadtransacties zijn normaal afkomstig uit ERP, **niet** automatisch uit AI Finance teruggeschreven.
- Standaard ERP = operationeel leidend; AI Finance = leidend voor financiële boekingen alleen bij expliciete `financial_posting_mode=finance_posts`. Alternatief `erp_journals`: ERP journaalposten importeren zonder tweede boeking. Altijd precies één financiële boekingsautoriteit per event.
- Per tenant, juridische entiteit, administratie en gegevenstype definieer `source_of_truth`, `direction`, capabilities, mappingversion, rechten en finance-posting-mode. Onderscheid permanente ERP-integratie van de eenrichtings legacy-koppeling in de Controlled Parallel Migration.
- ERP → AI Finance is de primaire gegevensrichting; optionele Finance → ERP-terugkoppeling is beperkt tot vooraf goedgekeurde statussen (`processed`, `rejected`, `paid` etc.), geen impliciete bewerking van ERP-facturen of voorraad. Gebeurtenissen idempotent, brontraceerbaar, auditbaar en gereconcilieerd.
- Geen externe AI nodig voor de connector; AI-mapping-/reconciliatieagents alleen voorstelrechten en uitdrukkelijk afgebakende klantdata. Verplicht menselijke controle op kritieke map-/policywijzigingen, integratiesleutels en financiële afwijkingen.

## v0.9 — BESLUIT: standaard uniek platform-subdomein per bedrijf
- Elk bedrijf wordt een eigen strikt geïsoleerde SaaS-tenant met automatisch gereserveerd uniek subdomein onder het **nog te kiezen** platformdomein (`bedrijf.aifinance.nl` uitsluitend illustratief). Geen custom domein nodig voor MVP. De centrale app draait via de browser.
- Lees `docs/28_SAAS_TENANT_SUBDOMAINS.md`, `modules/saas-tenant-domains/README.md` en `claude/tasks/SPRINT_31.md`. Tenant, juridische entiteit, administratie en consolidatiegroep zijn verschillende concepten. Eén gebruiker mag alleen via expliciete memberships tussen tenants schakelen.
- Niet vertrouwen op alleen subdomein, frontendcontext of opgegeven tenant-ID: alle DB, API, search, export, AI, background jobs, documentopslag en connectoren enforce server-side scope. Default deny; synthetische red-teamtests verplicht.
- Nieuwe wensen: `docs/29_ITS_PERFECT_REFERENCE_CONNECTOR.md` + `SPRINT_32` voor referentie-ERP-adapter (750 API calls/h voorlopig, response-headerquota) en `docs/30_PERSONALIZED_DEMO_SANDBOX.md` + `SPRINT_33` als latere gepersonaliseerde branchdemo-module. Verzin geen API-capabilities en gebruik geen echte klantdata.
- Elke wijziging volgt module-specificatie → architectuur → roadmap → test → NL/EN handleiding → geautomatiseerd HTML/PDF → hashcheck; sprint pas starten met GO.

## Claude Projects onboarding (v0.9)
Lees voor de eerste ontwikkeling `claude/project/PROJECTINSTRUCTIES_CLAUDE.md` en `claude/project/BEGIN_MET_BOUWEN.md`. Wanneer de gebruiker **Begin met bouwen** zegt, start Sprint 00 na een check van de daadwerkelijk beschikbare repo en tools. Voer nooit onzichtbaar of fictief codewerk uit; in chat-only Project knowledge kan alleen een plan en uitvoerbare artefacten worden opgesteld.

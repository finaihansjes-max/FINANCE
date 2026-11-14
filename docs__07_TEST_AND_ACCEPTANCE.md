# 07 — Verificatie- en acceptatieplan

## Testpiramide
1. Unit: money arithmetic, journal balance, VAT rule fixtures, policy compilation, SoD, input schema.
2. Integration: DB constraints/migrations, tenant-scoped repository, approval binding, outbox, webhook signatures, exports.
3. E2E: AP lifecycle, bank sandbox, two approval flows, reopen period, user delegation.
4. Security: IDOR, API auth, privilege escalation, tenant A/B data bleed, prompt injection, exfiltration, malicious CSV/XLSX values, SSRF via uploads, webhook spoofing, large input/cost.
5. AI evaluation: labeled synthetic invoices; hallucinations; missing information; adversarial instructions; tax edge cases; model drift; agent disagreement; provider outage and fallback policy.
6. Nonfunctional: load, concurrency, idempotent retries, DR/recovery, audit tampering, backup restore, data export/delete, retention enforcement.

## Verplichte negative tests
- Tenant A met IDs van B krijgt 403/404 zonder informatielek in UI, API, exports, vector search of logs.
- AP creator/IBAN editor kan eigen factuur niet goedkeuren of betaling fiat geven.
- Wijziging van bedrag, supplier, IBAN, tax code, currency of payee na approval invalidate snapshot.
- AI ziet `POSTING_APPROVED` niet als het alleen zelf 'akkoord' schreef; token zonder scope geweigerd.
- Model in LOCAL_ONLY probeert outbound call -> network policy blokkeert.
- DPA ontbreekt of regio/model verkeerd -> provider request geblokkeerd.
- Ingesloten prompt-injectie in PDF/e-mail kan geen credentials, data, banktool of policy beïnvloeden.
- Fictieve fiscale aanname / model-output zonder bron -> human queue, geen auto-post.
- Duplicate bank webhook/retry creëert niet twee betaalinstructies.
- .xlsx met `=`, `+`, `-`, `@` en control-char tekstvelden wordt veilig geëxporteerd.
- Kritiek event kan niet uit audit verdwijnen; restore test bewijst consistente ledger.

## Testevidence
Voor iedere release: test-run-ID, commit hash, migratieversie, agent/modelversie, policyversie, gescande dependencies, red-teamrapport, buglijst met ernst, accorderende rol. Definieer foutbudgetten/false-accept thresholds in productbesluit voordat automatische posting wordt geactiveerd.

## Go/No-go productiegates
- Cross-tenant isolation, access control, immutable approval, accounting invariant, provider egress policy, audit trails en recovery zijn 100% geslaagd in voorgeschreven beveiligings- en acceptatiescenario's.
- Externe security review + fiscal/accounting UAT, privacy/legal review en incidentproces uitgevoerd.
- Geen kritieke/hoog-severity kwetsbaarheden zonder vastgelegde mitigatie en formele acceptatie; geen kritieke AI risk workaround via agent consensus.
- Klantverklaring getoetst aan échte configuratie en leveranciersvoorwaarden, inclusief mogelijke uitzonderingen.

## Aanvullende acceptatietests — profielen/modules
- ZZP-selectie toont correcte defaultmodules maar laat direct `uitgebreide_rapportages` aanvinken; rapportage kan direct na configuratie getoond worden met bevoegde gegevens.
- Klein/middelgroot MKB en leverancier-aangemaakte templates tonen defaults; tenantwijzigingen blijven behouden bij latere template-update.
- Deactiveren van een optionele module stopt nieuwe mutaties, achtergrondjobs en agentcalls; oude boekingen, bewijsstukken en audit blijven volgens bewaarbeleid terugvindbaar.
- Core security/ledger/approval kan niet uitgezet worden; dependencies worden vooraf gevalideerd; conflicterende feature toggles falen gesloten.
- Modulegebruiksrecht != userpermission != AI data authorization; test deze onafhankelijk, op frontend én server.
- Reportagemodule voor ZZP werkt zonder externe AI-aanbieder.

## Aanvullende acceptatietests — entiteiten/consolidatie
- Tenant A met entiteiten Retail/Wholesale; tenant B andere entiteiten. Operationele entries alleen in bevoegde administratie. Retail-only account ziet niet Wholesale via drilldown/export/agent.
- Retail en Wholesale als **business units van dezelfde legal entity** geeft segment-roll-up en niet onterecht juridische groepsconsolidatie.
- Kostenplaats `E-commerce` en kostendrager `Collectie` zijn optioneel; geen dimension, losstaande dimensies, geneste dimensies en versiegewijzigde hiërarchieën functioneren.
- Rapportages voor entiteit, administratie, geselecteerde groep en geconsolideerde run hebben expliciete scope, valuta, periode en consolidatiestatus.
- Consolidated reporting endpoints afwijzen `create/edit/post`; additionele, streng beschermde `consolidation_workspace` accepteert uitsluitend geautoriseerde elimination proposals -> approval -> balanced consolidation posting.
- Retail factureert Wholesale €1.000: beide operationele boekingen blijven bestaan; eliminatie verwijdert de interne omzet/kosten of vordering/schuld **alleen** uit het geconsolideerde resultaat, eenmalig, met bewijs en mapping.
- Ontbrekende tegenboeking, FX-rate, accountmapping, verschillende perioden of niet-goedgekeurde scope -> markeer consolidatie onvolledig of blokkeer; AI mag geen balanspost verzinnen.
- Wijziging groep/ownership/mapping creëert nieuwe runversie; historische goedgekeurde runs blijven reproduceerbaar; heropening/override vraagt strengere goedkeuring.
- Cross-tenant/cross-entity privacy-/ABAC-tests voor XLSX, AI-retrieval, embeddings, dashboard en API; geen verborgen datascope-unie.

## v0.3 — jaarrekening en externe audit
- Unit/golden tests voor micro/klein/andere bedrijfsklasse, rechtsvorm en boekjaar met expliciete unknown/unsupported-branch; geen verzonnen wettelijke status.
- Financiële tests: jaarrekeningbalans en winst/verlies aansluiten met exact frozen ledger; groepscijfers met goedgekeurde consolidatierun; mappingversies; post-approval bronwijziging trekt approval in.
- Autorisatietests: gast scoped op één entiteit/periode, kan niet in andere documenten of RAG-index zoeken, export of posten, betaling starten, IAM bewerken of guestrechten escaleren.
- Revocation- en expiry-tests op sessies, presigned downloads en gedeelde links. Review comments/evidence-requests mogen; ledger mutatie moet geweigerd worden.
- Filingtests: geen indieningsstatus zonder externe ontvangstbevestiging; handmatige review bij fiscale/juridische twijfel.
- AI egress-denial, trainingsverbod-policy en klanttransparantie voor beide opt-in modules verifiëren.

## v0.5 — extra verplichte test- en release-gates
- Multi-agent separation evidence: architect/developer/reviewer/tester/finance/privacy/red-team/documentation afzonderlijk gerapporteerd; vermeld als rollen sequentieel zijn uitgevoerd.
- Red-team binnen synthetische lokale sandbox: cross-tenant en cross-entity IDOR, guest viewer export, AI-egress, prompt injection, privilege escalation en approval bypass. Retest elke fix. Geen echte data of externe targets.
- Traceability gate: voor iedere gewijzigde requirement een implementatieverwijzing, positieve/negatieve tests en exacte gebruikershandleidingsectie.
- Documentatiegate: `python scripts/build_readable.py --check` is groen; HTML-dekking 1-op-1 voor alle Markdown; PDFs voor alle hoofddocumenten; hashes en auditmanifest consistent.
- Functionele waarheid: review of menutekst, gebruikersrollen, modulekeuzes, echte schermen en foutpaden in gebruikershandleiding kloppen met uitgevoerde code. Markeer niet-bestaande functies GEPLAND.
- Productie NO-GO bij niet-getoetste kritieke fixes, ontbrekende auditbewijzen, stale handleiding of discrepantie tussen bron en leesbaar document.

## v0.6 — Aanvullende acceptatie
- NL/EN alle routes, rapporten en handles; locale switch verandert geen financiële waarde.
- Adapter contract tests en rollback-tests op synthetische data, inclusief 429, timeouts, idempotentie en bron-wijzigingsdetectie.
- Niet synchroniseerbare data is zichtbaar, terugkeer wordt fail-closed afgewezen totdat opgelost.
- Guided Learning voert uit zonder AI-verbinding; security/permissions gelden per echte mutatie; selector regressie.
- Alle modulewensen in modulebestanden en docs-sync hash check.

## v0.7 — UX-regressie en templateveiligheid
Verplichte testmatrix per rol, tenant, administratie/consolidatiescope, NL/EN, experience-template, links/boven-navigatie en relevante modules. Assert: dezelfde gerenderde toegestane acties, identieke financiële backendresponscodes en exports, geen nieuwe ledgerpostings bij layoutwisseling, geen gedwongen AI-verwerking, stabiele Guided Learning selectors of fail-closed. Voeg toetsenbord/screenreader/responsive/WCAG 2.2 AA-testen en visuele regressie t.o.v. de eigen referentiestijl toe. Test dat het read-only consolidatiescherm nooit betaal-/boekacties krijgt. Geen onbevoegd gekopieerde assets/merken. Zie `docs/26_DESIGN_SYSTEM_AND_NAVIGATION.md`.

## v0.8 — ERP Integration Hub acceptatie (GEPLAND)
Voeg contract-, finance-, tenant-/entity-isolatie-, duplicate/replay-, API-token-, mismatch-, correction-, errorqueue- en webhook-securitytests toe. Run synthetische scenario's van ERP-invoice → een financieel journal, ERP-journal → geen tweede journal, 100 stuks voorraadtransfers → operationele voorraad uitsluitend in ERP, statuscallback zonder invoice-edit, kwaadwillend cross-tenant event → blokkade, en parallel legacy sync overlap → geen double-post. Onafhankelijke finance reviewer en API-security review vóór GO; productmanuals NL/EN en HTML/PDF check verplicht.

## v0.9 — verplichte nieuwe acceptatietests
- Onbekende of vervalste subdomeinen/forwarded headers, racecondition bij subdomain-aanmaak, tenant switch, export-/AI-/cache-/signed URL-lekken en accountantsgast-cross-tenancy worden in synthetische negatieve tests geweigerd.
- Provisioning faalt veilig bij DNS/TLS/databasefout en is idempotent/herstelbaar.
- IT's Perfect mock met 750/h quota en `X-Request-Limit-Date`/`X-Request-Limit-Remaining`; queue gaat niet verloren bij API-uitval.
- Demo-data heeft consistente ledger-/btw-/voorraadstanden; geen mogelijkheid om echte bank/ERP endpoints te bereiken.

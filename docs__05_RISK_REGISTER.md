# 05 — AI & Finance Risk Register

## Methodiek
Initiële scores = Kans (1–5) × Impact (1–5). **Hypothesen voor ontwerpprioritering**, geen gemeten frequenties. 1–5 laag, 6–14 verhoogd, 15–25 hoog/kritiek. Impact=5 vereist review ongeacht score. Restrisico alleen vaststellen met evidence (tests, monitoring, incidentdata); geen verzonnen restscore.

| ID | Risico | K | I | Score | Preventie/detectie | Test/exitcriterium | Eigenaar |
|---|---|---:|---:|---:|---|---|---|
| R01 | verkeerde grootboekcodering | 4 | 3 | 12 | ledger rules, evidence, steekproef | invalid account blocked; review logs | Finance architect |
| R02 | verkeerde btw-code | 4 | 4 | 16 | versioned tax engine, fiscal review | tax scenario suite | Tax SME |
| R03 | dubbele factuur | 3 | 5 | 15 | unique keys + fuzzy duplicate engine | duplicate retry blocked | AP lead |
| R04 | fictieve factuur/leverancier | 4 | 5 | 20 | vendor verification, PO/GR, flags | fake vendor escalates | Risk officer |
| R05 | IBAN-swap/fraude | 4 | 5 | 20 | out-of-band verification, SoD | same user cannot change+approve | Security + AP |
| R06 | AI ongeautoriseerde betaling | 3 | 5 | 15 | no payment tools, separate bank auth | tool escalation fails | Security lead |
| R07 | prompt injection in PDF/mail | 4 | 5 | 20 | untrusted boundary, typed output | adversarial prompt corpus fails safely | AI security |
| R08 | approval bypass | 3 | 5 | 15 | independent policy, immutable binding | mutated invoice approval invalid | Workflow lead |
| R09 | cross-tenant leak | 3 | 5 | 15 | tenant scoping & row/field ABAC | tenant A sees zero B data | Security lead |
| R10 | onjuiste belastingaangifte | 4 | 5 | 20 | deterministic reconciliation, human signoff | no auto filing | Tax SME |
| R11 | historie verwijderd | 3 | 5 | 15 | append-only audit, reversal | direct delete fails; restore passed | Core lead |
| R12 | bulkcorrectie | 4 | 4 | 16 | dry run, limits, approvals | batch threshold blocks | Controller |
| R13 | hallucinated financiële analyse | 4 | 4 | 16 | report-derived facts, citations | fabricated amounts rejected | Reporting lead |
| R14 | incomplete source data | 4 | 4 | 16 | schema, evidence, manual queue | incomplete file held | AP lead |
| R15 | model regression | 4 | 4 | 16 | evals, shadow, canary, rollback | regression gate passes | AI lead |
| R16 | AI outage | 4 | 3 | 12 | manual mode, queues | manual AP remains functional | SRE |
| R17 | privilege escalation | 3 | 5 | 15 | separate IAM, step-up | self-grant denied | IAM lead |
| R18 | onjuiste externe verzending | 4 | 5 | 20 | recipients allowlist, pre-send check | unintended recipient blocked | Security + AR |
| R19 | missed/false fraud flags | 4 | 4 | 16 | rules + anomaly reviews + appeal | precision/recall tracked | Risk officer |
| R20 | wijziging na goedkeuring | 4 | 5 | 20 | canonical snapshot hash + invalidation | stale approval rejected | Workflow lead |
| R21 | verkeerde administratie | 3 | 5 | 15 | entity scope required, API tests | cross-entity posting denied | Core lead |
| R22 | onbetrouwbare externe integratie | 4 | 4 | 16 | signature/schema validation, quarantine | forged webhook rejected | Integrations lead |
| R23 | modelprovider houdt data langer vast dan beloofd | 3 | 5 | 15 | contract/endpoint verification, egress policy | unsupported endpoint blocked | Privacy officer |
| R24 | gevoelige data in logging/embeddings | 4 | 5 | 20 | redaction, access, TTL, isolation | DLP/log/vector tests | Privacy + SRE |
| R25 | agents beoordelen elkaar onterecht positief | 4 | 4 | 16 | deterministic checks, diverse tests, human signoff | consensus alone never posts critical item | AI lead |
| R26 | prompt data onbedoeld als training feedback | 3 | 5 | 15 | no opt-in, feedback disabled, vendor review | verify data-control config | Privacy officer |
| R27 | data-exfiltratie via XLSX-export | 4 | 5 | 20 | server-side ACL, DLP, approval, audit | export bypass blocked | IAM lead |
| R28 | AI verkeerde ontvanger of betaalbatch | 3 | 5 | 15 | immutable pre-send/payment manifest | mismatch invalidates release | Controller |

## Top release-blockers
R05/R06/R07/R08/R09/R10/R11/R17/R18/R20/R21/R23/R24/R26/R27/R28: iedere kritieke boundary heeft een geautomatiseerde negatieve test, onafhankelijke security review en incidentprocedure nodig. Geen productie-Go zonder aantoonbaar werkende maatregelen.

## Risicobeheer per tenant
Bewaar eigenaar, datum, bron, initiële score, maatregel, testbewijs, restrisico, acceptatie door bevoegd persoon, volgende reviewdatum en incidentsignalering. Tenant mag strengere limieten kiezen, niet product-level hard safety rails uitzetten.

## Aanvullende risico's — modules / groepen / consolidatie
| ID | Risico | Kans (1–5) | Impact (1–5) | Score | Controle + eigenaar |
|---|---|---:|---:|---:|---|
| R29 | ZZP-template beperkt onterecht toegang tot gekozen rapportagemodule | 3 | 3 | 9 | Onboarding/feature-flag contracttest; Product Owner |
| R30 | Module-opt-in activeert onbedoeld externe AI-datadoorgifte | 4 | 5 | 20 | Aparte tenant-AI-policy, egress deny by default, disclosure-evidence; Privacy Lead |
| R31 | Deactiveren module verwijdert historische boekingen/gegevens | 3 | 5 | 15 | Retention guards, soft-disable, migratie/rollbacktests; Accounting Lead |
| R32 | Module-dependency wijzigt permissions of omzeilt SoD | 3 | 5 | 15 | Backend capability + ABAC checks, SoD regression tests; Security Lead |
| R33 | Klant ziet via consolidatie gegevens van niet-geautoriseerde entiteit | 4 | 5 | 20 | Group + member + user scope, deny/incomplete response, cross-entity tests; Security Lead |
| R34 | Retail/Wholesale onjuist als afzonderlijke juridische entiteiten gemodelleerd | 4 | 4 | 16 | Expliciete entity-type classificatie + validatie; Finance Architect |
| R35 | Onjuiste of dubbele intercompany-eliminatie | 4 | 5 | 20 | Matching, balance checks, evidence, version-bound dual approvals, uniqueness; Consolidation Lead |
| R36 | Onjuiste consolidatie door FX, periode, ownership of ontbrekende mappings | 4 | 5 | 20 | Completeness gates, rate/policy versions, finance UAT; Accounting Lead |
| R37 | Eliminatie wijzigt juridische bronadministratie | 3 | 5 | 15 | Gescheiden consolidatiejournal, forbidden API/path, immutable source tests; Backend Lead |
| R38 | AI-agent combineert onbevoegd data tussen entiteiten | 4 | 5 | 20 | Task-bound scoped retrieval, privacy gateway, disclosure log, red-team; AI/Security Leads |
| R39 | Dimensiehiërarchie veroorzaakt dubbeltellingen in roll-up | 4 | 4 | 16 | Acyclische versioned hiërarchieën, explicit aggregation/membership tests; Reporting Lead |
| R40 | Gewijzigd groepslidmaatschap herschrijft historische consolidatie | 4 | 4 | 16 | Immutable run snapshots, effective dating, versioned re-runs; Consolidation Lead |

Deze scores zijn initiële scenario-inschattingen, geen geobserveerde incidentkansen. Verplichte restrisico-acceptatie per release na geautomatiseerde tests + accountantreview.

## v0.3 — additionele risico's
| ID | Risico | Kans×impact (indicatief) | Beheersing | Validatie |
|---|---|---:|---|---|
| R23 | AI produceert fictieve jaarrekeningbedragen of toelichtingen | 4×5=20 | Immutable ledger snapshots, deterministic statements, source-linked proposals, reviewer | Mutated prompt cannot change balance sheet figures |
| R24 | Verkeerde classificatie en onterecht achterwege laten van deponering | 3×5=15 | Juridische beslisboom per rechtsvorm/boekjaar + expertreview + alert deadlines | BV vs eenmanszaak fixtures |
| R25 | Gastaccountant krijgt inzage buiten opdracht, periode of entiteit | 4×5=20 | Purpose/time-bound ABAC, row/field/document policies, step-up, revoke | Cross-entity and expired-grant denial |
| R26 | Gast exporteert meer persoons-/financiële gegevens dan toegestaan | 4×5=20 | Separate export grant, masking, scope tests, DLP/approvals, logging | Bulk export denied without grant |
| R27 | 'Gecontroleerd' of 'gedeponeerd' zonder geldige status/bewijs | 3×5=15 | Verified filing receipt and explicit professional status; no AI claims | Fake receipt / status spoof rejected |
| R28 | Jaarrekening na afsluiting gebaseerd op gewijzigde cijfers | 4×5=20 | Snapshot hash, invalidation, re-review, immutable versions | Post-approval journal change blocks filing |
| R29 | AI geeft auditor bewijs buiten toegewezen leesrechten | 4×5=20 | Retrieval scoped by guest grant, no cross-scope RAG, deny-by-default gateway | Prompt injection / scope escape test |
| R30 | Auditnotities beïnvloeden automatisch definitief ledger | 3×4=12 | Segregated review workspace; staff-only typed commands + approvals | Guest cannot invoke post command |

## v0.6 — Aanvullende risico’s
| Risico | Beheersmaatregel |
|---|---|
| Onvolledige import uit Exact/anders | Connector capability matrix, staging, mappingtests, subledger- en saldi-reconciliatie |
| Oud pakket is toch gewijzigd | Bronwijzigingsdetectie, legacy-readonly waar mogelijk, synchronisatie blokkeren en escaleren |
| Terugkeer terwijl sync achterloopt | `rollback_ready` fail-closed, pending queue = 0 voor vereiste data, financiële sign-off |
| Dubbele boeking door retry | Durable idempotency keys, source IDs, outbox, immutable ledger |
| Andere template wijzigt finance flow | Alleen presentation-schema, canonieke business actions en policy engine |
| Broken Guided Learning selector | Stabiele `data-guide-id`, UI-contracttests, blokkeren tour tot herpublicatie |
| AI-tour doet ongeautoriseerde actie | Alleen draft, review/test/approval; runtime via bestaande server authorization |
| Vertaalfout in belastingtekst | Locale/regressie en finance review |
| Modulewensen verdwijnen/vervuilen scope | Verplicht module-README, index, reviewer en traceability |

## v0.8 — ERP Integration Hub additionele risicoscenario's (GEPLAND)
| ID | Risico | Initiële kans×impact (indicatief) | Controle |
|---|---|---|---|
| ERP01 | ERP en AI Finance genereren beide dezelfde journaalpost | 4×5=20 | Eén financial posting authority per source event, DB uniqueness + reconciliatie |
| ERP02 | Verkeerde entiteit/administratie wordt geboekt | 3×5=15 | Server-side routing en fail-closed entity mapping |
| ERP03 | Verloren/duplicaat/vertraagde events | 4×4=16 | Inbox/outbox, retries, idempotency, sequence reconciliation |
| ERP04 | ERP API-token heeft overmatige rechten | 3×5=15 | Least privilege, secret vault, MFA en tokenrotation |
| ERP05 | Verkeerde mapping of btw/currency conversie | 4×4=16 | Golden tests, dry-run, review, versiebeheer |
| ERP06 | Finance schrijft operationele voorraad terug | 3×4=12 | Outbound allowlist alleen status, ERP als inventory owner |
| ERP07 | ERP-event manipuleert AI of boekingen | 4×5=20 | Untrusted input, typed validation, tool isolation |
| ERP08 | Legacy migration en ERP sync creëren dubbel ingestroomde transacties | 3×5=15 | Gescheiden connector modes, correlation/dedup, cutoverplan |

Scores zijn ontwerpinschattingen; valideer via synthetische integration- en adversarial-tests. Restrisico pas accepteren met onafhankelijke finance/security signoff.

## v0.9 — nieuwe risico's en minimale beheersmaatregelen
| ID | Scenario | Beheersing | Verantwoordelijk |
|---|---|---|---|
| R-tenant-01 | Host-header spoofing routeert verzoek naar vreemde tenant | Allowlisted hostresolver, expliciete membershipcheck, deny-unknown host, E2E tests | Security architect |
| R-tenant-02 | Cross-tenant lek via cache, AI-context of Excel-export | Tenant-keyed caches en scoped retrieval/exports, review + red-team | Security/Privacy |
| R-tenant-03 | Subdomain takeover/recycled slug | Atomaire reservering, DNS/TLS checks, quarantine lifecycle, alerts | Platform ops |
| R-tenant-04 | Onvolledig geprovisioneerde omgeving krijgt toegang | Idempotente saga + gated ACTIVE status, fail-closed en rollback | Platform architect |
| R-erp-05 | Quota 750/h uitgeput, financieel event ontbreekt | Quota headers monitoren, queue, backpressure, gap/replay en reconciliation | Integration owner |
| R-demo-06 | Synthetische demo komt terecht in productie of extern ERP | Aparte namespace/tenant, hard egress-deny en test fixtures | Security/Finance |

Restrisico's moeten later gekwantificeerd en onafhankelijk geaccordeerd worden; dit zijn ontwerpchecks, geen certificering.

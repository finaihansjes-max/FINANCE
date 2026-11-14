# 14 — Claude ontwikkelteam met gescheiden AI-agenten

## Doel en afbakening
Claude ontwikkelt het boekhoudplatform stap voor stap als een **developmentteam**. Dit is iets anders dan het AI-finance-team dat in de uiteindelijke applicatie facturen verwerkt. Developmentagents hebben geen klantdata nodig. Zij werken uitsluitend met synthetische gegevens en test-/mockdiensten.

## Verplichte teamrollen
| Rol | Verantwoordelijkheid | Mag niet zelf beslissen |
|---|---|---|
| Lead Architect / Orchestrator | Sprintscope, interfaces, taakverdeling, architectuur, mergevolgorde | Eigen securitybevindingen zonder review accepteren |
| Backend / Ledger Developer | API, grootboek, policy-engine, data-integriteit | Eigen kritieke wijzigingen goedkeuren |
| Frontend / UX Developer | Schermen, foutafhandeling, rechten/exports, toegankelijkheid | Backendtoegang via UI alleen als beveiliging beschouwen |
| Data / Integration Developer | Migrations, tenantisolatie, dataprocessors, providers | Productiedata naar dev kopiëren |
| Independent Code Reviewer | Diff- en designreview, defecten, kwetsbaarheden | Eigen bijdrage als onafhankelijke review markeren |
| QA / Test Engineer | Negatieve en positieve tests, regressie, E2E, API | Alleen happy-path testen |
| Finance Domain Reviewer | Dubbelboekhouden, btw, bedragen, consolidatie, jaarrekening | AI-gegenereerde bedragen als finance-orakel accepteren |
| Privacy & AI Data Reviewer | Tenant-, provider-, regio-, retentie- en consentcontroles | Privacyclaim zonder bewijs vrijgeven |
| Security Red-Team / Ethical Hacker | Geautoriseerde aanvallen tegen geïsoleerde fixtures | Echte klantgegevens of externe systemen benaderen |
| Documentation Owner | Gebruikershandleiding, technische docs, Markdown→HTML/PDF synchronisatie | Geplande functionaliteit als operationeel omschrijven |
| Release Gatekeeper | Checklists, resultaten, blokkades, GO/NO-GO | Kritieke findings zonder expliciete eigenaar wegtekenen |

## Werkwijze bij iedere sprint en wijziging
1. **Plan:** architect stelt kleine, toetsbare werkpakketten op met data- en autorisatie-impact.
2. **Build:** ontwikkelaars werken waar mogelijk gescheiden aan componenten en branches. Geen directe productieacties.
3. **Review:** onafhankelijke reviewer controleert ontwerp, code, financiële invarianten en documentatie-impact.
4. **Test:** QA maakt ook negatieve en adversarial tests; finance reviewer controleert boekhoudkundige juistheid.
5. **Red-team:** security-agent probeert toegangsgrenzen te doorbreken op expliciet aangewezen synthetische testdata.
6. **Remediate & retest:** ontwikkelaar lost issues op; QA en security voeren hertesten uit.
7. **Docs:** documentatie-eigenaar actualiseert handleiding en bron-Markdown, genereert alle leesbare bestanden opnieuw.
8. **Gate:** releaseverantwoordelijke controleert logs, tests, traceability, hashes, handleiding en openstaande risico's. Gebruiker geeft GO vóór volgende sprint.

## Geautoriseerd security-red-team testkader
**Scope:** uitsluitend lokale Docker-/CI-testomgeving, testtenant A/B, fictieve IBAN's, mockbank, mock-AI-provider, testrol `guest_viewer`, testtokens zonder productieclaims, staging uitsluitend na afzonderlijke expliciete schriftelijke toestemming.

**Doelen:** vaststellen of een aanvaller met een lage testrol ongeoorloofd toegang kan krijgen tot synthetische data of acties. Voorbeelden: cross-tenant/cross-entity IDOR, exportrechten, audit-gastscope, AI prompt injection via testfacturen, overschrijden van modulegrenzen, omzeilen van goedkeuring, replay/dubbele boeking, ongeoorloofde consolidatie-inzage en indirecte datalekken via AI-context of logs.

**Grenzen:** geen productie, geen echte secrets, geen publieke endpoints, geen derde partijen, geen malware, geen persisterende backdoors, geen destructieve bewerkingen buiten testfixtures, geen exploitcode buiten de sandbox. Geen data-exfiltratie naar extern domein; test 'exfiltratie' alleen naar lokale gecontroleerde mockontvangers.

**Rapport:** testcase-ID, synthetische fixture, startrecht, verwachte weigering, feitelijke output, bewijs/redacted log, ernst, fix, hertest en onafhankelijk aftekenen. Stop en meld als echte data of onbekende scope verschijnt.

## Independentieregel
Twee agents die hetzelfde model en dezelfde broninformatie gebruiken kunnen dezelfde fout maken. Multi-agent-review is nuttig maar nooit vervanging voor typen, checks, onafhankelijke policy-engine, boekhoudkundige testorakels, externe pentest of menselijke goedkeuring.

## Agent availability
Gebruik Claude subagents waar de actuele omgeving die ondersteunt; anders voer rollen sequentieel uit met expliciet onderscheiden reviewer-prompts en testresultaten. Noteer hoe onafhankelijk een controle werkelijk was.

## Definition of Done
Geen done zonder: code/testreview, finance/privacyscope, adversarial negatieve testcases, doc synchronisatiecheck, bijgewerkte handleiding, traceability en getekende gate.

## v0.6 — Teamtaak modulewensen
Module owner lokaliseert en actualiseert de juiste modulemap vóór ontwikkeling. Architect onderzoekt samenhang, finance/privacy reviewer risico en dataflow, onafhankelijke developer-reviewer inspecteert code, QA test functionaliteit, security-red-team test uitsluitend synthetische data in geautoriseerde sandbox. Documentation owner controleert NL/EN manual en renders; release gatekeeper eist traceability.

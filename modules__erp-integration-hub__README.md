# Module: ERP Integration Hub

**Status:** GEPLAND  
**Type:** opt-in, permanent, los van migratie en 60-daagse parallel run  
**Module ID:** `erp-integration-hub`

## Doel
Operationeel ERP is standaard bron voor orders, voorraadbewegingen en de facturen die daar ontstaan; AI Finance is de financiële verwerker en rapportagebron wanneer de ingestelde financiële boekingsmodus dat voorschrijft. Financial events lopen hoofdzakelijk **ERP → AI Finance** met beperkt configureerbaar statusverkeer terug. Zie de volledige specificatie `docs/27_ERP_INTEGRATION_HUB.md`.

## Invariants
- Per entiteit, administratie en objecttype kan de eigenaar/richting/capability/boekingmodus worden ingesteld; geen vast schema of gedwongen bidirectionele mutaties.
- Eén financiële boekingsautoriteit per event: importeer óf ERP-journalen óf bereken boekingen uit ERP-bronfacturen; nooit beide.
- ERP is leidend voor fysiek voorraadbeheer; AI Finance wijzigt geen ERP-voorraad.
- Betrouwbare eventverwerking met typed mapping, idempotency, replay, audit en reconciliatie; onbekende events in quarantaine.
- Rechten tenant/entiteit/administratie, SoD, human review voor kritieke wijzigingen; AI maximaal voorstellen, niet rechtstreeks boekingen of API-mutaties uitvoeren.
- UI voor verbinding, connectorcapabilities, monitoring, eigenaarschap, mappings, fouten, data-export, transparantie in NL/EN.
- Permanente koppeling is anders dan tijdelijke Controlled Parallel Migration; samenloop expliciet coördineren.

## Nieuwe wensen / backlog
<!-- Claude: noteer toekomstige wensen met datum, intentie, prioriteit, status, acceptatiecriteria. -->
- Gepland: generiek connectorplatform en eerste connector pas na technisch en privacy-capability assessment.

## Acceptatiecriteria
Alle checks uit `docs/27_ERP_INTEGRATION_HUB.md` plus negatieve tenant- en entiteitsgrenzen, malafide payloads, ontbrekende gebeurtenissen, duplicate journal events, onvoldoende API-rechten en export-/AI-dataveiligheid. Onafhankelijke finance reviewer en security reviewer tekenen af. Handleidingen en leesbare documenten moeten synchroon blijven.

## v0.9 — IT's Perfect API v2 voorbeeldadapter
Gebruik `docs/29_ITS_PERFECT_REFERENCE_CONNECTOR.md`. Behandel de 750 requests/uur als voorlopig leverancierquotum voor gecombineerde client/server calls; consumeer response headers voor resterend budget en reset. Gebruik server-side authenticatie, limit-aware queues, delta/pagination alleen als daadwerkelijk door endpoint ondersteund, audit en reconciliatie. Connector blijft opt-in en vendor-neutraal. Onbekende API-capabilities zijn OPEN.

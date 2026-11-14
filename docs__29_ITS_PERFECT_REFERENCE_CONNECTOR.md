# 29 — IT's Perfect API v2 referentieconnector

**Status: GEPLAND; verificatie per endpoint vereist.** Bron: https://www.itsperfect-solutions.com/api-v2 . Dit is een concreet referentievoorbeeld voor de vendor-neutrale permanente ERP Integration Hub, geen garantie van alle benodigde mutatie-/filtermogelijkheden.

## Ontwerpcontract
- ERP leidend voor operationele facturen, orders en fysieke voorraadbewegingen. AI Finance ontvangt genormaliseerde bron-events en wordt afhankelijk van gekozen `financial_posting_mode` financiële posting owner of importeert ERP-journals; nooit dubbele journaalpost.
- Connectoradapter scheidt authenticatie, veldmapping, rate limit, capabilities, polling/cursors, validatie, replay, audit en reconciliatie van financiële kern.
- Per endpoint leg vast: beschikbare velden, paginering, incremental filter/cursor (alleen indien echt ondersteund), wijzigings- en delete-semantiek, webhookopties, terugschrijfmogelijkheden, rate-limitbucket, rechten, tenant-/entiteitmapping, dataclassificatie.
- Financiële API-key nooit in browser-URL, analytics, error reporting, logs of documentatie; als legacy API tokens in queryparameters verlangt, roep uitsluitend server-side aan en redigeer querystrings in proxies en trace logs.

## Gebruikslimiet (voorlopig op basis van gedeelde documentatie)
- API v2 noemt **750 requests per uur**, totaal van client- en server-side calls. Response headers: `X-Request-Limit-Date` (eind limietvenster), `X-Request-Limit-Remaining` (resterende requests).
- Dit zijn geen gegarandeerde burst-capaciteit of gegarandeerde limieten per tenant/token; bevestig exacte pool-scope, resetformaat en eventuele tariefverhoging met leverancier.
- Plan centraal een budget per externe rate-limitpool (initieel conservatief richtbudget 500/h als configuratievoorbeeld, geen leverancierseis). Reserveer marge voor andere API-verbruikers, herstel en kritieke transacties.
- Queue, backpressure, paginering en incremental sync waar ondersteund, polling-cadans per recordtype, deduplicatie, exponential backoff met jitter; respecteer headers; bij quota-uitputting geen verlies, wel zichtbare achterstand.
- Voorafgaande **capability assessment** moet bepalen of voldoende mutatiegegevens via API beschikbaar zijn en of een afgesproken synchronisatie-SLA haalbaar is. Geen verzonnen exacte endpoints of terugschrijfrechten.

## Acceptatiecriteria
- 750/h-simulatie plus gelijktijdig externe callers: geen onnodige limietoverschrijding; scheduler houdt backlog en herstelt na reset.
- Onjuiste/missende rate-limitheaders veilig afvangen; geen hardcoded planning die altijd lukt claimt.
- Duplicate/out-of-order events, endpointpaginering, incomplete records, foutieve entiteitskoppeling en ERP-downtime: financiële integriteit blijft intact.
- Tenant ziet datastroom en connectorcapabilities; gemigreerde en live ERP-data worden niet zonder afzonderlijke AI-toestemming extern verwerkt.
- Connector werkt met synthetische fixtures en contracttests vóór echte tokens. Een bekende bank-/voorraad- of journalflow krijgt expliciet één posting authority.

# 18 — Internationalisatie NL/EN

## Status en scope
**GEPLAND**. Dit document beschrijft eisen en acceptatievoorwaarden, niet een reeds werkende implementatie. Bij iedere wijziging: privacy-impact, rechten, finance-controls, geautomatiseerde tests, handleidingen NL/EN, document-rendering en reviewersignoff bijwerken. Alle voorbeelden gebruiken synthetische testdata.

## Vereisten
- Nederlands en Engels vanaf het fundament, vertaalsleutels voor UI, validations, workflow, notificaties, AI-context, PDF/Excel-rapporten en Guided Learning.
- Gebruikerstaal en organisatiestandaard apart; fallback `nl-NL`/`en-GB` zonder ontbrekende vertalingen. Admin kan bedrijfsbegrippen per tenant aanpassen zonder fiscale betekenis te wijzigen.
- ISO 4217 valuta, ISO datums in opslag, locale-formattering in presentatie, EU-tijdzones en gescheiden boekdatum/uitvoer-timestamp.
- Geen systeemtekst hardcoded; vertaalcatalogus met reviewproces, pluralisatie, toegankelijkheid en screenshot regressietests. Vertaalde fiscale termen worden juridisch nagekeken.
- Gebruikers- en beheerdershandleiding voor beide talen, gegenereerde leesbare versies synchroon; één taalbron per gespecificeerde versie en traceerbare vertaling.

## Acceptatie
- Elke schermroute en foutmelding heeft NL en EN en dezelfde business-identificaties.
- Taalwissel verandert geen waarden, rechten of boekingen en introduceert geen verschillende fiscale uitkomsten.
- Exportheader-taal is expliciet configureerbaar, gegevenstypes blijven numeriek/datum.

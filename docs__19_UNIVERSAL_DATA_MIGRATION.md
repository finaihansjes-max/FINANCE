# 19 — Universele datamigratie

## Status en scope
**GEPLAND**. Dit document beschrijft eisen en acceptatievoorwaarden, niet een reeds werkende implementatie. Bij iedere wijziging: privacy-impact, rechten, finance-controls, geautomatiseerde tests, handleidingen NL/EN, document-rendering en reviewersignoff bijwerken. Alle voorbeelden gebruiken synthetische testdata.

## Connector architectuur
- Adapterinterface voor API, CSV, XLSX, XML/UBL en auditfiles; Exact Online **eerste onderzochte adapter**, geen gegarandeerde volledige API-dekking. Ook andere leveranciers per connector-capability matrix (read/write objecten, rate limits, authenticatie, historische mutaties, documenten en scopes).
- Import via beveiligde staging met originele payload/hash, provenance, normalisatie, canoniek model, veldmapping GUI, transforms, validatieregels, mappingversies, dry run en expliciet commitbesluit.
- Per tenant + juridische entiteit + administratie + boekjaar scope; behoud legacy IDs, verwijzingen, documentbewijs, boekingshistorie en originele boekdatum.
- Migratievolgorde: entiteiten, rekeningschema, dimensies, stamgegevens, openingsbalans/historie, open posten, transacties, documenten en aansluitingen. Dubbele import idempotent; geen silent merge.
- Reconciliatie o.a. proef-/saldibalans, bank, open posten, btw, subledgers, aantal records, bedragen, debet/credit; markeer unsupported data expliciet.
- Client kan regels en mappings aanpassen, maar wijzigingen vereisen audit en revalidatie; AI doet uitsluitend voorstellen, beslissingen via control engine + goedkeuring.

## Veiligheid en privacy
- Verbinding via least-privilege OAuth waar ondersteund; secrets in vault; encryptie, logging zonder persoonsgegevens, retentionlimieten en verwijderprocedure staging.
- Externe AI standaard uit voor migratie; geen echte data naar development, trainingen of onbevestigde providers.
- Klant krijgt rapport: welke velden, bestanden, hoeveel records, uitzonderingen, welke externe systemen en eventuele AI-verwerking.

## Acceptatie
- Re-import is veilig en idempotent; corrupte en gedeeltelijke records worden geïsoleerd.
- Geen automatisme dat gegevens aanpast vóór financiële aansluitingen en goedkeuring.
- Connector-capability matrix toont objecten die niet kunnen worden teruggeschreven.

## v0.8 — Permanente ERP-koppeling is geen migratie
Deze migratielaag importereert en reconcilieert een bestaande boekhouding voor overstap. Permanente ERP Integration Hub beheert doorlopende operationele ERP-events. Houd namespaces, bron-ID's, idempotency, tokenrechten, ownership en financial posting modes gescheiden. Bij overlap, dedup/cutover-contract vastleggen vóór activering; zie `docs/27_ERP_INTEGRATION_HUB.md`.

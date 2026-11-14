# 20 — Controlled Parallel Migration en Sync Center

## Status en scope
**GEPLAND**. Dit document beschrijft eisen en acceptatievoorwaarden, niet een reeds werkende implementatie. Bij iedere wijziging: privacy-impact, rechten, finance-controls, geautomatiseerde tests, handleidingen NL/EN, document-rendering en reviewersignoff bijwerken. Alle voorbeelden gebruiken synthetische testdata.

## Contract van de proefperiode
- Configureerbare standaard 60 dagen, eerder opzeggen of definitief overstappen mogelijk; **eigen platform is de enige actieve schrijfbron** na cutover. Legacy pakket zo mogelijk technisch read-only; waar niet afdwingbaar: detectie van externe edits en automatische blokkade/escalatie.
- Eenrichtingssynchronisatie van ons platform naar oud systeem, **geen algemene bi-directionele mutatiesynchronisatie**. Verificatie van connector-write-capabilities vóór start; onbekende of unsupported functies blokkeren claim 'veilig terug'.
- Cutover heeft precieze tijd, bron-snapshot, beide saldibalansen, gegevensinventaris, capability check, rechtenbevriezing, initial import en expliciete menselijke goedkeuring.
- Events via transactional outbox, immutable source IDs, idempotency keys, retries met dead-letter queue, watermarks, rate limiting, replay en backpressure.
- Geen destructieve overschrijving van geboekte historische journaalregels. Legacy mapping van correcties/reversals moet worden getoetst.

## Migration & Parallel Run Center
Toon importvoortgang, status/scope connector, laatste geslaagde sync, uitstaande jobs, fouten, retries, blokkades, ongesynchroniseerde objecten, saldo- en subledger-reconciliaties, afwijkingen, auditbewijs en expliciete `rollback_ready`-status. '100% sync' alleen als alle *vereiste* objecten aantoonbaar zijn aangesloten; overige uitsluitingen apart benoemen.

## Vroegtijdige terugkeer
- Nieuwe boekingen kort blokkeren; pending jobs afhandelen; final reconciliatie; export/overdracht van unsupported data; audit trail; menselijke handoff en terugvalgoedkeuring; oude pakket pas daarna schrijvend heropenen.
- Als rollback niet volledig mogelijk is: zichtbaar 'niet terugkeergereed', herstelplan en menselijke besluitvorming. Nooit automatische belofte van geen dataverlies.
- Tijdens proefperiode historische snapshots, data lineage en continuïteitsplan; controle op fiscale gevolgen/period locks.

## Acceptatie
- Legacy-edit detectie, verloren API-token, 429/5xx, retry, volgorde, dubbele events, gedeeltelijke writes en vroegtijdige rollback getest op synthetische data.
- Grootboek, bank, open posten en btw blijven aantoonbaar aansluiten of status is geblokkeerd.

## v0.8 — ERP-integratie vs legacy parallel run
De 60-daagse proefperiode gebruikt AI Finance als actieve bron en synchroniseert voor terugval naar het oude boekhoudpakket. Permanente ERP-integratie ontvangt doorgaans events uit operationeel ERP en stuurt hoogstens verwerkings-/betalingsstatus terug. Beide connectors mogen parallel bestaan alleen met expliciete source/event ownership, dedup en reconciliatie, zodat geen dubbele boekingen ontstaan. Zie `docs/27_ERP_INTEGRATION_HUB.md`.

# Sprint 30 — ERP Integration Hub (permanente ERP-koppeling)

**STATUS: GEPLAND; begin alleen na expliciete GO.** Afhankelijk van connectorfundament (Sprint 23), tenant/entity/IAM (01/03), ledger (04), finance approvals (08), debiteuren/crediteuren (09/12) en waar van toepassing intercompany (14). Architectuurcontracten moeten vanaf het begin worden meegenomen.

## Lees eerst
`CLAUDE.md`, `docs/27_ERP_INTEGRATION_HUB.md`, `modules/erp-integration-hub/README.md`, `modules/integraties/README.md`, `docs/19_UNIVERSAL_DATA_MIGRATION.md`, `docs/20_PARALLEL_RUN_SYNC.md`, `docs/04_DATA_GOVERNANCE_SECURITY.md`, `docs/05_RISK_REGISTER.md`, beide NL/EN-handleidingen en `docs/14_MULTI_AGENT_DEVELOPMENT_TEAM.md`.

## Taken
1. Ontwerp getypeerde per-tenant/entiteit/administratie ERP-connectors inclusief capability registry, encrypted API credentials en field mapping schema's.
2. Voeg `system_of_record`, `sync_direction`, `financial_posting_mode` en `source_event_fingerprint` toe; maak ERP-journal-import en Finance-generates-journal **wederzijds uitsluitend**.
3. Implementeer betrouwbare ERP → AI Finance eventinname via inbox/outbox/queues, validatie, dedup, ordering, retries, replay, dead-letter en audit.
4. Bouw alleen expliciet toegestane Finance → ERP-statuscallbacks; geen factuur-, artikel- of voorraadwriteback zonder nieuw goedgekeurd ontwerp.
5. Bouw integratiedashboard (NL/EN), export, mapping-dryrun, tenantgescopeerde logs, reconciliatie en monitoring.
6. Test met synthetische ERP fixtures: dubbele factuur/webhook, ERP-journalmodus, ontbrekende goederenontvangst, voorraadtransfer intercompany, verkeerd tenant-ID, verkeerde valuta/btw en misbruik van API-toegang.
7. Laat onafhankelijke reviewer, finance reviewer, privacy reviewer en security-red-team testen in geautoriseerde lokale testomgeving. Werk guide-/ops-handleidingen en documentatie synchroon bij en voer `python scripts/build_readable.py --check` uit.

## Exit-gate
Geen dubbele financiële boeking per bron-event, geen mutatie van ERP-voorraad, aantoonbare statuscallback allowlist, gebruikersrechten/AI-consent afgedwongen, reconciliatie/exception-queue controleerbaar, betrouwbare integratietests en NL/EN handleidingen afgestemd op de werkelijk gebouwde code. Geen live ERP-API's of klantdata testen zonder afzonderlijke autorisatie en contractuele toetsing. Rapporteer GO/NO-GO en stop.

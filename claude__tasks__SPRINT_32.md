# Sprint 32 — IT's Perfect API v2 connector proof of concept

**Status: GEPLAND; afhankelijk van ERP Hub Sprint 30 en tenant foundation Sprint 31.**

1. Lees `docs/27_ERP_INTEGRATION_HUB.md` en `docs/29_ITS_PERFECT_REFERENCE_CONNECTOR.md`; inventariseer endpointcapabilities in officiële documentatie, markeer onzekere velden en rate-limit scope als OPEN.
2. Implementeer uitsluitend synthetische connector-mocks/contracttests zonder echte credentials totdat integratiecontract, scopes en privacy zijn goedgekeurd.
3. Test queue, headers, quota 750/h, retry, gap detection en reconciliation. Geen dubbele boekingen.
4. Demonstratie van tenant/entiteit gescheiden mappings en correcte posting authority, beperkte statuscallbacks alleen indien ondersteund.
5. Check redactie van tokens in URL, gatewaylogs en foutregistratie. Security review en docs synchroon; stop bij GO-gate.

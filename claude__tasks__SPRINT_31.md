# Sprint 31 — SaaS tenant provisioning & subdomain security gate

**Status: GEPLAND.** Vereiste bouwstappen en contracten uit `docs/28_SAAS_TENANT_SUBDOMAINS.md` en `modules/saas-tenant-domains/README.md`. Integreer dit in Sprint 00–02 voordat een live multi-tenant launch plaatsvindt.

## Implementatie
1. Model tenant, tenant domain, memberships, rollen, status en audit. Default wildcard-subdomain router; géén custom domain noodzaak.
2. Beveilig subdomainreservering met uniqueness, canonicalization, anti-enumeration, reserved names en domain takeover-beleid.
3. Edge/ingress hostname-verificatie; sessies bind aan geldige tenantmembership; per object en per worker RLS/ABAC defense-in-depth.
4. Provisioning saga: DNS/TLS + opslag/IAM + basisadministratieopties + profielen + secure defaults; heldere retry/rollback zonder half geactiveerde tenants.
5. UI onboarding in NL/EN met modulekeuze, demo of migratie; losse AI-toestemmingen en Guided Learning paid entitlement.
6. Lifecycle voor suspension/offboarding, data-export, retention en support break-glass.

## Mandatory adversarial acceptance
- Cross-tenant via verwisselde host/header, API query, Excel, AI-context, cache, documenten, signed URLs, background jobs, accountantgast en websocket; allemaal geweigerd.
- Parallelle registratie van hetzelfde subdomein geeft exact één eigenaar.
- TLS/provisioning fail-closed; terugzetten en stoppen zijn auditbaar; nooit ongezien data verliezen.
- Finance/privacy/security onafhankelijke reviews; documentatie/handleidingen NL/EN synchroniseren; Markdown→HTML/PDF `--check` groen.
- Geen live self-registration of klantdata totdat alle gates aantoonbaar slagen. Stop en vraag GO voor volgende sprint.

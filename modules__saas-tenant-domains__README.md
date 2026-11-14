# Module: SaaS Tenant & Subdomain Management

**Status:** GEPLAND  
**Type:** verplichte platformkern (niet opt-out)  
**Keuze:** elk bedrijf krijgt standaard een uniek subdomein van ons platform; eigen custom domain is geen vereiste.

## Verplichte functionaliteit
- Automatische registratie en gecontroleerde provisioning van unieke company subdomains; DNS/TLS, slug-reservering en lifecycle states.
- Een tenant heeft eigen identity-memberships, rollen, modules, entiteiten, administraties, data, AI-consent en integraties.
- Tenantadmin kan uitnodigen en bevoegdheden toewijzen; externe accountant blijft purpose-/tijdgebonden gast.
- Een persoon kan tot meerdere tenants behoren zonder dat dat een mogelijkheid geeft data te vermengen.
- Hostname niet voldoende voor beveiliging: server-side tenancy, sterke scope checks op API, DB, storage, search, AI, exports en jobs.

## Nieuwe wensen / backlog
- Centrale platformdirectory en self-service registratie.
- Domeinnaam/branding en custom domains uitsluitend als latere afzonderlijk goedgekeurde wens.

## Acceptatie
Zie `docs/28_SAAS_TENANT_SUBDOMAINS.md` en `claude/tasks/SPRINT_31.md`. Red-team toegangspogingen tot uitsluitend synthetische cross-tenant testdata, privacy-/finance-review, documentatie en NL/EN guides verplicht.

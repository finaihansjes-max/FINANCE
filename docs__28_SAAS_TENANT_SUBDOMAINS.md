# 28 — SaaS Tenant & Subdomain Management (v0.9)

**Status: GEPLAND. Besluit opdrachtgever:** optie 1 is de standaard: **ieder bedrijf krijgt automatisch een uniek subdomein op het platformdomein**, bijvoorbeeld `retailbedrijf.aifinance.nl`. De domeinnaam is illustratief; geen geregistreerd domein of live dienst wordt hiermee geclaimd. Een eigen custom domain is **geen launchvereiste**.

## Scope en datamodel
- `Tenant` = contractuele klantomgeving / security boundary; bevat `tenant_id`, status, gekozen subdomain, profiel, module-entitlements, AI-databeleid, locale defaults, plan en lifecycle timestamps.
- `LegalEntity` = afzonderlijke juridische entiteit binnen tenant; `Administration` = grootboekadministratie; `BusinessUnit` = optionele operationele structuur; `ConsolidationGroup` = expliciet geautoriseerde groepsrapportages.
- `Identity` kan uitgenodigd worden voor meerdere tenants; alle autorisaties worden opnieuw bepaald **per gekozen tenant en administratie**. Geen globale rol die andere tenants doorsnijdt.
- `TenantDomain` bevat genormaliseerde hostname, unieke constraint, challenge/ownership-status waar nodig, TLS-status en DNS-provisioningstatus. Default slechts één platform-subdomein, met eventuele aliases uitsluitend via gecontroleerd beleidsbesluit.
- `TenantMembership` koppelt identity aan tenant met rol, status, expiratie en MFA-beleid; toegang tot entiteiten/administraties via aparte ABAC-scope grants.

## Aanmelden en provisioning
1. Bevestig bedrijfsregistratie, bevoegde eerste beheerder, uniek subdomein en noodzakelijke overeenkomsten; reservering is atomair en conflictvrij.
2. Normaliseer slugs (lowercase ASCII/punycode waar passend), blokkeer reserved words zoals `admin`, `api`, `www`, `support`, `login` en verwarrende/homografische namen; voorkom account-enumeratie.
3. Provision tenant in een idempotente workflow: database tenant-scope, policies/rolbasis, opslagruimte, moduleprofiel, veilige defaults, MFA, audit, observability en TLS via gecontroleerde wildcard- of managed certs.
4. Markeer omgeving pas `ACTIVE` als DNS/TLS, beleidsregels, health checks en negatieve isolatietests slagen. Foutpad = `PROVISIONING_FAILED` en herstelbaar; nooit half-ingeschakelde tenant.
5. Klant kan kiezen voor lege administratie, gecontroleerde migratie of volledig gescheiden gepersonaliseerde demo/sandbox. Profieltemplates zijn aanbevelingen, geen modulebeperkingen.

## Routing en isolatie (niet te omzeilen)
- Edge accepteert uitsluitend gevalideerde hostnames op de allowlist; bepaal de tenant via een server-side domain registry. Vertrouw geen willekeurige `Host`, `X-Forwarded-Host` of door client aangeleverde `tenant_id`.
- URL is alleen een ingang; echte scope = gevalideerde sessie + tenantmembership + entiteits-/administratierechten + module-entitlement + AI-consent.
- Voor iedere query, searchindex, documentobject, auditlog, cache key, background job, webhook, bestandsdownload en export wordt tenant-isolatie en objectautorisation afgedwongen. PostgreSQL RLS is defense-in-depth, geen exclusieve maatregel.
- Geen financiële data in gedeelde cache of AI-context over tenants heen; secretbeheer en connector tokens per tenant; logs minimaliseren en tenant IDs pseudonimiseren waar nodig.
- Centraal platformbeheer ziet metadata over abonnementen/gezondheid, **geen standaardinzage in financiële records**. Supporttoegang alleen tijdelijk, expliciet geautoriseerd, doelgebonden, MFA, gelogd en onafhankelijk te controleren.
- Navigatie tussen tenants kan met één login, maar iedere switch creëert een expliciet nieuwe scope/context, hercontroleert rollen en wist sessiegebonden cache, geselecteerde administraties en AI-context.

## Lifecycle en privacy
- Staten: `REGISTERING → PROVISIONING → ACTIVE → SUSPENDED → OFFBOARDING → CLOSED`, met veilige retries, audit en toegangsbeperkingen per overgang.
- Bij beëindiging: klantgeautoriseerde export, wettelijke retentie/contractuele bewaartermijnen, verwijderverzoeken, backups en verwerkersafspraken; geen belofte van onmiddellijke volledige verwijdering.
- Domein heruitgifte pas na quarantainebeleid en einde van sessies, links, certificaten en opslagkoppelingen. Maak subdomain takeover-tests verplicht.
- Tenant-instellingen en privacyvoorwaarden duidelijk zichtbaar vóór AI-inschakeling; externe AI uit bij initiële provisioning, geen modeltraining met klantgegevens als ontwerpuitgangspunt.

## Acceptatiecriteria / security-negatieven
1. Gelijktijdige registratie van dezelfde slug: maximaal één eigenaar en consistente foutrespons.
2. Onbekende/vervalste host of forwarded header: verzoek wordt geweigerd, nooit aan verkeerde tenant gekoppeld.
3. Cross-tenant API, Excel-export, AI-zoekopdracht, chat, bijlage, cache, background worker, signed URL en audit-lookup: altijd denied.
4. Multi-tenant gebruiker schakelt van A naar B: geen lekkende UI-, document- of AI-context en uitsluitend bevoegdheden van B.
5. Provisioningfailures, DNS/TLS-fout en tijdelijke tenant-suspension: fail-closed en herstelbaar, zonder scheiding te verliezen.
6. Accountantgast krijgt alleen de expliciete entiteiten/tijdvensters; geen posting, approver of tenantadminrechten.
7. NL/EN UI en handleiding correct; routes werken met links/topnavigatie en alle experience-templates.
8. Externe security-/finance-review plus geïsoleerde synthetische red-team tests. Geen productieclaims zonder bewijs.

## Afhankelijkheden
Vanaf Sprint 00 domein-, tenant- en scopecontract; implementatie in Sprint 01 en onboarding in Sprint 02. `SPRINT_31` is de specifieke hardening-/E2E gate voor platform-subdomeinen; moet vóór externe klantregistraties en pilot afgerond worden.

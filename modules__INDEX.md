# Modules — register en routering van nieuwe wensen

Dit register is het vaste startpunt voor Claude bij **alle latere modulewensen**. Elke module heeft een eigen canoniek `README.md`; zie [governance](../docs/25_MODULE_SPEC_GOVERNANCE.md).

| Module | Type | Specificatie |
|---|---|---|
| Grootboek en journalisering | core | [core-ledger](./core-ledger/README.md) |
| Gebruikers, rollen en autorisaties | core | [iam-governance](./iam-governance/README.md) |
| Inkoopaanvragen en bestellingen | opt-in | [inkoop](./inkoop/README.md) |
| Crediteuren en inkoopfacturen | opt-in | [crediteuren](./crediteuren/README.md) |
| Debiteuren en verkoopfacturen | opt-in | [debiteuren](./debiteuren/README.md) |
| Bank en betalingen | opt-in | [bank-betalingen](./bank-betalingen/README.md) |
| Btw en fiscale aangiften | opt-in | [btw-belastingen](./btw-belastingen/README.md) |
| Rapportages en dashboards | opt-in | [rapportages](./rapportages/README.md) |
| Budgetten en prognoses | opt-in | [budgetten](./budgetten/README.md) |
| Voorraad en waardering | opt-in | [voorraad](./voorraad/README.md) |
| Vaste activa | opt-in | [vaste-activa](./vaste-activa/README.md) |
| Entiteiten en analytische dimensies | core | [entiteiten-dimensies](./entiteiten-dimensies/README.md) |
| Intercompany matching | opt-in | [intercompany](./intercompany/README.md) |
| Consolidatie | opt-in | [consolidatie](./consolidatie/README.md) |
| Jaarrekening opstellen | opt-in | [jaarrekening](./jaarrekening/README.md) |
| Accountantgast en auditworkspace | opt-in | [accountantsportaal](./accountantsportaal/README.md) |
| AI Finance-assistent | opt-in | [ai-assistent](./ai-assistent/README.md) |
| AI Control Center | opt-in | [ai-control-center](./ai-control-center/README.md) |
| Universele datamigratie | opt-in | [migratie](./migratie/README.md) |
| Migration & Parallel Run Center | opt-in | [parallel-run](./parallel-run/README.md) |
| Interface-templates | opt-in | [interface-templates](./interface-templates/README.md) |
| Guided Learning Plugin | opt-in | [guided-learning](./guided-learning/README.md) |
| AI Guided Learning Builder | opt-in | [guided-learning-ai-builder](./guided-learning-ai-builder/README.md) |
| NL/EN internationalisatie | core | [localization](./localization/README.md) |
| Koppelingen en externe APIs | opt-in | [integraties](./integraties/README.md) |

## v0.7 — Design System module
- `design-system` → `modules/design-system/README.md`: centrale stijl/tokens, UI-shell en links-/bovennavigatie; **verplicht gedeeld fundament**.
- `interface-templates` blijft de optionele presentatietemplate-laag, met `design-system` als gedeelde dependency en `docs/26_DESIGN_SYSTEM_AND_NAVIGATION.md` als specificatie.

## Betaalde add-on
`guided-learning` is expliciet een **betaalde opt-in add-on, standaard uit** in alle profieltemplates. `guided-learning-ai-builder` is een optionele afhankelijke uitbreiding die niet zonder een actieve Guided Learning-licentie kan worden geactiveerd. Zie `docs/11_MODULES_AND_PROFILES.md` en `docs/22_GUIDED_LEARNING_PLUGIN.md`.

## v0.8 — Nieuwe opt-in module
| ERP Integration Hub (permanente ERP-gegevensuitwisseling) | opt-in | [erp-integration-hub](./erp-integration-hub/README.md) |

Deze module is **niet** hetzelfde als universele migratie of de 60-daagse parallel run. Zet wijzigingen eerst in `modules/erp-integration-hub/README.md`, daarna architectuur, sprint, tests en NL/EN handleidingen.

## v0.9 — module-eigenaren en nieuwe toekomstige wensen
| SaaS Tenant & Subdomain Management | verplichte kern | [saas-tenant-domains](./saas-tenant-domains/README.md) |
| Gepersonaliseerde Demo Data & Sandbox | toekomstig opt-in | [demo-data-sandbox](./demo-data-sandbox/README.md) |

IT's Perfect v2 is een referentieconnector **binnen** `erp-integration-hub`, niet een aparte concurrerende boekhoudmotor. Requirements eerst registreren in bijbehorende modulemap, dan roadmap, security, NL/EN handleiding en 1-op-1 leesbare bestanden bijwerken.

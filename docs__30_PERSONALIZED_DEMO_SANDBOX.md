# 30 — Gepersonaliseerde Demo Data & Sandbox (voorlopige eisen)

**Status: GEPLAND / later verder specificeren.** Niet verwarren met klantproductieadministratie of gecontroleerde migratieproef.

## Bedoeling
- Bevoegde klant of medewerker kan een **fictieve, geïsoleerde demo-administratie** opbouwen op basis van ondernemingsprofiel (ZZP, klein MKB, etc.), **branche** (retail, e-commerce, wholesale, dienstverlening, horeca, bouw, productie, etc.), fictieve bedrijfsnaam, optionele entiteiten, kostenplaatsen/-dragers, modules en financiële historie.
- Branche beïnvloedt relevante rekeningschema's, product-/dienstcatalogus, klanten/leveranciers, facturen, btw-cases, voorraad, workflow en rapportages. Meerdere branches/entiteiten combineren waar passend.
- Synthese moet interne integriteit behouden: debet=credit, btw-aansluiting, ontvangsten, betalingen, voorraad, open posten, perioden en consolidatie/eliminaties.
- Een demo kan echte **sandbox-mutaties** uitvoeren (afletteren, boeken, approvals en tours), maar geen betaalopdrachten, belastingindieningen of e-mails aan echte ontvangers versturen.
- Demo-data altijd synthetisch; echte klantdata nooit stilzwijgend kopiëren/anonimiseren. Optionele productie-naar-demo transformatie is een **afzonderlijk toekomstig ontwerp**, onder privacyreview.
- Reset, seeded deterministische scenario's, audit, beperkingen op egress en volledig gescheiden opslag/scope. Duidelijke demo-indicator in de UI.
- Standaard moduleprofielen kunnen via opt-in uitgebreid worden; Guided Learning blijft betaalde add-on, eventuele demo-uitzondering alleen via expliciete contractuele proefentitlement.

## Toekomstige specificatiepunten
- Configuratievorm, branchtemplates, historieperiode (bijvoorbeeld 12 maanden), volumeniveaus, rapportagegeneratie, resetpolicy, AI-agentgedrag, aparte tenant versus aparte admin en demo-toegang voor prospects.
- Tot uitwerking: **alleen roadmap/backlog; geen automatische productie-import, geen echte bankkoppelingen**.

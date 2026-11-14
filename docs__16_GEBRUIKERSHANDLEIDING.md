# 16 — Gebruikershandleiding AI Finance Platform

**Versiestatus: ONTWERP / GEPLAND — dit projectpakket bevat specificaties en bouwopdrachten, geen reeds werkend boekhoudproduct.** Deze handleiding beschrijft de beoogde werking en wordt bij *iedere* functionele aanpassing synchroon met de software en overige documentatie bijgewerkt.

## 1. Inloggen, organisatie en profiel (GEPLAND)
Een bevoegde beheerder maakt een organisatie aan, kiest een profieltemplate (bijv. ZZP, klein MKB, middelgroot MKB) en past modules meteen aan. Het profiel bepaalt **alleen standaard voorgeselecteerde modules**, geen beperkende maximale modulelijst. Een ZZP kan direct `Uitgebreide rapportages` inschakelen.

## 2. Modules inschakelen of uitschakelen (GEPLAND)
Bevoegdheid: organisatiebeheerder met modulebeheerrecht. Route: **Instellingen → Modules**. Selecteer modules, bekijk afhankelijkheden, kosten/licenties indien van toepassing en de gevolgen voor bestaande workflows. Bevestig wijziging. Deactiveren wist **geen** historische boekingen of auditlogs. Een module-opt-in is **geen** toestemming voor externe AI-doorgifte.

## 3. Entiteiten en administraties (GEPLAND)
Kies bovenaan een geautoriseerde administratie, bijvoorbeeld Retail of Wholesale. Daar kunnen bevoegde medewerkers boekingen maken. Entiteiten kunnen juridische entiteiten of bedrijfsonderdelen zijn; de exacte structuur wordt door de klant ingesteld.

## 4. Kostenplaatsen, kostendragers en eigen dimensies (GEPLAND)
Beheer optionele dimensietypen en hiërarchieën. Voorbeeld: Wholesale → kostenplaats E-commerce → kostendrager Collectie. Deze volgorde en niveaus zijn niet verplicht en kunnen per bedrijf verschillen.

## 5. Financiële transacties (GEPLAND)
Facturen kunnen worden ontvangen en als concept door AI worden herkend. De onafhankelijke validatielaag controleert btw, boekingsbalans, duplicaten en rechten. Kritieke acties zoals goedkeuren en betalen vragen bevoegde menselijke autorisatie. Aanpassing na goedkeuring maakt die goedkeuring ongeldig.

## 6. Goedkeuringen (GEPLAND)
Bevoegde beheerders configureren beleid op rol, persoon, bedrag, leverancier, project, kostenplaats en risicosignaal. Een gebruiker kan niet zijn eigen vereiste onafhankelijke goedkeuring vervangen. Bij afwezigheid kan gecontroleerde delegatie plaatsvinden.

## 7. Betalingen (GEPLAND)
AI mag betaalvoorstellen voorbereiden maar niet zelfstandig bankbetalingen autoriseren. De betalingsverantwoordelijke controleert bedrag, IBAN, begunstigde, batch en opdrachtversie en gebruikt de externe/afgesproken bankautorisatie.

## 8. Consolidatie en intercompany (GEPLAND)
Kies de geconsolideerde view om geautoriseerde administraties gecombineerd te bekijken. **In de geconsolideerde rapportage kan niet regulier worden geboekt.** Eliminaties gebeuren in een aparte consolidatiewerkruimte met goedkeuring en audittrail; bronboekingen blijven onveranderd.

## 9. Rapportages en Excel-export (GEPLAND)
Iedere dataview heeft een knop **Exporteren** met `.xlsx` en passende filters. De export toont alleen geautoriseerde velden en rijen. Grootschalige exports kunnen extra autorisatie vereisen.

## 10. Jaarrekening (optionele module, GEPLAND)
Kies de opt-in jaarrekeningmodule. Selecteer administratie of consolidatiegroep, boekjaar, rechtsvorm en verslaggevingsgrondslag. Gebruik een bevroren gecontroleerde snapshot, bekijk toelichtingen en laat bevoegde mensen de conceptjaarrekening vrijgeven. **Geen wettelijke controleplicht betekent niet dat geen jaarrekening of deponering vereist is.**

## 11. Accountant als gast (optionele module, GEPLAND)
Een bevoegde beheerder nodigt een accountant uit voor een afgebakende periode en scope. De gast kan standaard alleen de toegekende administraties/documenten bekijken en onderzoeksvragen in de auditwerkruimte stellen. Geen posten, betalen of wijzigen; export en AI-vragen vergen extra afzonderlijke rechten. Toegang kan ingetrokken worden.

## 12. Wat gebeurt er met mijn financiële data? (GEPLAND)
Data worden per klant en geautoriseerde administratie gescheiden. AI ziet alleen toegestane gegevens binnen een doelgebonden taak. Externe AI-verwerking staat standaard **uit** en mag pas starten na expliciete configuratie én controle van leverancier, regio, retentie, contract en toegestane categorieën. Moduleactivering betekent niet automatisch externe AI-toestemming. Geen vrijgave voor modeltraining. De klant krijgt inzicht in AI-provider, doel, regio, categorieën data en verwerkingshistorie zodra deze functies aantoonbaar geïmplementeerd zijn. Er zijn nog **geen productiegaranties** totdat de implementatie is getest en contracten zijn getoetst.

## 13. Afwijkingen en AI Control Center (GEPLAND)
Gebruikers met passende rechten kunnen afwijkingen onderzoeken. AI-adviezen zijn voorstellen. Bekijk bronverwijzingen, risicosignalen, reviewers en goedkeuringstoestand. Menselijke beslissingen blijven zichtbaar en herleidbaar.

## 14. Problemen melden (GEPLAND)
Bij onjuiste bedragen, ongeautoriseerde inzage, een foutieve AI-suggestie of een verdachte betalingswijziging: verwerking stopzetten, incident via de beveiligde meldroute rapporteren, geen klantdata delen via onbeveiligde kanalen. Definitieve menu's, contactpunten en screenshots worden toegevoegd zodra ze bestaan.

## Onderhoudsregel
Bij iedere sprint wijzigen ontwikkelaars de relevante secties, de status, het rechtenmodel, de klikpaden en controles. **Nooit** een nieuwe functie releasen zonder bijgewerkte en door een reviewer tegen de applicatie getoetste handleiding. Render HTML/PDF vanuit exact deze Markdown-bron.

## GEPLAND — Nieuw in v0.6
- Kies Nederlands of Engels onder profielinstellingen; alle schermen en begeleidende teksten volgen deze keuze.
- Via Migratie kunt u bestaand pakket verbinden, mappings beoordelen en de import pas na controle bevestigen.
- In de Migration & Parallel Run Center (60 dagen standaard) werkt u in ons systeem; het oude pakket ontvangt waar mogelijk eenrichtingssynchronisaties en mag niet actief worden bewerkt. U kunt eerder terug, maar alleen na aansluiting en handoff.
- Kies een werkomgeving/template onafhankelijk van uw ZZP/MKB-bedrijfsprofiel.
- Start een vooraf gepubliceerde Guided Learning uit het hulp-overzicht of via AI-chat; de instructies markeren knoppen en laten u zelf echte handelingen uitvoeren. AI stuurt tijdens de walkthrough niets live aan.
- Guided Learning en migratie zijn optionele modules; privacy van externe AI staat apart.
- Alle nieuwe functies hierboven zijn **GEPLAND**.

## PLANNED — English user guidance
- Choose English or Dutch in your user profile.
- Review migration mappings and reconciliation before importing. During the default 60-day trial our platform is the sole writable source and your previous accounting package receives supported one-way synchronization. Early return requires verified reconciliation.
- UI template changes presentation only. Start a published Guided Learning from help or an AI chat suggestion; the deterministic runtime highlights UI and checks real actions without live AI.
- These features are **PLANNED**, not available yet.

## v0.7 — GEPLAND: de werkomgeving aanpassen (Nederlands)
Het standaarddashboard krijgt de lichte AI Finance-stijl met financieel overzicht, KPI-tegels, transacties en AI-assistent. Via `Instellingen > Weergave` kan de gebruiker, indien beschikbaar, kiezen voor **Navigatie links** (standaard) of **Navigatie boven**. Onder **Werkomgeving** kan een gepubliceerde template worden geselecteerd, waaronder AI Finance Default of een herkenbare compatibiliteitspreset. Gebruik **Voorbeeld** vóór **Opslaan**. De keuze beïnvloedt nooit bedragen, goedkeuringen, data of rollen; bestaande Guided Learnings moeten met beide indelingen werken. Een ZZP'er kan dezelfde templates/modules kiezen als een MKB-bedrijf, afhankelijk van expliciete licentie-/beheerinstellingen. Deze instellingen zijn nog **GEPLAND**, geen instructie voor een bestaand product.

## v0.7 — PLANNED: change your workspace (English)
The default dashboard will use the modern, light AI Finance visual style with finance KPIs, transactions and an AI panel. In `Settings > Appearance`, choose **Left navigation** (default) or **Top navigation**, and select a published **Workspace template**. Preview before saving. Changing the layout never changes ledger data, approvals, roles, access permissions or AI data-processing consent. Published Guided Learnings must work in both navigation layouts. This feature is **PLANNED**, not yet implemented.

## Guided Learning aanschaffen en activeren (NL; GEPLAND)
Guided Learning is een **betaalde extra module** en hoort niet bij de standaardboekhouding. Een bevoegde beheerder kan bij **Instellingen > Modules** zien of deze add-on voor de organisatie beschikbaar is, de commerciële voorwaarden bekijken en de module na afzonderlijke afname activeren. Zonder actieve licentie zijn Guided Learnings niet startbaar. Alle gewone boekhoudfuncties en statische uitleg blijven beschikbaar. AI-gegenereerde instructies zijn daarnaast een afzonderlijke optionele mogelijkheid waarvoor passende AI-toestemming nodig is. Er worden geen tarieven genoemd totdat deze officieel zijn vastgesteld.

## Guided Learning subscription and activation (EN; PLANNED)
Guided Learning is a **paid optional add-on**, not included in the standard accounting system. An authorized administrator can review subscription availability under **Settings > Modules** and enable the module only after it has been separately purchased. Without an active entitlement, walkthroughs cannot be launched. Core accounting functions and static help remain available. AI-generated tours additionally require separately configured AI-processing permissions. Pricing will be shown only once contractually defined.

## ERP Integration Hub (NL) — GEPLAND
1. Activeer optioneel de ERP-integratiemodule en laat een bevoegde beheerder de ERP-koppeling autoriseren.
2. Kies per administratie de operationele bron, beschikbare objecten, richting en gekozen financiële boekingsmodus.
3. Bekijk in `Integraties → ERP Integration Hub` de ontvangen ERP-facturen en voorraadgerelateerde financiële mutaties, status, laatst gesynchroniseerde moment, eventuele fouten en afwijkingen.
4. Bij een verschil of ontbrekend event: dien een controleverzoek in; verwerk niet zelf een duplicaat om een waarschuwing weg te werken.
5. In de standaardconfiguratie maakt u orders, voorraadmutaties en ERP-facturen in het ERP; ons platform verzorgt de geconfigureerde financiële verwerking en rapporteert waar mogelijk beperkte statussen terug.

**EN — PLANNED:** Connect an ERP as the operational source; configure entity scope, object ownership, sync direction and posting mode. Check transactions, errors and reconciliation in the ERP Integration Hub. ERP remains the source for operational invoices/orders/inventory. Finance processes permitted accounting entries and can send limited status callbacks. The ERP connector does not imply external AI data processing. Actual availability depends on verified ERP capabilities.

## Gepland — eigen bedrijfs-subdomein en tenantkeuze
Na registratie ontvangt uw bedrijf een uniek platform-subdomein (voorbeeld `uwbedrijf.aifinance.nl`). Via dat adres logt u in, activeert modules en kiest indien van toepassing een juridische entiteit en financiële administratie. Als u voor meerdere bedrijven bent uitgenodigd, schakelt u expliciet tussen toegestane klantomgevingen; de gegevens worden niet samengevoegd buiten geautoriseerde consolidatierapportages. Automatische registratie en toegang via subdomein zijn momenteel **GEPLAND**, niet reeds beschikbaar.

## Gepland — demo en IT's Perfect ERP-koppeling
Branche- en bedrijfsprofielafhankelijke fictieve demo-administraties en IT's Perfect v2 ERP-gegevensuitwisseling zijn toekomstige modules; echte API-capabilities en klantdata-uitwisseling moeten vooraf gecontroleerd worden.

# Wat gebeurt er met uw financiële gegevens als u onze AI-boekhouding gebruikt?

> **CONCEPT voor potentiële klanten — nog niet publiceren als feitelijke belofte.** De beschreven beveiliging en keuzemogelijkheden moeten eerst gebouwd, getest, contractueel geborgd en geverifieerd worden. Pas daarna mogen we formuleringen veranderen van 'we ontwerpen' naar 'we doen'.

## Uw administratie blijft uw administratie
Ons platform wordt ontworpen om uw financiële administratie te verwerken met ondersteuning van AI. Denk aan het uitlezen van inkoopfacturen, het voorstellen van boekingen, het signaleren van dubbele facturen en het voorbereiden van rapportages. Uw medewerkers blijven bevoegd om kritieke handelingen te beoordelen en goed te keuren.

## Waar komen de gegevens terecht?
Uw originele facturen, boekingen en bankgegevens worden opgeslagen in een beveiligde administratie binnen de afgesproken hostingomgeving. Per klant worden toegang en gegevens van andere organisaties afgeschermd. De exacte hostingregio en onderaannemers worden contractueel en in uw klantportaal getoond voordat het systeem live gaat.

## Wat ziet AI precies?
Dat hangt af van de taak. Bij het uitlezen van een factuur kan AI bijvoorbeeld leveranciersnaam, factuurdatum, omschrijving, bedragen, btw en rekeningnummer nodig hebben. Bij een rapportage kan AI vaak volstaan met geaggregeerde cijfers. We ontwerpen het systeem om alleen benodigde informatie aan de betreffende AI-functie te verstrekken. Sommige documenten kunnen ook persoonsgegevens bevatten; daarom zijn extra toegangs- en privacycontroles nodig.

## Gaan mijn gegevens naar ChatGPT of Claude?
**Dat hangt af van uw gekozen AI-verwerkingsmodus en de daadwerkelijk geconfigureerde leveranciers.** Als uw organisatie een externe AI-provider inschakelt, worden de benodigde gegevens voor die specifieke bewerking naar die provider doorgestuurd. Ook als de provider deze gegevens niet gebruikt om modellen te trainen, is dat nog steeds externe verwerking.

We ontwerpen drie mogelijkheden:
1. **Alleen lokale/beheerde AI**: geen documenten naar externe generatieve AI-API's; wel afhankelijkheid van de eigen hostinginfrastructuur en eventuele subverwerkers.
2. **EER-gecontroleerde AI-verwerking**: uitsluitend aantoonbaar goedgekeurde verwerkingsroutes; internationale doorgifte en eventuele externe toegang worden afzonderlijk beoordeeld.
3. **Goedgekeurde externe AI**: expliciet gekozen externe AI-leveranciers op basis van contract, doel, locatie, datacategorie en bewaartermijn.

Een instelling zoals 'EU datacenter' alleen is geen garantie dat gegevens nooit buiten de EER toegankelijk zijn. Dit moet contractueel en technisch worden vastgesteld.

## Wordt mijn administratie gebruikt om AI te trainen?
Het uitgangspunt is **nee**: wij richten het systeem zo in dat financiële klantgegevens niet worden vrijgegeven voor training van externe AI-modellen. We gebruiken alleen zakelijke contracten en configuraties waarin dit is geborgd. Als een leverancier die garantie niet kan geven voor de benodigde functie, wordt die functie niet beschikbaar gesteld voor uw gegevens. Dit geldt pas als de contracten en maatregelen daadwerkelijk zijn geverifieerd.

## Hoe lang worden gegevens bewaard?
- **Uw financiële administratie:** doorgaans minimaal 7 jaar vanwege Nederlandse fiscale verplichtingen; voor bepaalde documenten 10 jaar.
- **Gegevens die tijdelijk door AI verwerkt worden:** afhankelijk van de gebruikte modelprovider, het endpoint, de contractafspraken en eventuele veiligheidsuitzonderingen. Ook als modeltraining uitstaat kan tijdelijk verwerken of loggen plaatsvinden.
- **Onze operationele logs en AI-activiteitenregister:** eigen, geminimaliseerde bewaartermijnen volgens het gepubliceerde privacy- en bewaarbeleid; geen onnodige documentinhoud.

We tonen de per provider geldende afspraken en bewaren niet langer dan nodig is, rekening houdend met wettelijke verplichtingen.

## Kan een AI-agent geld overmaken of mijn boekhouding aanpassen?
Agents mogen administratieve handelingen voorbereiden, documenten interpreteren en voorstellen toetsen. Het systeem wordt ontworpen met onafhankelijke technische controles die de AI zelf niet kan uitschakelen. Bij kritieke handelingen, zoals definitieve betaalautorisatie, IBAN-wijzigingen, fiscale aangiften en belangrijke correcties, is menselijke goedkeuring vereist. Meerdere agents kunnen elkaar controleren, maar een AI-agent mag geen menselijk akkoord vervangen.

## Wie kan mijn gegevens inzien?
Alleen gebruikers en diensten met de juiste, aantoonbaar toegewezen rechten. Uw organisatie kan rollen instellen voor bekijken, bewerken, goedkeuren en exporteren. Platformmedewerkers krijgen niet standaard onbeperkt inzage. Noodzakelijke supporttoegang wordt beperkt, geregistreerd en waar mogelijk vooraf goedgekeurd.

## Kan ik zien welke gegevens naar AI zijn gegaan?
Dat is een expliciete producteis. In het geplande **AI Data Control Panel** kunt u per verwerking bekijken: welk doel, welke AI-leverancier en welk model, welke datacategorieën, welke verwerkingslocatie, wanneer, welke regels en of een externe doorgifte heeft plaatsgevonden. Dit wordt beschikbaar zodra deze functie geïmplementeerd en getest is.

## Kan ik AI uitschakelen, mijn data exporteren of verwijderen?
Het platform wordt ontworpen met AI-uitgeschakelde werkmodus, geautoriseerde exports en verzoeken om gegevens te verwijderen. Wettelijk verplichte financiële stukken kunnen we niet zomaar eerder wissen. Wij moeten ook rekening houden met back-ups, lopende geschillen en andere wettelijke bewaarplichten.

## Welke documenten kan ik als klant opvragen?
Verwerkersovereenkomst, actuele lijst subverwerkers, datastroomoverzicht, beveiligingssamenvatting, afspraken over locaties en bewaartermijnen, incidentprocedure, export- en verwijderbeleid, en audit-/testverklaringen zodra deze bestaan. We claimen geen certificeringen zolang die niet werkelijk zijn behaald.

## Korte tekst voor website of salesgesprek (pas na implementatie valideren)
> AI helpt bij het verwerken en controleren van uw boekhouding, maar beslist niet zelfstandig over kritieke financiële handelingen. U behoudt controle over wie uw data ziet, welke AI-verwerking is toegestaan en welke acties menselijke goedkeuring nodig hebben. Financiële gegevens worden niet ingezet voor modeltraining. Externe AI-verwerking, bewaarafspraken en gebruikte leveranciers maken we transparant.

## Vragen waarop we vóór contractsluiting een concreet antwoord moeten kunnen geven
1. In welk land staan primaire opslag, back-ups en AI-verwerking?
2. Welke subverwerkers zien welke categorieën gegevens?
3. Zijn modeltraining, feedback en veiligheidslogging uitgeschakeld of contractueel beperkt?
4. Welke exacte retentie en uitzonderingen gelden per AI-provider/endpoint?
5. Kan het bedrijf volledig zonder externe AI werken en is dat technisch getest?
6. Hoe worden tenantisolatie, autorisaties, audits en herstelbaarheid onafhankelijk getest?
7. Welke bankhandelingen zijn expliciet alleen voor mensen toegestaan?
8. Wat gebeurt er bij opzegging en wie blijft fiscale gegevens bewaren?

## Bronnen om claims te toetsen
- Belastingdienst: https://www.belastingdienst.nl/wps/wcm/connect/bldcontentnl/belastingdienst/zakelijk/btw/administratie_bijhouden/administratie_bewaren/administratie_bewaren
- Autoriteit Persoonsgegevens — verwerkersovereenkomst: https://autoriteitpersoonsgegevens.nl/themas/basis-avg/avg-algemeen/verwerkersovereenkomst
- Autoriteit Persoonsgegevens — doorgifte buiten EER: https://autoriteitpersoonsgegevens.nl/themas/internationaal/doorgifte-binnen-en-buiten-de-eer/doorgifte-persoonsgegevens-buiten-de-eer
- OpenAI API data controls: https://developers.openai.com/api/docs/guides/your-data
- Claude API retention: https://platform.claude.com/docs/en/build-with-claude/api-and-data-retention

## Welke modules kiest u zelf?
Tijdens de inrichting kiest u een bedrijfsprofiel zoals zelfstandig ondernemer (ZZP) of MKB. Dit is uitsluitend een **startadvies**: u kunt tijdens de inrichting direct extra onderdelen aanvinken, bijvoorbeeld uitgebreide rapportages bij een ZZP-profiel. Ook later kunt u uw modules aanpassen. Beveiligings- en boekhoudkundige basiscontroles kunnen niet worden uitgezet.

**Let op:** een extra module aanzetten is niet hetzelfde als toestemming geven om gegevens naar een externe AI-aanbieder te sturen. U moet afzonderlijk kunnen zien en instellen welke AI-functies welke gegevens verwerken, met welk doel, bij welke aanbieder en in welke regio. Beschikbare AI-verwerkingsmodi hangen af van de uiteindelijk gerealiseerde en gecontracteerde dienstverlening.

## Wat gebeurt er bij meerdere bedrijven of administraties?
Als u bijvoorbeeld Retail en Wholesale heeft, kunnen dit afzonderlijke juridische entiteiten met eigen administraties zijn, of twee bedrijfsonderdelen binnen één entiteit. U krijgt per administratie toegang op basis van uw rechten. Binnen een geautoriseerde consolidatiegroep kunt u ook gecombineerde cijfers **bekijken**, zonder rechtstreeks in dat geconsolideerde overzicht te boeken.

Onderlinge transacties tussen juridische entiteiten kunnen voor een groepsrapportage via gecontroleerde intercompany-eliminaties uit de groepscijfers worden gehaald. De oorspronkelijke boekingen in de afzonderlijke administraties blijven bestaan. Eliminaties worden afzonderlijk geregistreerd en moeten volgens het ingestelde goedkeuringsbeleid worden gecontroleerd.

AI-agenten die een groepsrapportage of intercompany-reconciliatie ondersteunen, krijgen alleen de benodigde gegevens binnen een geautoriseerde scope. Extra agenten kunnen extra gegevensverwerkingsstappen met zich meebrengen; het klantportaal moet daarom inzicht geven in welke gegevens per agent en externe leverancier zijn gedeeld.

**Deze tekst beschrijft het beoogde productontwerp en is geen garantie dat functies al beschikbaar zijn.** Concrete beloften over opslaglocatie, doorgiften, subverwerkers, retentie, training en verwijdering mogen pas worden gepubliceerd na technische en contractuele verificatie.

## v0.3 — concepttekst voor klanten: jaarrekening en accountantinzage
**Zelf jaarrekening maken (optionele module):** als u deze module aanzet, gebruikt het programma de voor uw onderneming toegestane boekhoudgegevens om conceptjaarstukken en controles voor te bereiden. AI kan helpen met concepttoelichtingen en het signaleren van afwijkingen, maar de bedragen komen uit uw administratie. U beoordeelt en bevestigt het eindresultaat. De module bepaalt niet zelfstandig of u een wettelijke controle of deponering nodig hebt.

**Een accountant uitnodigen (optionele module):** u kunt een externe accountant tijdelijk inzage geven in specifiek gekozen administraties, boekjaren en documenten. Deze gast kan informatie beoordelen en vragen stellen, maar niet boeken, betalen of de administratie wijzigen. U beheert de toegang en kunt die intrekken. Downloads zijn afzonderlijk te beperken en worden geregistreerd.

**Wat gebeurt er met uw data bij AI?** Het activeren van deze modules betekent niet dat uw documenten automatisch naar een externe AI-aanbieder gaan. Of externe AI wordt gebruikt, welke gegevens worden gedeeld, met welke aanbieder, in welke regio en met welk bewaarbeleid, moet vóór gebruik inzichtelijk en expliciet geconfigureerd zijn. Gegevens worden niet voor modeltraining vrijgegeven. Claims als 'geen opslag' zijn pas mogelijk indien technisch en contractueel aantoonbaar.

**Let op:** deze klanttekst is een beoogd productbeleid, geen bewezen garantie. Publiceer alleen wat door implementatie, contracten en audits is vastgesteld.

## Nieuwe modules: wat gebeurt er met uw gegevens? (GEPLAND)
Bij migratie haalt het platform via een door u geautoriseerde koppeling de benodigde financiële gegevens uit uw oude boekhoudpakket op. Het systeem bewaart bron- en controlegegevens veilig en geeft in het Migration Center inzicht in uitgewisselde gegevens, fouten en uitzonderingen. Tijdens de proefperiode worden nieuwe mutaties vanuit ons platform naar uw oude pakket teruggestuurd, mits de externe software dit ondersteunt. Het oude pakket hoort in die periode niet meer voor nieuwe boekingen te worden gebruikt. Een veilige terugkeer wordt pas bevestigd na controle en reconciliatie.

Een Guided Learning voert een vooraf opgeslagen interactieve handleiding uit zonder AI-aanroep. AI kan optioneel helpen bij het ontwerpen van zulke handleidingen, waarbij standaard alleen schermmetadata wordt gedeeld. AI-chat gebruikt uitsluitend toegestane context en volgt uw AI-toestemmingsinstellingen. Financiële data wordt niet voor modeltraining vrijgegeven. Concrete claims over regio, retentie en externe AI-verwerkers worden pas gepubliceerd na technisch en contractueel bewijs.

## ERP-integratie: wat gebeurt er met uw gegevens? (GEPLAND)
Wanneer u optioneel uw ERP-systeem koppelt, haalt AI Finance alleen de vooraf geconfigureerde gegevens op die noodzakelijk zijn voor de financiële administratie, zoals facturen, orders en financieel relevante voorraadmutaties. Uw ERP blijft normaal het operationele bronsysteem. AI Finance verwerkt de gegevens op basis van uw geselecteerde administraties en ingestelde rechten; terug naar ERP worden uitsluitend goedgekeurde, beperkte statussen verstuurd, zoals of een factuur verwerkt of betaald is. In het toekomstige integratiedashboard kunt u per koppeling bekijken welke gegevensvelden, doelen, verwerkingslocaties, opslag-/bewaarregels en partijen betrokken zijn. Een ERP-koppeling betekent **niet** automatisch dat uw data aan een extern AI-model wordt gegeven; externe AI-diensten vereisen aparte configuratie en juridische/contractuele waarborgen. Welke concrete ERP-functies en garanties beschikbaar zijn, wordt vooraf per ERP-provider geverifieerd.

## Hoe werkt uw eigen subdomein? (geplande SaaS-dienst)
Elke klantorganisatie krijgt bij registratie standaard een eigen platform-subdomein, bijvoorbeeld `uwbedrijf.aifinance.nl` (voorbeeldnaam). U werkt via uw browser in een afgeschermde tenant met eigen gebruikers, rollen, administraties en modules. Een subdomein is geen fysieke, afzonderlijke server en biedt **op zichzelf geen garantie op dataseparatie**. Ons ontwerp vereist daarom afzonderlijke toegangscontroles bij opslag, API, AI, exports en koppelingen.

Heeft uw organisatie meerdere juridische entiteiten, dan kunnen hun administraties binnen uw tenant afzonderlijk beheerd worden, met passende inzage- en consolidatierechten. Een accountant kan met één account toegang tot expliciet uitgenodigde tenants krijgen, maar ziet nooit automatisch andere klanten. Medewerkers van het SaaS-platform zien niet standaard uw financiële inhoud. De daadwerkelijke beveiligingsimplementatie, hostinglocaties, subverwerkers en privacygaranties moeten vóór commerciële lancering controleerbaar zijn. Een demo is alleen fictief en gescheiden; ERP-koppelingen delen alleen gegevens volgens expliciete instellingen.

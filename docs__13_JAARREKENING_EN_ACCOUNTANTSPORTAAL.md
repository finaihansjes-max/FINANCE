# 13 — Opt-in jaarrekeningmodule en accountantsportaal (v0.3)

> Ontwerpspecificatie, geen claim dat fiscale of accountantsrechtelijke conformité al is gerealiseerd. Toets verslaggevingsmodellen, bedrijfsklasse en toepasselijke verplichtingen met Nederlandse professionals vóór productie.

## 1. Productdoel / afbakening
- **Opt-in `annual_accounts`**: ondernemingen zonder wettelijke controleplicht kunnen zelf een conceptjaarrekening of financiële jaarstukken samenstellen, controleren, formeel vaststellen en waar van toepassing gereedmaken voor digitale deponering. Dit is **niet** beperkt tot ondernemingen zonder controleplicht; ook andere klanten kunnen de module gebruiken als hun verplichtingen en accountantworkflow dit toelaten.
- **Opt-in `external_auditor_portal`**: klanten nodigen een externe accountant of adviseur tijdelijk uit voor een opdrachtgebonden **guest viewer**-rol met lees-/onderzoeksrechten; standaard geen boeking, geen mutatie van ledger, geen betaling, geen AI-autorisatiewijziging en geen uitnodigen van andere gebruikers.
- Onderscheid **interne managementjaarstukken**, **statutaire jaarrekening**, **publicatiestukken** en **fiscale rapportage**. Niet als hetzelfde document presenteren.
- **Geen controleplicht ≠ geen opmaak-/deponeringsplicht**. Een eenmanszaak heeft doorgaans geen KVK-deponeringsplicht voor jaarrekening, een bv kan die wel hebben. Bepaal de wettelijke classificatie per rechtsvorm, financiële omvang, verslagjaar en uitzonderingen; vraag de klant feiten en laat conclusies reviewen.
- Een **zelf opgestelde** jaarrekening of een AI-check heeft **geen accountantsverklaring** en is geen controle-, beoordelings- of samenstellingsopdracht. Claims als 'gecontroleerd door accountant' uitsluitend bij geldig opdrachtresultaat en bevoegd actor.
- Vermijd autonome definitieve verzending/deponering en fictieve compliance. Verifieer actuele SBR/XBRL-taxonomie, authenticatie, kanaal en filing policy. Tot succesvolle, gecontroleerde koppeling: export voor handmatige deponering met duidelijke status.

## 2. Functionaliteit jaarrekening
1. Start jaarrekeningwerkdossier voor `{tenant, legal_entity, administration(s), fiscal_year, accounting_framework, report_type}`; geconsolideerd dossier alleen voor geautoriseerde consolidation group met frozen consolidation run en eigen toelichting.
2. Voorbereiding: afsluitstatus, proef- en saldibalans, vergelijking vorig jaar, balans, winst-en-verliesrekening, toelichting/noten, grondslagen, mutatieoverzichten en bronverwijzingen.
3. Versiebeheerde mapping rekeningschema → rapportagerubrieken; check saldo debet/credit, balansvergelijking, aansluitingen met ledger, intercompany, valuta, jaarafsluiting, grootboekcorrecties, consistentie van toelichtingen, materialiteit en openstaande taken.
4. Juridisch reviewbare templates voor passende Nederlandse verslaggevingscategorieën en rechtsvormen; geen impliciete aanname dat micro-/klein-/middelgrootformaten identiek zijn. Optionele sector-/bedrijfsspecifieke noten via plugin.
5. AI stelt narratieve toelichtingen voor en signaleert ontbrekende noten en inconsistenties, maar **bedragen komen uitsluitend uit bevroren, traceerbare financial read models**, nooit uit LLM-berekening. Meerdere agents mogen voorstellen vergelijken; onafhankelijke rule engine en mens beslissen.
6. Dossierstaten: `DRAFT → PREPARED → UNDER_REVIEW → APPROVED_FOR_ADOPTION → ADOPTED → READY_TO_FILE → FILED` met alternatieve `REWORK_REQUIRED`; transitions op basis van actor, privileges, vooraf bepaalde voorwaarden en audit. 'Goedgekeurd' ≠ 'wettelijk vastgesteld' ≠ 'gedeponeerd'.
7. Materiële wijziging in bronjournaal, verslaggevingsregels, documentinhoud of consolidatierun maakt voorgaande relevante approvals ongeldig en vereist nieuwe versie/review.
8. Concept PDF, onderliggende `.xlsx`, XBRL/SBR-geschikte uitvoer **alleen na daadwerkelijk geïmplementeerde en geteste taxonomievalidatie**; geen valse melding dat KVK indiening gelukt is. Digitale indiening is aparte kritieke actie met menselijke step-up en ontvangstbewijs/status.
9. Module deactiveren: niets verwijderen; historische jaarrekeningen, bronverwijzingen, documenten, handtekeningen/approvals en wettelijke bewaartermijnen behouden; open workflows gecontroleerd afhandelen.

## 3. Gastaccountant / externe auditor
- Uitnodiging uitsluitend door bevoegd bedrijfsbeheerder; expliciet `guest_invitation` gekoppeld aan specifiek accountantskantoor/contact, opdrachtsoort (`view`, `compilation`, `review`, `audit` als context, geen bewijs van uitvoering), administraties, entiteiten, perioden, documentcategorieën, toegestane exports en verloopdatum.
- Guest is **default read-only**, scoped op expliciet toegekende dossiers en periodes. Geen automatische toegang tot alle entiteiten, toekomstige perioden, payroll, IBAN's of persoonsgegevens; minimale inzage met veldmaskering.
- Strong auth/MFA, tijdelijke toegang, toegangsreview, intrekbare sessies en expliciete audittrail van inloggen, inzage, downloaden, AI-vragen, export en intrekking.
- **Onderzoeksfunctionaliteit**: drill-down jaarrekening → rubriek → grootboek → journaalregel → bronfactuur/bon; audit trail, steekproefselectie, filteren, kruisverbanden, reconciliaties, evidence-pack export.
- **Vraag-en-antwoord/dossierverzoeken**: accountant mag bevindingen, comments en verzoeken om bewijs vastleggen in een aparte review-workspace, **niet** financiële boekingen wijzigen. Klant antwoordt/voegt bewijs toe; mutaties alleen door reguliere bevoegde interne rollen met approvals. Notes van auditor zijn gescheiden van ledger.
- Export is een **eigen expliciet recht** (standaard uit of begrensd per opdracht) en server-side beperkt; bulkdocumentexport, persoonsgegevens en gevoelige data extra approval. Iedere export gelogd.
- Rechten nooit via gewone medewerkerrollen erven. Guest-serviceaccounts niet gebruiken om AI of IAM te omzeilen. Gasttoegang kan onder klantbeleid worden beperkt tot specifieke datacategorieën en alleen interne verwerking.
- Auditrapportages onderscheiden accountantscontrole, beoordeling, samenstelling, vrijblijvende dossierinzage en interne controle; software beweert nooit dat zij een wettelijke controle heeft uitgevoerd.

## 4. Opt-ins, privacy en AI-data
- `annual_accounts` en `external_auditor_portal` zijn afzonderlijke modules, direct selecteerbaar voor **elk** profiel (ook ZZP), indien technisch passend. Profiel is template, geen uitsluiting. Guest portal kan onafhankelijk werken om bestaande administratie te laten beoordelen.
- Activatie van jaarrekeningmodule of accountantportaal is geen toestemming voor externe AI. Externe modelverwerking vereist afzonderlijk goedgekeurde AI-processing policy met doel, subprocessor, data fields, regio, retentie en auditbare egress.
- Accountantgast krijgt niet stilzwijgend toegang tot de AI-chat of AI-vragen over meer data dan dossierrechten; een prompt is dezelfde autorisatiegevoelige read-query als een UI-filter/export.
- **Klanttransparantie**: wie kon welke jaarrekeninggegevens/documenten bekijken, wie heeft exports gemaakt, welke documenten zijn naar welke AI-verwerker verstuurd, periode, grondslag/contractuele inrichting, bewaartermijn, en welke persoonsgegevens zijn gemaskeerd. Vermijd garanties van 'geen opslag' zonder providerverificatie.
- AI evidence: modelversie, gebruikte bronrefs/velden, ontvangers, dataverschaffingsroute, output en menselijke beslissingen; geen ruwe PII/prompts in algemene telemetry. Rechtstreeks gebruik van auditorstukken voor modeltraining verboden.

## 5. Kernentiteiten / API voorstel
```
AnnualAccountsCase { id, tenantId, legalEntityId, groupId?, fiscalYear, reportType, accountingFramework,
                     scopeSnapshotId, mappingVersion, ledgerCloseVersion, consolidationRunId?, status, version }
AnnualAccountsReportVersion { caseId, version, sourceSnapshotHash, documentHash, authorId, status, createdAt }
StatementLineMapping { tenantId, frameworkVersion, ledgerAccountId, statementCode, validFrom, validTo }
DisclosureNote { caseId, version, noteType, sourceRefs[], content, aiAssisted, approvedBy? }
GuestEngagement { id, tenantId, professionalOrg?, contactId, engagementType, status, validFrom, validUntil }
GuestAccessGrant { engagementId, adminIds[], entityIds[], periodRange, fields[], docCategories[], canExport, aiAccess }
AuditEvidenceRequest { engagementId, id, references[], description, status, respondentId?, resolutionRefs[] }
ReviewFinding { engagementId, id, severity, sourceRefs[], comment, disposition, ownerId?, status }
FilingSubmission { caseId, reportVersion, payloadHash, format, destination, submittedBy, submittedAt?, receiptId?, status }
```
- `GET /v1/annual-accounts/cases`, `POST .../cases`, `POST .../prepare`, `POST .../review`, `POST .../adopt`, `GET .../exports`.
- `POST /v1/guest-engagements/invitations`, `POST .../revoke`, `GET .../evidence`, `POST .../evidence-requests`, `GET /v1/guest/review-workspace`.
- Iedere read/write/export heeft tenant + entity + administration + report-period + engagement + entitlement + ABAC check. Submitted/adopted/filing status alleen via typed human-approved commands.

## 6. Definition of Done
- ZZP kan tijdens onboarding **zelf jaarrekening samenstellen** aanvinken, zonder automatisch een verkeerde wettelijke deponeringsverplichting te krijgen; een kleine bv ziet correcte review- en deponeringsgates volgens getoetste regels.
- Jaarrekeningbedragen zijn reproduceerbaar uit exacte bevroren journaal-/consolidatiebron, verschillen worden geblokkeerd, AI kan geen cijfer aanpassen via tekstgeneratie.
- Accountantgast kan met verlopen/verwijderde grant niet meer inloggen, zien, zoeken, AI-vragen stellen of downloaden; cross-tenant & cross-entity negatieve tests slagen.
- Auditor kan bronbewijzen doorlopen en opmerkingen plaatsen, maar geen grootboek- of betaalcommando uitvoeren.
- Jaarrekeningstatusën, menselijke autorisatie, versies, deponeringsbewijzen en AI-/exporttoegang zijn auditbaar.
- Geen externe AI-dataverschaffing door louter activatie van een van de modules.

## Bronnen (controleer actuele versie vóór productrelease)
- KVK: https://www.kvk.nl/deponeren/jaarrekening-deponeren-bedrijfsklasse-micro-en-klein/
- KVK: https://www.kvk.nl/deponeren/zelf-deponeren-jaarrekening/
- KVK: https://www.kvk.nl/hulp-en-contact/deponeren/
- KVK: https://www.kvk.nl/deponeren/uiterste-termijn-deponeren-jaarrekening/
- NBA: https://www.nba.nl/over-nba/zoek-een-accountant/wat-doet-een-accountant/

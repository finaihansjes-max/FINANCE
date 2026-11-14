# 23 — AI Builder voor Guided Learnings

## Status en scope
**GEPLAND**. Dit document beschrijft eisen en acceptatievoorwaarden, niet een reeds werkende implementatie. Bij iedere wijziging: privacy-impact, rechten, finance-controls, geautomatiseerde tests, handleidingen NL/EN, document-rendering en reviewersignoff bijwerken. Alle voorbeelden gebruiken synthetische testdata.

## Eénmalige AI-ondersteunde creatie
- Beheerder vraagt AI: 'maak een walkthrough voor banktransactie afletteren'. AI krijgt uitsluitend toegestane productmetadata (route-ID, action-ID, selectors, API-statuscontracten, zichtbare teksten, templateversie en taalcatalogus).
- AI produceert strikt gestructureerde **concepttour**, inclusief NL/EN instructies, selectors en gewenste verificatie-events; geen onbeperkte tooltoegang en geen automatische publicatie.
- Schema validatie, security review, synthetische end-to-end test, selector-coverage, finance review bij muterende processen en menselijke goedkeuring zijn vereist vóór publiceren.
- Model/providergegevens, payloads, opt-in, retentie en gebruikte metadata worden geregistreerd. Standaard geen productie-klantgegevens naar AI; minimaliseer screenshots/documentinhoud.
- Verouderde tours krijgen waarschuwing/ontpublicatie bij onverenigbare UI/proceswijzigingen. Publicatie is versiegebonden.

## Verbod
AI genereert geen nieuwe stappen tijdens live-uitvoering. Gepubliceerde tour is immutable voor een actieve sessie; wijzigingen vereisen nieuwe versie en review.

## Acceptatie
- Ongeldige/onjuiste selector of ontbrekende verificatie leidt tot reject, niet tot een live tour.
- AI mag geen permissions, betalingsgoedkeuring of auditlog omzeilen.

## Licentieafhankelijkheid
AI-tourgeneratie vereist actieve Guided Learning-add-on én expliciet ingeschakelde AI Builder-feature en toegestane AI-gegevensverwerking. Aanschaf van de Guided Learning-module geeft **geen automatische toestemming** voor externe AI of AI-Builder. Niet-gelicentieerde tenants mogen geen concepten genereren, ook niet via directe API of chattools. De handmatige builder/runtime blijven zonder AI bruikbaar voor gelicentieerde klanten.

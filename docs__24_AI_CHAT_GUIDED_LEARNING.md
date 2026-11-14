# 24 — Chat zoekt bestaande Guided Learnings

## Status en scope
**GEPLAND**. Dit document beschrijft eisen en acceptatievoorwaarden, niet een reeds werkende implementatie. Bij iedere wijziging: privacy-impact, rechten, finance-controls, geautomatiseerde tests, handleidingen NL/EN, document-rendering en reviewersignoff bijwerken. Alle voorbeelden gebruiken synthetische testdata.

## Gedrag
- Een gebruiker vraagt 'Hoe moet ik afletteren?' (NL) of 'How do I reconcile a bank transaction?' (EN).
- AI-intentclassificatie of toegestane tekstsearch zoekt uitsluitend **gepubliceerde** tours; server-filter op tenant, administratie, actieve module, rol/rechten, interface-template, taal en routecontext.
- Chat toont titel, beschrijving, geschatte stappen en startkaart/link; gebruiker start expliciet; deterministische runtime voert tour uit zonder live AI.
- Zonder passende tour: eerlijk melden; optioneel verzoek om nieuwe tour aan beheerder/builder. Geen improvisatie van live handelingen.
- Er mag een niet-AI zoekfunctie bestaan, zodat Guided Learning ook volledig bruikbaar blijft zonder AI-opt-in.

## Data
- Aan chat alleen minimaal benodigde context; geen ongevraagde grootboekdata/documentinhoud. Geen cross-tenant vectorindex; toegangscontrole vóór retrieval, opnieuw bij openen en bij iedere stap.

## Acceptatie
- Tour voor onbevoegde rol wordt niet aangeboden of gestart; talen/templates correct; geen live LLM in tour runtime.

## Betaalde Guided Learning-gate
Wanneer Guided Learning niet is afgenomen of de entitlement is verlopen, toont de AI-chat **geen startbare tourkaart** en probeert deze geen verborgen tourcontent op te halen. De chat mag een normale tekstuele uitleg geven indien toegestaan en eventueel de betaalde uitbreiding benoemen zonder ongevraagde aankoop of activering. Filter op commerciële entitlement vóór tour-retrieval en opnieuw vóór de player start. AI-chat en reguliere helpfunctionaliteit mogen zonder de add-on blijven functioneren.

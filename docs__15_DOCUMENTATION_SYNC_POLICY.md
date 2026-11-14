# 15 — Documentatie-invariant: Markdown en leesbare bestanden altijd 1-op-1

## Eén bron van waarheid
- **Markdown (`README.md`, `CLAUDE.md`, `docs/**/*.md`, `claude/tasks/**/*.md`) is canoniek.**
- `Leesbare_documenten/HTML/` bevat voor **elk** Markdown-bestand precies één equivalente HTML-render, met behoud van paragrafen, tabellen, voorbeelden, code en waarschuwingsnotities.
- `Leesbare_documenten/PDF/` bevat de hoofd- en gebruikersdocumentatie als PDF, opgebouwd uit dezelfde Markdown-bron. Sprintinstructies hebben altijd HTML en mogen tevens PDF hebben.
- Wijzig **nooit** handmatig gegenereerde HTML of PDF. Verander de bron en draai daarna de generator.
- De index en het manifest worden automatisch gegenereerd. Een ontbrekende, extra of verouderde render is een harde **NO-GO**.

## Reproduceerbare workflow
1. Pas bron-Markdown én relevante handleidingparagrafen aan.
2. Voer `python scripts/build_readable.py` uit in de projectroot; verwacht Python, Pandoc en WeasyPrint.
3. Voer `python scripts/build_readable.py --check` uit: valideert alle canonieke MD-bronnen, HTML/PDF-bestanden, manifestchecksums en ontbrekende/onverwachte documenten.
4. Voer daarnaast softwaretests, privacychecks, review en UI-walkthrough uit. Synchronisatie bewijst **geen** inhoudelijke juistheid t.o.v. code.
5. Commit **code, MD, leesbare bestanden en manifest samen**. CI moet `--check` uitvoeren.

## Functionele handleiding-synchronisatie
- Iedere zichtbare feature, module-opt-in, nieuwe bevoegdheid, goedkeuringsstap, export, auditfunctie, AI-verwerking en foutmelding wordt bijgewerkt in de handleiding in dezelfde PR.
- De handleiding bevat per functie: status (GEPLAND/GEIMPLEMENTEERD/GEVALIDEERD), vereiste rol, locatie in de UI, stappen, resulterende handeling, uitzonderingen en data-/privacygevolgen.
- Een functie is **GEIMPLEMENTEERD** pas na werkende code+tests en **GEVALIDEERD** pas na onafhankelijke walkthrough. Ontwerpdocumenten in deze ZIP beschrijven grotendeels GEPLAND werk.
- Bij wijzigingen van AI-provider, regio, inputvelden, retentie of externe doorgifte worden ook `docs/04_DATA_GOVERNANCE_SECURITY.md` en `docs/08_DATA_PRIVACY_VOOR_KLANTEN.md` gewijzigd. Klantclaims blijven concept totdat bevestigd.
- Vereis een traceability-matrix: requirement-ID → module/endpoint/scherm → test-ID → handleidingsectie.

## Acceptatie
Release wordt tegengehouden bij: stale manifest, MD/HTML/PDF-mismatch, ontbrekende handleidingupdate, handmatig gewijzigde renderversie, niet-geïmplementeerde feature als bestaand omschreven, ontbrekende reviewersign-off.

## Technische nuance
Bestands-hashes controleren dat de gegenereerde artefacten exact de vastgelegde build zijn; inhoudelijke 1-op-1 ontstaat doordat **één automatische renderpipeline** beide formaten uit dezelfde bron genereert. Controleer daarnaast steekproefsgewijs de HTML/PDF-opmaak en tekstafbreking. Genereer nooit afzonderlijk nieuwe tekst voor een leesbare versie met een taalmodel.

## v0.6 — Module-specificaties zijn ook canonieke Markdown
`modules/**/*.md` is onderdeel van de verplichte 1-op-1 rendering en hash-manifest. Nieuwe modulewens, implementatie of wijziging veroorzaakt in dezelfde PR bijgewerkte relevante module-specificatie, docs, sprint, beide taalhandleidingen en nieuwe HTML/PDF. `--check` moet PASS geven.

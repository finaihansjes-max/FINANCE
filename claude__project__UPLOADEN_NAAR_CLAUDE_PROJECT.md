# Installatie: Claude Project + Claude Code

## Route A — Claude Project voor ontwerp en kennis

1. Maak een nieuw Claude Project met naam **AI Finance — Bouwplatform**.
2. Plak de inhoud van `claude/project/PROJECTBESCHRIJVING_CLAUDE.md` in de projectbeschrijving.
3. Plak de inhoud van `claude/project/PROJECTINSTRUCTIES_CLAUDE.md` bij **Project instructions** (niet alleen bij de beschrijving).
4. Pak `AI_Finance_Claude_Project_Kennis_v0.9.zip` uit. Voeg de Markdown-bestanden daarvan toe als Project knowledge. Als Claude een aantal uploads tegelijk beperkt, upload dan in batches, te beginnen met `CLAUDE`, `README`, `docs`, `modules` en `claude/tasks`.
5. Controleer in een chat: 'Noem de relevante eisen voor Sprint 00 en de bronbestanden.'
6. Typ **Begin met bouwen**. Voor daadwerkelijke codebewerkingen en tests is een gekoppelde ontwikkelomgeving nodig.

## Route B — daadwerkelijke ontwikkeling in Claude Code

1. Pak de **volledige blueprint-ZIP** uit, bijvoorbeeld in `~/Projects/ai-finance/`.
2. Maak daar een Git-repository en open de map met Claude Code of een daarmee verbonden ontwikkelomgeving.
3. Zorg dat `CLAUDE.md` op rootniveau staat, naast `docs/`, `modules/`, `claude/tasks/` en `scripts/`.
4. Typ **Begin met bouwen**. Claude voert eerst uitsluitend Sprint 00 uit.
5. Voeg pas na testen en controle volgende sprints toe.

## Waarom twee pakketten?

- **Volledige ZIP**: behoudt paden, scripts, assets, HTML/PDF, en sprintmappen voor echte softwareontwikkeling.
- **Projectkennis-ZIP**: bevat alleen Markdown met unieke, van het pad afgeleide bestandsnamen, zodat namen als `README.md` niet botsen in een platte uploadlijst.
- De bron van waarheid blijft de originele Markdown in de volledige repo. Vervang geüploade projectkennis als de repo-specs veranderen; een eenmalige upload synchroniseert niet vanzelf.

## Veiligheid

Gebruik bij ontwerp en testen uitsluitend fictieve datasets, geen echte API-tokens of klantadministraties. Externe AI-verwerking vereist aparte contractuele, technische en tenant-specifieke controles.

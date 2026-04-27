# Feature Specification: Energihandel Forslagsbehandling

**Feature Branch**: `001-vedlikeholdsstopp-registrering`  
**Created**: 2026-04-21  
**Status**: Draft  
**Input**: User description: "Andre feature for Markedsdrevet Vedlikehold: Energihandel mottar vedlikeholdsforesporsler som driftsleder har sendt inn i forrige feature (001-vedlikeholdsstopp-registrering), behandler dem, og sender tre eller flere foreslatte vedlikeholdsvindu tilbake."

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Prioritere innkomne saker (Priority: P1)

Energihandel-saksbehandler skal kunne se en innboks med aktive vedlikeholdsforesporsler i status "Ny" og "Under behandling", slik at innkomne saker kan prioriteres og tas i arbeid.

**Why this priority**: Uten en fungerende innboks kan ikke Energihandel starte behandling av saker, og arbeidsflyten stopper ved mottak.

**Independent Test**: Kan testes uavhengig ved at systemet inneholder saker i ulike statuser og at innboksen kun viser saker i "Ny" og "Under behandling" med forventet informasjon og eierskap.

**Acceptance Scenarios**:

1. **Given** flere vedlikeholdsforesporsler med ulike statuser finnes, **When** Energihandel apner innboksen, **Then** vises kun saker med status "Ny" eller "Under behandling".
2. **Given** en sak med status "Under behandling" har en eier, **When** den vises i innboksen, **Then** fremgar eierens navn og saken kan ikke apnes eller overtas av andre brukere.
3. **Given** en sak har status "Alternativer sendt" eller "Besluttet", **When** Energihandel ser innboksen, **Then** vises ikke saken i aktiv innboks.

---

### User Story 2 - Ta sak til behandling og lase den (Priority: P1)

Energihandel-saksbehandler skal kunne apne en "Ny" sak, lese hele foresporselen, og starte behandling slik at saken far status "Under behandling" og lases til riktig saksbehandler.

**Why this priority**: Kontrollert oppstart og lasefunksjon hindrer parallell redigering og sikrer tydelig ansvar for hver sak.

**Independent Test**: Kan testes uavhengig ved at en "Ny" sak apnes og "Start behandling" utfores, med verifisering av statusendring, eier og at andre brukere ikke kan redigere.

**Acceptance Scenarios**:

1. **Given** en sak i status "Ny", **When** Energihandel apner detaljvisningen, **Then** vises alle registrerte felt fra foresporselen i lesevisning.
2. **Given** en sak i status "Ny", **When** saksbehandler klikker "Start behandling", **Then** endres status til "Under behandling" og saken far saksbehandler som eier.
3. **Given** en sak er laset til en eier, **When** en annen Energihandel-bruker forsoker a redigere eller sende forslag, **Then** blokkeres handlingen og eierinformasjon vises.

---

### User Story 3 - Registrere og sende forslag til drift (Priority: P1)

Energihandel-saksbehandler skal kunne registrere rangerte forslag for en sak under behandling og sende saken til drift nar minimumskravene er oppfylt.

**Why this priority**: Dette leverer den sentrale forretningsverdien i featuren: strukturerte og sporbare vedlikeholdsvinduer tilbake til drift.

**Independent Test**: Kan testes uavhengig ved at en sak i "Under behandling" far forslag registrert, valideres mot domene-regler og sendes til drift med status "Alternativer sendt".

**Acceptance Scenarios**:

1. **Given** en sak i status "Under behandling", **When** Energihandel registrerer forslag, **Then** lagres hvert forslag med startdato, sluttdato, estimert inntektstap, vannforingsprognose og rangering basert pa registreringsrekkefolge.
2. **Given** et forslag ligger utenfor driftsleders fleksibilitetsvindu, **When** Energihandel lagrer forslaget, **Then** kreves begrunnelse for at forslaget skal kunne lagres.
3. **Given** saken har minst tre forslag innenfor fleksibilitetsvinduet og gyldig begrunnelse pa forslag utenfor vinduet, **When** Energihandel klikker "Send til drift", **Then** endres status fra "Under behandling" til "Alternativer sendt" og saken fjernes fra aktiv innboks.
4. **Given** minimum tre forslag innenfor fleksibilitetsvinduet ikke er oppfylt, **When** Energihandel klikker "Send til drift", **Then** blokkeres sending med tydelig forklaring av hva som mangler.
5. **Given** en sak er sendt med status "Alternativer sendt", **When** Energihandel apner saken senere, **Then** kan forslag ikke redigeres, slettes eller trekkes tilbake i denne featuren.

---

### Edge Cases

- Hva skjer dersom to Energihandel-brukere forsoker a starte behandling av samme "Ny"-sak nesten samtidig?
- Hvordan handteres forsok pa lagring av forslag der sluttdato er tidligere enn startdato?
- Hvordan handteres forsok pa sending dersom tre forslag finnes, men ett av dem bare delvis overlapper fleksibilitetsvinduet?
- Hva skjer dersom en sak er laset av en bruker som logger ut eller forlater siden uten a sende forslag?
- Hvordan vises valideringsfeil nar flere forslag utenfor fleksibilitetsvinduet mangler begrunnelse samtidig?

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: Systemet MA vise en aktiv innboks for Energihandel med vedlikeholdsforesporsler i status "Ny" og "Under behandling".
- **FR-002**: Innboksen MA vise for hver sak: stasjon, aggregat, onsket startperiode, estimert varighet, type vedlikehold, fleksibilitet, kritikalitet og tid siden innsending.
- **FR-003**: For saker i status "Under behandling" MA systemet vise hvilken Energihandel-saksbehandler som eier saken.
- **FR-004**: Systemet MA hindre at andre enn eier apner for redigering eller overtar en sak i status "Under behandling".
- **FR-005**: Ved apning av en "Ny" sak MA systemet vise full detaljvisning av alle felt fra opprinnelig vedlikeholdsforesporsel.
- **FR-006**: Systemet MA tilby handlingen "Start behandling" for saker i status "Ny".
- **FR-007**: Nar "Start behandling" utfores MA systemet sette status til "Under behandling" og knytte saken til aktuell Energihandel-bruker som eier.
- **FR-008**: For saker i status "Under behandling" MA systemet la eier registrere, redigere og slette forslag fram til sending.
- **FR-009**: Hvert forslag MA inneholde startdato, sluttdato, estimert inntektstap og vannforingsprognose.
- **FR-010**: Systemet MA tildele rangering til forslag basert pa rekkefolgen de registreres i.
- **FR-011**: Nar et forslag ligger utenfor driftsleders fleksibilitetsvindu MA systemet kreve begrunnelse for forslaget.
- **FR-012**: Systemet MA tilby handlingen "Send til drift" for saker i status "Under behandling".
- **FR-013**: For a kunne sende saken MA systemet validere at minst tre forslag ligger fullstendig innenfor driftsleders fleksibilitetsvindu.
- **FR-014**: For a kunne sende saken MA systemet validere at alle forslag utenfor fleksibilitetsvinduet har begrunnelse.
- **FR-015**: Nar validering feiler ved sending MA systemet blokkere handlingen og vise tydelig forklaring pa hva som mangler.
- **FR-016**: Nar validering er godkjent ved sending MA systemet endre status til "Alternativer sendt", fjerne saken fra aktiv innboks, og eksponere forslagene for neste steg i arbeidsflyten.
- **FR-017**: Etter at status er "Alternativer sendt" MA systemet hindre tilbaketrekking, redigering og sletting av forslag i denne featuren.
- **FR-018**: Feature-scope MA ekskludere driftsleders mottak/beslutning av forslag, Energihandels markedsanalyseverktoy, eksterne integrasjoner, vedlegg, kommentartrad, historikk for ferdigbehandlede saker, manuell frigjoring av las og automatisk timeout pa las.

### Key Entities *(include if feature involves data)*

- **Vedlikeholdsforesporsel**: Sak sendt inn av driftsleder, med statuslivslop, foresporselsdata og kobling til eventuell Energihandel-eier.
- **Energihandel-innboksvisning**: Prioritert visning av aktive saker med et avgrenset sett felter for rask triagering.
- **Sakslas**: Regelsett som knytter en sak i "Under behandling" til en eier og blokkerer samtidige endringer fra andre brukere.
- **Forslag**: Energihandels vedlikeholdsvindu med start/slutt, estimert inntektstap, vannforingsprognose, rangering og eventuell begrunnelse.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Minst 90% av Energihandel-saksbehandlere i workshop-test kan ga fra innboks til "Under behandling" pa under 60 sekunder for en ny sak.
- **SC-002**: 100% av forsok pa samtidig redigering av samme sak fra to brukere blokkeres for ikke-eier nar saken er "Under behandling".
- **SC-003**: 100% av sendte saker oppfyller regelen om minimum tre forslag innenfor fleksibilitetsvinduet og begrunnelse for alle forslag utenfor vinduet.
- **SC-004**: Minst 95% av Energihandel-brukere i evaluering opplever at de kan sende forslag i en sammenhengende arbeidsflyt uten telefon eller e-post for intern koordinering.

## Assumptions

- Feature 001 leverer gyldige vedlikeholdsforesporsler med felter og statusverdier som denne featuren kan bruke videre uten ny datamodell.
- En seedet Energihandel-identitet brukes i v0.1, men losningen modelleres slik at flere Energihandel-brukere kan eksistere samtidig.
- Driftsleders mottak, visning og beslutning av forslag handteres i en senere feature.
- Inntektstap og vannforingsprognose registreres manuelt av Energihandel basert pa analyser utenfor produktet.
- Det finnes ingen timeout eller manuell opplasning av las i denne leveransen; las beholdes til saken sendes til drift.

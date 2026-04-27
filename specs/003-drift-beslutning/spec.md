# Feature Specification: Drift Beslutning

**Feature Branch**: `004-drift-beslutning`  
**Created**: 2026-04-27  
**Status**: Draft  
**Input**: User description: "Tredje feature for Markedsdrevet Vedlikehold: driftsleder mottar Energihandels rangerte forslag til vedlikeholdsvindu, vurderer dem, og fatter en endelig beslutning ved a velge ett av forslagene. Dekker overgangen Alternativer sendt → Besluttet, med audit trail speilet til Fabric."

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Oppdage saker med forslag (Priority: P1)

Driftsleder skal tydelig se når det foreligger forslag fra Energihandel for egne vedlikeholdsforesporsler (status "Alternativer sendt"), slik at beslutning kan tas uten manuell oppfølging på e-post/telefon.

**Why this priority**: Uten synlig varsling/oppdagelse stopper arbeidsflyten før beslutning, selv om Energihandel allerede har levert forslag.

**Independent Test**: Kan testes ved å ha minst én sak for driftsleder i status "Alternativer sendt" og verifisere at et in-app banner og listevisning leder brukeren til riktig detaljside.

**Acceptance Scenarios**:

1. **Given** at driftsleder har en sak i status "Alternativer sendt", **When** driftsleder åpner sin oversikt, **Then** vises et tydelig in-app banner med stasjon/aggregat og lenke "Se forslag" til sakens detaljvisning.
2. **Given** at "Dine henvendelser"-listen inneholder saker i ulike statuser, **When** driftsleder ser listen, **Then** vises saker i status "Alternativer sendt" med oppdatert statusbadge og er klikkbare til detaljvisningen.
3. **Given** at driftsleder åpner en ekstern direktelink til en sak, **When** siden lastes, **Then** åpnes samme detaljvisning uten ekstra omveier, med stabil og forutsigbar URL per sak.

---

### User Story 2 - Se beslutningsgrunnlag (Priority: P1)

Driftsleder skal kunne se hele beslutningsgrunnlaget samlet på én detaljside for en sak i status "Alternativer sendt", slik at drift kan sammenligne forslag opp mot opprinnelig behov og fleksibilitetsvindu.

**Why this priority**: Beslutningen må tas på korrekt og komplett grunnlag; hvis informasjon er fragmentert eller ufullstendig øker risiko for feilvalg og merarbeid.

**Independent Test**: Kan testes ved å åpne detaljvisning og verifisere at opprinnelig forespørsel, Energihandels kommentar (hvis finnes) og alle forslag vises korrekt og i riktig rekkefølge, inkludert begrunnelser utenfor fleksibilitetsvindu.

**Acceptance Scenarios**:

1. **Given** en sak i status "Alternativer sendt", **When** driftsleder åpner detaljvisningen, **Then** vises en oppsummering av opprinnelig forespørsel (stasjon, aggregat, ønsket periode, varighet, type, fleksibilitet, kritikalitet, avhengighet, kommentar).
2. **Given** at Energihandel har lagt til en samlet kommentar, **When** driftsleder åpner detaljvisningen, **Then** vises kommentaren; og hvis kommentaren er tom, vises ikke kommentarseksjonen.
3. **Given** at Energihandel har sendt rangerte forslag, **When** driftsleder ser forslagslisten, **Then** vises forslag sortert etter rangering (#1 først) og forslag #1 er visuelt merket "Anbefalt".
4. **Given** at et forslag ligger utenfor opprinnelig fleksibilitetsvindu, **When** driftsleder ser forslaget, **Then** vises Energihandels begrunnelse for det forslaget.

---

### User Story 3 - Velge forslag og beslutte (Priority: P1)

Driftsleder skal kunne velge nøyaktig ett av Energihandels forslag, lagre beslutning, og få saken oppdatert til status "Besluttet", slik at happy-path arbeidsflyt avsluttes og beslutningen blir sporbar.

**Why this priority**: Dette er selve målhandlingen i featuren og gir forretningsverdi ved at beslutningskjeden dokumenteres end-to-end.

**Independent Test**: Kan testes ved å velge et forslag, lagre, og verifisere statusendring, at valgt forslag og kommentar er lagret, at bekreftelsesside vises, og at audit trail er speilet til Fabric.

**Acceptance Scenarios**:

1. **Given** en sak med forslag i status "Alternativer sendt", **When** detaljvisningen lastes, **Then** er ingen forslag forhåndsvalgt og driftsleder må aktivt velge ett alternativ.
2. **Given** at driftsleder ikke har valgt et forslag, **When** driftsleder forsøker å lagre beslutning, **Then** blokkeres lagring med tydelig melding "Velg et forslag for å fortsette" (og/eller lagreknappen er deaktivert).
3. **Given** at driftsleder har valgt ett forslag og (valgfritt) skrevet kommentar, **When** driftsleder klikker "Godkjenn og lagre beslutning", **Then** endres status til "Besluttet", valgt forslag markeres som besluttet valg, og kommentar lagres på saken.
4. **Given** at beslutning lagres, **When** lagringen fullføres, **Then** speiles beslutningstilstand til Fabric som audit trail (inkludert alle alternativer Energihandel sendte, valgt alternativ, kommentar, tidspunkt og beslutter-identitet).
5. **Given** at beslutning er lagret, **When** bekreftelsessiden vises, **Then** vises forespørsel-ID, stasjon/aggregat, valgt periode, estimert inntektstap for valgt forslag, status "Besluttet", og beslutter-identitet med tidsstempel.

---

### User Story 4 - Se beslutningshistorikk etterpå (Priority: P2)

Driftsleder skal kunne åpne en "Besluttet" sak i read-only modus og se hele beslutningshistorikken, slik at sporbarhet og ettersyn er mulig uten å endre utfallet.

**Why this priority**: Etterprøvbarhet er sentralt for læring og analyse (hvorfor avvik fra anbefalt), og for intern kommunikasjon.

**Independent Test**: Kan testes ved å åpne en besluttet sak og verifisere at alle deler av historikken vises, men at beslutningskontroller ikke er tilgjengelige.

**Acceptance Scenarios**:

1. **Given** en sak i status "Besluttet", **When** driftsleder åpner detaljvisningen, **Then** vises opprinnelig forespørsel, alle forslag, valgt forslag tydelig markert, Energihandels kommentar (hvis den fantes) og drifts kommentar.
2. **Given** en sak i status "Besluttet", **When** detaljvisningen vises, **Then** er siden read-only og viser ikke radioknapper eller "Godkjenn og lagre beslutning".

---

### Edge Cases

- Hva skjer dersom statusoppdatering til Fabric feiler ved beslutning (skal beslutning vises som lagret eller skal brukeren få tydelig feilmelding og kunne prøve igjen)?
- Hva skjer dersom en direktelink peker til en sak som ikke tilhører innlogget driftsleder i denne fasen med seedet identitet?
- Hvordan vises tomtilstand når driftsleder ikke har saker i status "Alternativer sendt"?
- Hvordan håndteres visning når Energihandel har sendt flere enn tre forslag (scroll/utvidelser uten at informasjon blir uoversiktlig)?
- Hvordan håndteres svært lange begrunnelser/kommentarer uten at beslutningsområdet blir vanskelig å bruke?

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: Systemet MÅ vise saker som tilhører driftsleder i status "Alternativer sendt" både i "Dine henvendelser"-listen og via et tydelig in-app banner.
- **FR-002**: Banneret MÅ vise hvilken sak det gjelder (minst stasjon og aggregat) og tilby en lenke "Se forslag" som åpner sakens detaljvisning.
- **FR-003**: Systemet MÅ støtte en stabil og forutsigbar URL per sak, slik at direktelink kan åpne detaljvisning uten ekstra navigasjonstrinn.
- **FR-004**: I detaljvisning for status "Alternativer sendt" MÅ systemet vise en oppsummering av opprinnelig vedlikeholdsforespørsel (inkludert fleksibilitetsvinduet).
- **FR-005**: Systemet MÅ vise Energihandels samlede kommentar for saken når den finnes, og MÅ skjule seksjonen når kommentaren er tom.
- **FR-006**: Systemet MÅ vise Energihandels forslag sortert etter rangering og visuelt merke forslag #1 som "Anbefalt".
- **FR-007**: Hvert forslag MÅ vise startdato, sluttdato, estimert inntektstap, vannføringsprognose og eventuelt begrunnelse dersom forslaget er utenfor fleksibilitetsvinduet.
- **FR-008**: Driftsleder MÅ velge nøyaktig ett av Energihandels forslag for å kunne lagre beslutning.
- **FR-009**: Ingen forslag MÅ være forhåndsvalgt når siden lastes.
- **FR-010**: Ved forsøk på lagring uten valgt forslag MÅ systemet blokkere og vise tydelig valideringsmelding.
- **FR-011**: Ved gyldig lagring MÅ systemet endre status fra "Alternativer sendt" til "Besluttet" og knytte valgt forslag som drifts beslutning til saken.
- **FR-012**: Systemet MÅ lagre drifts valgfri kommentar sammen med beslutningen.
- **FR-013**: Ved gyldig lagring MÅ systemet speile beslutningstilstand til Fabric som audit trail, inkludert alle alternativer, valgt alternativ, kommentarer, beslutningstidspunkt og beslutter-identitet.
- **FR-014**: Systemet MÅ ikke duplisere Energihandels forslag i Fabric, men knytte beslutningsdata til samme sak/record slik at Fabric viser helhetlig spor (analyse + utfall).
- **FR-015**: Etter at en sak er "Besluttet" MÅ detaljvisningen være read-only og ikke tilby endring/annullering av beslutningen i denne featuren.
- **FR-016**: Feature-scope MÅ ekskludere Teams-varsling, ISY-integrasjon, revisjonssløyfe (avvise alle og sende tilbake), motforslag fra drift, og rollebasert tilgangsstyring utover seedet identitet.

### Key Entities *(include if feature involves data)*

- **Vedlikeholdsforesporsel**: Saken som flyter gjennom statuser (inkl. "Alternativer sendt" og "Besluttet") og binder sammen opprinnelig forespørsel, Energihandels forslag og drifts beslutning.
- **Forslag**: Ett vedlikeholdsvindu sendt fra Energihandel med rangering og relevante attributter (periode, økonomi, prognoser, begrunnelse).
- **Drifts beslutning**: Valg av ett forslag + valgfri kommentar + beslutter-identitet + tidsstempel.
- **Audit trail (Fabric)**: Helhetlig spor for saken som inkluderer alle alternativer, valg og begrunnelser for analyse og etterprøvbarhet.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Driftsleder kan gå fra synlig varsel til lagret beslutning på under 2 minutter for en typisk sak i workshop-test.
- **SC-002**: Hele beslutningsgrunnlaget (forespørsel + forslag + markering av anbefalt/valgt + kommentarer) er synlig på én skjerm uten behov for navigering til andre sider i normaltilfellet.
- **SC-003**: 100% av lagringsforsøk uten aktivt valg blokkeres og gir tydelig tilbakemelding.
- **SC-004**: 100% av lagrede beslutninger er lesbare både i applikasjonen og som audit trail i Fabric (for samme sak).

## Assumptions

- Driftsleder-identitet er seedet i v0.1 på samme måte som i feature 001, men modellert slik at flere driftsledere kan støttes senere.
- Energihandel er ikke en aktiv aktør i denne featuren; de kan se utfallet via Fabric, men mottar ikke aktiv push-varsling her.
- Teams-varsling er ikke implementert, men løsningens ruting/URL-sti er stabil nok til å kunne brukes av en senere varslingskanal.
- Det finnes ingen frist/utløpstid for beslutning i v0.1; saker forblir i "Alternativer sendt" inntil drift beslutter.
- Status "Besluttet" er endelig i denne featuren; endring/annullering er eksplisitt utsatt til senere feature.

# Feature Specification: Vedlikeholdsstopp Registrering

**Feature Branch**: `001-vedlikeholdsstopp-registrering`  
**Created**: 2026-04-21  
**Status**: Draft  
**Input**: User description: "First feature for Markedsdrevet Vedlikehold: driftsleder registration form and confirmation screen for vedlikeholdsforesporsel, including personal request history."

## User Scenarios & Testing *(mandatory)*

<!--
  IMPORTANT: User stories should be PRIORITIZED as user journeys ordered by importance.
  Each user story/journey must be INDEPENDENTLY TESTABLE - meaning if you implement just ONE of them,
  you should still have a viable MVP (Minimum Viable Product) that delivers value.
  
  Assign priorities (P1, P2, P3, etc.) to each story, where P1 is the most critical.
  Think of each story as a standalone slice of functionality that can be:
  - Developed independently
  - Tested independently
  - Deployed independently
  - Demonstrated to users independently
-->

### User Story 1 - Registrere vedlikeholdsforesporsel (Priority: P1)

En driftsleder skal kunne registrere en ny vedlikeholdsforesporsel med alle obligatoriske opplysninger slik at Energihandel kan starte vurdering av markedstiming.

**Why this priority**: Dette er den utløsende handlingen for hele arbeidsflyten. Uten korrekt registrering finnes ingen sak å behandle videre.

**Independent Test**: Kan testes uavhengig ved at en driftsleder fyller ut skjema med gyldige data og får lagret foresporselen med generert ID, tidsstempel, brukeridentitet og status "Ny".

**Acceptance Scenarios**:

1. **Given** en tom registreringsside, **When** driftsleder fyller ut alle obligatoriske felt og sender inn, **Then** lagres en ny vedlikeholdsforesporsel med unik ID og status "Ny".
2. **Given** at driftsleder har valgt en stasjon, **When** aggregatfeltet vises, **Then** skal kun aggregater for valgt stasjon kunne velges.
3. **Given** minst ett obligatorisk felt mangler, **When** driftsleder forsøker innsending, **Then** blokkeres innsending og tydelige feltfeil vises.

---

### User Story 2 - Se bekreftelse etter innsending (Priority: P2)

Etter vellykket innsending skal driftsleder umiddelbart se en bekreftelsesside med en kompakt oppsummering av det viktigste innholdet i foresporselen og tydelig beskjed om videre prosess.

**Why this priority**: Brukeren trenger umiddelbar trygghet om at foresporselen er mottatt og sendt videre, ellers oppstår usikkerhet og manuell oppfolging.

**Independent Test**: Kan testes uavhengig ved innsending av en gyldig foresporsel og verifisering av at bekreftelsessiden viser korrekt oppsummering og at redigering ikke tilbys.

**Acceptance Scenarios**:

1. **Given** en nylig innsendt foresporsel, **When** bekreftelsessiden vises, **Then** skal den vise foresporsel-ID, stasjon/aggregat, onsket periode, varighet og status.
2. **Given** bekreftelsessiden er vist, **When** driftsleder ser informasjonsteksten, **Then** skal det fremga at Energihandel er varslet og vil komme tilbake med forslag.
3. **Given** en innsendt foresporsel med status "Ny", **When** driftsleder er pa bekreftelsessiden, **Then** kan foresporselen redigeres fram til den er godkjent.

---

### User Story 3 - Folg egne foresporsler i historikk (Priority: P3)

Driftsleder skal kunne se en personlig historikkliste over egne vedlikeholdsforesporsler med status, slik at vedkommende kan folge progresjon uten a matte ringe fysisk (eller sende e-post) for statusoppdatering.

**Why this priority**: Historikken reduserer operativ friksjon og avklaringsbehov, men krever at registrering og bekreftelse allerede fungerer.

**Independent Test**: Kan testes uavhengig ved at flere foresporsler for samme bruker finnes, og listen viser kun denne brukerens saker med riktige statusverdier og sentrale felter.

**Acceptance Scenarios**:

1. **Given** at driftsleder har tidligere foresporsler, **When** bekreftelsessiden vises, **Then** skal en historikkliste vise stasjon, aggregat, type vedlikehold, tid siden innsending og status for hver foresporsel.
2. **Given** foresporsler fra flere driftsledere finnes i systemet, **When** en driftsleder ser egen historikk, **Then** skal bare egne foresporsler vises.
3. **Given** en ny foresporsel nettopp er sendt inn, **When** historikken oppdateres, **Then** skal den nye posten vises med status "Ny".

---

### Edge Cases

- Hva skjer dersom driftsleder bytter stasjon etter at aggregat er valgt?
- Hvordan handteres innsending nar estimert varighet er 0 eller ikke gyldig tall?
- Hva skjer dersom onsket periode er ufullstendig eller utenfor tillatt datointervall?
- Hvordan vises brukerfeil nar flere obligatoriske felt mangler samtidig?
- Hva vises pa bekreftelsessiden dersom brukeren ikke har tidligere foresporsler?
- Hva skjer dersom lagring feiler i det brukeren sender inn skjemaet?

## Requirements *(mandatory)*

<!--
  ACTION REQUIRED: The content in this section represents placeholders.
  Fill them out with the right functional requirements.
-->

### Functional Requirements

- **FR-001**: Systemet MÅ tilby et registreringsskjema for vedlikeholdsforesporsel organisert i tre grupper: anlegg/enhet, stoppdetaljer og kontekst/avhengigheter.
- **FR-002**: Systemet MÅ kreve alle felt unntatt kommentarfeltet for a tillate innsending.
- **FR-003**: Systemet MÅ validere obligatoriske felt før innsending og vise tydelig feltspesifikk feilinformasjon ved mangler.
- **FR-004**: Systemet MÅ la driftsleder velge stasjon først, og deretter begrense aggregatvalg til aggregater som tilhorer valgt stasjon.
- **FR-005**: Systemet MÅ bruke aggregatets registrerte MW-kapasitet som systemegenskap uten manuell input fra driftsleder.
- **FR-006**: Systemet MÅ samle inn onsket startperiode, estimert varighet i dager, vedlikeholdstype, fleksibilitet, kritikalitet, avhengighet og valgfri kommentar.
- **FR-007**: Ved innsending MÅ systemet opprette en ny vedlikeholdsforesporsel med unik foresporsel-ID, innsendingstidspunkt, innsendingens brukeridentitet og initial status "Ny".
- **FR-008**: Initial status "Ny" MÅ settes automatisk av systemet og kan ikke velges eller overstyres manuelt i denne featureen.
- **FR-009**: Etter vellykket innsending MÅ systemet vise en bekreftelsesside med kompakt oppsummering av foresporsel-ID, stasjon/aggregat, onsket periode, varighet og status.
- **FR-010**: Bekreftelsessiden MÅ vise en tydelig melding om at Energihandel er varslet og vil returnere forslag til stoppvindu.
- **FR-011**: Bekreftelsessiden MÅ ikke tilby redigering eller sletting av innsendt foresporsel.
- **FR-012**: Bekreftelsessiden MÅ vise historikk over innlogget driftsleders egne foresporsler med stasjon, aggregat, type vedlikehold, tid siden innsending og status.
- **FR-013**: Historikkliste MÅ kun inneholde foresporsler opprettet av innlogget driftsleder.
- **FR-014**: Systemet MÅ støtte visning av statusverdier for hele livslopet i historikken: "Ny", "Under behandling", "Alternativer sendt", og "Besluttet".
- **FR-015**: Feature-scope MÅ ekskludere Energihandels analyseflyt, forslagshandtering, beslutningsskjerm, redigering/sletting, vedlegg og eksterne integrasjoner.

### Key Entities *(include if feature involves data)*

- **Vedlikeholdsforesporsel**: Kjerneobjektet som representerer en innsending fra driftsleder med feltene foresporsel-ID, innsendingstidspunkt, brukeridentitet, stasjon, aggregat, aggregatkapasitet (MW), onsket startperiode, estimert varighet (dager), type vedlikehold, fleksibilitet, kritikalitet, avhengighet, kommentar (valgfri) og status.
- **Stasjon**: Kraftstasjon som eier ett eller flere aggregater og fungerer som overordnet valg i skjemaet.
- **Aggregat**: Produksjonsenhet tilknyttet en stasjon; brukes for identifikasjon av stoppobjekt og tilhorende MW-kapasitet.
- **Brukerhistorikkvisning**: Visningssett av tidligere vedlikeholdsforesporsler filtrert pa innlogget driftsleder.

## Success Criteria *(mandatory)*

<!--
  ACTION REQUIRED: Define measurable success criteria.
  These must be technology-agnostic and measurable.
-->

### Measurable Outcomes

- **SC-001**: Minst 90% av driftsledere i workshop-test kan fullfore registrering og innsending av en vedlikeholdsforesporsel pa under 2 minutter uten hjelp.
- **SC-002**: 100% av vellykkede innsendinger viser bekreftelsesside med korrekt foresporsel-ID, stasjon/aggregat, onsket periode, varighet og status.
- **SC-003**: Minst 95% av innsendinger med manglende obligatoriske felt stoppes før lagring og viser tydelige feltfeil.
- **SC-004**: I brukerfeedback etter demo svarer minst 80% av driftsledere at historikkvisningen gir tilstrekkelig oversikt til at de slipper manuell statusoppfolging.

## Assumptions

- Innlogget brukeridentitet er tilgjengelig i losningen slik at "mine foresporsler" kan filtreres uten at denne featureen bygger egen autentiseringsflyt.
- Liste over stasjoner og tilhorende aggregater finnes som tilgjengelig grunnlag i systemet ved registreringstidspunkt.
- Varsling til Energihandel trigges ved innsending, men kanal og teknisk implementasjon handteres utenfor denne featureen.
- Registreringsskjema og bekreftelsesside er primart designet for desktopbruk i kontor-/moteromskontekst, med grunnleggende responsiv oppforsel.
- Endring av status etter "Godkjent" ligger utenfor denne leveransen.

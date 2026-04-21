# Product Vision: Markedsdrevet Vedlikehold

## Business Context

Akershus Energi (AE) er et norsk fornybarenergiselskap med ca. 99 ansatte, heleid av Akershus fylkeskommune. Selskapet opererer en diversifisert portefølje av vannkraft, vindkraft, fjernvarme, sol, hydrogen og batteri. Som kraftprodusent i det nordiske spotmarkedet er AEs inntjening direkte eksponert mot prisvolatilitet — og timing av produksjonsnedetid har dermed direkte resultatkonsekvenser.

Det europeiske markedet for prediktivt og markedsoptimert vedlikehold i energisektoren er i sterk vekst, med en CAGR på over 27 % frem mot 2033. Driverne er økt prisvolatilitet som følge av mer variabel sol- og vindkraft, samt et voksende behov for å optimere driftsøkonomi på eksisterende anlegg. For norske vannkraftprodusenter er utfordringen særlig relevant: vann kan lagres og produksjon kan forskyves, men vedlikehold krever planlagt nedetid — og den nedetiden bør ideelt sett falle i perioder med lavest mulig kraftpris.

I dag mangler AE et verktøy som kobler vedlikeholdsplanlegging og markedsdata. Drift og marked opererer i adskilte siloer, koordinert via telefon og Excel. Konsekvensen er at vedlikeholdsbeslutninger tas på bakgrunn av rutine og erfaring fremfor systematisk analyse av prisutsikter — med unødvendig inntektstap som resultat.

## Problem Statement

Driftsledere ved Akershus Energis kraftanlegg planlegger vedlikeholdsvindu uten systematisk tilgang til kraftprisprognoser. Vedlikeholdsplaner lages i Excel eller i vedlikeholdssystemer som ikke er integrert med markedsdata, og koordineringen med traders og markedsavdeling skjer ad hoc via telefon og e-post. Det finnes ingen felles arena der planlagt nedetid og forventede kraftpriser kan ses i sammenheng.

Resultatet er at vedlikehold legges til perioder som passer driftsmessig og logistisk, uten at man systematisk vurderer om det finnes lavprisdager i det aktuelle tidsrommet der inntektstapet ville vært vesentlig lavere. For et selskap med direkte eksponering mot spotmarkedet er dette et strukturelt problem som koster penger hver gang en turbin tas ut av drift på feil tidspunkt.

## Target User

**Primær bruker:** Driftsleder ved AEs kraftanlegg — ansvarlig for å planlegge og koordinere vedlikeholdsoppgaver, og for å balansere tekniske krav mot produksjonsmål.

**Brukerens jobb-som-skal-gjøres:** "Finne det tidspunktet for vedlikehold som er teknisk forsvarlig og gir minst mulig inntektstap — uten å måtte sy sammen data fra fem ulike steder."

**Sekundær bruker:** Markedsavdeling/traders — som i dag involveres ad hoc, men som har behov for forutsigbarhet om planlagt nedetid for å optimere porteføljestrategi.

**Nåværende alternativer:** Excel-regneark med egne prisvurderinger + manuell kommunikasjon med markedsavdeling. Vedlikeholdsplan og markedsdata lever i separate siloer uten systematisk kobling.

## Value Proposition

**For** driftsledere ved Akershus Energis kraftanlegg
**som** i dag planlegger vedlikeholdsvindu uten systematisk tilgang til kraftprisprognoser og tar beslutninger basert på rutine fremfor markedsdata,
**er Markedsdrevet Vedlikehold** et webbasert beslutningsverktøy
**som** anbefaler optimale vedlikeholdsvindu ved å vise estimert inntektstap for ulike plasseringer av nedetiden — slik at driftsleder kan velge tidspunktet som minimerer tapte inntekter.
**I motsetning til** dagens manuelle koordinering mellom drift og marked via Excel og telefon,
**gir det** ett samlet bilde der vedlikeholdsoppgaver og prisforventninger ses i sammenheng, med en konkret anbefaling om når nedetid bør legges.

## Core Features (Workshop Scope)

Disse funksjonene definerer v0.1 som skal bygges i workshopen:

1. **Vedlikeholdskalender med fleksibilitetsvindu:** Bruker legger inn en vedlikeholdsoppgave per anlegg med tre parametere: estimert varighet, tidligste mulige startdato, og seneste mulige startdato (fleksibilitetsvinduet). Verktøyet bruker dette vinduet som søkerom for anbefalingen.

2. **Tapskalkulator med scenariovisning:** Verktøyet beregner og viser estimert inntektstap for tre alternative plasseringer av vedlikeholdsvinduet innenfor det angitte tidsrommet. Beregningene baseres på manuelt inntastede prisforventninger (kr/MWh) og anleggets installerte effekt (MW). Brukeren ser alternativene side om side og kan sammenligne dem direkte.

3. **Anbefalingsmotor:** Basert på tapskalkulatoren fremhever verktøyet det anbefalte vedlikeholdsvinduet — det som gir lavest estimert inntektstap — med en tydelig visuell markering og et sammendrag av besparelsen sammenlignet med det dyreste alternativet.

## Future Roadmap (Post-Workshop)

- **Prisintegrasjon:** Automatisk henting av spotpris-prognoser fra Nord Pool eller interne tradingmodeller, slik at manuell inntasting av prisforventninger erstattes.
- **Flerannlegg-oversikt:** Dashboard som viser planlagte vedlikeholdsvindu på tvers av alle AEs anlegg, slik at markedsavdeling får samlet oversikt over forventet nedetid i porteføljen.
- **Historisk kalibrering:** Sammenligning av faktisk inntektstap mot estimat etter gjennomført vedlikehold — bygger over tid en treningsbase for bedre estimater.
- **Eksport og deling:** PDF-rapport eller delbar lenke som lar driftsleder dele anbefalt vedlikeholdsvindu med markedsavdeling og ledelse uten manuell kopiering.
- **Ressursplanlegging:** Kobling mot tilgjengelighet for mannskap og eksterne leverandører som et tilleggsfilter i anbefalingen.

## Success Criteria

Slik vet vi at v0.1 fungerer:

- Driftsleder kan legge inn et vedlikeholdsoppdrag og få opp tre alternative vindu med estimert inntektstap for hvert — i løpet av under 2 minutter.
- Anbefalingen er umiddelbart forståelig uten opplæring: det tydelig hvilket alternativ som anbefales og hva besparelsen er.
- Brukeren vurderer verktøyet som et bedre beslutningsgrunnlag enn dagens manuelle prosess med Excel og telefonkoordinering.

## Technical Assumptions

- **Produkttype:** React single-page application, norskspråklig UI
- **Primær teknologi:** React + Tailwind CSS + recharts for visualisering
- **Data:** Brukerinntastet data (anleggsnavn, effekt i MW, vedlikeholdsvarighet, fleksibilitetsvindu, prisforventninger per uke). Mock-data for demo av ett anlegg med forhåndsutfylte verdier.
- **Beregningslogikk:** Klient-side. Inntektstap = installert effekt (MW) × varighet (timer) × prisforventning (kr/MWh) for hvert scenario. Ingen backend eller ekstern API-integrasjon i v0.1.
- **Autentisering:** Ingen i v0.1 — enkeltbruker prototype.
- **Layout:** Desktop-first (brukes på kontor/møterom), men responsiv grunnstruktur.

## Workshop Implementation Notes

- **Estimert byggekompleksitet:** Medium
- **Viktigste risiko for workshop-bygg:** Tapskalkulatoren krever at beregningslogikken (effekt × tid × pris) er korrekt implementert og gir troverdige tall — feil her undergraver tillit til verktøyet. Anbefalt byggerekkefølge: (1) skjema for innlegging av vedlikeholdsoppgave, (2) tapsberegning og scenariovisning, (3) anbefalingsmarkering.
- **Foreslåtte Spec Kit-begrensninger:** Bruk recharts for alle visualiseringer. All state i React (ingen ekstern state management). Mock-data i en JSON-fil med ett forhåndsutfylt anlegg (f.eks. "Nitelva kraftverk", 12 MW). Norsk som eneste språk i UI. Ingen lagring til backend — state nullstilles ved refresh.

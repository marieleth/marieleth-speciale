# Marie Leth Hansen - speciale
## Introduktion
Dette repository indeholder do-fil og dataanalyser til mit speciale fra 2023 på Institut for Statskundskab ved Aarhus Universitet. I specialet afdækkes vælgernes kompetencevurdering af mandlige og kvindelige politikere. Det vil derudover undersøges hvorvidt forskelle i vælgernes "Personal Need for Structure" påvirker sammenhængen mellem en politikers køn og sandsynligheden for at stemme på politikeren, samt hvorvidt politikerens ideologiske blok påvirker denne sammenhæng. Do-filen er struktureret ud fra nedenstående beskrivelse.

Forskningsdesign og undersøgelse af specialets 4 hypoteser: I projektet benyttes et eksperimentelt undersøgelsesdesign, udformet som et survey-eksperiment med vignetter. Indledningsvist er de primære variable kodet og de eksperimentelle grupper dannet. 
For at teste specialets hypotese 1 og 2 afdækkes den kausale effekt af at læse beskrivelsen af en mandlig eller kvindelig politiker på deres kompetence til at varetage traditionelt maskuline og feminine ansvarsområder. Analysen er udført ved brug af OLS-regression. 
For at teste hypotese 3 er der foretaget en OLS-regression med robuste standardfejl af effekten af politikerens køn på sandsynligheden for at stemme på politikeren i en situation med øget dansk sikkerhedstrussel og med graden af ”Personal Need for Structure” som interaktionsled. 
På baggrund af hypotese 4 forventes det, at effekten af "Personal Need for Structure" på sammenhængen mellem kandidatens køn og stemmeadfærd vil være mindre, når vælgerne præsenteres for en kandidat fra blå blok. For at teste hypotese 4 er der udført en trevejsinteraktion, hvor den fiktive politikers blok indgår som interaktionled på sammenhængen mellem politikerens køn og graden af "Personal Need for Structure" på sandsynlighed for at stemme på politikeren.

Forudsætningstest: Der er foretaget relevante forudsætningstest; herunder undersøgelse af indflydelsesrige observationer via Cooks D, test for normalfordelte fejlled, White’s test for homoskedasticitet samt undersøgelse af multikollinearitet. 

Manipulationstjek:  Som en del af projektet er det blevet undersøgt om manipulation af den uafhængige variabel (den fiktive politikers køn) samt den danske sikkerhedssituation har virket efter hensigten. Dette testes konkret igennem en chi-i-anden test.

Randomisering og balancetest:  Idet automatisk randomisering i SurveyXact ikke var muligt, er der også opstillet en række balancetabeller for at teste om randomisering ud fra fødselsdag var succesfuld. Konkret er dette gjort ved udregning af gennemsnit for baggrundsvariablene: køn, uddannelsesniveau, ideologi, geograf, alder og chi-i-anden-test foretages.

Poweranalyse: Der er som en del af analysen foretaget en power-calculation på baggrund af de forventede effektstørrelser. Dette blev gjort for at fastlægge hvilken størrelse stikprøven som minimum burde være for at kunne måle effekterne. Dette gøres da størrelsen på en undersøgelses sample og risikoen for type 2 fejl er inverst forbundet. Det betyder at jo mindre statisk power en undersøgelse har desto større risiko for type 2 fejl. 

## Indhold
- DO-FILE-SPECIALE.do: Stata-kode til dataanalysen
- README.md: Denne fil

## Data
Analysen beror på data som er indsamlet fra den 29. november til d. 18. december 2022 igennem et onlinespørgeskema, og analysens sample kan betegnes som et convenience sample. I samlet indgår 672 respondenter i analysen. Selve datafilen er ikke inkluderet i dette repository. Data kan rekvireres efter forespørgsel.

## Kontakt
Har du spørgsmål til projektet, er du velkommen til at skrive til mig på marielhansen94@gmail.com


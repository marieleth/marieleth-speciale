***********************************DO-FILE SPECIALE***********************************

***********************************Indledende kodning af variable og eksperimentelle grupper***********************************
/*Frasortering af respondenter med missing-værdier*/
keep if udsattemand == 1 | udsattemand == 2 | udsattemand == 3 | udsattemand == 4 | udsattemand == 5 | udsattekvinde == 1 | udsattekvinde == 2 | udsattekvinde == 3 | udsattekvinde == 4 | udsattekvinde == 5

/*Kommando til kodning alder i intervalkategori*/
recode alder (1=1) (2=1) (3=1) (4=1) (5=1) (6=1) (10=1) (11=1) (12=1) (13=1) (14=1) (15=1) (16=2) (17=2) (18=2) (19=2) (20=2) (21=2) (22=2) (24=2) (25=2) (26=2) (27=3) (28=3) (29=3) (30=3) (31=3) (32=3) (33=3) (34=3) (35=3) (36=3) (37=4) (38=4) (39=4) (40=4) (41=4) (42=4) (43=4) (44=4) (45=4) (46=4) (47=5) (48=5) (49=5) (50=5) (51=5) (52=5) (53=5) (54=5) (55=5) (56=5) (57=6) (58=6) (59=6) (60=6) (61=6) (62=6) (63=6) (64=6) (65=6) (66=6)(67=6) (68=6) (69=6) (70=6) (71=6) (72=6) (86=6), generate(intervalalder)
label define intervalalderLB 1 "18-29 år" 2 "30-39 år" 3 "40-49 år" 4 "50-59 år"  5 "60-69 år" 6 "+70 år"
label values intervalalder intervalalderLB


/*Danne de 4 eksperimentelle grupper*/
recode fødselsdag (1=1) (5=1) (9=1) (13=1) (17=1) (21=1) (25=1) (29=1) (2=2) (6=2) (10=2) (14=2) (18=2) (22=2) (26=2) (30=2) (3=3) (7=3) (11=3) (15=3) (19=3) (23=3) (27=3) (31=3)(4=4) (8=4) (12=4) (16=4) (20=4) (24=4) (28=4), generate(defiregrupper)
label define defiregrupperLB 1 "Kvinde, rød blok" 2 "Mand, rød blok" 3 "Kvinde, blå blok" 4 "Mand, blå blok"
label values defiregrupper defiregrupperLB

/*De to eksperimentelle grupper*/
recode fødselsdag (1=0) (5=0) (9=0) (13=0) (17=0) (21=0) (25=0) (29=0) (2=1) (6=1) (10=1) (14=1) (18=1) (22=1) (26=1) (30=1) (3=0) (7=0) (11=0) (15=0) (19=0) (23=0) (27=0) (31=0) (4=1) (8=1) (12=1) (16=1) (20=1) (24=1) (28=1), generate(detokøn)
label define detokønLB 0 "Kvinde" 1 "Mand"
label values detokøn detokønLB


/*Kommando til samkodning af den afhængige variabel og omkodning til 0-1-skala*/
egen forsvar6=rowmean(forsvarkvinde forsvarmand)
egen skat=rowmean(skattekvinde skattemand)
egen ældre=rowmean(ældrekvinde ældremand)
egen udsatte=rowmean(udsattekvinde udsattemand)


***********************************Test af hypotese 1 og 2***********************************
reg forsvar6 i.detokøn uddannelse, r
reg skat i.detokøn uddannelse, r
reg ældre i.detokøn uddannelse, r
reg udsatte i.detokøn udannelse, r

margins, dydx(detokøn) over(forsvar6)
margins, dydx(detokøn) over(ældre)


***********************************Test af hypotese 3***********************************
/*Kodning af NFS-index og afrunding af decimaler*/
alpha nfs1 nfs2 nfs3 nfs4 nfs5 nfs6, casewise generate(nfsindex1)
replace nfsindex1 = trunc(nfsindex1)

/*Kommando til samkodning af den afhængige variabel: "sandsynlighed for at stemme" */
egen stemme=rowmean(stemmemand stemmekvinde)


/*Selve interaktionen og med marginsplot*/
reg stemme i.detokøn##c.nfsindex1,r
reg stemme i.detokøn##c.nfsindex1 uddannelse køn aldertal region, r

margins, dydx(detokøn) over(nfsindex1)
marginsplot, recast(scatter) yline(0) xscale(range(2 4)) title("") xtitle ("NFS-index") ytitle ("Marginaleffekt af den mandlige politiker") legend(on label(1 "95% konfidensinterval") label(2 "Marginaleffekt")) scheme(s1mono)

margins, dydx(detokøn) over(forsvar6 ældre udsatte)
marginsplot, recast(scatter) yline(0) xscale(range(2 4)) title("") xtitle ("NFS-index") ytitle ("Marginaleffekt af den mandlige politiker") legend(on label(1 "95% konfidensinterval") label(2 "Marginaleffekt")) scheme(s1mono)
mean udsatte, over(detokøn)


***********************************Test af hypotese 4***********************************
/*Danne to grupper til de to blokker*/
recode fødselsdag (1=0) (5=0) (9=0) (13=0) (17=0) (21=0) (25=0) (29=0) (2=0) (6=0) (10=0) (14=0) (18=0) (22=0) (26=0) (30=0) (3=1) (7=1) (11=1) (15=1) (19=1) (23=1) (27=1) (31=1) (4=1) (8=1) (12=1) (16=1) (20=1) (24=1) (28=1), generate(politikerblok)
label define politikerblokLB 0 "Rød blok" 1 "Blå blok"
label values politikerblok politikerblokLB

/*Regressionen af trevejsinteraktionen*/
reg stemme i.detokøn##c.nfsindex1##politikerblok
reg stemme i.detokøn##c.nfsindex1##politikerblok uddannelse køn aldertal region

reg stemme i.detokøn##c.nfsindex1 if politikerblok == 0
margins, dydx(detokøn) over(nfsindex1)
marginsplot, recast(scatter) yline(0) xscale(range(2 4)) title("Figur 6a: Præsenteret for politiker fra rød blok") xtitle ("PNS-index") ytitle ("Marginaleffekt af den mandlige politiker") legend(on label(1 "95% konfidensinterval") label(2 "Marginaleffekt")) scheme(s1mono)

reg stemme i.detokøn##c.nfsindex1 if politikerblok == 1
margins, dydx(detokøn) over(nfsindex1)
marginsplot, recast(scatter) yline(0) xscale(range(2 4)) title("Figur 6b: Præsenteret for politiker fra blå blok") xtitle ("PNS-index") ytitle ("Marginaleffekt af den mandlige politiker") legend(on label(1 "95% konfidensinterval") label(2 "Marginaleffekt")) scheme(s1mono)


***********************************Forudsætningstest med interaktion af køn***********************************
/*Fjerne "andet køn"*/
recode køn (3/.=.)

/*Selve trevejsinteraktionen*/
recode køn (1=0) (0=1), generate(køn1)
label define køn1LB 0 "Kvindelig respondent" 1 "Mandlig respondent"
label values reskøn køn1LB

reg forsvar6 i.detokøn##reskøn, r
margins, dydx(detokøn) over(reskøn)
marginsplot, recast(scatter) yline(0) xscale(range(-1.5 3)) title("Vurdering af politikerens kompetencer til at håndtere forsvars- og udenrigspolitik") xtitle ("Respondentens køn") ytitle ("Marginaleffekt af den mandlige politiker") legend(on label(1 "95% konfidensinterval") label(2 "Marginaleffekt")) scheme(s1mono)

reg skat i.detokøn##reskøn, r
margins, dydx(detokøn) over(reskøn)
marginsplot, recast(scatter) yline(0) xscale(range(-1.5 3)) title("Vurdering af politikerens kompetencer til at håndtere skat") xtitle ("Respondentens køn") ytitle ("Marginaleffekt af den mandlige politiker") legend(on label(1 "95% konfidensinterval") label(2 "Marginaleffekt")) scheme(s1mono)

reg ældre i.detokøn##reskøn, r
margins, dydx(detokøn) over(reskøn)
marginsplot, recast(scatter) yline(0) xscale(range(-1.5 3)) title("Vurdering af politikerens kompetencer til at håndtere ældrepleje") xtitle ("Respondentens køn") ytitle ("Marginaleffekt af den mandlige politiker") legend(on label(1 "95% konfidensinterval") label(2 "Marginaleffekt")) scheme(s1mono)

reg udsatte i.detokøn##reskøn, r
margins, dydx(detokøn) over(reskøn)
marginsplot, recast(scatter) yline(0) xscale(range(-1.5 3)) title("Vurdering af politikerens kompetencer til at håndtere socialt udsatte") xtitle ("Respondentens køn") ytitle ("Marginaleffekt af den mandlige politiker") legend(on label(1 "95% konfidensinterval") label(2 "Marginaleffekt")) scheme(s1mono)

reg stemme i.detokøn##c.nfsindex1##køn uddannelse køn alder region ideologi, r
reg stemme i.detokøn##c.nfsindex1##politikerblok##køn uddannelse køn alder region ideologi, r


***********************************Andre forudsætningstest***********************************
/*TEST 1: FRAVÆR AF INFFLYDELSESRIGE OBSERVATIONER*/
/*Leverage-Versus-residual plot*/
reg forsvar6 i.detokøn
lvr2plot

reg skat i.detokøn
lvr2plot

reg ældre i.detokøn
lvr2plot

reg udsatte i.detokøn
lvr2plot

/*Cooks d*/
reg forsvar6 i.detokøn
predict tempcooksd, cooksd
display 4/e(N)
summarize tempcooksd if tempcooksd >(4/e(N))

reg skat i.detokøn
predict tempcooksd, cooksd
display 4/e(N)
summarize tempcooksd if tempcooksd >(4/e(N))

reg ældre i.detokøn
predict tempcooksd, cooksd
display 4/e(N)
summarize tempcooksd if tempcooksd >(4/e(N))

reg udsatte i.detokøn
predict tempcooksd, cooksd
display 4/e(N)
summarize tempcooksd if tempcooksd >(4/e(N))


/*TEST 2: NORMALTFORDELT FEJLLED*/
reg forsvar6 i.detokøn,r
predict tempresrid, residuals 
histogram tempresrid, normal 

reg skat i.detokøn,r
predict tempresrid, residuals 
histogram tempresrid, normal 

reg ældre i.detokøn,r
predict tempresrid, residuals 
histogram tempresrid, normal 

reg udsatte i.detokøn,r
predict tempresrid, residuals 
histogram tempresrid, normal 

reg stemme i.detokøn,r
predict tempresrid, residuals 
histogram tempresrid, normal 


/*TEST 3: VARIANSHOMOGENITET*/
/*White's test*/

reg forsvar6 i.detokøn
estat imtest,white

reg skat i.detokøn
estat imtest,white

reg ældre i.detokøn
estat imtest,white

reg udsatte i.detokøn
estat imtest,white

reg stemme i.detokøn
estat imtest,white

reg udsatte i.detokøn##nfs
estat imtest,white


/*TEST 4: FRAVÆR AF PERFEKT (OG STÆRK MULTIKOLLINEARITET)*/
reg udsatte i.detokøn
vif


***********************************Manipulationstjek***********************************
/*Manipulationstjek: politikerens køn*/
tab manitjekpolitiker defiregrupper, chi

/*Manipulationstjek: den danske sikkerhedssituation*/
tab manitjeksikkerhed defiregrupper, chi

recode fødselsdag (1=1) (5=1) (9=1) (13=1) (17=1) (21=1) (25=1) (29=1) (2=2) (6=2) (10=2) (14=2) (18=2) (22=2) (26=2) (30=2) (3=1) (7=1) (11=1) (15=1) (19=1) (23=1) (27=1) (31=1)(4=2) (8=2) (12=2) (16=2) (20=2) (24=2) (28=2), generate(detogrupper)
label values detogrupper detogrupperLB
label define detogrupperLB 1 "Kvinde" 2 "Mand"
tab manitjeksikkerhed detogrupper, chi

recode fødselsdag (1=1) (5=1) (9=1) (13=1) (17=1) (21=1) (25=1) (29=1) (2=1) (6=1) (10=1) (14=1) (18=1) (22=1) (26=1) (30=1) (3=1) (7=1) (11=1) (15=1) (19=1) (23=1) (27=1) (31=1)(4=1) (8=1) (12=1) (16=1) (20=1) (24=1) (28=1), generate(allegrupper)
label values allegrupper allegrupperLB
label define allegrupperLB 1 "Alle respondenter"

generate kvindevar4 = fødselsdag if fødselsdag == 1 | fødselsdag == 5 | fødselsdag == 9 | fødselsdag == 13 | fødselsdag == 17 | fødselsdag == 21 | fødselsdag == 25 | fødselsdag == 29 | fødselsdag == 3 | fødselsdag == 7 | fødselsdag == 11 | fødselsdag == 15 | fødselsdag == 19 | fødselsdag == 23 | fødselsdag == 27 | fødselsdag == 31
tab kvindevar4

generate mandevar = fødselsdag if fødselsdag == 2 | fødselsdag == 6 | fødselsdag == 10 | fødselsdag == 14 | fødselsdag == 18 | fødselsdag == 22 | fødselsdag == 26 | fødselsdag == 30 | fødselsdag == 4 | fødselsdag == 8 | fødselsdag == 12 | fødselsdag == 16 | fødselsdag == 20 | fødselsdag == 24 | fødselsdag == 28
tab mandevar

tab manitjeksikkerhed

label define manitjeksikkerhedLB 1 "Sikkerhedstrussel mod Danmark" 2 "Corona-virus i Danmark" 3 "Det danske sundhedsvæsen" 4 "Den grønne omstilling i Danmark" 88 "Ved ikke"
label values manitjeksikkerhed manitjeksikkerhedLB

graph bar (count) kvindevar4 mandevar, over(manitjeksikkerhed)


/*Manipulationstjek: grad af bekymring*/
tab bekymringsikkerhed
recode bekymringsikkerhed (88/.=.)


***********************************Balancetabel***********************************
/*Test 1: respondentens køn*/
recode køn (3/.=.)
mean køn, over (defiregrupper)
tab køn defiregrupper, chi

/*Test 2: Uddannelse*/
tab defiregrupper uddannelse, chi
tabulate defiregrupper uddannelse, row

/*Test 3: Ideologi*/
recode ideologi (88/.=.)
mean ideologi, over (defiregrupper)
tab ideologi defiregrupper, chi

/*Test 4: Bopælsregion*/
tab defiregrupper region, chi
tabulate defiregrupper region, row
tabulate region

/*Test 5: Alder*/
*Omkodning af alder til faktiske alder*
recode alder (1=18) (2=19) (3=20) (4=21) (5=22) (6=23) (10=24) (11=25) (12=26) (13=27) (14=28) (15=29) (16=30) (17=31) (18=32) (19=33) (20=34) (21=35) (22=36) (24=37) (25=38) (26=39) (27=40) (28=41) (29=42) (30=43) (31=44) (32=45) (33=46) (34=47) (35=48) (36=49) (37=50) (38=51) (39=52) (40=53) (41=54) (42=55) (43=56) (44=57) (45=58) (46=59) (47=60) (48=61) (49=62) (50=63) (51=64) (52=65) (53=66) (54=67) (55=68) (56=69) (57=70) (58=71) (59=72) (60=73) (61=74) (62=75) (63=76) (64=77) (65=78) (66=79) (67=80) (68=81) (69=82) (70=83) (71=84) (72=85) (86=98), generate(aldertal)
mean aldertal, over (defiregrupper)
tab aldertal defiregrupper, chi
mean aldertal

***********************************Poweranalyse***********************************
power twomeans 0 (0.10 0.15 0.20 0.25), power (0.80)


***********************************Udregning af justeret R2***********************************
regress forsvar6 i.detokøn uddannelse,r
r2_a

regress skat i.detokøn uddannelse,r
r2_a

regress ældre i.detokøn,r
r2_a

regress udsatte i.detokøn,r
r2_a

reg stemme1 i.detokøn##c.pnsindex1 uddannelse køn aldertal region ideologi,r
r2_a

reg stemme1 i.detokøn##c.pnsindex1##politikerblok uddannelse køn aldertal region ideologi,r
r2_a

mean ældre, over(detokøn)
sum ældre

mean udsatte, over(detokøn)
sum udsatte

mean 

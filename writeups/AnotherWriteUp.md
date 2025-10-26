# My second Writeup
For tredje år på rad avholder (N)PST en julekalender med små og store oppgaver. I år var det rimelig likt 2020, med samme plattform og metode for å sende inn flagg, noen egg å finne, og stor variasjon i typer oppgaver. Nytt av året var fri på mandager, oppgaveslipp klokka 18, og at egg var verdt 1 poeng i stedet for kun en stjerne i margen. Det var også en kobling mot E-tjenesten sin CyberTalent-CTF, for den som ville prøve seg på noe mer utfordrende.

Writeups her forsøker ikke å belyse alle måter å løse oppgavene på, og kodesnuttene er renskrevet noe etter at en oppgave er løst, så det er mulig å forstå hva som skjer for de som ikke løste den.


## Table of Contents
- [Dag 1 - Velkommen](#dag-1---velkommen)
- [Dag 2 - Huskelapp](#dag-2---huskelapp)

## Dag 1 - Velkommen

```
Fra: HR
Sendt: 1.12.2021
Til: <brukernavn>
Emne: Velkommen til DASS!

Velkommen <brukernavn>!

Veldig hyggelig å ha deg ombord og fint å se at du har funnet veien inn til DASS. For at du skal finne deg mer til rette anbefaler jeg deg å sette ditt eget preg på systemet! Dette kan du gjøre ved å velge «Mal» fra startmenyen, mal din egen skrivebordsbakgrunn og velg Fil -> Sett som skrivebordsbakgrunn. Her er det bare kreativiteten som setter begrensninger, men i tilfelle du trenger litt starthjelp, legger jeg ved et eksempelbilde.

Spent på å følge deg videre, lykke til!

Hilsen HR

📎eksempel_bakgrunnsbilde.png
```

Vedlagt er et vakkert bilde av julenissen på sleden sin. Det virker ikke som om det er noe gjemt på selve bildet, men ulike verktøy viser at det er noe spesielt oppe i venstre hjørne, hvor pixlene varierer litt i stedet for å ha vanlig bakgrunnsfarge. Dette minner om LSB-steganografi, hvor noen bits med data er inkorporert i de siste bitsene i hver farge i noen pixler. `zsteg` kan sjekke en del slike konfigurasjoner.

```
$ ~/bin/zsteg eksempel_bakgrunnsbilde.png
imagedata           .. file: MIPSEB Ucode
b1,b,lsb,xy         .. file: GLS_BINARY_LSB_FIRST
b1,rgb,lsb,xy       .. text: "PST{HelloDASS}"
...
```

Dagens flagg er `PST{HelloDASS}`

```
Fra: HR
Sendt: 1.12.2021
Til: <brukernavn>
Emne: Re: Velkommen til DASS!

Bra jobba <brukernavn>! Mellomleder tar kontakt med deg i morgen med mer konkret informasjon angående hva du skal jobbe med.
```



## Dag 2 - Huskelapp

```
Fra: Mellomleder
Sendt: 2.12.2021
Til: <brukernavn>
Emne: Huskelapp

Velkommen til teamet <brukernavn>!

Vi går rett på sak. I fjor rakk ikke julenissen å dele ut pakker til alle som hadde gjort seg fortjent. For å komme til bunns i årsaken ble det satt ned et utvalg med mandat til å utnevne en kommisjon som skulle starte arbeidet med opprettelsen av en granskningskommité. Da granskningskommiteen kom med sin utredelse viste det seg at mulighetsrommet for å utøve slemme handlinger ble betraktelig redusert ved nedstenging og isolasjon. Det hadde rett og slett blitt for mange snille barn.

Da nedstenging og isolasjon delvis har vedvart, har det høy prioritet i år å finne en ny, mer optimal rute.

Julenissen fant i går en huskelapp som han tror kan være relevant, men han klarer ikke å finne ut av hva han skulle huske. Kunne du hjulpet han med det?

Mvh Mellomleder

📎huskelapp_til_2021.txt
```

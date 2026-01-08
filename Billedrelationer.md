# Billederelationer

databasen består af
- tabeller med faciliteter
- tabel med fotos

Ved upload får foto et objektID og navnet afspejler ID (og en stedfæstelse - ud fra fotos lokation)

Der skal (herefter) dannes en relation mellem facilitet og foto

Gennem GeoFA-editor og GeoFA-webkortet kan relation (tabel med objektID) skabes

I fototabellen er der i databasen mulighed for attributter
Disse kan i dag ikke udfyldes gennem GeoFA - men Udinaturen tilbyder denne mulighed

Data om fotos kan udtrækkes gennem SQL-api

OBS "Vilkår for anvendelse af fotos: Vær opmærksom på, at ved upload af billeder, så stilles de til fri afbenyttelse. Hvis der er angivet en copyright, skal denne akkrediteres. Du finder copyright, som en attribut til det pågældende foto."

------


Billeddelen er dokumenteret i 5.79 Billedunderstøttelse – men er dette fyldestgørende???
Vi ved, at mange har svært ved at forstå det med billeddelen. Vi har fået følgende kommentar fra en GIS-leverandør: ”Billedhåndtering er også en vigtig del, især i forhold til tabellernes opbygning, database-struktur og relationer. Det ville desuden være en fordel med konkrete eksempler – ligesom dem, I har i jeres API-vejledning på GitHub.”

Det er også vigtigt, at beskrivelsen redegør for 
-	Forskellen mellem foto_link og geofafoto
-	Mulighed for at markere om et foto skal være primært (vises først)
-	Mulighed for at markede om et foto viser stedets/facilitetens tilgængelighed (”handicapvenlighed”)
-	Mulighed for at angive evt. copyright
-	Mulighed for at angive en billedtekst
-	Mulighed for at angive tilgængelighedstekst – beskrivelse af fotoets indhold til højtlæsning på hjemmesiden
Skriv gerne generelt om copyright til fotos, der uploades til GeoFA:
1.	At ved upload af billeder til GeoFA, så stilles de til fri afbenyttelse.
2.	At GeoFA generelt er underlagt Creative Commons Licens (CC BY)
3.	At hvis der er angivet en ”copyright”, skal denne konkret akkrediteres
4.	For at overholde persondatalovgivningen er det vigtigt, at sikre sig samtykkeerklæringer på de billeder, hvor det lovmæssigt kræves. Et billede af en genkendelig/identificerbar person udgør en oplysning om personen (en personoplysning). Det gælder, uanset om billedet er ledsaget af en tekst, der identificerer den pågældende. Som udgangspunkt gælder databeskyttelsesreglerne derfor. Det er imidlertid ikke i alle tilfælde, at offentliggørelse af billeder på internettet af genkendelige personer er omfattet af reglerne i databeskyttelsesforordningen, f.eks. billeder af publikum til en koncert, billeder af besøgende i en zoologisk have eller lignende. Du kan læse mere her: https://www.datatilsynet.dk/hvad-siger-reglerne/vejledning/internet-medier-og-apps-/billeder-paa-internettet



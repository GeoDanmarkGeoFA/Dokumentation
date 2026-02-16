# Billederelationer i GeoFA

GeoFA indeholder en billeddatabase. I denne database kan man indlægge billeder som *kan* tilknyttes et eller flere geometriske elementer, der er registreret i GeoFA. Det er således muligt at skabe en M:N relation (mange-til-mange relation) mellem billeder og registrerede features i GeoFA.

I simple tilfælde kan man indlægge billeder til de tre friluftslag gennem GeoFA Editoren som beskrevet i afsnittet herunder.

Man kan også benytte GeoFAs faciliteter direkte, hvilket beskrives i afsnittene længere nede.

## Billeder gennem GeoFA Editor 

Gennem GeoFA Editor kan man tilføje billeder til GeoFAs billeddatabase og skabe forbindelsen mellem de enkelte billeder og elementer lagt ind i de tre lag under **Friluftsliv**

En (ældre) video om hvordan man bruger GeoFA Editor til at uploade og tilknyte billeder til faciliteter kan ses her https://vimeo.com/710733192/725175d9f6 

## Teknisk beskrivelse af fotos i GeoFA

Overordnet set skal man først oprette metaoplysninger om sit foto i tabellen *t_7901_foto* i GeoFA. Dernæst skal billedfilen sendes til GeoFA-systemet. Til sidst skal man tilføje sin (M:N) forbindelse mellem sit (nyindlagte) foto og det element i GeoFA, det skal være tilknyttet - det sker i tabellen *t_7900_fotoforbindelse*. 

Når man kan benytter GeoFA Editoren til først at uploade billeder, får man skabt en række i tabellen *t_7901_foto* og fotoet lægges ind i systemet - disse trin sker automatisk.

Når man derefter tilknytter billedet til et element i GeoFA (for eksempel til et shelter) får man skabt en række i tabellen *t_7900_fotoforbindelse* - dette sker ligeledes automatisk.

GeoFA Editoren kan kun bruges til at skabe fotoforbindelse mellem billeder og elementer i de tre lag under **Friluftsliv**. Man kan dog bruge andre metoder til at  tilknytte billeder til et vilkårlig element i et vilkårligt lag i GeoFA.

Selve billedet ligger ikke i GeoFA databasen, men ligger i et eksternt repo/bibliotek. GeoFA danner ved upload af billeder flere udgaver af billederne. Den komplette beskrivelse findes i GeoFA Specifikationen.

### Tabellen t_7901_foto

Tabelen *t_7901_foto* indeholder information om et billede som er lagt ind i GeoFA's billededatabase. Generelt følger indholdet i tabellen den generelle datastruktur i GeoFA. Følgende felter kan fremhæves:

- **object_id**
  Denne UUID skal bruges når man uploader billed-filen til systemet og når man skaber forbindelse mellem foto og GeoFA feature.

- **copyright**

  Feltet kan bruges til at angive en særlig information om ophavsret til det pågældende billede. Typisk vil man indsætte fotografens navn. Ved brug af GeoFA Editor udfyles feltet ikke.

- **billedtekst**

  Feltet kan bruges til at angive en billedtekst til det pågældende billede. Ved brug af GeoFA Editor udfyles feltet ikke.

- **alt_tekst**

  Feltet kan bruges til at angive en alternativ tekst til det pågældende billede. Alt_tekst benyttes typisk til oplæsning af hjemmesider eller andre hjælpefunktioner. Ved brug af GeoFA Editor udfyldes feltet ikke.

Resultatet af en indlæggelse af en billede er en ny række i tabellen t_7901_foto der er identificeret med et *objekt_id* af typen UUID. Denne værdi skal bruges i det videre arbejde for at skabe forbindelse med en eller flere faciliteter registreret i GeoFA.

### Indlægning af billede
Ønsker man at indlægge et billede i GeoFA's billededatabse udenom GeoFA Editoren, skal man først oprette en række i tabellen *t_7901_foto*. Det UUID man får retur skal benttes i nedenstående kald.

Et billede uploades til billedbiblioteket og navngives med med det returnerede objekt_id (returneret UUID='9d69bbf8-ef8a-11f0-be38-2b4180875278'):

```
curl -XPOST --header "Content-Type: multipart/form-data" "https://fkg.mapcentia.com/extensions/fkgmedia/api/image" -F files[]=@mit_billede.JPG -F names[]=9d69bbf8-ef8a-11f0-be38-2b4180875278.jpg
```

### Tabellen t_7900_fotoforbindelse

Tabellen *t_7900_fotoforbindelse* indeholder information om, hvilke billeder der er tilknyttet specifikke features i GeoFA. Generelt følger indholdet i tabellen den generelle datastruktur i GeoFA. Nedenstående felter kan nævnes som særligt vigtige:

- **fkg_tema**

  Angiver hvilket tema i GeoFA som denne tilknytning til et billede har. Indholdet angiver både schema- og tabelidenifikation, eksempelvis *t_5800_fac_pkt*. Feltet skal udfyldes og bliver automatisk udfyldt, hvis man bruger GeoFA Editoren.

- **foto_objek**

  Angiver hvilket *objekt_id* som billedtilkytningen vedrører. Det angivne objekt_id skal findes i den tabel, som er angivet i feltet *fkg_tema*. Feltet skal udfyldes og bliver automatisk udfyldt, hvis man bruger GeoFA Editoren.

- **foto_lokat**

  Angiver *objekt_id* for det billede, man ønsker at tilknytte. Det angivne objekt_id skal findes i tabellen *t_7901_foto*. Feltet skal udfyldes og bliver automatisk udfyldt hvis man bruger GeoFA Editoren.

- **foto_navn**

  Feltet kan benyttes til at angive et navn til billedet i forbindelse med den aktuelle tilknytning. Ved brug af GeoFA Editor udfyldes feltet ikke.

- **primaer_kode**

  Feltet angiver om fototilknytningen vedrører det primære (vigtigste) foto til en feature i GeoFA. Feltet kan indeholde 0 eller 1 som oversættes i feltet *primaer* til *Nej* eller *Ja*. Systemet der anvender data fra tabellen kan således benytte feltet til at vise det mest relevante billede først. Ved brug af GeoFA Editor udfyldes primaer_kode automatisk med *0* (*Nej*).

- **tilgaeng_kode**

  Feltet angiver om fototilknytningen viser noget som er relevant for stedets/facilitetens tilgængelighed for personer med reduceret mobilitet (PRM). Feltet kan indeholde 0 eller 1 som oversættes i feltet *tilgaeng* til *Nej* eller *Ja*. Feltet kan bruges af det anvendende system til at udvælge fotos af særlig relevans for PRM. Ved brug af GeoFA Editor udfyldes automatisk med *0* (*Nej*).

### URL til indlagte billeder
Billedbiblioteket er cloud-baseret og når et billede uploades, genereres automatisk fire afledte versioner af billedet i forskellige opløsning/antal pixels. Samme billede er således tilgængeligt i 4 bredder (hhv. 171, 360, 560 og 1600 pixels). På denne måde er det muligt at henvise til det billede, som passer bedst i størrelsen på den aktuelle anvendelse.

Her ses fire eksempler på url til samme billede i forskellige størrelser :

https://geofa-foto.geodanmark.dk/1600/9d69bbf8-ef8a-11f0-be38-2b4180875278.jpg

https://geofa-foto.geodanmark.dk/560/9d69bbf8-ef8a-11f0-be38-2b4180875278.jpg

https://geofa-foto.geodanmark.dk/360/9d69bbf8-ef8a-11f0-be38-2b4180875278.jpg

https://geofa-foto.geodanmark.dk/171/9d69bbf8-ef8a-11f0-be38-2b4180875278.jpg


Den værdi (*9d69bbf8-ef8a-11f0-be38-2b4180875278*) som står i de ovenstående URLer, er den samme som objekt_id for billedet i tabellen *t_7901_foto*.

## Billedfelter på lag

Billeder indlagt i GeoFAs fotodatabase som beskrevet ovenfor vil kunne hentes automatisk i de lag som GeoFA udstiller. Hvilke lag der automatisk viser tilknyttede billeder og hvilket felter de viser, fremgår af GeoFA specifikationen.

Det typiske eksempel der gennemgås her, viser hvilke felter der indeholder billedeinformationer for laget *Friluftsliv faciliteter, punkter (5800)* (i tabellen *fkg.t_7900_fotoforbindelse*).

> [!IMPORTANT]
> Langt fra alle lag i GeoFA indeholder billedfelter. Se hvilke i GeoFA specifikationen.

### Attributfelter vedrørende billeder

I GeoFA findes der typisk et eller flere felter man som bruger (dataejer) kan udfylde med et (ekstern for GeoFA) link til et billede, som man bruger selv har hosted et andet sted. GeoFA kontrollerer ikke billedet og ansvaret for billedet påhviler alene den der har lagt billedet op.

På friluftslagene findes der *foto_link*, *foto_link1*, *foto_link2* og *foto_link3*.

### Felter der vedrører billeder fra GeoFAs fotobibliotek

De (første fire) fotos der tilføjes til en facilitet i GeoFA som beskrevet ovenfor vil være tilgængelige direkte som felter der indeholder en direkte URL til billedet i GeoFA's billedbibliotek. Vedligeholdelsen af disse felters indhold sker automatisk af GeoFA ud fra registreringen af fototilknytningen i tabellen *fkg.t_7900_fotoforbindelse*.

På friluftslagene findes der *geofafoto*, *geofafoto1*, *geofafoto2* og *geofafoto3*.

### Lignende felter

På de tre lag under friluftsliv findes der et felt *gpx_link*, der indeholder et link til en GPX-fil for faciliteten. GPX-filer kan hentes og indlægges på løbeure og lignende devices. Brug af disse filer ligger uden for GeoFAs område.

## Ophavsret mv.
Undlad at uploade billeder til GeoFA som du ikke har ophavsretten til.

**Generelt gælder det**

1.	At billeder uploaded til GeoFA stilles til fri benyttelse
2.	At GeoFA og brug heraf generelt er underlagt Creative Commons Licens (CC BY)
3.	At hvis der er angivet en ”copyright”, skal denne konkret akkrediteres
4.	For at overholde persondatalovgivningen er det vigtigt, at sikre sig samtykkeerklæringer på de billeder, hvor det lovmæssigt kræves.

**Om billeder omfattet af persondatalovgivningen**

Et billede af en genkendelig/identificerbar person udgør en oplysning om personen (en personoplysning). Dette gælder, uanset om billedet er ledsaget af en tekst, der identificerer den pågældende. Som udgangspunkt gælder databeskyttelsesreglerne derfor. Det er imidlertid ikke i alle tilfælde, at offentliggørelse af billeder på internettet af genkendelige personer er omfattet af reglerne i databeskyttelsesforordningen, f.eks. billeder af publikum til en koncert, billeder af besøgende i en zoologisk have eller lignende. Du kan læse mere her: https://www.datatilsynet.dk/hvad-siger-reglerne/vejledning/internet-medier-og-apps-/billeder-paa-internettet

---
Opdateret 13/1 2026 af LLJ, Geopartner

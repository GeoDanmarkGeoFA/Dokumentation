# Vejkoder

For at sikre en ensartet registrering af adresser i GeoFA er der *ikke mulighed* for at indtaste alle adresseoplysninger. I stedet skal man registere en relevant nøgle til enten ***DAR*** (Danmarks Adresseregister) eller kombinationen af ***vejkode***, ***husnummer*** og ***postnr***

Denne vejledning beskriver mulighederne ved indtastning og visning af data.

Nederst på siden findes desuden beskrivelse af andre felter, som vedrører en facilitets beliggenhed.

## Generelt om felter til adresse mv.

Der findes i GeoFAs generelle datamodel (fælles-fælles delen) følgende felter som vedrører adresse:

- **adr_id**

  Udfyldes med den UUID der angiver den adresse, som faciliteten i GeoFA ligger på/ved og hvis udfyldt bruges til auomatisk beregning af andre felter.

- **vejkode**

  Udfyldes med den vejkode som angiver på hvilken vej faciliteten i GeoFA ligger på/ved. Hvis feltet er udfyldt opdateres vejnavn (hvis muligt).

- **vejnavn**

  Udfyldes af systemet med navnet på den vej som hører til feltet *vejkode*.

- **husnummer**

  Udfyldes med husnummer inkl. evt. bogstav.

- **postnr**

  Udfyldes med det 4-cifrede nummer for postdistrikt.

- **postnr_by**

  Udfyldes af systemet med navnet på det postdistrikt, som hører til feltet *postnr*.

- **cvf_vejkode**

  (Beskrivelsen er ukendt - mangler i specifikationen. Antageligvis en vejkode fra CVF/VD).

Se den komplette specifikation af felterne i GeoFA specifikationen. Felterne er, hvis det er bestemt således, tilknyttet de enkelte lag i GeoFA og fungerer da som beskrevet.

Felterne vil i visse situationer udfyldes automatisk i de udtræk/tabeller/views som findes i GeoFA. Reglerne for dette er beskrevet herunder.

> [!IMPORTANT]
> Man kan kun benytte en af de 2 metoder nævnt nedenfor. Man skal således vælge om man vil udfyde feltet *adr_id* eller *vejkode*.

## Brug af UUID fra DAR (anbefales)

Der er i GeoFA mulighed for at angive en adresse på en registreret facilitet ved at udfylde feltet *adr_id*. Feltet skal udfyldes med en værdi på UUID-format.

Værdierne kan eksempelvis findes ved at slå finde adressen på https://danmarksadresser.dk/adresser-i-danmark og folde elementet **Adresse** ud i listen til venstre på kortet. Den værdi der skal indsættes findes i feltet *ID*:

![... i GeoFA](/pics/4_DAR.png  "... i GeoFA")

I eksemplet herover skal man kopiere teksten "0a3f5089-25f0-32b8-e044-0003ba298018" og indsætte den i feltet "adr_id" i GeoFA.

> [!NOTE]
> Når feltet **adr_id** får en værdi, vil en systemmæssig natlig kørsel indsætte værdier ud fra et opslag i DAR (Danmarks Adresseregister).

Den natlige kørsel vil indsætte værdier i følgende felter:
- vejkode
- vejnavn
- husnummer
- postnr
- postnr_by

Værdien af felterne bliver hentet fra den angivne adresses *husnummer*-element.

## Brug af vejkode

Brugeren kan indtaste en vejkode i feltet *vejkode*. Vejkoden er opbygget på den måde, at de føste 3 cifre er kommunekoden (uden foranstillet 0) og de sidste 4 cifre er en (indenfor kommunen unik) kode, der henviser til vejnavnet.

Feltet *vejnavn* vil automatisk udfyldes med den værdi som findes gennem relationen til tabellen **d_vejnavn**.

Brugeren skal derudover selv udfylde felterne (hvis relevant):

- husnummer
- postnr
- cvf_vejkode

## Automatisk udfyldelse af postdistrikt

I en lang række af temaerne kan man udfylde et felt med navnet *postnr*. Feltet udfyldes med et 4-cifret tal, som svarer til det postnmummer, der hører til adressen. Postnumre vedligeholdes ultimativt i DAGI (se https://confluence.sdfi.dk/pages/viewpage.action?pageId=10616969) og replikeres til GeoFA, hvor det benyttes til tilknytning af værdien til feltet *postnr_by*

Hvis man indsætter en (lovlig) værdi i feltet *postnr*, vil feltet *postnr_by* automatisk indeholde navnet på postdistriktet.

Hvis man ønsker at fjerne tilknytningen til postdistriktet, skal værdien af feltet *postnr* slettes (sættes til værdien NULL).

## Automatisk udfyldning af felter med koordinater

Nogle temaer har to felter, *koordinat_east* og *koordinat_north*, som er beskrevet i *fælles-fælles*-delen af GeoFA specifikationen. Felterne er tilføjet til nogle lag og fungerer som beskrevet herunder.
 
![... i GeoFA](/pics/4_p-zone_auto_koordinater.png  "... i GeoFA")

Felterne indeholder en tekstuel oversættelse af det geometriske punkt, som er registreret for faciliteten i GeoFA. Felterne er i DMS-format (grader, minutter, sekunder + retning) og kan benyttes af brugersystemer der ikke kan håndtere geometrien som den ellers leveres af GeoFA. Felteterne udfyldes automatisk hver nat af systemet.

## Automatisk udfyldning af feltet Beliggenhedskommune

Nogle temaer har et felt, *beliggenhedskommune*, som er beskrevet i *fælles-fælles*-delen af GeoFA specifikationen. Feltet er tilføjet til nogle lag og fungerer som beskrevet herunder.

Feltet indeholder kommunenummeret (uden foranstillet 0) for den kommune, som punktet er beliggende i. Feltet udfyldes automatisk hver nat af systemet på baggrund af en analyse mellem facilitetets punkt og DAGI-kommunegrænserne.

---
Opdateret 13/1 2026 af LLJ, Geopartner

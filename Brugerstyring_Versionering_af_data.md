# Versionering af data

GeoFA holder styr på de enkelte elementer og deres ændringer gennnem deres livscyklus. Denne side beskriver kort de elementer som man bør vide i den forbindelse.

Følgende felter indgår i versionsstyringen af data:

- **objekt_id**

  Entydigt ID for objektet gennem hele dets levetid.

- **versions_id**

  Entydigt ID for objektet i dets nuværende version. Er unik gennem hele systemets levetid.

- **systid_fra**

  Feltet indeholder det dato/tidspunkt hvor *denne version* af objektet blev oprettet.

- **systid_til**

  Feltet indeholder det dato/tidspunkt hvor *denne version* af objektet ikke længere er gyldig. Når feltet bliver udfyldt kan de ske forde at objektet enten er opdateret (rettet) eller objektet er slettet. Hvis værdien er NULL, så er objektet den seneste (og gyldige) version af objektet.

- **oprettet**

  Feltet indeholder det dato/tidspunkt hvor den første version af objektet blev oprettet i systemet.

> [!IMPORTANT]
> Alle de nævnte felter udfyldes automatisk af systemet - brugeren skal således ikke selv angive værdier i de nævnte felter.

## Feltet objekt_id

Når man bruger skabelon-filer til indlæggelse af data i GeoFA har man kun adgang til feltet *objekt_id*. Der er i den forbindelse nogle forhold som man skal være opmærksom på da feltets udfyldelse har indflydelse på hvordan rækken behandles ved upload til GeoFA gennem GeoFA Editoren.

Der gælder følgende for feltet *objekt_id*:

- Hvis *objekt_id* er tomt betragtes det som et nyt objekt. Databasen vil danne en værdi og indsætte den i rækken ved indsættelse.

- Ved editering af data må man **ikke** ændre værdien af feltet *objekt_id*, da systemet bruger dette felt til at genkene hvilket objekt som brugeren ønsker at rette.

- Når feltet *objekt_id* er udfyldt vil systemet finde elementet med samme værdi og opdatere de øverige feltet (inkl. geometri) i databasen.

> [!IMPORTANT]
> Hvis du sætter flueben ved "Slet objekter, som ikke er i uploadede data", vil objekter, der ikke er i de uploadede data, blive slettet. Dette bør kun gøres med omhu, og det anbefales at tage backup af data under fanen "Hent data".

Hvis man er usikker på hvordan ændringer (særligt ved upload via GeoFA Editor) fungerer, anbefales det kraftigt at bruge GeoFA Test-miljøet. 

Det anbefales også at tage en backup af data som GeoPackages fra fanen "Hent data" i driftsmiljøet.

Ovenstående principper er vist i figuren her:

![Illustration af versionering af objekter i GeoFA](/pics/2_versionering.png "Illustration af versionering af objekter i GeoFA")

---
Opdateret 6/1 2026 af LLJ, Geopartener

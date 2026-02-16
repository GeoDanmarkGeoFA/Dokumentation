# Geofencing

GeoFA håndterer bl.a. rettigheder til at editere data ved hjælp af et givent objekts placering og brugerens rettigheder - kaldet *Geofencing*. Geofencing giver således mulighed for at styre hvilke brugere der kan rette i objekter i et givent geografisk område. 

Figuren herunder viser princippet i brugerstyringen.

![... i GeoFA](/pics/1_geofence.png "... i GeoFA")

Som det fremgår, er det muligt at indlæse og ajourføre data, der er indenfor eller rører ved den GeoDanmark områdepolygon man selv er tilknyttet. Man kan dog ikke slette objekter, der er indenfor eller rører ved områdepolygonen, hvis objektet ikke har samme cvr_kode, som den man er tilknyttet.

> [!NOTE]
> Der findes brugere som ikke er omfattet af nedenstående regler omkring *Geofencing*, men som har ret til at ændre/slette flere elementer.

## Regler for geofencing
Brugere (tilhørende kommunerne) har tilknyttet en områdepolygon som svarer til egen kommunegrænse.

Der findes også brugere som har tilknyttet en områdepolygon der svarer til "Danmark + havområde".

### Features af punkttype 
Der gælder følgende regler for geometrier oprettet af typen punkt-geometri:
- Punkter kan kun oprettes inden for egen områdepolygon
- Punkter kan kun editeres inden for egen områdepolygon
- Punkter kan kun slettes inden for egen områdepolygon

### Features af linjetype
For egne linjer gælder følgende:
- Linjer kan kun oprettes hvis de har mindst et ende-/knækpunkt inden for egen områdepolygon
- Linjer kan editeres hvis der bevares mindst et ende-/knækpunkt indenfor egen områdepolygon
- Egne linjer kan slettes

*Egne linjer* er de linjer som man selv eller en anden bruger med samme tilhørende områdepolygon har oprettet.

Der gælder følgende regler for geometrier oprettet som linje-geometri og som er oprettet af brugere med en anden områdepolygon (kommune) end sig selv:

- Linjer kan editeres hvis der bevares mindst et ende-/knækpunkt inden for egen områdepolygon og opretternes områdepolygon
- Linjer kan ikke slettes

### Features af polygontype
For egne polygoner gælder følgende:
- Polygoner kan kun oprettes hvis de har mindst et hjørnepunkt inden for egen områdepolygon
- Polygoner kan editeres hvis der bevares mindst et hjørnepunkt inden for egen områdepolygon
- Egne polygoner kan slettes

*Egne polygoner* er de polygoner som man selv eller en anden bruger med samme tilhørende områdepolygon har oprettet.

Der gælder følgende regler for geometrier oprettet som polygon-geometri og som er oprettet af brugere med en anden områdepolygon (kommune) end sig selv:

- Polygoner kan editeres hvis der bevares mindst et hjørnepunkt inden for egen områdepolygon og opretternes områdepolygon
- Polygoner kan ikke slettes

> [!IMPORTANT]
> Når linje og polygoner overskrider flere områdepolygoner (kommuner) bør man etabere et samarbejde således at man ikke ændrer utilsigtet i både geometri såvel som atributter for de features der *går over grænsen*.

## Afslutning
Brugerstyringen i GeoFA giver mulighed for at man kan samarbejde om data på tværs af fx kommunegrænser.

Eksempelvis kan en medarbejder fra Hjørring Kommune indlægge en cykelrute, som strækker sig over Hjørring og Brønderslev Kommune, hvor ruten vil blive stemplet med brugerens bruger_id og kommunens cvr_kode. Efterfølgende kan en medarbejder fra Brønderslev Kommune ajourføre ruten. Medarbejderen fra Brønderslev Kommune kan dog ikke slette ruten.

---
Opdateret 5/1 2026 af LLJ, Geopartner

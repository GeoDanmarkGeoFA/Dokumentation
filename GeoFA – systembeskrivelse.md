
# GeoFA – systembeskrivelse 
<p align="center">
<img width="625" height="556" alt="image" src="https://github.com/user-attachments/assets/3170cc14-c9d3-4364-aa67-0e8c8db90023" />
</p>

* GeoFA (Geografiske fagdata i GeoDanmark) er en fællesoffentlig, frivillig, åben database 

* Databasen udstiller fagdata fra myndigheder og andre samfundsmæssige relevante data fra andre dataejere (virksomheder og civilsamfundet) 

* Flere datasæt er konsoliderede og nationalt dækkende, mens andre kun anvendes af få dedikerede myndigheder baseret på behov, fagligheder, fællesoffentlige samarbejder eller projektindsatser 

* Specifikation, beskrivelse af dataindhold og vejledninger findes her: [Link til specifikation](https://confluence.sdfi.dk/pages/viewpage.action?pageId=191398874) 

* Systemet kører stabilt og sikkert – med ny brugerstyring på vej (ultimo 2025) 

* Data udstilles gennem WFS, WFS-t, WMS, SQL-API snitflader. Der er derfor mange muligheder for at (anvende og) editere i data: QGIS, egne GIS-systemer, fagsystemer, apps mm 

* GeoFA stiller to simple men effektive værktøjer til rådighed: [GeoFA-webkortet og GeoFA-editor](https://confluence.sdfi.dk/pages/viewpage.action?pageId=175014432)

* Systemet rummer et test-miljø og et produktionsmiljø. [Find links til begge her (nederst på siden)](https://confluence.sdfi.dk/display/GS/GeoFA+vejledninger)

### System-opbygning 

* GeoFA er opbygget i open source systemet GC2/vidi 

* Databasen er en PostgreSQL 

* Data er lagret i Amazon-cloud på en serverless-løsning (opskalerbar kapacitet ved flere kald)

* Placering i EU (Irland) 

* Der sker både virtuelt og lokalt backup af data (ultimo 2025) 

* Hvert datasæt består af en tabel og udstilles som views fra databasen Se mere her: https://github.com/GeoDanmarkGeoFA  

* Billeder ligger i selvstændig tabel, og kræver kobling for at kunne ses sammen med data. Se mere her:  LINK 

* GeoFA understøtter rollebaseret adgangsstyring med forskellige niveauer for visning, redigering og administration. 

* Evt. datahøst (af eksterne data) sker vha. ogr2ogr 

### Governance 

* GeoDanmark driver GeoFA-systemet 

* Dataejere er selv ansvarlige for dataindhold og -vedligehold 

* GeoDanmark bestyrelsen har ansvar for udviklingen af GeoFA 

* GeoFA-forum indstiller udviklingsønsker til bestyrelsen 

* GeoFA-drifts- og udviklingsgruppen står for den daglige drift 

* Yderligere oplysninger og kontakt Til GeoFA-driftsgruppen og -forum findes her: https://www.geodanmark.dk/om-geodanmark/organisation/geofa-forum/
 

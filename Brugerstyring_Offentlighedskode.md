# Offentlighedskode

Man kan i GeoFA styre hvilke typer af brugere som må se data som man læger ind. Dette gøres ved at sætte en værdi i feltet *off_kode*. 

![... i GeoFA](/pics/3_offentlighedskode.png "... i GeoFA")

Kodefeltet oversættes de steder hvor dette er relevant til feltet *offentlig* ud fra værdierne i ovennævnte tabel.

Man man ved at offentlighedskoden styre hvem der kan se elementerne. Dette kan eksempelvis bruges til at styre data i forbindelse med en kvalitetssikringsproces.

# Eksempel på anvendelse

Et eksempel på brugen heraf er Cykelknudepunkter (5608) og Cykelknudestrækninger (5609). Dansk Kyst og Naturturisme har lavet en databaseret løsning, der udpeger det potentielle netværk at cykelruter. Dette har de indlagt i GeoFA og sat følgende felter for de indlagte elementer:

- Planstatus er sat til "Planlagt" (**planstatus=2**)
- Offentlighedskoden er sat til "Synlig for alle myndigheder, men ikke for offentligheden" (**off_kode=3**)

Når kommunerne efterfølgende har arbejdet med data (kvalificeret dem, kontrolleret ruteforløb mv.), så overtager kommunerne data og gør dem synlige for omverdenen ved at ændre følgende:

- Planstatus ændres til "Etableret" (**planstatus=1**)
- Offentlighedskoden ændres til "Synlig for alle" (**off_kode=1**)

Derudover sætter kommunen feltet **cvr_kode** til eget CVRnr og overtager derved ejerskabet at den pågældende cykelrute.

---
Opdateret 6/1 2026 af LLJ, Geopartener
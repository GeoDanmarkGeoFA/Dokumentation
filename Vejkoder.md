# Vejkoder

Der er i princippet to måder at angive adresser på
1. Adresse_ID
2. vejkode
3. Evt også en CVF vejkode (?)
Der er ingen fritekstmuligheder!
Det anbefales at bruge 1.
Der står noget tekst i den temaspecifikkedel og en nyere tekst i beskyttelsesrum

Vi har i dag kun lidt tekst i specifikationen – samskriv en ny tekst ud fra denne.
OBS  - jeg tror, det er forkert, at vi i specifikationen snakker om CPR-system (og for den sags skyld også om CVF). Jeg tror, databasen og specifikationen skal tilrettes, så vi kun anvender DAR. Start gerne med et sparringsmøde med Nina, Martin og jeg.
-----
![... i GeoFA](/pics/4_spdcifikation.png  "... i GeoFA")
----
Vejkode - Hvis man udfylder vejkode (skal udfyldes med foranstillet kommunekode, eksempel fra Odense Kommune 46199999), så udfyldes adresse automatisk natten efter indtastning. CPR-system er målrettet til at styre oplysningerne om vejens omgivelser og adresser på vejen, herunder beboerne ved vejen.
vejnavn - Kombination af kommunekode og vejkode giver vejnavn. Vejkode og vejnavn tilknyttet som opslagstabel.
husnr - Husnummer med både tal og bogstav sat sammen uden mellemrum.
postnr - Feltet benyttes til at angive et postnummer via en opslagskode.
postnr_by - Entydig by i forhold til postnr.
adr_id - Entydig databasenøgle fra officielle adresseregister (UUID) taget fra adresse-niveauet i DAR (og IKKE adgangsadresseniveauet).”. Ønsker man at angive en adresse kan man enten indsætte adressens UUID, som fx kan findes på https://danmarksadresser.dk/, se screendump herunder, hvorefter vejnavn, vejkode, mv. automatisk vil blive oversat om natten
 
![... i GeoFA](/pics/4_DAR.png  "... i GeoFA")

Skriv også gerne om autoudfyldning af koordinater for p-zone (5800)
 
![... i GeoFA](/pics/4_p-zone_auto_koordinater.png  "... i GeoFA")

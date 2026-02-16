# FAQ - særligt om fejlmeddelelser

Dette dokument indeholder en kort oversigt over ofte forekommende fejl og deres løsning. Fejl omfatter også situationer, der ikke er fejl, men hvor det oplevede kan have en karakter der gør, at GeoFA kan opføre sig anderledes end forventet. Dokumentet beskriver ikke alle fejltyper.

**Indhold på denne side:**

- [FAQ - særligt om fejlmeddeleleser](#faq---særligt-om-fejlmeddeleleser)
  - [Fejl der kan ses i GeoFA Webkortet](#fejl-der-kan-ses-i-geofa-webkortet)
    - [Der vises ingen eller meget få features når et lag tændes](#der-vises-ingen-eller-meget-få-features-når-et-lag-tændes)
    - [Generel fejlløsning ved indtasting/editering](#generel-fejlløsning-ved-indtastingeditering)
    - [String data, right truncated](#string-data-right-truncated)
    - [Not null violation](#not-null-violation)
    - [Check violation](#check-violation)
    - [Limit error](#limit-error)
  - [Fejl der kan ses i GeoFA Editor](#fejl-der-kan-ses-i-geofa-editor)
    - [Kunne ikke læse temakode](#kunne-ikke-læse-temakode)
    - [Invalid parameter value: 7 ERROR:  Geometry type (MultiPoint) does not match column type (MultiPolygon)](#invalid-parameter-value-7-error--geometry-type-multipoint-does-not-match-column-type-multipolygon)
    - [Obligatorisk felt xxx mangler](#obligatorisk-felt-xxx-mangler)
    - [Foreign key violation](#foreign-key-violation)
    - [Alle mine nye features ligger dobbelt](#alle-mine-nye-features-ligger-dobbelt)
    - [Hvornår skal "Slet objekter som ikke er i uploaded data" bruges?](#hvornår-skal-slet-objekter-som-ikke-er-i-uploaded-data-bruges)

Enhver liste og oversigt bliver bedre hvis brugerne bidrager til den. Har du forslag til forbedringer eller tilføjelser er du meget velkommen til at skrive til [GeoFA Supporten](support@geopartner.dk) eller kontakte [GeoFA-forum under KL](https://www.geodanmark.dk/home/vejledninger/geofa/)

## Fejl der kan ses i GeoFA Webkortet

Her er en liste over fejl, der kan opstå, når man benytter GeoFA Webkortet.

### Der vises ingen eller meget få features, når et lag tændes

Når man tænder for et lag kan det ske, at man ikke kan se nogen features eller at der er meget få features i forhold til det forventede. Typisk vil det skyldes, at der er opsat et filter, som er uhensigtsmæssigt i situationen.

![... i GeoFA](/pics/7_filter_enabled.png "... i GeoFA")

Ovenfor vises et eksempel på et aktivt filter (bemærk det røde udråbstegn). Filtret fjernes nemmest ved at klikke på "Deaktiver" eller "Nulstil".

### Generel fejlløsning ved indtasting/editering

Fejl ved indtastning kan generelt løses på to måder:

1. Afbryd editeringen ved at klikke på ![... i GeoFA](/pics/7_revert_edit.png "... i GeoFA") for det pågældende lag

eller 

2. Editer den pågældende feature igen og indsæt en lovlig værdi. Gem efterfølgende.

Herunder er der vist nogle eksempler på fejl, som kan opstå, når man bruger GeoFA Webkortet til indsættelse eller redigering af data.

### String data, right truncated

![... i GeoFA](/pics/7_truncate.png "... i GeoFA")

Denne fejl vises hvis den værdi, man har indtastet, er for lang (for mange tegn) i forhold til det, som er tilladt i henhold til specifikationen. Find ud af hvor lang (hvor mange tegn) værdien må være og ret fejlen som beskrevet under [Generel fejlløsning ved indtasting/editering](#Generel-fejlløsning-ved-indtastingeditering).

### Not null violation

![... i GeoFA](/pics/7_not_null.png "... i GeoFA")

Denne fejl vises, når der ikke er indtastet en værdi i et felt, hvor dette kræves. Ret fejlen ved at indsætte en lovlig værdi som beskrevet under [Generel fejlløsning ved indtasting/editering](#Generel-fejlløsning-ved-indtastingeditering).

### Check violation

![... i GeoFA](/pics/7_check_constraint.png "... i GeoFA")

Denne fejl vises typisk når en indtastet værdi ikke opfylder et kriterie for den indtastede værdi. Find ud af hvilke værdier som må indtastes i feltet og ret fejlen som beskrevet under [Generel fejlløsning ved indtasting/editering](#Generel-fejlløsning-ved-indtastingeditering).

Typisk kan der gælde regler som:

- Værdien skal ligge i et bestemt interval fx. 5000-7999
- Værdien skal ligge efter en bestemt dato fx > 2006-12-31

Reglerne findes i GeoFA specifikationen for det enkelte lag og er beskrevet i kolonnen "Værdiområde".

### Limit error

![... i GeoFA](/pics/7_limit_error.png "... i GeoFA")

Denne fejl vises typisk, hvis man forsøger at oprette eller editere en feature der ligger udenfor det [geofence](./Brugerstyring_Geofencing.md) som den indloggede bruger har. Eksempelvis hvis man forsøger at placere en feature i *Københavns Kommune*, men man som brugerer logget ind som *Frederiksberg Kommune*.

## Fejl der kan ses i GeoFA Editor

Fejl i GeoFA Editoren kan ses i feltet nederst på skærmen efter (forsøg på) upload.

Hvis man vil sende en mail til GeoFA Suppporten om fejl ved indlæsning via GeoFA Editoren, er det meget relevant at vedhæfte den/de filer, man har forsøgt at uploade sammen med hele indholdet af log-feltet. Husk også at angive hvilken bruger, man har benyttet.

### Kunne ikke læse temakode

![... i GeoFA](/pics/7_editor_temakode.png "... i GeoFA")

Når man får denne fejl, så skyldes det, at man enten:

- ikke har angivet en værdi i feltet *temakode* ved udfyldelse af skabelonen,

eller

- ikke har benyttet en korrekt GeoFA skabelonfil,

eller

- benytter en skabelonfil, som er tom.

Fejlen løses ved at kontrollere, at det er en korrekt (og ikke tom) GeoFA skabelonfil, man har benyttet, og at man har udfyldt feltet *temakode* korrekt for alle rækker i skabelonfilen.

### Invalid parameter value: 7 ERROR:  Geometry type (MultiPoint) does not match column type (MultiPolygon)

![... i GeoFA](/pics/7_editor_geometry_type.png "... i GeoFA")

Fejlen kommer typisk når man har indtegnet en forkert type geometri i skabelonfilen i forhold til, hvad der er tilladt i GeoFA. Typisk vil fejlen også kunne ses, hvis man har angivet en forkert værdi i feltet *temakode* i forhold til den værdi, som hører til skabelonfilen.

Ret datafejlen og upload filen igen.

### Obligatorisk felt xxx mangler

![... i GeoFA](/pics/7_editor_obligatorisk_felt_mangler.png "... i GeoFA")

Denne fejl fremkommer, når et felt mangler i skabelonfilen i forhold til specifikationen. Hvis det angivne felt faktisk mangler i skabelonfilen bør man hente en ny udgave af skabelonfilen og prøve med denne. Fejlen kommer dog typisk, når man har angivet en forkert værdi i feltet *temakode*. Hvis dette er fejlen, så rettes den ved at angive den korrekte værdi i feltet *temakode* og derefter uploade filen igen.

### Foreign key violation

![... i GeoFA](/pics/7_editor_foreign_key_violation.png "... i GeoFA")

Fejlen kommer, når man har angivet en værdi i et *kode-felt* som ikke er et lovligt valg i henhold til GeoFA specifikationen, dvs værdien findes ikke i den fremmed-tabel, der benyttes til at oversætte kode-værdien til en læselig tekst. 

I ovenstående eksempel ses det, at værdien **99** er blevet indsat i feltet **off_kode**. De lovlige værdier (**1**, **2** eller **3**) kan findes i GeoFA specifikationen ved at kigge efter tabellen **d_basis_offentlig**.

Fejlen rettes ved at indsætte en lovlig værdi i feltet og uploade skabelonfilen igen.

### Alle mine nye features ligger dobbelt

Hvis man efter upload af en GeoFA skabelonfil, ser i kortet at features "ligger dobbelt", skyldes det som regel, at man har uploaded den samme skabelonfil med nye elementer til GeoFA to gange.

Problemet løses ved at redigere elementerne i laget og slette de features som er lagt ind for mange gange.

> [!TIP]
> Efter upload af skabelonfil med nye elementer (elementer der ikke har en værdi i feltet *objekt_id*) anbefales det, at man har en arbejdsgang der **forhindrer** at man kan komme til at uploade samme fil igen.

### Hvornår skal "Slet objekter som ikke er i uploaded data" bruges?

I GeoFA Editoren findes denne checkboks:

![... i GeoFA](/pics/7_editor_slet_features.png "... i GeoFA")

I langt de fleste tilfælde skal man **IKKE** sætte markering i checkboksen.

Hvis man har hentet en skabelonfil, editeret den og man *faktisk ønsker* at slette de features i GeoFA, som man har slettet i skabelonfilen, skal man sætte markeringen - ellers ikke.

> [!WARNING]
> Hvis andre brugere har editeret samme lag online (eller med anden skabelonfil), vil upload af en skabelonfil ikke nødvendigvis medføre de ændringer man har forventet. Derfor bør man altid forsøge at minimere tiden mellem hentning af skabelonfilen og upload af den tilrettede skabelonfil.

---
Opdateret 15/1 2026 af LLJ, Geopartner

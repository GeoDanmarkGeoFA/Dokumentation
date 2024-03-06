# SQL API

## Auth
Alle API requests skal have en access token i http headers. Denne fås således:

```http
POST https://geofa-test.geodanmark.dk/api/v4/oauth
Content-Type: application/json
Accept: application/json; charset=utf-8
{
  "grant_type": "password",
  "username": "kom851",
  "password": "xxxxx",
  "database": "fkg"
}
```

## Meta
Meta APIet returnerer information om en eller flere relationer. Her kan fx ses felter og disses typer, herunder enums:

```http
GET https://geofa-test.geodanmark.dk/api/v3/meta/fkg.t_5607_ladefacilitet
Content-Type: application/json
Accept: application/json; charset=utf-8
Authorization: Bearer {{token}}

```
Den sidste del af uri kan have flere relationer separeretet med komma. Alle udgivne relationer kan hentes således:

```http
GET https://geofa-test.geodanmark.dk/api/v3/meta/tag:udgivet
Content-Type: application/json
Authorization: Bearer {{token}}
Accept: application/json; charset=utf-8
```

## SQL
Data kan hentes og ændres gennem SQL APIet. Systemet har en regelbaseret adgangskontrol, som omkriver den sendte SQL statement med `where` clauses eller blokkerer (eller giver lov) til de pågældende relationer.

Den anvendte database engine er PostgreSQL med PostGIS extension.

### Select
En statememt skal sendes som en string literal. En statement kan parametriseres, og ved insert/update/delete kan der sendes flere sæt parametre for en statement.

Her er en simple **select** statement uden parametre:  

```http
POST https://geofa-test.geodanmark.dk/api/v4/sql
Content-Type: application/json
Accept: application/json; charset=utf-8
Authorization: Bearer {{token}}

{
  "q": "select * from fkg.t_5607_ladefacilitet where kommunekode=851 limit 10"
}
```

Tilsidst i repsonsen ses property `auth_check.statement`, som indeholder den statement, der faktisk er kørt. Fx som user kom851 ser den såleds ud:

```sql
select *
  from fkg.t_5607_ladefacilitet
  where kommunekode = 851
    and (off_kode in (1, 3) or kommunekode = 851)
limit 10
```

Det er muligt at parametrisere statement strengen:

```http
POST https://geofa-test.geodanmark.dk/api/v4/sql
Content-Type: application/json
Accept: application/json; charset=utf-8
Authorization: Bearer {{token}}

{
  "q": "select * from fkg.t_5607_ladefacilitet where kommunekode=:kode limit 10",
  "params": {"kode":  851},
  "srs": 25832
}
```
Geometry leveres som default i EPSG:4326. Man ændre dette ved at angive den ønskede EPSG kode med en `srs` property.

### Insert

Inserts kan også laves med eller uden parametre. Men her kan der være en stor fordel at anvende parametre. Specielt hvis der skal indsættes flere objekter på en gang. Her kan der nemlig sendes én statement og et vilkårlig antal sæt parametre. Dette er langt mere effektivt end at sende hver enkelt insert i sin egen request. Alle inserts bliver også behandlet i en transaktion (enten bliver alle indsat eller ingen bliver det).

Hvis man indsætter objekter, som ikke overholder reglerne for den pågældende bruger, vil der blive returneret en LIMIT error.

Her er et **insert** eksemple med to objekter. Bemærk at PostGIS funktionen `st_geomfromgeojson` anvendes til geometrien. Andre funktioner kan også anvendes fx `st_geomfromtext` eller `st_point`. Bemærk også, at statementen har `returning objekt_id` tilsidst. Derved leveres de to nye `objekt_id` tilbage. Flere felter kan angives separeret med komma eller `*`, som giver de nye objekter tilbage med alle felter:

```http
POST https://geofa-test.geodanmark.dk/api/v4/sql
Content-Type: application/json
Accept: application/json; charset=utf-8
Authorization: Bearer {{token}}

{
  "q": "insert into fkg.t_5607_ladefacilitet (cvr_kode,bruger_id,oprindkode,statuskode,off_kode,ladefacilitet_type_kode,geometri) values(:cvr_kode,:bruger_id,:oprindkode,:statuskode,:off_kode,:ladefacilitet_type_kode,st_geomfromgeojson(:geometri)) returning objekt_id",
  "params": [
    {
      "cvr_kode": 29188378,
      "bruger_id": "Clever",
      "oprindkode": 0,
      "statuskode": 3,
      "off_kode": 1,
      "ladefacilitet_type_kode": 1,
      "geometri": {
        "type": "MultiPoint",
        "crs": {
          "type": "name",
          "properties": {
            "name": "EPSG:25832"
          }
        },
        "coordinates": [
          [
            555870.780456494,
            6320180.089515179
          ]
        ]
      }
    },
    {
      "cvr_kode": 29188378,
      "bruger_id": "Clever",
      "oprindkode": 0,
      "statuskode": 3,
      "off_kode": 1,
      "ladefacilitet_type_kode": 1,
      "geometri": {
        "type": "MultiPoint",
        "crs": {
          "type": "name",
          "properties": {
            "name": "EPSG:25832"
          }
        },
        "coordinates": [
          [
            555170.780456494,
            6320480.089515179
          ]
        ]
      }
    }
  ]
}
```

### Update

Ved updates kan der ligeledes anvendes parametre. Update statements bliver typisk omskrevet i regelsystemet, hvor der bliver sat en `where` clause på. Endvidre bliver der kørt et post-tjek på de ændrede objekter for at tjekke, at de stadig overholder regelsættet for den pågældende bruger efter ændringen (fx kan man ikke flytte et objekt fra sin egen kommune til en anden). Hvis ikke, vil en LIMIT error blive sendt og i det tilfælde vil ingen objekter blive ændret.   

Hvis reglerne bevirker, at der ikke foretages ændringer i mindst et objekt (fx alle objekterne ligger uden for kommunegrænsen fra starten) vil en COUNT 0 error blive sendt. Dette gøres for at oplyse brugeren om, at intet blev ændret. Men bemærk, at denne fejl kun bliver sendt hvis intet skete. Fx hvis to objekter forsøges ændret og det ene ligger udenfor for kommunegrænsen og det andet inde, vil transationen blive gennemført med én ændring. Det kan være en god ide, at tjekke `affected_rows` i responsen, som angiver hvor mange objekter, der blev ændret.   

Her et **update** eksempel, der ændrer to objekter:

```http
{
  "q": "update fkg.t_5607_ladefacilitet set off_kode=:off_kode,statuskode=:statuskode where objekt_id=:objekt_id",
  "params": [
    {
      "objekt_id": "6427cf5c-db9f-11ee-bb57-bb467bf148f9",
      "off_kode": 1,
      "statuskode": 3
    },
    {
      "objekt_id": "642adbb6-db9f-11ee-bb57-bb467bf148f9",
      "off_kode": 1,
      "statuskode": 2
    }
  ]
}
```

### Delete

Delete kan også laves med og uden paremtre. Her er et eksempel på en **delete** statement, som sletter et enkelt objekt. Som ved update, vil en kørt delete statement, der ikke ændrer noget resultere i en LIMIT 0 error. Og ligeledes vil regelsystemet typisk sætte en `where` clause på statement:

```http
{
  "q": "delete from fkg.t_5607_ladefacilitet where objekt_id=:objekt_id",
  "params": [
    {
      "objekt_id": "6427cf5c-db9f-11ee-bb57-bb467bf148f9"
    }
  ]
}
```

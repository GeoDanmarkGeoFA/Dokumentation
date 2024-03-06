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
Data kan hentes og ændres gennem SQL APIet. Systemet har en regelbaseret adgangskontrol, som omkriver den sendte SQL statement eller blokkerer (eller giver lov) til den pågældende relations.

### Select
En statememt skal enten sendes som en string literal. En statement kan parametriseres, og ved insert/update/delete kan der sendes flere sæt parametre for en statement.

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
Geometry leveres som default i EPSG:4326. Man ændre dette ved at angive den ønskede EPSG kode i `srs`



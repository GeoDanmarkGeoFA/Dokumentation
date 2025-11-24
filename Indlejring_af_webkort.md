# Guide til indlejring af webkort på din hjemmeside

Denne guide viser, hvordan du indlejrer **GeoFA’s webkort** på en hjemmeside.  

---

## 1. Brug den fulde udgave af kortet
For at kunne indlejre kortet, skal du bruge GeoFA kortets fulde version:  

https://geofa-kort.geodanmark.dk/app/fkg/?config=/api/v2/configuration/fkg/configuration_fkg_udgivet_fuld_685d0845b118b494625616.json

Det er herinde du henter din token til din indlejring. 
Du skal oprette dit eget projekt under fanen vist på billedet herunder, det gemmer hvilke lag du har slået til under oprettelse.
Du kan altid opdatere ved at tilføje nye lag eller filtre og derefter trykke på opdater og gem.

<img width="542" height="473" alt="image" src="https://github.com/user-attachments/assets/81d105af-095b-491e-ba73-600a7514b53f" />

---

## 2. Indlejring af GeoFA’s kort på din hjemmeside
For at indlejre webkortet kan du følge manualen for Vidi-platformen, som GeoFA’s kort bygger på:  

[Manual: Indlejring af webkort](https://vidi.readthedocs.io/da/latest/pages/standard/95_embed.html)

Vigtigst fra ovenstående side: ```<script src="https://vidi.swarm.gc2.io/js/embed.js"></script>```

og 

```<div data-vidi-use-config="true" data-vidi-token="eyJ0aXRsZSI......" data-vidi-width="800px" data-vidi-height="600px"></div>```

Husk at erstatte med dit eget token fra webkortet i *data-vidi-token* feltet.

---

## 3. Ekstra ressourcer
Workshop med praktiske eksempler:  

[Workshop: Vidi embed](https://gc2vidi.github.io/workshops/Vidi-embed/)

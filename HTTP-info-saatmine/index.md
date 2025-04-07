---
title: 2. HTTP info saatmine. Parkimismaja.
layout: default
nav_order: 8
has_children: true
---

Kuna meie linnas on juba olemas valgusfoorid, võime eeldada, et seal liiguvad ka autod. Autode jaoks võiks olla meie linnas parkimismaja, mille kohta saame alati infot, millised parkimiskohad on võetud ja millised on vabad. 

Selle jaoks teeme Node-RED keskkonnas tabeli, kus on näidatud parkimiskohtade numbrid ning olek, ja saadame parkimiskohtade kohta infot ESP32-st HTTP POST päringu abil. 

Eelmises õpetuses tutvusime HTTP GET päringuga, mille abil saime serverist infot. HTTP POST päringu abil saadame infot serverisse. POST päringut kasutavad tihti nt. sisestusvormid veebilehtedel. 

POST päringut tehes peab päringu `Content-Type` nimelises päises ütlema ära, mis tüüpi meie päringus olev info on. Meie info tüübiks saab olema JSON.

JSON ehk **J**ava**s**cript **O**bject **N**otation on lihtne ja inimloetav andmevahetusvorming. See põhineb JavaScript objektide vormingul, ning tänapäeval toetavad seda peaaegu kõik programmeerimiskeeled. 

JSON objekt koosneb järjestamata nimi/väärtus paaridest. Näiteks kui me tahaks teha JSON objekti, kus on info meie ESP32 kohta, näeks see välja selline:
```json
{"arenduslaud":"esp32","programm":"ülekäigurada"}
```

JSON objekt mitme erineva ESP32 kohta näeks välja selline:
```json
[
{"ID":"1","arenduslaud":"esp32","programm":"ülekäigurada"},
{"ID":"2","arenduslaud":"esp32","programm":"valgusfoor"}
]
```

Teise JSON objekti on lisatud ID väli, et kahel sama nime väärtusega objektil kergemini vahet teha. JSON-iga töötades on heaks tavaks kasutada objektidel vähemalt ühte välja, mis on igal objektil unikaalne.
{: .info}

Meie parkimismaja jälgimise süsteem hakkab töötama nii: Node-RED dashboard-il on tabel, kus on kirjas parkimiskoha number ning selle juures punane värv, kui parkimiskoht on hõivatud, ning roheline värv, kui parkimiskoht on vaba. ESP32 poolt muudame parkimiskoha olekut nupuga, mille vajutamisel parkimiskoha olek muutub, ning edastame info parkimiskohtade kohta JSON objektina HTTP POST päringu abil.

[HTTP POST päringu vastuvõtmine Node-RED-is](./node-red-http)

**Kasutatud allikad:**
- [https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/POST](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/POST)
- [https://www.json.org/json-en.html](https://www.json.org/json-en.html)
- [https://akit.cyber.ee/term/10850-json](https://akit.cyber.ee/term/10850-json)
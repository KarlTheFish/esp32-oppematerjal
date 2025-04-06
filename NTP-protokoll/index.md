---
title: 9. NTP protokoll. Ülekäiguraja statistika
layout: default
nav_order: 29
has_children: true
---
Siiamaani oleme lasknud ESP32-l infot saata ning vastu võtta, kui infot uuendatakse. Mõnikord soovime aga infot saata või midagi muud teha kindlatel kellaaegadel. Seda saame teha RTC(**R**eal **T**ime **C**lock) mooduli abil.

Mitme seadme omavahelises suhtluses vigade vältimiseks on oluline, et neil oleks ühine arusaam kellaajast. Näiteks kui me tahaksime ESP32-st saata info Node-RED dashboardile kell 19:00, kuid mingil põhjusel näitab ESP32 kellaaeg 19:00 siis, kui meie arvuti näitab 18:30, saaksime me info kätte alles kell 19:30. Samuti tekitaks probleeme kellakeeramine, mille kuupäevad iga aasta muutuvad. Kaks korda aastas ESP32-te hakata ümber programmeerima võib olla päris tüütu.

Erinevate seadmete kellaaja sünkroniseerimiseks kasutatakse NTP-d ehk **Network Time Protocol**\-li. Tõenäoliselt saavad selle abil õige kellaaja kätte ka sinu arvuti ning telefon. 

Loome targa ülekäiguraja, mis PIR sensori abil tuvastab, kui teda ületama hakatakse. Kui ülekäigurada hakatakse ületama, läheb põlema LED tuli ning ESP32 paneb ületamise kirja. Iga 5 minuti tagant saadab ESP32 Node-RED dashboardile info, mitu ületamist on viimase 5 minuti jooksul toimunud. Lisaks sünkroniseerib iga päev kell 00:00 ESP32 oma kellaaja ülemaailmse NTP serveriga. 

Siin õpetuses läheb vaja ühte ESP32 arenduslauda, ühte LED tuld, ühte 220 oomist takistit, ühte ds3231 RTC moodulit, ja ühte PIR sensorit. 

[ESP32 ja NTP serveri aja sünkroniseerimine](./esp32-ntp)

**Kasutatud allikad:**
- [https://www.ntp.org/documentation/4.2.8-series/\#introduction](https://www.ntp.org/documentation/4.2.8-series/#introduction)  
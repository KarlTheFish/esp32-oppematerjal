---
title: 6. HTTP server esp32 peal. Infovahetus mitme ESP32 vahel. Keskmine õhuniiskus ja temperatuur.
layout: default
nav_order: 20
has_children: true
---

Siiamaani oleme tutvunud ESP32 kasutamisega HTTP kliendina, mis teeb päringuid. ESP32 on võimalik ka kasutada (suhteliselt väikse) HTTP serverina, mis suudab päringutele vastata.

Õhuandurite kasutamisega tutvusime 4\. õpetuses, kus panime ESP32 saatma infot enda anduri kohta. Loome siin õpetuses 2 ESP32 HTTP serverit, mis päringut saades tagastavad info enda õhuandurite mõõdikute kohta, ning ühe ESP32 HTTP kliendi, mis pärib info ESP32 HTTP serveritest, arvutab nende saadetud õhuniiskuse ja temperatuuri keskmise, ning saadab selle info Node-RED dashboardile.

Selles õpetuses läheb vaja kolme ESP32 arenduslauda, kahte DHT11, DHT21 või DHT22 andurit ja kahte 10k oomist takistit.

[esp32 HTTP server](./esp32-http)
---
title: 1. HTTP info saamine. Valgusfoor
layout: default
nav_order: 4
has_children: true
---

HTTP ehk **H**ypertext **T**rans**p**ort **P**rotocol on TCP protokolli peal töötav protokoll. Seda kasutatake erinevate ressurside, näiteks HTML dokumentide, kätte saamiseks. HTTP protokoll loodi 1990-date aastate alguses ja tänapäeval põhineb sellel peaaegu kogu infovahetus veebis. 

HTTP protokoll töötab klient-server põhimõttel, s.t. päringu algatajaks on klient. Kliendi ja serveri suhtlus käib üksikute sõnumite vahetusega. Kliendi poolt saadetud sõnumeid kutsutakse päringuteks ning serveri poolt saadetud sõnumeid kutsutakse vastusteks. HTTP päringud koosnevad tavaliselt sõnumi meetodist(Siin kursusel tutvume GET ning POST meetoditega), URL teest, kus soovitav ressurss asub, HTTP protokolli versioonist, päringu päistest, ning mõningatel juhtudel päringu kehast(Inglise keeles *body*). HTTP vastus koosneb tavaliselt HTTP protokolli versioonist, staatuse koodist, staatuse sõnumist, HTTP päisest, ning vastuse kehast.

Linnades sõidavad tavaliselt autod, ning ohutuse mõttes võiksime esimese asjana enda tarka linna lisada valgusfoorid. 

Loome targa valgusfoori, mille tsükli pikkust saab veebibrauseris muuta. Et tsükli pikkuse kohta infot saada, teeb ESP32 HTTP päringu Node-RED-ile.

[ESP32 http info saamine](./esp32-http)

**Kasutatud allikad:**
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Overview
- https://developer.mozilla.org/en-US/docs/Web/HTTP


---
title: Node-RED ja esp32 HTTP serveri suhtlus
layout: default
nav_order: 22
parent: 6. HTTP server esp32 peal. Infovahetus mitme ESP32 vahel. Keskmine õhuniiskus ja temperatuur.
---

Leiame Node-Red vasakult menüüst *http in* sõlme, paneme meetodiks POST, URL-iks /keskmine , ja nimeks Keskmine HTTP Sisse.

![Node-RED http in](./pildid/2.png)

Leiame ka *http response* sõlme. Paneme staatusekoodiks 200 ja nimeks Keskmine HTTP Välja.

![Node-RED http response](./pildid/3.png)

Järgmisena leiame *Switch* sõlme, mille abil suuname HTTP päringuga saadetud väärtuse olenevalt sellest, kas tegu on keskmise õhuniiskuse või temperatuuriga. *Switch* sõlmed toimivad Node-RED-is tingimuslausetena. *Switch* sõlme nimeks paneme Keskmine Switch, võrreldavaks muutujaks *msg.payload.naitaja*, esimeseks tingimuseks *\==niiskus* ja teiseks *\==temperatuur*.

![Node-RED switch node](./pildid/4.png)

Seejärel loome kaks identset *Template* sõlme. Mõlemas paneme muudetavaks omaduseks *msg.payload* ning *Template* väärtuseks *{{payload.arv}}*. Nii paneme HTTP laadungi väärtuseks ainult arvu, mida me soovime *dashboard*\-il näidata.

Paneme esimese *Template* sõlme nimeks Niiskus Template ja teise nimeks Temperatuur Template.

![Node-RED niiskus template](./pildid/5.png)

![Node-RED temperatuur template](./pildid/6.png)

Samuti loome kaks *text* sõlme. Esimese nimeks ja sildiks saab Niiskus ja teise nimeks ja sildiks Temperatuur.

![Node-RED niiskus text node](./pildid/7.png)

![Node-RED temperatuur text node](./pildid/8.png)

Ühendame *Keskmine HTTP Sisse* sõlme *Keskmine HTTP Välja* ning *Keskmine Switch* sõlmedega.

![Node-RED flow](./pildid/9.png)

Näeme, et *Keskmine Switch* sõlmel on kaks kohta, kust on võimalik väljaminevaid ühendusi luua. Ülemise kohaga liigutakse edasi, kui on täidetud tingimuslause “naitaja \== niiskus”, ning alumise kohaga, kui on täidetud tingimuslause “naitaja \== temperatuur”. Tingimuslauset, mille puhul edasi liigutakse, saab näha, hoides kursorit ühenduskoha peal.

![Node-RED flow](./pildid/10.png)

Ühendame ülemise ühenduskoha *Niiskus Template* sõlmega, ning alumise *Temperatuur Template* sõlmega.

![Node-RED flow](./pildid/11.png)

Viimasena ühendame *Niiskus Template* ja *Temperatuur Template* vastavate tekstivälja sõlmedega.

![Node-RED flow](./pildid/12.png)

![Node-RED flow](./pildid/13.png)

Kui me nüüd Node-RED üleval paremas nurgas *Deploy* nuppu vajutame ja läheme Node-RED *Dashboard*\-ile, näeme ESP32 edastatud keskmist temperatuuri ning õhuniiskust.

![Node-RED näidud](./pildid/14.png)

**Iseseisvaks nuputamiseks:**  
- Lisa Node-RED-i näidik, kus on näha ESP32 serverite arvu, mille andmetest keskmine arvutatakse.

[Järgmine õpetus](../WebSocket-1/)
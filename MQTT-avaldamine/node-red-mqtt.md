---
title: MQTT teemade kohta info tellimine Node-RED-is
layout: default
nav_order: 16
parent: 4. MQTT protokoll. Teemade kohta info avaldamine. Õhuandur
---

Avame enda arvutis Node-RED ja leiame vasakult menüüst *mqtt in* sõlme. Tirime selle keskele, paneme serveriks sama, mis eelmises õpetuses. Teemaks(*topic*) paneme “temperatuur”, teenuskvaliteediks(QoS) paneme 0, ning nimeks paneme “Temperatuur MQTT in”.

![Node-RED mqtt in](./pildid/3.png)

Loome veel ühe *mqtt in* sõlme, mille muud sätted on samad, kuid teemaks paneme “niiskus” ja nimeks paneme “Õhuniiskus MQTT in”.

![Node-RED mqtt in](./pildid/4.png)

Leiame vasakult menüüst *chart* sõlme ja toome selle keskele. Nimeks paneme “MQTT temperatuur graafik” ja sildiks(*label*) paneme “Temperatuur”.

![Node-RED graafik node](./pildid/5.png)

Loome samasuguse sõlme ka õhuniiskuse jaoks. Ühendame *Temperatuur MQTT in* sõlme *MQTT temperatuur graafik* sõlmega ja *Õhuniiskus MQTT in* sõlme *MQTT õhuniiskus graafik* sõlmega.

![Node-RED flow](./pildid/6.png)

Paneme enda arvutis käima Mosquitto, laeme tehtud programmi ESP32-le, ning vajutame Node-RED-is *Deploy*. Kui me nüüd läheme veebilehitsejas aadressile *localhost:1880/dashboard*, peaksime nägema graafikut temperatuuri ning õhuniiskuse kohta. Katsetamiseks võib proovida DHT anduri peale hingeõhku puhuda, et näha, kuidas temperatuur ning õhuniiskus selle peale muutub.

![Node-RED graafikud](./pildid/7.png)


**Iseseisvaks nuputamiseks:**

 - Proovi panna veel mõned ESP32-d temperatuuri kohta infot saatma. Mis juhtub graafikuga? Tee iga ESP32 kohta eraldi graafik.  
 - Arvuta ESP32-s viimase 50 mõõdetud temperatuuri ja õhuniiskuse keskmine ning kasuta Node-RED dashboard 2 *text* sõlme, et neid *dashboard*\-il kuvada. (Vihje \- üks esp32 võib avaldata infot mitme teema kohta\!).

[Järgmine õpetus](../MQTT-turvalisus/)
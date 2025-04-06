---
title: Node-RED-iga turvaliselt MQTT info saatmine
layout: default
nav_order: 19
parent: 5. MQTT protokoll ja turvalisus. Kutsung.
---

Liigume node-RED juurde. Leia node-RED vasakult menüüst sõlm nimega *button* ja tiri see keskele. Paneme nupu nimeks kutsung ja sildiks “Häire”. *Payload* väärtuseks paneme “alert”.

![Node-RED button node](./pildid/1.png)

Teeme nii, et häire kestab 5 minutit, enne, kui nuppu peab uuesti vajutama. Selle jaoks leia vasakult menüüst *delay* sõlm ja ühenda see *kutsung* sõlmega. Paneme *delay* sõlme nimeks “5 minutit delay” ja pikkuseks 5 minutit.

![Node-RED delay node](./pildid/2.png)

Lisame ka *template* sõlme, mille nimeks saab “Häire läbi template” ja mis paneb msg.payload väärtuseks “clear”.

![Node-RED template node](./pildid/3.png)

Ühendame omavahel *kutsung*, *5 minutit delay*, ning *häire läbi template* sõlmed.

![Node-RED flow](./pildid/4.png)

Selleks, et häiret varem lõpetada, lisame veel ühe *button* sõlme nimega “kutsung läbi”. Paneme selle *payload*\-iks samuti “clear”.

![Node-RED button node](./pildid/5.png)

Viimasena lisame *mqtt out* sõlme. Loome node-RED-is uue MQTT serveri. Serveri aadress on sama, mis varem(172.17.0.1), kuid *Security* vahelehel paneme kasutajanimeks “alert-info” ning parooliks “alert123”(Need seadistasime õpetuse alguses Mosquittos\!).

![mqtt-broker config](./pildid/6.png)

Lisame MQTT serveri, paneme *MQTT out* sõlme nimeks “MQTT kutsung out”, teemaks “kutsung”, *retain* väärtuseks *false* ning *QoS* väärtuseks 2\.

![mqtt-server config](./pildid/7.png)

Kui *MQTT kutsung out* sõlm on tehtud, ühendame sellega järgmised sõlmed:
- kutsung läbi
- kutsung
- häire läbi template

![Node-RED flow](./pildid/8.png)

Vajutame *Deploy* ning läheme *localhost:1880/dashboard* . Kui kõik on õigesti tehtud, näeme kahte nuppu: *Häire* ning *Häire läbi*. Proovime neid kordamööda vajutada või peale häire nupu vajutamist oodata. Kui kõik on õigesti tehtud, peaks *häire* nupu vajutamisel LED tuli hakkama vilkuma ja *häire läbi* nupu vajutamisel või peale 5 minutit tuli kustuma.

**Iseseisvaks nuputamiseks:**  
Proovi ka varem tehtud MQTT asjad teha turvaliseks.

[Järgmine õpetus](../HTTP-server/)


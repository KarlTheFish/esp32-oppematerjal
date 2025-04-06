---
title: 3. MQTT protokoll. Teemade tellimine. Tänavavalgusti.
layout: default
nav_order: 7
has_children: true
---

**MQTT**(**M**essage **Q**ueuing **T**elemetry **T**ransport) on sõnumiedastuse protokoll, (mis on loodud suhtluseks erinevate seadmete vahel(source????)). MQTT protokoll võtab vähe võrguressurssi. 

Rohkem infot MQTT kohta!!
{: .todo}

MQTT töötab avalda/telli (publish/subscribe) põhimõttel: **avaldaja** ehk *publisher* masin saadab info **vahendaja** ehk *broker* masinale mingi kindla teemaga. **Tellija** ehk *subscriber* masinad saavad selle teema kohta infot tellida, ning vahendaja masin edastab neile tellitud teema kohta info.

Päriselus võib tuua näiteks postkontori: kirjastaja saadab postkontorisse kolm erinevat ajakirja. Ajakirjade nimed on Robootika 24, Linux Uudised ning Küberturvalisuse Ekspress. Postkontori klientideks on Mati, Peeter, Juku ja Tõnu.

Kergemaks visualiseerimiseks on meil tabel sellest, milliseid ajakirju igaüks tellib

| Inimene | Ajalehed |
| :---- | :---- |
| Mati | Robootika 24, Linux Uudised |
| Peeter | Küberturvalisuse Ekspress |
| Juku | Linux Uudised, Küberturvalisuse Ekspress |
| Tõnu | Robootika 24, Linux Uudised, Küberturvalisuse Ekspress |

Postkontor edastab kõikidele need ajakirjad, mille tellijateks nad on ennast registreerinud.

Siin näites on MQTT avaldaja rollis ajakirjade kirjastaja. Vahendajaks on postkontor ja tellijad on postkontori kliendid. Teemaks on ajakirjade pealkirjad, olenemata sellest, mis informatsioon ajakirjas on. 

Võrdleme seda HTTP protokolliga. Kui sama näidet kasutada HTTP protokolliga, siis peaks igaüks minema postkontorisse kohale sooviga saada kindlat ajakirja.

Meie targa linna mudelis võiks MQTT-st kasu olla tänavavalgustite kontrollimisel. Asi võiks toimida nii: Node-RED saadab teemal “tänavavalgus” sõnumi “on” või “off”. ESP32 tellib “valgus” teemalist infot ning vastavalt saadud infole lülitab LED tule sisse või välja.

Lisaks on meil vaja sõnumite vahendajat. Selleks kasutame enda arvutis vabavaralist programmi **Mosquitto**, mille saame alla laadida [siit](https://mosquitto.org/download/).

Õpetuse Mosquitto kasutamiseks Windows operatsioonisüsteemiga leiad [siit](https://team-ethernet.github.io/guides/How%20to%20install%20and%20use%20Mosquitto%20for%20Windows.pdf).

Kui oled Mosquitto installinud Linux operatsioonisüsteemiga arvutisse, mine faili `/etc/mosquitto/mosquitto.conf` ning lisa read:  
`listener 1883 0.0.0.0`  
`allow_anonymous true`

Linux peal Mosquitto käivitamiseks sisesta käsureal:  
`sudo mosquitto -c /etc/mosquitto/mosquitto.conf && sudo tail -f /var/log/mosquitto/mosquitto.log`

Mosquitto töö lõpetamiseks saad teha käsureal `ctrl + c`.

Vaata arvutis üle tulemüüri sätted \- port 1883 võib olla vaikimisi blokeeritud\!
{: .important}

Kui Mosquitto on installitud ja tööle pandud, saame liikuda ESP32 programmeerimise juurde. 

[MQTT teemade tellimine ESP32-ga](./mqtt-esp32)



**Kasutatud allikad:**  

- [https://www.oasis-open.org/standards/](https://www.oasis-open.org/standards/)[http://docs.oasis-open.org/mqtt/mqtt/v5.0/os/mqtt-v5.0-os.html](http://docs.oasis-open.org/mqtt/mqtt/v5.0/os/mqtt-v5.0-os.html) 
- [https://scispace.com/pdf/the-use-of-mqtt-in-m2m-and-iot-systems-a-survey-1u39tkgj3g.pdf](https://scispace.com/pdf/the-use-of-mqtt-in-m2m-and-iot-systems-a-survey-1u39tkgj3g.pdf)
- [https://mqtt.org/](https://mqtt.org/)
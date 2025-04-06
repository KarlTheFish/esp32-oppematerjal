---
title: 5. MQTT protokoll ja turvalisus. Kutsung.
layout: default
nav_order: 17
has_children: true
---

Siiamaani MQTT protokolliga asjad küll töötavad, kuid need on turvamata. Igaüks, kellel on olemas meie MQTT vahendaja IP aadress ning teemad, mille kohta infot edastatakse, saab samuti infot saata. Targas linnas oleks tegu suure turvaauguga. 

Õnneks on MQTT protokolliga võimalik infot saata ka turvatud moel. Kasutame seda, et luua kutsungisüsteem. Loome Mosquitto vahendajaga MQTT kasutaja ning ACL(Access Control List), millega piirame teemadele ligipääsu. Teeme häirekutsungisüsteemi, mis edastab ESP32-le signaali, mille peale LED tuli hakkab vilkuma. Selline süsteem võiks olla kasulik näiteks selleks, et teavitada turvatöötajaid, et koguneda kuskile kindlasse kohta. (Tänapäeval saab muidugi selleks kasutada telefone ning muid suhtluskanaleid, aga mõnes olukorras võivad ka primitiivsed meetodid, nagu meie oma, kasulikud olla.)

Muudame kõigepealt oma Mosquitto konfiguratsioonifaili. Linux operatsioonisüsteemides leiad selle `/etc/mosquitto/mosquitto.conf` , Windows operatsioonisüsteemis samas kaustas, kus on Mosquitto.exe fail, nt `C:\\Program Files\\mosquitto\\mosquitto.conf` 

Lisame read:

Linux kasutajad:

`acl\_file /etc/mosquitto/acl-kutsung`  
`password\_file /etc/mosquitto/passwd`

Windows kasutajad:

`acl_file C:\Program Files\mosquitto\acl-kutsung.txt`
`password_file C:\Program Files\mosquitto\passwd.txt`

Salvesta konfiguratsioonifail. Järgmiseks lisame mosquitto-ga samasse teeki acl faili. Loo uus acl fail nimega acl-kutsung(Windowsi kasutajad: acl-kutsung.txt) ja lisa sinna read:

```bash
topic readwrite #

user alert-info
topic write kutsung

user alert-station
topic read kutsung
```

Mida need read tähendavad?

`topic readwrite #` - kõik kasutajad, kelle kohta pole ACL failis midagi muud öeldud, saavad kirjutada ning lugeda infot kõikide teemade kohta, mille kohta ei ole ACL failis midagi muud öeldud. Selle rea lisame, et meie varem tehtud MQTT protokolliga tehtud asjad töötaksid. \# märk Mosquitto ACL failis tähistab kõike. Näiteks kui me lisaksime rea:  
`topic readwrite #/info`   
saaksid kõik kasutajad, kelle kohta pole ACL failis midagi muud öelda, kirjutada ja lugeda infot teemade valgusfoor/info , tuled/info , teekond/info jne kohta, kuid teema tuled/olek ei oleks ligipääsetav.

`user alert-info` \- järgnevad read käivad kasutaja alert-info kohta  
`topic write kutsung` \- eelpool öeldud kasutaja saab teemal kutsung kirjutada(kuid ei saa seda teemat lugeda\!)

`user alert-station` \- järgnevad read käivad kasutaja alert-station kohta  
`topic read kutsung` \- kasutaja saab teemat kutsung lugeda, kuid ei saa selle teema kohta infot kirjutada.

**Mosquitto passwd faili õpetus Linux kasutajatele**  
jooksuta käsureal:  
`mosquitto_passwd -c  /etc/mosquitto/passwd alert-station`

Programm küsib parooli. Pane parooliks “esp321”.

Järgmisena jooksuta käsureal:  
`mosquitto_passwd /etc/mosquitto/passwd alert-info`

Pane seekord parooliks “alert123”

Mosquitto paroolifail on loodud.

**Mosquitto passwd faili õpetus Windows kasutajatele**  
Loo uus fail nimega “passwd.txt” ja sisesta sinna:  
`alert-station:esp321`  
`alert-info:alert123`

Salvesta fail ning liiguta see samasse kausta, kus on mosquitto.exe(Tavaliselt `C:\Program Files\mosquitto\`). Ava mosquitto kaustas käsurida ning sisesta:  
`mosquitto_passwd -c passwd.txt`

Mosquitto paroolifail on loodud.

Nüüd võib Mosquitto käivitada(Või taaskäivitada, kui see käib teenusena).


[esp32-ga MQTT info turvaliselt saamine](./esp32-mqtt)


**Kasutatud allikad:**

- [http://www.steves-internet-guide.com/topic-restriction-mosquitto-configuration/](http://www.steves-internet-guide.com/topic-restriction-mosquitto-configuration/)
- [https://shop.theengs.io/blogs/news/installing-mosquitto-on-windows-and-make-it-accessible-from-your-local-network](https://shop.theengs.io/blogs/news/installing-mosquitto-on-windows-and-make-it-accessible-from-your-local-network)
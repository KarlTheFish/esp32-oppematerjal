---
title: Node-RED HTTP info saamine
layout: default
nav_order: 6
parent: 2. HTTP info saatmine. Parkimismaja
---

Teeme seekord kõigepealt valmis Node-RED poole. Paneme käima Node-RED(*docker start TarkLinn* või *sudo docker start TarkLinn)* ning läheme veebilehitsejas aadressile *localhost:1880*.

Võrreldes eelmise õpetusega on seekord Node-RED pool märksa lihtsam. Leia vasakult menüüst sõlm nimega *table*(*dashboard 2* sektsioonis) ning tiri see keskele(Jäta vasakule poole natuke ruumi\!). Tee sõlme peal topeltklikk, et avada menüü tabeli sätete muutmiseks. Paneme tabeli nimeks “parkimismaja tabel” ja sildiks(*label*) “parkimismaja”. *Max Rows* väärtuseks paneme 100\.  *Action* rippmenüüst vali “*Replace*”. *Search* valikus võta ära märge *Show* ning *Columns* valikus märge *Auto Calculate Columns*. 

Menüüse ilmub uus valik, millega saad lisada tabelisse tulpasi. Lisa kaks tulpa: esimese väärtuseks saab *key:koht*, sildiks “Koht” ja tüübiks *text*. Teise tulba väärtuseks saab *key:vaba*, sildiks “Vaba” ning tüübiks *“Color(\#RRGGBB)”.*

![Node-RED table node](./pildid/1.png)

Loome HTTP päringu sõlme. Leia vasakult menüüst sõlm nimega *http in* ja tiri see keskele. Paneme sõlme meetodiks POST, URL-iks /parkimine ja nimeks *Parkimismaja HTTP in*.

![Node-RED http in node](./pildid/2.png)

Lisame ka *http response* sõlme, et HTTP päringut tehes ESP32 saaks vastuse, et päring oli edukas. Paneme sõlme nimeks Parkimismaja HTTP res ning koodiks 200\.

![Node-RED http response node](./pildid/.3png)

Ühendame *Parkimismaja HTTP in* sõlme enda tehtud tabeli ning *HTTP response* sõlmedega.

![Node-RED flow](./pildid/4.png)

Kui me nüüd *Deploy* vajutame ja läheme aadressile *localhost:1880/dashboard*, näeme, et tabeli päised on olemas, aga tabelis infot veel ei ole.

![Node-RED tabel](./pildid/5.png)

Et tabelisse infot kuvada, paneme ESP32 HTTP POST päringuid tegema.

[ESP32 HTTP info saatmine](./esp32-http)
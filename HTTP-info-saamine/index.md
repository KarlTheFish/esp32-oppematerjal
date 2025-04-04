---
title: 1. HTTP info saamine. Valgusfoor
layout: default
nav_order: 4
has_children: false
---

TODO: Infot HTTP protokolli kohta
{: .todo}

Linnades sõidavad tavaliselt autod, ning ohutuse mõttes võiksime esimese asjana enda tarka linna lisada valgusfoorid. 

Konstrueerime kõigepealt valgusfoore mudeli maketeerimislaual. Selleks läheb vaja:  
- 1 punast LED tuld  
- 1 kollast LED tuld  
- 1 rohelist LED tuld  
- 3 220 ohm takistit

Joonis maketeerimislaual ühendamisest näeb välja selline:

![ESP32 joonis](HTTP-info-saamine/pildid/1.png)

Ühendused on:  
- ESP32 pin 5 \- 220 ohm takisti  
- 220 ohm takisti \- punane LED positiivne jalg(pikem)  
- punane LED negatiivne jalg(lühem) \- maandus  

Samad ühendused on kollase ja rohelise LED-iga, kuid kollase LED-i ees olev takisti on ühendatud 6\. ning rohelise LED-i ees olev takisti 7\. pin-iga.

Avame Arduino IDE ning valime enda arenduslaua(Õige arenduslaua valimisest oli juttu 0.1. peatükis). Alguses on meil kohe ees selline kood:
```cpp
void setup() {
// put your setup code here, to run once:

  

}

  

void loop() {
// put your main code here, to run repeatedly:

  

}
```

Arduino vajab töötamiseks neid kahte funktsiooni. **setup** funktsioon käivitub siis, kui esp32 sisse lülitatakse, ning **loop** funktsioon käivitub korduvalt alates setup funktsiooni lõpust kuni esp32 välja lülitamiseni. 

Deklareerime kõigepealt pin-id, millega LED tuled on ühendatud. Pin numbrid deklareeritakse täisarv(int) tüüpi muutujatega. Kirjutame enne setup funktsiooni:
```cpp
int punanePin = 5;
int kollanePin = 6;
int rohelinePin = 7;
```

Setup funktsioonis ütleme ESP32-le, et kasutame deklareeritud LED pin-e väljunditena. Lisame setup funktsiooni read:
```cpp
pinMode(punanePin, OUTPUT);
pinMode(kollanePin, OUTPUT);
pinMode(rohelinePin, OUTPUT);
```
Teeme uue funktsiooni nimega **punane**. Funktsiooni sees kirjutame punase LED pin-ile väärtuse HIGH, et see põlema panna. Lisame **delay,** et programm ootaks 3000 millisekundit ehk 3 sekundit, enne kui edasi liigub.
```cpp
void punane() {
digitalWrite(punanePin, HIGH);
delay(3000);
}
```
Et punane tuli enne kustumist vilguks, lisame ka for-tsükli, mille sees kirjutame pin-ile HIGH väärtuse, lisame 1000-millisekundilise delay, kirjutame pin-ile LOW väärtuse, ning lisame veel ühe 1000-millisekundilise delay.
```cpp
void punane() {
 digitalWrite(punanePin, HIGH);
 delay(3000);
 for(int i = 0; i < 2; i++){
   digitalWrite(punanePin, HIGH);
   delay(1000);
   digitalWrite(punanePin, LOW);
   delay(1000);
 }
}
```

Teeme samasuguse funktsiooni, kus sama asi toimub rohelise pin-iga.
```cpp
void roheline() {
digitalWrite(rohelinePin, HIGH);
delay(3000);
 for(int i = 0; i < 2; i++){
   digitalWrite(rohelinePin, HIGH);
   delay(1000);
   digitalWrite(rohelinePin, LOW);
   delay(1000);
 }
}

```

Kollase pin-i jaoks teeme funktsiooni, mille sees on ainult üks while-tsükkel, kus kolm korda vilgutatakse kollast tuld.
```cpp
void kollane() {
 for(int i = 0; i < 3; i++){
   digitalWrite(kollanePin, HIGH);
   delay(1000);
   digitalWrite(kollanePin, LOW);
   delay(1000);
 }
}

```

Lõpuks kutsume **loop** funktsioonis välja punase, kollase, rohelise, ning siis jälle kollase funktsiooni.
```cpp
void loop() {
 // loop funktsioon käivitub korduvalt kogu programmi töö ajal
 punane();
 kollane();
 roheline();
 kollane();
}

```

Lõpuks näeb meie kood välja selline:
```cpp
//Defineerime pin-id, millega eri värvi LED tuled on ühendatud
int punanePin = 5;
int kollanePin = 6;
int rohelinePin = 7;


void setup() {
 // setup funktsioon käivitub iga kord, kui esp32 lülitatakse sisse


 //Ütleme ESP32-le, et kasutame deklareeritud pin-e väljundina
 pinMode(punanePin, OUTPUT);
 pinMode(kollanePin, OUTPUT);
 pinMode(rohelinePin, OUTPUT);
}


void loop() {
 // loop funktsioon käivitub korduvalt kogu programmi töö ajal
 punane();
 kollane();
 roheline();
 kollane();
}


void punane() {
 digitalWrite(punanePin, HIGH); //Kirjutame pin-ile HIGH väärtuse - tuli läheb põlema
 delay(3000); //Ootame 3000 millisekundit ehk 3 sekundit, enne kui programmi töö jätkub
 for(int i = 0; i < 2; i++){
   digitalWrite(punanePin, HIGH);
   delay(1000);
   digitalWrite(punanePin, LOW); //Kirjutame pin-ile LOW väärtuse - tuli kustub
   delay(1000);
 }
}


void roheline() {
   digitalWrite(rohelinePin, HIGH);
 delay(3000);
 for(int i = 0; i < 2; i++){
   digitalWrite(rohelinePin, HIGH);
   delay(1000);
   digitalWrite(rohelinePin, LOW);
   delay(1000);
 }
}


void kollane() {
 for(int i = 0; i < 3; i++){
   digitalWrite(kollanePin, HIGH);
   delay(1000);
   digitalWrite(kollanePin, LOW);
   delay(1000);
 }
}

```

**Miks me kasutame delay() funktsiooni?** Kui me delay funktsiooni ei kasutaks, liiguks programm edasi nii kiiresti, et me ei näeks, kui LED tuli hetkeks põlema läheb või kustub.
{: .info}

Ühendame oma ESP32 arvutiga ning vajutame IDE-s *upload* nuppu. Kui kõik on õigesti tehtud, näeme, et tuled lähevad põlema, vilguvad, ning lähevad kustu.

![Arduino IDE](./pildid/2.png)

Kui sinu ESP32-l on kaks kohta, kuhu USB kaabel ühendada, töötab tavaliselt Arduino IDE-st koodi laadimiseks pesa, mille juures on kirjas UART.
{: .important}

Valgusfoor küll töötab, aga kui me tahaksime muuta, kui kaua üks tsükkel kestab, peaksime me uuesti ESP32 arvutiga ühendama ning koodi manuaalselt muutma. See ei oleks eriti praktiline, kuna erinevatel aegadel võib olla kõige parem kasutada eri pikkusega tsüklit \- näiteks kui autosid kuigi palju ei sõida, võib tsükkel olla lühem, aga kui liiklust on palju, võiks tsükkel kesta kauem, et võimalikult palju autosi saaksid ühe tsükliga ristmikust üle sõita. Mugav oleks, kui me saaksime tsükli pikkust reguleerida läbi veebi.

Et veebiga suhelda, läheb meil vaja väliseid teeke. Deklareerime programmi alguses kaks välist teeki: WiFi.h ning HTTPClient.h
```cpp
#include <WiFi.h>
#include <HTTPClient.h>
```

Seejärel defineerime wifi nime ning parooli, millega ESP32 peab ühenduma(Arvuti, kus töötab Node-RED ja ESP32 peavad olema samas võrgus\!). Lisaks defineerime URL-i, kuhu ESP32 päringut hakkab tegema. Aadressiks saab arvuti IP pordiga 1880, ning sellele lisame tee(*path*), kust ESP32 hakkab infot saama.
```cpp
const char* ssid = "wifi-nimi"; //esp32 toetab 2.4GhZ wifit!
const char* password = "wifi–parool";


String url = "http://1.2.3.4:1880/valgus";
```

(Enda arvuti IP aadressi saad teada Linux-is *hostname \-I* ning Windows-is *ipconfig* käsuga käsureal. ESP32-le vajalik minev IP aadress **ei alga** numbritega 172\!)

Deklareerima ka uue täisarvu tüüpi muutuja **aeg**, milles hakkame hoidma HTTP päringust saadud väärtust, kui kaua valgusfoori valgus põleb. Paneme tema vaikimisi väärtuseks 1\.
```cpp
int aeg = 1;
```

Asendame **delay(3000)** **punane** ning **roheline** funktsioonides reaga **delay(aeg)**.
```cpp
void punane() {
 digitalWrite(punanePin, HIGH); //Kirjutame pin-ile HIGH väärtuse - tuli läheb põlema
 delay(aeg);
 for(int i = 0; i < 2; i++){
   digitalWrite(punanePin, HIGH);
   delay(1000);
   digitalWrite(punanePin, LOW); //Kirjutame pin-ile LOW väärtuse - tuli kustub
   delay(1000);
 }
}

```

Arduino IDE-s on mugav jälgida programmi tööd **Serial monitor** abil. Lisame **setup** funktsiooni kaks rida:
```cpp
 Serial.begin(115200); //Alustame serial ühendust 115200 baudi peal
 WiFi.begin(ssid, password); //Alustame wifi ühendust eespool defineeritud nime ning parooliga
```

Kuvame serial monitoris teksti “Connecting…” ning lisame while-tsükli, mis laadimise näitamiseks kuvab punkte iga 500 millisekundi tagant senikaua, kuni ESP32 ei ole wifi-ga ühendust saanud.
```cpp
 Serial.print("Connecting...");
  while(WiFi.status() != WL_CONNECTED){
   delay(500);
   Serial.print(".");
 }

```

Lõpuks näeb meie setup funktsioon välja selline:
```cpp
void setup() {
 // setup funktsioon käivitub iga kord, kui esp32 lülitatakse sisse


 Serial.begin(115200); //Alustame serial ühendust 115200 baudi peal
 WiFi.begin(ssid, password); //Alustame wifi ühendust eespool defineeritud nime ning parooliga


 //Ütleme ESP32-le, et kasutame deklareeritud pin-e väljundina
 pinMode(punanePin, OUTPUT);
 pinMode(kollanePin, OUTPUT);
 pinMode(rohelinePin, OUTPUT);


 Serial.print("Connecting...");
   while(WiFi.status() != WL_CONNECTED){
   delay(500);
   Serial.print(".");
 }
}

```

Et hoida enda kood lihtsana, teeme uue funktsiooni **httpParing()**. Seda funktsiooni hakkame kasutama, et saada infot, kui kaua valgusfoori valgus peaks põlema.
```cpp
void httpParing(){

}
```

Lisame funktsiooni tingimuslause - HTTP päringut hakkame tegema ainult siis, kui wifi ühendus on olemas.
```cpp
void httpParing(){
   if(WiFi.status() == WL_CONNECTED){ //Kontrollime, et wifi ühendus on olemas
 }
}

```

Kirjutame tingimuslause sisse 3 rida:
```cpp
WiFiClient client;
HTTPClient http;


http.begin(client, url); //alustame http päringut, kasutades WiFiClient-i ning varem defineeritud URL-i
```

Lisaks defineerime täisarvulise muutuja, mille väärtuseks saab GET päringu vastusena saadud HTTP kood. Erinevate HTTP koodide kohta saab lugeda [siit](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status).
```cpp
int responseCode = http.GET(); //defineerime muutuja HTTP päringu vastuse koodi hoidmiseks
```

Lisame uue tingimuslause: kui HTTP päringust saadud kood on suurem, kui 0, prindime serial monitori vastuse koodi ja jätkame päringu vastusest info saamisega. Kui ei ole, prindime serial monitoris veakoodi.
```cpp
if(responseCode > 0){
    Serial.print("HTTP Response code: ");
    Serial.println(responseCode);


}
else {
    Serial.print("Error code: ");
    Serial.println(responseCode);
}

```

Kui HTTP päringu vastuse kood on 200, saame muutuja “aeg” väärtuseks panna HTTP vastuse väärtuse täismuutujaks teisendatuna. Kuna me tahame kestust anda sekundites, kuid Arduino delay() funktsioon töötab millisekunditega, korrutame saadud vastuse tuhandega.
```cpp
 if(responseCode == 200){ //Jätkame kui kood on 200 - OK
   //Saame HTTP vastuse stringina, cast-ime selle täisarvuliseks muutujaks, ning korrutame selle 1000-ga, et saada väärtus millisekundites
   aeg = http.getString().toInt() * 1000;
   Serial.println(aeg);
 }
```

httpParing funktsiooni lõpus lõpetame http ühenduse.
```cpp
  http.end();
```

Lõpuks võiks httpParing funktsioon välja näha selline:
```cpp
void httpParing(){
   if(WiFi.status() == WL_CONNECTED){ //Kontrollime, et wifi ühendus on olemas
   WiFiClient client;
   HTTPClient http;


   http.begin(client, url); //alustame http päringut, kasutades WiFiClient-i ning varem defineeritud URL-i


   int responseCode = http.GET(); //defineerime muutuja HTTP päringu vastuse koodi hoidmiseks


   if(responseCode > 0){ //kontrollime, et HTTP päringu kood oleks suurem, kui 0
     Serial.print("HTTP Response code: ");
     Serial.println(responseCode);


     if(responseCode == 200){ //Jätkame kui kood on 200 - OK
       //Saame HTTP vastuse stringina, cast-ime selle täisarvuliseks muutujaks, ning korrutame selle 1000-ga, et saada väärtus millisekundites
       aeg = http.getString().toInt() * 1000;
       Serial.println(aeg);
     }
   }
   else { //kui HTTP päringu kood on väiksem, kui 0, prindime veateate
     Serial.print("Error code: ");
     Serial.println(responseCode);
   }
   http.end(); //lõpetame HTTP ühenduse
 }
}

```

Kutsume httpParing funktsiooni välja loop funktsioonis, et enne igat tsüklit ESP32 teeks HTTP päringu.
```cpp
void loop() {
 // loop funktsioon käivitub korduvalt kogu programmi töö ajal
 httpParing();
 punane();
 kollane();
 roheline();
 kollane();
}

```

Lõpuks näeb meie programm välja selline:
```cpp
#include <WiFi.h>
#include <HTTPClient.h>


//WiFi andmed
const char* ssid = "wifi-nimi"; //Defineerime wifi nime. NB! esp32 toetab 2.4GhZ wifit!
const char* password = "wifi-parool"; //Defineerime wifi parooli


String url = "http://1.2.3.4:1880/valgusfoor"; //URL, kuhu hakkame päringut tegema


int aeg = 1;


//Defineerime pin-id, millega eri värvi LED tuled on ühendatud
int punanePin = 5;
int kollanePin = 6;
int rohelinePin = 7;


void setup() {
 // setup funktsioon käivitub iga kord, kui esp32 lülitatakse sisse


 Serial.begin(115200); //Alustame serial ühendust 115200 baudi peal
 WiFi.begin(ssid, password); //Alustame wifi ühendust eespool defineeritud nime ning parooliga


 //Ütleme ESP32-le, et kasutame deklareeritud pin-e väljundina
 pinMode(punanePin, OUTPUT);
 pinMode(kollanePin, OUTPUT);
 pinMode(rohelinePin, OUTPUT);


 Serial.print("Connecting...");
   while(WiFi.status() != WL_CONNECTED){
   delay(500);
   Serial.print(".");
 }
}


void loop() {
 // loop funktsioon käivitub korduvalt kogu programmi töö ajal
 httpParing();
 punane();
 kollane();
 roheline();
 kollane();
}


void punane() {
 digitalWrite(punanePin, HIGH); //Kirjutame pin-ile HIGH väärtuse - tuli läheb põlema
 delay(aeg);
 for(int i = 0; i < 2; i++){
   digitalWrite(punanePin, HIGH);
   delay(1000);
   digitalWrite(punanePin, LOW); //Kirjutame pin-ile LOW väärtuse - tuli kustub
   delay(1000);
 }
}


void roheline() {
   digitalWrite(rohelinePin, HIGH);
 delay(aeg);
 for(int i = 0; i < 2; i++){
   digitalWrite(rohelinePin, HIGH);
   delay(1000);
   digitalWrite(rohelinePin, LOW);
   delay(1000);
 }
}


void kollane() {
 for(int i = 0; i < 3; i++){
   digitalWrite(kollanePin, HIGH);
   delay(1000);
   digitalWrite(kollanePin, LOW);
   delay(1000);
 }
}


void httpParing(){
   if(WiFi.status() == WL_CONNECTED){ //Kontrollime, et wifi ühendus on olemas
   WiFiClient client;
   HTTPClient http;


   http.begin(client, url); //alustame http päringut, kasutades WiFiClient-i ning varem defineeritud URL-i


   int responseCode = http.GET(); //defineerime muutuja HTTP päringu vastuse koodi hoidmiseks


   if(responseCode > 0){ //kontrollime, et HTTP päringu kood oleks suurem, kui 0
     Serial.print("HTTP Response code: ");
     Serial.println(responseCode);


     if(responseCode == 200){ //Jätkame kui kood on 200 - OK
       //Saame HTTP vastuse stringina, cast-ime selle täisarvuliseks muutujaks, ning korrutame selle 1000-ga, et saada väärtus millisekundites
       aeg = http.getString().toInt() * 1000;
       Serial.println(aeg);
     }
   }
   else { //kui HTTP päringu kood on väiksem, kui 0, prindime veateate
     Serial.print("Error code: ");
     Serial.println(responseCode);
   }
   http.end(); //lõpetame HTTP ühenduse
 }
}

```

ESP32 poolt on nüüd kõik vajalik tehtud, aga kui me programmi tööle paneks, ei saaks me kätte mingit infot. Et panna enda arvuti vajalikku infot saatma, kasutame Node-RED-i.

Kui see su arvutis juba ei käi, käivita Dockeris meie eelmises õpetuses tehtud konteineri. Selleks tee Docker käsureal:  
*docker start TarkLinn*  
Linuxi kasutajad saavad enda eelistatud käsureal teha:  
*sudo docker start TarkLinn*

Node-RED-ile pääsed ligi veebilehitsejas minnes aadressile *localhost:1880* või *127.0.0.1:1880*. 

Ekraani vasakus ääres leiad Node-RED sõlmede menüü või siis noole, millele klikkides menüü avaneb. Kerime seal menüüs alla, kuni leiame **dashboard 2** alt **slider** sõlme ja tirime selle ekraani keskele.

![node-red sõlm](./pildid/3.png)

Tehes topeltkliki keskele pandud sõlme peal, avaneb ekraani paremas ääres menüü sõlme omaduste muutmiseks. 

Paneme sõlme nimeks *valgusfoor slider*, sildiks(label) *valgusfoor*, ning Range paneme min 1 ja max 60\. Output rippmenüüst valime “only on release”.

![node-red slider konfiguratsioon](./pildid/4.png)

Kui *group* väljal on valik *none*, ei ole sliderit dashboardil näha. Uue grupi saad teha vajutades plussmärgiga nuppu *group* välja kõrval.
{: .important}

Vajutame üleval paremas nurgas nuppu *Deploy* ning läheme veebilehitsejas aadressile *localhost:1880/dashboard* (Või *127.0.0.1:1880/dashboard*). Näeme, et *slider* on täitsa olemas, kuid suurt kasu meile sellest ei ole. Läheme tagasi Node-RED redaktorisse.

Üks Node-RED eripäradest on see, et vaikimisi saadetakse infot signaalidena, mitte ei salvestata muutujates. See tähendab, et nt. HTTP päringule saab vastuse ainult see hetk, kui signaal saadetakse. See aga ei ole HTTP päringuga infot proovides saada väga praktiline. Et enda tehtud *slider*\-i väärtust salvestada nii, et me saaks seda kätte kogu aeg, kui Node-RED töötab, kasutame ***change*** sõlme.

Võtame vasakult menüüst *change* sõlme ja tirime selle keskele. Teeme selle peal topeltkliki, paneme nimeks “valgusfoor väärtus”, ja *Rules* välja all teeme ühe *Set* reegli, kus paneme *flow.valgusfoor* väärtuseks *msg.payload*

![Node-RED change sõlm](./pildid/5.png)

*flow* eesliidesega väärtused on Node-RED jaoks muutujad, mille väärtus säilitatakse. 

*msg.payload* on väärtus, mille väärtus on tavaliselt sõlmede väljunditeks.

Ühendame *Valgusfoor slider* sõlme *Valgusfoor väärtus* sõlmega, tirides ühe sõlme äärest nool teise sõlme äärde.

![Node-RED flow](./pildid/6.png)

Nüüd salvestatakse *dashboard*\-il antud *slider*\-i väärtus muutujana.

Järgmisena loome HTTP päringu sõlme, et ESP32 saaks muutuja väärtuse kätte. Leia vasakult menüüst sõlm *http in* ning tiri see keskele. Tee sõlmel topeltklikk. Jätame meetodiks GET, paneme URL-iks /valgusfoor ning nimeks “valgusfoor HTTP sisse”.

![HTTP sõlme konfiguratsioon](./pildid/7.png)

Kasutame jälle *change* sõlme, et päringus saadava *payload*\-i väärtus muuta samaks, mis on *flow.valgusfoor*. Leiame vasakult menüüs *change* sõlme, lohistame selle keskele ning teeme selle peal topeltkliki. Paneme sõlme nimeks “valgusfoor muutuja payloadiks”, ja teeme *set* reegli, millega paneme *msg.payload* väärtuseks *flow.valgusfoor*.

![Change sõlme konfiguratsioon](./pildid/8.png)

Node-RED keskkonnas *flow* algusega muutujad kehtivad ühe *flow* ehk voolu sees (Voolud on eraldatud Node-RED siseste vahelehtedega). *msg* algusega muutujad kehtivad ainult omavahel ühendatud sõlmedes. See tähendab, et meil võib olla ühel Node-RED lehel näiteks mitu *msg.payload* muutujat, mis on üksteisest täiesti sõltumatud senikaua, kuni nendega tegelevad sõlmed ei ole ühendatud.
{: .info}

Järgmisena leiame vasakult menüüst *template* sõlme ja tirime selle samuti keskele. *Template* sõlme abil saame seadistada HTTP päringute(ja ka muude päringute) vastuseid. Paneme sõlme nimeks “valgusfoor HTTP template”, *Property* väärtuseks jätame *msg.payload* ning *Template* väärtuseks paneme lihtsalt “{{payload}}”.

![Template sõlme konfiguratsioon](./pildid/9.png)

Viimasena leiame vasakult menüüst *http response* sõlme. Tirime selle keskele ja teeme topeltkliki selle peal. Nimeks paneme “valgusfoor HTTP vastus” ja *Status code* väärtuseks paneme 200\.

![HTTP response sõlme konfiguratsioon](./pildid/10.png)

Ühendame omavahel HTTP sõlmed ning vajutame jälle *Deploy*.

![Node-RED flow](./pildid/11.png)

Praeguseks peaks meie Node-RED nägema välja selline:

![Node-RED flow](./pildid/12.png)

Läheme uuesti dashboard-ile(localhost:1880/dashboard), võtame lahti Arduino IDE, laeme üles tehtud koodi, ja muudame *dashboard*\-il *slider*\-i väärtust. Kui kõik on õigesti tehtud, peaksid nüüd valgusfoore roheline ja punane tuli kestma kauem või vähem vastavalt Node-RED *dashboard*\-il antud väärtusele.

Koodi kirjutades oli mitu korda mainitud **serial monitor**. Serial monitor on tööriist, mille abil ESP32 saab saata sõnumeid otse Arduino IDE-sse, ning seda on hea kasutada veaotsinguks. Et serial monitor avada, kasuta Arduino IDE-s klahvikombinatsiooni ctrl+shift+M või võta ülevalt lahti *Tools* rippmenüü ja vali sealt *Serial Monitor*.

![Arduino IDE menüü](./pildid/13.png)

Programmi *setup* funktsioonis panime kirja, et Serial Monitor hakkab tööle 115200 baudi peal. Baudide väärtust, mida Serial Monitor loeb, saab muuta Serial Monitori akna paremal pool.

![Arduino IDE Serial Monitor](./pildid/14.png)

Kui valgusfoor töötab, võib soovi korral selle kokku joota ning nt. papist valgusfoori mudelisse paigaldada.

**Iseseisvaks nuputamiseks:**  
Kuidas teha nii, et rohelise ja punase tule kestust saaks eraldi reguleerida?

**Kasutatud allikad:**  
- [https://flowfuse.com/node-red/getting-started/node-red-messages/](https://flowfuse.com/node-red/getting-started/node-red-messages/)  
- [https://www.digikey.com/en/maker/tutorials/2022/how-to-use-variables-in-node-red](https://www.digikey.com/en/maker/tutorials/2022/how-to-use-variables-in-node-red)  
- [https://docs.arduino.cc/built-in-examples/strings/StringToInt/](https://docs.arduino.cc/built-in-examples/strings/StringToInt/)   
- [https://flowfuse.com/node-red/core-nodes/change/](https://flowfuse.com/node-red/core-nodes/change/)


[Järgmine õpetus](../http-info-saatmine)


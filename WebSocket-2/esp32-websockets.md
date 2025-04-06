---
title: WebSockets info saatmine esp32-ga
layout: default
nav_order: 27
parent: 8. WebSocket protokoll. Kiiruskaamera.
---

Ehitame valmis mudeli maketeerimislaual. Joonis mudelist näeb välja selline:
![ESP32 joonis](./pildid/1.png)

- HC-SR04 GND pin on ühendatud maandusega
- HC-SR04 ECHO pin on ühendatud ESP32 5. pin-iga
- HC-SR04 TRIG pin on ühendatud ESP32 4. pin-iga
- HC-SR04 VCC pin on ühendatud ESP32  5v pin-iga

Avame Arduino IDE ja paneme kirja välised teegid, mida kasutame.

```cpp
#include <WiFi.h>
#include <ArduinoWebsockets.h>
#include <Arduino_JSON.h>
```

Defineerime pin-id, mida kasutame, ning teeme 5 ujuvkomaarvu tüüpi muutujat:

```cpp
const int trigPin = 4;
const int echoPin = 5;
float duration, distance, dist1, dist2, speed;
```

Paneme kirja WiFi andmed ning WebSocket serveri, kuhu hakkame infot saatma(Selle loome hiljem Node-RED-is)
```cpp
const char* ssid = "wifi-nimi";
const char* password = "wifi-parool";
const char* websocket_server = "ws://1.2.3.4:1880/kiirus";
```

Kuna me hakkame infot saatma JSON kujul, loome muutuja kiirus_json.

```cpp
JSONVar kiirus_json;
```

Kasutame websockets nimeruumi

```cpp
using namespace websockets;
```

Loome wifi ning websockets kliendid

```cpp
WiFiClient wifiClient;
WebsocketsClient WSclient;
```

Teeme WebSockets tööks vajaliku onEventsCallBack funktsiooni. See funktsioon on identne funktsiooniga, mida kasutasime eelmises õpetuses.

```cpp
void onEventsCallback(WebsocketsEvent event, String data) {
 switch(event){
   case WebsocketsEvent::ConnectionOpened:
     Serial.println("Connnection Opened");
     break;
   case WebsocketsEvent::ConnectionClosed:
     Serial.println("Connnection Closed");
     break;
   case WebsocketsEvent::GotPing:
     Serial.println("Got a Ping!");
     break;
   case WebsocketsEvent::GotPong:
     Serial.println("Got a Pong!");
     break;
 }
}
```

Loome funktsiooni kauguse mõõtmiseks, mis tagastab ujuvkomaarvu.

```cpp
float kaugus(){
 digitalWrite(trigPin, LOW); 
 delayMicroseconds(2); 
 digitalWrite(trigPin, HIGH); 
 delayMicroseconds(10); 
 digitalWrite(trigPin, LOW); 
 duration = pulseIn(echoPin, HIGH);
 return (duration * .0343)/2;
}
```

Mida see funktsioon teeb?

HC-SR04 ultrahelisensor tööpõhimõte koosneb kahest osast: kõigepealt saadab sensor ultrahelilaine. Kui laine puutub millegagi kokku, põrkab see tagasi vastu HC-SR04 sensorit. Vaadates, kui kaua läheb aega tagasi põrkunud laine sensorini jõudmiseks, saame me arvutada objektide kaugust.

```cpp
 digitalWrite(trigPin, LOW); 
 delayMicroseconds(2); 
 digitalWrite(trigPin, HIGH); 
 delayMicroseconds(10); 
 digitalWrite(trigPin, LOW); 
```

Kirjutame 10 mikrosekundiks trigPin pin-ile HIGH väärtuse, et sensor saadaks välja ultrahelilaine.

Senikaua, kuni sensor ootab tagasipõrkunud lainet, on echoPin pin väärtus HIGH. Kui laine on tagasi põrkunud, saab echoPin pin väärtuseks LOW.

Selleks, et vaadata, kui kaua aega läks laine saamiseks, kasutame Arduino pulseIn funktsiooni, mis mõõdab, kui kaua oli pin-il kindel väärtus.

```cpp
duration = pulseIn(echoPin, HIGH);
```

Et kestusest saada kaugus, tuletame meelde põhikooli füüsikat. Teatavasti oli valem kiiruse arvutamiseks teepikkus jagada ajaga. Helikiirus on 0.0343 sentimeetrit mikrosekundis, ning aeg on meil mikrosekundites *duration* muutujas. Järelikult läheb meil teepikkuse arvutamiseks vaja korrutada aeg kiirusega. 

Kuna me mõõtsime aega, et ultrahelilaine jõuaks objektini ning tagasi, jagame saadud väärtuse kahega.

```cpp
return (duration * .0343)/2;
```

Valmis funktsioon kauguse mõõtmiseks näeb välja selline:

```cpp
float kaugus(){
 digitalWrite(trigPin, LOW); 
 delayMicroseconds(2); 
 digitalWrite(trigPin, HIGH); 
 delayMicroseconds(10); 
 digitalWrite(trigPin, LOW); 
 duration = pulseIn(echoPin, HIGH);
 return (duration * .0343)/2;
}
```

Loome järgmisena funktsiooni kiiruse mõõtmiseks. Mõõdame kaugust kaks korda, ühe sekundilise vahega.

```cpp
void kiirus(){
 dist1 = kaugus();
 delay(1000);
 dist2 = kaugus();

}
```

Kiiruse väärtuseks saame teise kauguse vahe esimesest kaugusest. Prindime kiiruse info ka serial monitori.

```cpp
void kiirus(){
 dist1 = kaugus();
 delay(1000);
 dist2 = kaugus();
 kiirus = dist1 - dist2;
 Serial.print("Kiirus: ");
 Serial.print(kiirus);
 Serial.println(" cm/s");

}
```

Salvestame saadud kiiruse kiirus_json objekti “kiirus” võtme väärtusena. Lisame ka “aeg” ning “alla piiri” võtmed. “Aeg” võtme väärtuseks paneme 0, kuna selle registreerime Node-RED-is, ning “alla piiri” väärtuseks paneme “true”, kuna me eeldame, et liiklejad peavad piirkiirusest kinni. Piirkiiruse määrame me Node-RED-is, ning seetõttu on kergem seda kontrollida seal.

```cpp
void kiirus(){
 dist1 = kaugus();
 delay(1000);
 dist2 = kaugus();
 kiirus = dist1 - dist2;
 Serial.print("Kiirus: ");
 Serial.print(kiirus);
 Serial.println(" cm/s");
 kiirus_json["kiirus"] = kiirus;
 kiirus_json["aeg"] = 0;
 kiirus_json["alla piiri"] = true;

}
```

Et meil oleks olemas ülevaade programmi tööst, prindime kiirus_json objekti serial monitoris, ning saadame objekti sõne kujul WebSocket kliendi abil serverisse.

```cpp
void kiirus(){
 dist1 = kaugus();
 delay(1000);
 dist2 = kaugus();
 kiirus = dist1 - dist2;
 Serial.print("Kiirus: ");
 Serial.print(kiirus);
 Serial.println(" cm/s");
 kiirus_json["kiirus"] = kiirus;
 kiirus_json["aeg"] = 0;
 kiirus_json["alla piiri"] = true;
 Serial.println(kiirus_json);
 WSclient.send(JSON.stringify(kiirus_json));
}
```

Liigume edasi setup funktsiooni juurde. Paneme varem deklareeritud trigPin-i väljundiks(Sellest saadetakse ultrahelilaine) ning echoPin-i sisendiks(See tuvastab tagasipõrkunud laine).

```cpp
void setup() {
 pinMode(trigPin, OUTPUT);
 pinMode(echoPin, INPUT);
```

Alustame Serial Monitori 115200 baudi peal, loome wifi ühenduse, loome WebSocket ühenduse, määrame WebSocket kliendile onEveent funktsiooni, ja saadame serverile pingi.

```cpp
void setup() {
 pinMode(trigPin, OUTPUT);
 pinMode(echoPin, INPUT);
 Serial.begin(115200);
 WiFi.begin(ssid, password);
    Serial.print("Connecting to WiFi...");
   while(WiFi.status() != WL_CONNECTED){
   delay(500);
   Serial.print(".");
 }
 Serial.println("WiFi connected!");

 Serial.print("Connecting to websocket server");
 while(!WSclient.connect(websocket_server)){
   Serial.print(".");
 }
 Serial.println("WebSocket connected!");

 WSclient.onEvent(onEventsCallback);

 WSclient.ping();
}
```

Viimasena määrame loop funktsiooni. Mõõdame kõigepealt, kui kaugel sõiduk on, ning alustame kiiruse mõõtmist, kui sõiduk on 19-20 cm kaugusel. Täpse väärtuse asemel kasutame vahemikku, et arvestada väikse ebatäpsusega mõõtmisel.

```cpp
void loop() {
 distance = kaugus();
 if(distance > 19 && distance < 20){
   Serial.println("Kiiruse möötmine...");
   kiirus();
 } 
}
```

ESP32 poolt on nüüd kõik vajalik tehtud. Liigume edasi Node-RED juurde.

[Node-RED-is WebSockets sõnumite vastu võtmine](./node-red-websockets)

**Kasutatud allikad:**  
- [https://lastminuteengineers.com/arduino-sr04-ultrasonic-sensor-tutorial/](https://lastminuteengineers.com/arduino-sr04-ultrasonic-sensor-tutorial/)   
- [https://projecthub.arduino.cc/Isaac100/getting-started-with-the-hc-sr04-ultrasonic-sensor-7cabe1](https://projecthub.arduino.cc/Isaac100/getting-started-with-the-hc-sr04-ultrasonic-sensor-7cabe1)   
- [https://arduinojson.org/v7/example/generator/](https://arduinojson.org/v7/example/generator/)   
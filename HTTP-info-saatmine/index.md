---
title: 2. HTTP info saatmine. Parkimismaja.
layout: default
nav_order: 8
has_children: true
---

Kuna meie linnas on juba olemas valgusfoorid, võime eeldada, et seal liiguvad ka autod. Autode jaoks võiks olla meie linnas parkimismaja, mille kohta saame alati infot, millised parkimiskohad on võetud ja millised on vabad. 

Selle jaoks teeme Node-RED keskkonnas tabeli, kus on näidatud parkimiskohtade numbrid ning olek, ja saadame parkimiskohtade kohta infot ESP32-st HTTP POST päringu abil. 

TODO: Rohkem infot HTTP POST kohta?? Rohkem infot JSON kohta??
{: .todo}

Meie parkimismaja jälgimise süsteem hakkab töötama nii: Node-RED dashboard-il on tabel, kus on kirjas parkimiskoha number ning selle juures punane värv, kui parkimiskoht on hõivatud, ning roheline värv, kui parkimiskoht on vaba. ESP32 poolt muudame parkimiskoha olekut nupuga, mille vajutamisel parkimiskoha olek muutub, ning edastame info parkimiskohtade kohta JSON objektina HTTP POST päringu abil.

[Järgmine õpetus](./node-red-http)
---
layout: post
title:  "UniFi Migration: USG + separatem Controller zu UCG"
date:   2024-06-01 21:59:00 +0200
categories: ubiquiti unifi firewall migration
author: d33pjs
---

# Einfache Anleitung: Migration von UniFi Security Gateway (USG) zu UniFi Cloud Gateway Ultra (UCG)

## Einführung

Wenn du dich fragst, was die Abkürzungen im Titel bedeuten und worum es in diesem Blogeintrag geht, hier eine kurze Erklärung:

Meine betagte Firewall, das UniFi Security Gateway (USG), sollte ersetzt werden. Zuvor hatte ich schlechte Erfahrungen mit der UniFi Express (UX) gemacht. Nun gibt es jedoch das UniFi Cloud Gateway Ultra (UCG), das möglicherweise eine bessere Lösung darstellt und nicht so teuer wie die Dream Machines ist. Allerdings war es im Online-Shop oft ausverkauft.

Kurze Zusammenfassung:
- Die USG... ist alt. Eine reine Firewall.
- Die UX ist eine Kombination aus Firewall, Access Point und Management/Controller, geeignet für kleine Umgebungen (maximal 5 UniFi-Geräte).
- Die UCG ist eine reine Firewall und ein Management-Gerät (30+ UniFi-Geräte, 300+ Clients), jedoch kein Access Point.

Da ein Austausch solcher Geräte oft kompliziert sein kann, möchte ich mein Vorgehen beim Ersatz der alten USG durch eine UCG teilen.

## Rahmenbedingungen

Bei mir sieht die Netzwerkumgebung wie folgt aus:
- Provider FRITZ!Box
- 1x Firewall (alt: USG)
- 1x Access Point (U6-LR)
- 4x Switche (alle UniFi, von 16 Port PoE bis hin zu kleinen 5 Port Flex)
- Management/Controller (die "Network Application" von UniFi) auf einem Raspberry Pi 4 in einem separaten Management-Netz

## Der Plan
Die USG und der RasPi-Controller sollen (beide) durch die UCG ersetzt werden. Die USG konnte keinen ausreichenden IDS-Throughput mehr leisten und hatte häufige Probleme bei Regelwerksänderungen. Zudem wurde die Controller-Software nicht mehr automatisch aktualisiert (WTF, UniFI?).

---

## Vorgehen - Detailliert

Hier die detaillierten Schritte zur Migration, gefolgt von einer kurzen Zusammenfassung (TL;DR):

1. **Vorbereitung der UCG:**
   - UCG an einen freien Port der FRITZ!Box anschließen und einschalten.
   - Nach dem Booten der UCG (erkennbar über das LED-Display) mit der mobilen App verbinden (angeblich per Bluetooth?!).
   - Einige Fragen zur Erstinstallation beantworten. Die UCG wird beim Reboot automatisch ein Firmware-Update installieren (dauert etwa 15 Minuten).
   - In meiner Erstinstallation habe ich vergessen, die mobile App bei ui.com anzumelden. UCG war (vielleicht deshalb) anschließend nicht erreichbar.

2. **Reset der UCG:**
   - UCG resetten (Reset-Button 10 Sekunden gedrückt halten)
   - Währenddessen die mobile App bei ui.com anmelden
   - WLAN der FRITZ!Box einrichten und aktivieren, Handy mit dem FRITZ!Box WLAN verbinden (denn angeblich müssen die App und die UCG im selben Subnetz sein)

3. **Erneute Einrichtung:**
   - UCG erneut einrichten. Nach dem Neustart ist die UCG dann über https://unifi.ui.com oder die IP-Adresse https://192.168.115.3 erreichbar.

4. **Backup und Wiederherstellung:**
   - Aktuelles Backup vom Controller erstellen (Einstellungen, System, Backup, Download) und auf der UCG wiederherstellen. Trotz Versionsunterschied (7.4.156 auf dem RasPi, 8.1.127 auf der UCG) gab es keine Probleme.
   - Nach dem Reboot ist die UCG ein Abbild der alten USG + dem alten Controller, jedoch mit den neuen Funktionen der Version 8.

5. **Physischer Austausch:**
   - USG herunterfahren und durch UCG ersetzen.
   - Auf der FRITZ!Box die Einstellungen anpassen (Exposed Host, statische IP für die UCG).
   
6. **Geräte verbinden:**
   - **Wichtig** den alten UniFi-Geräte muss der neue Controller bekannt gemacht werden. Das geht **NUR** über den alten Controller unter: System, Advanced: "Inform Host". Dort die IP der neuen Firewall eintragen, fertig. Nach 10-15 Minuten (sollten) alle Geräte auf der neuen UCG online sein (und auf dem alten Controller: offline).

## Vorgehen - TL;DR

1. **UCG anschließen und starten:**
   - Erstinstallation nur über die mobile UniFi-App möglich.
   - UCG muss im selben Subnetz erreichbar.
   - Nach der Einrichtung über https://unifi.ui.com oder die IP-Adresse (https, kein separater Port) erreichbar.
   - Automatischer Firmware-Update dauert ca. 15 Minuten.

2. **Backup wiederherstellen:**
   - Backup wiederherstellen. Von Version 7.4.156 und Restore auf 8.1.127 funktionierte in meinem Setup einwandfrei.

3. **Austausch:**
   - USG herunterfahren, durch UCG ersetzen, UCG starten.
   - Über alten Controller "Inform Host" auf neue IP der UCG.

4. **Fertigstellen:**
   - Alle UniFi-Geräte verbinden, prüfen und fertig.

---

## Bekannte Probleme

### config.gateway.json (DNS, NAT)
Manuelle DNS-Einträge und NAT-Einstellungen müssen neu vorgenommen werden, da die `config.gateway.json` nicht übernommen wird.

### Access Point Probleme
802.1x-Profile wurden migriert, jedoch hatten einige Geräte Verbindungsprobleme. Ein kurzer Neustart des Access Points löste das Problem.

---

Mit dieser Anleitung sollte die Migration von einer USG zu einer UCG reibungsloser verlaufen. Falls du Fragen oder Anmerkungen hast, hinterlasse gerne einen Kommentar!
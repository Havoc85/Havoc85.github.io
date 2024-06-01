---
layout: post
title:  "UniFi Migration: USG + separatem Controller zu UCG"
date:   2023-12-27 20:37:00 +0200
categories: ubiquiti unifi firewall migration
author: d33pjs
---

# kurze Einführung

Um kurz alle Abzuholen, was die kryptischen Abkürzungen im Titel sein sollen und worum es in diesem Blogeintrag überhaupt geht, nachfolgend ein paar einführende Worte.

Meine betagte Firewall, das UniFi Security Gateway (typischerweise und nachfolgend USG abgekürzt), sollte ersetzt werden. Bei diesem Versuch hatte ich vor einigen Monaten schon Mal schlechte Erfahrungen mit der (damals) neu eingeführten UniFi Express (UX) gemacht, aber inzwischen ist das UniFi Cloud Gateway Ultra (UCG-Ultra, nachfolgend UCG) heraus gekommen die vielleicht endlich Erlösung bringt (und nicht so teuer wie die Dream Machines sind). Nur typisch Ubiquiti: Im Onlinestore war sie selten vorrätig. 

Ganz kurz: Die UX ist eine Firewall, Access Point und Management/Controller in Einem - gedacht für kleine Umgebungen (allerdings stand __nur im Kleingedruckten__, dass man maximal 5 weitere UniFi Geräte verwalten kann - ein Gerät zu wenig für mich, was außerdem zu komischen Phänomen bei der Migration geführt hat). Die UCG ist hingegen "nur" eine Firewall und ein Management (30+ UniFi Geräte, 300+ Clients - was auch immer diese Marketingzahlen von Ubiquiti heißen sollen) - kein Access Point.

Da so ein Austausch manchmal etwas hakelig sein, man bei den Gedankenspielen schnell ein Knoten im Hirn bekommen und man sich schnell den Ast auf dem man Sitzt absägen kann, hier mein Vorgehen zum Austausch der alten USG + Management durch eine UCG.

# Rahmenbedingungen

Bei mir findet man folgenden Aufbau vor:
* Provider FRITZ!Box
* 1x Firewall (alt: USG)
* 1x Access Point (U6-LR)
* 4x Switche (alle UniFi, von 16 Port bis hin zu kleinen 5 Port Flex)
* Management / Controller (also die von UniFi genannte "Network Application") auf einem Raspberry Pi 4 in einem separaten Management Netz

Das heißt, der Plan: USG Firewall und Raspi-Controller sollen durch durch die UCG ersetzt werden. Denn: Die USG konnte keinen ausreichenden IDS Throughput mehr leisten, hat sich sehr häufig (fast bei jeder Regelwerksänderung) verschluckt und die Controller Software wird nicht mehr automatisch geupdated. 

# Vorgehen - detailliert

Nun zur Migration. Meine Schritte im Detail, im Anschluss noch ein TL;DR - wobei ich mir vorstellen könnte, dass es hier und da auch noch Abkürzungen geben könnte
* Die UCG habe ich neben die USG an einen freien Port der FRITZ!Box gesteckt und eingeschaltet.
* Nachdem die UCG gebootet hat (über das kleine LED Display an der Vorderseite zu sehen), habe ich mich (leider) mit der mobilen App verbunden (das gehate darüber, wie man sowas machen kann, erspare ich mir) - das ging über Bluetooth recht simple.
* Ein paar Fragen zur Erstinstallation habe ich beantwortet, habe aber, nachdem die UCG wieder gerebootet wurde und automatisch ohne zu Fragen ein neues Firmware Update installiert hat (hat sicher 15 Minuten gedauert), vergessen mich bei ui.com anzumelden.
* Obwohl die UCG hochgefahren war, konnte ich sie nicht mehr finden - auch über die IP Adresse (über das FRITZ!Box Interface), kam ich an kein Management Interface heran (laut Nmap alle Ports geschlossen). Die mobile App hat die UCG auch nicht mehr gefunden (wobei ich hier auch noch mit einem anderen Netz hinter der USG verbunden war).
* Daher: Reset (10 Sekunden den Resetbutton an der Rückseite gedrückt halten) und während dem Reboot+Reset die mobile App an ui.com angemeldet.
* Während dessen habe ich zusätzlich das WLAN der FRITZ!Box eingerichtet, aktiviert und mein Handy mit dem FIRTZ!Box WLAN verbunden, statt mit meinem "internen Netz"... (auch hier spare ich mir den Hate für solche schlecht gemachten Autodiscovery Funktionen...)
* Also, alles auf Start, aber dieses Mal klappt's: die Fragen zur Erstinstallation schnell durchgeklickt, nach dem Neustart konnte ich die UCG dann (endlich) über https://unifi.ui.com bzw. über die IP Adresse https://192.168.115.3 (IP Subnetz auf meiner FRITZ!Box) erreichen.

Der Rest geht dann schnell:
* Von meinem Controller das aktuellste Backup (unter Einstellungen, System, Backup, Download) erstellt und heruntergeladen (waren bei mir so rund 50 MB) und auf der UCG in den selben Einstellungen auf Restore hochgeladen. Trotz des Versionsunterschiedes (auf dem RasPi hatte ich die 7.4.156 drauf) gab es keine Probleme. 
* Nach dem Reboot war die UCG bereits ein Abbild der alten USG+Controller, nur eben mit den neuen Funktionen der 8er Version (aktuell bei mir 8.1.127)
* Als nächstes habe ich die USG physisch durch die UCG ausgetauscht (und auf so Details geachtet wie Exposed Host Settings auf der FRITZ!Box und statische IP Adresse für die neue UCG, die die gleiche IP wie die alte USG bekommen sollte - da waren auch ein paar Neustarts auf der FRITZ!Box notwendig, bis da alles sauber war)
* Wichtig: Als nächstes müssen alle UniFi Geräte (siehe oben: Access Point und Switche) zur neuen UCG verbinden. Das geht über das alte Management: Dort unter System, Advanced: "Inform Host" (und dort dann einfach die IP der neuen Firewall eintragen). Nach 10-15 Minuten waren auf dem alten Controller alle Devices offline und auf der neuen UCG alle devices Online.

# Vorgehen - tl;dr

* UCG an freien Port mit Internet angesteckt und gestartet
    * Hinweise:
        * Erstinstallation nach Boot geht (inzwischen) nur über die mobile UniFi App
        * angeblich müsst ihr im selben Subnetz der neuen UCG sein, anders hat es bei mir auch nicht funktioniert
        * erreichbar ist die UCG __nach erfolgreicher Einrichtung__ dann über https://unifi.ui.com oder https://<ipaddresse> (kein Port wie 8443 oder so)
        * nach der Ersteinrichtung wurde in meinem Fall automatisch das aktuellste Firmwareupdate installiert (kann einige Minuten dauern, bei mir so ca. 15 Minuten)
* Restore eines Backups 
    * Hinweis: 
        * Backup von Version 7.4.156 und Restore auf 8.1.127 hat bei mir einwandfrei geklappt (siehe Known Issues, gerade bei config.gateway.json)
* (pyhsischer) Austausch: USG herunterfahren, USG durch UCG ersetzen, UCG starten
* über alten Controller (**Wichtig!**) "Inform Host" auf neue IP der UCG im Management Netz (also die IP, über die die UCG erreicht werden kann, es wird automatisch ein Reboot durchgeführt)
* Fertig (schaut Euch trotzdem nochmal kurz die nachfolgenden Known Issues bzw. Hickups an)

# Known Issues

## config.gateway.json (DNS, NAT...)
Nichts desto trotz gab es ein paar Probleme. Über die "magische" Datei `config.gateway.json` (auf dem Controller unter `/usr/lib/unifi/data/sites_default` zu finden) habe ich ein paar DNS Einträge (CNAMEs und so) und NAT Einstellungen vorgenommen. Die wurden (natürlich?) nicht übernommen. Inzwischen sind aber zumindest manuelle DNS Einträge über das jeweilige Endgerät in der Network Appliance zu hinterlegen. Das musste ich dann bei den wichtigsten Geräten manuell nachtragen.
Wer die Datei aber nie benutzt oder angepasst hat, hat hier auf jeden Fall keine Probleme.

## Access Point Hickup
Um eine SSID für IoT Geräte weiter aufzuteilen, habe ich mit 802.1x Profilen gearbeitet. Diese wurden zwar automatisch migriert, aber aus irgendeinem Grund konnten sich diverse Endgeräte nicht mit dem WLAN Verbinden. Allen voran der Thermomix, der natürlich just genau in dem Moment von meiner Partnerin Verwendung finden wollte. Kurz den AP stromlos machen, hat aber geholfen. Nach wenigen Sekunden konnten sich auch alle anderen Geräte weiter einwandfrei verbinden.



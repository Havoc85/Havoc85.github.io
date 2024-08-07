---
layout: post
title: "Analyse des Crowdstrike Ausfalls"
date: 2024-08-07 15:41:00 +0200
categories: crowdstrike crash microsoft windows ausfall
author: d33pjs
---

## Einführung

Gestern hat Crowdstrike ihre Analyse des Ausfalls unter [Crowdstrike, External Technical Root Cause Analysis - Channel File 291](https://www.crowdstrike.com/wp-content/uploads/2024/08/Channel-File-291-Incident-Root-Cause-Analysis-08.06.2024.pdf) veröffentlicht. In diesem Blogeintrag möchte ich auf diesen Bericht eingehen.

## Rückblick

Am Morgen des 19.07.2024 sind auf der ganzen Welt viele Millionen Endgeräte mit Windows Betriebssystem aufgrund eines Softwareproblems mit der Sicherheitssoftware von Crowdstrike (Falcon) ausgefallen. Dies führte international zu Flugausfällen und sogar zu Problemen an der Börse.
Nicht zu schweigen von den vielen Problemen bei Firmen und in diversen Lieferketten z.B. bei Automobilherstellern.

Crowdstrike, ein Hersteller von *Sicherheitssoftware* hat also geschafft, was Ransomware immer wieder probiert.

## technische Crowdstrike Grundlagen

Der CrowdStrike Falcon Sensor ist die Software, die auf dem jeweiligen Endgerät installiert wird und, laut Marketing, über Künstliche Intelligenz und maschinellem Lernen Euren PC sichert.

Diese KI-Modelle werden immer wieder aktualisiert. Diese Updates kommen von Telemetriedaten anderer Kunden (gefiltert und aggregiert) und werden von Menschen aus dem "Falcon Adversary OverWatch", dem "Falcon Complete" und den "CrowdStrike threat detection engineers" überarbeitet, verbessert bzw. überprüft. Außerdem werden diese Updates über die Cloud aktualisiert bzw. an die Kunden übertragen. Hierbei handelt es sich um kombinierte Daten aus built-in Sensor Inhalten und den so genannten Rapid Response Contents. Der Vorteil der Rapid Response Conents ist, dass damit neue Regeln für Angriffe an die Kunden ausgegeben werden können, ohne Updates für die Falcon Software an sich bereitstellen zu müssen. Diese Rapid Response Contents werden über so genannte Channel Files übertragen und vom Sensor Content Interpreter über eine Regular-Expression basierte Engine ausgeführt.

Jedes Channel File, ist mit einem bestimmten Template Typ ausgestattet. Diese Template Typen werden dann bei bestimmten Aktivitäten auf dem Endgerät gegenüber den Angriffsregeln aus den Rapid Response Contents gematched. Für die Version 7.11 des Falcon Sensors im Februar 2024 ist ein neuer Template Typ hinzugekommen, um IPC Aktivitäten zu überprüfen (Windows interprocess communication) und Angriffe mit so genannten "named pipes" zu identifizieren und zu verhindern. 

## Fehlerbild

Der seit Februar neu existierende Template Typ wurde so programmiert, dass 21 Parameter übergeben werden können. Das Channel File mit der Nummer 291, das am 19.07.2024 für die Kunden zur Verfügung gestellt wurde und erstmals den neuen Template Typ für IPC verwendete, brachte zwei neue IPC Template Typen mit. Eines davon nutze den 21. Parameter, der aber leider von der Fehlerprüfung
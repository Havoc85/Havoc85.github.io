---
layout: post
title:  "SBOM: Die Zutatenliste für Software"
date:   2024-12-17 21:29:00 +0100
categories: SBOM BOM CycloneDX SPDX OpenSource OSS
author: d33pjs
---

## SBOM – Die Zutatenliste für Softwareprodukte

In einer zunehmend digitalisierten Welt ist Software allgegenwärtig: Sie steuert unsere Produktionsabläufe, bildet das Rückgrat von Cloud-Infrastrukturen und ermöglicht neue Geschäftsmodelle. Eigentlich existiert kaum ein elektronisches Gerät ohne Software. Im Auto beispielsweise gibt es, vereinfacht gesagt, nicht nur Software im Entertainmentsystem, sondern auch im Bremssystem oder bei den Fensterhebern.

Doch wer überprüft eigentlich, aus welchen Bausteinen diese Software besteht? An dieser Stelle tritt das sogenannte *Software Bill of Materials (SBOM)* ins Rampenlicht – ein strukturierter Überblick aller in einem Softwareprodukt verwendeten Komponenten.

Im Folgenden möchten wir erklären, was ein SBOM ist, warum es so wichtig ist und welche Rolle Standards wie SPDX und CycloneDX spielen.

### Was ist ein SBOM?

Ein *Software Bill of Materials* ist eine detaillierte und maschinenlesbare Auflistung aller in einem Softwareprodukt enthaltenen Bestandteile. Dazu gehören nicht nur eigens entwickelte Quellcodes, sondern vor allem auch Open-Source-Bibliotheken, externe Frameworks und alle anderen Drittanbieter-Komponenten. Laut Statistiken besteht aktuelle Software durchschnittlich aus 80 % Open-Source-Komponenten. Das klingt viel, aber wenn man sich vor Augen hält, dass zum Beispiel das Ruby-on-Rails-Web-Framework aus Open-Source-Komponenten besteht und die verwendete Programmiersprache Ruby auch öffentlich ist, bestehen bei vielen Webprojekten schon die Grundlagen komplett aus Open Source.

Ein SBOM beschreibt also die “Zutaten” eines Softwareprodukts – ähnlich einer Zutatenliste auf Lebensmittelverpackungen.

**Das Ziel:** Transparenz, Rückverfolgbarkeit und ein klarer Überblick über Komponenten, deren Ursprung, verwendete Versionen, Lizenzen und, zum Zeitpunkt der Erstellung, bekannte Schwachstellen. Ein SBOM ist damit die Basis für ein fundiertes Risiko-, Qualitäts- und Compliance-Management in der Softwareentwicklung und -bereitstellung.

### Warum benötigt man ein SBOM?

**1. Aus der Perspektive des Software-Kunden:**  
Stellen Sie sich vor, Sie erwerben eine Software. Würden Sie nicht gerne wissen, aus welchen Bausteinen sie besteht? Erst recht, wenn diese Software für Sie geschäftskritisch ist?  
Zu viele veraltete Komponenten oder verwahrloste Bibliotheken lassen schnell Rückschlüsse auf Qualität, mögliche Sicherheitslücken oder mangelnde Wartbarkeit zu. Außerdem möchten Sie sicher sein, dass keine problematischen Lizenzen eingebunden sind, die plötzlich Ihr Geschäftsmodell gefährden (siehe [Mercedes vs. Nokia](https://www.kanzlei.biz/kanzlei-blog/patentstreit-zwischen-nokia-und-daimler-verkaufsstopp-fuer-mercedes-21-12-2020/) ([archive](https://web.archive.org/web/20210918221853/https://www.kanzlei.biz/kanzlei-blog/patentstreit-zwischen-nokia-und-daimler-verkaufsstopp-fuer-mercedes-21-12-2020/)) und [Ford vs. Avanci](https://www.chip.de/news/Streit-um-Patente-Ford-kann-Verkaufsverbot-und-Neuwagen-Verschrottung-in-Deutschland-doch-noch-abwenden_184268707.html) ([archive](https://web.archive.org/web/20220706104453/https://www.chip.de/news/Streit-um-Patente-Ford-kann-Verkaufsverbot-und-Neuwagen-Verschrottung-in-Deutschland-doch-noch-abwenden_184268707.html))). SBOMs schaffen hier Klarheit: Sie können, mehr oder weniger, auf einen Blick erkennen, ob verwendete Bibliotheken aktuell, sicher und sauber lizenziert sind.

**2. Aus der Perspektive des Software-Herstellers:**  
Für Softwareproduzenten bietet ein SBOM gleich zwei wesentliche Vorteile: Zum einen wirkt die Transparenz nach außen vertrauensbildend – ein Qualitätsmerkmal, das sich in der Kundenansprache positiv niederschlägt. Zum anderen hilft es auch intern, die Softwarewartung zu optimieren. Entwicklerteams wissen genau, welche Komponenten sie nutzen, können diese effizienter aktualisieren und Schwachstellen schneller identifizieren und beheben. Der Vollständigkeit halber sei gesagt, dass je nach eingesetzter Entwicklungsumgebung, Programmiersprache und Paketmanager hierfür auch andere Alternativen existieren. SBOMs können dennoch unterstützen – und, wie eingangs erwähnt, direkt den Kunden zur Verfügung gestellt werden.

**3. Aus der Perspektive der Zulieferer-Integration (Hard- oder Softwarehersteller):**  
Viele Hersteller, etwa von Embedded-Geräten oder Software-Stacks, erhalten Code von diversen Zulieferern. Ein SBOM ermöglicht ihnen, alle eingekauften Komponenten zentralisiert und übersichtlich auf Herz und Nieren zu prüfen: Qualität, Lizenzkonformität und Sicherheit. Nur so lässt sich sicherstellen, dass das Endprodukt den hohen Anforderungen an Zuverlässigkeit, Compliance und Sicherheit genügt – und das ohne zeitraubende, manuelle Analysen im Nachhinein. Denn besonders Software, die als Binärdatei übermittelt und weiterverwendet wird, lässt sich nur noch mit speziellen Tools analysieren, die dann unter Umständen nicht einmal alle eingesetzten Bibliotheken identifizieren. Das heißt: Wird der SBOM nicht aus dem Source Code heraus erstellt, können Teile unidentifiziert bleiben. Das heißt dennoch nicht, dass SBOMs auf die Binärdaten keinen Sinn ergeben – im Gegenteil.

### SBOM, *BOM – gibt es da Unterschiede?

SBOM ist eine spezielle Form einer sogenannten *Bill of Materials (BOM)*. Während eine BOM in der Industrie allgemein die Bauteile eines physischen Produkts (z. B. eines Autos oder Smartphones) beschreibt, ist ein SBOM auf Softwarekomponenten fokussiert. Das Prinzip bleibt jedoch ähnlich: Eine BOM oder SBOM dient der Transparenz und Rückverfolgbarkeit der in ein Produkt eingeflossenen Elemente. Inzwischen kristallisieren sich aber weitere *-BOMs heraus, die sich auf bestimmte Umgebungen spezialisieren: SaaSBOM (Software-as-a-Service BOM), AIBOM (A.I. BOM) / ML-BOM (Machine Learning BOM), HBOM/HWBOM (Hardware BOM), CBOM (Cryptographic BOM) uvm. Der SaaSBOM beispielsweise würde nicht (nur) Software-Komponenten und Bibliotheken auflisten, sondern auch verwendete Services einer genutzten Infrastruktur (z. B. Amazon-AWS-Services wie S3).

### SPDX vs. CycloneDX – Formate oder Protokolle?

SPDX (Software Package Data Exchange) und CycloneDX sind zwei weit verbreitete, standardisierte Formate, um ein SBOM zu erstellen. Beide sind als offene, maschinenlesbare Spezifikationen konzipiert, die den Datenaustausch zwischen verschiedenen Tools, Systemen und Organisationen vereinfachen. SPDX kommt von der Linux Foundation, CycloneDX von der OWASP (Open Worldwide Application Security Project). Sie lassen sich eher als *Formate* und *Standards* statt als Protokolle verstehen.

- **SPDX:**  
  Ursprünglich stark auf Lizenzinformationen fokussiert, hat sich SPDX inzwischen zu einem umfassenden Standard entwickelt, der auch Sicherheitsaspekte berücksichtigt. Ein Schwerpunkt liegt auf klar definierter Terminologie für Lizenzen, was besonders wichtig für die Einhaltung von Lizenzbestimmungen ist.

- **CycloneDX:**  
  CycloneDX hat seine Wurzeln im Bereich der Anwendungs- und Dependency-Analysen. Es deckt neben Lizenz- und Sicherheitsangaben auch Qualitäts- und Integritätsattribute ab. CycloneDX ist oftmals besonders nützlich, wenn man Schwachstellenmanagement und Bedrohungsanalyse in den Fokus rückt.

Beide Standards haben also ihre Stärken und decken zusammen den Großteil der typischen Anforderungen an ein SBOM ab. Inzwischen sind die Standards aber so umfangreich, dass sie fast ohne Verluste direkt untereinander konvertiert werden können.

### Was ist ein SBOM nicht?

Teilweise habe ich schon gelesen, dass es sich bei SBOMs um eine Art Blaupause handeln soll, die wie bei einem Hausbau die Grundlagen bzw. Architektur aufzeigen. Dies ist nicht der Fall und stimmt in mehrerelei hinsicht nicht. Denn: Ein SBOM wird *nicht* vor dem Entwickeln einer Software erstellt, sondern erst am Schluss. Ein SBOM ist daher auch kein Bauplan, mit dem man die Software 1:1 nachbauen könnte. Wer befürchtet, durch Offenlegung der verwendeten Komponenten seine “intellectual property” zu verlieren, sollte sich die Frage stellen, ob das Problem wirklich beim SBOM liegt oder nicht doch woanders. Denn ein SBOM ist im Wesentlichen eine Liste von Bibliotheksnamen und deren Versionen. Das entspricht in etwa einer Liste von Zutaten auf der bereits erwähnten Lebensmittelverpackung: Niemand kann allein aus der Zutatenliste das Rezept perfekt nachkochen, insbesondere wenn die eigentlichen „Geheimnisse“ (sprich: der eigene Quellcode oder interne Algorithmen) gar nicht enthalten sind.
Wer hier den Verlust von geistigem Eigentum fürchtet, muss sich fragen, ob seine Produktidee tatsächlich so bahnbrechend ist, dass schon die Nennung von Open-Source-Komponenten ausreicht, um seinen Code nachzubauen. Diese Ängste erinnern an die Widerstände, als im Nahrungsmittelsektor die Kennzeichnung von Inhaltsstoffen verpflichtend wurde. Auch damals gab es Bedenken, ob die Offenlegung von Zutaten zu Kopien oder Wettbewerbsnachteilen führt – heute wissen wir: Transparenz fördert das Vertrauen in Produkte und schützt Verbraucher, ohne den Markt zum Erliegen zu bringen.

Also: Keine Angst vor dem SBOMs! Eine schlichte Liste von Komponenten ist kein Quelltext. Wenn ein Hersteller der Meinung ist, dass man durch die Kenntnis der verwendeten Open-Source-Bibliotheken bereits seinen Sourcecode rekonstruieren könnte, dann ist vielleicht nicht SBOM das Problem, sondern die schlechte Architektur der Software oder die fehlende Differenzierung im Wettbewerb.

### Schlusswort: SBOMs – Ein wichtiger Schritt zur Transparenz

SBOMs sind weit mehr als nur ein Trend. Sie sind ein entscheidender Baustein für mehr Qualität, Sicherheit und Transparenz in der Software-Welt. Entwickler sollten SBOMs standardmäßig erstellen und, zumindest für die eigenen Kunden, veröffentlichen. Kunden wiederum sollten aktiv nach SBOMs fragen und von Herstellern, die nur widerwillig Auskunft geben, die Finger lassen. Denn die Wahrscheinlichkeit ist hoch, dass hier Sorgfalt und Offenheit fehlen.

Die Zeiten ändern sich und so werden wohl viele in Zukunft häufiger von "SBOMs" hören. Sobald neue EU- oder internationale Regulierungen wie der Cyber Resilience Act (CRA) die Transparenz einfordern, fällt es schwarzen Schafen umso schwerer, sich zu verstecken. Letztlich profitieren alle: Kunden erhalten einen zuverlässigen Überblick, Hersteller können ihre Prozesse verbessern, und der Markt gewinnt durch höhere Qualitätsstandards an Glaubwürdigkeit. Wer also einen SBOM auf Knopfdruck liefern kann oder sogar veröffentlicht, beweist damit ein höheres Maß an Professionalität, proaktives Sicherheitsdenken und Respekt vor den legitimen Informationsbedürfnissen seiner Kunden. Kurz gesagt: SBOMs sind ein starkes Signal für die Zukunft der Softwareentwicklung.
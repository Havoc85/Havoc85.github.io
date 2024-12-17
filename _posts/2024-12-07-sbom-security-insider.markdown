---
layout: post
title:  "Causa Kehl bei Security-Insider: Open Source & SBOM"
date:   2024-12-07 02:21:00 +0100
categories: SBOM CycloneDX SPDX OpenSource OSS Kehl
author: d33pjs
---

### Offener Brief an Herrn Dieter Kehl  

Sehr geehrter Herr Kehl,  

ich habe mit großem Interesse Ihren Artikel [„Open Source kommt in Zukunft nicht ohne SBOM aus“ auf Security-Insider](https://www.security-insider.de/open-source-software-braucht-sbom-a-ab67253f08be1785db87d428f45a297e/) ([archive](https://web.archive.org/web/20241207150253/https://www.security-insider.de/open-source-software-braucht-sbom-a-ab67253f08be1785db87d428f45a297e/)) gelesen. Es ist erfrischend zu sehen, dass das Thema SBOM (Software Bill of Materials) mehr Aufmerksamkeit bekommt – ein Konzept, das Transparenz und Sicherheit fördert und zweifellos ein zentraler Bestandteil moderner Softwareentwicklung sein sollte. Dennoch möchte ich auf einige Punkte eingehen, die in Ihrem Beitrag aus meiner Sicht nicht nur ungenau sind, sondern der Open-Source-Community und ihrer Bedeutung für die gesamte Softwarewelt nicht gerecht werden.  

#### 1. Die Überschrift – ein Irrtum in sich  

„Open Source kommt in Zukunft nicht ohne SBOM aus“ ist eine plakative Überschrift, aber inhaltlich schlicht falsch. Der Kern von Open Source liegt in seiner Transparenz. Per Definition kann jeder den Quellcode einsehen, prüfen und bewerten – das ist der eigentliche Vorteil von Open Source. Natürlich kann SBOM hier ein nützliches Werkzeug sein, aber Open Source ist nicht darauf angewiesen, um Transparenz zu bieten.  

Und: ein Blick auf bestehende Systeme wie `npm` oder `pip` zeigt, dass es oft bereits speziell auf die Bedürfnisse der jeweiligen Ökosysteme um eine Programmiersprache herum zugeschnittene Lösungen gibt. Diese fördern Transparenz und Abhängigkeitserkennung – ein SBOM ist hier ein weiteres Werkzeug, aber keinesfalls der einzige Weg zur Sicherstellung von Sicherheit und Nachvollziehbarkeit.  

#### 2. Zahlen und Statistik – wo ist die Grundlage?  

Sie erwähnen, dass 84 % der untersuchten Codebasen mindestens eine Open-Source-Komponente enthalten, und beziehen sich dabei auf einen Report von OpenLogic. Das erscheint wenig überraschend, da Open Source in nahezu jedem Softwareprojekt eine Rolle spielt. Von Programmiersprachen wie Python, Go oder Rust bis hin zu Frameworks wie Django oder React – Open Source ist das Fundament moderner Software.

Außerdem frage ich mich, wie viele Codebasen überhaupt untersucht wurden. Und vor allem: Wie konnten diese Codebasen analysiert werden, wenn sie nicht öffentlich waren? Wenn sie jedoch öffentlich waren, wären sie dann nicht per Definition als Open Source zu betrachten?

Ihr Verweis auf die Bitkom-Umfrage, in der angeblich nur 69 % der Unternehmen Open Source aktiv einsetzen, zeigt für mich eher, dass ein erheblicher Teil der IT-Verantwortlichen keine ausreichende Transparenz über ihre eigenen Systeme hat. Open Source ist allgegenwärtig – nicht nur im Unternehmensumfeld, sondern längst auch zuhause: OpenHAB, Home Assistant und viele andere Projekte finden zunehmend Anwendung. Und ich habe bislang kein Unternehmen erlebt, das nicht irgendwo Apache, Nginx, Docker oder Kubernetes einsetzt. Selbst auf dem Desktop kommen mit Chromium oder Firefox Open-Source-Lösungen zum Einsatz. Es scheint, als würde hier die tatsächliche Verbreitung massiv unterschätzt.

#### 3. Das Wording – gefährlich irreführend  

Die Formulierung „Open-Source-Schwachstelle“ halte ich für hochgradig problematisch. Schwachstellen sind kein exklusives Merkmal von Open Source. Die proprietäre Softwarewelt ist genauso betroffen, oft sogar schlimmer, weil Lücken länger bestehen bleiben können, bevor sie entdeckt werden. Die Überschrift "Das Spiel mit dem Open-Source-Feuer" möchte ich mit diesen Hintergründen erst gar nicht kommentieren.

Noch absurder wird es, wenn Sie den SolarWinds-Angriff als Beispiel anführen – dieser hatte rein gar nichts mit Open Source zu tun. Hier wurde ein Build-System kompromittiert, was schlicht auf schlechte interne Sicherheitspraktiken des betroffenen Unternehmens zurückzuführen ist. Open Source an dieser Stelle zu erwähnen, ist nicht nur unpassend, sondern vermittelt ein völlig falsches Bild.

Und das Problem bei Log4j lag vor allem in schlechten Entwicklungsprozessen: Viele Softwarehersteller hatten gar keinen Überblick darüber, ob sie die Log4j-Bibliothek in ihren Java-Produkten verwenden. Selbst heute tauchen noch Hersteller von proprietärer Software auf, die Log4j fixen. Genau hier hätte mutmaßlich ein SBOM helfen können – und zwar nicht nur in internen Entwicklungsprozessen, sondern auch Kunden. Ich erinnere mich, dass ich damals Wochen, teilweise Monate, auf Antworten von Herstellern warten musste. Jahre später stellte sich bei einigen Produkten, zu denen ich nie ein offizielles Statement erhalten habe, heraus, dass sie verwundbar waren. Das hat das Unternehmen, für das ich damals arbeitete, einem enormen Risiko ausgesetzt. In dieser Situation hätte ich mir eine funktionierende Transparenz durch SBOMs gewünscht.

#### 4. Die Sicht auf Open Source – eine verpasste Chance  

Es ist schade, dass Sie die Open-Source-Community in Ihrem Artikel so pauschal kritisieren. Die Realität ist, dass Open Source vorangig von Personen vorangetrieben wird, die ihre private Zeit und Energie investieren, um großartige Projekte zu schaffen. Unternehmen profitieren massiv davon, investieren aber oft nichts zurück. Statt diese Anstrengungen zu würdigen, wird der Fokus auf vermeintliche Gefahren gelegt – ohne zu erwähnen, dass viele Schwachstellen entstehen, weil Unternehmen (teilweise "absichtlich" oder zumindest im vollen Wissen) veraltete Versionen einsetzen und nichts zur Weiterentwicklung der Open Source Komponenten beitragen.

Sie erwähnen selbst die erheblichen Einsparungen durch den Verzicht auf teure Lizenzen. Wäre es da nicht angemessen, wenn ein Teil dieser Einsparungen zurückfließt? Sei es in Form von Patches, der Entwicklung neuer Features oder auch einfach nur durch eine kleine Geste wie ["Buy Me a Coffee"](https://buymeacoffee.com/) für die Entwicklerinnen und Entwickler, die diese Open-Source-Projekte und damit oft die Fundamente der Softwarehersteller möglich machen.

#### 5. SBOM – ja, aber richtig  

Ich stimme Ihnen zu, dass SBOMs eine große Rolle in der Zukunft der Softwareentwicklung spielen bzw. hoffentlich spielen werden. Doch die Diskussion sollte differenzierter geführt werden. Tools wie `syft`, `trivy` oder `cdxgen` existieren bereits und machen die Erstellung von SBOMs denkbar einfach. Dass zwei Formate – SPDX und CycloneDX – aktuell um die Vorherrschaft kämpfen, ist eine wichtige Entwicklung, die leider in Ihrem Artikel unerwähnt bleibt. Aus meiner Sicht helfen solche Stichworte den Lesern, weiterführende Informationen zu recherchieren. 

Übrigens: Beide Standards sind Produkte der Open-Source-Community: SPDX stammt von der Linux Foundation, CycloneDX von OWASP. Das zeigt, wie sehr Open Source bereits an Lösungen arbeitet, auch im Bereich proprietäre Software die Transparenz und Sicherheit zu verbessern.

#### Fazit  

Herr Kehl, ich schätze es, dass Sie das Thema SBOM aufgreifen, doch Ihr Artikel stellt Open Source in ein unverdient schlechtes Licht. Open Source ist nicht das Problem, sondern ein zentraler Bestandteil der Lösung. Eine ausgewogenere, besser recherchiertere, weniger manipulative Darstellung wäre wünschenswert gewesen.  

Mit besten Grüßen

[Update 07.12.2024: Habe zusätzlich den Ursprunungsbeitrag von Security-Insider als Wayback Machine Link hinzugefügt, die Darstellung etwas verbesert, Ergänzung in Kapitel 2, zweiter Abschnitt, Rechtschreibung in Kapitel 4, zweiter Absatz verbessert.]

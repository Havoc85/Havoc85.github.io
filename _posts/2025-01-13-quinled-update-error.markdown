---
layout: post
title: "QuinLED Update failed"
date: 2025-01-13 19:15:00 +0100
categories: [quinled, wled, update, esp32]
lang: de-en
author: d33pjs
---

## DE: Fehlerhaftes Update eines QuinLED Controllers

(see EN below)

### Ausgangslage

Normalerweise kann ich über Home Assistant ganz unkompliziert meinen [QuinLED Dig Uno LED Controller](https://quinled.info/pre-assembled-quinled-dig-uno/) OTA (Over-the-Air) updaten.
Das klappte aber, komischerweise, nicht mehr von Version 0.14.4 auf 0.15.0. Sowohl das OTA-Update als auch das manuelle Update über die WLED WebUI schlugen fehl.

Nach intensiver Recherche in diversen Reddit-Beiträgen und auf GitHub fand ich schließlich die richtige Lösung. Diese möchte ich hier mit euch teilen, um euch Zeit und Frustration zu ersparen.

### Die Lösung

1. **Backup erstellen**: Bevor ihr fortfahrt, erstellt unbedingt ein Backup eurer aktuellen Einstellungen (und Presets). Das folgende Vorgehen löscht **ALLE** Daten, da der ESP32 neu partitioniert wird.
2. **ESP32 abnehmen**: Beim Dig Uno ist der USB Port von einer Sicherung verdeckt. Daher: Strom entfernen und das weiße Board einfach nach oben abnehmen. Das ist der ESP32. Nur der wird benötigt. Das ist in meinem Fall sowieso geschickt, weil der Rest verbaut ist und ich mir sparen konnte, das kompletten QuinLED abzubauen.
3. **Flashen über USB**: Verbindet den ESP32 über USB-C mit Eurem PC oder Mac. Besucht die Website [install.quinled.info](https://install.quinled.info/) und flasht euer Board.. Zum Zeitpunkt dieses Beitrags (13. Januar 2025) bietet die Website jedoch nur die Version 0.14.2 an. Dann führt einfach ein Downgrade durch.
4. **Downgrade**: Flasht euer Board mit der 0.14.2-Version, auch wenn das einen Downgrade bedeutet. In meinem Fall, von 0.14.4 auf 0.14.2, hat dies problemlos funktioniert.
5. **Update auf 0.15.0**: Danach könnt ihr (mit der neuen Partition; siehe zusätzliche Ressourcen) auf die neuste Version updaten
   - Ladet die aktuelle WLED-Firmware für ESP32 von [Aircoookie/WLED](https://github.com/Aircoookie/WLED/releases) herunter (z. B. `WLED_0.15.0_ESP32.bin`). Alternativ gibt es eine angepasste QuinLED-Version unter [intermittech/QuinLED-Firmware](https://github.com/intermittech/QuinLED-Firmware/releases).
   - Flashen könnt ihr entweder erneut über USB oder eventuell auch über OTA.

### Nach dem Flashen

- Verbindet euch über WLAN mit dem Board (SSID: `WLED-AP`, Passwort: `wled1234`). Oder über den Qr Code hier:
  ![WLED-AP Connection QR-Code](/assets/wled-ap-conn-qrcode.png)

- Ruft die Web-Oberfläche unter [http://4.3.2.1](http://4.3.2.1) und fügt unter Config Eure WLAN Settings hinzu. Übrigens: iOS hat nach ein paar Sekunden wieder auf mein "normales" WLAN zurück geschwenkt, weil "kein Internet". Behaltet das einfach im Hinterkopf, wenn ihr Euer Smartphone nutzen möchtet.
- Fertig. Danach könnt ihr den QuinLED wieder entsprechend in Eurem "normalen" WLAN erreichen.

- Eine detaillierte Anleitung gibt es unter [kno.wled.ge](https://kno.wled.ge/basics/getting-started/#quick-start-guide).

### Zusätzliche Ressourcen

- Reddit-Beitrag mit relevanten Informationen: [Reddit: Any idea how to update to wled 15 the quinled quad dig? updates always fails](https://www.reddit.com/r/WLED/comments/1h3mojl/comment/m1ntmc5/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button)
- Alternativer WLED Web Installer: [install.wled.me](https://install.wled.me/) (nicht von mir getestet).
- Relevante GitHub-Issues:
  - [Github: ESP32 Unable to upgrade from 0.14.4 to any 0.15.x releases #4369](https://github.com/Aircoookie/WLED/issues/4369)
  - [Github: Kommentar zur Erläuterung aus #4369](https://github.com/Aircoookie/WLED/issues/4369#issuecomment-2530816154)

---

## EN: Faulty Update of a QuinLED Controller

(see DE above)

### The Situation

Normally, I can easily update my [QuinLED Dig Uno LED Controller](https://quinled.info/pre-assembled-quinled-dig-uno/) OTA (Over-the-Air) via Home Assistant. However, for some reason, the update from version 0.14.4 to 0.15.0 failed. Both the OTA update and the manual update via the WLED WebUI didn’t work.

After extensive research in various Reddit posts and on GitHub, I finally found the right solution. I want to share it here to save you time and frustration.

### The Solution

1. **Create a Backup**: Before proceeding, be sure to back up your current settings (and presets). The following steps will **ERASE ALL** data as the ESP32 partition changes.
2. **Remove the ESP32**: On the Dig Uno, the USB port is blocked by a fuse. So, disconnect the power and simply lift the white board upwards. This is the ESP32. Only this part is needed. In my case, this was especially convenient because the rest of the controller was installed, and I didn’t have to dismantle the entire QuinLED.
3. **Flash via USB**: Connect the ESP32 to your PC or Mac using a USB-C cable. Visit [install.quinled.info](https://install.quinled.info/) and flash your board. As of this post’s date (January 13, 2025), the website only offers version 0.14.2. Proceed with the downgrade.
4. **Downgrade**: Flash your board with version 0.14.2, even if it’s a downgrade. In my case, from 0.14.4 to 0.14.2, this worked without issues.
5. **Update to 0.15.0**: Afterward, you can update to the latest version with the new partition (see Additional Resources).
   - Download the latest WLED firmware for ESP32 from [Github: Aircoookie/WLED releases](https://github.com/Aircoookie/WLED/releases) (e.g., `WLED_0.15.0_ESP32.bin`). Alternatively, there’s a customized QuinLED version available at [Github: intermittech/QuinLED-Firmware releases](https://github.com/intermittech/QuinLED-Firmware/releases).
   - Flash either via USB again or possibly OTA.

### After Flashing

- Connect to the board via Wi-Fi (SSID: `WLED-AP`, Password: `wled1234`). Or use this QR Code:
  ![WLED-AP Connection QR-Code](/assets/wled-ap-conn-qrcode.png)

- Access the web interface via [http://4.3.2.1](http://4.3.2.1) and add your Wi-Fi settings under Config. Btw. iOS was switching back to my default Wifi, because "no Internet" - just keep that in mind if you're using your smartphone.
- Done. You can now reach the QuinLED on your regular Wi-Fi network.

- A detailed guide is available at [kno.wled.ge](https://kno.wled.ge/basics/getting-started/#quick-start-guide).

### Additional Resources

- Reddit post with relevant information: [Reddit: Any idea how to update to wled 15 the quinled quad dig? updates always fails](https://www.reddit.com/r/WLED/comments/1h3mojl/comment/m1ntmc5/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button)
- Alternative WLED Web Installer: [install.wled.me](https://install.wled.me/) (not tested by me).
- Related GitHub Issues:
  - [Github: ESP32 Unable to upgrade from 0.14.4 to any 0.15.x releases #4369](https://github.com/Aircoookie/WLED/issues/4369)
  - [Github: Solution Explanation from #4369](https://github.com/Aircoookie/WLED/issues/4369#issuecomment-2530816154)

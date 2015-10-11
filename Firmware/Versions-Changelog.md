# Changelog
## Freifunk Bremen Versionen
### 2015.1.2+bremen2~testing
* Gluon-Version: [2015.1.2](http://wiki.bremen.freifunk.net/Firmware/Changelog#gluon-versionen_2015-1-2)
* VPNs 4-6 hinzugefügt  
  Test, ob per Ansible aufgesetzte VPNs einwandfrei funktionieren

### 2015.1.2+bremen1~testing
* Gluon-Version: [2015.1.2](http://wiki.bremen.freifunk.net/Firmware/Changelog#gluon-versionen_2015-1-2)

### 2015.1.1+bremen1
* Gluon-Version: [2015.1.1](http://wiki.bremen.freifunk.net/Firmware/Changelog#gluon-versionen_2015-1-1)
* Node-Namen-Präfix `ffhb-` nicht mehr standardmäßig vorgegeben

### 2014.4+bremen0 - **erste Stable** ([28.06.2015](http://bremen.freifunk.net/blog/2015/06/28/Erste-stabile-Firmware.html))
urspr. 0.6~testing1 ([03.05.2015](http://bremen.freifunk.net/blog/2015/05/03/Neue-Firmware.html))
* Gluon-Version: [2014.4](http://wiki.bremen.freifunk.net/Firmware/Changelog#gluon-versionen_2014-4)

### 0.5~testing5 ([06.09.2014](http://bremen.freifunk.net/blog/2014/09/06/Neue-Testing-Channel-Survey.html))
* Gluon-Version: [2014.3](http://wiki.bremen.freifunk.net/Firmware/Changelog#gluon-versionen_2014-3)
* Kanalanalyse-Komponenten integriert
* WLAN-Feintuning
  * 2,4Ghz-Kanalbreite von 40Mhz auf 20Mhz reduziert
  * Multicast-Rate auf 6 Mbit/s herabgesetzt (die Grenze ab wann 2 Knoten Meshen/sich verbinden)
* DHCP-Server und IPv6-Router hinter einem Node werden nicht mehr durchgelassen ("Funktion Internet" im Freifunk-Netz sonst leicht störbar)

### 0.5~testing4 ([08.08.2014](http://bremen.freifunk.net/blog/2014/08/08/Neue-Testing.html))
* Gluon-Version: [2014.3](http://wiki.bremen.freifunk.net/Firmware/Changelog#gluon-versionen_2014-3)
* Ende der WLAN-Probleme (mit Überwachung evtl. Probleme)
* Autoupdater standardmäßig an

### 0.5~testing3 ([23.06.2014](http://bremen.freifunk.net/blog/2014/06/23/Facebook-und-Testing-3.html))
* Gluon-Version: [2014.2](http://wiki.bremen.freifunk.net/Firmware/Changelog#gluon-versionen_2014-2)
* Mesh-ESSID von batman.bremen.freifunk.net zu mesh.ffhb geändert um Verwirrung zu vermeiden
* neue WLAN-Treiber und -Software, dadurch hoffentlich keine Signal-Abbruch-Probleme mehr
* vom Knoten übertragene Statistik wird nun komprimiert
* Probleme mit TP-LINK TL-WR841N/ND v9 behoben
* Firewall block alles Eingehende auf dem WAN-Port außer SSH

### 0.5~testing2
* Gluon-Version: [2014.2](http://wiki.bremen.freifunk.net/Firmware/Changelog#gluon-versionen_2014-2)

#### bekannte Probleme
* Signal-Abruch-Problem auf 2,4Ghz
* TP-LINK TL-WR841N/ND v9 macht Probleme


### 0.5~testing1 - **erste Testing** ([24.03.2014](http://bremen.freifunk.net/blog/2014/03/24/testing-firmware.html))
* Gluon-Version: [2014.2](http://wiki.bremen.freifunk.net/Firmware/Changelog#gluon-versionen_2014-2)
* alle Nightly-Geräte haben auf den Testing-Branch gewechselt
* Doppel-Key-Problem behoben



## Gluon-Versionen
Hier aufgeführt sind die von Freifunk Bremen genutzten Gluon-Versionen mit einer kurzen Zusammenfassung von relevanten Änderungen.

### [2015.1.2](http://gluon.readthedocs.org/en/v2015.1.2/releases/v2015.1.2.html)
* Unterstützung für mehr Geräte
* eigene Images (nur umbenannte Kopien) für einige Ubiquiti Geräte, welche bis jetzt das Bullet M Image benutzen mussten, für mehr Klarheit und Einfachheit
* Download-Link von OpenSSL korrigiert

### [2015.1.1](http://gluon.readthedocs.org/en/v2015.1.2/releases/v2015.1.1.html)
* Unterstützung für ein weiteres Gerät
* Download-Link von OpenSSL korrigiert
* Problem mit der LED-Anzeige für die Signalstäkre bei TP-LINK CPE210/510 behoben

### [2015.1](http://gluon.readthedocs.org/en/v2015.1/releases/v2015.1.html)
* Unterstützung für mehr Geräte
* Zweisprachiger Config-Mode (Deutsch; Englisch)
* Mesh-on-LAN
* Option um Mesh oder Client Netzwerke standardmäßig auszuschalten
* fastd "performance mode" mit ausgeschalteter Verschlüsselung
* optionales Feld für Höhe in Geo-Daten
* Vorbereitung von announced um alfred zu ersetzen
* Option um beim Aufruf des Autoupdaters temporär einen anderen Branch zu wählen
* vorgegebenes Namens-Prefix jetzt optional

### [2014.4](http://gluon.readthedocs.org/en/v2014.4/releases/v2014.4.html)
* Update auf OpenWRT 14.09 (Barrier Breaker)  
  mehr Stabilität und mehr Performance
* Unterstützung für mehr Geräte
* Privates WLAN bridged with WAN möglich
* fastd v16 mit UMAC  
  mehr Sicherheit und Performance
* Option um SSH-Keys fest in die Firmware zu integrieren
* Node-Status-Page erkennt Nachbar-Nodes und zeigt ihre Namen an und macht sie klickbar

### [2014.3](http://gluon.readthedocs.org/en/v2014.4/releases/v2014.3.html)
* Unterstützung für ein weiteres Gerät
* Autoupdater wurde neugeschrieben
  * geplante Updates mit Datumsangabe möglich
  * zeitlich verteiltes Update möglich mit Angabe eines Zeitraums
* erste Implementation von announced für querys von nodeinfo Daten für zukünftige Statuspage
* erste Implementation für fastd/VPN über IPv6
* Mesh-on-WAN hinzugefügt
* site.conf wird nach dem Kompilieren validiert
* ath9k verbessert für bessere WLAN Stabilität
* IPv6 nun präferiert für wget (Autoupdater und NTP)
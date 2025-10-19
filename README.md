<img width="915" height="997" alt="image" src="https://github.com/user-attachments/assets/c92c141a-96d5-4e65-b02e-8a1b97d73354" />

# ⚡ Zendure HUB1200 + ACE1500 Flow mit openDTUonBattery

Dieser Node-RED Flow steuert ein Zendure Solarflow-System mit HUB1200 und ACE1500, ergänzt durch eine openDTU-Integration auf Batteriebasis.  
Die Datei heißt:

**Zendure HUB1200+ACE1500 openDTUonBattery.json**

Der Flow nutzt **Node-RED Dashboard 2.0** und ist modular aufgebaut. Standort, ioBroker-Datenpunkte und InfluxDB-Zugang müssen vom Nutzer selbst angepasst werden.

---

## 🔧 Funktionen im Überblick

### ☀️ Sonnenzeiten mit Offset
- Berechnung von Sonnenaufgang und Sonnenuntergang über `node-red-contrib-sun-position`.  
- Zeiten werden lokal (Europe/Berlin) formatiert und als globale Variablen (`zendure.sunrise`, `zendure.sunset`) gespeichert.  
- Zwei UI-Slider erlauben die Einstellung eines Offsets in Minuten. Daraus entstehen `sunrisecalc` und `sunsetcalc`.

### 🔋 Basismodus (manuell)
- Ein UI-Schalter im Dashboard setzt `zendure.control.basemode` auf `true` oder `false`.  
- Keine automatische Steuerung mehr – der Nutzer entscheidet selbst.

### ⚡ Entladefreigabe-Logik
Alle 5 Sekunden wird geprüft, ob Entladen erlaubt ist (`zendure.control.discharge`).  
Die Logik läuft gestapelt ab:
1. Wenn `basemode` aktiv → Entladen erlaubt  
2. Wenn Batterie tagsüber voll (`BATTERYFULLONDAY`) → Entladen erlaubt  
3. Wenn Nacht erkannt (`isNight`) → Entladen erlaubt  
4. Sonst → Entladen gesperrt  

### 🌦️ minSOC bei schlechtem Wetter
- Ein Subflow setzt den minimalen Ladezustand (minSOC) auf 10 %, wenn ein externes Flag `true` ist.  
- Andernfalls wird minSOC auf 0 % gesetzt.  
- Ausgabe erfolgt als Payload und kann direkt in die Steuerung übernommen werden.

### 📊 Werte für SOC
- Alle 10 Sekunden werden via ioBroker folgende Datenpunkte abgefragt:  
  - Zellspannungen: `minVol`, `maxVol` (für bis zu 4 Packs)  
  - Leistung: `outputPackPower` (Akku-Eingang), `packInputPower` (Akku-Ausgang)  
- Die Werte werden im `join`-Node gesammelt und als Objekt zusammengeführt.  
- Die Abschaltung nach `minVol` erfolgt gestapelt – alle Werte werden gemeinsam ausgewertet.

### 📈 InfluxDB Logging
- Zellspannungen (`low_minVol`) werden in eine InfluxDB geschrieben.  
- Konfiguration ist vorbereitet für InfluxDB v2 (Bucket + Org).  

---

## 🧰 Voraussetzungen

- Node-RED mit folgenden Nodes:
  - `node-red-contrib-sun-position`
  - `node-red-contrib-cron-plus`
  - `node-red-dashboard` (Version 2.0)
  - `node-red-contrib-iobroker`
  - `node-red-contrib-influxdb`

---

## 🚀 Nutzung

1. Flow importieren: **Zendure HUB1200+ACE1500 openDTUonBattery.json**  
2. Standortdaten in der `position-config` Node setzen  
3. ioBroker-Datenpunkte und InfluxDB-Zugang anpassen  
4. Dashboard öffnen und Offsets/Basismodus steuern  

---

## 📄 Lizenz

MIT – frei nutzbar, modifizierbar und weiterverwendbar.

---

## ⚠️ Haftungsausschluss

Dieser Flow wird ohne jede Gewährleistung bereitgestellt. Nutzung auf eigene Verantwortung.  
Der Autor übernimmt keinerlei Haftung für Schäden, Datenverluste oder Fehlfunktionen, die durch den Einsatz dieses Flows entstehen können.  
Vor produktivem Einsatz sollte der Flow in einer Testumgebung geprüft und an die eigenen Anforderungen angepasst werden.

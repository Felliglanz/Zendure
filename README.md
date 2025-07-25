<img width="2047" height="1035" alt="image" src="https://github.com/user-attachments/assets/0d82ce20-d69c-4a30-9ece-67e2457fc589" />

# 🌞 Zendure Solarflow + ACE1500 + Node-RED

Ein vollständiger Node-RED-Flow zur intelligenten Steuerung deiner Zendure Solarflow-Anlage mit ACE1500 und openDTU.  
Mit Zellspannungs-Schutz (minVol), manueller SOC-Berechnung, Sonnenzeit-gesteuerter Entladelogik und manuell oder automatisch kontrollierten Lade- und Entladephasen.

---

## 🚦 Features im Überblick

- ✅ minVol-Schutz gegen Tiefentladung  
- 🔋 SOC-Berechnung per Wh-Integration (ohne Adapter)  
- ⛅️ Sommer-/Winterbetrieb über minVol-Grenzen  
- 💡 Sonnenzeitenlogik für Entladefreigabe  
- 🛑 Gate-Logik: Sperrt Stromfluss bei Bedarf  
- 🤖 Manuelle Steuerung über ioBroker-Datenpunkte  
- 📉 Optionales InfluxDB-Logging für Zellspannung  
- 🧱 Modularer Flow mit Subflows für einfache Erweiterung  

---

## 🧰 Voraussetzungen

| Komponente          | Beschreibung                                       |
|---------------------|----------------------------------------------------|
| Node-RED (v2+)      | Flow-Engine zur Logiksteuerung                     |
| ioBroker            | Adapter und Userdata-Verwaltung                    |
| EMQX MQTT Broker    | Kommunikation mit openDTU                          |
| openDTU (z. B. Deye)| Wechselrichtersteuerung über MQTT                  |
| sunrise-sunset      | Adapter für Sonnenzeitsteuerung                    |
| InfluxDB (optional) | Logging von Zellspannungswerten                    |
---

## 📁 ioBroker: erforderliche Datenpunkte

| Datenpunkt                                      | Typ     | Zweck                                 |
|------------------------------------------------|---------|---------------------------------------|
| `0_userdata.0.Zendure_Werte.minVol`            | Number  | Kleinste Zellspannung (aus Script)  
| `0_userdata.0.Steuerung.AC_Notladen_Start`     | Boolean | Notladen manuell aktivieren  
| `0_userdata.0.Steuerung.Entladen_Stop`         | Boolean | Entladung manuell stoppen  
| `0_userdata.0.Steuerung.Laden_Stop`            | Boolean | Laden manuell stoppen  
| `0_userdata.0.Steuerung.Zendure_Flow_Stop`     | Boolean | Flow komplett deaktivieren  
| `0_userdata.0.Steuerung.Zendure_schlecht_Wetter`| Boolean | minVol-Schwelle saisonal anpassen  
| `0_userdata.0.Steuerung.Zendure_Basismodus`    | Boolean | Geräte manuell schalten  
| `0_userdata.0.PV-Daten.SOC_calc`               | Number  | Berechneter SOC-Wert  

---

## ⚡ minVol – Zellspannungsbasierte Entladesperre

In ioBroker berechnest du `minVol` (kleinste Zellspannung aus 4 Packs) und speicherst ihn.  
Node-RED liest diesen Wert und vergleicht ihn mit `low_minVol`, z. B. 3.10 V.  
Bei Unterschreitung wird:
- das Gate geschlossen  
- Entladeleistung auf 0 W gesetzt  
- openDTU erkennt das → WR schaltet ab  

Freigabe erfolgt erst wieder nach Sonnenaufgang.

Beispiel:
```js
let min = Math.min(pack1, pack2, pack3, pack4);
setState("0_userdata.0.Zendure_Werte.minVol", min);
```

---

## 🔋 SOC-Berechnung (manuell & dynamisch)

Der Flow summiert die Energiebewegungen:

    ΔWh = (InputPower − OutputPower) × (Intervall / 3600)

Diese Wh ergeben den aktuellen Ladestand.  
Wenn `maxVol ≥ 3.57 V` erkannt wird:
- SOC = 100 %  
- Kapazität (`realCapacity`) wird auf den aktuellen Wert angepasst

Danach:

    SOC = accumulatedWh ÷ realCapacity × 100

→ Der berechnete SOC wird nach `PV-Daten.SOC_calc` geschrieben

Vorteil:  
- Keine Adapterabhängigkeit  
- Dynamischer Abgleich  
- Unabhängig von Herstellerlogik

---

## 🌡️ Sommer-/Wintermodus: minVol anpassen

Per Datenpunkt `Zendure_schlecht_Wetter` kannst du zwischen zwei Zellschutz-Schwellen wählen:

| Modus   | `low_minVol`   |
|---------|----------------|
| Sommer  | 3.05 V          |
| Winter  | 3.20 V          |

Der Flow passt diese Schwelle automatisch an und verwendet sie in der Gate-Logik.

---
## ⚠️ Haftungsausschluss, Debug & Credits

### 📌 Haftungsausschluss
Die hier vorgestellten Konzepte und Flows wurden aus Tests und Erfahrungswerten zusammengetragen. Es gibt keine Garantie für die Genauigkeit, Funktion oder Sicherheit bei abweichenden Systemen.  
👉 Verwende die Logik nur, wenn du sie verstehst und bei deinem Setup validieren kannst.

### 🐞 Debug-Hilfe
Falls der SOC nicht korrekt berechnet wird, prüfe:
- Stromsensor liefert korrekte Werte (`InputPower`, `OutputPower`)
- Zellenspannungen werden regelmäßig aktualisiert (`maxVol`, `minVol`)
- Der Flow läuft durch und speichert Daten (`SOC_calc`, `realCapacity`, etc.)
- Intervall-Zeit stimmt (z. B. alle 30 Sekunden)

📍 Tipp:  
Füge temporär einen Debug-Knoten hinter `accumulatedWh` ein, um die Zwischenwerte zu prüfen.

### 🎉 Credits
Entwickelt durch Community-Mitglieder aus dem ioBroker-Forum  
🌍 Open Source – Weitergabe & Anpassung ausdrücklich erlaubt!  
💬 Bei Fragen: Einfach hier weitermachen oder Forum aufsuchen

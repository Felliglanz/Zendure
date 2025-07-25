<img width="2047" height="1035" alt="image" src="https://github.com/user-attachments/assets/0d82ce20-d69c-4a30-9ece-67e2457fc589" />

# AC-Ladungs- und Entladungssteuerung für Zendure Solarflow 1200 & ACE1500 mit openDTUonBattery

## Überblick

Dieser Node-RED-Flow dient zur Steuerung der **Ladung und Entladung** der **Zendure Solarflow 1200 Batterie** und **ACE1500** in Kombination mit openDTUonBattery. Er optimiert die Energie-Nutzung deines Balkonkraftwerks und zusätzlicher PV-Anlagen. Ziel ist es, überschüssigen Solarstrom effizient zu speichern und bei Bedarf in deinem Haushalt zu nutzen – unter Berücksichtigung einer präzisen Regelung über **openDTUonBattery**.

## Vorteile

- **Dynamische Regelung durch openDTUonBattery**: Mit dem neuen Ansatz wird die Steuerung direkt an die Batterie abgegeben, basierend auf Echtzeit-Daten und Zuständen.
- **Effizientes Laden und Entladen**: Lade und entlade den Speicher dynamisch, abhängig von den aktuellen Bedingungen.
- **Automatisierte Schaltvorgänge**: Der Flow steuert Relais, Gate und AC-Modus automatisch für maximale Effizienz.
- **Optimale Akku-Überwachung**: Durch SOC-History wird der Akku-Zustand laufend überwacht und für eine bessere Kalibrierung gespeichert.

## Funktionsweise

1. **Laden**: Überschüssiger Solarstrom wird tagsüber zum Laden der Batterie verwendet.
2. **Entladen**: Die Abgabe wird freigegeben, sobald der Akku die obere Ladegrenze erreicht hat oder nach Sonnenuntergang.
3. **Notlademodus**: Wenn die Batterie auf kritische Werte wie 7 % SOC oder 2,9 V Zellspannung fällt, wird das Gate geschlossen und die Batterie in den Notlademodus versetzt.
4. **Gate-Steuerung**: Das Gate bleibt geschlossen, bis der SOC mindestens 50 % erreicht hat, danach wird es automatisch wieder freigegeben.
5. **Bypass-Modus**: Optional kann ein Bypass aktiviert werden, um die Steuerung zu umgehen.
6. **openDTU-Funktionalität**: Ermöglicht die dynamische Anpassung der Lade- und Entladeparameter über ein abgestimmtes DPL-System.
7. **Anpassbare Parameter**: Alle Schwellenwerte und Parameter, können via Flow Variablen definiert werden.
8. **Mauelle Schalter für Dashboard**: Im Flow sind rechts manuelle Schalter integriert, die man entweder in sein Dashboard oder in Google Home legen kann, falls man den HUB offline betreiben möchte, wie ich es tue. 

## Voraussetzungen

- **Node-RED**: Installiere Node-RED auf deinem System.
- **openDTU auf Batterie**: Stelle sicher, dass openDTU korrekt eingerichtet ist.
- **Leistungsmesser**: Zum Beispiel mit einem Lesekopf und Tasmota zur Erfassung des `currentpower`.
- **Sonnenzeiten-Adapter**: Ein Adapter für Sonnenaufgangs- und -untergangszeiten.
- **MQTT**: Kommunikation zwischen den Geräten muss über MQTT eingerichtet sein.
- **ioBroker**: Für die Datenübermittlung und Steuerung.

## Installation

1. **Flow importieren**: Öffne Node-RED und importiere den bereitgestellten Flow.
2. **Konfiguration prüfen**:
   - Stelle sicher, dass alle Nodes wie Sonnenzeiten, SOC-Werte und AC-Modus richtig konfiguriert sind.
   - Die Verbindung zu ioBroker und openDTU muss aktiv und stabil sein.
3. **openDTU einstellen**: Passe die Lade- und Entladegrenzen an (z. B. max. 830W Entladeleistung).

## Anpassungen

- **Gate-Steuerung**: Die Funktion `SOC min 50%` gewährleistet, dass das Gate erst bei einem SOC von 50 % oder mehr wieder geöffnet wird.
- **AC-Ladung**: Über die Funktion `Berechnung laden` wird dynamisch die Ladeleistung angepasst, abhängig von Überschussenergie.
- **Entladung**: Nutze die Funktion `Berechnung entladen`, um die Entladeleistung auf den maximalen SOC oder bis zum Notmodus zu regulieren.
- **Manuelle Steuerung**: Es stehen mehrere manuelle Steuerungsmöglichkeiten zur Verfügung (z. B. Entladestop, Notladen manuell starten).

## Haftungsausschluss

Die Verwendung dieses Flows erfolgt auf eigene Verantwortung. Teste ihn gründlich, bevor du ihn produktiv einsetzt. Der Autor übernimmt keine Haftung für direkte oder indirekte Schäden.

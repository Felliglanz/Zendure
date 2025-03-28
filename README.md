![image](https://github.com/user-attachments/assets/b3986f93-cd40-4db7-873f-593505ebe415)

#AC-Ladungs- und Entladungssteuerung für Zendure Solarflow 1200 & ACE1500 mit openDTUonBattery
Überblick
Dieser Node-RED-Flow dient zur Steuerung der Ladung und Entladung der Zendure Solarflow 1200 Batterie und ACE1500 in Kombination mit openDTUonBattery. Er optimiert die Energie-Nutzung deines Balkonkraftwerks und zusätzlicher PV-Anlagen. Ziel ist es, überschüssigen Solarstrom effizient zu speichern und bei Bedarf in deinem Haushalt zu nutzen – unter Berücksichtigung einer präzisen Regelung über openDTUonBattery.

Vorteile
Dynamische Regelung durch openDTUonBattery: Mit dem neuen Ansatz wird die Steuerung direkt an die Batterie abgegeben, basierend auf Echtzeit-Daten und Zuständen.

Effizientes Laden und Entladen: Lade und entlade den Speicher dynamisch, abhängig von den aktuellen Bedingungen.

Automatisierte Schaltvorgänge: Der Flow steuert Relais, Gate und AC-Modus automatisch für maximale Effizienz.

Optimale Akku-Überwachung: Durch SOC-History wird der Akku-Zustand laufend überwacht und für eine bessere Kalibrierung gespeichert.

Funktionsweise
Laden: Überschüssiger Solarstrom wird tagsüber zum Laden der Batterie verwendet.

Entladen: Nach Sonnenuntergang wird gespeicherte Energie verwendet, um den Haushaltsstrombedarf zu decken.

Notlademodus: Wenn die Batterie auf kritische Werte wie 20% SOC oder 3,2V Zellspannung fällt, wird das Gate geschlossen und die Batterie in den Notlademodus versetzt.

Gate-Steuerung: Das Gate bleibt geschlossen, bis der SOC mindestens 50% erreicht hat, danach wird es automatisch wieder freigegeben.

Bypass-Modus: Optional kann ein Bypass aktiviert werden, um die Steuerung zu umgehen.

openDTU-Funktionalität: Ermöglicht die dynamische Anpassung der Lade- und Entladeparameter über ein abgestimmtes DPL-System.

Voraussetzungen
Node-RED: Installiere Node-RED auf deinem System.

openDTU auf Batterie: Stelle sicher, dass openDTU korrekt eingerichtet ist.

Leistungsmesser: Zum Beispiel mit einem Lesekopf und Tasmota zur Erfassung des currentpower.

Sonnenzeiten-Adapter: Ein Adapter für Sonnenaufgangs- und -untergangszeiten.

MQTT: Kommunikation zwischen den Geräten muss über MQTT eingerichtet sein.

ioBroker: Für die Datenübermittlung und Steuerung.

Installation
Flow importieren: Öffne Node-RED und importiere den bereitgestellten Flow.

Konfiguration prüfen:

Stelle sicher, dass alle Nodes wie Sonnenzeiten, SOC-Werte und AC-Modus richtig konfiguriert sind.

Die Verbindung zu ioBroker und openDTU muss aktiv und stabil sein.

openDTU einstellen: Passe die Lade- und Entladegrenzen an (z. B. max. 830W Entladeleistung).

Anpassungen
Gate-Steuerung: Die Funktion SOC min 50% gewährleistet, dass das Gate erst bei einem SOC von 50% oder mehr wieder geöffnet wird.

AC-Ladung: Über die Funktion Berechnung laden wird dynamisch die Ladeleistung angepasst, abhängig von Überschussenergie.

Entladung: Nutze die Funktion Berechnung entladen, um die Entladeleistung auf den maximalen SOC oder bis zum Notmodus zu regulieren.

Manuelle Steuerung: Es stehen mehrere manuelle Steuerungsmöglichkeiten zur Verfügung (z. B. Entladestop, Notladen manuell starten).

Haftungsausschluss
Die Verwendung dieses Flows erfolgt auf eigene Verantwortung. Teste ihn gründlich, bevor du ihn produktiv einsetzt. Der Autor übernimmt keine Haftung für direkte oder indirekte Schäden.





![image](https://github.com/user-attachments/assets/eb0c8eb4-fa1b-4610-8e43-00fc17b0c809)




# AC-Ladungs- und Entladungssteuerung für Zendure Solarflow 1200 & ACE1500

## Überblick

Mit diesem Node-RED-Flow kannst du die **Energie** deines Balkonkraftwerks und einer zusätzlichen PV-Anlage im Haushalt effizient nutzen. Er steuert das **Laden und Entladen** der **Zendure Solarflow 1200 Batterie**, um überschüssigen Solarstrom zu speichern und bei Bedarf wieder abzugeben – ganz ohne Netzstrom. Der Flow sorgt für **optimale Nutzung** deines Solarstroms und hilft dir, deinen **Eigenverbrauch** zu maximieren.

## Vorteile auf einen Blick

- **Laden mit Überschussenergie**: Dein Akku wird mit überschüssigem Solarstrom geladen, wenn er verfügbar ist.
- **Automatisiertes Entladen bei Bedarf**: Nutze die gespeicherte Energie, wenn keine Sonne mehr scheint, um deinen Haushalt mit Strom zu versorgen.
- **Optimale Akku-Überwachung**: Durch die SOC-History wird der Ladezustand regelmäßig überprüft und die Kalibrierung des Akkus gewährleistet.

## Funktionsweise

1. **Laden**: Tagsüber (von Sonnenaufgang bis Sonnenuntergang) wird der Akku mit überschüssigem Solarstrom geladen – sowohl von deinem **Balkonkraftwerk** als auch einer **weiteren PV-Anlage** im Haushalt.
2. **Notladung**: Sollte der Akku unter 15% fallen, sorgt der Flow dafür, dass er mindestens auf **20%** aufgeladen wird, um eine Tiefentladung zu vermeiden.
3. **Entladen**: Nach Sonnenuntergang wird die Batterie entladen, um den Strombedarf deines Haushalts zu decken – ohne Netzstrom.
4. **SOC-History**: Der Flow überwacht den **State of Charge (SOC)** der Batterie, um sicherzustellen, dass der Akku immer korrekt kalibriert ist.

## Wichtige Parameter

- **Lade- und Entladezeiten**: Der Flow nutzt die Sonnenzeiten und arbeitet tagsüber zum Laden und nachts zum Entladen der Batterie.
- **Leistungswerte**: Der `currentpower`-Wert, der vom **Leistungsmesser** kommt, zeigt, ob überschüssiger Strom verfügbar ist.
- **Lade- und Entladegrenzen**: Der Akku wird nicht unter 20% entladen und wird bei Bedarf bis auf 100% geladen, um den SOC korrekt zu kalibrieren.

## Voraussetzungen

- **Node-RED**: Stelle sicher, dass Node-RED auf deinem System installiert ist.
- **Leistungsmesser**: Ein Gerät wie **Shelly** oder **Lesekopf mit Tasmota** ist erforderlich, um den Stromverbrauch zu messen.
- **Sonnenzeiten-Adapter**: Ein Adapter, der die Sonnenauf- und Untergangszeiten liefert.

## Installation und Anpassungen

1. **Flow importieren**: Lade den Flow in deine Node-RED-Instanz.
2. **Leistungsmesser konfigurieren**: Vergewissere dich, dass dein Leistungsmesser den `currentpower`-Wert korrekt überträgt.
3. **Sonnenzeiten konfigurieren**: Setze den Adapter für Sonnenzeiten ein.

Du kannst die Ladeleistung und die Schwellenwerte für den Notladebetrieb sowie die Entladegrenzen nach deinen Bedürfnissen anpassen.

## Haftungsausschluss

Die Nutzung dieses Flows erfolgt auf eigene Gefahr. Der Autor übernimmt keine Haftung für Schäden, die durch die Verwendung entstehen könnten. Teste den Flow gründlich in einer sicheren Umgebung, bevor du ihn produktiv einsetzt.

![image](https://github.com/user-attachments/assets/f9e240ff-8931-41a2-a3eb-6e9b49a15574)


# AC-Ladungs- und Entladungssteuerung für Zendure Solarflow 1200

Dieser Node-RED-Flow steuert sowohl das Laden als auch das Entladen einer Batterie des **Zendure Solarflow 1200** mit Solarstrom. Er wurde entwickelt, um den Akku effizient zu laden, wenn überschüssiger Strom verfügbar ist, und ihn zu entladen, wenn der Energiebedarf im Haushalt steigt oder die Batterie vollständig geladen ist. Zusätzlich beinhaltet der Flow eine SOC-History. Wenn mehr als 3 Tage keine 100% erreicht wurden, wird das System keine Leistung ins Haus abgeben, bis dieser Zustand erreicht ist. 

## Funktionsweise

### 1. **AC-Ladung basierend auf überschüssigem Solarstrom**
- Der Flow prüft regelmäßig den aktuellen Stromverbrauch des Haushalts (wird über den `currentpower`-Wert ermittelt, der von einem Leistungsmesser im Hausanschluss kommt).
- Solange überschüssiger Strom (negative Werte) vorhanden ist, wird die Batterie mit einer variablen Leistung geladen.
- Der Flow versucht, die Batterie nur mit überschüssigem Strom zu laden, um eine Null-Einspeisung zu gewährleisten und keine Energie aus dem Netz zu ziehen.

### 2. **Notladefunktion im Winter**
- Wenn der Akku auf unter 15% fällt, aktiviert der Flow den Notladebetrieb. In diesem Fall wird der Akku auf 20% aufgeladen, um Schäden durch Tiefentladung zu vermeiden.
- Sobald der Akku 20% erreicht, schaltet der Flow in den normalen Betrieb zurück und setzt das Laden nur mit überschüssigem Solarstrom fort.

### 3. **Akku-Entladung**
- Wenn die Batterie vollständig geladen ist und überschüssiger Strom aus dem Haushalt entnommen wird, kann die Batterie entladen werden, um den Strombedarf zu decken.
- Der Flow ermöglicht es, die Batterie zu entladen, wenn kein überschüssiger Solarstrom vorhanden ist, sodass der Haushalt weiterhin mit Strom versorgt wird, ohne auf das Netz zurückgreifen zu müssen.
- Die SOC-History überwacht, ob der Akku mehr als 3 Tage nicht bei 100% war. Da das Zendure System hier realtiv empfindlich ist, habe ich das so eingepflegt. 

### 4. **Parameter und Einstellungen**
- **Aktuelle Leistungswerte (`currentpower`)**: Dieser Wert wird regelmäßig abgerufen, um festzustellen, ob überschüssiger Strom für die Ladung verfügbar ist. Ein positiver Wert bedeutet, dass Strom aus dem Netz bezogen wird, während ein negativer Wert auf überschüssigen Solarstrom hinweist.
- **Akku-Ladegrenzen**: Der Flow überprüft den Akkustand und stellt sicher, dass der Akku im Notfall auf 20% aufgeladen wird, um eine Tiefentladung zu verhindern. Der normale Ladebereich ist zwischen 20% und 100%.
- **Entladegrenzen**: Wenn der Akku vollständig geladen ist, wird er entladen, wenn dies für den Haushalt erforderlich ist.

## Voraussetzungen

- **Node-RED**: Der Flow ist für die Nutzung in Node-RED konzipiert und sollte auf einem Server oder Raspberry Pi installiert sein.
- **Leistungsmesser**: Du benötigst einen Leistungsmesser wie einen Shelly oder Lesekopf mit Tasmota, der den aktuellen Stromverbrauch (`currentpower`) ermittelt.
- **Node-RED Nodes**: Der Flow verwendet verschiedene Nodes wie Switch, Inject und Function, um die Lade- und Entladefunktionen zu steuern.

## Installation

1. Stelle sicher, dass du Node-RED auf deinem System installiert hast.
2. Importiere diesen Flow in dein Node-RED Dashboard.
3. Konfiguriere deinen Leistungsmesser, damit der `currentpower`-Wert korrekt übermittelt wird.
4. Installiere einen Adapter der die Sonnenzeiten bereit stellt im String Format. 

## Anpassungen

- Du kannst die Ladeleistung anpassen, indem du den Wert für den AC-Lader änderst (z.B. 900W).
- Ebenso kannst du die Schwellenwerte für den Notladebetrieb (15% für den Einstieg und 20% für das Ziel) nach deinen Bedürfnissen anpassen.
- Die Entladegrenzen und -logik können ebenfalls angepasst werden, je nach dem gewünschten Verhalten des Systems.

## Haftungsausschluss

Die Verwendung dieses Node-RED-Flows erfolgt auf eigene Gefahr. Der Autor übernimmt keine Haftung für Schäden, die durch die Nutzung dieses Flows entstehen, einschließlich, aber nicht beschränkt auf Schäden an Geräten, Datenverlust oder andere unerwünschte Auswirkungen. Stellen Sie sicher, dass Sie die Funktionsweise des Flows verstehen und testen, bevor Sie ihn in einer produktiven Umgebung einsetzen.

Verwenden Sie den Flow nur, wenn Sie sich der Risiken bewusst sind und geeignete Sicherheitsvorkehrungen getroffen haben.

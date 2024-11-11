![image](https://github.com/user-attachments/assets/d2c6b45e-1a54-4b34-bc73-c1f1eaf27974)

# Node-RED Flow für Batteriemanagement und AC-Ladung/Entladung mit Überschuss-Energie für Solarflow System (HUB1200)

Dieses Repository enthält einen Node-RED-Flow, der das Batteriemanagement in einem Zwndure Solarflowsystem steuert und sicherstellt, dass der Akku mit überschüssiger Solarenergie geladen wird, ohne Netzstrom zu beziehen. Der Flow verhindert Überladung und sorgt für eine effiziente Nutzung der verfügbaren Solarenergie. Ebenso steuert er die Entladung des Akkus. Beides wird gesteuert nach den Sonnenauf oder Untergangszeiten. Da ich parralel zum dem BKW noch eine große PV-Anlage betreibe, passt dies gut zu meinem Usecase. 

## Funktionen

- **Batteriemanagement**: Der Flow überwacht den Ladezustand der Batterie und steuert das Laden oder Entladen basierend auf festgelegten Schwellenwerten.
- **AC-Lade-Management**: Das AC-Lademodul (ACE1500) wird nur dann aktiviert, wenn überschüssige Solarenergie zur Verfügung steht und keine Energie aus dem Netz bezogen wird.
- **Entlade-Management**: Die Entladung des Akkus wird anhand des aktuellen Hausverbrauchs berechnet. Das es sich um ein BKW handelt, ist die maximale Abgabe auf 800 Watt begrenzt. Alle Werte lassen sich aber über variablen anpassen. 
- **Notfall-Ladung**: Wenn der Ladezustand der Batterie unter 15% fällt, wird automatisch der Notfallmodus aktiviert, um die Batterie auf 20% aufzuladen.
- **Überschuss-Erkennung**: Der Flow erkennt überschüssige Solarenergie und lädt die Batterie nur dann, wenn der **Leistungswert (`msg.payload.currentpower`)** aus dem Netz negativ ist, also mehr Energie produziert wird als verbraucht. Eine Schwelle (z.B. -100W) muss definiert werden, damit das System nur mit Solarenergie lädt.

## Systemanforderungen

- **Node-RED**: Ein Tool zur Programmierung von Abläufen, das verschiedene Geräte, APIs und Online-Dienste miteinander verbindet.
- **ioBroker**: Ein Hausautomationssystem, das mit deinem Solarsystem und dem Batterie-Management interagiert.
- **Zendure**: Ein Batteriesystem, das Überladung verhindert und den Akku in gutem Zustand hält.
- **Leistungsmesser**: Benötigt wird ein Lesitungsmesser im Hausanschluss. Zum Beispiel Tasmota Lesekopf oder Shelly. Dieser muss einen Leistungswert ausgeben der positiv oder negativ ist, je nachdem ob man einspeißt oder bezieht. 

## Ablauf des Flows

1. **Überwachung des Akkus**:
   - Der Flow überwacht kontinuierlich den Ladezustand der Batterie und schaltet die Ladeleistung je nach Zustand ein oder aus.
   - Bei einem Ladezustand unter 20% wird der **Notfallmodus** aktiviert, und der Akku wird auf 20% aufgeladen.
   - Wenn der Akku mehr als 20% hat, stoppt der Flow das Laden, um Überladung zu verhindern.

2. **Erkennung von Überschuss-Energie**:
   - Der Flow prüft den **Leistungswert** (`msg.payload.currentpower`) von einem Leistungsmesser (z.B. Shelly oder Tasmota).
   - Wenn der Wert negativ ist (mehr Solarenergie wird produziert als verbraucht), beginnt der Flow mit dem Laden der Batterie.
   - Ist der Wert positiv (Strom wird aus dem Netz bezogen), wird das Laden gestoppt oder reduziert.

3. **Lade-Logik**:
   - Das AC-Lademodul wird nur aktiviert, wenn ein Überschuss an Solarenergie vorhanden ist, und das Netz wird nicht beansprucht. Der Flow lädt die Batterie mit einer variablen Leistung, wenn der Stromüberschuss den definierten Schwellenwert überschreitet.
   - Wenn der Leistungswert positiv wird (d.h., Netzstrom fließt), wird das Laden gestoppt, um eine Einspeisung in das Netz zu vermeiden.

4. **Wintermodus**:
   - Bei längeren Perioden ohne Sonne (z.B. in den Wintermonaten) sorgt der Flow dafür, dass die Batterie bei einem Ladezustand unter 15% auf 20% aufgeladen wird, ohne überladen zu werden.
   - Sobald der Akku 20% erreicht, wird das Laden gestoppt, bis wieder überschüssige Solarenergie zur Verfügung steht.

## Installation

1. Installiere Node-RED auf deinem Server.
2. Richte die notwendigen Abhängigkeiten für die Verbindung zu ioBroker und deinem Batterie-Management-System (Zendure) ein.
3. Importiere den bereitgestellten Node-RED-Flow in dein Node-RED-System.
4. Konfiguriere den Flow mit deinen spezifischen Parametern wie den Schwellenwerten für den Akkustand, den Ladeleistungen und den Solarwerten.
5. Definiere einen Überschusswert (z.B. -100W) für den Leistungswert (`msg.payload.currentpower`), damit das System nur mit überschüssiger Solarenergie lädt.

## Zukünftige Verbesserungen

- **Dynamische Anpassung der Ladeparameter**: Erweitere die Logik, um Ladeparameter basierend auf Wettervorhersagen oder Netzbedingungen dynamisch anzupassen.
- **Erweiterte Energieverwaltung**: Integriere zusätzliche Energiequellen wie Wind- oder Netzstrom für eine bessere Effizienz und Lastenverteilung.
- **Mobile Benachrichtigungen**: Sende Benachrichtigungen, wenn kritische Akkustände erreicht werden oder der Flow in den Notfallmodus wechselt.

## Mitwirken

Forke das Repository, nehme Änderungen vor und erstelle Pull Requests! Alle Verbesserungen, Bugfixes oder neue Funktionen sind willkommen.

## Lizenz

Dieses Projekt ist unter der MIT-Lizenz lizenziert – siehe die [LICENSE](LICENSE)-Datei für Details.

---

Bei Fragen oder Problemen öffne bitte ein Issue oder kontaktiere mich direkt!

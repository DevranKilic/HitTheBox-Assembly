Dieses Projekt wurde im Rahmen des Praktikums "Grundlagen von Hardwaresystemen" erstellt.

# Hit The Box

Dieses Projekt implementiert ein einfaches Spiel für eine 16-Bit RISC-Prozessorarchitektur im Rahmen eines Praktikums. Ziel ist es, ein steuerbares Sprite mit Hardwaretastern über den Bildschirm zu bewegen und eine zufällig platzierte Hitbox zu treffen. Die Umsetzung erfolgt vollständig in Assemblersprache innerhalb der Entwicklungsumgebung TINY RISC STUDIO v1.3.

## Spielprinzip und Ablauf

Das Programm basiert auf einer einfachen Zustandsmaschine mit drei Zuständen:

### Zustand 00 – Initialisierung

- VGA-Ausgabe wird auf indirekte Adressierung gesetzt.
- Ein Sprite mit 8x8 Pixeln wird gezeichnet.
- Die Hitbox (10x10 Pixel) wird in Farbe gesetzt.
- Sprite und Hitbox werden zufällig auf dem Bildschirm platziert.
- Nach Abschluss aller Initialisierungen erfolgt ein Übergang in Zustand 01.

### Zustand 01 – Kollisionserkennung

- Es wird überprüft, ob das Sprite (R17, R18) die Hitbox (R19, R20) überdeckt.
- Wenn ja:
  - Der Bildschirm wird gelöscht.
  - Eine neue Hitbox-Position wird per Zufallsgenerator bestimmt.
  - Die Hitbox wird neu gezeichnet.
- Danach Übergang in Zustand 10.

### Zustand 10 – Tastersteuerung

- Die Hardwaretaster werden regelmäßig abgefragt (BT_0 bis BT_3).
- Bei Tastendruck wird das Sprite in die entsprechende Richtung um 1 Pixel verschoben.
- Danach folgt wieder Zustand 01.
- Ohne Eingabe bleibt das Programm in Zustand 10.

## Implementierte Funktionen

- **Sprite-Konfiguration:**  
  Das Sprite wird zeilenweise über 8-Bit-Werte gesetzt. Die Position wird über zwei spezielle Register (X- und Y-Koordinaten) bestimmt.

- **Hitbox-Zeichnung:**  
  Pixel der Hitbox werden im VGA-Speicher manuell gesetzt. Die Position basiert auf Zufallszahlen.

- **Zufallszahlengenerator:**  
  Implementiert als lineares rückgekoppeltes Schieberegister (LFSR) zur Berechnung zufälliger X- und Y-Werte im gültigen Bereich.

- **Eingabeverarbeitung:**  
  Die Abfrage der Taster erfolgt über Polling. Es wird nur jeweils ein Taster gleichzeitig ausgewertet.

- **Zustandssteuerung:**  
  Die Hauptlogik des Spiels ist über Sprungmarken realisiert. Alle Zustände sind durch Labels im Code strukturiert.

## Code-Übersicht

- `STATE_00`: Initialisiert VGA-Modus, Sprite und Hitbox
- `REAL_STATE_01`: Kollisionsabfrage (Sprite)
- `STATE_10`: Liest Tasteneingaben aus und verschiebt das Sprite
- `Generate_X, Generate_Y`: Zufallszahlengenerator für Hitbox-Position
- `Generate_Quadrat`: Funktion zum Zeichnen der Hitbox an einer Zielkoordinate
- `Flush`: Bildschirm löschen
- `Sprite_einfugen`: Sprite in VGA-Speicher schreiben

## Ausführung

1. Assembler-Code in **TINY RISC STUDIO v1.3** laden.
2. Emulation starten
3. Sprite mit den Buttons steuern
4. Bei Treffer auf die Hitbox wird diese neu positioniert und Spiel läuft weiter.

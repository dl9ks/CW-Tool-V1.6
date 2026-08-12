# 📻 CW-Tool V1.6 (MEGA Edition)
**Ein Freeware-Projekt für die Amateurfunk-Gemeinschaft (Hamspirit)**  
*(C) 2026 de DL9KS*

Dieses leistungsstarke und autarke CW Tool basiert auf dem **Arduino Mega 2560** und dient als universelles Werkzeug im Shack oder für den Portabelbetrieb. Es bietet eine automatische Dekodierung von Handtasten, Paddles sowie echten Funksignalen und beinhaltet einen flexiblen Morse-Trainer.

**WICHTIGER HINWEIS ZUM SCHUTZ DES PROJEKTS:**  
Um zu verhindern, dass dieses für die Community entwickelte Projekt von Dritten kommerziell verwertet, verändert oder unter falschem Namen verbreitet wird, bleibt der Quellcode unter Verschluss. Das Programm wird als fertig kompilierte Firmware (**`.hex`-Datei**) absolut kostenlos zur privaten Nutzung zur Verfügung gestellt. Jede kommerzielle Nutzung oder der unautorisierte Verkauf ist strengstens untersagt!

---

## 🎯 Hauptmerkmale
* **3-in-1 Betriebsmodus:** Unterstützt manuelle Handtasten, automatische Iambic-Paddles (integrierter Keyer) und einen digitalen Audio-Decoder.
* **Integrierter CW-Trainer:** Generiert zufällige 5er-Gruppen (**Buchstaben**, **Zahlen**, **Gemischt** oder **Sonderzeichen**) im Paris-Standard mit komfortbarer WPM-Vorauswahl und live regelbarem Tempo über das Drehrad.
* **Optimierte Einzeilen-Laufschrift:** Alle empfangenen, gegebenen oder gelernten Zeichen laufen einheitlich und flugschonend als Laufschrift durch die 3. Zeile des Displays.
* **Ergonomische Menüführung:** Schnelle Auswahl aller Modi über einen Drehencoder. Ein kurzer Druck (1,5 Sek.) bringt dich im laufenden Betrieb sofort zurück ins Hauptmenü.
* **Intelligenter Stromsparmodus:** Nach 5 Minuten Inaktivität schaltet das Gerät das LCD-Display komplett aus und geht in den Tiefschlaf. Während des Betriebs sind störende Interrupts entkoppelt. Erst im Schlaf werden die Pfade scharfgeschaltet: Das Gerät wacht verlustfrei per Encoder-Druck, Handtastendruck oder durch ein eintreffendes Funksignal in 1 ms auf.
* **Software-Reset:** Bequemes Neustarten des gesamten Boards inklusive vollem Logorun direkt über den letzten Menüpunkt ("Reset").

---

## 🛠️ Benötigte Bauteile (BOM)
1. **Mikrocontroller:** 1x Arduino Mega 2560
2. **Display:** 1x W164B-NLW (16x4 Zeichen reines LCD-Display mit I2C Modul HLF8574T)
3. **Bedienelement:** 1x Drehencoder-Modul mit Drucktaster (SW)
4. **Audio-Decoder:** 1x NE567- oder LM567-Tondecoder-Modul (Frequenzselektiver PLL-Filter)
5. **Mithörton:** 1x Passiver Piezo-Summer (Buzzer)
6. **Morsetaste:** Handtaste (z. B. Junkers) und/oder Iambic-Paddle

---

## 🔌 Anschluss- und Schaltplan (Arduino Mega 2560)

| Bauteil | Pin-Beschriftung | **Pin am Mega 2560** | Hinweis |
| :--- | :--- | :--- | :--- |
| **W164B Display** | VCC | **5V** | Stromversorgung Display |
| | GND | **GND** | Masse Display |
| | SDA | **20 (SDA)** | Festgelegte I2C Datenleitung |
| | SCL | **21 (SCL)** | Festgelegte I2C Taktleitung |
| **Drehencoder** | SW (Taster) | **2 (PWM 2)** | Echter Hardware-Interrupt 0 (Aufwachen) |
| | CLK (A-Leitung)| **11 (PWM 11)** | Interruptfreier Navigations-Pin |
| | DT (B-Leitung) | **12 (PWM 12)** | Interruptfreier Navigations-Pin |
| | VCC / + | **5V / 3.3V** | Versorgung der Modul-Widerstände |
| | GND / - | **GND** | Masse Encoder |
| **Morsetaste** | Dash (Strich) | **7 (PWM 7)** | Nur für Paddles benötigt |
| | Dot / Junkers | **8 (PWM 8)** | Signalleitung (Aufwachen & Geben) |
| | Gemeinsame Masse | **GND** | Masse für die Tasterkontakte |
| **Audio-Decoder** | DO / OUT | **18 (TX1)** | Hardware-Interrupt 5 (NE567 Signal & Aufwachen) |
| **Buzzer** | Plus (+) | **9 (PWM 9)** | PWM-Mithörton (700 Hz) |
| | Minus (-) | **GND** | Masse Summer |

---

## 💾 Anleitung: Firmware auf den Arduino Mega laden

Es wird keine Arduino IDE benötigt, um das CW-Tool einsatzbereit zu machen. Die fertige Firmware lässt sich in wenigen Sekunden direkt auf das Board flashen:

1. Lade dir die Datei **`CW-Tool-V1.6.ino.hex`** von dieser GitHub-Seite herunter.
2. Lade dir das kostenlose Windows-Tool **XLoader** aus dem Internet herunter (portabel, keine Installation notwendig).
3. Verbinde deinen Arduino Mega 2560 per USB-Kabel mit dem PC.
4. Öffne den **XLoader**:
   * Wähle bei **Hex file** deine heruntergeladene `.hex`-Datei aus.
   * Stelle bei **Device** unbedingt `Mega(ATmega2560)` ein.
   * Wähle bei **COM Port** den passenden Anschluss deines Arduinos aus.
   * Belasse die **Baudrate** auf dem Standardwert (`115200`).
5. Klicke auf **Upload**. Nach wenigen Sekunden blinken die TX/RX-LEDs auf dem Board und im XLoader steht *"Upload abgeschlossen"*. Das CW-Tool startet sofort!

---

## 🚀 Bedienungsanleitung

### 1. Der Systemstart
Beim Einschalten erscheint für 5 Sekunden der Bootscreen mit dem Rufzeichen `CW-Tool by DL9KS`. Danach lädt das Hauptmenü. Drehe den Encoder, um den Pfeil zu bewegen, und drücke auf die Achse, um zu bestätigen.

### 2. Modus: Handtaste
Schließe deine Handtaste an Pin 8 und GND an. Der Arduino misst dein Gebertempo adaptiv (Paris-Standard) und zeigt dir das errechnete Tempo oben rechts in **WPM** an. Der Mithörton (700 Hz) läuft über den Summer mit.

### 3. Modus: Paddle (Keyer)
Schließe die Punkt-Ader an Pin 8 und die Strich-Ader an Pin 7 an (Masse auf GND). Der Arduino übernimmt das exakte 1:3 Timing elektronisch von selbst. Das Standardtempo ist auf 16 WPM voreingestellt.

### 4. Modus: CW-Trainer
Wähle im Untermenü die gewünschte Lerngruppe (**Buchstaben**, **Zahlen**, **Gemischt** oder **Sonderzeichen**).
* Danach öffnet sich der **WPM-Auswahlbildschirm**. Stelle dein gewünschtes Starttempo ein und drücke den Encoder zum Starten.
* Der Trainer spielt ein Zeichen ab. In Zeile 2 siehst du kurz das Signalmuster (z. B. `.-.`).
* Nach einer ultrakurzen Pause (150 ms) wird das Zeichen in der Laufschriftzeile (Zeile 3) aufgedeckt.
* Das Tempo lässt sich auch während der Übung live über das Drehrad verändern. Die Zeichen werden automatisch in **5er-Gruppen** unterteilt.

### 5. Modus: RX: CW Audio
Verbinde den Ausgang (OUT) deines abgeglichenen NE567-Moduls mit Pin 18. Schließe das Funkgerät oder den PC an den Modul-Eingang an. Der Arduino wertet die eingehenden Digitalsignale aus und errechnet die WPM-Geschwindigkeit der Gegenstation vollautomatisch.

### 6. Zurück ins Menü / Reset
* **Menü:** Halte die Achse des Drehencoders im Betrieb für **1,5 Sekunden gedrückt**, um zurück ins Hauptmenü zu springen.
* **Reset:** Wähle im Hauptmenü den Punkt **`> Reset`**, um einen echten Hardware-Kaltstart (Reboot) des Mega 2560 via Watchdog auszulösen.

---
*Viel Spaß beim Nachbauen, Üben und Dekodieren! Vy 73 de DL9KS*

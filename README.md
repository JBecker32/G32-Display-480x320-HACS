Absolut! Hier ist ein umfassender README.md-Entwurf, der auf der von Ihnen bereitgestellten YAML-Datei basiert. Er ist für die Verwendung auf GitHub strukturiert und enthält Erklärungen zu Konfiguration, Hardware und Funktionen.

---

# ESPHome G32 Grill Monitor Display

Dieses ESPHome-Projekt verwandelt ein ESP32-S3-Touchscreen-Display in einen hochentwickelten Monitor für Ihren Otto Wilde G32 Grill. Es verbindet sich direkt über Bluetooth (BLE) mit dem Grill, um Echtzeitdaten abzurufen und bietet eine reichhaltige, über LVGL erstellte Benutzeroberfläche zur Visualisierung aller wichtigen Grillinformationen.

Die Benutzeroberfläche ist in zwei Hauptansichten unterteilt, zwischen denen durch Wischen (Swipen) gewechselt werden kann:
1.  **Arc-Ansicht:** Eine farbenfrohe, grafische Darstellung der Temperaturen mit kreisförmigen Bögen.
2.  **Zahlen-Ansicht:** Eine schlichte, zahlenbasierte Ansicht, die sich auf die wichtigsten Werte konzentriert.


## ✨ Features

* **Echtzeit-Überwachung:** Zeigt Temperaturen für bis zu 4 Grillzonen und 4 externe Temperatursensoren an.
* **Gas-Level-Anzeige:** Überwacht den Füllstand des Gasbuddy in Prozent und Gramm, inklusive einer visuellen Leiste.
* **Temperaturalarme:** Setzen Sie individuelle Temperaturlimits für jede Zone und jeden Sensor. Bei Überschreitung wird ein akustischer Alarm über den integrierten Beeper ausgelöst.
* **Zwei Display-Modi:** Wechseln Sie durch Wischen zwischen einer grafischen "Arc"-Ansicht und einer minimalistischen "Zahlen"-Ansicht.
* **Status-Symbole:** Icons auf dem Display zeigen den Verbindungsstatus von WLAN und Bluetooth sowie den Zustand von Grillhaube und Licht an.
* **Home Assistant Integration:** Nahtlose Einbindung in Home Assistant über die ESPHome API.
* **Umfassend anpassbar:** Passen Sie Farben, Texte, Anzeigeelemente und Schwellenwerte direkt in der YAML-Datei an Ihre Wünsche an.
* **Batterieüberwachung (Optional):** Zeigt den Ladezustand (SOC) eines optional angeschlossenen Akkus an.
* **Fallback-WLAN:** Erstellt einen eigenen Access Point, falls die Verbindung zum primären WLAN fehlschlägt.

## 🛠️ Hardware-Voraussetzungen

Dieses Projekt ist für eine bestimmte Kombination aus Mikrocontroller und Display konzipiert.

* **Mikrocontroller:** Ein **ESP32-S3** Board (z.B. ESP32-S3-DevKitC-1). Das Projekt nutzt PSRAM, stellen Sie also sicher, dass Ihr Board damit ausgestattet ist.
* **Display:** Ein **480x320 Pixel** LCD-Display mit einem **AXS15231** Controller und einer **QSPI**-Schnittstelle.
* **Touchscreen:** Ein kapazitiver Touchscreen-Controller, der über **I2C** angebunden wird (z.B. GT911, kompatibel mit dem AXS15231).
* **Beeper/Lautsprecher:** Ein kleiner Lautsprecher zur Ausgabe von Alarmtönen.
* **(Optional) Akku:** Ein LiPo/Li-Ion-Akku zur mobilen Stromversorgung, dessen Spannung über einen Spannungsteiler an einen ADC-Pin geführt wird.

### Pinbelegung

Stellen Sie sicher, dass Ihre Hardware gemäß den folgenden Pins aus der Konfiguration verbunden ist:

| Komponente | Pin-Name (Funktion) | ESP32-S3 Pin |
| :--- | :--- | :--- |
| **QSPI Display** | `clk_pin` | 47 |
| | `data_pins` | [21, 48, 40, 39] |
| | `cs_pin` | 45 |
| **I2C Touch** | `sda` | 4 |
| | `scl` | 8 |
| **Backlight** | `gpio_backlight_pwm`| 1 |
| **Beeper** | `rtttl_out` | 9 |
| **Batterie-ADC** | `adc` | 5 |

## ⚙️ Installation & Konfiguration

### Voraussetzungen
1. Eine laufende **Home Assistant** Instanz.
2. **ESPHome** als Add-on in Home Assistant oder als eigenständige Installation.

### Installationsschritte

1.  **Neues Gerät in ESPHome erstellen:**
    * Gehen Sie zum ESPHome Dashboard.
    * Klicken Sie auf "+ NEW DEVICE" und folgen Sie den Anweisungen. Dies ist notwendig, um einen `api_encryption_key` und ein `ota_password` zu generieren. Notieren Sie sich diese Werte.

2.  **YAML-Datei konfigurieren:**
    * Laden Sie die Datei `G32_JC3248W535_ESPHome_V110d_both_BT.YAML` herunter und kopieren Sie sie in Ihr `/esphome`-Verzeichnis.
    * Öffnen Sie die Datei und passen Sie den oberen `substitutions`-Abschnitt an Ihre Bedürfnisse an. Dies ist der einzige Bereich, den Sie bearbeiten müssen.

3.  **Firmware kompilieren und hochladen:**
    * **Erstes Flashen:** Verbinden Sie Ihr ESP32-S3-Board über USB mit dem Computer, auf dem ESPHome läuft. Klicken Sie im ESPHome-Dashboard auf "Install" und wählen Sie die USB-Option.
    * **Zukünftige Updates:** Sobald das Gerät im Netzwerk ist, können Sie Updates drahtlos (OTA) durchführen, indem Sie auf "Install" und "Wirelessly" klicken.

### Benutzerkonfiguration (`substitutions`)

Passen Sie die folgenden Variablen in der YAML-Datei an:

| Variable | Beispielwert | Beschreibung |
| :--- | :--- | :--- |
| `name` | `g32-display-bt` | Der Name Ihres Geräts, wie er in ESPHome und Home Assistant angezeigt wird. Keine Leer- oder Sonderzeichen. |
| `g32_mac_address` | `94:E6:86:0A:27:52`| **WICHTIG:** Die Bluetooth-MAC-Adresse Ihres G32 Grills. (Siehe Hinweis unten) |
| `wifi_ssid` | `!secret wifi_ssid` | Der Name (SSID) Ihres WLAN-Netzwerks. |
| `wifi_password` | `!secret wifi_password` | Das Passwort für Ihr WLAN-Netzwerk. |
| `api_encryption_key` | `"E1f...U6k="` | Der Verschlüsselungsschlüssel aus dem in Schritt 1 erstellten ESPHome-Gerät. |
| `ota_password` | `"398...701a"` | Das Passwort für drahtlose Updates aus dem in Schritt 1 erstellten ESPHome-Gerät. |
| `ap_ssid` | `"G32 Display..."`| Der Name des Fallback-WLAN-Hotspots. |
| `ap_password` | `"4Lyt...DNh"` | Das Passwort für den Fallback-Hotspot. |
| `Zone_Text` | `"Zone "` | Der Präfix-Text für die Grillzonen (ergibt "Zone 1", "Zone 2" usw.). |
| `Sensor_Text` | `"Sensor "` | Der Präfix-Text für die externen Sensoren. |
| `Zone_Arc_Color` | `"0xff0000"` | Farbe (Hex-Code) der Temperatur-Bögen für die Zonen. |
| `Sensor_Arc_Color`| `"0xffff00"` | Farbe (Hex-Code) der Temperatur-Bögen für die Sensoren. |
| `Hide_Battery_Symbol`| `'false'` | Setzen Sie auf `'true'`, wenn Sie kein Akku-Symbol anzeigen möchten. |
| `Min_SOC` | `"25"` | Der Akku-Prozentsatz, unter dem das Symbol rot angezeigt wird. |
| `Hide_Inactive_Sensors`| `'false'` | Setzen Sie auf `'true'`, um inaktive Sensoren komplett auszublenden. |
| `Enable_Temperature_Limits`| `'true'` | Setzen Sie auf `'false'`, um die Funktion zum Setzen von Temperaturlimits zu deaktivieren. |
| `Sensor1_Max` | `"130"` | Maximale einstellbare Temperatur für Sensor 1 (z.B. für Kerntemperatur). |
| `Beeper_interval`| `"5s"` | Das Zeitintervall für Alarmmeldungen des Lautsprechers. |
| `Show_Time` | `'true'` | Setzen Sie auf `'true'`, um die Uhrzeit (von Home Assistant synchronisiert) anzuzeigen. |
| `Show_Arcs_Page` | `'true'` | Legt fest, ob die grafische "Arc"-Seite angezeigt werden soll. |
| `Show_Numbers_Page`| `'true'` | Legt fest, ob die "Zahlen"-Seite angezeigt werden soll. |

> **💡 So finden Sie die MAC-Adresse Ihres Grills:**
> Wenn Sie die MAC-Adresse Ihres G32 nicht kennen, können Sie sie mit diesem Projekt selbst finden.
> 1.  Entfernen Sie die Kommentarzeichen (`#`) vor den Zeilen des `esp32_ble_tracker:` in der YAML-Datei.
> 2.  Installieren Sie die Firmware auf Ihrem ESP32.
> 3.  Öffnen Sie die "Logs" für das Gerät im ESPHome-Dashboard.
> 4.  Schalten Sie Ihren G32 Grill ein. In den Logs sollte eine Meldung erscheinen, die mit "G32 gefunden" beginnt und die MAC-Adresse anzeigt.
> 5.  Kopieren Sie diese Adresse in das Feld `g32_mac_address`.
> 6.  Kommentieren Sie den `esp32_ble_tracker:`-Block wieder aus, um Ressourcen zu sparen.

## 🕹️ Bedienung

* **Seiten wechseln:** Wischen Sie auf dem Display nach links oder rechts, um zwischen der "Arc"-Ansicht und der "Zahlen"-Ansicht zu wechseln (sofern beide aktiviert sind).
* **Temperaturlimit setzen:** Tippen Sie auf eine Temperaturanzeige (entweder in der Arc- oder der Zahlen-Ansicht). Es öffnet sich eine neue Seite, auf der Sie mit den Tasten `+` / `-` oder dem Schieberegler ein Temperaturlimit festlegen können. Bestätigen Sie mit "OK".
* **Bildschirm aufwecken:** Wenn der Bildschirm nach der eingestellten Zeit in den Ruhezustand wechselt (Bildschirm aus), tippen Sie einfach einmal auf das Display, um ihn wieder zu aktivieren.

# ESPHome G32 Grill-Display (owg-g32-ha-jc3248w535-arcs-numbers)

Diese ESPHome-Konfiguration verwandelt ein ESP32-basiertes Display in eine **interaktive Überwachungseinheit für Ihren OW G32 Grill**. Sie bietet Echtzeit-Temperaturanzeigen für Zonen und Sensoren, Gasfüllstandsanzeigen, anpassbare Alarme und eine benutzerfreundliche Oberfläche.

-----

## Funktionen

  * **Anpassbare Bezeichnungen**: Legen Sie eigene Namen für Zonen und Sensoren fest (z.B. "Zone 1", "Sensor 1").
  * **Farbkodierte Temperaturanzeigen**: Visualisieren Sie Temperaturen und Grenzwerte mit anpassbaren Farben für die Kreisbögen und Texte.
  * **Gasfüllstandsanzeige**: Überwachen Sie den Gasfüllstand mit einem farbkodierten Balken, der bei niedrigem Stand die Farbe ändert.
  * **Temperaturgrenzwerte**: Setzen und verwalten Sie individuelle Temperaturgrenzwerte für jede Zone und jeden Sensor direkt auf dem Display.
  * **Akku-Überwachung**: Das Batteriesymbol wird angezeigt, wenn der Ladezustand unter einen konfigurierbaren Wert fällt.
  * **Ausblenden inaktiver Sensoren**: Option zum Ausblenden von Sensoren, die keine Daten liefern.
  * **Akustische Alarme**: Unterschiedliche Sirenengeräusche warnen Sie, wenn Zonen- oder Sensortemperaturgrenzwerte überschritten werden.
  * **Einstellbarer Bildschirm-Timeout**: Konfigurieren Sie, wann das Display in den Ruhezustand wechseln soll, um Energie zu sparen.
  * **Uhrzeitanzeige**: Optionale Anzeige der aktuellen Uhrzeit in der Statusleiste.
  * **WLAN-Konnektivität**: Verbindet sich mit Ihrem Heim-WLAN und bietet einen Fallback-Hotspot, falls die Verbindung fehlschlägt.
  * **Over-The-Air (OTA) Updates**: Ermöglicht drahtlose Firmware-Updates.

-----

## Hardware

Diese Konfiguration ist für ein Display mit folgenden Komponenten optimiert:

  * **Guition JC3248W553C Display Einheit**
  * **ESP32-S3-DevKitC-1**
  * **QSPI-DBI Display** (Modell: axs15231) mit 480x320 Pixeln
  * **GT911 Touchscreen**

-----

## Erste Schritte

1.  **Klonen Sie dieses Repository:**

    ```bash
    git clone https://de.wikipedia.org/wiki/Repository
    cd [Name des Repositories]
    ```

2.  **Passen Sie die Konfiguration an:**
    Öffnen Sie die `substitutions:` Sektion in der `.yaml` Datei. Hier können Sie die Geräteeinstellungen, Wi-Fi-Anmeldeinformationen, API-Schlüssel und OTA-Passwörter anpassen.

    ```yaml
    # Name des ESPHome Geräts
    name: g32-display
    device_description: "Monitor an OW G32 grill"
    g32_nickname: "elgrillo"

    # WLAN-Anmeldeinformationen (verwenden Sie secrets.yaml für die Produktion)
    wifi_ssid: !secret wifi_ssid
    wifi_password: !secret wifi_password

    # Home Assistant API-Verschlüsselungsschlüssel
    api_encryption_key: "IHR_HOME_ASSISTANT_API_KEY_HIER"

    # OTA-Passwort
    ota_password: "IHR_OTA_PASSWORT_HIER"

    # WLAN-Fallback-Hotspot
    ap_ssid: "G32 Display Fallback Hotspot"
    ap_password: "IHR_AP_PASSWORT_HIER"

    # ... weitere Anpassungen wie Texte, Farben, Sensormaximalwerte
    ```

    **Wichtig**: Es wird dringend empfohlen, sensible Daten wie Wi-Fi-Passwörter und API-Schlüssel in einer `secrets.yaml`-Datei zu speichern, die sich im selben Verzeichnis wie Ihre ESPHome-Konfiguration befindet und nicht im Git-Repository veröffentlicht wird.

3.  **Installieren Sie ESPHome:**
    Wenn Sie ESPHome noch nicht installiert haben, folgen Sie der offiziellen Anleitung: [ESPHome Installation](https://www.google.com/search?q=https://esphome.io/guides/getting_started_yaml.html%23installing-esphome)

4.  **Kompilieren und Flashen:**
    Verwenden Sie die ESPHome CLI, um die Firmware zu kompilieren und auf Ihr Gerät hochzuladen:

    ```bash
    esphome run g32-display.yaml
    ```

    Ersetzen Sie `g32-display.yaml` durch den tatsächlichen Namen Ihrer ESPHome-Konfigurationsdatei.

-----

## Anpassung

Die Konfiguration bietet zahlreiche Optionen zur **Personalisierung des Displays**:

  * **`Zone_Text`** und **`Sensor_Text`**: Ändern Sie die Präfixe für Ihre Zonen- und Sensorbezeichnungen. (nur noch im Limits Screen wirksam!)
  * **Farbdefinitionen**: Passen Sie die Farben der Temperatur- und Gasbalken an. Die Farben werden im Hexadezimalformat `0xRRGGBB` angegeben.
  * **`Min_SOC`**: Legen Sie den Prozentsatz fest, unter dem das Batteriesymbol angezeigt wird.
  * **`Hide_Inactive_Sensors`**: Schalten Sie dies auf `true`, um Sensoren auszublenden, die keine gültigen Messwerte liefern.
  * **`Enable_Temperature_Limits`**: Deaktivieren Sie dies, wenn Sie die Möglichkeit, Grenzwerte auf dem Display zu setzen, nicht benötigen.
  * **`SensorX_Max`**: Passen Sie die maximalen Werte für die Sensortemperaturkreise an, z.B. 130°C für Kerntemperaturen oder 300°C für Garraumtemperaturen.
  * **`Beeper_Max`** und **`Beeper_interval`**: Stellen Sie die Lautstärke und das Alarmintervall des Beeper ein.
  * **`Show_Time`**: Aktivieren oder deaktivieren Sie die Anzeige der Uhrzeit in der Statusleiste.

-----

## Fehlerbehebung

  * **WLAN-Verbindungsprobleme**: Überprüfen Sie Ihre `wifi_ssid` und `wifi_password` in der Konfiguration oder in Ihrer `secrets.yaml`. Stellen Sie sicher, dass Ihr Gerät in Reichweite des WLAN-Netzwerks ist.
  * **Display bleibt leer**: Überprüfen Sie die Verkabelung Ihres Displays und Touchscreens. Stellen Sie sicher, dass die `spi` und `i2c` Einstellungen korrekt sind.
  * **Sensoren zeigen "---" an**: Dies deutet darauf hin, dass der Sensor keine gültigen Daten liefert (Wert \> 1500). Überprüfen Sie die Sensorverkabelung oder ob der Sensor korrekt erkannt wird.
  * **Keine Alarme**: Stellen Sie sicher, dass der `Beeper_Max`-Wert über 0 liegt und dass die Temperaturgrenzwerte korrekt gesetzt sind und überschritten werden.

-----

## Mitwirken

**Beiträge, Problemberichte und Feature-Anfragen sind willkommen\!** Bitte öffnen Sie ein Issue oder reichen Sie einen Pull Request ein.

-----

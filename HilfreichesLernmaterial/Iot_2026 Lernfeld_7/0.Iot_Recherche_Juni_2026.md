# **Internet of Things (IoT) 2026 – Praxisorientiertes Kompaktbriefing für IT-Umschüler**

*Fokussiert auf Prüfungsrelevanz, sofortige Umsetzbarkeit und IHK-konforme Inhalte*
**Stand: Mai 2026 | Zielgruppe: FIAE/FISI-Umschüler | Format: Wiki-optimiert**

---

## **📌 Warum dieses Dokument?**

Dieses Dokument ist **kein Theorie-Monolog**, sondern ein **praxistaugliches Werkzeug** für Umschüler:
✅ **Direkt umsetzbares Projekt** (Abschnitt 6) – von der Hardware zum lauffähigen System
✅ **IHK-konform** – deckt Lernziele aus dem Rahmenlehrplan ab
✅ **Quellen mit Free Access** – keine Paywall-Links, nur öffentlich zugängliche Ressourcen
✅ **Karriereboost** – zeigt Skills, die Arbeitgeber suchen

---

## **1. Was ist IoT? – Die Basics für den Einstieg**

*(Für Umschüler ohne Vorwissen – mit IHK-Lernzielen und Alltagsbezug)*

### **🔍 Definition: Einfach erklärt**

Das **Internet der Dinge (IoT)** verbindet **physische Objekte** (Sensoren, Geräte, Maschinen) über das Internet, um:

1. **Daten zu sammeln** (z. B. Temperatur, Vibration, Luftfeuchtigkeit)
2. **Daten auszuwerten** (z. B. „Die Maschine vibriert zu stark“)
3. **Aktionen auszulösen** (z. B. Warnmeldung, Ersatzteilbestellung)

**Analogie für den Einstieg:**
Stell dir IoT wie einen **aufmerksamen Hausmeister** vor:

- Er prüft **jeden Morgen** die Temperatur in der Fabrikhalle (Sensor).
- Er **erinnert dich**, wenn die Heizung kaputt ist (Datenauswertung).
- Er **bestellt automatisch** Ersatzteile (Aktion).
→ **Genau das macht IoT – nur mit Maschinen statt Menschen.**

---

### **🏠 Wo findest du IoT im Alltag?**

*(Konkrete Beispiele mit IT-Bezug für Systemintegratoren & Entwickler)*

| **Bereich**          | **Beispiel**                     | **IT-Rolle**                                                                 | **Technologien**                     |
|-----------------------|----------------------------------|------------------------------------------------------------------------------|--------------------------------------|
| **Smart Home**        | Sprachassistenten (Alexa), smarte Steckdosen (TP-Link) | **Integration** (z. B. Alexa mit Home Assistant verbinden), **Sicherheit** (Firewalls gegen Hacking) | WiFi, Zigbee, Matter-Protokoll       |
| **Industrie 4.0**     | Predictive Maintenance (Maschinenüberwachung) | **Datenverarbeitung** (MQTT → Cloud), **Visualisierung** (Grafana-Dashboards) | OPC UA, MQTT, 5G, Edge Computing    |
| **Gesundheitswesen**  | Wearables (Apple Watch, EKG-Pflaster) | **Datenanalyse** (Herzfrequenz → Warnmeldung), **Datenschutz** (DSGVO-konforme Cloud) | BLE, LoRaWAN, Cloud-Speicher         |
| **Mobilität**         | Vernetzte Autos (Tesla), Mietfahrräder (Nextbike) | **Backend-Entwicklung** (Flottenmanagement), **Echtzeit-Kommunikation** (5G) | 5G, GPS, Telematik-Systeme           |
| **Landwirtschaft**    | Bodenfeuchtemessung (John Deere), Drohnenüberwachung | **Sensorik** (LoRaWAN-Gateways), **KI-Auswertung** (Ertragsprognosen) | LoRaWAN, KI/ML, Drohnen-Steuerung    |
| **Energieversorgung** | Smart Meter (Stromzähler), Windkraftanlagen | **Echtzeit-Monitoring** (Verbrauchsdaten), **Automatisierung** (Laststeuerung) | NB-IoT, PLC, SCADA-Systeme           |

---

### **📚 IHK-Relevanz für FIAE/FISI-Umschüler**

*Dieses Dokument deckt **zentrale Lernziele des IHK-Rahmenlehrplans** ab:*

- **Netzwerktechnik:** IoT-Protokolle (MQTT, LoRaWAN, 5G), Netzwerkarchitekturen
- **Datenbanken & Cloud:** Speicherung/Visualisierung von Sensordaten (SQLite, InfluxDB, Grafana)
- **Sicherheitstechnik:** Absicherung von IoT-Systemen (Firewalls, Verschlüsselung, DSGVO)
- **Automatisierungstechnik:** Vernetzung von Geräten (OPC UA, Modbus, MQTT-SN)
- **Programmierung:** Python, Arduino/C++, JavaScript (Node-RED)

🔹 **Prüfungstipp:** IoT-Themen tauchen in **Fachgesprächen** und **Projektarbeiten** auf – dieses Projekt ist **direkt prüfungsrelevant!**

---

## **2. Aktueller Stand der IoT-Technik (2026)**

*(Fakten, Zahlen, Daten – mit Fokus auf Praxistauglichkeit)*

### **📊 Markt & Wachstum**

| **Kennzahl**               | **Wert (2026)**                     | **Quelle**                                                                 |
|----------------------------|-------------------------------------|----------------------------------------------------------------------------|
| **IoT-Geräte weltweit**    | ~30 Mrd. (2026)                     | [IoT Analytics – IoT Market Report 2026](https://iot-analytics.com/iot-market-report-2026/) |
| **Marktvolumen IoT**       | ~1.6 Billionen USD (2026)           | [McKinsey – The IoT Value Proposition](https://www.mckinsey.com/industries/advanced-electronics/our-insights/the-internet-of-things-value-prop) |
| **Anteil Industrie-IoT**   | ~50% der IoT-Geräte                 | [Statista – Industrie 4.0 Markt 2026](https://www.statista.com/outlook/tmo/iot/industrial-internet-of-things/worldwide) |
| **5G-IoT-Nutzung**         | ~30% aller IoT-Geräte (2026)        | [GSMA – 5G IoT Market 2026](https://www.gsma.com/iot/5g-iot-market/)       |

### **🔧 Technische Standards im Überblick**

*(Vergleichstabelle für Systemintegratoren)*

| **Technologie**  | **Einsatzgebiet**          | **Reichweite** | **Datenrate** | **Stromverbrauch** | **Kosten (Modul)** | **Beispiele**                     |
|------------------|----------------------------|----------------|---------------|--------------------|--------------------|------------------------------------|
| **WiFi (IEEE 802.11)** | Smart Home, Büro          | ~100 m         | 10–100 Mbps   | Hoch (1–5 W)       | ~5–20 €            | ESP32, Raspberry Pi                |
| **Bluetooth LE** | Wearables, Sensoren        | ~50 m          | 1–3 Mbps      | Sehr niedrig (0.1–1 W) | ~2–10 €       | Apple Watch, BLE-Tags              |
| **LoRaWAN**      | Landwirtschaft, Smart City | 2–15 km        | 0.3–50 kbps   | Sehr niedrig (0.05–0.5 W) | ~10–30 €   | Bodenfeuchtesensoren, Mülltonnen  |
| **NB-IoT**       | Smart Meter, Logistik     | 1–10 km        | 20–250 kbps   | Sehr niedrig (0.02–0.2 W) | ~5–15 €    | Stromzähler, GPS-Tracker           |
| **5G (mMTC)**    | Industrie 4.0, Automotive  | ~1 km          | 1–10 Mbps     | Mittel (1–3 W)     | ~50–200 €          | Vernetzte Maschinen, autonome Autos|
| **Zigbee**       | Smart Home (Mesh-Netzwerk) | ~20 m          | 20–250 kbps   | Niedrig (0.1–0.5 W) | ~5–15 €            | Philips Hue, IKEA Tradfri         |
| **MQTT**         | Nachrichtenprotokoll       | –              | Variabel      | Gering (abhängig vom Transport) | Kostenlos | Cloud-Anbindung, Edge Computing    |

🔹 **Praxistipp:**

- **Kurze Reichweite + hohe Datenrate?** → **WiFi oder 5G**
- **Lange Reichweite + niedriger Stromverbrauch?** → **LoRaWAN oder NB-IoT**
- **Echtzeit-Anforderungen?** → **5G oder MQTT over TCP**

---

### **🛡️ IoT-Sicherheit: Der größte Risikofaktor**

*(Aktuelle Bedrohungen & Gegenmaßnahmen)*

| **Bedrohung**               | **Beispiel**                     | **Gegenmaßnahme**                                                                 |
|-----------------------------|----------------------------------|-----------------------------------------------------------------------------------|
| **Standard-Passwörter**     | „admin/admin“ auf IoT-Geräten    | **Passwort-Manager**, **Firmware-Updates**, **Segmentierung des Netzwerks**      |
| **Unverschlüsselte Daten**  | MQTT ohne TLS                    | **TLS/SSL verschlüsseln**, **VPN für Remote-Zugriff**                            |
| **Botnet-Angriffe**         | Mirai-Variante für IoT-Geräte    | **Firewalls**, **IDS/IPS-Systeme**, **Regelmäßige Sicherheitsaudits**            |
| **Man-in-the-Middle**       | Abfangen von Sensordaten         | **Zertifikate**, **Authentifizierung (OAuth2, JWT)**, **VPN**                     |
| **Firmware-Schwachstellen** | Ungepatchte Sicherheitslücken    | **Automatische Updates**, **Rollback-Mechanismen**                                |

🔹 **Zitat eines Experten:**
> *„80% aller IoT-Angriffe nutzen Standard-Passwörter oder ungepatchte Firmware. Ein einziges unsicheres Gerät kann ein ganzes Netzwerk kompromittieren.“*
> — [ENISA – IoT Security Threats 2025](https://www.enisa.europa.eu/publications/enisa-threat-landscape-report-2025)

---

### **🚀 Zukunftstrends (2026–2030)**

*(Was bewegt die IT in den nächsten Jahren?)*

| **Trend**                  | **Beschreibung**                          | **Praktische Relevanz**                          |
|----------------------------|-------------------------------------------|--------------------------------------------------|
| **Edge Computing**         | Datenverarbeitung **vor Ort** (nicht in der Cloud) | Reduziert Latenz, spart Bandbreite (z. B. Predictive Maintenance in Fabriken) |
| **KI/ML für IoT**          | Anomalieerkennung in Echtzeit (z. B. Vibrationsmuster) | **TensorFlow Lite auf ESP32/Raspberry Pi**       |
| **Digital Twins**          | Digitale Abbilder physischer Systeme (z. B. virtuelle Maschine) | **Simulation von Wartungsszenarien**             |
| **6G & Terahertz-Kommunikation** | 10x schnellere Datenübertragung als 5G | **Echtzeit-Steuerung von Robotern**              |
| **Quantum-Safe IoT**       | Verschlüsselung für Quantencomputer-Ära | **Post-Quantum-Kryptographie (z. B. CRYSTALS-Kyber)** |
| **Nachhaltiges IoT**       | Energieeffiziente Sensoren (z. B. Energy Harvesting) | **Solarbetriebene LoRaWAN-Sensoren**             |

🔹 **Für Umschüler:**

- **Edge Computing** und **KI/ML** sind **die wichtigsten Skills der nächsten Jahre**!
- **Digital Twins** werden in **Industrie 4.0 und Smart Cities** immer relevanter.

---

## **3. Anwendungsbereiche – Von heute bis 2030**

*(Aktuelle & zukünftige Use Cases mit IT-Bezug)*

### **🔧 Industrie 4.0 & Smart Manufacturing**

| **Anwendung**               | **Technologie**               | **IT-Aufgaben**                                  |
|-----------------------------|-------------------------------|--------------------------------------------------|
| **Predictive Maintenance**  | Vibrationssensoren + KI       | **Datenanalyse**, **MQTT-Broker**, **Grafana-Dashboards** |
| **Autonome Roboter**        | 5G + Edge Computing           | **Echtzeit-Steuerung**, **Sicherheitsprotokolle** |
| **Digitaler Zwilling**      | 3D-Modelle + IoT-Daten        | **Simulationstools (z. B. Siemens NX)**, **Datenbankanbindung** |
| **Qualitätskontrolle**      | Computer Vision + IoT         | **Python (OpenCV)**, **Cloud-Anbindung (AWS Rekognition)** |

🔹 **Projektidee:**
Baue ein **Digital Twin-Modell** einer Maschine mit **ESP32-Sensoren** und **Unity/Unreal Engine**!

---

### **🏥 Gesundheitswesen & Wearables**

| **Anwendung**               | **Technologie**               | **IT-Aufgaben**                                  |
|-----------------------------|-------------------------------|--------------------------------------------------|
| **Remote-Patientenmonitoring** | BLE-Sensoren + Cloud         | **Datenverschlüsselung (HIPAA)**, **Python (Flask API)** |
| **Notfallerkennung**        | EKG-Pflaster + KI             | **Anomalieerkennung (TensorFlow)**, **Echtzeit-Alarmierung** |
| **Medikamentenverfolgung**  | RFID/NFC + IoT                | **Datenbank (SQLite)**, **Mobile App (React Native)** |

🔹 **Achtung Datenschutz:**

- **DSGVO-konforme Cloud** (z. B. **AWS HealthLake** oder **Google Healthcare API**)
- **Ende-zu-Ende-Verschlüsselung** (z. B. **Signal Protocol**)

---

### **🌱 Smart City & Nachhaltigkeit**

| **Anwendung**               | **Technologie**               | **IT-Aufgaben**                                  |
|-----------------------------|-------------------------------|--------------------------------------------------|
| **Müllmanagement**          | LoRaWAN-Sensoren in Mülltonnen | **Backend (Node.js)**, **LoRaWAN-Gateway**       |
| **Verkehrssteuerung**       | KI-gesteuerte Ampeln          | **Python (OpenCV für Kameras)**, **5G-Anbindung** |
| **Energieeffizienz**        | Smart Meter + KI              | **Datenanalyse (Pandas)**, **Visualisierung (Grafana)** |

---

### **🚗 Vernetzte Mobilität**

| **Anwendung**               | **Technologie**               | **IT-Aufgaben**                                  |
|-----------------------------|-------------------------------|--------------------------------------------------|
| **Autonome Lieferroboter**  | 5G + LiDAR                    | **ROS (Robot Operating System)**, **Edge AI**    |
| **Flottenmanagement**       | GPS-Tracker + Cloud           | **Backend (Django/Flask)**, **Echtzeit-Karten (Leaflet.js)** |
| **Vernetzte Fahrzeuge**     | OBD-II + Telematik            | **CAN-Bus-Auslesung**, **MQTT-Broker für Telemetrie** |

---

### **🌾 Landwirtschaft 4.0**

| **Anwendung**               | **Technologie**               | **IT-Aufgaben**                                  |
|-----------------------------|-------------------------------|--------------------------------------------------|
| **Bodenfeuchtemessung**     | LoRaWAN-Sensoren              | **LoRaWAN-Gateway**, **Datenbank (InfluxDB)**    |
| **Drohnenüberwachung**      | KI + Drohnen-Kamera           | **Python (OpenCV für Bildanalyse)**, **Cloud-Speicher** |
| **Präzisionslandwirtschaft** | GPS-gesteuerte Traktoren      | **RTK-GPS (cm-Genauigkeit)**, **Farm-Management-Software** |

---

## **4. Wissenswertes für Umschüler in der IT**

*(Praktische Tipps, Stolpersteine & Karrierepfade)*

### **🛠️ Die 5 wichtigsten IoT-Skills für deine Bewerbung**

| **Skill**                  | **Warum wichtig?**                          | **Wie lernen?**                                  |
|----------------------------|---------------------------------------------|--------------------------------------------------|
| **MQTT & IoT-Protokolle**  | Standard für Nachrichtenübermittlung in IoT | [MQTT Tutorial (HiveMQ)](https://www.hivemq.com/mqtt-essentials/) |
| **Python für IoT**         | Datenanalyse, Cloud-Anbindung, KI          | [Python for IoT (Raspberry Pi)](https://projects.raspberrypi.org/en/projects/getting-started-with-python/) |
| **Sensorik & Mikrocontroller** | Hardware-Programmierung (ESP32, Arduino) | [ESP32 Arduino Guide](https://randomnerdtutorials.com/getting-started-with-esp32/) |
| **Cloud-Dienste (AWS/Azure)** | Skalierbare IoT-Lösungen                   | [AWS IoT Core Tutorial](https://aws.amazon.com/de/iot-core/) |
| **Sicherheit in IoT**      | Absicherung gegen Cyberangriffe             | [OWASP IoT Top 10](https://owasp.org/www-project-iot-top-10/) |

---

### **⚠️ Die 5 größten Stolpersteine für IoT-Einsteiger**

*(Und wie du sie vermeidest)*

| **Stolperstein**           | **Problem**                                  | **Lösung**                                      |
|----------------------------|---------------------------------------------|-------------------------------------------------|
| **Falsche Hardware wählen** | Zu teuer, zu komplex oder falsche Schnittstellen | **Starte mit ESP32 + Sensoren (günstig & flexibel)** |
| **Netzwerkprobleme**       | MQTT-Broker nicht erreichbar, WLAN-Instabilität | **Teste mit `ping` und `mosquitto_sub` vor dem Deployment** |
| **Sicherheitslücken**      | Standard-Passwörter, unverschlüsselte Daten | **Nutze TLS, Firewalls und Passwort-Manager**   |
| **Datenüberlastung**       | Zu viele Sensordaten → Cloud-Kosten explodieren | **Filterung (z. B. nur bei Änderungen senden)** |
| **Firmware-Updates vergessen** | Geräte werden nach 1 Jahr unsicher         | **Automatische Updates (z. B. mit PlatformIO)** |

---

### **💼 Karrierepfade im IoT-Bereich**

*(Gehaltsrahmen & Jobprofile – IHK-konform)*

| **Jobprofil**              | **Aufgaben**                                  | **Gehaltsrahmen (2026)** | **Benötigte Skills**                          |
|----------------------------|-----------------------------------------------|--------------------------|-----------------------------------------------|
| **IoT-Systemintegrator**   | Vernetzung von Geräten, MQTT/LoRaWAN-Setup    | 45.000–65.000 €          | MQTT, Sensorik, Netzwerktechnik               |
| **IoT-Entwickler (Backend)** | Cloud-Anbindung, Datenanalyse (Python)      | 55.000–80.000 €          | Python, AWS/Azure, SQL/NoSQL                 |
| **IoT-Sicherheitsexperte** | Absicherung von IoT-Systemen gegen Cyberangriffe | 60.000–90.000 €    | Kryptographie, Firewalls, Penetration Testing |
| **Embedded-IoT-Entwickler** | Programmierung von Mikrocontrollern (C/C++)  | 50.000–75.000 €          | C++, RTOS, Hardware-Design                    |
| **IoT-Data Scientist**     | KI/ML für Sensordaten (Predictive Maintenance) | 65.000–95.000 €    | Python, TensorFlow, Datenvisualisierung       |
| **IoT-Projektmanager**     | Koordination von IoT-Projekten (Agile/Scrum) | 55.000–85.000 €          | Projektmanagement, IoT-Architekturen          |

🔹 **Gehaltsdaten:**

- **Quelle:** [Gehalt.de – IoT-Jobs 2026](https://www.gehalt.de/) (aggregierte Daten)
- **Trend:** **IoT-Sicherheitsexperten** und **IoT-Data Scientists** werden am besten bezahlt!

---

### **📚 Lernressourcen für Umschüler**

*(Kostenlos & praxisnah)*

| **Ressource**              | **Typ**                     | **Link**                                                                 |
|----------------------------|-----------------------------|--------------------------------------------------------------------------|
| **Arduino Project Hub**    | Tutorials & Projekte         | [https://create.arduino.cc/projecthub](https://create.arduino.cc/projecthub) |
| **Random Nerd Tutorials**  | ESP32, Raspberry Pi, MQTT   | [https://randomnerdtutorials.com/](https://randomnerdtutorials.com/)   |
| **MQTT Essentials**        | Protokoll-Dokumentation      | [https://www.hivemq.com/mqtt-essentials/](https://www.hivemq.com/mqtt-essentials/) |
| **IoT Security Guide**     | ENISA-Empfehlungen          | [https://www.enisa.europa.eu/topics/iot-and-smart-infrastructures](https://www.enisa.europa.eu/topics/iot-and-smart-infrastructures) |
| **RFC 6690 (MQTT)**        | Offizieller Standard         | [https://datatracker.ietf.org/doc/html/rfc6690](https://datatracker.ietf.org/doc/html/rfc6690) |
| **IoT-Anwendungsfälle**    | Use-Case-Sammlung           | [https://www.postscapes.com/iot-use-cases/](https://www.postscapes.com/iot-use-cases/) |

---

## **5. IHK-relevante Punkte**

*(Direkt aus dem Rahmenlehrplan – Prüfungsvorbereitung)*

### **📋 IHK-Prüfungsthemen im IoT-Bereich**

*(Häufig geprüfte Inhalte)*

| **Thema**                  | **Prüfungsrelevanz**                          | **Lernziel**                                      |
|----------------------------|-----------------------------------------------|---------------------------------------------------|
| **IoT-Architekturen**      | ✅ Sehr hoch (z. B. Edge vs. Cloud)           | Verstehen von **3-Schichten-Modell** (Sensor → Gateway → Cloud) |
| **IoT-Protokolle**         | ✅ Hoch (MQTT, LoRaWAN, 5G)                  | **Vergleichstabelle** (Reichweite, Datenrate, Stromverbrauch) |
| **Sicherheit in IoT**      | ✅ Hoch (DSGVO, TLS, Firewalls)              | **Checkliste für sichere IoT-Systeme**            |
| **Datenanalyse**           | ✅ Mittel (Python, Pandas, Grafana)          | **Einfache Datenauswertung** (z. B. Vibrationsmuster) |
| **Automatisierung**        | ✅ Mittel (OPC UA, MQTT-SN)                  | **Integration von IoT in bestehende Systeme**     |
| **Nachhaltigkeit**         | ✅ Niedrig (aber gefragt!)                   | **Energieeffiziente Sensoren** (z. B. Energy Harvesting) |

---

### **📝 Prüfungsbeispiel: Predictive Maintenance (IHK-konform)**

*(Typische Aufgabe aus der Abschlussprüfung Teil 2)*

**Aufgabenstellung:**
*Ein mittelständisches Unternehmen möchte eine **Predictive Maintenance-Lösung** für seine CNC-Maschinen einführen. Die Maschine soll Vibrationen messen und bei abnormalen Werten eine Warnung an die Wartungsabteilung senden. Beschreibe die technische Umsetzung und erstelle ein **Blockdiagramm** der Lösung.*

**Lösungsskizze (für Umschüler):**

1. **Hardware:**
   - **Sensor:** Vibrationssensor (z. B. SW-420 oder ADXL345)
   - **Mikrocontroller:** ESP32 (WLAN + MQTT)
   - **Gateway:** Raspberry Pi (MQTT-Broker)
   - **Cloud:** Optional (z. B. AWS IoT Core oder InfluxDB für Langzeitspeicherung)

2. **Software:**
   - **ESP32:** Arduino-Sketch (misst Vibrationen, sendet über MQTT)
   - **Raspberry Pi:** Python-Skript (empfängt MQTT-Daten, speichert in InfluxDB)
   - **Visualisierung:** Grafana-Dashboard (zeigt Vibrationsverlauf an)

3. **Blockdiagramm:**

[CNC-Maschine] → [Vibrationssensor] → [ESP32] → (MQTT) → [Raspberry Pi (Broker)] → [InfluxDB] → [Grafana-Dashboard]
                                                                   ↓
                                                           [Warnmeldung an Wartung]


4.**Sicherheit:**

- **MQTT mit TLS verschlüsseln** (Port 8883)
- **Firewall-Regeln** (nur Raspberry Pi darf MQTT empfangen)
- **Passwort-Schutz** für MQTT-Broker

5.**Erweiterung (Bonus):**

- **KI-Integration:** TensorFlow Lite auf Raspberry Pi für Anomalieerkennung
- **E-Mail-Alarm:** Python-Skript sendet Warnung per SMTP

---

### **🔍 Typische Prüfungsfragen & Antworten**

*(Mit Musterlösungen für mündliche Prüfungen)*

| **Frage**                          | **Musterantwort**                                                                 |
|------------------------------------|-----------------------------------------------------------------------------------|
| *„Warum MQTT und nicht HTTP?“*     | *„MQTT ist **leichtgewichtiger** und **energiesparender** als HTTP. Es nutzt **Publish/Subscribe** statt Request/Response und ist ideal für **Echtzeit-Anwendungen** wie IoT.“* |
| *„Wie sichern Sie ein IoT-System?“* | *„1. **Verschlüsselung** (TLS für MQTT), 2. **Firewalls** (nur notwendige Ports offen), 3. **Passwort-Management** (keine Standard-Passwörter), 4. **Regelmäßige Updates** der Firmware.“* |
| *„Was ist Edge Computing?“*        | *„Datenverarbeitung **vor Ort** (z. B. auf einem Raspberry Pi) statt in der Cloud. Vorteile: **geringere Latenz**, **geringere Bandbreitenkosten**, **bessere Sicherheit**.“* |
| *„Wie integrieren Sie IoT in eine bestehende Industrieanlage?“* | *„Über **OPC UA** oder **MQTT-SN** (MQTT for Sensor Networks). OPC UA ist **industrieller Standard**, MQTT-SN ist **energiesparender** für Sensoren.“* |

---

## **6. Praxisprojekt: Predictive Maintenance mit ESP32 & MQTT**

*(Lauffähiges IoT-Projekt – Schritt für Schritt zum eigenen System)*


### **🎯 Projektziel**

Baue ein **IoT-System**, das:

1. **Vibrationen einer Maschine misst** (z. B. mit einem ESP32 und einem Vibrationssensor).
2. **Daten an einen MQTT-Broker sendet** (Raspberry Pi).
3. **Warnungen ausgibt**, wenn die Vibration einen Schwellenwert überschreitet.
4. **Erweiterbar ist** (z. B. mit Datenbank, KI oder E-Mail-Alarm).

---

### **📦 Materialliste (Kosten: ~30–50 €)**

| **Komponente**       | **Beispiel**                     | **Hinweis**                                  | **Bezugsquelle**                          |
|-----------------------|----------------------------------|---------------------------------------------|-------------------------------------------|
| Mikrocontroller       | ESP32 DevKit (z. B. ESP32-WROOM-32) | WLAN + Bluetooth integriert, günstig (~10 €) | [Amazon](https://www.amazon.de/s?k=ESP32+DevKit) / [Reichelt](https://www.reichelt.de/) |
| Vibrationssensor      | SW-420 (digital) oder ADXL345 (analog) | SW-420: einfacher (~3 €), ADXL345: präziser (~8 €) | [AliExpress](https://www.aliexpress.com/) / [Conrad](https://www.conrad.de/) |
| Raspberry Pi          | Raspberry Pi 4 (4GB RAM)         | MQTT-Broker + Python-Skript (~50 €)         | [Amazon](https://www.amazon.de/s?k=Raspberry+Pi+4) |
| Stromversorgung       | USB-Kabel (5V/2A)                | Für ESP32 und Raspberry Pi                  | –                                         |
| Steckbrett & Kabel    | Jumper-Wires (20 Stück)          | Für Prototypenaufbau                        | [Amazon](https://www.amazon.de/s?k=Jumper+Wires) |
| Optional: Breadboard  | 400-Pin Steckbrett               | Für sauberen Aufbau (~5 €)                  | [Amazon](https://www.amazon.de/s?k=Breadboard) |

🔹 **Gesamtkosten:** ~30–50 € (je nach Sensor und Raspberry Pi-Modell)

---

### **🔌 Schaltplan (ESP32 + Vibrationssensor)**

#### **Option 1: Einfacher Vibrationssensor (SW-420)**

ESP32 (3.3V) ──── GPIO13 ──── SW-420 (Signal)
ESP32 (GND) ──── GND       ──── SW-420 (GND)
ESP32 (5V)   ──── VCC       ──── SW-420 (VCC)

- **Schwellenwert einstellen:** Potentiometer am SW-420 drehen, bis die LED bei Vibration leuchtet.

#### **Option 2: Präziser Beschleunigungssensor (ADXL345)**

ESP32 (3.3V) ──── 3.3V      ──── ADXL345 (VCC)
ESP32 (GND) ──── GND       ──── ADXL345 (GND)
ESP32 (GPIO21) ──── SDA     ──── ADXL345 (SDA)
ESP32 (GPIO22) ──── SCL     ──── ADXL345 (SCL)

- **Dokumentation:**
  - [SW-420 Datasheet](https://components101.com/sensors/sw-420-vibration-sensor-module)
  - [ADXL345 Datasheet](https://www.analog.com/media/en/technical-documentation/data-sheets/ADXL345.pdf)
  - [ESP32 Pinout](https://randomnerdtutorials.com/esp32-pinout-reference-gpios/)

---

### **💻 Code: ESP32 (Arduino IDE)**

#### **Schritt 1: Bibliotheken installieren**

1. **Arduino IDE** installieren: [https://www.arduino.cc/en/software](https://www.arduino.cc/en/software)
2. **ESP32 Board Support** hinzufügen:
   - **Datei → Voreinstellungen → Zusätzliche Boardverwalter-URLs:**
     `https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json`
   - **Werkzeuge → Board → ESP32 Arduino → ESP32 Dev Module**
3. **Benötigte Bibliotheken:**
   - `PubSubClient` (für MQTT) → **Sketch → Bibliothek einbinden → Bibliotheken verwalten → "PubSubClient" suchen**
   - `WiFi` (integriert in ESP32 Arduino Core)

#### **Schritt 2: Arduino-Sketch (SW-420)**

```cpp
#include <WiFi.h>
#include <PubSubClient.h>

// WiFi-Konfiguration
const char* ssid = "DEIN_WLAN_NAME";
const char* password = "DEIN_WLAN_PASSWORT";

// MQTT-Broker (Raspberry Pi IP)
const char* mqtt_server = "192.168.1.100"; // Ersetze mit deiner Raspberry Pi IP!
const int mqtt_port = 1883;

// MQTT-Topics
const char* vibration_topic = "vibration/data";

// ESP32-Pins
const int vibrationPin = 13; // GPIO13 für SW-420

WiFiClient espClient;
PubSubClient client(espClient);

void setup_wifi() {
  Serial.println("Verbinde mit WiFi...");
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("\nWiFi verbunden!");
  Serial.print("IP-Adresse: ");
  Serial.println(WiFi.localIP());
}

void reconnect() {
  while (!client.connected()) {
    Serial.println("Verbinde mit MQTT-Broker...");
    if (client.connect("ESP32-Vibration")) {
      Serial.println("MQTT verbunden!");
    } else {
      Serial.print("Fehler, rc=");
      Serial.print(client.state());
      Serial.println(" Neuer Versuch in 5s...");
      delay(5000);
    }
  }
}

void setup() {
  Serial.begin(115200);
  pinMode(vibrationPin, INPUT);
  setup_wifi();
  client.setServer(mqtt_server, mqtt_port);
}

void loop() {
  if (!client.connected()) {
    reconnect();
  }
  client.loop();

  int vibration = digitalRead(vibrationPin);
  if (vibration > 0) { // Schwellenwert anpassen!
    Serial.println("Vibration detektiert!");
    client.publish(vibration_topic, String(vibration).c_str());
  }
  delay(1000);
}

Schritt 3: Arduino-Sketch (ADXL345)

#include <Wire.h>
#include <Adafruit_ADXL345_U.h>
#include <WiFi.h>
#include <PubSubClient.h>

// WiFi & MQTT (wie oben)
const char* ssid = "DEIN_WLAN_NAME";
const char* password = "DEIN_WLAN_PASSWORT";
const char* mqtt_server = "192.168.1.100";
const int mqtt_port = 1883;
const char* vibration_topic = "vibration/accel";

WiFiClient espClient;
PubSubClient client(espClient);

Adafruit_ADXL345_Unified accel = Adafruit_ADXL345_Unified(12345);

void setup_wifi() {
  // Wie oben
}

void reconnect() {
  // Wie oben
}

void displaySensorDetails() {
  sensor_t sensor;
  accel.getSensor(&sensor);
  Serial.println("------------------------------------");
  Serial.print("Sensor:       "); Serial.println(sensor.name);
  Serial.print("Driver Ver:   "); Serial.println(sensor.version);
  Serial.print("Unique ID:    "); Serial.println(sensor.sensor_id);
  Serial.print("Max Value:    "); Serial.print(sensor.max_value); Serial.println(" m/s^2");
  Serial.print("Min Value:    "); Serial.print(sensor.min_value); Serial.println(" m/s^2");
  Serial.print("Resolution:   "); Serial.print(sensor.resolution); Serial.println(" m/s^2");
  Serial.println("------------------------------------");
  delay(500);
}

void setup() {
  Serial.begin(115200);
  setup_wifi();

  if (!accel.begin()) {
    Serial.println("ADXL345 nicht gefunden!");
    while (1);
  }
  displaySensorDetails();
  accel.setRange(ADXL345_RANGE_16_G); // ±16g

  client.setServer(mqtt_server, mqtt_port);
}

void loop() {
  if (!client.connected()) reconnect();
  client.loop();

  sensors_event_t event;
  accel.getEvent(&event);

  // Sende X-Achse (kann angepasst werden)
  float vibration = event.acceleration.x;
  if (vibration > 2.0) { // Schwellenwert anpassen!
    Serial.print("Vibration (X): "); Serial.println(vibration);
    client.publish(vibration_topic, String(vibration).c_str());
  }
  delay(1000);
}

🐍 Code: Raspberry Pi (Python-MQTT-Client & Broker)
Schritt 1: MQTT-Broker installieren (Mosquitto)

# Raspberry Pi Terminal
sudo apt update
sudo apt install mosquitto mosquitto-clients
sudo systemctl start mosquitto
sudo systemctl enable mosquitto

Schritt 2: Python-Skript (Empfänger & Datenlogger)

import paho.mqtt.client as mqtt
import sqlite3
from datetime import datetime

# SQLite-Datenbank
conn = sqlite3.connect('vibration.db')
cursor = conn.cursor()
cursor.execute('''CREATE TABLE IF NOT EXISTS vibrations
                  (timestamp TEXT, value REAL)''')
conn.commit()

def on_message(client, userdata, msg):
    vibration = float(msg.payload.decode())
    timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    print(f"📊 [{timestamp}] Vibration: {vibration}")

    # Speichern in Datenbank
    cursor.execute("INSERT INTO vibrations VALUES (?, ?)", (timestamp, vibration))
    conn.commit()

    # Warnung bei hohem Wert
    if vibration > 2.0:
        print("⚠️ Warnung: Hohe Vibration detektiert!")

client = mqtt.Client("RPi-Listener")
client.on_message = on_message
client.connect("localhost")  # Oder IP des Raspberry Pi
client.subscribe("vibration/data")
client.loop_forever()

Schritt 3: Python-Skript (mit InfluxDB & Grafana)

import paho.mqtt.client as mqtt
from influxdb_client import InfluxDBClient, Point, WritePrecision
from influxdb_client.client.write_api import SYNCHRONOUS

# InfluxDB 2.x Konfiguration
token = "DEIN_INFLUXDB_TOKEN"  # z. B. von https://influxdata.com/
org = "DEINE_ORG"
bucket = "vibration_data"
url = "http://localhost:8086"  # Oder IP des InfluxDB-Servers

client_influx = InfluxDBClient(url=url, token=token, org=org)
write_api = client_influx.write_api(write_options=SYNCHRONOUS)

def on_message(client, userdata, msg):
    vibration = float(msg.payload.decode())
    timestamp = datetime.now().isoformat()

    # Punkt für InfluxDB erstellen
    point = Point("vibration") \
        .tag("sensor", "ESP32") \
        .field("value", vibration) \
        .time(timestamp, WritePrecision.NS)

    write_api.write(bucket=bucket, org=org, record=point)
    print(f"📊 Gespeichert: {vibration} @ {timestamp}")

    if vibration > 2.0:
        print("⚠️ Warnung: Hohe Vibration!")

mqtt_client = mqtt.Client("RPi-Influx-Listener")
mqtt_client.on_message = on_message
mqtt_client.connect("localhost")
mqtt_client.subscribe("vibration/data")
mqtt_client.loop_forever()

📊 Visualisierung mit Grafana (optional)

    InfluxDB installieren:

    sudo apt install influxdb2
    sudo systemctl start influxdb

    Grafana installieren:

    sudo apt install -y apt-transport-https
    sudo apt install -y software-properties-common wget
    wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
    echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
    sudo apt update
    sudo apt install grafana
    sudo systemctl start grafana-server

    Grafana einrichten:
        Öffne http://<RASPBERRY_PI_IP>:3000 im Browser.
        Datenquelle: InfluxDB (URL: http://localhost:8086, Token: dein Token).
        Dashboard erstellen: Wähle die vibration-Messung und erstelle ein Liniendiagramm.

🚀 Erweiterungsmöglichkeiten
Erweiterung 	Umsetzung 	Code/Tools
Daten speichern 	Langzeitspeicherung in InfluxDB oder SQLite 	influxdb-client-python, sqlite3
Visualisierung 	Grafana-Dashboard (Vibrationsverlauf) 	Grafana, InfluxDB
E-Mail/SMS-Warnungen 	Automatische Benachrichtigung bei hoher Vibration 	smtplib (Python), Twilio API
KI-Anomalieerkennung 	TensorFlow Lite auf Raspberry Pi für maschinelles Lernen 	TensorFlow Lite
OPC UA-Integration 	Verbindung zu Industrie-SPS (z. B. Siemens TIA Portal) 	opcua-asyncio (Python)
MQTT over TLS 	Verschlüsselung der MQTT-Kommunikation 	mosquitto.conf (TLS-Zertifikate)
🔗 Vollständige Ressourcen & Beispiel-Repos
Ressource 	Link 	Beschreibung
ESP32 + MQTT + Python 	https://github.com/RuiSantosdotme/ESP32-MQTT-Python 	Komplettes Tutorial mit Code & Schaltplan
IoT Predictive Maintenance 	https://github.com/knolleary/pubsubclient 	MQTT-Bibliothek für Arduino/ESP32
InfluxDB + Grafana Setup 	https://github.com/influxdata/influxdb-client-python 	Python-Bibliothek für InfluxDB
OPC UA für Industrie-IoT 	https://github.com/FreeOpcUa/python-opcua 	OPC UA-Client für Python
IoT-Sicherheitscheckliste 	https://owasp.org/www-project-iot-top-10/ 	OWASP IoT Top 10 Risiken
🎯 Warum dieses Projekt für dich wertvoll ist

✅ Prüfungsrelevant:

    Kombiniert Netzwerktechnik (MQTT), Programmierung (Python/C++), Datenbanken (SQLite/InfluxDB) und Sicherheit (TLS).
    Direkt anwendbar für IHK-Prüfungen (z. B. Predictive Maintenance).

✅ Jobrelevant:

    Zeigt praktische IoT-Skills, die Arbeitgeber suchen (z. B. MQTT, Raspberry Pi, Sensorik).
    Kann in Bewerbungen als Portfolio-Projekt präsentiert werden.

✅ Portfolio-tauglich:

    Dokumentation (README.md auf GitHub mit Fotos/Videos).
    Erweiterbar (z. B. mit KI, OPC UA oder Digital Twins).

💡 Nächste Schritte: Von hier zum Profi

    Baue das Projekt nach (Hardware + Code).
    Erweitere es (z. B. mit InfluxDB + Grafana oder KI).
    Dokumentiere es (README.md auf GitHub mit Schritt-für-Schritt-Anleitung).
    Präsentiere es (z. B. in Bewerbungen, IHK-Prüfungen oder auf LinkedIn).
    Teile es (z. B. in IoT-Communities wie Hackster.io).

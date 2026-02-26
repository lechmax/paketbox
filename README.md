# Paketbox Steuerung 📦

![Tests](https://github.com/lechmax/paketbox/workflows/Paketbox%20Tests/badge.svg)

![Paketbox](blueprint/Paketbox.jpeg)
*Die intelligente Paketbox mit automatischer Lieferannahme und Verriegelung.*

Dieses Projekt steuert eine intelligente Paketbox mit einem Raspberry Pi. Die Box kann Pakete sicher aufnehmen, automatisch verriegeln und entleeren. Die Steuerung erfolgt über Motoren, Sensoren und Relais mit professioneller Fehlerbehandlung und Logging.

## ✨ Features
- **Automatisches Öffnen und Schließen** der Entleerungsklappen
- **Intelligente Türverriegelung** nach Paketeingang
- **Umfassende Sensorüberwachung** für alle Klappen und Türen
- **Robuste Fehlerbehandlung** mit automatischen ERROR-States
- **Professionelles Logging** mit Datei- und Console-Output
- **Mock-Modus** für lokale Entwicklung ohne Hardware
- **MQTT-Integration** für IoT-Benachrichtigungen
- **Thread-sichere Zustandsverwaltung** mit Locking-Mechanismen

- **Modulare Architektur** mit separaten Komponenten
- **15-Minuten-Watchdog** für geöffnete Paketzustellertüren inkl. MQTT-Warnung und Auto-Entleerung
- **Lichtschranke mit Notstopp** für Schutz vor Einklemmungen
- **MQTT-basierte Steuerung** (Status, Zusteller/Briefkasten/Entleerung, Auto-Lock-Door)
- **Zeitgesteuerte Türverriegelung** mit konfigurierbaren Sperrzeiten
- **Ereignisse für Briefkasten & Paketbox** (ON/OFF) plus Entleerungsbetrieb

## 🆕 Neueste Änderungen (Frühjahr 2026)
- **Watchdog für geöffnete Türen**: In `handler.py` überwacht ein 15-Minuten-Timer offene Paketzustellertüren, verschickt bei Bedarf MQTT-Warnungen und startet automatisch den Entleerungszyklus.
- **Zentraler TimerManager**: `TimerManager.py` verwaltet jetzt alle Motor-, Prüf- und Watchdog-Timer mit Thread-Lock, wodurch Nothalt-Szenarien sämtliche Timer zuverlässig abbrechen.
- **Verbesserter Fehler-Reset**: `ResetErrorState()` initialisiert Türsensoren neu, setzt Motorzustände sicher auf `STOPPED` und verhindert doppelte MQTT-Fehlerbenachrichtigungen.

## 🔧 Hardware
- **Raspberry Pi** mit GPIO-Steuerung
- **2 Motoren** für Entleerungsklappen mit Endlagensensoren
- **Relais-Board** für Motorsteuerung und Türverriegelung
- **Sensoren**:
  - Endlagensensoren für beide Klappen (offen/geschlossen)
  - Magnetkontaktsensoren für Paketzustellertür
  - Briefkastensensoren für Entnahme
  - **Lichtschranke** für Einklemmschutz (GPIO 11)
- **Beleuchtung** für Mülltonne und Paketbox

![Elektronische Komponenten der Paketbox](blueprint/electronic_components.jpg)
*Elektronische Komponenten: Raspberry Pi, Relais-Board, Sensorleitungen und Spannungsversorgung.*

## 🚀 Quick Start

### Installation
1. **Python 3** installieren (3.7+)
2. **Repository klonen**:
   ```bash
   git clone https://github.com/lechmax/paketbox.git
   cd paketbox
   ```
3. **Abhängigkeiten** (optional):
   ```bash
   pip install paho-mqtt  # Für MQTT-Funktionalität
   pip install RPi.GPIO   # Nur auf Raspberry Pi
   ```

### Erste Schritte
```bash
# Lokaler Test (Mock-Modus)
python paketbox.py

# Tests ausführen (empfohlen)
python tests/run_tests.py

```

### Produktive Verwendung
```bash
# Auf Raspberry Pi mit Hardware
python paketbox.py
```

## 🛠️ Entwicklung & Test

### Lokale Entwicklung
```bash
# Mock-Modus für Entwicklung ohne Hardware
python paketbox.py
# Ausgabe: "[MOCK] GPIO setmode(BCM)" zeigt Simulation an

# Tests ausführen (umfassend)
python tests/run_tests.py

# Spezifische Tests
python -m unittest tests.test_paketbox.TestPaketBox.test_Klappen_oeffnen_success -v
```

### Test-Umgebung
Das Projekt enthält eine umfassende Test-Suite:
- **GPIO-Simulation**: Vollständige Hardware-Simulation ohne Raspberry Pi
- **Unit Tests**: Testen einzelne Komponenten und Funktionen
- **Integration Tests**: Testen komplette Arbeitsabläufe
- **Thread-Safety Tests**: Prüfen gleichzeitige Operationen

```bash
# Alle Tests ausführen (dauert ~1-2 Minuten)
PYTHONPATH=. python tests/run_tests.py

# Einzelne Test-Klasse
PYTHONPATH=. python -m unittest tests.test_paketbox.TestPaketBox -v
```

### Logging & Debugging
```bash
# Log-Datei überwachen
tail -f paketbox.log

# Debug-Level erhöhen (in paketbox.py)
logging.basicConfig(level=logging.DEBUG)
```

### Code-Qualität
- **GPIO-Debouncing**: Verhindert Mehrfachauslösung von Sensoren
- **Thread-Safe**: Alle Zustandsänderungen sind thread-sicher implementiert
- **Error Recovery**: Automatische ERROR-States bei Hardware-Fehlern
- **Zentrale Konfiguration**: Alle Parameter in `config.py`
- **Timer-Management**: Sichere Verwaltung von Motor-Timern

## 📁 Projektstruktur

Die Bilder für die Dokumentation liegen im Ordner `blueprint`.

```
max_paket_box/
├── paketbox.py              # Hauptsteuerung (Version 0.7.0)
├── handler.py               # Handler-Funktionen für GPIO und Motoren
├── state.py                 # Zentrale Zustandsverwaltung
├── config.py                # Konfiguration und GPIO-Pin-Zuordnungen
├── PaketBoxState.py         # Zustandsklassen (Door/Motor States)
├── TimerManager.py          # Timer-Verwaltung für Motoren
├── mqtt.py                  # MQTT-Integration für IoT-Benachrichtigungen
├── tests/
│   ├── test_paketbox.py     # Umfassende Unit Tests
   ... (rest of content continues same as original)

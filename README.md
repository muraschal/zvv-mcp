# ZVV-MCP Projekt

## Übersicht
ZVV-MCP ist eine Anwendung, die Spice.ai mit Daten von opendata.swiss kombiniert, um öffentliche Verkehrsdaten des Zürcher Verkehrsverbunds (ZVV) mittels LLM abfragbar zu machen.

## Features
- **Spice.ai Integration**: Beschleunigte Datenabfragen für effiziente Verarbeitung großer Verkehrsdatensätze
- **opendata.swiss Anbindung**: Zugriff auf offizielle ZVV-Daten
- **LLM-Abfragen**: Natürlichsprachliche Abfrage von Verkehrsinformationen
- **Responsive Design**: Optimale Nutzung auf allen Geräten

## Projektstruktur
- `src/`: Quellcode der Anwendung
  - `components/`: Wiederverwendbare UI-Komponenten
  - `pages/`: Next.js-Seiten und API-Routes
  - `styles/`: Globale CSS-Stile
- `public/`: Statische Dateien
- `docs/`: (geplant) Dokumentation
- `zvv-data/`: Spice.ai Konfiguration und Daten

## Technische Architektur

### Systemkomponenten
Die Anwendung besteht aus drei Hauptkomponenten:
1. **Next.js Frontend**: React-basierte Benutzeroberfläche
2. **Spice.ai Engine**: Verarbeitet und analysiert Verkehrsdaten
3. **OpenAI Service**: Stellt LLM-Funktionalitäten für natürlichsprachliche Abfragen bereit

### Datenfluss

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌──────────────┐
│ GTFS-Daten  │───>│ Spice.ai    │<───│ Next.js     │<───│ Benutzer-    │
│ (opendata)  │    │ Engine      │    │ Frontend    │    │ anfrage      │
└─────────────┘    └──────┬──────┘    └──────┬──────┘    └──────────────┘
                          │                   │
                          │                   │
                          │                   ▼
                          │           ┌─────────────┐
                          └──────────>│ OpenAI LLM  │
                                      │ (GPT-4o)    │
                                      └─────────────┘
```

### Sequenzdiagramm für eine Benutzeranfrage

```
┌─────────┐          ┌─────────┐          ┌──────────┐          ┌────────┐
│ Benutzer│          │ Frontend│          │ OpenAI   │          │Spice.ai│
└────┬────┘          └────┬────┘          └─────┬────┘          └────┬───┘
     │                     │                     │                    │
     │ Stellt Frage        │                     │                    │
     │ zu ZVV-Linien       │                     │                    │
     │────────────────────>│                     │                    │
     │                     │                     │                    │
     │                     │ Verbindungsstatus   │                    │
     │                     │ abfragen            │                    │
     │                     │───────────────────────────────────────────>
     │                     │                     │                    │
     │                     │ Daten-Metadaten     │                    │
     │                     │<───────────────────────────────────────────
     │                     │                     │                    │
     │                     │ LLM-Anfrage mit     │                    │
     │                     │ Kontext & Frage     │                    │
     │                     │────────────────────>│                    │
     │                     │                     │                    │
     │                     │ LLM-Antwort         │                    │
     │                     │<────────────────────│                    │
     │                     │                     │                    │
     │ Antwort anzeigen    │                     │                    │
     │<────────────────────│                     │                    │
     │                     │                     │                    │
┌────┴────┐          ┌────┴────┐          ┌─────┴────┐          ┌────┴───┐
│ Benutzer│          │ Frontend│          │ OpenAI   │          │Spice.ai│
└─────────┘          └─────────┘          └──────────┘          └────────┘
```

## Installation

### Voraussetzungen
- Node.js (>= 14.x)
- npm oder yarn
- Spice.ai CLI (siehe offizielle Dokumentation)
- Docker (optional, für Containerisierung)

### Setup
```bash
# Projektabhängigkeiten installieren
npm install

# Spice.ai installieren (siehe offizielle Dokumentation für Details)
curl -fsSL https://raw.githubusercontent.com/spiceai/spiceai/trunk/install/install.sh | bash

# Spice.ai Projekt initialisieren
spice init zvv-data
```

## Entwicklung
```bash
# Next.js Entwicklungsserver starten
npm run dev

# In einem separaten Terminal: Spice.ai Runtime starten
cd zvv-data
spice run
```

## Datenmodell

Die Anwendung verwendet GTFS-Daten (General Transit Feed Specification) mit folgenden Hauptdatensets:

| Dataset | Beschreibung | Anzahl Einträge |
|---------|--------------|----------------|
| zvv_stops | Alle Haltestellen im ZVV-Netz | ~5.791 |
| zvv_routes | Alle Linien im ZVV-Netz | ~383 |
| zvv_trips | Alle Fahrten auf den Linien | ~205.526 |
| zvv_stop_times | Fahrplanzeiten | ~3.456.460 |
| zvv_calendar | Betriebstage | ~1.695 |
| zvv_transfers | Umsteigeverbindungen | ~10.342 |

## Best Practices

### Entwicklung
- **Komponenten-Design**: Funktionale React-Komponenten mit Hooks verwenden
- **Typisierung**: TypeScript für statische Typprüfung und bessere IDE-Unterstützung
- **State Management**: React Context für geteilten Zustand zwischen Komponenten
- **CSS-Organisation**: Tailwind-Klassen für konsistentes Design

### Datenverarbeitung
- **Daten-Caching**: Spice.ai Beschleunigung für schnelle Abfragen großer Datensätze
- **Inkrementelles Laden**: Nur benötigte Daten laden, um Leistung zu optimieren
- **Error Handling**: Robuste Fehlerbehandlung für API-Aufrufe und Datenverarbeitung

### LLM-Integration
- **Prompt Engineering**: Präzise System-Prompts für genaue Antworten
- **Kontext-Management**: Relevante Daten ins LLM-Prompt aufnehmen
- **Antwort-Parsing**: Strukturierte Verarbeitung der LLM-Antworten

## Produktion
```bash
npm run build
npm start
```

## Dokumentation
Die ausführliche Dokumentation wird derzeit vorbereitet.

## Technologie-Stack
- **Frontend**: Next.js, React, TypeScript, Tailwind CSS
- **Datenverarbeitung**: Spice.ai
- **Datenquellen**: opendata.swiss (GTFS-Daten)
- **KI-Modelle**: OpenAI GPT-4o für natürlichsprachliche Abfragen 

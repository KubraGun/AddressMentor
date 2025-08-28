# NOVAERA – AddressMentor  
**AI Agent-Based Address Resolution System**  
*Developed for TEKNOFEST 2025 – Hepsiburada Address Resolution Hackathon*  

---

## 🚀 Overview
**AddressMentor** is an intelligent, modular pipeline designed to solve one of the most critical challenges in logistics and e-commerce: **messy and incomplete address data**.  

Our system uses **AI Agents**, **Natural Language Processing (NLP)**, and **Geographic Information Systems (GIS)** to normalize, complete, and geocode free-text addresses, reducing last-mile delivery errors and improving operational efficiency.  

This project is developed under **Team NOVAERA** for the **Hepsiburada Address Resolution Hackathon at TEKNOFEST 2025**.  
All architecture, design, and core idea belong to **Team Captain: Ayşe Kübra Gün**.

---

## 🧩 Core Architecture

### Core AI Agents:contentReference[oaicite:0]{index=0}
- **AddressCleanerAgent** → Cleans raw text (spelling, punctuation, encoding, Turkish character restoration, abbreviation expansion).  
- **GeoCoderAgent** → Maps normalized addresses to precise geographic coordinates (lat/lon).  
- **SuggestionAgent** → Completes missing fields and suggests candidate addresses.  

### Extended Sub-Agent Mapping:contentReference[oaicite:1]{index=1}
- **AddressCleanerAgent**  
  - InputNormalizerAgent  
  - StructuralParserAgent  
  - AbbreviationNormalizerAgent  
  - OCRCorrectionAgent  
  - MultilingualAgent  

- **GeoCoderAgent**  
  - GeoDisambiguationAgent  
  - GeocodingAgent (Nominatim primary, Google fallback)  
  - IndoorMappingAgent  
  - EntranceDisambiguationAgent  
  - SatelliteDiscoveryAgent  

- **SuggestionAgent**  
  - CompletionAgent  
  - SemanticMatchingAgent  
  - ValidationAgent  
  - ContactUpdateAgent  

---

## 🌍 Geocoding Strategy

We adopt a **Nominatim + Cache + Fallback** architecture:contentReference[oaicite:2]{index=2}:

1. **Clean First, Geocode Second** → Always geocode normalized/expanded text.  
2. **Disambiguation** → Resolve ambiguous neighborhood/street names before geocoding.  
3. **Caching** → Redis/SQLite caching to handle rate limits and speed.  
4. **Fallbacks** → Use Google Geocoding API if Nominatim fails.  
5. **Validation** → Cross-check with OSM/municipality data to ensure accuracy.  

```yaml
# configs/postprocess/geocoding.yaml
provider:
  primary: nominatim
  fallbacks: [google]
thresholds:
  min_confidence: 0.75
cache:
  backend: redis
  ttl_hours: 168

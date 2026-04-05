# Costco-Preis-Tracker

[![Bright Data](https://img.shields.io/badge/Powered%20by-Bright%20Data-blue?style=flat-square)](https://brightdata.de)
[![Costco Price Tracker](https://img.shields.io/badge/Costco%20Price%20Tracker-Managed%20Solution-orange?style=flat-square)](https://brightdata.de/products/insights/price-tracker/costco)
[![Python](https://img.shields.io/badge/Python-3.9%2B-yellow?style=flat-square)](https://python.org)

[![Bright Insights Price Tracker](https://raw.githubusercontent.com/danielshashko/bright-insights-assets/main/price-tracker-hero-v2.png)](https://brightdata.de/products/insights/price-tracker/costco)

Echtzeit-Preisverfolgung für Costco – eine Einzelhandelskette von Warehouse Clubs nur für Mitglieder. Zwei Möglichkeiten für den Einstieg: eine **vollständig verwaltete** Intelligence-Plattform oder eine **Self-Service-API**, um Ihre eigene Pipeline zu erstellen.

---

## Option 1: Bright Insights - KI-gestützte Preisverfolgung (Empfohlen)

**[Bright Insights](https://brightdata.de/products/insights/price-tracker/costco)** ist die vollständig verwaltete Retail-Intelligence-Plattform von Bright Data. Keine Scraper zu erstellen, keine Infrastruktur zu warten – nur strukturierte, analysebereite Preisdaten, die an Dashboards, Data Feeds oder Ihre BI-Tools geliefert werden.

**Warum Teams Bright Insights wählen:**
- 🚀 **Kein Setup** – In wenigen Minuten live mit sofort einsatzbereiten Dashboards und Data Feeds
- 🤖 **KI-gestützte Empfehlungen** – Ein konversationeller KI-Assistent verwandelt Millionen von Datenpunkten sofort in umsetzbare Erkenntnisse
- ⚡ **Echtzeit-Monitoring** – Stündliche bis tägliche Aktualisierungsraten mit sofortigen Alerts (E-Mail, Slack, webhook)
- 🌍 **Unbegrenzte Skalierung** – Jede Website, jede Geografie, jede Aktualisierungsfrequenz
- 🔗 **Plug-and-play-Integrationen** – AWS, GCP, Databricks, Snowflake und mehr
- 🛡️ **Vollständig verwaltet** – Bright Data übernimmt Schemaänderungen, Website-Updates und Datenqualität automatisch

**Wichtige Anwendungsfälle:**
- ✅ **Costco-Preise überwachen** über alle Produktkategorien hinweg
- ✅ **Lagerbestände und Verfügbarkeit verfolgen** in Echtzeit
- ✅ **Preisalarme einrichten** für Produkte, die für Sie wichtig sind
- ✅ Einhaltung der MAP-Richtlinie überwachen und Preisverstöße erkennen
- ✅ Wettbewerber-Promotions und Promotionsdynamiken verfolgen
- ✅ Saubere, harmonisierte Daten direkt in dynamische Preisalgorithmen oder KI-Modelle einspeisen

> **Ab $250/Monat – [Individuelles Angebot anfordern →](https://brightdata.de/products/insights/price-tracker/costco)**

---

## Option 2: Self-Service über Web Scraper API

Möchten Sie lieber Ihre eigene Pipeline erstellen? Die **Web Scraper API** von Bright Data gibt Ihnen programmatischen Zugriff auf Costco-Produktdaten – Preise, Verfügbarkeit, Bewertungen und mehr – ohne dass Sie Proxys oder Scraping-Infrastruktur verwalten müssen.

### Voraussetzungen

- Python 3.9 oder höher
- Ein [Bright Data account](https://brightdata.de) (kostenlose Testversion verfügbar)
- Ein Bright Data **API token** ([so erhalten Sie einen](https://docs.brightdata.de/general/account/account-settings#api-token))
- Eine Bright Data **Web Scraper ID** für Costco-Produkte ([finden Sie Ihre im Web Scrapers Control Panel](https://brightdata.de/cp/scrapers))

### Setup

1. **Dieses repository klonen**

   ```bash
   git clone https://github.com/bright-data-de/costco-price-tracker.git
   cd costco-price-tracker
   ```

2. **Abhängigkeiten installieren**

   ```bash
   pip install -r requirements.txt
   ```

3. **Zugangsdaten konfigurieren**

   Kopieren Sie `.env.example` nach `.env` und tragen Sie Ihre Werte ein:

   ```bash
   cp .env.example .env
   ```

   ```env
   BRIGHTDATA_API_TOKEN=your_api_token_here
   BRIGHTDATA_DATASET_ID=your_dataset_id_here
   ```

   > **Ihre Web Scraper ID finden**
   > Melden Sie sich im [Bright Data Control Panel](https://brightdata.de/cp/scrapers) an, navigieren Sie zu **Web Scrapers**,
   > suchen Sie nach "Costco" und kopieren Sie die Web Scraper ID (Format: `gd_xxxxxxxxxxxx`).

---

## Verwendung

### 1. Bestimmte Produkte per URL verfolgen

Übergeben Sie eine Liste von Costco-Produkt-URLs, um strukturierte Preisdaten abzurufen:

```python
from price_tracker import track_prices

urls = [
    "https://www.costco.com/sample-product.product.1234567.html",
    # Add more product URLs here
]

results = track_prices(urls)
for item in results:
    print(f"{item.get('title')} - {item.get('final_price', item.get('price'))} {item.get('currency', '')}")
```

Oder direkt ausführen:

```bash
python price_tracker.py
```

### 2. Produkte per Keyword entdecken

Finden Sie Produkte, die einer Keyword-Suche entsprechen:

```python
from price_tracker import discover_by_keyword

results = discover_by_keyword("laptop", limit=50)
```

### 3. Produkte per Kategorie-URL durchsuchen

Sammeln Sie alle Produkte von einer Costco-Kategorieseite:

```python
from price_tracker import discover_by_category

results = discover_by_category(
    "https://costco.com/category/example",
    limit=100,
)
```

---

## Ausgabefelder

Jeder Ergebnisdatensatz enthält die folgenden Felder:

| Field | Description |
|-------|-------------|
| `url` | URL der Produktseite |
| `title` | Produktname / Titel |
| `brand` | Marke oder Hersteller |
| `initial_price` | Ursprünglicher / Listenpreis |
| `final_price` | Aktueller Verkaufspreis |
| `currency` | Währungscode (z. B. USD, EUR) |
| `discount` | Rabattbetrag oder -prozentsatz |
| `in_stock` | Ob der Artikel verfügbar ist |
| `rating` | Durchschnittliche Sternebewertung |
| `reviews_count` | Gesamtzahl der Bewertungen |
| `seller_name` | Name des Verkäufers |
| `images` | Array von Produktbild-URLs |
| `description` | Produktbeschreibungstext |
| `timestamp` | Zeitstempel der Datenerfassung |

### Beispielausgabe

```json
[
  {
    "url": "https://www.costco.com/sample-product.product.1234567.html",
    "title": "Example Product Name",
    "brand": "Example Brand",
    "initial_price": 59.99,
    "final_price": 44.99,
    "currency": "USD",
    "discount": "25%",
    "in_stock": true,
    "rating": 4.5,
    "reviews_count": 1234,
    "images": ["https://costco.com/images/product1.jpg"],
    "description": "Product description text...",
    "timestamp": "2025-01-15T10:30:00Z"
  }
]
```

---

## Erweiterte Optionen

Die Funktion `trigger_collection()` akzeptiert optionale Parameter zur Steuerung der Datenerfassung:

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `limit` | integer | - | Maximale Anzahl der zurückzugebenden Datensätze |
| `include_errors` | boolean | `true` | Fehlerberichte in die Ergebnisse einschließen |
| `notify` | string (URL) | - | Webhook-URL, die aufgerufen wird, wenn der Snapshot bereit ist |
| `format` | string | `json` | Ausgabeformat: `json`, `csv` oder `ndjson` |

Beispiel mit Optionen:

```python
from price_tracker import trigger_collection, get_results

inputs = [{"url": "https://www.costco.com/sample-product.product.1234567.html"}]
snapshot_id = trigger_collection(inputs, limit=200, notify="https://your-webhook.com/hook")
results = get_results(snapshot_id)
```

---

## Ressourcen

- 🌟 [Costco Price Tracker - Bright Insights (Managed)](https://brightdata.de/products/insights/price-tracker/costco)
- 🔧 [Costco Scraper API](https://brightdata.de/products/web-scraper/costco)
- 📖 [Bright Data Web Scraper API Documentation](https://docs.brightdata.de/scraping-automation/web-scraper-api/overview)
- 🗄️ [Web Scrapers Control Panel](https://brightdata.de/cp/scrapers)
- 🔑 [How to get an API token](https://docs.brightdata.de/general/account/account-settings#api-token)
- 🌐 [Bright Data Homepage](https://brightdata.de)

---

*Erstellt mit [Bright Data](https://brightdata.de) – der branchenführenden Plattform für Webdaten.*
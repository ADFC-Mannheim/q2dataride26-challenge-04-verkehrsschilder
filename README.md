# Hackathon Challenge 4: Analyse Verkehrsschilder-Kataster Mannheim & BW

## Fragestellung

Wie nutzen Mannheim und andere Kommunen in Baden-Württemberg Verkehrszeichen zur
Verkehrsberuhigung oder für Geschwindigkeitseinschränkungen (z. B. Tempo 30)?

- Welche Schildertypen werden wo eingesetzt?
- Gibt es Unterschiede zwischen Kommunen – z. B. abhängig von Größe oder Einwohnerzahl?
- Lassen sich räumliche Muster erkennen (z. B. Häufung von Tempo-30-Zonen in Wohngebieten)?

## Datengrundlage

### Verkehrsschilder-Kataster

Die Schilderdaten liegen als GeoJSON-Datei vor und sind direkt mit `geopandas` ladbar:

```python
import geopandas as gpd

gdf = gpd.read_file("Hackathon_VZ.geojson")

# Nicht notwendig aber nützlich um den Aufbau sehen zu können
gdf.head()
```

Ein Weg die Schilder grafisch auszugeben:

```python
import matplotlib.pyplot as plt
import contextily as ctx

ax = gdf[gdf["Verwaltungsebene"] == "Mannheim"].plot(figsize=(10, 8), alpha=0.5, edgecolor="red")
ctx.add_basemap(ax, source=ctx.providers.OpenStreetMap.Mapnik)

plt.show()
```

### OpenStreetMaps

Für OpenStreetMap lohnt sich evtl. ein Blick auf [Overpass](https://wiki.openstreetmap.org/wiki/DE:Overpass_API).

Browsertool: https://overpass-turbo.eu/

### Einwohnerdaten

Diese lassen sich bspw. für Baden Württemberg über das statistische Landesamt abrufen:

https://daten.statistik-bw.de/genesisonline/online/


### Grafana-Dashboard

Zur schnellen visuellen Exploration steht ein Grafana-Dashboard zur Verfügung, hier sind sowohl die Verkehrszeichen als auch die Einwohnerzahlen bereits importiert:

[Grafana](https://monitor.quadradentscheid.de/)

[Beispiel Dashboard mit Karte](https://monitor.quadradentscheid.de/d/adg4t5p/verkehrszeichen?orgId=1&from=now-6h&to=now&timezone=Europe%2FBerlin&var-verkehrszeichen=$__all&var-verwaltungsebene=Mannheim)

> **Hinweis:** Anmeldedaten werden vor Ort bereitgestellt.

## Aufgabenstellung

### 1. Datenexploration

- Welche Verkehrszeichen-Typen sind im Kataster enthalten?
- Wie vollständig sind die Daten (fehlende Werte, Abdeckung je Kommune)?
- Räumliche Verteilung der Schilder auf einer Karte darstellen

### 2. Analyse Geschwindigkeitseinschränkungen

- Anteil und Verteilung von Tempo-30-Schildern, Tempo-30-Zonen und verkehrsberuhigten
  Bereichen je Kommune
- Vergleich: Welche Kommunen setzen besonders viel oder wenig auf Tempo 30?
- ggf. Normalisierung auf Einwohnerzahl oder Straßenlänge für einen fairen Vergleich

### 3. Vergleich zwischen Kommunen

- Gibt es Cluster ähnlicher Kommunen nach Schilderverteilung?
- Zusammenhang mit Einwohnerzahl: Skaliert der Einsatz von Verkehrsberuhigung
  mit der Stadtgröße?

### 4. Visualisierung & Präsentation

Beispielsweise:
- Karte der Schilderstandorte, einfärbbar nach Typ (z. B. Tempo 30, Spielstraße,
  Einbahnstraße)
- Balkendiagramm: Anzahl Schilder je Typ und Kommune (normalisiert)
- Scatter-Plot: Einwohnerzahl vs. Anteil Tempo-30-Schilder

## Mögliche Zusatzdaten

| Datenquelle | Inhalt | Link |
|---|---|---|
| Einwohnerdaten Statistik BW | Bevölkerungsdaten je Kommune (auch in Grafana verfügbar) | [daten.statistik-bw.de](https://daten.statistik-bw.de/genesisonline/online?operation=ergebnistabelleUmfang&levelindex=3&levelid=1775053405020&downloadname=12111_0005#abreadcrumb) |
| OpenStreetMap | Straßennetz und Straßenlängen je Kommune | [openstreetmap.org](https://openstreetmap.org) |
| Verkehrszeichen | Liste aller Zeichen mit Nummer und Bezeichnung | [bast.de](https://www.bast.de/DE/Themen/Sicherheit/HF_2/Massnahmen/verkehrszeichen/vz-download.html?nn=401752) |

---

Viel Erfolg beim Hackathon! 🚲

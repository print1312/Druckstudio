# Niimbot Druckstudio V18

Web-App zum Gestalten und Drucken von Etiketten auf Niimbot-Druckern über Web Bluetooth – für iPhone (in der App **Bluefy**) und Laptop (Chrome oder Edge).

## Installation auf GitHub Pages

1. Das ZIP **entpacken**.
2. Auf GitHub ein Repository öffnen bzw. neu anlegen (**Public**).
3. `Add file` → `Upload files`. **Nur die Dateien aus dem entpackten Ordner** in die Uploadfläche ziehen – nicht den Ordner selbst und nicht das ZIP. Vorhandene Dateien mit gleichem Namen werden dabei ersetzt.
4. `Commit changes`.
5. Einmalig: `Settings` → `Pages` → `Deploy from a branch` → Branch `main`, Ordner `/(root)` → `Save`.
6. Nach 1–2 Minuten ist die App unter `https://DEINNAME.github.io/REPOSITORY/` erreichbar.
7. iPhone: Adresse in **Bluefy** öffnen (Safari/Chrome auf iOS können kein Bluetooth). Laptop: Chrome oder Edge.

Alle Dateien müssen direkt in der obersten Ebene liegen:

```
index.html          addon.js            niimbot-demo.html
niimbot.js          label-memory.js     label-size.js
registry.json       pdf.min.js          pdf.worker.min.js
README.md           PRUEFBERICHT.md     LIZENZEN.md
NIIMBOT-LICENSE.txt PDFJS-LICENSE.txt
```

Wenn das Update nicht sofort erscheint: Seite einmal neu laden (in Bluefy ggf. Tab schließen und neu öffnen).

## Bedienung in Kürze

- **Oben – Drucker:** `🔗 Verbinden` öffnet die Bluetooth-Auswahl. Modell wird automatisch erkannt. Über `⚙ Drucker` lassen sich Modell, Etikettgröße und Dichte ändern. Auf dem iPhone klappt die Leiste nach dem Verbinden zusammen, damit die Vorschau größer ist.
- **Mitte – Vorschau:** Der rote Rahmen ist der Druckbereich. Elemente antippen und ziehen; mit zwei Fingern zoomen und drehen. `− 100% +` ändert nur die Ansichtsgröße (auf die Prozentzahl tippen = 100 %).
  - Importierte Bilder erscheinen **unverändert** (Farbe, volle Auflösung) und werden – wie in den ersten App-Versionen – unverändert an den Niimbot-Treiber übergeben. Erst wenn du im Tab „Bild“ einen anderen Modus wählst, zeigt die Vorschau das umgewandelte Druckbild.
  - Kopfzeile: **Schwelle** (Schwarzweiß: höher = mehr Schwarz; Foto: höher = dunkler; 128 = neutral), **↻ 90°** dreht den Druckbereich – auch schon vor dem ersten Bild, neue Etiketten übernehmen die Ausrichtung.
  - **⬚ Außen** zeigt Teile außerhalb des Druckbereichs blass an.
  - `↶` / `↷` = Rückgängig / Wiederholen.
- **Unten – Werkzeuge** (Tabs): Dateien, Text, Rundtext, Bild, Ebenen, Position, Vorlagen. Auf dem iPhone lässt sich die Höhe am Griff ziehen oder mit Klein/Mittel/Groß wählen. Auf dem Laptop stehen die Werkzeuge rechts neben der Vorschau.
- **Leiste ganz unten:** `💾 Speichern` (als Vorlage), `🖨️ Drucken`, `🏷 Rollen` (Rollen-Chip lesen, eigene Etikettgrößen anlegen), `⋯` (Druckerstatus, Protokoll).

### Bilder richtig drucken: Druckmodus & Zeichenstil

Im Tab **Bild** gibt es eine gemeinsame Auswahl für das ausgewählte Bild:

**Darstellung & Druckmodus**
- **🎨 Original** (Standard nach dem Import): Bild bleibt unverändert, in Vorschau und beim Senden an den Drucker. Der Niimbot-Treiber druckt dann – wie bisher – alles, was dunkler als 50 % ist, schwarz.
- **⚫ Schwarzweiß:** für Logos, Text, Grafiken. Die Grenze zwischen Schwarz und Weiß stellst du mit **Schwelle** oben in der Vorschau-Kopfzeile ein (höher = mehr Schwarz).
- **🖼️ Foto:** Graustufen als feine Druckpunkte, Helligkeit und Look wie im Original – ohne automatische Kontrast- oder Schärfeänderung. Mit **Schwelle** heller/dunkler.

Die App verändert ein Bild nur, wenn du selbst einen anderen Modus, einen Zeichenstil, die Regler unter „Bild bearbeiten“ oder die Schwelle verwendest. Verschiebst du die Schwelle bei „Original“, wechselt die Darstellung auf „Schwarzweiß“, damit du die Wirkung siehst.

**Als Zeichnung** (wandelt das Foto in Linien um – druckt am klarsten)

| Stil | Wirkung | geeignet für |
|---|---|---|
| ✏️ Bleistift | klare Konturen mit leichten Bleistiftstrichen | Porträts, Gebäude, Gegenstände |
| 🖌️ Weich | weiche Skizze mit heller Schattierung | Porträts, Tiere, Pflanzen |
| 💥 Comic | kräftige Linien, dunkle Flächen schwarz | Figuren, Tiere, Gegenstände |
| 〰️ Umrisse | nur die Konturen des Motivs | Räume, Möbel, Technik |
| ▤ Schraffur | Schatten als Linienraster (Kupferstich) | Landschaften, Architektur |

Regler (mit − / +): **Linienstärke**, **Details** (wie viel vom Bild zur Linie wird), **Schattierung** (nur Linien ↔ viel Fläche), **Vereinfachen** (glättet Hautporen, Laub, Rauschen, damit das Motiv klar heraustritt). „Stil-Standardwerte“ setzt sie zurück.

Die Bildauflösung wird nur verändert, wenn du im Tab **Dateien** bei „Import-Pixeldichte“ 300 oder 201 dpi wählst. Bei „Original“ wird das Bild unverändert übernommen und – wie in den ersten Versionen – erst beim Drucken auf die Pixel des Etiketts gebracht.

### Etikettgröße wechseln

Beim Wechsel der Etikettgröße werden alle Texte und Bilder proportional angepasst und bleiben bearbeitbar. Jede Größe merkt sich ihre eigene Anordnung – beim Zurückwechseln ist alles wieder genau wie vorher. Rückgängig funktioniert auch hier.

### Vorlagen (Tab „💾 Vorlagen“)

- **Speichern:** Namen eingeben, unter **Ordner** einen vorhandenen Ordner wählen oder „＋ Neuer Ordner …“ anlegen, dann `💾 Speichern`. Ist das Etikett bereits eine Vorlage, wird sie aktualisiert (auch umbenennen). `Als Kopie speichern` legt eine zusätzliche Vorlage an. `💾 Speichern` unten in der Leiste macht dasselbe.
- **Ordner:** Die Knöpfe über der Liste filtern nach Ordner („Alle“, „📁 Küche (3)“ …). In jeder Karte lässt sich die Vorlage per Auswahlliste in einen anderen Ordner verschieben.
- **Laden:** `📂 Öffnen` lädt die Vorlage zum Bearbeiten (ein leeres Etikett wird ersetzt, eine bereits geöffnete Vorlage nur angezeigt – keine Doppelten). `⧉ Als Kopie` öffnet sie als neues Etikett, das Original bleibt unverändert. `➕ Warteschlange` legt sie zusätzlich zum Drucken bereit, `🖨 Drucken` druckt direkt.
- Die gerade bearbeitete Vorlage ist blau umrandet („in Bearbeitung“).
- Vorlagen liegen nur in diesem Browser auf diesem Gerät. Zum Sichern oder für ein anderes Gerät `↓ Datei` bzw. `↓ Als Datei` und dort `↑ Datei öffnen`. Bei anderer Etikettgröße wird eine Vorlage proportional angepasst; das gespeicherte Original bleibt unverändert.

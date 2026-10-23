# Prüfbericht V18 – 23. Oktober 2026

Grundlage: V17 („Mehrbild, Ebenen, Rahmen“). Der gesamte Code (`index.html`, `addon.js`, `niimbot-demo.html`, Treiber-Schnittstelle in `niimbot.js`) wurde durchgesehen und alle Funktionen wurden im Browser durchgetestet – mit iPhone-Größe (390 × 664 und 375 × 553, Touch) und Laptop-Größe (1366 × 768 und 1280 × 720).

## Gefundene Fehler und Korrekturen

| # | Problem in V17 | Korrektur in V18 |
|---|---|---|
| 1 | **Vorschau ≠ Druck.** Der Treiber druckt jeden Pixel mit Helligkeit < 50 % schwarz, alles andere weiß. Die Vorschau zeigte aber Farben/Graustufen. Ein farbiges Foto wurde z. B. als schwarze Fläche gedruckt, Text darauf verschwand. | Die Vorschau zeigt jetzt immer exakt das Druckbild in Schwarz/Weiß. Pro Bild wählbarer Druckmodus, für Fotos eine eigene Rasterung (berechnet in Druckauflösung). JPG-Fotos nutzen sie automatisch. Der Drucker bekommt dasselbe 1-Bit-Bild wie die Vorschau. |
| 2 | Bedienmodus „Graustufen“ / „Hartes Schwarzweiß“ wirkte nur auf die Vorschau bzw. falsch skaliert (Schwelle in Originalauflösung). Der Schwellen-Regler im Tab „Bild“ war immer aktiv, wirkte aber nur in einem Modus. | Druckmodi Schwarzweiß und Foto (plus Zeichenstile). Die **Schwelle** sitzt in der Vorschau-Kopfzeile direkt neben dem Etikettnamen und wirkt in Druckauflösung auf das ausgewählte Bild. Alte Vorlagen werden weiter korrekt gedruckt. |
| 3 | **Laptop:** Werkzeuge unter der Vorschau, sichtbar waren nur ~100 px – kaum bedienbar. | Ab 900 px Breite: Vorschau links, Werkzeuge rechts in voller Höhe. |
| 4 | **iPhone:** Werkzeugbereich nur ~70 px hoch, Auswahllisten abgeschnitten („Niimbot“, „3 (mode“), Statuszeile halb abgeschnitten, Zoom-Schieber senkrecht (auf älteren iOS-Versionen nicht verlässlich). | Druckerleiste klappt nach dem Verbinden auf eine Zeile zusammen (`⚙ Drucker` öffnet sie), Werkzeugbereich ca. halbe Höhe und verstellbar, Zoom als `− 100% +` in der Vorschau, Statuszeile einzeilig. |
| 5 | Zuschneide-Dialog auf dem iPhone: Knöpfe „Zurücksetzen/Übernehmen“ abgeschnitten. | Dialog passt in den Bildschirm, Knöpfe in einer Zeile. |
| 6 | **Größenwechsel:** Texte blieben an alten Pixelpositionen (z. B. außerhalb des neuen Etiketts), Bilder wurden neu eingepasst; Vorlagen wurden in ein nicht mehr bearbeitbares Gesamtbild umgewandelt. | Alles wird proportional angepasst und bleibt bearbeitbar. Jede Etikettgröße merkt sich ihre Anordnung, Hin- und Herwechseln ist verlustfrei. |
| 7 | Beim Import fehlerhafter Dateien wurde der Dateiname von der Zusammenfassung überschrieben. | Meldung nennt jede übersprungene Datei mit Grund. WebP/GIF/HEIC (falls vom Browser lesbar) werden als PNG übernommen. |
| 8 | Import-Pixeldichte „201 dpi“ (Niimbot-Geräte haben 203 dpi). | 203 dpi; alte Werte werden weiter korrekt gelesen. |
| 9 | Prüffehler vor dem Druck (nicht verbunden, falsches Modell) zeigten „Bitte Papier prüfen; ein Etikett kann bereits ausgegeben worden sein“. | Klare Meldung ohne Papier-Hinweis. |
| 10 | „💾 Speichern“ unten öffnete nur den Vorlagen-Tab. | Speichert direkt als Vorlage. |
| 11 | Ebenenliste zeigte nach Textänderung noch den alten Text. | Wird sofort aktualisiert. |
| 12 | Rundtext erzeugte einen Kreis größer als das Etikett. | Startgröße passt ins Etikett. |
| 13 | Tab „Bild“ mit ausgewähltem Text zeigte „Aktiv: Textfeld“, Tab „Rundtext“ war ohne Auswahl komplett gesperrt. | Tabs wählen automatisch das passende Element. |
| 14 | Teile der Original-Demo englisch (Rollen, Protokoll, Statusmeldungen, „model default“, „cable flag“). | Auf Deutsch übersetzt. |
| 15 | Warteschlange zeigte nur „IMG/PDF“. | Kleine Druckbild-Vorschau, Anzahl Bilder/Texte, Zeile antippen = auswählen. |

## Neu: Druckmodus & Zeichenstil, saubere Fotos

- **Fotos sehen aus wie das Original:** Die Zwischenversion hob Kontrast und Schärfe automatisch an und rasterte grob – das veränderte den Look (dunkle Hintergründe wurden schwarz, helle Bereiche weiß, sichtbare Punkte). Jetzt: tongetreue Rasterung (Jarvis-Judice-Ninke) ohne automatische Veränderungen. Gemessen am Porträt: mittlere Helligkeitsabweichung zwischen Druckbild (aus Betrachtungsabstand) und Original 1,8 von 255, Helligkeitsverschiebung −0,1. Die Vorschau zeigt Fotobereiche so, wie die feinen Druckpunkte auf dem Etikett wirken (als Graustufen), Text/Linien/Logos bleiben scharf. Foto oder Grafik wird beim Import am Bildinhalt erkannt, nicht am Dateityp.
- **Schwelle immer bedienbar:** War bei Fotos gesperrt. Jetzt wirkt sie bei Fotos als Helligkeit (128 = unverändert), bei Schwarzweiß als Schwelle. Getestet mit Maus-Ziehen, Pfeiltasten, Antippen und Touch-Ziehen.
- **↻ 90° in der Vorschau-Kopfzeile:** dreht den Druckbereich, auch ohne Datei; neue Etiketten übernehmen die Ausrichtung.
- **„Druckansicht/Original“ entfernt:** Die Vorschau zeigt immer das Druckergebnis, auch bei „⬚ Außen“ (Teile außerhalb des Druckbereichs werden genauso berechnet und nur abgeblendet). Vergrößerte Ansichten zeigen gleichmäßig große Druckpunkte (kein Moiré).
- **Eine Auswahl im Tab „Bild“:** Schwarzweiß, Foto und die fünf Zeichenstile (Bleistift, Weich, Comic, Umrisse, Schraffur) stehen gemeinsam als Knöpfe zur Wahl; die alte Auswahlliste entfällt.
- **Zeichenstile überarbeitet:** kantenerhaltende Vereinfachung (Kuwahara-Filter) vor der Linienerkennung, zweistufige Konturerkennung mit Hysterese (durchgehende statt gestrichelte Linien, Motiv tritt hervor, Hintergrundstruktur wird ruhiger), Entfernen kleiner Störflecken, geglättete Strichkanten. Berechnung in Druckauflösung, ca. 0,1 s pro Etikett.
- **Auflösung:** Bilder werden nur bei gewählter Import-Pixeldichte (300/203 dpi) umgerechnet. PDF-Seiten werden bei „Original“ mit 300 dpi statt 144 dpi gerendert (schärfer).

Getestet mit 9 Motiven aus 6 Bereichen (2 Porträts, 2 Landschaften/Architektur, 1 Wohnraum – künstlich erzeugtes Testbild, 2 Gegenstände, 1 Tier, 1 Pflanze) in allen Modi als echtes 1-Bit-Druckbild.

## Automatisierte Tests (Chromium, Playwright, simulierter Drucker)

Alle Tests bestanden: **202 von 202** Prüfungen in 12 Durchläufen (iPhone 390 px, iPhone SE 375 px, Laptop 1366 px).

Schwellen-Test: Anteil schwarzer Pixel im Druckbild bei Schwelle 60 / 128 / 200 = 1,5 % / 14,6 % / 42 % – Vorschau und Druckbild ändern sich gleich, Druckbild bleibt reines Schwarz/Weiß, +/−, Rückgängig und eigene Werte je Bildebene funktionieren.

Geprüft u. a.: Verbinden, Mehrbild-Import als Ebenen, PDF als einzelne Etiketten, defekte Datei, Text/Fett/Rundtext, Bildkorrekturen, Ausrichten/Drehen, 90°-Druckbereich (physisches Druckbild 576 × 354), Rückgängig/Wiederholen (auch beim Größenwechsel), Vorlage speichern/exportieren/importieren/in anderer Größe öffnen, eigene Schrift hochladen, Pfeiltasten (Laptop), Touch-Ziehen und Zwei-Finger-Zoom/Drehen (iPhone), Kopien, Dichte-Übergabe, Stopp während Stapeldruck, falsches Modell, Druckbild ist reines Schwarz/Weiß bzw. gerastert, keine horizontale Überbreite, keine JavaScript-Fehler.

## Nicht geprüft / Einschränkungen

- **Kein echter Druck:** Die Bluetooth-Übertragung wurde mit einem simulierten Drucker getestet. Der Treiber `niimbot.js` wurde **nicht verändert**. Bitte zuerst ein einzelnes Etikett drucken.
- **Kein echtes iPhone/Bluefy:** Getestet wurde in Chromium mit iPhone-Bildschirmgröße und Touch-Simulation, nicht in WebKit. Erforderlich ist iOS 15.4 oder neuer.
- Vorlagen aus V17 lassen sich öffnen; Vorlagen, die V17 bereits als „Gesamtbild“ umgewandelt hatte, bleiben ein Bild.

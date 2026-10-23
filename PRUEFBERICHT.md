# Prüfbericht V18 – 23. Oktober 2026

Grundlage: V17 („Mehrbild, Ebenen, Rahmen“). Der gesamte Code (`index.html`, `addon.js`, `niimbot-demo.html`, Treiber-Schnittstelle in `niimbot.js`) wurde durchgesehen und alle Funktionen wurden im Browser durchgetestet – mit iPhone-Größe (390 × 664 und 375 × 553, Touch) und Laptop-Größe (1366 × 768 und 1280 × 720).

## Gefundene Fehler und Korrekturen

| # | Problem in V17 | Korrektur in V18 |
|---|---|---|
| 1 | Der Treiber druckt jeden Pixel mit Helligkeit < 50 % schwarz; bei farbigen Fotos war das vorab nicht zu sehen. | Bilder bleiben standardmäßig unverändert (wie V17). Wer das Druckbild vorab sehen oder steuern will, wählt im Tab „Bild“ Schwarzweiß (mit Schwelle), Foto oder einen Zeichenstil. |
| 2 | Bedienmodus „Graustufen“ / „Hartes Schwarzweiß“ wirkte nur auf die Vorschau bzw. falsch skaliert (Schwelle in Originalauflösung). Der Schwellen-Regler im Tab „Bild“ war immer aktiv, wirkte aber nur in einem Modus. | Druckmodi Schwarzweiß und Foto (plus Zeichenstile). Die **Schwelle** sitzt in der Vorschau-Kopfzeile direkt neben dem Etikettnamen und wirkt in Druckauflösung auf das ausgewählte Bild. Alte Vorlagen werden weiter korrekt gedruckt. |
| 3 | **Laptop:** Werkzeuge unter der Vorschau, sichtbar waren nur ~100 px – kaum bedienbar. | Ab 900 px Breite: Vorschau links, Werkzeuge rechts in voller Höhe. |
| 4 | **iPhone:** Werkzeugbereich nur ~70 px hoch, Auswahllisten abgeschnitten („Niimbot“, „3 (mode“), Statuszeile halb abgeschnitten, Zoom-Schieber senkrecht (auf älteren iOS-Versionen nicht verlässlich). | Druckerleiste klappt nach dem Verbinden auf eine Zeile zusammen (`⚙ Drucker` öffnet sie), Werkzeugbereich ca. halbe Höhe und verstellbar, Zoom als `− 100% +` in der Vorschau, Statuszeile einzeilig. |
| 5 | Zuschneide-Dialog auf dem iPhone: Knöpfe „Zurücksetzen/Übernehmen“ abgeschnitten. | Dialog passt in den Bildschirm, Knöpfe in einer Zeile. |
| 6 | **Größenwechsel:** Texte blieben an alten Pixelpositionen (z. B. außerhalb des neuen Etiketts), Bilder wurden neu eingepasst; Vorlagen wurden in ein nicht mehr bearbeitbares Gesamtbild umgewandelt. | Alles wird proportional angepasst und bleibt bearbeitbar. Jede Etikettgröße merkt sich ihre Anordnung, Hin- und Herwechseln ist verlustfrei. |
| 7 | Beim Import fehlerhafter Dateien wurde der Dateiname von der Zusammenfassung überschrieben. | Meldung nennt jede übersprungene Datei mit Grund. WebP/GIF/HEIC (falls vom Browser lesbar) werden als PNG übernommen. |
| 9 | Prüffehler vor dem Druck (nicht verbunden, falsches Modell) zeigten „Bitte Papier prüfen; ein Etikett kann bereits ausgegeben worden sein“. | Klare Meldung ohne Papier-Hinweis. |
| 10 | „💾 Speichern“ unten öffnete nur den Vorlagen-Tab. | Speichert direkt als Vorlage. |
| 11 | Ebenenliste zeigte nach Textänderung noch den alten Text. | Wird sofort aktualisiert. |
| 12 | Rundtext erzeugte einen Kreis größer als das Etikett. | Startgröße passt ins Etikett. |
| 13 | Tab „Bild“ mit ausgewähltem Text zeigte „Aktiv: Textfeld“, Tab „Rundtext“ war ohne Auswahl komplett gesperrt. | Tabs wählen automatisch das passende Element. |
| 14 | Teile der Original-Demo englisch (Rollen, Protokoll, Statusmeldungen, „model default“, „cable flag“). | Auf Deutsch übersetzt. |
| 15 | Warteschlange zeigte nur „IMG/PDF“. | Kleine Druckbild-Vorschau, Anzahl Bilder/Texte, Zeile antippen = auswählen. |

## Bilddarstellung und Druck wie in den ersten Versionen

- **„🎨 Original“ ist der Standard:** Importierte Bilder erscheinen unverändert in Farbe und voller Auflösung und werden unverändert an den Niimbot-Treiber übergeben – genau wie in V17. Nachweis: Das an den Treiber gesendete Druckbild ist bei 6 Testfällen (Farbfoto, Logo, Porträt + Text, zwei Bilder auf einem Etikett, 90° gedreht + Text, PDF) **pixelgleich mit V17** (Farbabweichung 0, abweichende Druckpunkte 0).
- Zwischenstände dieser Sitzung, die Bilder automatisch umwandelten (Rasterung, Kontrast, Foto-Erkennung, PDF mit 300 dpi), sind rückgängig gemacht. Auch die Import-Pixeldichte heißt wieder „201 dpi“ wie in V17.
- **Umwandlung nur auf Wunsch:** Im Tab „Bild“ stehen Original, Schwarzweiß (mit Schwelle), Foto (feine Graustufen-Punkte) und die Zeichenstile Bleistift, Weich, Comic, Umrisse, Schraffur zur Wahl. Nur dann zeigt die Vorschau das umgewandelte Druckbild.
- **Schwelle** in der Vorschau-Kopfzeile: immer bedienbar (Maus, Pfeiltasten, Antippen, Wischen getestet). Bei „Original“ schaltet sie auf „Schwarzweiß“, damit die Wirkung sichtbar ist.
- **↻ 90°** in der Vorschau-Kopfzeile dreht den Druckbereich, auch ohne Datei; neue Etiketten übernehmen die Ausrichtung.
- **„Druckansicht/Original“-Umschalter entfernt.**

### Zeichenstile
Kantenerhaltende Vereinfachung (Kuwahara) vor der Linienerkennung, zweistufige Konturerkennung mit Hysterese (durchgehende Linien, Motiv tritt hervor), Entfernen kleiner Störflecken, geglättete Striche, Berechnung in Druckauflösung. Getestet mit 9 Motiven aus 6 Bereichen (2 Porträts, 2 Landschaften/Architektur, 1 Wohnraum – künstlich erzeugtes Testbild, 2 Gegenstände, 1 Tier, 1 Pflanze).

## Vorlagen überarbeitet

- **Fehler behoben:** Nach dem ersten Speichern blieben „Speichern“, „Als Kopie speichern“ und „Als Datei“ gesperrt – Namens- oder Ordneränderungen ließen sich nicht mehr sichern.
- **Ordner auswählbar:** Auswahlliste mit allen Ordnern und „＋ Neuer Ordner …“, Ordner-Filter über der Liste, Verschieben direkt in der Vorlagenkarte. Angelegte Ordner bleiben erhalten.
- **Laden:** „📂 Öffnen“ erzeugt keine Duplikate mehr, ersetzt ein leeres Etikett, bleibt im Vorlagen-Tab und übernimmt den Vorlagennamen; die bearbeitete Vorlage ist markiert. Klare Beschriftung von Kopie/Warteschlange/Drucken.
- Getestet (iPhone und Laptop, je 26 Prüfungen): Speichern mit neuem Ordner, erneutes Speichern/Umbenennen, Kopie, Ordnerfilter, Verschieben, Neuladen der Seite (alles bleibt erhalten), Öffnen ohne Duplikat, Bearbeiten + Speichern, Kopie/Warteschlange/Direktdruck, Suche, Öffnen in anderer Größe, Export/Import mit Ordner, Löschen.

## Automatisierte Tests (Chromium, Playwright, simulierter Drucker)

Alle Tests bestanden: **276 von 276** Prüfungen in 15 Durchläufen (iPhone 390 px, iPhone SE 375 px, Laptop 1366 px).

Schwellen-Test: Anteil schwarzer Pixel im Druckbild bei Schwelle 60 / 128 / 200 = 1,5 % / 14,6 % / 42 % – Vorschau und Druckbild ändern sich gleich, Druckbild bleibt reines Schwarz/Weiß, +/−, Rückgängig und eigene Werte je Bildebene funktionieren.

Geprüft u. a.: Verbinden, Mehrbild-Import als Ebenen, PDF als einzelne Etiketten, defekte Datei, Text/Fett/Rundtext, Bildkorrekturen, Ausrichten/Drehen, 90°-Druckbereich (physisches Druckbild 576 × 354), Rückgängig/Wiederholen (auch beim Größenwechsel), Vorlage speichern/exportieren/importieren/in anderer Größe öffnen, eigene Schrift hochladen, Pfeiltasten (Laptop), Touch-Ziehen und Zwei-Finger-Zoom/Drehen (iPhone), Kopien, Dichte-Übergabe, Stopp während Stapeldruck, falsches Modell, Druckbild ist reines Schwarz/Weiß bzw. gerastert, keine horizontale Überbreite, keine JavaScript-Fehler.

## Nicht geprüft / Einschränkungen

- **Kein echter Druck:** Die Bluetooth-Übertragung wurde mit einem simulierten Drucker getestet. Der Treiber `niimbot.js` wurde **nicht verändert**. Bitte zuerst ein einzelnes Etikett drucken.
- **Kein echtes iPhone/Bluefy:** Getestet wurde in Chromium mit iPhone-Bildschirmgröße und Touch-Simulation, nicht in WebKit. Erforderlich ist iOS 15.4 oder neuer.
- Vorlagen aus V17 lassen sich öffnen; Vorlagen, die V17 bereits als „Gesamtbild“ umgewandelt hatte, bleiben ein Bild.

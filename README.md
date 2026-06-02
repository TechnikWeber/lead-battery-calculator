# Batterie-Lebensdauer-Rechner (Blei VRLA)

Ein eigenständiges HTML-Tool zur Planung, Auswertung und Plausibilisierung von Tests an Blei-VRLA-Batterien (AGM/Gel). Eine einzige Datei, keine Installation, läuft offline in jedem modernen Browser.

## Hintergrund

Die Lebensdauer von Blei-Batterien hängt massiv von Betriebs- und Prüfbedingungen ab. Datenblattangaben gelten typischerweise für 25 °C — Norm-Prüfungen nach IEC 60254-1 laufen dagegen bei 33–43 °C, was Zyklenzahlen scheinbar drastisch reduziert (Faustformel: Halbierung der Lebensdauer je ~7 °C über 25 °C).

Wer im Labor Batterien gegeneinander testet, vergleicht außerdem ständig zwischen:

- 25-°C-Marketingwerten
- 40-°C-IEC-Compliance-Werten
- den eigenen Prüfbedingungen

Hier hilft das Tool: Es macht die einschlägigen Formeln (Faustformel, IEC-λ-Korrektur, East-Penn-Temperaturkompensation, IEEE-1188-Kt-Faktoren) direkt im Browser anwendbar.

## Funktionsumfang

Zwölf Module, gruppiert nach Aufgabe:

### Lebensdauer / Zyklenzahl

- **▶ Vorwärts** — Datenblattwert (25 °C) auf Ziel-Temperatur umrechnen. Optional inkl. Effekt einer nicht abgesenkten Ladespannung.
- **◀ Rückwärts** — Testwert bei erhöhter Temperatur auf 25-°C-Äquivalent zurückrechnen.
- **⊕ Kombiniert** — DoD, C-Rate und Temperatur gleichzeitig variieren, ausgehend von einem bekannten Datenblattwert.
- **≈ Schätzen** — absolute Schätzung ohne Datenblatt, verankert über Qualitätsklasse oder eigene Referenz.

### Test- und Auswertungshilfen

- **⏱ Testdauer** — Zyklen × Zeit/Zyklus + Auslastung → Testdauer. Zeigt direkt die Zeitersparnis durch erhöhte Prüftemperatur.
- **⚡ Ladespannung** — temperaturkompensierte Ladespannung nach East-Penn (3 mV/°C/Zelle).
- **🌡 Kapazität (Kt)** — Kapazitätskorrektur nach IEEE 1188 (East-Penn Appendix F, dazwischen interpoliert).
- **↻ C-Rate** — Umrechnung Ah / Stunden / Ampere.

### Speziell für ISO 7176-25 / IEC 60254-1

- **λ Kapazität (IEC)** — IEC-60254-1-Kapazitätskorrektur `Ca = C / [1 + λ₁(t₀−tr)]` (λ₁ = 0,006/°C bei 5h-Entladung).
- **⚠ Ladespg-Check** — Vergleicht normativ vorgegebene Ladespannung mit kompensiertem Soll und schätzt den Schadensfaktor durch Überladung (Stromverdopplung je 0,05 V/Zelle).
- **✓ Akzeptanz** — Prüft Ca gegen alle CN-Kriterien (≥0,85·CN bei Zyklus 1, ≥1,00·CN bis Zyklus 10, Abbruch <0,80·CN, ≥300 Zyklen).
- **⏲ Prüfzeit ISO** — Hochrechnung mit 50-Zyklen-Serien und Messzeit-Overhead.

## Benutzung

1. `Batterie_Lebensdauer_Rechner.html` herunterladen
2. Datei im Browser öffnen (Doppelklick reicht)
3. Oben Halbierungs-Intervall und Referenztemperatur einstellen, Reiter wählen, rechnen

Alle Eingaben werden live ausgewertet. Jedes Modul zeigt den Rechenweg und eine Bandbreite, damit der Charakter als Näherung sichtbar bleibt.

## Formel-Grundlagen

| Modul | Quelle |
|---|---|
| Temperatur-Faustformel | Arrhenius-Vereinfachung, Battery University, EMSYS |
| Temperaturkompensation Ladespannung | East-Penn `8A & 8G Battery Installation Instructions`, Appendix B |
| Kt-Faktoren | IEEE 1188-2005, East-Penn Appendix F |
| λ-Kapazitätskorrektur | IEC 60254-1 |
| Akzeptanzkriterien | ISO 7176-25:2013, IEC 60254-1 Section 5.5 |
| Überladungs-Modell | Multitel, EMSYS (Stromverdopplung je +0,05 V/Zelle bzw. +10 °C) |

## Einschränkungen

- Faustformeln sind Näherungen, keine exakten Naturkonstanten — Zellentyp und Hersteller beeinflussen die tatsächlichen Werte.
- Für den relativen Vergleich mehrerer Batterien unter identischen Bedingungen ist keine Umrechnung nötig — das Tool ersetzt keinen Test, es ordnet ein.
- Gilt für Blei-VRLA (AGM/Gel). Für Lithium-Ionen siehe das separate Schwestertool.
- Kalendarische Alterung ist eine eigene, parallele Lebensdauergrenze und nicht Bestandteil der Zyklenformeln.

## Technik

- Eine einzige HTML-Datei (~30 KB)
- Vanilla JavaScript, kein Build, keine Abhängigkeiten
- Kein Tracking, keine externen Calls — vollständig offline lauffähig

## Lizenz

MIT — Nutzung, Anpassung und Weitergabe frei. Verwendung auf eigene Verantwortung; das Tool ersetzt keine fachliche Prüfung.

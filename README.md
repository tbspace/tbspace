# P-/b-Strukturen — Schlusskurs-Zählung (Pine Script v6)

TradingView-Indikator, der **P-** und **b-Strukturen** nach der **Schlusskurs-Zählung
(PbD-Methodik)** aus *Trade The Trader Ausbildung → Modul 4 → „Schlusskurs-Zählung"*
automatisch erkennt und im Chart darstellt.

Datei: [`P-b-Strukturen_Schlusskurs-Zaehlung.pine`](./P-b-Strukturen_Schlusskurs-Zaehlung.pine)

## Logik

Es wird ausschließlich mit **Schlusskursen** gezählt:

- **Range-High (Aufwärtsimpuls):** Referenz ist der Schlusskurs der letzten Kerze
  des Impulses. Ein höherer Schluss aktualisiert die Referenz. Schließen **zwei
  Kerzen in Folge** gleich oder tiefer, wird die Referenz als **Range-High** bestätigt.
- **Range-Low (Gegenbewegung):** Referenz ist der tiefste Schluss der Gegenbewegung.
  Ein tieferer Schluss aktualisiert die Referenz. Schließen **zwei Kerzen in Folge**
  gleich oder höher, wird die Referenz als **Range-Low** bestätigt.
- **P-Struktur** = Aufwärtsimpuls + Konsolidierung (Long-Richtung) → Box am oberen Ende.
- **b-Struktur** = Abwärtsimpuls + Konsolidierung (Short-Richtung) → Box am unteren Ende.

Eine Struktur entsteht **nur aus Impuls + folgender Konsolidierung** – der Impuls
selbst bekommt keine Box. Die Trendrichtung wird über den Vergleich der Pivots
bestimmt: ein **höheres Tief** nach einem Aufwärtsimpuls ergibt eine P-Struktur, ein
**tieferes Hoch** nach einem Abwärtsimpuls eine b-Struktur. Dadurch wird eine
Momentum-Bewegung (z. B. Long-Impuls mit grünen Kerzen) korrekt **nicht** als
Gegenstruktur markiert.

Für Short-Trends gilt die Logik spiegelverkehrt. Der Indikator arbeitet immer auf
dem **aktuell im Chart geöffneten Time Frame** (keine feste TF-Vorgabe).

### Inside- / Outside-Bars (optional, Standard: aus)

Zusätzlich können Inside- und Outside-Bars markiert werden (Modul 4):

- **Inside-Bar** (`IB`): Kerze liegt vollständig innerhalb der Vorkerzen-Range
  (`high ≤ high[1]` und `low ≥ low[1]`) – Akkumulation im Trend.
- **Outside-Bar** (`OB`): Kerze umschließt die Vorkerze komplett
  (`high ≥ high[1]` und `low ≤ low[1]`) – Hinweis auf mögliche Trendumkehr.

Diese Funktion ist **standardmäßig deaktiviert** und im Code klar abgegrenzt
(Block `INSIDE-/OUTSIDE-BARS — START … ENDE`), sodass sie bei Bedarf komplett
entfernt werden kann.

## Verwendung

1. In TradingView den **Pine Editor** öffnen.
2. Inhalt der `.pine`-Datei einfügen.
3. **Zum Chart hinzufügen.**

### Einstellungen

| Einstellung | Bedeutung |
|---|---|
| **Bestätigungskerzen** | Anzahl Gegen-Schlusskurse zur Bestätigung einer Range-Grenze (Methodik = 2). |
| **Nur abgeschlossene Kerzen** | Zählung erst beim Kerzenschluss → kein Repainting. |
| **Strukturen / Range-Punkte / P-b-Beschriftung** | Anzeige-Optionen. |
| **Letzte Struktur verlängern** | Aktuelle Range live nach rechts ziehen. |
| **Farben** | Füllung und Rahmen für P- bzw. b-Strukturen. |
| **Inside-/Outside-Bars anzeigen** | Optionale IB/OB-Markierung (Standard: aus). |

## Hinweis

Boxen werden erst gezeichnet, wenn beide Range-Grenzen (High **und** Low) durch je
zwei Gegen-Schlusskurse bestätigt sind. Eine Struktur erscheint daher bewusst erst
nach Abschluss der Konsolidierung.

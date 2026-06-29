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

Zusätzlich muss ein **Mindestverhältnis Bewegung:Konsolidierung** (Default **3:1**)
erfüllt sein: Die Impulsbewegung muss mindestens das 3-fache der Konsolidierungs-
(Box-)Höhe betragen, damit nicht jede kleine Bewegung und Gegenbewegung als Struktur
erkannt wird. Der Wert ist in den Einstellungen anpassbar.

Für Short-Trends gilt die Logik spiegelverkehrt. Der Indikator arbeitet immer auf
dem **aktuell im Chart geöffneten Time Frame** (keine feste TF-Vorgabe).

### Inside- / Outside-Bars (optional, Standard: aus)

Zusätzlich können Inside- und Outside-Bars markiert werden (Modul 4):

- **Inside-Bar**: Kerze liegt vollständig innerhalb der Vorkerzen-Range
  (`high ≤ high[1]` und `low ≥ low[1]`) – Akkumulation im Trend, markiert mit einer
  **grauen Box**. Es wird immer nur **eine Inside-Bar gleichzeitig** verwaltet, eine
  neue entsteht erst, wenn die aktuelle abgeschlossen ist (**keine Verschachtelung**).
- **Outside-Bar** (`OB`): Kerze umschließt die Vorkerze komplett
  (`high ≥ high[1]` und `low ≤ low[1]`) – Hinweis auf mögliche Trendumkehr.

**Inside-Bar Box erweitern (Re-Test):**

- **Aus:** Die Box läuft mit, bis der Kurs sie per **Schlusskurs** verlässt, und wird
  danach noch um **x Balken** (Default 3, einstellbar) verlängert.
- **An:** Die Box wird auch nach dem Verlassen weiter nach rechts verlängert, bis der
  Kurs sie **erneut anläuft** (Re-Test) – passend zum Inside-Bar-Handelssignal aus
  Modul 4.

Inside- und Outside-Bars sind **getrennt schaltbar** und beide **standardmäßig
deaktiviert**. Der Code ist klar abgegrenzt (Block
`INSIDE-/OUTSIDE-BARS — START … ENDE`), sodass die Funktion bei Bedarf komplett
entfernt werden kann.

## Verwendung

1. In TradingView den **Pine Editor** öffnen.
2. Inhalt der `.pine`-Datei einfügen.
3. **Zum Chart hinzufügen.**

### Einstellungen

| Einstellung | Bedeutung |
|---|---|
| **Bestätigungskerzen** | Anzahl Gegen-Schlusskurse zur Bestätigung einer Range-Grenze (Methodik = 2). |
| **Mindestverhältnis Bewegung:Konsolidierung** | Impuls muss mind. dieses Vielfache der Box-Höhe betragen (Default 3:1). |
| **Nur abgeschlossene Kerzen** | Zählung erst beim Kerzenschluss → kein Repainting. |
| **Strukturen / Range-Punkte / P-b-Beschriftung** | Anzeige-Optionen. |
| **Letzte Struktur verlängern** | Aktuelle Range live nach rechts ziehen. |
| **Farben** | Füllung und Rahmen für P- bzw. b-Strukturen. |
| **Inside-Bars anzeigen** | Optionale Inside-Bar-Markierung als graue Box (Standard: aus). |
| **Inside-Bar Box erweitern (Re-Test)** | Aus: +x Balken nach Schlusskurs-Ausbruch. An: bis zum erneuten Anlaufen. |
| **↳ Erweiterung in Balken** | Zusätzliche Balken nach dem Ausbruch (nur bei „erweitern = Aus", Default 3). |
| **Outside-Bars anzeigen** | Optionale Outside-Bar-Markierung (Standard: aus). |

## Hinweis

Boxen werden erst gezeichnet, wenn beide Range-Grenzen (High **und** Low) durch je
zwei Gegen-Schlusskurse bestätigt sind. Eine Struktur erscheint daher bewusst erst
nach Abschluss der Konsolidierung.

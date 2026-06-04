# Padel Radar Berlin

Ein selbstlaufendes Dashboard, das die Auslastung und den geschätzten Umsatz
aller Berliner Padel-Clubs auf Basis der öffentlich sichtbaren Playtomic-
Verfügbarkeit auswertet. Es läuft komplett kostenlos auf GitHub – ganz ohne
eigenen Server und ohne Programmierkenntnisse.

## So funktioniert es (in einem Satz)

Alle 15 Minuten wird die Verfügbarkeit jedes Clubs abgefragt. Verschwindet ein
freier Slot zwischen zwei Abfragen (und liegt er noch weit genug in der
Zukunft), gilt er als gebucht. Daraus berechnen wir Auslastung und Umsatz –
getrennt nach „live gemessen" und „geschätztem Altbestand".

---

## Einrichtung in 7 Schritten

Du brauchst nur einen Webbrowser und ein kostenloses GitHub-Konto. Kein
Terminal, keine Installation.

### 1. GitHub-Konto erstellen
Gehe auf https://github.com und registriere dich (falls noch nicht vorhanden).

### 2. Neues Repository anlegen
- Oben rechts auf **+** → **New repository**.
- Name z.B. `padel-radar`.
- Sichtbarkeit: **Public** auswählen.
  > Warum public? Nur so sind die Automatik (GitHub Actions) und die
  > Webseiten-Anzeige (GitHub Pages) dauerhaft kostenlos. Nachteil: Code und
  > gesammelte Zahlen sind über die Adresse einsehbar. Wenn du das nicht
  > möchtest, siehe Abschnitt „Privat betreiben" weiter unten.
- **Create repository** klicken.

### 3. Projektdateien hochladen
- Im neuen Repository: **Add file** → **Upload files**.
- Entpacke die mitgelieferte ZIP-Datei und ziehe **den gesamten Inhalt**
  (die Ordner `padel_intel`, `docs`, `data`, `.github` und die einzelnen
  Dateien) in das Upload-Fenster.
- Unten **Commit changes** klicken.

### 4. Automatik aktivieren
- Reiter **Actions** öffnen.
- Falls ein grüner Knopf „I understand my workflows, go ahead and enable them"
  erscheint: anklicken.

### 5. Webseite (Dashboard) aktivieren
- Reiter **Settings** → links **Pages**.
- Unter **Source**: „Deploy from a branch".
- Branch: **main**, Ordner: **/docs** → **Save**.
- Nach ein paar Minuten zeigt GitHub dir oben die Adresse deines Dashboards an:
  `https://DEIN-NAME.github.io/padel-radar/`

### 6. Erstes Sammeln auslösen
- Reiter **Actions** → links **Collect** → rechts **Run workflow** → **Run workflow**.
- Warte 1–3 Minuten, bis der Lauf grün ist.

### 7. Dashboard öffnen
Öffne die Pages-Adresse aus Schritt 5. Ab jetzt aktualisiert sich alles
automatisch alle 15 Minuten.

---

## Auf dem iPhone als App
Adresse in **Safari** öffnen → **Teilen-Symbol** → **Zum Home-Bildschirm**.
Dann hast du ein App-Icon „Padel Radar".

---

## Was du am Anfang siehst
- Direkt nach dem Hochladen zeigt das Dashboard **Beispieldaten** (gelber
  Hinweis). Diese werden beim ersten echten Lauf automatisch ersetzt.
- Am ersten Tag sind die Zahlen noch grob: Es gibt viel „geschätzten
  Altbestand" und wenig „live gemessen". Je länger das Tool läuft, desto
  genauer wird die Trennung und desto vollständiger werden Woche/Monat.

## Einstellungen ändern
In der Datei `.github/workflows/collect.yml` unter `env:`:
- `PADEL_RADIUS_M` – Suchradius in Metern (Standard 30000 = 30 km).
- `PADEL_DAYS` – wie viele Tage in die Zukunft (Standard 14).
- `PADEL_LAT` / `PADEL_LNG` – Mittelpunkt der Suche (Standard Berlin-Mitte).

## Wenn keine Clubs gefunden werden
Falls die automatische Umkreissuche leer bleibt, kannst du Clubs fest vorgeben:
1. Öffne einen Club auf https://playtomic.io im Browser.
2. In der Adresszeile steht eine lange ID (eine UUID). Das ist die `tenant_id`.
3. Trage sie in `collect.yml` ein, z.B.:
   `PADEL_TENANT_IDS: "uuid-1,uuid-2,uuid-3"` (Raute davor entfernen).

## Privat betreiben (optional, später)
Wenn die Daten nicht öffentlich sein sollen: das Ganze auf einen kleinen
Server (z.B. Hetzner) oder einen Raspberry Pi umziehen und das Dashboard mit
Passwortschutz hinter einem Webserver betreiben. Die Sammel-Logik bleibt
identisch; nur der „alle-15-Minuten"-Auslöser und das Hosting wechseln.

## Wichtige Hinweise
- Alle Umsatz- und Auslastungszahlen sind **Schätzungen** aus öffentlich
  sichtbaren Daten, keine echten Buchungszahlen der Clubs.
- Es werden **keine personenbezogenen Daten** (keine Spielernamen o.ä.)
  gesammelt – nur Verfügbarkeit, Preise und Court-Typen.
- Automatisiertes Auslesen kann den Nutzungsbedingungen von Playtomic
  widersprechen. Die Abfragefrequenz ist bewusst moderat gehalten. Die
  rechtliche Einschätzung liegt bei dir (dies ist keine Rechtsberatung).

# Spots – eigene Karte als iPhone-App

Dateien in diesem Ordner: `index.html`, `manifest.webmanifest`, `icon-180.png`, `icon-192.png`, `icon-512.png`.
Es stehen keine Zugangsdaten in den Dateien – die Firebase-Verbindung wird erst in der App eingefügt.

## 1. Firebase (Online-Speicher, kostenlos)

1. https://console.firebase.google.com → **Projekt erstellen** (Name z. B. `spots`, Google Analytics aus).
2. Links **Build › Firestore Database** → **Datenbank erstellen** → Standort `europe-west3 (Frankfurt)` → **Produktionsmodus**.
3. Tab **Regeln** öffnen, alles ersetzen durch den Block unten, **Veröffentlichen**.
4. **Projektübersicht** (Zahnrad › Projekteinstellungen) → unten **App hinzufügen** → Web-Symbol `</>` → Name `Spots` → registrieren (Hosting *nicht* nötig).
5. Den angezeigten Block `const firebaseConfig = { … };` kopieren (z. B. in eine Notiz, um ihn aufs iPhone zu bekommen).

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /karten/{karte}/pins/{pin} {
      allow read, write: if karte.size() >= 24;
    }
  }
}
```

Die Regel erlaubt Zugriff nur, wenn man den langen, zufälligen Karten-Code kennt (steckt im Zugangsschlüssel der App). Man kann die vorhandenen Karten nicht auflisten.

## 2. Hosting über GitHub Pages (kostenlos, HTTPS – nötig für den Standort)

1. https://github.com/new → Repository-Name `spots`, **Public** → Create.
2. Auf der leeren Repo-Seite **uploading an existing file** → alle 5 Dateien hineinziehen → **Commit changes**.
3. **Settings › Pages** → Source: **Deploy from a branch** → Branch `main`, Ordner `/ (root)` → **Save**.
4. Nach ca. 1 Minute läuft die App unter `https://DEIN-GITHUB-NAME.github.io/spots/`.

## 3. Auf dem iPhone installieren

1. Die Adresse in **Safari** öffnen → Teilen-Symbol → **Zum Home-Bildschirm**.
2. Die App **vom Home-Bildschirm aus** öffnen (wichtig: sie hat eigenen Speicher, getrennt von Safari).
3. Den `firebaseConfig`-Block einfügen → **Weiter**. Die neue Karte ist angelegt.
4. Beim ersten Standort-Knopf den Ortungszugriff erlauben.

## 4. Zweites Handy verbinden

1. Auf Handy 1: Einstellungen (Regler-Symbol oben rechts) → **Teilen** oder **Kopieren** – der Schlüssel beginnt mit `SPOTS1.`.
2. Auf Handy 2: gleiche Adresse in Safari → Zum Home-Bildschirm → App öffnen → Schlüssel einfügen → **Weiter**.

Ab dann sehen beide Handys Änderungen sofort live (hinzufügen, bearbeiten, löschen).

## Gut zu wissen

- Kartenbild: CARTO-Basiskarten ohne Beschriftung (Stile Farbig / Hell / Dunkel in den Einstellungen). Daten © OpenStreetMap.
- Adresssuche und „naheliegende Adresse“: OpenStreetMap Nominatim.
- Profilbilder werden auf 320 × 320 px verkleinert und direkt in Firestore gespeichert – kein extra Speicher-Dienst nötig.
- Firestore-Gratiskontingent (50.000 Lesezugriffe/Tag, 1 GB) reicht für zwei Personen locker.
- App-Name ändern: in `index.html` alle „Spots“ ersetzen und in `manifest.webmanifest` die Felder `name`/`short_name`.
- Updates: geänderte Dateien einfach wieder ins Repo hochladen; die App holt sich die neue Version beim nächsten Öffnen.

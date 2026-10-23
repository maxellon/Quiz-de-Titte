# Foto-Quiz

Quiz mit Fotos und zwei Antworten (A/B). Die Spieler kommen per QR-Code mit dem Handy dazu, die Antworten laufen über eine Firebase-Datenbank und funktionieren so in jedem Netz (auch im Bus über mobile Daten).

- Host-Seite: `index.html` (Quiz anlegen, Spiel steuern)
- Spieler-Seite: `index.html?join=CODE` (steht im QR-Code) oder `index.html?spieler` (Code tippen)

## Einmalige Einrichtung: Firebase

1. https://console.firebase.google.com öffnen, mit Google-Konto anmelden, **Projekt hinzufügen** (Name z. B. `foto-quiz`). Google Analytics kann aus bleiben.
2. Links im Menü **Build → Realtime Database → Datenbank erstellen**. Standort **Belgium (europe-west1)**. Sicherheitsregeln: **Gesperrter Modus** wählen (wird gleich ersetzt).
3. Reiter **Regeln** öffnen, alles löschen und den Inhalt von `database.rules.json` aus diesem Repository einfügen, **Veröffentlichen**.
4. Zahnrad oben links → **Projekteinstellungen** → unten bei „Meine Apps“ auf das Web-Symbol `</>` → App-Spitzname `foto-quiz`, Hosting nicht ankreuzen, **App registrieren**.
5. Im angezeigten Code steht ein Block `const firebaseConfig = { apiKey: "…", … }`. Diese Werte in `index.html` oben im Block `FIREBASE_CONFIG` eintragen (`apiKey`, `authDomain`, `databaseURL`, `projectId`). Wenn `databaseURL` dort fehlt: die Adresse steht oben in der Realtime-Database-Ansicht (endet auf `firebasedatabase.app`).

Die Werte in `FIREBASE_CONFIG` sind keine Geheimnisse, sie gehören in jede Firebase-Web-App. Der Schutz liegt in den Datenbankregeln.

## Ablauf im Unterricht oder Bus

1. Host-Seite öffnen, **Neues Quiz**, Fotos hinzufügen, Antworten und die richtige Lösung eintragen. Quizze bleiben im Browser gespeichert; **Als Datei sichern** legt zusätzlich eine JSON-Datei an.
2. **Spiel starten**, Zeit pro Frage wählen, **Spiel eröffnen**. Es erscheint ein vierstelliger Code mit QR-Code.
3. Spieler scannen den Code, geben ihren Namen ein und sehen „Du bist dabei“.
4. **Erste Frage**. Frage 1 ist immer eine Aufwärmfrage (Bild und Antworten im Editor oben änderbar, Standard ist eine Sonne); sie läuft wie jede andere, vor Frage 2 werden alle Punkte auf 0 gesetzt. Alle sehen das Foto und tippen A oder B. Nach Ablauf der Zeit (oder per **Auflösen**) erscheint die Verteilung, die richtige Antwort und bei jedem Spieler die Punkte.
5. Nach der letzten Frage: Rangliste. **Spiel löschen** entfernt die Daten aus der Datenbank; Spiele älter als zwei Tage werden beim nächsten Start automatisch gelöscht.

Punkte: 100 pro richtiger Antwort plus bis zu 100 Bonus für Schnelligkeit (der Bonus sinkt mit der Zeit bis zum Ablauf der Fragezeit). Die Auflösung zeigt die drei Schnellsten.

## JGA-App (`jga/`)

Eigene Seite unter `/jga/` mit denselben Firebase-Zugangsdaten: Zeitplan (änderbar), Karte (OpenStreetMap, Orte aus dem Zeitplan per „Auf Karte setzen“), Bierpong-Turnier (2 Gruppen à 4 Teams, Halbfinale über Kreuz, Platz 3, Finale, live), Quiz-Zugang (zeigt automatisch den Code des gerade laufenden Foto-Quiz) und Hüttenfotos mit Infoblöcken. Alle dürfen alles bearbeiten („Bearbeiten“ oben rechts). Die Datenbankregeln brauchen zusätzlich den Knoten `jga` (siehe `database.rules.json`).

# SCHWIMMTRAINING. – Einrichtung der Online-Speicherung

Die Website ist fertig vorbereitet, aber ohne Firebase-Projekt gibt es **noch keine zentrale Datenspeicherung**. Bis zur Einrichtung sind Speichern, Bearbeiten und Löschen deaktiviert. Bitte keine Passwörter oder Zugangsschlüssel an ChatGPT senden.

## 1. Firebase-Projekt anlegen

1. Öffne https://console.firebase.google.com/ und erstelle ein neues Projekt, beispielsweise `schwimmtraining-ben`.
2. Analytics ist hierfür nicht notwendig.
3. Registriere unter den Projekteinstellungen eine **Web-App**. Kopiere aus der `firebaseConfig` die Werte `apiKey`, `authDomain`, `projectId` und `appId` in die Datei `firebase-config.js`.
4. Unter **Authentication → Sign-in method** aktiviere **Google** als Anmeldemethode und lege die Support-E-Mail fest.
5. Unter Authentication → Settings → Authorized domains trage `hoffmann87.github.io` ein (oder die tatsächliche Hosting-Domain).

## 2. Datenbank anlegen

1. Öffne **Firestore Database** und erstelle eine Datenbank (Region möglichst EU). Nicht im offenen Testmodus belassen.
2. Bei **Rules** zunächst die beigefügten `firestore.rules` verwenden. Solange `HIER_BENS_ELTERN_UID` nicht ersetzt wurde, sind Zugriffe gesperrt.

## 3. Berechtigung sicher freischalten

1. Veröffentliche die Dateien auf GitHub Pages (siehe Schritt 4) und öffne die Seite über HTTPS.
2. Melde dich im Reiter **Zugang** mit deinem Google-Konto an. Dort wird deine Benutzerkennung (UID) angezeigt.
3. Ersetze in **Firestore Database → Rules** die Zeichenfolge `HIER_BENS_ELTERN_UID` durch deine eigene UID. Klicke auf **Publish**.
4. Nun kannst du einen Testeintrag speichern. Auf einem zweiten Gerät mit demselben Google-Konto sollte er erscheinen.

**Wichtig:** Die Datei `firebase-config.js` ist öffentlich sichtbar. Das ist bei Firebase Web-Apps üblich und unkritisch, solange Firestore-Regeln korrekt gesetzt sind. Es dürfen **keine Service-Account-Schlüssel** oder privaten Passwörter auf GitHub liegen. Die Regeln erlauben nur der einen freigegebenen UID Zugriff auf die Einträge. Weitere Nutzer benötigen eine bewusste Änderung der Regeln und ggf. eine angepasste Struktur.

## 4. Eigenständige Website veröffentlichen

1. Erstelle unter https://github.com/new ein **öffentliches** Repository namens `schwimmtraining` in deinem GitHub-Konto `hoffmann87`.
2. Lade `index.html`, `firebase-config.js` und `EINRICHTUNG.md` in das Repository (im Hauptverzeichnis) hoch. Firestore-Regeln dürfen als Text im Repository liegen, sind aber nur aktiv, wenn du sie auch in Firebase veröffentlichst.
3. GitHub → Settings → Pages → Deploy from a branch → `main` → `/(root)` → Save.
4. Die Website wird dann unter `https://hoffmann87.github.io/schwimmtraining/` erreichbar sein. Der URL ist **noch nicht aktiv**, bevor die Einrichtung abgeschlossen ist.

## 5. Was wird gespeichert?

Pro Trainingseintrag: Datum, Beckenlänge (25/50 m), Schwimmart, Distanz, Gesamtzeit, optionale kumulative Zwischenzeiten, optionale Notiz sowie Erstellungs-/Änderungszeit. Firestore speichert die Einträge unter `owners/<deine UID>/sessions`.

Die Website trennt Bestzeiten und Diagramme strikt nach Schwimmart, Distanz und Beckenlänge. Daten anderer Benutzerkonten sind durch die Firestore-Regeln nicht einsehbar. 

## Hinweise zu Datenschutz und Kosten

Ben ist minderjährig. Öffentliche GitHub-Repositories sollten keine persönlichen Trainingsdaten enthalten. Diese liegen erst nach korrekter Einrichtung in Firestore und werden nur im angemeldeten Browser geladen. Das GitHub-Repository enthält lediglich die App-Dateien. Firebase und GitHub haben jeweils eigene Nutzungslimits, auch in kostenlosen Tarifen. Diese können sich ändern; aktuelle Preise in der jeweiligen Konsole prüfen.

Die Anwendung erfasst Zeiten, die du selbst einträgst. Es gibt keinen automatischen Import aus SWIMTRACK. Du kannst Daten später erweitern oder exportieren, sobald die Datenbank verbunden ist.
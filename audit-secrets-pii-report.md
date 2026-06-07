# Audit-Bericht: Secrets und personenbezogene Daten in KateiRen-Repositories

> **Hinweis:** Dieser Bericht ist vorläufig. Die Ergebnisse können **unvollständig** sein, weil GitHub Code Search pro Anfrage nur begrenzt viele Treffer zurückliefert.

## Zusammenfassung

Es wurden mehrere sicherheitsrelevante und datenschutzrelevante Auffälligkeiten gefunden:

- **Kritischer Secret-Leak** in `ESP32-Noise-Meter/boot.py` mit hartkodierten WLAN-Zugangsdaten
- **Interne Kontakt- und Zugangsdaten** in `MSFT-wichtige-Ressourcen`
- Mehrere Repositories mit **sensiblen Konfigurationsmustern**, aber ohne sichtbar geleakte echte Secret-Werte in den bisher geprüften Treffern

---

## Höchste Priorität

### 1. KateiRen/ESP32-Noise-Meter

- **Datei:** `boot.py`
- **Fundtyp:** `secret leak`
- **Beschreibung:** WLAN-Zugangsdaten sind hartkodiert im Quelltext.
- **Gefundene Werte:**
  - SSID: `KateiNet mobile`
  - Passwort: `Dana.Karsten.`
- **Risiko:** **Kritisch**
- **Empfohlene Maßnahmen:**
  1. Passwort **sofort rotieren**
  2. Datei aus der Git-History bereinigen
  3. Zugangsdaten künftig nur aus lokaler, ignorierter Konfiguration laden
  4. Falls das WLAN noch aktiv genutzt wird: bekannte Geräte und Logs prüfen

---

## Secret-/Credential-bezogene Funde

### 2. KateiRen/Azure-API

- **Dateien:**
  - `fetch_resource_groups.py`
  - `list_vm_skus_all_regions.py`
  - `test_access.py`
  - `Readme.md`
- **Fundtyp:** `secret handling / sensitive config`
- **Beschreibung:** Das Repo verarbeitet sensible Azure-Konfigurationswerte über Umgebungsvariablen.
- **Beispiele:**
  - `AZURE_CLIENT_SECRET`
  - `AZURE_TENANT_ID`
  - `AZURE_SUBSCRIPTION_ID`
- **Bewertung:** In den geprüften Treffern **kein sichtbarer Leak echter Werte**.
- **Risiko:** **Mittel**
- **Empfohlene Maßnahmen:**
  - prüfen, ob `.env` nie committed wurde
  - Commit-History auf frühere Secrets prüfen
  - Azure-App-Secrets vorsorglich rotieren, wenn Unsicherheit besteht

### 3. KateiRen/FinBot

- **Dateien:**
  - `DEVELOPMENT.md`
  - `docs/contracts/broker.md`
  - `messenger/app/config.py`
  - `docker-compose.yml`
  - `docs/roles/*.md`
- **Fundtyp:** `secret references / architecture docs`
- **Beispiele:**
  - `FINBOT_INTERNAL_SECRET`
  - `IB_PASSWORD`
  - `TELEGRAM_BOT_TOKEN`
  - `OPENROUTER_API_KEY`
- **Bewertung:** Sieht nach Dokumentation und korrekter Env-Nutzung aus, **kein sichtbarer Secret-Leak**.
- **Risiko:** **Niedrig bis Mittel**
- **Empfohlene Maßnahmen:**
  - `.env.example` und `.env` sauber trennen
  - Secret-Scanning in CI aktivieren
  - Repo-History auf versehentlich frühere echte Werte prüfen

### 4. KateiRen/airllm_test

- **Datei:** `run_airllm.py`
- **Fundtyp:** `secret reference`
- **Beispiele:**
  - `HF_TOKEN`
  - `HUGGINGFACEHUB_API_TOKEN`
- **Bewertung:** Nur Referenzen auf Umgebungsvariablen, **kein sichtbarer Token**.
- **Risiko:** **Niedrig**
- **Empfohlene Maßnahmen:**
  - optional History-Check auf frühere Test-Tokens

---

## Schwache Zugangsdaten / Security Weaknesses

### 5. KateiRen/KC-Remote-Keyboard

- **Datei:** `README.md`
- **Fundtyp:** `security weakness`
- **Beschreibung:** Default-AP-Passwort ist öffentlich dokumentiert.
- **Wert:** `kc85remote`
- **Risiko:** **Mittel**
- **Empfohlene Maßnahmen:**
  - Default-Passwort niemals produktiv verwenden
  - individuelles Passwort pro Gerät setzen
  - Warnhinweis in README prominenter machen

### 6. KateiRen/KC85-Tape-Player-ESP32-PIO

- **Dateien:**
  - `BUILD.md`
  - `COMPLETION_REPORT.md`
  - `src/main.cpp`
- **Fundtyp:** `security weakness / secret pattern`
- **Beschreibung:** Das Projekt setzt auf `WIFI_SSID` und `WIFI_PASSWORD` in `include/config.h`.
- **Bewertung:** Noch kein echter Leak gesehen, aber riskantes Muster, falls `config.h` versioniert wird.
- **Risiko:** **Mittel**
- **Empfohlene Maßnahmen:**
  - nur `config.h.example` versionieren
  - echte Werte in ignorierter lokaler Datei halten

---

## Personenbezogene Daten / mögliche PII

### 7. KateiRen/MSFT-wichtige-Ressourcen

- **Dateien:**
  - `WichtigeRessourcen.md`
  - `README.html`
  - `startpage/index_old.html`
- **Fundtyp:** `possible PII / internal contact data`
- **Beschreibung:** Mehrere interne Kontaktinformationen, Mailadressen, Telefonnummern und interne Zugangs-/Standortinformationen sind öffentlich sichtbar.
- **Beispiele:**
  - mehrere `@microsoft.com`-Adressen
  - interne Telefonnummern
  - `ovuser=...johamb@microsoft.com` in einer URL
  - Zugangsdatenhinweis:
    - `Username: Munich-HQ`
    - `Passwort: Parking`
- **Risiko:** **Hoch**
- **Empfohlene Maßnahmen:**
  1. Repo sofort auf interne/sensible Inhalte prüfen
  2. öffentliche Sichtbarkeit minimieren oder Repo privat stellen
  3. fragliche Zugangsdaten als kompromittiert behandeln und ändern
  4. Kontaktlisten, Intranet-Links und nutzerbezogene URLs entfernen
  5. Commit-History bereinigen

### 8. KateiRen/KateiRen.github.io

- **Dateien:** verschiedene Blog- und Markdown-Dateien
- **Fundtyp:** `possible low-risk PII`
- **Bewertung:** In den geprüften Treffern nichts klar Kritisches; überwiegend normale Autoren-/Namensnennungen.
- **Risiko:** **Niedrig**

---

## Wahrscheinliche False Positives / unkritische Treffer

### 9. KateiRen/micropython-mfrc522

- **Datei:** `examples/read.py`
- **Fundtyp:** `likely false positive`
- **Beschreibung:** Demo-Code mit Standard-MIFARE-Schlüssel.
- **Wert:** `FF FF FF FF FF FF`
- **Bewertung:** Kein persönliches Secret-Leak.
- **Risiko:** **Niedrig**

### 10. KateiRen/kc85-tape-player

- **Dateien:** `main.py`, `prompt.md`
- **Fundtyp:** `likely false positive / generated prompt content`
- **Bewertung:** Kein echter Secret-Leak in den sichtbaren Stellen.
- **Risiko:** **Niedrig**

---

## Priorisierte To-do-Liste

### Sofort

1. `ESP32-Noise-Meter/boot.py` bereinigen und WLAN-Passwort rotieren
2. `MSFT-wichtige-Ressourcen` auf interne Daten, Passwörter und Kontaktlisten prüfen und ggf. depublizieren
3. GitHub Secret Scanning und Push Protection aktivieren

### Danach

4. Alle Repos auf versehentlich committed prüfen:
   - `.env`
   - `config.h`
   - `local.settings.json`
   - `appsettings.*.json`
   - `*.pem`, `*.key`, `*.pfx`, `*.p12`
5. History-Scan auf alte Secrets durchführen
6. Alle potenziell kompromittierten Secrets rotieren

---

## Weiterführende GitHub-Code-Suchen

### Secrets

```text
https://github.com/search?q=user%3AKateiRen+%28AKIA+OR+ghp_+OR+github_pat_+OR+AIza+OR+sk-+OR+%22BEGIN+PRIVATE+KEY%22+OR+AZURE_CLIENT_SECRET+OR+OPENROUTER_API_KEY+OR+TELEGRAM_BOT_TOKEN+OR+IB_PASSWORD+OR+WIFI_PASSWORD%29&type=code
```

### PII / Kontakte

```text
https://github.com/search?q=user%3AKateiRen+%28%40microsoft.com+OR+mailto%3A+OR+tel%3A+OR+ovuser%3D+OR+password+OR+Passwort%29&type=code
```

---

## Empfohlene nächste Schritte

- pro betroffenem Repo eine Sanierungsliste erstellen
- kompromittierte Zugangsdaten rotieren
- `.gitignore`-Regeln härten
- History-Rewrite für echte Leaks vorbereiten
- optional: GitHub Actions für Secret-Scanning ergänzen

# 🚀 Automatische Changelog-Generierung mit LLM

🌐 **Sprachen**: [English](doc/README/en-US/README.md) | [中文](doc/README/zh-CN/README.md) | [日本語](doc/README/ja-JP/README.md) | [Deutsch](doc/README/de-DE/README.md) | [Español](doc/README/es-ES/README.md) | [Русский](doc/README/ru-RU/README.md) | [العربية](doc/README/ar-SA/README.md) | [繁體中文](doc/README/zh-TW/README.md)

Ein automatisches Tool basierend auf GitHub Actions, das DeepSeek und andere Large Language Models (LLM) verwendet, um standardisierte, strukturierte Versionsaktualisierungslogs (Changelog) automatisch zu generieren. Kein manuelles Schreiben erforderlich - nur den Workflow auslösen und eine professionelle Aktualisierungsdokumentation erhalten.

## ✨ Funktionen

- 🤖 **Intelligente Analyse**: Basierend auf LLM zur intelligenten Analyse von Git-Commits, automatisch kategorisiert in neue Funktionen, Leistungsverbesserungen, Bug-Fixes usw.
- 🏷️ **Automatische Tag-Verwaltung**: Unterstützt das automatische Erstellen von Git-Tags und intelligentes Berechnen von Versionsintervallunterschieden.
- 📁 **Strukturierter Speicher**: Generierte Changelog-Dokumente werden nach Hauptversion und Unterversion strukturiert gespeichert.
- 🎨 **Professionelle Vorlagen**: Bereitstellung standardisierter Markdown-Vorlagen mit einheitlichem und ansprechendem Ausgabeformat.
- 🔧 **Hochgradig konfigurierbar**: Unterstützt benutzerdefinierte LLM-APIs, Modelle, Prompt-Vorlagen und andere Parameter.
- ⚡ **Ein-Klick-Auslösung**: Durch GitHub Actions manuell ausgelöst, einfach die Versionsnummer eingeben und automatisch generieren.

## 🚀 Schnellstart

### Voraussetzungen

1. **GitHub-Repository**: GitHub Actions aktiviert
2. **LLM-API-Schlüssel**: API-Key für DeepSeek oder andere OpenAI-API-kompatible LLM-Dienste
3. **Python-Umgebung**: Ubuntu-Umgebung in GitHub Actions (automatisch konfiguriert)

### Installationsschritte

1. **Workflow-Datei kopieren**: Kopiere `.github/workflows/generate-changelog.yml` in den gleichen Pfad deines Repositories
2. **Skriptdateien kopieren**: Kopiere das Verzeichnis `.github/workflows/scripts/` in dein Repository
3. **Secrets konfigurieren**: Füge in den Repository-Einstellungen → Secrets and variables → Actions die folgenden Secrets hinzu:
   - `LLM_API_KEY`: Dein LLM-API-Schlüssel
   - (Optional) `LLM_API_ENDPOINT`: LLM-API-Endpunkt, standardmäßig DeepSeek
   - (Optional) `LLM_API_MODEL`: Zu verwendender Modellname, standardmäßig `deepseek-chat`

## 📖 Verwendung

### Manuelles Auslösen des Workflows

1. Gehe zur **Actions**-Seite deines GitHub-Repositories
2. Wähle den Workflow **Auto Generate Changelog with DeepSeek**
3. Klicke auf die Schaltfläche **Run workflow**
4. Gib folgende Parameter ein:
   - **main_version**: Hauptversionsnummer (z. B. `1.X`)
   - **sub_version**: Vollständige Versionsnummer (z. B. `1.X.X`)
   - **current_tag**: Aktueller Git-Tag-Name (z. B. `v1.X.X`, wird automatisch erstellt, wenn er nicht existiert)

### Workflow-Ausführungsprozess

1. **Code auschecken**: Pull der vollständigen Git-Historie und aller Tags
2. **Tag-Verarbeitung**: Überprüfung, ob der angegebene Tag existiert, Erstellung bei Nichtexistenz
3. **Commit-Differenzanalyse**: Intelligentes Berechnen der Commit-Unterschiede zur vorherigen Version
4. **LLM-Generierung**: Aufruf der DeepSeek-API zur Generierung eines strukturierten Changelogs
5. **Dateispeicherung**: Speichern des generierten Dokuments in `doc/Changelogs/{Hauptversion}/{Unterversion}/App.md`
6. **Automatischer Commit**: Commit der generierten Changelog-Datei in das Repository
7. **Tag-Push**: Wenn es sich um einen neu erstellten Tag handelt, automatischer Push zum Remote-Repository

## ⚙️ Konfigurationsanleitung

### Umgebungsvariablen-Konfiguration

Konfiguriere folgende Variablen in GitHub Secrets:

| Variablenname | Erforderlich | Standardwert | Beschreibung |
|--------------|-------------|-------------|-------------|
| `LLM_API_KEY` | ✅ | Keiner | LLM-API-Schlüssel |
| `LLM_API_ENDPOINT` | ❌ | `https://api.deepseek.com/chat/completions/` | API-Endpunkt-Adresse |
| `LLM_API_MODEL` | ❌ | `deepseek-chat` | Zu verwendender Modellname |

### Benutzerdefinierte Prompt-Vorlage

Um das Ausgabeformat zu ändern, bearbeite die Datei `.github/workflows/scripts/template.txt`:

```txt
Du bist ein Softwareentwicklung-Dokumentationstechniker. Bitte generiere basierend auf den folgenden Git-Commits ein规范、易读、strukturiertes Versionsaktualisierungslog (Changelog):
Anforderungen:
1. Ausgabe im Markdown-Format, angepasst an den Dokumentationsstil des App-Projekts
2. Kategorisierung: Neue Funktionen, Leistungsverbesserungen, Bug-Fixes, Code-Refactoring, Abhängigkeits-Updates
3. Knappe und formelle Sprache, Ausschluss von无效的merge- und wip-Commit-Beschreibungen
4. Kopfzeile mit Hauptversionsnummer, vollständiger Versionsnummer, aktuellem Tag und Aktualisierungsdatum
5. Nur reine Markdown-Haupttextausgabe, keine zusätzlichen Erklärungen, kein Einleitungstext
...
```

### Ausgabedateipfad

Der Pfad der generierten Changelog-Datei hat das folgende Format:
```
doc/Changelogs/{main_version}/{sub_version}/App.md
```

Beispielsweise bei `main_version=1`, `sub_version=1.2.3` lautet der Dateipfad:
```
doc/Changelogs/1/1.2.3/App.md
```

## 📁 Projektstruktur

```
.github/
├── workflows/
│   ├── generate-changelog.yml    # GitHub Actions Workflow-Definition
│   └── scripts/
│       ├── gen_changelog.py      # Hauptgenerierskript
│       └── template.txt          # LLM-Prompt-Vorlage
README.md                         # Projektdokumentation
```

### Kerndateibeschreibungen

- **generate-changelog.yml**: GitHub Actions Workflow-Definition mit vollständigem automatischem Generierungsprozess
- **gen_changelog.py**: Python-Skript zur Lesung von Commits, Aufruf der LLM-API und Speicherung der Ergebnisse
- **template.txt**: Prompt-Vorlage zur Steuerung des LLM-Ausgabeformats und -inhalts

## 🎯 Anwendungsbeispiel

### Beispiel-Workflow-Auslösung

1. **Eingabeparameter**:
   - main_version: `1`
   - sub_version: `1.2.0`
   - current_tag: `v1.2.0`

2. **Ausführungsergebnis**:
   - Automatisches Erstellen des Tags `v1.2.0` (falls nicht vorhanden)
   - Generierung der Datei `doc/Changelogs/1/1.2.0/App.md`
   - Automatischer Commit der generierten Datei in das Repository

### Beispiel für generiertes Changelog

```markdown
# 📝 Versionsaktualisierungslog
## [v1.2.0] - 2026-04-13

### ✨ Neue Funktionen
- 🌓 Neue Dunkel-/Hellmodus-Umschaltfunktion, Verbesserung der visuellen Benutzeroberfläche
- 🎨 Neue Farbwechsel-Logik für die Zeichenfläche, Unterstützung für dynamische Anpassung an das Thema

### 🐛 Fehlerbehebungen
- 🔧 Behebung von Kompatibilitätsproblemen mit cross-plattformen Backend-Startbefehlen
- 📂 Korrektur fehlerhafter Pfadkonfiguration in app.py

### 🚀 Funktionsverbesserungen
- 🎭 Vereinheitlichung hardcodierter Farbwerte in der Benutzeroberfläche zu Themenvariablen, Verbesserung der visuellen Stilkonsistenz
- 🎛️ Anpassung der Position des Sprachwechsel-Icons, Verbesserung der Bedienungslogik
```

## ⚠️ Hinweise

1. **API-Aufruf-Kosten**: Die Nutzung der LLM-API kann Kosten verursachen. Stellen Sie sicher, dass Sie die Abrechnungsmodalitäten des verwendeten Dienstes verstehen.
2. **Netzwerkstabilität**: Stellen Sie sicher, dass GitHub Actions den konfigurierten LLM-API-Endpunkt erreichen kann.
3. **Commit-Qualität**: Die Qualität des generierten Changelogs hängt von der Klarheit und Vollständigkeit der Commit-Nachrichten ab.
4. **Tag-Namenskonvention**: Es wird empfohlen, semantische Versionsnamen zu verwenden, wie `v1.0.0`, `v2.1.3` usw.
5. **Berechtigungsanforderungen**: Der Workflow benötigt Schreibberechtigungen für das Repository. Stellen Sie sicher, dass GitHub Actions über ausreichende Berechtigungen verfügt.

## ❓ Häufig gestellte Fragen

### Q1: Was passiert, wenn es keinen vorherigen Versions-Tag gibt?
A: Der Workflow verfügt über einen intelligenten Fallback-Mechanismus. Wenn kein vorheriger gültiger Tag gefunden wird, verwendet er automatisch die letzten 20 Commit-Nachrichten als Grundlage für die Generierung, um sicherzustellen, dass immer ein Changelog generiert werden kann.

### Q2: Kann ich andere LLM-Dienste verwenden?
A: Ja. Dieses Projekt ist kompatibel mit jedem LLM-Dienst, der eine OpenAI-Format-API bereitstellt. Konfigurieren Sie einfach die entsprechenden `LLM_API_ENDPOINT` und `LLM_API_MODEL` in den Secrets.

### Q3: In welchen Branch wird die generierte Changelog-Datei committet?
A: Standardmäßig wird in den Branch committet, in dem der Workflow ausgelöst wurde (normalerweise der `main`-Branch). Die Workflow-Konfiguration weist `ref: main` auf, um sicherzustellen, dass die Operation im Hauptbranch erfolgt.

### Q4: Wie kann ich die Kategorisierung des Changelogs ändern?
A: Bearbeiten Sie die Prompt-Vorlage in der Datei `.github/workflows/scripts/template.txt` und passen Sie die Kategorisierungsanforderungen an. Beispielsweise können Sie Kategorien wie "Sicherheitsupdates" oder "Dokumentationsverbesserungen" hinzufügen.

### Q5: Was passiert bei einem API-Aufruf-Fehler?
A: GitHub Actions zeigt automatisch Fehlerprotokolle an. Häufige Gründe sind: Ungültiger API-Schlüssel, fehlende Netzwerkzugänglichkeit zum API-Endpunkt, inkompatibles API-Antwortformat usw. Überprüfen Sie die Secrets-Konfiguration und die Netzwerkverbindung.

### Q6: Kann ich Changelogs für mehrere Versionen gleichzeitig generieren?
A: Ja. Geben Sie bei jedem manuellen Auslösen des Workflows unterschiedliche Versionsnummer-Parameter ein, um unabhängige Changelog-Dateien für verschiedene Versionen zu generieren.

### Q7: Warum ist die vollständige Git-Historie erforderlich?
A: Die vollständige Git-Historie (`fetch-depth: 0`) ist notwendig, um die Commit-Unterschiede zwischen Tags genau berechnen zu können. Dies ist die Grundlage für die Erstellung genauer Versionsaktualisierungslogs.

## 🔄 Benutzerdefinierte Erweiterungen

### Unterstützung für andere LLM-Dienste

Um zu einem anderen LLM-Dienst zu wechseln (z. B. OpenAI, Claude usw.), ändern Sie einfach folgende Konfigurationen:

1. Aktualisieren Sie `LLM_API_ENDPOINT` auf die API-Adresse des entsprechenden Dienstes
2. Aktualisieren Sie `LLM_API_MODEL` auf den entsprechenden Modellnamen
3. Stellen Sie sicher, dass das API-Antwortformat mit DeepSeek kompatibel ist (gibt `choices[0].message.content` zurück)

### Änderung des Ausgabeformats

Durch Bearbeiten der Datei `template.txt` können Sie das Ausgabeformat vollständig anpassen, beispielsweise:
- Anpassung der Kategorisierung
- Änderung der Emojis
- Hinzufügen benutzerdefinierter Abschnitte
- Änderung des Dokumentenstils

## 📄 Lizenz

Dieses Projekt unterliegt der MIT-Lizenz. Details finden Sie in der [LICENSE](https://github.com/Qiyao-sudo/Auto-Generate-Changelog-with-LLM/blob/main/LICENSE)-Datei.

## 🤝 Mitwirkungsrichtlinien

Beiträge in Form von Issues und Pull Requests zur Verbesserung dieses Projekts sind willkommen!

1. Forken Sie dieses Repository
2. Erstellen Sie einen Funktionsbranch (`git checkout -b feature/AmazingFeature`)
3. Committen Sie Ihre Änderungen (`git commit -m 'Add Changelog generation feature'`)
4. Pushen Sie zum Branch (`git push origin feature/AmazingFeature`)
5. Öffnen Sie einen Pull Request

## 🙏 Danksagungen

- Danke an [DeepSeek](https://www.deepseek.com/) für den excellenten LLM-Dienst
- Danke an GitHub Actions für die leistungsstarke Automatisierungsfähigkeit
- Danke an alle Mitwirkenden der Open-Source-Community

---

**Wenn dieses Projekt hilfreich für Sie ist, geben Sie bitte einen ⭐ Star als Unterstützung!**
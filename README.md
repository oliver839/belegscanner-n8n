# Belegscanner für n8n

Ein geplanter n8n-Workflow, der Belege aus E-Mails erkennt, relevante Daten extrahiert, vor der Ablage prüft und freigegebene Datensätze in Google Sheets schreibt.

> **Status:** Dieses Repository enthält derzeit die Dokumentation. Der n8n-Workflow-Export (`workflow.json`) wurde in dieser Sitzung nicht mitgeliefert und ist daher noch nicht enthalten. Die erwähnte PDF ist ein Beleg-/Wertpapier-Dokument und wurde aus Datenschutzgründen nicht in dieses öffentliche Repository hochgeladen.

## Vorgesehener Ablauf

1. **E-Mail-Eingang:** Gmail-Trigger überwacht ein festgelegtes Postfach bzw. Label auf neue Nachrichten mit Beleganhängen.
2. **Anhang prüfen:** Nur unterstützte Dokumente (z. B. PDF/JPG/PNG) werden verarbeitet; leere, beschädigte oder unerwartete Dateien werden zur manuellen Prüfung markiert.
3. **Daten extrahieren:** OCR bzw. ein konfigurierter Dokumenten-/KI-Dienst liest Lieferant, Belegdatum, Belegnummer, Währung, Netto-, Steuer- und Bruttobetrag aus.
4. **Verifizieren:** Pflichtfelder, Datums- und Zahlenformate sowie Summen (Netto + Steuer ≈ Brutto) werden geprüft. Unsichere oder widersprüchliche Ergebnisse werden **nicht** automatisch in das Sheet übernommen, sondern in einen Prüfpfad geleitet.
5. **Dubletten vermeiden:** Belegnummer und/oder ein Dokument-Fingerabdruck werden mit bereits erfassten Einträgen verglichen.
6. **Ablage:** Nur erfolgreich geprüfte Datensätze werden in Google Sheets eingefügt. E-Mail-/Datei-ID und Prüfstatus sollten zur Nachvollziehbarkeit mitgespeichert werden.

Die Verifizierung ist eine Plausibilitätsprüfung und ersetzt keine steuerliche oder buchhalterische Freigabe. Kritische bzw. unsichere Belege müssen von einer Person bestätigt werden.

## Voraussetzungen

- Eine n8n-Instanz mit Zugriff auf Gmail und Google Sheets
- OAuth2-Zugangsdaten für Gmail und Google Sheets, in n8n als Credentials hinterlegt
- Ein Google Sheet mit Spalten für z. B. `Belegdatum`, `Lieferant`, `Belegnummer`, `Währung`, `Netto`, `Steuer`, `Brutto`, `Prüfstatus`, `E-Mail-ID` und `Datei-ID`
- Falls OCR/KI verwendet wird: Zugangsdaten des gewählten Dienstes als n8n-Credentials oder Umgebungsvariablen

## Sicherheit und Datenschutz

- Keine Zugangsdaten, echten Belege oder personenbezogenen bzw. finanziellen Beispieldaten in dieses öffentliche Repository committen.
- OAuth-Tokens und API-Schlüssel ausschließlich in n8n Credentials/Secrets speichern, nicht in Workflow-JSON exportieren.
- Nur erforderliche E-Mail- und Sheet-Berechtigungen vergeben und Aufbewahrungs-/Löschfristen für Anhänge festlegen.
- Vor Produktivbetrieb den Workflow mit anonymisierten Belegen testen und Fehler-/Prüfpfade kontrollieren.

## Import

Sobald der n8n-Export vorliegt, kann er in n8n über **Workflows → Import from File** importiert werden. Anschließend Credentials zuweisen, Gmail-Trigger und Sheet-Spalten konfigurieren und zuerst mit Testdaten ausführen.

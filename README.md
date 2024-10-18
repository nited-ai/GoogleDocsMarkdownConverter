Google Apps Script: Dokumentverarbeitung und Markdown-Konvertierung
Dieses Google Apps Script ermöglicht die automatische Verarbeitung mehrerer Google Docs, indem es Markdown-Syntax in Formatierungen umwandelt (z.B. Überschriften, Listen, fett und kursiv), leere Absätze entfernt und die Dokumente nach der Verarbeitung in einen bestimmten Ordner verschiebt.

Funktionen
Verarbeitung mehrerer Dokumente in einem Durchlauf
Konvertierung von Markdown-Elementen (z.B. Überschriften, Listen, fett/kursiv) in Google Docs-Formatierungen
Automatisches Entfernen leerer Absätze
Setzen von Zeilenabständen nach Listenelementen
Verschieben verarbeiteter Dokumente in einen spezifischen Ordner auf Google Drive
Voraussetzungen
Google Apps Script Zugriff (über Google Apps Script)
Google Drive-Ordner für die Verarbeitung von Dokumenten
Web-App Bereitstellung für HTTP POST-Anfragen
Einrichtung
1. Klonen oder Erstellen eines Google Apps Script-Projekts
Gehe zu Google Apps Script und erstelle ein neues Projekt.
Füge den Code aus dem Skript in das Apps Script-Projekt ein.
2. Variablen anpassen
Im Skript gibt es zwei wichtige Variablen, die du anpassen musst:

javascript
Copy code
const SECRET_TOKEN = '0123456789ABCDEFGH'; // Ersetze dies durch dein starkes Token
const PROCESSED_FOLDER_ID = '1LpfMRuyarLzCkeuGHuD3dLVqG4mpZoUs'; // Ersetze dies durch die ID des Ordners, in den die verarbeiteten Dateien verschoben werden
SECRET_TOKEN: Ein Token, um sicherzustellen, dass nur autorisierte Anfragen an das Skript gesendet werden.
PROCESSED_FOLDER_ID: Die ID des Google Drive-Ordners, in den die verarbeiteten Dokumente verschoben werden.
Die Ordner-ID kannst du finden, indem du zu dem gewünschten Ordner auf Google Drive gehst und den letzten Teil der URL kopierst (z.B. https://drive.google.com/drive/folders/ID_HIER).

3. Bereitstellung als Web-App
Um das Skript über HTTP POST-Anfragen auszuführen, musst du es als Web-App bereitstellen:

Klicke in Google Apps Script auf Bereitstellen > Web-App.
Wähle die Berechtigungen:
Execute as: Me
Who has access: Anyone, even anonymous (oder eine geeignetere Einstellung, je nach Bedarf)
Klicke auf Deploy und kopiere die URL der Web-App. Diese URL wird für die HTTP POST-Anfragen verwendet.
4. HTTP-POST-Anfragen
Die Web-App erwartet JSON-Daten im folgenden Format:

json
Copy code
{
  "docIds": [
    "DOCUMENT_ID_1",
    "DOCUMENT_ID_2"
  ],
  "token": "s3cUr3T0k3n!@#4567890"
}
docIds: Ein Array mit den Google Docs-Dokumenten-IDs, die verarbeitet werden sollen.
token: Das geheime Token, um unbefugten Zugriff zu verhindern. Dieser Wert muss mit dem im Skript definierten SECRET_TOKEN übereinstimmen.
5. Beispiel für eine HTTP-POST-Anfrage
Mit curl:
bash
Copy code
curl --location 'https://script.google.com/macros/s/YOUR_SCRIPT_ID/exec' \
--header 'Content-Type: application/json' \
--data-raw '{
    "docIds": ["DOCUMENT_ID_1", "DOCUMENT_ID_2"],
    "token": "s3cUr3T0k3n!@#4567890"
}'
Mit Postman:
Wähle POST als HTTP-Methode.
Verwende die Web-App-URL, die du in Schritt 3 erhalten hast.
Füge den Header hinzu: Content-Type: application/json.
Füge im Body den JSON-Datensatz wie oben beschrieben hinzu.
6. Logs einsehen
Um die Logs der Verarbeitung einzusehen:

Öffne dein Apps Script-Projekt.
Gehe zu Ansicht > Protokolle (oder Logs), um die Ausgaben der Skriptausführung zu sehen.
Skriptübersicht
Hauptfunktionen
doPost(e):

Empfängt eine HTTP POST-Anfrage mit einem Array von Dokumenten-IDs (docIds) und einem Sicherheits-Token.
Verarbeitet jedes Dokument, konvertiert Markdown-Elemente in Google Docs-Formatierungen und verschiebt die Dokumente in den angegebenen Ordner.
convertMarkdownAndCleanUp(docId):

Diese Funktion verarbeitet ein einzelnes Dokument, erkennt Markdown-Syntax (z.B. # für Überschriften, Listenpunkte - oder *) und wendet die entsprechenden Formatierungen in Google Docs an.
Leere Absätze werden entfernt, und nach Listen-Elementen wird ein definierter Zeilenabstand eingefügt.
processDocument(docId):

Ruft die convertMarkdownAndCleanUp-Funktion auf und verschiebt das Dokument nach erfolgreicher Verarbeitung in den definierten Ordner.
Hilfsfunktionen:

getHeadingType: Bestimmt den entsprechenden ParagraphHeading-Typ (Überschriftenebene) basierend auf der Anzahl der # im Markdown.
applyTextFormatting: Wendet Markdown-Formatierungen wie Fett (**text**) und Kursiv (*text*) auf die entsprechenden Textabschnitte an.
setSpacingAfterLastListItems: Fügt nach dem letzten Listenelement einen Zeilenabstand hinzu.
removeEmptyParagraphs: Entfernt leere Absätze, außer dem letzten Absatz.
Beispiel für ein Markdown-Dokument
Hier ist ein Beispiel für ein Markdown-Dokument, das durch das Skript verarbeitet wird:

markdown
Copy code
# Überschrift 1

Dies ist ein normaler Absatz.

## Überschrift 2

- **Fetter Text** in einer Liste
- *Kursiver Text* in einer Liste

Mehr Text hier.

### Überschrift 3
Das Skript wird die Markdown-Elemente in entsprechende Google Docs-Formatierungen umwandeln.

Fehlerbehandlung
Wenn eine ungültige docId übermittelt wird, oder die Dokument-ID nicht zugänglich ist, wird eine Fehlermeldung zurückgegeben.
Wenn das token nicht übereinstimmt, wird ein Fehler mit "Unbefugter Zugriff" zurückgegeben.
Lizenz
Dieses Skript wird unter der MIT-Lizenz bereitgestellt.

Abschließende Hinweise:
Falls du das Skript erweitern möchtest, um weitere Markdown-Elemente oder zusätzliche Google Docs-Funktionen zu unterstützen, kannst du die convertMarkdownAndCleanUp-Funktion anpassen. Bei Fragen oder Problemen stehe ich gerne zur Verfügung!

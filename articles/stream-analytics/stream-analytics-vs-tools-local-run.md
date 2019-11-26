---
title: Lokales Testen von Azure Stream Analytics-Abfragen in Visual Studio
description: Dieser Artikel beschreibt, wie Abfragen mit den Azure Stream Analytics-Tools für Visual Studio lokal getestet werden.
services: stream-analytics
author: su-jie
ms.author: sujie
manager: kfile
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 07/10/2018
ms.openlocfilehash: 01383531293635af8c15f891928eec004e05b54d
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2019
ms.locfileid: "72925040"
---
# <a name="test-stream-analytics-queries-locally-with-visual-studio"></a>Lokales Testen von Stream Analytics-Abfragen mit Visual Studio

Mithilfe von Azure Stream Analytics-Tools für Visual Studio können Sie Ihre Stream Analytics-Aufträge lokal mit Beispieldaten oder [Livedaten](stream-analytics-live-data-local-testing.md) testen. 

In diesem [Schnellstarttutorial](stream-analytics-quick-create-vs.md) erfahren Sie, wie ein Stream Analytics-Auftrags mithilfe von Visual Studio erstellt wird.

## <a name="test-your-query"></a>Testen Ihrer Abfrage

Doppelklicken Sie in Ihrem Azure Stream Analytics-Projekt auf **Script.asaql**, um das Skript im Editor zu öffnen. Sie können die Abfrage kompilieren, um festzustellen, ob ein Syntaxfehler aufgetreten ist. Der Abfrage-Editor unterstützt IntelliSense, farbige Syntaxmarkierung und einen Fehlermarker.

![Abfrage-Editor](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-query-01.png)
 
### <a name="add-local-input"></a>Lokale Eingabe hinzufügen

Um Ihre Abfrage anhand der lokalen statischen Daten zu überprüfen, klicken Sie mit der rechten Maustaste auf die Eingabe, und wählen Sie **Lokale Eingabe hinzufügen** aus.
   
![Lokale Eingabe hinzufügen](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-add-local-input-01.png)
   
Wählen Sie im Popupfenster die Beispieldaten von Ihrem lokalen Pfad aus, und klicken Sie auf **Speichern**.
   
![Lokale Eingabe hinzufügen](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-add-local-input-02.png)
   
Eine Datei namens **local_EntryStream.json** wird automatisch Ihrem Ordner für Eingaben hinzugefügt.
   
![Dateiliste für lokalen Eingabeordner](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-add-local-input-03.png)
   
Wählen Sie im Abfrage-Editor die Option **Lokal ausführen** aus. Oder drücken Sie F5.
   
![Lokal ausführen](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-local-run-01.png)
   
Die Ausgabe kann im Tabellenformat direkt über Visual Studio angezeigt werden.

![Ausgabe im Tabellenformat](./media/stream-analytics-vs-tools-local-run/stream-analytics-for-vs-local-result.png)

Sie können den Ausgabepfad über die Konsolenausgabe finden. Drücken Sie eine beliebige Taste, um den Ergebnisordner zu öffnen.
   
![Lokaler Testlauf](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-local-run-02.png)
   
Überprüfen Sie die Ergebnisse im lokalen Ordner.
   
![Ergebnis des lokalen Ordners](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-local-run-03.png)
   

### <a name="sample-input"></a>Beispieleingabe
Sie können auch Stichproben von Eingabedaten aus Ihren Eingabequellen entnehmen und in einer lokalen Datei speichern. Klicken Sie mit der rechten Maustaste auf die Eingabekonfigurationsdatei, und wählen Sie **Beispieldaten** aus. 

![Beispieldaten](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-sample-data-01.png)

Sie können lediglich Stichproben von Datenstreaming von Event Hubs oder IoT Hubs entnehmen. Andere Eingabequellen werden nicht unterstützt. Geben Sie im Popupdialogfeld den lokalen Pfad ein, der zum Speichern von Beispieldaten verwendet wird, und klicken Sie auf **Beispiel**.

![Beispieldatenkonfiguration](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-sample-data-02.png)
 
Sie können den Status im Fenster **Ausgabe** anzeigen. 

![Beispieldatenausgabe](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-sample-data-03.png)

## <a name="next-steps"></a>Nächste Schritte


* [Schnellstart: Erstellen eines Stream Analytics-Auftrags mithilfe von Visual Studio](stream-analytics-quick-create-vs.md)
* [Anzeigen von Azure Stream Analytics-Aufträgen mit Visual Studio](stream-analytics-vs-tools.md)
* [Lokales Testen von Livedaten mithilfe von Azure Stream Analytics-Tools für Visual Studio (Vorschauversion)](stream-analytics-live-data-local-testing.md)
* [Tutorial: Bereitstellen eines Azure Stream Analytics-Auftrags mit CI/CD mithilfe von Azure DevOps](stream-analytics-tools-visual-studio-cicd-vsts.md)
* [Kontinuierliche Integration und Entwicklung mit Stream Analytics-Tools](stream-analytics-tools-for-visual-studio-cicd.md)
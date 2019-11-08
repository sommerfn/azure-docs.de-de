---
title: Vergleich der Features von Azure Stream Analytics
description: Dieser Artikel vergleicht die für die Azure Stream Analytics-Cloud und IoT Edge-Aufträge in Azure-Portal, Visual Studio und Visual Studio Code unterstützten Features.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: 4eb19a5b344cc5bda5ecad724daaddf9b0000d7e
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73580902"
---
# <a name="azure-stream-analytics-feature-comparison"></a>Vergleich der Features von Azure Stream Analytics

Mit Azure Stream Analytics können Sie mit [Azure-Portal](stream-analytics-quick-create-portal.md), [Visual Studio](stream-analytics-quick-create-vs.md) und [Visual Studio Code](quick-create-vs-code.md) in der Cloud und in IoT Edge Streaminglösungen erstellen. Die Tabellen in diesem Artikel zeigen, welche Features von jeder Plattform für beide Auftragstypen unterstützt werden.

## <a name="cloud-job-features"></a>Cloudauftragsfeatures


|Feature  |Portal  |Visual Studio  |Visual Studio Code  |
|---------|---------|---------|---------|
|Plattformübergreifend     |Mac</br>Linux</br>Windows         |Windows        |Mac</br>Linux</br>Windows          |
|Erstellen von Skripts     |Ja         |Ja         |Ja         |
|IntelliSense-Skript     |Syntaxhervorhebung         |Syntaxhervorhebung</br>Codevervollständigung</br>Fehlermarkierung         |Syntaxhervorhebung</br>Codevervollständigung</br>Fehlermarkierung         |
|Definieren von Eingaben, Ausgaben und Auftragskonfigurationen     |Ja         |Ja         |Ja         |
|Blobausgabepartitionierung     |Ja         |Ja         |Ja         |
|Power BI als Ausgabe     |Ja         |Ja         |Nein         |
|SQL-Datenbank-Referenzdaten     |Ja         |Ja         |Ja         |
|Benutzerdefinierte Nachrichteneigenschaften     |Ja         |Nein         |Nein         |
|Mehrere Abfragen übergreifende Freigabe von Eingaben und Ausgaben     |Nein         |Ja         |Ja         |
|JavaScript-UDF und -UDA     |Ja         |Ja         |Nur Windows         |
|Machine Learning-Legenden     |Ja, aber die Abfrage kann nicht getestet werden        |Ja, kann aber nicht lokal getestet werden         |Nein         |
|Kompatibilitätsgrad     |1.0</br>1.1</br>1.2         |1.0</br>1.1</br>1.2          |1.0</br>1.1</br>1.2          |
|Integrierte Anomalieerkennungsfunktionen auf ML-Basis     |Ja         |Ja         |Ja         |
|Integrierte Geofunktionen     |Ja         |Ja         |Ja         |
|Abfragetests mit einer Beispieldatei     |Ja         |Ja         |Ja         |
|Lokales Testen mit Livedaten     |Nein         |Ja         |Nein         |
|Auflisten von Aufträgen und Anzeigen von Auftragsentitäten     |Ja         |Ja         |Ja         |
|Exportieren eines Auftrags in ein lokales Projekt     |Nein         |Ja         |Ja         |
|Senden, Starten und Beenden von Aufträgen     |Ja         |Ja         |Ja         |
|Quellcodeverwaltung     |Nein         |Ja         |Ja         |
|CI/CD-Unterstützung     |Teilweise         |Ja         |Ja         |
|Anzeigen von Auftragsmetriken und Diagramm     |Ja         |Ja         |Im Portal öffnen         |
|Anzeigen von Auftragslaufzeitfehlern     |Ja         |Ja         |Nein         |
|Diagnoseprotokolle     |Ja         |Nein         |Nein         |


## <a name="iot-edge-job-features"></a>IoT Edge-Auftragsfeatures

|Feature  |Portal  |Visual Studio  |Visual Studio Code  |
|---------|---------|---------|---------|
|Auftragserstellung     |Ja         |Ja         |Nein         |
|Quellcodeverwaltung     |Nein         |Ja         |Nein         |
|Exportieren eines Auftrags in ein lokales Projekt     |Nein         |Ja         |Nein         |
|Abfragetests mit einer Beispieldatei     |Ja         |Ja         |Nein         |
|Mehrere Abfragen übergreifende Freigabe von Eingaben und Ausgaben     |Nein         |Ja         |Nein         |
|C#-UDF     |Nein         |Ja         |Nein         |
|Übermitteln von Aufträgen     |Ja         |Ja         |Nein         |
|Auflisten von Aufträgen und Anzeigen von Auftragsentitäten     |Ja         |Ja         |Nein         |
|Anzeigen von Auftragsmetriken und Diagramm     |Ja         |Teilweise         |Nein         |
|Anzeigen von Auftragslaufzeitfehlern     |Ja         |Teilweise         |Nein         |
|CI/CD-Unterstützung     |Nein         |Nein         |Nein         |


## <a name="next-steps"></a>Nächste Schritte

* [Azure Stream Analytics auf IoT Edge](stream-analytics-edge.md)
* [Tutorial: Schreiben einer benutzerdefinierten C#-Funktion für einen Azure Stream Analytics IoT Edge-Auftrag (Vorschauversion)](stream-analytics-edge-csharp-udf.md)
* [Entwickeln von Stream Analytics IoT Edge-Aufträgen mit Visual Studio-Tools](stream-analytics-tools-for-visual-studio-edge-jobs.md)
* [Anzeigen von Azure Stream Analytics-Aufträgen mit Visual Studio](stream-analytics-vs-tools.md)
* [Erkunden von Azure Stream Analytics mit Visual Studio Code (Vorschauversion)](vscode-explore-jobs.md)



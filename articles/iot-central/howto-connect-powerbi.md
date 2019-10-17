---
title: Visualisieren der Azure IoT Central-Daten in einem Power BI-Dashboard | Microsoft-Dokumentation
description: Verwenden Sie die Power BI-Lösung für Azure IoT Central zum Visualisieren und Analysieren Ihrer IoT Central-Daten.
ms.service: iot-central
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 10/4/2019
ms.topic: conceptual
ms.openlocfilehash: 3ce2f4304787107d0d6875333e4630dae8d7d1dd
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2019
ms.locfileid: "71973775"
---
# <a name="visualize-and-analyze-your-azure-iot-central-data-in-a-power-bi-dashboard"></a>Visualisieren und Analysieren der Azure IoT Central-Daten in einem Power BI-Dashboard

[!INCLUDE [iot-central-original-pnp](../../includes/iot-central-original-pnp-note.md)]

*Dieses Thema gilt für Administratoren.*

![Power BI-Lösungspipeline](media/howto-connect-powerbi/iot-continuous-data-export.png)

Verwenden Sie die Power BI-Lösung für Azure IoT Central, um ein leistungsfähiges Power BI-Dashboard zur Überwachung der Leistung Ihrer IoT-Geräte zu erstellen. In Ihrem Power BI-Dashboard können Sie folgende Aktionen ausführen:
- Nachverfolgen, wie viele Daten im Laufe der Zeit von Ihren Geräten gesendet werden
- Vergleichen der Datenmenge zwischen Telemetrie, Status und Ereignissen
- Identifizieren von Geräten, die viele Messungen melden
- Beobachten von historischen Trends der Gerätemessungen
- Identifizieren von problematischen Geräten, die viele kritische Ereignisse senden

Diese Lösung richtet die Pipeline ein, mit der die Daten in Ihrem Azure Blob Storage-Konto aus dem [Kontinuierlichen Datenexport](howto-export-data.md) verwendet werden. Diese Daten fließen zum Verarbeiten und Transformieren zu Azure Functions, Azure Data Factory und Azure SQL-Datenbank. Die Ausgabe kann in Power BI-Berichten, die Sie als PBIX-Datei herunterladen können, visuell dargestellt und analysiert werden. All diese Ressourcen werden in Ihrem Azure-Abonnement erstellt, damit Sie jede Komponente nach Ihren Anforderungen anpassen können.

> [!Note] 
> Die Power BI-Lösung für Azure IoT Central funktioniert mit IoT Central-Apps, die IoT Plug & Play nicht unterstützen (heute Vorschau-Apps).

## <a name="get-the-power-bi-solution-for-azure-iot-centralhttpsakamsiotcentralpowerbisolutiontemplate-from-microsoft-appsource"></a>Rufen Sie die [Power BI-Lösung für Azure IoT Central](https://aka.ms/iotcentralpowerbisolutiontemplate) aus Microsoft AppSource ab.

## <a name="prerequisites"></a>Voraussetzungen
Zum Einrichten der Lösung benötigen Sie Folgendes:
- Zugriff auf ein Azure-Abonnement
- IoT Central-Anwendung, die IoT Plug & Play nicht unterstützt (heute Vorschau-Apps)
- Fortlaufender Datenexport, der aus ihrer IoT Central-App für Azure Blob Storage eingerichtet wurde
    - Sicherstellen, dass das Datenformat Avro verwendet wird
    - Es wird empfohlen, dass Sie Datenströme zu Messungen, Geräten und Gerätevorlagen aktivieren, um das Power BI-Dashboard optimal zu nutzen.
    - Informieren Sie sich über das [Einrichten des fortlaufenden Datenexports](howto-export-data-blob-storage.md).
- Power BI Desktop (neueste Version)
- Power BI Pro (wenn Sie das Dashboard für andere Benutzer freigeben möchten)

## <a name="reports"></a>Berichte

Zwei Berichte werden automatisch generiert. 

Der erste Bericht zeigt eine historische Ansicht der Messungen, die von Geräten gemeldet wurden. Er schlüsselt die verschiedenen Typen von Messungen und Geräten auf, die die höchste Anzahl von Messungen gesendet haben.

![Seite 1 des Power BI-Berichts](media/howto-connect-powerbi/template-page1-hasdata.PNG)

Der zweite Bericht zeigt mehr Details zu Ereignissen und eine historische Ansicht der gemeldeten Fehler und Warnungen. Er zeigt auch, welche Geräte die höchste Anzahl von Ereignissen insgesamt melden, sowie speziell Fehlerereignisse und Warnungsereignisse.

![Seite 2 des Power BI-Berichts](media/howto-connect-powerbi/template-page2-hasdata.PNG)

## <a name="architecture"></a>Architecture
Auf alle Ressourcen, die erstellt wurden, kann im Azure-Portal zugegriffen werden. Alles sollte sich in einer Ressourcengruppe befinden.

![Azure-Portal-Ansicht der Ressourcengruppe](media/howto-connect-powerbi/azure-deployment.PNG)

Die Einzelheiten der einzelnen Ressourcen und ihre Verwendung werden im Folgenden beschrieben.

### <a name="azure-functions"></a>Azure-Funktionen
Die Azure Function-App wird jedes Mal ausgelöst, wenn eine neue Datei in Blob Storage geschrieben wird. Die Funktionen extrahieren die Felder innerhalb der einzelnen Messungen-, Geräte- und Gerätevorlagendateien und füllen mehrere vorläufige SQL-Tabellen auf, die von Azure Data Factory verwendet werden sollen.

### <a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory stellt eine Verbindung mit der SQL-Datenbank als verknüpften Dienst her. Er führt die Aktivitäten mit gespeicherten Prozeduren aus, die die Daten verarbeiten und in den Analysetabellen speichern.

### <a name="azure-sql-database"></a>Azure SQL-Datenbank
Diese Tabellen werden automatisch erstellt, um die Standardberichte aufzufüllen. Untersuchen Sie diese Schemas in Power BI, und Sie können Ihre eigenen Visualisierungen dieser Daten erstellen.

| Tabellenname |
|------------|
|[analytics].[Measurements]|
|[analytics].[Messages]|
|[stage].[Measurements]|
|[analytics].[Properties]|
|[analytics].[PropertyDefinitions]|
|[analytics].[MeasurementDefinitions]|
|[analytics].[Devices]|
|[analytics].[DeviceTemplates]|
|[dbo].[date]|
|[dbo].[ChangeTracking]|

## <a name="estimated-costs"></a>Geschätzte Kosten

Hier ist eine Schätzung der hiermit verbundenen Azure-Kosten (Azure Function, Data Factory, Azure SQL). Sämtliche Preise in US-Dollar. Bedenken Sie, dass die Preise von Region zu Region variieren, informieren Sie sich daher immer über die aktuellen Preise der einzelnen Dienste.
Die folgenden Standardwerte werden für Sie in der Vorlage festgelegt (Sie können diese nach der Einrichtung ändern):

- Azure Functions: App Service-Plan S1, 74,40 US-Dollar/Monat
- Azure SQL-S1, ~30 US-Dollar/Monat

Wir empfehlen Ihnen, sich mit den verschiedenen Preisoptionen vertraut zu machen und Anpassungen vorzunehmen, die Ihren Anforderungen entsprechen.

## <a name="resources"></a>Ressourcen

Besuchen Sie AppSource, um die [Power BI-Lösung für Azure IoT Central](https://aka.ms/iotcentralpowerbisolutiontemplate) abzurufen.

## <a name="next-steps"></a>Nächste Schritte

Da Sie nun wissen, wie Sie Ihre Daten in Power BI visualisieren können, empfiehlt sich als nächster Schritt:

> [!div class="nextstepaction"]
> [Verwalten von Geräten](howto-manage-devices.md)
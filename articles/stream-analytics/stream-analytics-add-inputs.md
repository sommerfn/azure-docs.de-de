---
title: Grundlegendes zu Eingaben für Azure Stream Analytics
description: In diesem Artikel wird das Konzept von Eingaben in einem Azure Stream Analytics-Auftrag beschrieben, wobei die Eingabe von Streamingdaten mit der Eingabe von Verweisdaten verglichen wird.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: 87e260c97a748807929a0e7021e3efb2ae8f8e7b
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2019
ms.locfileid: "67329287"
---
# <a name="understand-inputs-for-azure-stream-analytics"></a>Grundlegendes zu Eingaben für Azure Stream Analytics

Azure Stream Analytics-Aufträge stellen eine Verbindung mit einer oder mehreren Dateneingaben her. Jede Eingabe definiert eine Verbindung mit einer vorhandenen Datenquelle. Stream Analytics akzeptiert eingehende Daten aus verschiedenen Arten von Ereignisquellen einschließlich Event Hubs, IoT Hub und Blob Storage. Auf die Eingaben wird in der SQL-Streamingabfrage, die Sie für jeden Auftrag schreiben, namentlich verwiesen. In der Abfrage können Sie mehrere Eingaben verknüpfen, um Daten zu kombinieren oder Streamingdaten anhand einer Suche mit Verweisdaten zu vergleichen, und die Ergebnisse an Ausgaben übergeben. 

Stream Analytics bietet eine erstklassige Integration in drei Arten von Ressourcen als Eingaben:
- [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)
- [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) 
- [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/) 

Diese Eingabequellen können aus demselben Azure-Abonnement wie dem Ihres Stream Analytics-Auftrags oder aus einem anderen Abonnement stammen.

Zum Erstellen, Bearbeiten und Testen von Stream Analytics-Auftragseingaben können Sie das [Azure-Portal](stream-analytics-quick-create-portal.md#configure-job-input), [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.streamanalytics/New-azStreamAnalyticsInput), die [.NET-API](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.inputsoperationsextensions), die [REST-API](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-input) und [Visual Studio](stream-analytics-tools-for-visual-studio-install.md) verwenden.

## <a name="stream-and-reference-inputs"></a>Stream- und Verweiseingaben
Werden Daten mithilfe von Push an eine Datenquelle übertragen, werden sie vom Stream Analytics-Auftrag genutzt und in Echtzeit verarbeitet. Eingaben werden in zwei Typen unterteilt: Datenstromeingaben und Verweisdateneingaben.

### <a name="data-stream-input"></a>Datenstromeingabe
Ein Datenstrom ist eine ungebundene Abfolge von Ereignissen im Verlauf der Zeit. Stream Analytics-Aufträge müssen mindestens eine Datenstromeingabe enthalten. Event Hubs, IoT Hubs und Blob Storage werden als Datenstrom-Eingabequellen unterstützt. Event Hubs werden verwendet, um Ereignisdatenströme von verschiedenen Geräten und Diensten zu erfassen. Bei diesen Datenströmen kann es sich beispielsweise um Aktivitätsfeeds sozialer Medien, Börseninformationen oder Daten von Sensoren handeln. IoT Hubs sind zum Sammeln von Daten von verbundenen Geräten in IoT-Szenarien (Internet of Things) optimiert.  Blob Storage kann als Eingabequelle für die Erfassung von Massendaten als Datenstrom (z.B. Protokolldateien) verwendet werden.  

Weitere Informationen zu Streamingdateneingaben finden Sie unter [Streamen von Daten als Eingabe in Stream Analytics](stream-analytics-define-inputs.md).

### <a name="reference-data-input"></a>Verweisdateneingabe
Stream Analytics unterstützt auch Eingaben wie *Verweisdaten*. Verweisdaten sind entweder vollständig statisch oder werden nur langsam geändert. Sie werden normalerweise zum Ausführen von Korrelationen und Suchvorgängen verwendet. Beispielsweise können Sie Daten in der Datenstromeingabe mit den Daten in den Verweisdaten verknüpfen – ähnlich wie bei einer SQL-Verknüpfung zum Suchen statischer Werte. Azure Blob Storage und Azure SQL-Datenbank werden derzeit als Eingabequellen für Verweisdaten unterstützt. Quellblobs für Referenzdaten dürfen je nach Komplexität der Abfrage und zugeordneten Streamingeinheiten nicht mehr als 300 MB umfassen (weitere Informationen finden Sie im Abschnitt [Größenbeschränkung](stream-analytics-use-reference-data.md#size-limitation) in der Dokumentation zu Referenzdaten).

Weitere Informationen zu Verweisdateneingaben finden Sie unter [Verwenden von Verweisdaten für Suchvorgänge in Stream Analytics](stream-analytics-use-reference-data.md).

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Schnellstart: Erstellen eines Stream Analytics-Auftrags mithilfe des Azure-Portals](stream-analytics-quick-create-portal.md)

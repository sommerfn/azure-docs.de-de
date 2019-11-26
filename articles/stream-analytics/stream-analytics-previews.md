---
title: Azure Stream Analytics – Vorschaufeatures
description: Dieser Artikel beschreibt die Azure Stream Analytics-Features, die sich derzeit in der Vorschau befinden.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 10/30/2019
ms.openlocfilehash: df3e8c1cd91c676c64d15c46c5acdc3d5bcfaa8e
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73161380"
---
# <a name="azure-stream-analytics-preview-features"></a>Azure Stream Analytics – Vorschaufeatures

Dieser Artikel fasst alle Features zusammen, die sich derzeit in der Vorschau für Azure Stream Analytics befinden. Die Verwendung von Vorschaufeatures in einer Produktionsumgebung wird nicht empfohlen.

## <a name="public-previews"></a>Öffentliche Vorschauversionen

Die folgenden Features befinden sich in der öffentlichen Vorschau. Sie können diese Features heute nutzen, sollten sie aber nicht in Ihrer Produktionsumgebung einsetzen.

### <a name="online-scaling"></a>Onlineskalierung

Bei der Onlineskalierung müssen Sie den Auftrag nicht anhalten, wenn Sie die SU-Zuweisung ändern müssen. Sie können die SU-Kapazität eines laufenden Auftrags erhöhen oder verringern, ohne ihn anzuhalten. Dieser Fähigkeit liegt das Kundenversprechen von unternehmenskritischen Pipelines mit langer Ausführungszeit zugrunde, die Stream Analytics nun bietet. Weitere Informationen finden Sie unter [Konfigurieren von Azure Stream Analytics-Streamingeinheiten](stream-analytics-streaming-unit-consumption.md#configure-stream-analytics-streaming-units-sus).

### <a name="c-custom-de-serializers"></a>Benutzerdefinierte C#-Deserialisierer
Entwickler können von der Leistung von Azure Stream Analytics profitieren und Daten in Protobuf, XML oder einem beliebigen benutzerdefinierten Format verarbeiten. Sie können [benutzerdefinierte Deserialisierer](custom-deserializer-examples.md) in C# implementieren, um Ereignisse zu deserialisieren, die von Azure Stream Analytics empfangen wurden.

### <a name="extensibility-with-c-custom-code"></a>Erweiterbarkeit durch benutzerdefinierten C#-Code

Entwickler, die Stream Analytics-Module in der Cloud oder in IoT Edge erstellen, können benutzerdefinierte C#-Funktionen schreiben oder wiederverwenden und diese über [benutzerdefinierte Funktionen](stream-analytics-edge-csharp-udf-methods.md) direkt in der Abfrage abrufen.

### <a name="managed-identity-authentication-with-power-bi"></a>Authentifizierung der verwalteten Identität mit Power BI

Azure Stream Analytics bietet umfassende Unterstützung der auf der verwalteten Identität basierenden Authentifizierung mit Power BI, sodass eine dynamische Dashboard-Erfahrung gegeben ist.

### <a name="anomaly-detection"></a>Erkennung von Anomalien

Die Machine Learning-Modelle von Azure Stream Analytics unterstützen neben der bidirektionalen, langsam positiven und langsam negativen Trenderkennung auch die Erkennung von *Spikes* und *Dips*. Weitere Informationen finden Sie auf der Seite [Anomalieerkennung in Azure Stream Analytics](stream-analytics-machine-learning-anomaly-detection.md).

### <a name="debug-query-steps-in-visual-studio"></a>Schritte zum Debuggen von Abfragen in Visual Studio

Sie können auf einfache Weise eine Vorschau des Zwischenrowsets für ein Datendiagramm aufrufen, wenn Sie lokale Tests in Azure Stream Analytics-Tools für Visual Studio ausführen. 

### <a name="local-testing-with-live-data-in-visual-studio-code"></a>Lokales Testen mit Livedaten in Visual Studio Code

Sie können Ihre Abfragen mit Livedaten auf dem lokalen Computer testen, bevor Sie den Auftrag an Azure senden. Jede Testiteration dauert durchschnittlich zwei bis drei Sekunden, wodurch ein höchst effizienter Entwicklungsprozess sichergestellt ist.

### <a name="visual-studio-code-for-azure-stream-analytics"></a>Visual Studio Code für Azure Stream Analytics

Azure Stream Analytics-Aufträge können in Visual Studio Code erstellt werden. Siehe [Visual Studio Code-Schnellstart](https://docs.microsoft.com/azure/stream-analytics/quick-create-vs-code).


### <a name="anomaly-detection"></a>Erkennung von Anomalien

Azure Stream Analytics stellt neue Machine Learning-Modelle vor, die neben der bidirektionalen, langsam positiven und langsam negativen Trenderkennung auch die Erkennung von *Spikes* und *Dips* unterstützen. Weitere Informationen finden Sie auf der Seite [Anomalieerkennung in Azure Stream Analytics](stream-analytics-machine-learning-anomaly-detection.md).


### <a name="integration-with-azure-machine-learning"></a>Integration mit Azure Machine Learning

Sie können Stream Analytics-Aufträge mit Machine Learning (ML)-Funktionen skalieren. Um weitere Informationen dazu, wie Sie ML-Funktionen in Ihrem Stream Analytics-Auftrag verwenden können, finden Sie unter [skalieren ein Stream Analytics-Auftrags mit Azure Machine Learning-Funktionen](stream-analytics-scale-with-machine-learning-functions.md). Schauen Sie sich ein praktisches Szenario an in [Durchführen von Standpunktanalysen mit Azure Stream Analytics und Azure Machine Learning](stream-analytics-machine-learning-integration-tutorial.md).


### <a name="live-data-testing-in-visual-studio"></a>Livedatentests in Visual Studio

Visual Studio-Tools für Azure Stream Analytics erweitern die lokale Testfunktion, mit der Sie Ihre Abfragen anhand von Liveereignisstreams aus Cloudquellen wie Event Hub oder IoT Hub testen können. Erfahren Sie mehr über [Lokales Testen von Livedaten mithilfe von Azure Stream Analytics-Tools für Visual Studio](stream-analytics-live-data-local-testing.md).


### <a name="net-user-defined-functions-on-iot-edge"></a>Benutzerdefinierte NET-Funktionen auf IoT Edge

Mit benutzerdefinierten .NET-Standardfunktionen können Sie .NET-Standardcode als Teil Ihrer Streamingpipeline ausführen. Sie können einfache C#-Klassen erstellen oder komplette Projekte und Bibliotheken importieren. In Visual Studio wird das vollständige Erstellen von Dokumenten und Debuggen unterstützt. Weitere Informationen finden Sie unter [Entwickeln von benutzerdefinierten .NET Standard-Funktionen für Azure Stream Analytics Edge-Aufträge](stream-analytics-edge-csharp-udf-methods.md).

## <a name="other-previews"></a>Weitere Previews

Die folgenden Features stehen auf Anfrage ebenfalls in der Preview zur Verfügung.

### <a name="real-time-high-performance-scoring-with-custom-ml-models-managed-by-azure-machine-learning"></a>Leistungsstarke Echtzeitbewertung mit benutzerdefinierten ML-Modellen, die von Azure Machine Learning verwaltet werden

Azure Stream Analytics unterstützt die leistungsstarke Echtzeitbewertung anhand benutzerdefinierter, bereits trainierter ML-Modelle, die von Azure Machine Learning verwaltet und in Azure Kubernetes Service (AKS) oder Azure Container Instances (ACI) gehostet werden; dabei wird ein Workflow verwendet, für den Sie keinen Code schreiben müssen. [Registrieren Sie sich](https://aka.ms/asapreview1) für das Vorschaufeature.

### <a name="support-for-azure-stack"></a>Unterstützung für Azure Stack
Mit der Aktivierung dieses Features für die Azure IoT Edge-Runtime werden benutzerdefinierte Azure Stack-Features wie die native Unterstützung lokaler Ein- und Ausgaben in Azure Stack (z. B. Event Hubs, IoT Hub, Blob Storage) genutzt. Durch diese neue Integration können Sie hybride Architekturen erstellen, die Ihre Daten nah am Erstellungsort analysieren, wodurch Wartezeiten verringert und die gewonnenen Erkenntnisse maximiert werden.
Mit diesem neuen Feature können Sie hybride Architekturen erstellen, die Ihre Daten nah am Erstellungsort analysieren, wodurch Wartezeiten verringert und die gewonnenen Erkenntnisse maximiert werden. Sie müssen sich für dieses Vorschaufeature [registrieren](https://aka.ms/asapreview1).

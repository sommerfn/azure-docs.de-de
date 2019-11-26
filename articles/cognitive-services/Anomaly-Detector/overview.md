---
title: Worum handelt es sich bei der Anomalieerkennungs-API?
titleSuffix: Azure Cognitive Services
description: Verwenden Sie den erweiterten Algorithmus der Anomalieerkennungs-API, um Anomalien in Zeitreihendaten zu identifizieren.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: overview
ms.date: 09/05/2019
ms.author: aahi
ms.openlocfilehash: 706bd9c141b191632acee1c04745d64baaf50ee5
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74012055"
---
# <a name="what-is-the-anomaly-detector-api"></a>Worum handelt es sich bei der Anomalieerkennungs-API?

Die Anomalieerkennungs-API bietet Ihnen die Möglichkeit, Anomalien in Zeitreihendaten durch maschinelles Lernen zu überwachen und zu erkennen. Die Anomalieerkennungs-API passt sich an, indem die am besten passenden Modelle für Ihre Daten automatisch identifiziert und angewendet werden, unabhängig von der Branche, dem Szenario oder der Datenmenge. Mithilfe der Zeitreihendaten bestimmt die API die Grenzen für die Anomalieerkennung, die erwarteten Werte und, welche Datenpunkte Anomalien sind.

![Erkennen von Musteränderungen in Dienstanforderungen](./media/anomaly_detection2.png)

Für die Verwendung der Anomalieerkennung sind keine Erfahrungen im Bereich des maschinellen Lernens erforderlich. Dank der RESTful-API können Sie den Dienst einfach in Ihre Anwendungen und Prozesse integrieren.

## <a name="features"></a>Features

Mit der Anomalieerkennung können Sie Anomalien in allen Zeitreihendaten, oder auch während sie in Echtzeit auftreten, automatisch erkennen. 

|Feature  |BESCHREIBUNG  |
|---------|---------|
|Erkennen von Anomalien in Echtzeit | Anomalien werden in Ihren Streamingdaten erkannt, indem anhand vorheriger Datenpunkte ermittelt wird, ob der letzte Punkt eine Anomalie ist. Bei diesem Vorgang wird anhand der von Ihnen gesendeten Datenpunkte ein Modell generiert und bestimmt, ob der Zielpunkt eine Anomalie ist. Durch Aufrufen der API mit jedem neu generierten Datenpunkt können Sie Daten während der Erstellung überwachen. |
|Erkennen von Anomalien für das ganze Dataset als Batch | Die Zeitreihe wird verwendet, um alle Anomalien zu erkennen, die in den gesamten Daten potenziell vorhanden sind. Bei diesem Vorgang wird anhand der gesamten Zeitreihendaten ein Modell generiert, bei dem jeder Punkt mit demselben Modell analysiert wird.         |
| Erhalten zusätzlicher Informationen zu Ihren Daten | Erhalten Sie hilfreiche Details zu Ihren Daten und allen beobachteten Anomalien, wie erwarteten Werten, Anomaliegrenzen und Positionen. |
| Anpassen der Grenzen für die Anomalieerkennung | Die Anomalieerkennungs-API erstellt automatisch Grenzen für die Anomalieerkennung. Passen Sie diese Grenzen an, um die Empfindlichkeit der API für Datenanomalien zu erhöhen oder zu verringern, sodass diese besser zu Ihren Daten passt. |

## <a name="demo"></a>Demo

Sehen Sie sich [diese interaktive Demonstration](https://aka.ms/adDemo) an, um sich mit der Funktionsweise der Anomalieerkennung vertraut zu machen.
Zum Ausführen der Demonstration müssen Sie eine Ressource für die Anomalieerkennung und den API-Schlüssel und den Endpunkt abrufen.

## <a name="notebook"></a>Notizbuch

Weitere Informationen zum Abrufen der API für die Anomalieerkennung finden Sie unter diesem [Azure Notebook](https://aka.ms/adNotebook). In diesem im Web gehosteten Jupyter Notebook wird gezeigt, wie Sie eine API-Anforderung senden und das Ergebnis visualisieren.

Führen Sie die folgenden Schritte aus, um das Notebook auszuführen:

1. Holen Sie sich einen gültigen Abonnementschlüssel für die Anomalieerkennungs-API sowie einen API-Endpunkt. Im folgenden Abschnitt finden Sie Anweisungen zur Registrierung.
1. Melden Sie sich an, und klicken Sie rechts oben in der Ecke auf „Klon“.
1. Deaktivieren Sie im Dialogfeld die Option „Öffentlich“, bevor Sie den Klonvorgang durchführen. Andernfalls ist Ihr Notebook, einschließlich aller Abonnementschlüssel, öffentlich zugänglich.
1. Klicken Sie auf **Run on free compute (Mit kostenlosem Compute ausführen)** .
1. Wählen Sie eines der Notebooks aus.
1. Fügen Sie Ihren gültigen Abonnementschlüssel für die Anomalieerkennungs-API zur Variablen `subscription_key` hinzu. 
1. Ändern Sie die Variable `endpoint` in Ihren Endpunkt. Beispiel: `https://westus2.api.cognitive.microsoft.com/anomalydetector/v1.0/timeseries/last/detect`
1. Klicken Sie auf der oberen Menüleiste auf **Zelle** und dann auf **Alle ausführen**.

## <a name="workflow"></a>Workflow

Die Anomalieerkennungs-API ist ein RESTful-Webdienst und kann somit problemlos in jeder Programmiersprache aufgerufen werden, die HTTP-Anforderungen beherrscht und JSON analysieren kann.

[!INCLUDE [cognitive-services-anomaly-detector-data-requirements](../../../includes/cognitive-services-anomaly-detector-data-requirements.md)]

[!INCLUDE [cognitive-services-anomaly-detector-signup-requirements](../../../includes/cognitive-services-anomaly-detector-signup-requirements.md)]

Nach der Registrierung:

1. Konvertieren Sie die Zeitreihendaten in ein gültiges JSON-Format. Folgen Sie den [bewährten Methoden](concepts/anomaly-detection-best-practices.md), wenn Sie die Daten vorbereiten, um die besten Ergebnisse zu erzielen.
1. Senden Sie eine Anforderung mit Ihren Daten an die Anomalieerkennungs-API.
1. Analysieren Sie die zurückgegebene JSON-Nachricht, um die API-Antwort zu verarbeiten.

## <a name="algorithms"></a>Algorithmen

* Weitere Informationen zu den im Hintergrund verwendeten Algorithmen finden Sie in den technischen Blogs [Introducing Azure Anomaly Detector API](https://techcommunity.microsoft.com/t5/AI-Customer-Engineering-Team/Introducing-Azure-Anomaly-Detector-API/ba-p/490162) und [Overview of SR-CNN algorithm in Azure Anomaly Detector](https://techcommunity.microsoft.com/t5/AI-Customer-Engineering-Team/Overview-of-SR-CNN-algorithm-in-Azure-Anomaly-Detector/ba-p/982798).
* Informationen zu den von Microsoft entwickelten hochmodernen SR-CNN-Algorithmen finden Sie unter [Time-Series Anomaly Detection Service at Microsoft](https://arxiv.org/abs/1906.03821) (Anomalieerkennungsdienst für Zeitreihen bei Microsoft) (akzeptiert von KDD 2019).

> [!VIDEO https://www.youtube.com/embed/ERTaAnwCarM]

## <a name="join-the-anomaly-detector-community"></a>Beitreten zur Anomalieerkennungs-Community

* Beitreten zur [Ratgebergruppe für die Anomalieerkennung auf Microsoft Teams](https://aka.ms/AdAdvisorsJoin)
* Siehe [Benutzergenerierte Inhalte](user-generated-content.md)

## <a name="next-steps"></a>Nächste Schritte

* [Schnellstart: Erkennen von Anomalien in Zeitreihendaten mithilfe der Anomalieerkennungs-REST-API](quickstarts/detect-data-anomalies-csharp.md)
* [Onlinedemo](https://notebooks.azure.com/AzureAnomalyDetection/projects/anomalydetector) zur Anomalieerkennungs-API
* [REST-API-Referenz](https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-entire-detect) zur Anomalieerkennung

---
title: Debuggen und Problembehandlung für Machine Learning-Pipelines
titleSuffix: Azure Machine Learning
description: Debuggen und Problembehandlung für Machine Learning-Pipelines im Azure Machine Learning SDK für Python. Lernen Sie häufige Fallstricke bei der Entwicklung von Pipelines kennen, und erhalten Sie Tipps, die Ihnen helfen, Ihre Skripts vor und während der Remoteausführung zu debuggen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: trbye
ms.author: trbye
author: trevorbye
ms.date: 10/01/2019
ms.openlocfilehash: 50593741e185a146c5a376c34da959063198e7d0
ms.sourcegitcommit: 80da36d4df7991628fd5a3df4b3aa92d55cc5ade
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71817357"
---
# <a name="debug-and-troubleshoot-machine-learning-pipelines"></a>Debuggen und Problembehandlung für Machine Learning-Pipelines

In diesem Artikel erfahren Sie, wie Sie das Debuggen und die Problembehandlung für [Machine Learning-Pipelines](concept-ml-pipelines.md) im [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) durchführen.

Die folgenden Abschnitte bieten einen Überblick über häufige Fallstricke beim Erstellen von Pipelines und verschiedene Strategien zum Debuggen von Code, der in einer Pipeline ausgeführt wird. Verwenden Sie die folgenden Tipps, wenn Sie Schwierigkeiten haben, eine Pipeline wie erwartet auszuführen. 

## <a name="testing-scripts-locally"></a>Lokales Testen von Skripts

Einer der häufigsten Fehler in einer Pipeline ist, dass ein angefügtes Skript (Skript zur Datenbereinigung, Bewertungsskript usw.) nicht wie vorgesehen ausgeführt wird oder Laufzeitfehler im Remotecomputekontext enthält, die in Ihrem Arbeitsbereich im Azure-Portal schwer zu debuggen sind. 

Pipelines selbst können nicht lokal ausgeführt werden, aber der Unit-Test der zugrunde liegenden Skripts ist eine einfache Möglichkeit, um sicherzustellen, dass Ihre Skripts unabhängig voneinander das Erwartete ausführen, ohne auf die volle Ausführungsdauer der Pipeline zu warten. Hierfür ist ein gewisser Entwicklungsaufwand erforderlich:

* Wenn sich Ihre Daten in einem Clouddatenspeicher befinden, müssen Sie Daten herunterladen und Ihrem Skript zur Verfügung stellen. Die Verwendung einer kleinen Stichprobe Ihrer Daten ist eine gute Möglichkeit, die Laufzeit zu verkürzen und schnell Feedback zum Skriptverhalten zu erhalten.
* Wenn Sie versuchen, einen Pipelinezwischenschritt zu simulieren, müssen Sie die Objekttypen, die das jeweilige Skript vom vorherigen Schritt erwartet, möglicherweise manuell erstellen.
* Sie müssen auch Ihre eigene Umgebung definieren und die in Ihrer Remotecomputeumgebung definierten Abhängigkeiten replizieren.

Nachdem Sie ein Skript für die Ausführung in Ihrer lokalen Umgebung eingerichtet haben, gestalten sich Debugaufgaben wie die Folgenden viel einfacher:

* Anfügen einer benutzerdefinierten Debugkonfiguration
* Anhalten der Ausführung und Überprüfen des Objektzustands
* Abfangen von Typfehlern oder logischen Fehlern, die bis zur Laufzeit nicht verfügbar gemacht werden

## <a name="debugging-scripts-from-remote-context"></a>Debuggen von Skripts aus Remotekontext

Das lokale Testen von Skripts ist eine hervorragende Möglichkeit, wichtige Codefragmente und eine komplexe Logik zu debuggen, bevor Sie mit dem Erstellen einer Pipeline beginnen. Aber irgendwann werden Sie Skripts wahrscheinlich während der eigentlichen Pipelineausführung selbst debuggen müssen, insbesondere bei der Diagnose von Verhaltensweisen, die während der Interaktion zwischen Pipelineschritten auftreten. Wir empfehlen die großzügige Verwendung von `print()`-Anweisungen in Ihren Schrittskripts, damit Sie den Objektzustand und die erwarteten Werte während der Remoteausführung sehen können, ähnlich wie beim Debuggen von JavaScript-Code.

Die Protokolldatei `70_driver_log.txt` enthält: 

* Alle während der Ausführung Ihres Skripts ausgegebenen Anweisungen
* Die Stapelüberwachung für das Skript 

Um diese und andere Protokolldateien im Portal zu finden, klicken Sie zunächst auf die Pipeline in Ihrem Arbeitsbereich.

![Seite „Pipelines“ im Portal](./media/how-to-debug-pipelines/pipeline-1.png)

Navigieren Sie zu der übergeordneten Ausführung der Pipeline.

![Übergeordnete Ausführung der Pipeline](./media/how-to-debug-pipelines/pipeline-2.png)

Klicken Sie auf die ID der Ausführung für den jeweiligen Schritt.

![Seite „Pipelines“ im Portal](./media/how-to-debug-pipelines/pipeline-3.png)

Navigieren Sie zur Registerkarte **Protokolle**. Andere Protokolle enthalten Informationen zum Erstellungsvorgang für Ihr Umgebungsimage und zu Skripts zur Vorbereitung von Schritten.

![Seite „Pipelines“ im Portal](./media/how-to-debug-pipelines/pipeline-4.png)

> [!TIP]
> Ausführungen für *veröffentlichte Pipelines* finden Sie auf der Registerkarte **Pipelines** in Ihrem Arbeitsbereich im Portal. Ausführungen für *nicht veröffentlichte Pipelines* finden Sie unter **Experimente**.

## <a name="troubleshooting-tips"></a>Tipps zur Problembehandlung

Die folgende Tabelle enthält häufige Probleme bei der Pipelinentwicklung mit möglichen Lösungen.

| Problem | Mögliche Lösung |
|--|--|
| Daten können nicht in das Verzeichnis `PipelineData` übergeben werden | Stellen Sie sicher, dass Sie im Skript ein Verzeichnis erstellt haben, das dem entspricht, wo Ihre Pipeline die Schrittausgabedaten erwartet. In den meisten Fällen definiert ein Eingabeargument das Ausgabeverzeichnis, und dann erstellen Sie das Verzeichnis explizit. Verwenden Sie `os.makedirs(args.output_dir, exist_ok=True)`, um das Ausgabeverzeichnis zu erstellen. Im [Tutorial](tutorial-pipeline-batch-scoring-classification.md#write-a-scoring-script) finden Sie ein Beispiel für ein Bewertungsskript, das dieses Entwurfsmuster zeigt. |
| Fehler bei Abhängigkeiten | Wenn Sie Skripts lokal entwickelt und getestet haben, aber beim Ausführen auf einem Remotecomputer in der Pipeline Abhängigkeitsprobleme auftreten, stellen Sie sicher, dass Ihre Abhängigkeiten und Versionen der Computeumgebung mit Ihrer Testumgebung übereinstimmen. |
| Mehrdeutige Fehler bei Computezielen | Das Löschen und erneute Erstellen von Computezielen kann bestimmte Probleme mit Computezielen beheben. |
| Pipeline verwendet Schritte nicht wieder | Die Wiederverwendung von Schritten ist standardmäßig aktiviert, aber stellen Sie sicher, dass Sie sie in einem Pipelineschritt nicht deaktiviert haben. Wenn die Wiederverwendung deaktiviert ist, wird der Parameter `allow_reuse` im Schritt auf `False` festgelegt. |
| Pipeline wird unnötigerweise erneut ausgeführt | Um sicherzustellen, dass Schritte nur dann erneut ausgeführt werden, wenn sich die zugrunde liegenden Daten oder Skripts ändern, entkoppeln Sie Ihre Verzeichnisse für die einzelnen Schritte. Wenn Sie dasselbe Quellverzeichnis für mehrere Schritte verwenden, kann es zu unnötigen Wiederholungen kommen. Verwenden Sie den Parameter `source_directory` in einem Pipelineschrittobjekt, um auf Ihr isoliertes Verzeichnis für diesen Schritt zu verweisen, und stellen Sie sicher, dass Sie nicht denselben `source_directory`-Pfad für mehrere Schritte verwenden. |

## <a name="next-steps"></a>Nächste Schritte

* Hilfe zu den Paketen [azureml-pipelines-core](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) und [azureml-pipelines-steps](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py) finden Sie in der SDK-Referenz.

* Befolgen Sie das [erweiterte Tutorial](tutorial-pipeline-batch-scoring-classification.md) zur Verwendung von Pipelines für die Batchbewertung.
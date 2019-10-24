---
title: Azure Data Factory Mapping Data Flow – Debugmodus
description: Starten einer interaktiven Debugsitzung beim Erstellen von Datenflüssen
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 09/06/2019
ms.openlocfilehash: c65ed14abb86b3b434e46fbe857487b06e217aad
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72387374"
---
# <a name="mapping-data-flow-debug-mode"></a>Mapping Data Flow – Debugmodus



## <a name="overview"></a>Übersicht

Mit dem Debugmodus von Azure Data Factory Mapping Data Flow können Sie die Transformation der Datenform interaktiv beobachten, während Sie Ihre Datenflüsse erstellen und debuggen. Die Debugsitzung kann sowohl in Datenfluss-Entwurfssitzungen sowie während der Ausführung der Pipeline zum Debuggen von Datenflüssen verwendet werden. Um den Debugmodus zu aktivieren, verwenden Sie die Schaltfläche „Datenfluss-Debugmodus“ am oberen Rand der Entwurfsoberfläche.

![Schieberegler für Debugmodus](media/data-flow/debugbutton.png "Schieberegler für Debugmodus")

Nachdem Sie den Schieberegler aktiviert haben, werden Sie aufgefordert, die Integration Runtime-Konfiguration auszuwählen, die Sie verwenden möchten. Wird „AutoResolveIntegrationRuntime“ ausgewählt, wird ein Cluster mit acht allgemeinen Compute-Kernen und einer Dauer von 60 Minuten gestartet. Weitere Informationen zu Datenfluss-Integration Runtimes finden Sie unter [Datenflussleistung](concepts-data-flow-performance.md#increasing-compute-size-in-azure-integration-runtime).

![Auswahl der Integration Runtime zum Debuggen](media/data-flow/debugbutton2.png "Auswahl der Integration Runtime zum Debuggen")

Wenn der Debugmodus eingeschaltet ist, erstellen Sie Ihren Datenfluss mit einem aktiven Spark-Cluster. Die Sitzung wird beendet, sobald Sie das Debuggen in Azure Data Factory deaktivieren. Beachten Sie, dass stündlich Gebühren durch Azure Databricks anfallen, solange die Debugsitzung aktiviert ist.

In den meisten Fällen ist es eine gute Vorgehensweise, Ihre Datenflüsse im Debugmodus zu erstellen, sodass Sie Ihre Geschäftslogik validieren und Ihre Datentransformationen anzeigen können, bevor Sie Ihre Arbeit in Azure Data Factory veröffentlichen. Verwenden Sie im Bereich „Pipeline“ die Schaltfläche „Debuggen“, um Ihren Datenfluss in einer Pipeline zu testen.

## <a name="cluster-status"></a>Clusterstatus

Die Clusterstatusanzeige im oberen Bereich der Entwurfsoberfläche wird grün, wenn der Cluster zum Debuggen bereit ist. Wenn Ihr Cluster bereits betriebsbereiten ist, wird die Anzeige nahezu sofort grün. Wenn Ihr Cluster noch nicht ausgeführt wird, wenn Sie in den Debugmodus wechseln, müssen Sie 5-7 Minuten warten, bis der Cluster gestartet ist. Der Indikator wechselt solange, bis er bereit ist.

Wenn Sie mit dem Debuggen fertig sind, deaktivieren Sie den Debugmodus, damit Ihr Azure Databricks-Cluster beendet werden kann. Danach werden Ihnen keine Debugaktivitäten mehr in Rechnung gestellt.

## <a name="debug-settings"></a>Debugeinstellungen

Debugeinstellungen können bearbeitet werden, indem Sie auf „Debugeinstellungen“ in der Symbolleiste der Datenflusscanvas klicken. Hier können Sie den Zeilengrenzwert oder die Dateiquelle auswählen, der/die für jede Ihrer Quelltransformationen verwendet werden soll. Die Zeilengrenzwerte in dieser Einstellung gelten nur für die aktuelle Debugsitzung. Sie können auch dem mit dem Stagingprozess verknüpften Dienst auswählen, der für eine SQL Data Warehouse-Quelle verwendet werden soll. 

![Debugeinstellungen](media/data-flow/debug-settings.png "Debugeinstellungen")

Wenn Sie in Ihrem Datenfluss oder in einem der referenzierten Datasets Parameter verwenden, können Sie angeben, welche Werte beim Debuggen verwendet werden sollen, indem Sie die Registerkarte **Parameter** auswählen.

![Parameter für Debugeinstellungen](media/data-flow/debug-settings2.png "Parameter für Debugeinstellungen")

## <a name="data-preview"></a>Datenvorschau

Wenn das Debuggen aktiviert ist, wird im unteren Bereich die Registerkarte „Datenvorschau“ angezeigt. Wenn der Debugmodus deaktiviert ist, werden im Datenfluss auf der Registerkarte „Überprüfen“ nur die für Ihre Transformationen eingehenden und ausgehenden Metadaten angezeigt. Die Datenvorschau fragt nur die Anzahl der Zeilen ab, die Sie in Ihren Debugeinstellungen als Grenzwert festgelegt haben. Klicken Sie auf **Aktualisieren**, um die Datenvorschau abzurufen.

![Datenvorschau](media/data-flow/datapreview.png "Datenvorschau")

> [!NOTE]
> Durch Dateiquellen wird nur die Anzahl der angezeigten Zeilen eingeschränkt, aber nicht die Anzahl der gelesenen Zeilen. Für sehr große Datasets empfiehlt es sich, einen kleinen Teil der Datei auszuwählen und zu Testzwecken zu verwenden. Sie können unter „Debugeinstellungen“ eine temporäre Datei für jede Quelle auswählen, die den Dataset-Dateityp aufweist.

Wenn der Datenfluss im Debugmodus ausgeführt wird, werden Ihre Daten nicht in die Senkentransformation geschrieben. Eine Debugsitzung soll als Testumgebung für Ihre Transformationen dienen. Senken sind während des Debuggens nicht erforderlich und werden in Ihrem Datenfluss ignoriert. Wenn Sie das Schreiben der Daten in Ihre Senke testen möchten, führen Sie den Datenfluss aus einer Azure Data Factory-Pipeline aus, und verwenden Sie die Debugausführung aus einer Pipeline.

### <a name="testing-join-conditions"></a>Testen der Join-Bedingungen

Stellen Sie beim Ausführen von Komponententests für Joins-, Exists- oder Lookup-Transformationen sicher, dass Sie eine kleine Menge bekannter Daten für den Test verwenden. Mithilfe der oben erwähnten Option „Debugeinstellungen“ können Sie eine temporäre Datei für den Test festlegen. Dies ist erforderlich, da Sie beim Einschränken oder Sampling von Zeilen aus einem großen Dataset nicht vorhersagen können, welche Zeilen und welche Schlüssel zu Testzwecken in den Flow eingelesen werden. Das Ergebnis ist nicht deterministisch. Das bedeutet, dass die Verknüpfungsbedingungen Fehler verursachen können.

### <a name="quick-actions"></a>Schnelle Aktionen

Sobald die Datenvorschau angezeigt wird, können Sie eine schnelle Transformation generieren, um eine Spalte zu typisieren, zu entfernen oder zu ändern. Klicken Sie auf die Spaltenüberschrift, und wählen Sie dann auf der Symbolleiste der Datenvorschau eine der Optionen aus.

![Schnelle Aktionen](media/data-flow/quick-actions1.png "Schnelle Aktionen")

Nach Auswahl einer Änderung wird die Datenvorschau sofort aktualisiert. Klicken Sie in der oberen rechten Ecke auf **Bestätigen**, um eine neue Transformation zu generieren.

![Schnelle Aktionen](media/data-flow/quick-actions2.png "Schnelle Aktionen")

Beim **Typisieren** und **Ändern** wird eine Transformation für abgeleitete Spalten generiert, und bei Auswahl von **Entfernen** wird eine Auswahltransformation generiert.

![Schnelle Aktionen](media/data-flow/quick-actions3.png "Schnelle Aktionen")

> [!NOTE]
> Wenn Sie Ihren Datenfluss bearbeiten, müssen Sie die Datenvorschau erneut abrufen, bevor Sie eine schnelle Transformation hinzufügen.

### <a name="data-profiling"></a>Datenprofilerstellung

Wenn Sie auf der Registerkarte „Datenvorschau“ eine Spalte auswählen und auf der Symbolleiste der Datenvorschau auf **Statistik** klicken, wird ganz rechts in Ihrem Datenraster ein Diagramm mit detaillierten Statistiken zu jedem Feld angezeigt. Azure Data Factory wird basierend auf dem Datensampling bestimmen, welche Art von Diagramm für die Anzeige verwendet wird. Felder mit hoher Kardinalität werden standardmäßig mit NULL/NOT NULL-Diagrammen angezeigt, kategorische und numerische Daten mit niedriger Kardinalität werden in Balkendiagrammen mit Datenwerthäufigkeit dargestellt. Außerdem wird die maximale Länge der Zeichenkettenfelder, Min/Max-Werte in numerischen Feldern, Standardabweichung, Perzentile, Anzahl und Mittelwert angezeigt.

![Spaltenstatistiken](media/data-flow/stats.png "Spaltenstatistiken")

## <a name="next-steps"></a>Nächste Schritte

* Sobald Sie mit dem Erstellen und Debuggen Ihres Datenflusses fertig sind, [führen Sie ihn in einer Pipeline aus](control-flow-execute-data-flow-activity.md).
* Wenn Sie Ihre Pipeline mit einem Datenfluss testen, verwenden Sie die Option zum [Ausführen des Debuggens](iterative-development-debugging.md) für die Pipeline.

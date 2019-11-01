---
title: Azure Data Factory Mapping Data Flow-Parameter
description: Erfahren Sie, wie Sie eine Mapping Data Flow-Funktion aus Data Factory-Pipelines parametrisieren können.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 06/10/2019
ms.openlocfilehash: 0a1051d67bf45e96f82833ef8190008204cdc90b
ms.sourcegitcommit: 38251963cf3b8c9373929e071b50fd9049942b37
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "72387537"
---
# <a name="mapping-data-flow-parameters"></a>Mapping Data Flow-Parameter



Mapping Data Flows in Azure Data Factory unterstützen die Verwendung von Parametern. Sie können Parameter innerhalb Ihrer Datenflussdefinition definieren, die Sie dann in Ihren Ausdrücken verwenden können. Die Parameterwerte können von der aufrufenden Pipeline über die Aktivität „Datenfluss ausführen“ festgelegt werden. Sie haben drei Möglichkeiten, die Werte in den Ausdrücken der Datenflussaktivität festzulegen:

* Verwenden der Ausdruckssprache für die Pipeline-Ablaufsteuerung, um einen dynamischen Wert festzulegen
* Verwenden der Ausdruckssprache für den Datenfluss, um einen dynamischen Wert festzulegen
* Verwenden einer der beiden Ausdruckssprachen, um einen statischen Literalwert festzulegen

Nutzen Sie diese Funktion, um Ihre Datenflüsse universell einsetzbar, flexibel und wiederverwendbar zu gestalten. Mit diesen Parametern können Sie Datenflusseinstellungen und -ausdrücke parametrisieren.

> [!NOTE]
> Um Ausdrücke der Pipeline-Ablaufsteuerung zu verwenden, muss der Datenflussparameter vom Zeichenfolgentyp sein.

## <a name="create-parameters-in-mapping-data-flow"></a>Erstellen von Parametern in Mapping Data Flow

Wenn Sie Parameter zu Ihrem Datenfluss hinzufügen möchten, klicken Sie auf den leeren Bereich der Datenflusscanvas, um die allgemeinen Eigenschaften anzuzeigen. Im Einstellungsbereich sehen Sie eine Registerkarte mit dem Namen „Parameter“. Klicken Sie auf die Schaltfläche „Neu“, um einen neuen Parameter zu generieren. Für jeden Parameter müssen Sie einen Namen angeben, einen Typ auswählen und optional einen Standardwert festlegen.

![Erstellen von Datenflussparametern](media/data-flow/create-params.png "Erstellen von Datenflussparametern")

Parameter können in jedem Datenflussausdruck verwendet werden. Parameter beginnen mit $ und sind unveränderlich. Die Liste der verfügbaren Parameter finden Sie im Ausdrucks-Generator auf der Registerkarte „Parameter“.

![Datenflussparameterausdruck](media/data-flow/parameter-expression.png "Datenflussparameterausdruck")

## <a name="use-parameters-in-your-data-flow"></a>Verwenden von Parametern in Ihrem Datenfluss

* Sie können Parameterwerte innerhalb Ihrer Transformationsausdrücke verwenden. Sie finden die Parameterliste im Ausdrucks-Generator auf der Registerkarte „Parameter“. ![Verwenden von Datenflussparametern](media/data-flow/params9.png "UVerwenden von Datenflussparametern)

* Parameter werden auch verwendet, um dynamische Werte für Ihre Einstellungen der Quell- und Senkentransformation zu konfigurieren. Wenn Sie in konfigurierbare Felder klicken, wird der Link „Dynamischen Inhalt hinzufügen“ angezeigt. Wenn Sie darauf klicken, gelangen Sie zu einem Ausdrucks-Generator, in dem Sie Parameter zur Verwendung dynamischer Werte einsetzen können. ![Dynamischer Inhalt im Datenfluss](media/data-flow/params6.png "DDynamischer Inhalt im Datenfluss)

## <a name="set-mapping-data-flow-parameters-from-pipeline"></a>Festlegen von Mapping Data Flow-Parametern aus der Pipeline

Nachdem Sie Ihren Datenfluss mit Parametern erstellt haben, können Sie ihn mit der Aktivität „Datenfluss ausführen“ aus einer Pipeline ausführen. Nachdem Sie die Aktivität zur Pipelinecanvas hinzugefügt haben, werden Ihnen auf der Registerkarte „Parameter“ der Aktivität die verfügbaren Datenflussparameter angezeigt.

![Festlegen eines Datenflussparameters](media/data-flow/parameter-assign.png "Festlegen eines Datenflussparameters")

Wenn Ihr Parameterdatentyp eine Zeichenfolge ist, können Sie beim Klicken auf das Textfeld zum Festlegen von Parameterwerten entweder einen Pipelineausdruck oder einen Datenflussausdruck eingeben. Wenn Sie sich für den Pipelineausdruck entscheiden, wird Ihnen das Fenster für den Pipelineausdruck angezeigt. Stellen Sie sicher, dass Pipelinefunktionen in die Zeichenfolgen-Interpolationssyntax mit `'@{<expression>}'` aufgenommen werden, z. B.:

```'@{pipeline().RunId}'```

Wenn Ihr Parameter nicht vom Zeichenfolgentyp ist, wird Ihnen immer der Data Flow-Ausdrucks-Generator angezeigt. Hier können Sie beliebige Ausdrücke oder Literalwerte eingeben, die dem Datentyp des Parameters entsprechen. Nachfolgend sehen Sie Beispiele für den Datenflussausdruck und ein Zeichenfolgenliteral aus dem Ausdrucks-Generator:

* ```toInteger(Role)```
* ```'this is my static literal string'```

Jede Mapping Data Flow-Funktion kann eine beliebige Kombination von Pipeline- und Datenfluss-Ausdrucksparametern aufweisen. 

![Beispiel für Datenflussparameter](media/data-flow/parameter-example.png "Beispiel für Datenflussparameter")



## <a name="next-steps"></a>Nächste Schritte
* [Ausführen der Datenflussaktivität](control-flow-execute-data-flow-activity.md)
* [Ausdrücke für die Ablaufsteuerung](control-flow-expression-language-functions.md)

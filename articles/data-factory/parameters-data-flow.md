---
title: Azure Data Factory Mapping Data Flow Parameters
description: Erfahren Sie, wie Sie einen Mapping-Datenfluss aus Data Factory Pipelines parametrisieren können
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 06/10/2019
ms.openlocfilehash: 998f8080509e7ba18ea1a759dff2ed8b8742c910
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2019
ms.locfileid: "68253098"
---
# <a name="mapping-data-flow-parameters"></a>Abbilden von Datenflussparametern

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Die Abbildung von Data Flows in der Azure Data Factory unterstützt die Verwendung von Parametern. Sie können Parameter innerhalb Ihrer Datenflussdefinition definieren, die Sie dann in Ihren Ausdrücken verwenden können. Die Werte der Parameter können von der aufrufenden Pipeline über die Aktivität ausführen Datenfluss festgelegt werden. Sie haben drei Möglichkeiten, die Werte in den Ausdrücken der Datenflussaktivitäten einzustellen:

* Verwenden Sie die Sprache Pipeline Control Flow Expression, um einen dynamischen Wert festzulegen
* Verwenden Sie die Sprache des Datenflussausdrucks, um einen dynamischen Wert zu setzen
* Verwenden Sie eine der beiden Ausdruckssprachen, um einen statischen Literalwert zu festzulegen

Nutzen Sie diese Funktion, um Ihre Datenflüsse universell, flexibel und wiederverwendbar zu machen. Mit diesen Parametern können Sie Datenflusseinstellungen und Ausdrücke parametrieren.

> [!NOTE]
> Um Fließausdrücke der Pipeline-Kontrolle zu verwenden, muss Ihr Datenflussparameter vom Typ string sein.

## <a name="create-parameters-in-mapping-data-flow"></a>Parameter im Mapping-Datenfluss anlegen

Um Parameter zu Ihrem Datenfluss hinzuzufügen, klicken Sie auf den leeren Bereich der Datenfluss-Leinwand, um die allgemeinen Eigenschaften anzuzeigen. Im Einstellungsbereich sehen Sie eine Registerkarte mit dem Namen "Parameter". Klicken Sie auf die Schaltfläche "Neu", um einen neuen Parameter zu generieren. Für jeden Parameter müssen Sie einen Namen vergeben, einen Typ auswählen und optional einen Standardwert festlegen.

![Datenflussparameter erstellen](media/data-flow/create-params.png "Datenflussparameter erstellen")

Parameter können in jedem beliebigen Datenflussausdruck verwendet werden. Parameter beginnen mit $ und sind unveränderlich. Die Liste der verfügbaren Parameter finden Sie im Expression Builder unter der Registerkarte 'Parameter'.

![Datenflussparameterausdruck](media/data-flow/parameter-expression.png "Datenflussparameterausdruck")

## <a name="use-parameters-in-your-data-flow"></a>Verwenden von Parametern in Ihrem Datenfluss

* Sie können Parameterwerte innerhalb Ihrer Transformationsausdrücke verwenden. Sie finden die Parameterliste unter der Registerkarte Parameter im Expression Builder. ![Datenflussparameter nutzen](media/data-flow/params9.png "Datenflussparameter nutzen")

* Parameter werden auch verwendet, um dynamische Werte für Ihre Source- und Sink-Transformationseinstellungen zu konfigurieren. Wenn Sie in konfigurierbare Felder klicken, erscheint der Link "Add dynamic contect". Wenn Sie dort klicken, gelangen Sie zu einem Expression Builder, in dem Sie Parameter zur Verwendung dynamischer Werte verwenden können. ![Datenfluss dynamischer Inhalt](media/data-flow/params6.png " Datenfluss dynamischer Inhalt")

## <a name="set-mapping-data-flow-parameters-from-pipeline"></a>Festlegen von Mapping-Datenflussparametern aus der Pipeline

Nachdem Sie Ihren Datenfluss mit Parametern angelegt haben, können Sie ihn aus einer Pipeline mit Execute Data Flow Activity ausführen. Nachdem Sie die Aktivität zu Ihrer Pipeline-Leinwand hinzugefügt haben, werden Ihnen auf der Registerkarte "Parameter" der Aktivität die verfügbaren Datenflussparameter angezeigt.

![Festlegen eines Datenflussparameters](media/data-flow/parameter-assign.png "Festlegen eines Datenflussparameters ")

Wenn Ihr Parameterdatentyp string ist, können Sie beim Anklicken des Textfelds zum Festlegen von Parameterwerten entweder eine Pipeline oder einen Datenflussausdruck eingeben. Wenn Sie den Pipelineausdruck wählen, wird Ihnen das Fenster Pipelineausdruck angezeigt. Stellen Sie sicher, dass Pipelinefunktionen in die Zeichenfolgen-Interpolationssyntax mit `'@{<expression>}'` aufgenommen werden, z. B.:

```'@{pipeline().RunId}'```

Wenn Ihr Parameter nicht vom Typ string ist, wird Ihnen immer der Data Flow Expression Builder angezeigt. Hier können Sie beliebige Ausdrucke oder Literalwerte eingeben, die dem Datentyp des Parameters entsprechen. Nachfolgend finden Sie Beispiele für den Datenfluss-Ausdruck und eine literale Zeichenkette aus dem Expression Builder:

* ```toInteger(Role)```
* ```'this is my static literal string'```

Jeder Mapping-Datenfluss kann eine beliebige Kombination von Pipeline- und Datenfluss-Ausdrucksparametern aufweisen. 

![Datenflussparameter Beispiel](media/data-flow/parameter-example.png "Datenflussparameter Beispiel")



## <a name="next-steps"></a>Nächste Schritte
* [Ausführen der Datenflussaktivität](control-flow-execute-data-flow-activity.md)
* [Flussaktivitäten kontrollieren](control-flow-expression-language-functions.md)

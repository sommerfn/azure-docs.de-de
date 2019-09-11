---
title: Verarbeiten von Textdateien mit fester Länge mithilfe von Mapping Data Flows in Azure Data Factory
description: Erfahren Sie, wie Sie Textdateien mit fester Länge in Azure Data Factory mithilfe von Mapping Data Flows verarbeiten.
services: data-factory
author: balakreshnan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 8/18/2019
ms.author: makromer
ms.openlocfilehash: e1ba09f459152616941071c23f7a6545ec2a8b73
ms.sourcegitcommit: d470d4e295bf29a4acf7836ece2f10dabe8e6db2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2019
ms.locfileid: "70210508"
---
# <a name="process-fixed-length-text-files-using-data-factory-mapping-data-flows"></a>Verarbeiten von Textdateien mit fester Länge mithilfe von Data Factory Mapping Data Flows

Data Factory Mapping Data Flows unterstützt die Transformation von Daten aus Textdateien mit fester Breite. Sie definieren ein Dataset für eine Textdatei ohne Trennzeichen und richten dann Unterteilungen in Teilzeichenfolgen basierend auf der Ordinalposition ein.

## <a name="create-a-pipeline"></a>Erstellen einer Pipeline

1. Wechseln Sie zu **+Neue Pipeline**, um eine neue Pipeline zu starten.

2. Fügen Sie eine Datenflussaktivität hinzu, die zum Verarbeiten von Dateien mit fester Breite verwendet wird.

  ![Pipeline mit fester Breite](media/data-flow/fwpipe.png)

3. Wählen Sie in der Datenflussaktivität die Option für eine neue Mapping Data Flow-Instanz aus.

4. Fügen Sie eine Transformation für Quelle, abgeleitete Spalte, Auswahl und Senke hinzu.

  ![Datenfluss mit fester Breite](media/data-flow/fw2.png)

5. Konfigurieren Sie die Quellentransformation so, dass ein neues Dataset vom Typ „durch Trennzeichen getrennter Text“ verwendet wird.

6. Legen Sie kein Spaltentrennzeichen und keine Header fest.

Es werden nun einfach die Ausgangspunkte und Längen der Felder für den Inhalt dieser Datei festgelegt:

```
1234567813572468
1234567813572468
1234567813572468
1234567813572468
1234567813572468
1234567813572468
1234567813572468
1234567813572468
1234567813572468
1234567813572468
1234567813572468
1234567813572468
1234567813572468
```

7. Auf der Registerkarte „Projektion“ der Quellentransformation wird eine Zeichenfolgenspalte mit dem Namen „Column_1“ angezeigt.

8. Erstellen Sie nun unter „Abgeleitete Spalte“ eine neue Spalte.

9. Geben Sie den Spalten einfache Namen wie „Col1“.

10. Geben Sie dann im Ausdrucks-Generator Folgendes ein:

  ```substring(Column_1,1,4)```

  ![Abgeleitete Spalte](media/data-flow/fwderivedcol1.png)

10. Wiederholen Sie diesen Schritt für alle Spalten, die Sie analysieren müssen.

12. Klicken Sie auf die Registerkarte „Untersuchen“, um die neuen Spalten anzuzeigen, die generiert werden.

  ![Untersuchen](media/data-flow/fwinspect.png)

13. Verwenden Sie die Auswahltransformation, um alle Spalten zu entfernen, die Sie nicht für die Transformation benötigen.

  ![Auswahltransformation](media/data-flow/fwselect.png)

14. Zum Schluss verwenden Sie „Senke“, um die Daten in einem Ordner auszugeben:

  ![Senke mit fester Breite](media/data-flow/fwsink.png)

  Die Ausgabe sieht wie folgt aus:

  ![Ausgabe mit fester Breite](media/data-flow/fxdoutput.png)

  Die Daten mit fester Breite sind nun in jeweils vier Zeichen unterteilt und „Col1“, „Col2“, „Col3“, „Col4“ usw. zugeordnet. Basierend auf dem obigen Beispiel werden die Daten in vier Spalten aufgeteilt.

## <a name="next-steps"></a>Nächste Schritte

* Erstellen der restlichen Datenflusslogik mithilfe von Mapping Data Flow-[Transformationen](concepts-data-flow-overview.md)

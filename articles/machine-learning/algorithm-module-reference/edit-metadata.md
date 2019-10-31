---
title: 'Bearbeiten von Metadaten: Modulreferenz'
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie Metadaten, die Spalten in einem Dataset zugeordnet sind, mit dem Modul „Metadaten bearbeiten“ im Azure Machine Learning Service ändern können.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: b6e28577fefe4892d719e211e16edafed1504e87
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72693825"
---
# <a name="edit-metadata-module"></a>Modul „Metadaten bearbeiten“

In diesem Artikel wird ein Modul der grafischen Benutzeroberfläche (Vorschau) für den Azure Machine Learning Service beschrieben.

Verwenden Sie das Modul „Metadaten bearbeiten“, um Metadaten zu ändern, die Spalten in einem Dataset zugeordnet sind. Nach Verwendung des Moduls „Metadaten bearbeiten“ werden Wert und Datentyp des Datasets geändert.

Typische Metadatenänderungen können Folgendes umfassen:
  
+ Behandeln boolescher oder numerischer Spalten als kategorische Werte
  
+ Angeben, welche Spalte die Bezeichnung **Klasse** oder die Werte enthält, die Sie kategorisieren oder prognostizieren möchten
  
+ Markieren von Spalten als Features
  
+ Ändern von Datum/Uhrzeit-Werten in numerische Werte oder umgekehrt
  
+ Umbenennen von Spalten
  
 Verwenden Sie „Metadaten bearbeiten“ immer dann, wenn Sie die Definition einer Spalte ändern müssen, in der Regel, um die Anforderungen eines nachgeschalteten Moduls zu erfüllen. Einige Module verarbeiten beispielsweise nur bestimmte Datentypen oder erfordern Flags für Spalten, wie z.B. `IsFeature` oder `IsCategorical`.  
  
 Nachdem Sie den erforderlichen Vorgang ausgeführt haben, können Sie die Metadaten in ihren ursprünglichen Zustand zurücksetzen.
  
## <a name="configure-edit-metadata"></a>Konfigurieren von „Metadaten bearbeiten“
  
1. Fügen Sie Ihrer Pipeline in Azure Machine Learning das Modul „Metadaten bearbeiten“ hinzu, und verknüpfen Sie das Dataset, das Sie aktualisieren möchten. Sie finden das Dataset unter **Datentransformation** in der Kategorie **Bearbeiten**.
  
1. Klicken Sie auf **Spaltenauswahl starten**, und wählen Sie die Spalte oder die Gruppe von Spalten aus, mit denen Sie arbeiten möchten. Sie können Spalten einzeln nach Name oder Index auswählen, oder Sie können eine Gruppe von Spalten nach Typ auswählen.  
  
1. Wählen Sie die Option **Datentyp** aus, wenn Sie den ausgewählten Spalten einen anderen Datentyp zuweisen müssen. Möglicherweise müssen Sie den Datentyp für bestimmte Vorgänge ändern. Wenn Ihr Quelldataset beispielsweise Zahlen enthält, die wie Text behandelt werden, müssen Sie diese vor der Verwendung von mathematischen Operationen in einen numerischen Datentyp ändern.

    + Die unterstützten Datentypen sind **String**, **Integer**, **Double**, **Boolean** und **DateTime**.

    + Wenn Sie mehrere Spalten auswählen, müssen Sie die Metadatenänderungen auf *alle* ausgewählten Spalten anwenden. Angenommen, Sie wählen zwei oder drei numerische Spalten aus. Sie können alle gleichzeitig in den Zeichenfolgendatentyp (String) ändern und umbenennen. Sie können jedoch nicht eine Spalte in einen Zeichenfolgendatentyp und eine andere Spalte in von einem Gleitkommawert in eine Ganzzahl ändern.
  
    + Wenn Sie keinen neuen Datentyp angeben, bleiben die Spaltenmetadaten unverändert.

    + Der Spaltentyp und die Werte werden geändert, nachdem Sie den Vorgang „Metadaten bearbeiten“ ausgeführt haben. Sie können den ursprünglichen Datentyp jederzeit wiederherstellen, indem Sie „Metadaten bearbeiten“ verwenden, um den Spaltendatentyp zurückzusetzen.  

    > [!NOTE]
    > Wenn Sie einen beliebigen Zahlentyp in den Datentyp **DateTime** ändern, lassen Sie das Feld **Format für „DateTime“** leer. Aktuell ist es nicht möglich, das Zieldatenformat festzulegen.  

1. Wählen Sie die Option **Kategorie** aus, um festzulegen, dass die Werte in den ausgewählten Spalten als Kategorien behandelt werden sollen.

    Angenommen, Sie verfügen über eine Spalte, die die Zahlen 0, 1 und 2 enthält, aber Sie wissen, dass die Zahlen eigentlich für „Raucher“, „Nichtraucher“ und „Unbekannt“ stehen. In diesem Fall stellen Sie durch Kennzeichnen der Spalte als Kategorie sicher, dass die Werte nur zur Gruppierung von Daten und nicht in numerischen Berechnungen verwendet werden.
  
1. Verwenden Sie die Option **Felder**, wenn Sie die Art und Weise ändern möchten, wie Azure Machine Learning die Daten in einem Modell verwendet.

    + **Feature**: Verwenden Sie diese Option, um eine Spalte als Feature in Modulen zu kennzeichnen, die nur mit Featurespalten arbeiten. Standardmäßig werden alle Spalten zunächst als Features behandelt werden.  
  
    + **Bezeichnung:** Verwenden Sie diese Option, um die Bezeichnung zu markieren. Dies wird auch als vorhersagbares Attribut oder Zielvariable bezeichnet. Viele Module erfordern, dass genau eine Bezeichnungsspalte im Datensatz vorhanden ist.

        In vielen Fällen kann Azure Machine Learning ableiten, dass eine Spalte eine Klassenbezeichnung enthält. Durch Festlegen dieser Metadaten können Sie sicherstellen, dass die Spalte richtig identifiziert wird. Durch Festlegen dieser Option werden keine Datenwerte geändert. Es wird nur die Art und Weise geändert, wie bestimmte Machine Learning-Algorithmen die Daten behandeln.
  
    > [!TIP]
    > Verfügen Sie über Daten, die nicht in diese Kategorien passen? Ihr Dataset könnte beispielsweise Werte wie eindeutige Bezeichner (IDs) enthalten, die nicht als Variablen verwendet werden können. Manchmal können solche IDs bei der Verwendung in einem Modell Probleme verursachen.
    >
    > Glücklicherweise behält Azure Machine Learning all Ihre Daten bei, sodass Sie solche Spalten nicht aus dem Dataset löschen müssen. Wenn Sie Operationen mit einem speziellen Satz von Spalten durchführen müssen, entfernen Sie einfach vorübergehend alle anderen Spalten unter Verwendung des Moduls [Spalten im Dataset auswählen](select-columns-in-dataset.md). Später können Sie die Spalten mit dem Modul [Spalten hinzufügen](add-columns.md) wieder in das Dataset einfügen.  
  
1. Verwenden Sie die folgenden Optionen, um vorherige Auswahlen aufzuheben und Metadaten auf die Standardwerte zurückzusetzen.  
  
    + **Feature löschen**: Verwenden Sie diese Option, um das Featureflag zu entfernen.  
  
         Alle Spalten werden zunächst als Features behandelt. Möglicherweise müssen Sie für Module, die mathematische Operationen ausführen, diese Option verwenden, um zu verhindern, dass numerische Spalten als Variablen behandelt werden.
  
    + **Bezeichnung löschen**: Verwenden Sie diese Option, um die Metadaten der **Bezeichnung** aus der angegebenen Spalte zu entfernen.  
  
    + **Bewertung löschen**: Verwenden Sie diese Option, um die Metadaten der **Bewertung** aus der angegebenen Spalte zu entfernen.  
  
         Derzeit können Sie in Azure Machine Learning eine Spalte nicht explizit als Bewertung markieren. Einige Vorgänge führen jedoch zu einer Spalte, die intern als Bewertung gekennzeichnet wird. Darüber hinaus kann ein benutzerdefiniertes R-Modul Bewertungswerte ausgeben.

1. Geben Sie für **Neue Spaltennamen** den neuen Namen der ausgewählten Spalte oder Spalten ein.  
  
    + Spaltennamen dürfen nur Zeichen enthalten, die von UTF-8-Codierung unterstützt werden. Leere Zeichenfolgen, NULL-Werte oder Namen, die vollständig aus Leerzeichen bestehen, sind nicht zulässig.  
  
    + Um mehrere Spalten umzubenennen, geben Sie die Namen als kommagetrennte Liste in der Reihenfolge der Spaltenindizes ein.  
  
    + Alle ausgewählte Spalten müssen umbenannt werden. Sie dürfen keine Spalten weglassen oder überspringen.  
  
1. Ausführen der Pipeline.  

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Gruppe der verfügbaren Module](module-reference.md) für den Azure Machine Learning Service an.

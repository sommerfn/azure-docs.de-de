---
title: 'Azure Data Factory Mapping Data Flow: Auswahltransformation'
description: 'Azure Data Factory Mapping Data Flow: Auswahltransformation'
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 6ef9712dd2fd6b8d53fd4ad2c3e07e1d6c8f1aec
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72387198"
---
# <a name="mapping-data-flow-select-transformation"></a>Mapping Data Flow: Auswahltransformation


Verwenden Sie diese Transformation für die Spaltenselektivität (Reduzierung der Anzahl von Spalten), um Aliase für Spalten und Datenstromnamen zuzuweisen und um Spalten neu anzuordnen.

## <a name="how-to-use-select-transformation"></a>Verwendung der Option „Select Transformation“ (Transformation auswählen)
Mit der Auswahltransformation können Sie einem gesamten Datenstrom oder Spalten in diesem Datenstrom einen Alias zuweisen, unterschiedliche Namen (Aliase) zuweisen und dann in Ihrem Datenfluss auf diese neuen Namen verweisen. Diese Transformation ist für Selbstverknüpfungsszenarien nützlich. Die Methode zum Implementieren einer Selbstverknüpfung in ADF-Datenfluss besteht darin, einen Datenstrom auszuwählen, ihn mit „Neue Verzweigung“ zu verzweigen und dann sofort danach eine Auswahltransformation hinzuzufügen. Dieser Datenstrom weist nun einen neuen Namen auf, mit dem Sie wieder eine Verknüpfung mit dem ursprünglichen Datenstrom herstellen können und dabei eine Selbstverknüpfung erstellen:

![Selbstverknüpfung](media/data-flow/selfjoin.png "Selbstverknüpfung")

Im obigen Diagramm befindet sich die SELECT-Transformation ganz oben. Dem ursprünglichen Datenstrom wird der Alias „OrigSourceBatting“ zugewiesen. In der hervorgehobenen Verknüpfungstransformation darunter können Sie sehen, dass wir diesen SELECT-Aliasdatenstrom der Auswahltransformation als rechte Verknüpfung verwenden, sodass wir sowohl auf der linken als auch auf der rechten Seite (des inneren Joins) auf den gleichen Schlüssel verweisen können.

Die Auswahltransformation kann auch als Möglichkeit verwendet werden, die Auswahl von Spalten in Ihrem Datenfluss aufzuheben. Wenn in Ihrer Senke beispielsweise 6 Spalten definiert sind, Sie aber nur 3 bestimmte Spalten für die Transformation auswählen und dann zur Senke fließen lassen möchten, können Sie mit der Auswahltransformation nur diese 3 Spalten auswählen.

![Auswahltransformation](media/data-flow/newselect1.png "Auswählen des Alias")

## <a name="options"></a>Optionen
* In der Standardeinstellung für die Auswahl werden alle eingehenden Spalten einbezogen und die ursprünglichen Namen beibehalten. Sie können dem Datenstrom einen Alias zuweisen, indem Sie den Namen der Auswahltransformation festlegen.
* Wenn Sie einzelnen Spalten Aliase zuweisen möchten, deaktivieren Sie „Alle auswählen“, und verwenden Sie im unteren Bereich die Spaltenzuordnung.
* Wählen Sie die Option „Skip Duplicates“ (Duplikate überspringen) aus, um doppelte Spalten aus Eingabe- oder Ausgabemetadaten zu entfernen.

![Überspringen von Duplikaten](media/data-flow/select-skip-dup.png "Überspringen von Duplikaten")

* Wenn Sie das Überspringen von Duplikaten auswählen, werden die Ergebnisse auf der Registerkarte „Untersuchen“ angezeigt. ADF behält das erste Vorkommen der Spalte bei. Sie werden feststellen, dass jedes nachfolgende Vorkommen der gleichen Spalte aus dem Flow entfernt wurde.

> [!NOTE]
> Zum Löschen von Zuordnungsregeln klicken Sie auf die Schaltfläche **Zurücksetzen**.

## <a name="mapping"></a>Zuordnung
Die Auswahltransformation ordnet alle Spalten standardmäßig automatisch zu, wodurch alle eingehenden Spalten an denselben Namen in der Ausgabe weitergeleitet werden. Der Name des Ausgabedatenstroms, der in „Select Settings“ (Auswahleinstellungen) festgelegt wird, definiert einen neuen Aliasnamen für den Datenstrom. Wenn Sie die für die automatische Zuordnung festgelegte Auswahl beibehalten, können Sie dem gesamten Datenstrom bei allen Spalten einen identischen Alias zuweisen.

![Regeln für die Auswahltransformation](media/data-flow/rule2.png "Regelbasierte Zuordnung")

Wenn Sie Spalten einen Alias zuweisen oder aber Spalten entfernen, umbenennen oder neu anordnen möchten, müssen Sie zuerst „Automatisch zuordnen“ deaktivieren. Standardmäßig wird eine automatisch eingegebene Standardregel namens „Alle Eingabespalten“ angezeigt. Sie können diese Regel beibehalten, wenn Sie möchten, dass alle eingehenden Spalten immer demselben Namen in der Ausgabe zugeordnet werden.

Wenn Sie aber benutzerdefinierte Regeln hinzufügen möchten, klicken Sie auf „Zuordnung hinzufügen“. Durch die Feldzuordnung erhalten Sie eine Liste mit den Namen der ein- und ausgehenden Spalten für die Zuordnung und den Alias. Wählen Sie „regelbasierte Zuordnung“, um Musterabgleichsregeln zu erstellen.

## <a name="rule-based-mapping"></a>Regelbasierte Zuordnung
Wenn Sie die regelbasierte Zuordnung auswählen, weisen Sie ADF an, Ihren Abgleichsausdruck auszuwerten, um passende Eingangsmusterregeln zu ermitteln und die Ausgangsfeldnamen zu definieren. Sie können eine beliebige Kombination aus feld- und regelbasierten Zuordnungen hinzufügen. Feldnamen werden dann von ADF zur Laufzeit auf der Grundlage eingehender Metadaten aus der Quelle generiert. Sie können die Namen der generierten Felder während des Debuggens sowie im Datenvorschaubereich anzeigen.

Weitere Informationen zum Musterabgleich finden Sie in der [Dokumentation zu Spaltenmustern](concepts-data-flow-column-pattern.md).

## <a name="next-steps"></a>Nächste Schritte
* Nachdem Sie die Option „Select“ (Auswählen) verwendet haben, um Spalten umzubenennen, neu anzuordnen und Aliase zuzuweisen, müssen Sie die [Sink transformation (Senkentransformation)](data-flow-sink.md) verwenden, um Ihre Daten im Datenspeicher zu speichern.

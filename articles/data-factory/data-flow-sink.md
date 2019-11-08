---
title: Einrichten einer Senkentransformation im Mappingdatenfluss-Feature von Azure Data Factory
description: Hier erfahren Sie, wie Sie im Mappingdatenfluss eine Senkentransformation einrichten.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: 7cfe0cf291e8c39a4600234632090c39ab5cd78e
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73519328"
---
# <a name="sink-transformation-for-a-data-flow"></a>Senkentransformation für einen Datenfluss

Nach der Transformation Ihres Datenflusses können Sie die Daten in ein Zieldataset weiterleiten. Wählen Sie in der Senkentransformation die Datasetdefinition für die Zielausgabedaten aus. Sie können so viele Senkentransformationen einrichten, wie Ihr Datenfluss erfordert.

Um Schemaabweichungen und Änderungen an eingehenden Daten zu berücksichtigen, leiten Sie die Ausgabedaten ohne ein definiertes Schema im Ausgabedataset an einen Ordner weiter. Außerdem können Sie für eventuelle Spaltenänderungen in Ihren Quellen die Option **Schemaabweichung zulassen** auswählen. Führen Sie dann eine automatische Zuordnung aller Felder in der Senke aus.

![Optionen auf der Registerkarte „Senke“, einschließlich der Option „Automatische Zuordnung“](media/data-flow/sink1.png "Senke 1")

Wenn alle eingehenden Felder an diese Senke weitergeleitet werden sollen, aktivieren Sie **Automatische Zuordnung**. Um die Zielfelder am Ziel auszuwählen oder die Namen der Felder am Ziel zu ändern, deaktivieren Sie **Automatische Zuordnung**. Öffnen Sie dann die Registerkarte **Zuordnung**, um die Ausgabefelder zuzuordnen.

![Optionen auf der Registerkarte „Zuordnung“](media/data-flow/sink2.png "Senke 2")

## <a name="output"></a>Output 
Für Senken des Typs Azure Blob Storage oder Data Lake Storage geben Sie die transformierten Daten in einen Ordner aus. Spark generiert partitionierte Ausgabedatendateien basierend auf dem Partitionsschema, das in der Senkentransformation verwendet wird. 

Sie können das Partitionsschema auf der Registerkarte **Optimieren** einrichten. Wenn Sie Ihre Ausgabe mit Data Factory in einer einzelnen Datei zusammenführen möchten, wählen Sie **Einzelne Partition** aus.

![Optionen auf der Registerkarte „Optimieren“](media/data-flow/opt001.png "Senkenoptionen")

## <a name="field-mapping"></a>Feldzuordnung
Auf der Registerkarte **Zuordnung** Ihrer Senkentransformation können Sie die eingehenden Spalten auf der linken Seite dem jeweiligen Ziel auf der rechten Seite zuordnen. Wenn Sie Datenflüsse in Dateien weiterleiten, schreibt Data Factory immer neue Dateien in einen Ordner. Bei der Zuordnung zu einem Datenbank-Dataset wählen Sie Optionen für Datenbanktabellenvorgänge aus, um Einfüge-, Aktualisierungs-, Upsert- oder Löschvorgänge auszuführen.

![Registerkarte „Zuordnung“](media/data-flow/sink2.png "Senken")

Sie können mit der Mehrfachauswahl in der Zuordnungstabelle mehrere Spalten verknüpfen, die Verknüpfung mehrerer Spalten aufheben oder mehrere Zeilen demselben Spaltennamen zuordnen.

Um den eingehenden Satz von Feldern immer unverändert einem Ziel zuzuordnen und flexible Schemadefinitionen vollständig zu akzeptieren, wählen Sie **Schemaabweichung zulassen** aus.

![Registerkarte „Zuordnung“ mit Feldern, die Spalten im Dataset zugeordnet sind](media/data-flow/multi1.png "Mehrere Optionen")

Wählen Sie zum Zurücksetzen Ihrer Spaltenzuordnungen **Re-map** (Neu zuordnen) aus.

![Registerkarte „Senke“](media/data-flow/sink1.png "Senke 1")

Wählen Sie **Schema überprüfen** aus, damit die Senke einen Fehler auslöst, wenn das Schema geändert wird.

Wählen Sie **Clear the folder** (Ordner leeren) aus, um den Inhalt des Senkenordners abzuschneiden, bevor die Zieldateien in diesen Zielordner geschrieben werden.

## <a name="fixed-mapping-vs-rule-based-mapping"></a>Feste Zuordnung und regelbasierte Zuordnung
Wenn Sie die automatische Zuordnung deaktivieren, können Sie eine spaltenbasierte Zuordnung (feste Zuordnung) oder eine regelbasierte Zuordnung hinzuzufügen. Die regelbasierte Zuordnung ermöglicht Ihnen das Schreiben von Ausdrücken mit Musterabgleich, während bei der festen Zuordnung logische und physische Spaltennamen zugeordnet werden.

![Regelbasierte Zuordnung](media/data-flow/rules4.png "Regelbasierte Zuordnung")

Wenn Sie die regelbasierte Zuordnung auswählen, weisen Sie ADF an, Ihren Abgleichsausdruck auszuwerten, um passende Eingangsmusterregeln zu ermitteln und die Ausgangsfeldnamen zu definieren. Sie können eine beliebige Kombination aus feld- und regelbasierten Zuordnungen hinzufügen. Feldnamen werden dann von ADF zur Laufzeit auf der Grundlage eingehender Metadaten aus der Quelle generiert. Sie können die Namen der generierten Felder während des Debuggens sowie im Datenvorschaubereich anzeigen.

Details zum Musterabgleich finden Sie in der [Dokumentation zu Spaltenmustern](concepts-data-flow-column-pattern.md).

Beim regelbasierten Abgleich können Sie auch Muster mit regulären Ausdrücken eingeben. Dazu erweitern Sie die Zeile und geben einen regulären Ausdruck neben „Namensübereinstimmungen:“ ein.

![Zuordnung mit regulärem Ausdruck](media/data-flow/scdt1g4.png "Zuordnung mit regulärem Ausdruck")

Ein sehr einfaches gängiges Beispiel zur Gegenüberstellung von regelbasierter Zuordnung und fester Zuordnung ist der Fall, in dem Sie alle eingehenden Felder demselben Namen im Ziel zuordnen möchten. Bei der festen Zuordnung listen Sie jede einzelne Spalte in der Tabelle auf. Bei der regelbasierten Zuordnung haben Sie eine einzelne Regel, die alle Felder mithilfe von ```true()``` dem gleichen eingehenden Feldnamen zuordnet, der durch ```$$``` dargestellt wird.

### <a name="sink-association-with-dataset"></a>Senkenzuordnung mit Dataset

Für das Dataset, das Sie für die Senke auswählen, ist möglicherweise ein Schema in der Datasetdefinition festgelegt. Wenn kein Schema definiert ist, müssen Sie eine Schemaabweichung zulassen. Wenn Sie eine feste Zuordnung definiert haben, wird die Zuordnung zwischen logischen und physischen Namen bei der Senkentransformation beibehalten. Wenn Sie die Schemadefinition des Datasets ändern, unterbrechen Sie möglicherweise die Senkenzuordnung. Um dies zu vermeiden, verwenden Sie die regelbasierte Zuordnung. Regelbasierte Zuordnungen werden generalisiert, das heißt, dass Schemaänderungen im Dataset keine Unterbrechung der Zuordnung bewirken.

## <a name="file-name-options"></a>Dateinamenoptionen

Richten Sie die Dateibenennung ein: 

   * **Standard:** Lassen Sie zu, dass Spark Dateien basierend auf den PART-Standards benennt.
   * **Muster:** Geben Sie ein Muster für Ihre Ausgabedateien ein. Zum Beispiel erstellt **loans[n]** die Dateien „loans1.csv“, „loans2.csv“ usw.
   * **Pro Partition:** Geben Sie einen Dateinamen pro Partition ein.
   * **Wie Daten in Spalte:** Legen Sie die Ausgabedatei auf den Wert einer Spalte fest.
   * **Ausgabe in eine einzelne Datei:** Mit dieser Option kombiniert ADF die partitionierten Ausgabedateien in einer einzelnen Datei. Um diese Option verwenden zu können, sollte Ihr Dataset in einen Ordnernamen aufgelöst werden. Bedenken Sie auch, dass dieser Zusammenführungsvorgang je nach Knotengröße zu Fehlern führen kann.

> [!NOTE]
> Dateivorgänge werden nur gestartet, wenn Sie die Aktivität zur Datenflussausführung ausführen. Im Datenfluss-Debugmodus werden sie nicht gestartet.

## <a name="database-options"></a>Datenbankoptionen

Wählen Sie die Datenbankeinstellungen aus:

![Registerkarte „Einstellungen“ mit Optionen für SQL-Senken](media/data-flow/alter-row2.png "SQL-Optionen")

* **Updatemethode:** Die Standardeinstellung ist das Zulassen von Einfügevorgängen. Deaktivieren Sie **Allow insert** (Einfügen zulassen), wenn keine neuen Zeilen aus der Quelle mehr eingefügt werden sollen. Wenn Sie für Zeilen Update-, Upsert- oder Löschvorgänge verwenden möchten, fügen Sie zunächst eine Transformation zur Änderung von Zeilen hinzu, um Zeilen für diese Aktionen zu kennzeichnen. 
* **Recreate table** (Tabelle neu erstellen): Löschen oder erstellen Sie die Zieltabelle, bevor der Datenfluss abgeschlossen wird.
* **Truncate table** (Tabelle abschneiden): Entfernen Sie alle Zeilen aus der Zieltabelle, bevor der Datenfluss abgeschlossen wird.
* **Batchgröße**: Eine Zahl eingeben, um Schreibvorgänge in Blöcke zu unterteilen. Verwenden Sie diese Option für große Datenmengen. 
* **Enable staging** (Staging aktivieren): Verwenden Sie bei Azure Data Warehouse als Senkendataset PolyBase.
* **Pre- und Post SQL-Skripts**: Geben Sie mehrzeilige SQL-Skripts ein, die ausgeführt werden, bevor Daten in die Senkendatenbank geschrieben werden (Vorverarbeitung) und danach (Nachbearbeitung).

![Vorverarbeitungs- und Nachbearbeitungs-SQL-Skripts](media/data-flow/prepost1.png "SQL-Verarbeitungsskripts")

> [!NOTE]
> Sie können Data Factory im Datenfluss anweisen, eine neue Tabellendefinition in der Zieldatenbank zu erstellen. Um die Tabellendefinition zu erstellen, legen Sie ein Dataset in der Senkentransformation mit einem neuen Tabellennamen fest. Wählen Sie im SQL-Dataset unter dem Tabellennamen **Bearbeiten** aus, und geben Sie einen neuen Tabellennamen ein. Aktivieren Sie dann in der Senkentransformation **Schemaabweichung zulassen**. Legen Sie **Schema importieren** auf **Nein** fest.

![SQL-Dataseteinstellungen mit dem Ort für die Bearbeitung des Tabellennamens](media/data-flow/dataset2.png "SQL-Schema")

> [!NOTE]
> Beim Aktualisieren oder Löschen von Zeilen in Ihrer Datenbanksenke müssen Sie die Schlüsselspalte festlegen. Diese Einstellung ermöglicht die Transformation zur Zeilenänderung, um die eindeutige Zeile in der Datenverschiebungsbibliothek (Data Movement Library, DML) zu bestimmen.

### <a name="cosmosdb-specific-settings"></a>CosmosDB-spezifische Einstellungen

Beim Anordnen von Daten in CosmosDB müssen Sie die folgenden zusätzlichen Optionen beachten:

* Partitionsschlüssel: Dies ist ein Pflichtfeld. Geben Sie eine Zeichenfolge ein, die den Partitionsschlüssel für Ihre Sammlung darstellt. Beispiel: ```/movies/title```
* Durchsatz: Legen Sie einen optionalen Wert für die Anzahl von RUs fest, die Sie für jede Ausführung dieses Datenflusses auf die CosmosDB-Sammlung anwenden möchten. Der Mindestwert ist 400.

## <a name="next-steps"></a>Nächste Schritte
Da Sie nun Ihren Datenfluss erstellt haben, fügen Sie [Ihrer Pipeline eine Aktivität zur Ausführung eines Datenflusses](concepts-data-flow-overview.md) hinzu.

---
title: Verhindern von Speicherlimits und Experimentlatenz mit Eingabe- und Ausgabeverzeichnissen
description: In diesem Artikel erfahren Sie, wo Sie Ihre Experimenteingabedateien speichern sollten, und wohin Sie Ausgabedateien schreiben sollten, um Speicherlimits und Experimentlatenz zu vermeiden.
services: machine-learning
author: rastala
ms.author: roastala
manager: danielsc
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 05/28/2019
ms.openlocfilehash: 28d8c47db8ea9c8a51bc8e9deb0a689eb0b20177
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/30/2019
ms.locfileid: "66392907"
---
# <a name="where-to-save-and-write-files-for-azure-machine-learning-experiments"></a>Verzeichnisse zum Speichern und Schreiben von Dateien für Azure Machine Learning-Experimente

In diesem Artikel erfahren Sie, wo Sie Ihre Eingabedateien speichern sollten, und wohin Sie Ausgabedateien Ihrer Experimente schreiben sollten, um Speicherlimitfehler und Experimentlatenz zu vermeiden.

Bei Beginn von Trainingsausführungen auf einem [Computeziel](how-to-set-up-training-targets.md) sind sie von externen Umgebungen isoliert. Der Zweck dieses Entwurfs besteht darin, die Reproduzierbarkeit und Portabilität des Experiments sicherzustellen. Wenn Sie dasselbe Skript auf demselben bzw. einem anderen Computeziel zweimal ausführen, erhalten Sie die gleichen Ergebnisse. Mit diesem Entwurf können Sie Computeziele als zustandslose Berechnungsressourcen betrachten, die jeweils keine Affinität zu den Aufträgen aufweisen, die nach deren Abschluss ausgeführt werden.

## <a name="where-to-save-input-files"></a>Speicherort von Eingabedateien

Bevor Sie ein Experiment auf einem Computeziel oder Ihrem lokalen Computer initiieren können, müssen Sie sicherstellen, dass die erforderlichen Dateien, z.B. Abhängigkeitsdateien und Datendateien, die Ihr Code zum Ausführen benötigt, für das Computeziel verfügbar sind.

Azure Machine Learning trainingsskripts durch Kopieren des gesamten Skriptordner im Ziel-computekontext ausgeführt, und klicken Sie dann eine Momentaufnahme. Die speicherbegrenzung für Momentaufnahmen des Experiments ist 300 MB bzw. 2000-Dateien.

Aus diesem Grund empfehlen wir folgende Aktionen ausführen:

* **Das Speichern der Dateien in einem Azure Machine Learning [Datenspeicher](https://docs.microsoft.com/python/api/azureml-core/azureml.data?view=azure-ml-py).** Dies verhindert, dass Experiment Latenzprobleme und hat den Vorteil, den Zugriff auf Daten von einem remote-Compute-Ziel, was bedeutet, dass die Authentifizierung und Bereitstellung von Azure Machine Learning-Dienst verwaltet werden. Weitere Informationen zum Angeben eines Datenspeichers als Ihr Quellverzeichnis und Hochladen von Dateien in Ihren Datenspeicher finden Sie im Artikel [Zugreifen auf Daten aus Ihren Datenspeichern](how-to-access-data.md).

* **Wenn lediglich ein Paar von Datendateien und Abhängigkeit Skripts und kann nicht verwendet einen Datenspeicher,** platzieren Sie die Dateien im selben Ordnerverzeichnis wie Ihr trainingsskript. Geben Sie diesen Ordner direkt in Ihrem Trainingsskript als Ihr `source_directory` an, oder in dem Code, der Ihr Trainingsskript aufruft.

<a name="limits"></a>

### <a name="storage-limits-of-experiment-snapshots"></a>Speicherlimits von Experimentmomentaufnahmen

Für Experimente erstellt Azure Machine Learning automatisch eine Momentaufnahme des Experiments Ihres Codes basierend auf das Verzeichnis, die, das Sie vorschlagen, wenn Sie die Ausführung konfigurieren. Hierfür gilt ein Grenzwert von insgesamt 300MB und/oder 2.000 Dateien. Wenn Sie diese Grenze überschreiten, sehen Sie folgende Fehlermeldung:

```Python
While attempting to take snapshot of .
Your total snapshot size exceeds the limit of 300.0 MB
```

Speichern Sie Ihr Experiment-Dateien in einem Datenspeicher, um diesen Fehler zu beheben. Wenn Sie einen Datenspeicher, verwenden, können die folgende Tabelle bietet mögliche alternative Lösungen.

Experiment&nbsp;description|Lösung zum Speicherlimit
---|---
Weniger als 2000 Dateien & einen Datenspeicher kann nicht verwendet werden.| Überschreiben Sie die Snapshot-größenbeschränkung mit <br> `azureml._restclient.snapshots_client.SNAPSHOT_MAX_SIZE_BYTES = 'insert_desired_size'`<br> Dies kann je nach Anzahl und Größe der Dateien mehrere Minuten dauern.
Spezifisches Skriptverzeichnis muss verwendet werden| Stellen Sie eine `.amlignore` Datei ausschließen von Dateien aus der Momentaufnahme des Experiments, die nicht Teil des Quellcodes. Fügen Sie die Dateinamen, die `.amlignore` Datei, und platzieren Sie es im gleichen Verzeichnis wie Ihr trainingsskript. Die `.amlignore` Datei verwendet die gleichen [Syntax und Muster](https://git-scm.com/docs/gitignore) als eine `.gitignore` Datei.
Pipeline|Verwenden von einem anderen Unterverzeichnis für jeden Schritt
Jupyter Notebooks| Erstellen Sie eine `.amlignore` Datei oder Ihrem Notebook in einem neuen, leeren Unterverzeichnis verschieben, und führen Sie den Code erneut aus.

## <a name="where-to-write-files"></a>Speicherort zum Schreiben von Dateien

Aufgrund der Isolation von Trainingsexperimenten werden die während Ausführungen auftretenden Änderungen an Dateien nicht unbedingt außerhalb Ihrer Umgebung beibehalten. Wenn Ihr Skript die Dateien, die lokale compute ändert, die Änderungen werden nicht beibehalten, für das nächste Experiment ausführen und werden nicht weitergegeben, zurück an den Clientcomputer automatisch. Aus diesem Grund die Änderungen während des ersten Experiments ausführen nicht und dürfen nicht die in der zweiten beeinträchtigen.

Wenn Sie Änderungen zu schreiben, wird empfohlen, Schreiben von Dateien auf einen Azure Machine Learning-Datenspeicher. Weitere Informationen finden Sie unter [Zugreifen auf Daten aus Ihren Datenspeichern](how-to-access-data.md).

Wenn Sie einen Datenspeicher nicht benötigen, Schreiben von Dateien, die `./outputs` und/oder `./logs` Ordner.

>[!Important]
> Zwei Ordner, *outputs* und *logs*, erhalten eine besondere Behandlung durch Azure Machine Learning. Wenn Sie während des Trainings Dateien in die Ordner `./outputs` und `./logs` schreiben, werden diese Dateien automatisch in Ihren Ausführungsverlauf hochgeladen, damit Sie Zugriff darauf haben, wenn die Ausführung abgeschlossen ist.

* **Für die Ausgabe wie statusmeldungen oder Bewertungsergebnisse** Schreiben von Dateien, die `./outputs` Ordner, sodass diese als Artefakte im Verlauf beibehalten werden. Achten Sie darauf, die Anzahl und Größe der Dateien, die in diesem Ordner geschrieben, wie Latenz auftreten kann, wenn der Inhalt hochgeladen werden, um den Ausführungsverlauf. Wenn Latenz ein Problem darstellt, wird das Schreiben von Dateien auf einen Datenspeicher empfohlen.

* **Zum Speichern von geschriebenen Datei als Protokolle im Ausführungsverlauf** Schreiben von Dateien auf `./logs` Ordner. Die Protokolle werden in Echtzeit hochgeladen, sodass diese Methode für das Streamen von Liveupdates von einer Remoteausführung geeignet ist.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über das [Zugreifen auf Daten aus Ihren Datenspeichern](how-to-access-data.md).

* Erfahren Sie mehr über das [Einrichten von Computezielen für das Modelltraining](how-to-set-up-training-targets.md).

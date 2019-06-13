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
ms.openlocfilehash: b9357c18784b0d731876d136f48d332191f054d1
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66355959"
---
# <a name="where-to-save-and-write-files-for-azure-machine-learning-experiments"></a>Verzeichnisse zum Speichern und Schreiben von Dateien für Azure Machine Learning-Experimente

In diesem Artikel erfahren Sie, wo Sie Ihre Eingabedateien speichern sollten, und wohin Sie Ausgabedateien Ihrer Experimente schreiben sollten, um Speicherlimitfehler und Experimentlatenz zu vermeiden.

Bei Beginn von Trainingsausführungen auf einem [Computeziel](how-to-set-up-training-targets.md) sind sie von externen Umgebungen isoliert. Der Zweck dieses Entwurfs besteht darin, die Reproduzierbarkeit und Portabilität des Experiments sicherzustellen. Wenn Sie dasselbe Skript auf demselben bzw. einem anderen Computeziel zweimal ausführen, erhalten Sie die gleichen Ergebnisse. Mit diesem Entwurf können Sie Computeziele als zustandslose Berechnungsressourcen betrachten, die jeweils keine Affinität zu den Aufträgen aufweisen, die nach deren Abschluss ausgeführt werden.

## <a name="where-to-save-input-files"></a>Speicherort von Eingabedateien

Bevor Sie ein Experiment auf einem Computeziel oder Ihrem lokalen Computer initiieren können, müssen Sie sicherstellen, dass die erforderlichen Dateien, z.B. Abhängigkeitsdateien und Datendateien, die Ihr Code zum Ausführen benötigt, für das Computeziel verfügbar sind.

Azure Machine Learning führt Trainingsskripts durch Kopieren des gesamten Skriptordners in den Computezielkontext aus. Aus diesem Grund empfehlen wir Ihnen die folgenden Methoden zum Speichern Ihrer Experimentdateien, die für Ihre Trainingsausführungen erforderlich sind.

* **Bei kleinen Daten- und Abhängigkeitsdateien:** Platzieren Sie die Dateien im selben Ordnerverzeichnis wie Ihr Trainingsskript. Geben Sie diesen Ordner direkt in Ihrem Trainingsskript als Ihr `source_directory` an, oder in dem Code, der Ihr Trainingsskript aufruft.

* **Bei großen Daten- und Abhängigkeitsdateien:** Speichern Sie Ihre Dateien in einem Azure Machine Learning-[Datenspeicher](https://docs.microsoft.com/python/api/azureml-core/azureml.data?view=azure-ml-py).  Dies hat die Vorteile des Datenzugriffs von einem Remotecomputeziel, was bedeutet, dass Funktionen wie Authentifizierung und Einbindung vom Azure Machine Learning Service verwaltet werden. Weitere Informationen zum Angeben eines Datenspeichers als Ihr Quellverzeichnis und Hochladen von Dateien in Ihren Datenspeicher finden Sie im Artikel [Zugreifen auf Daten aus Ihren Datenspeichern](how-to-access-data.md).

<a name="limits"></a>

### <a name="storage-limits-of-experiment-snapshots"></a>Speicherlimits von Experimentmomentaufnahmen

Für Experimente erstellt Azure Machine Learning auch eine Experimentmomentaufnahme Ihres Codes basierend auf dem Verzeichnis, das Sie bei der Konfiguration der Ausführung vorschlagen. Hierfür gilt ein Grenzwert von insgesamt 300MB und/oder 2.000 Dateien. Wenn Sie diese Grenze überschreiten, sehen Sie folgende Fehlermeldung:

```Python
While attempting to take snapshot of .
Your total snapshot size exceeds the limit of 300.0 MB
```

Probieren Sie zum Beheben des Fehlers eine der folgenden Lösungen aus.

Experiment&nbsp;Anforderung|Lösung zum Speicherlimit
---|---
Sehr große Dateien| Verschieben Sie Dateien in einem Datenspeicher, und geben Sie den Datenspeicher als Ihr „source_directory“ an, um Latenzprobleme zu verhindern, wenn der Skriptordner zur Laufzeit in die Computezielumgebung kopiert wird.
Viele/große Dateien, Datenspeicher jedoch nicht erwünscht| Überschreiben Sie das Limit durch Festlegen von SNAPSHOT_MAX_SIZE_BYTES auf den für Ihr Experiment benötigten Wert. <br> `azureml._restclient.snapshots_client.SNAPSHOT_MAX_SIZE_BYTES = 'insert_desired_size'`
Spezifisches Skriptverzeichnis muss verwendet werden| Erstellen Sie eine Ignorierdatei (`.gitignore` oder `.amlignore`), um zu verhindern, dass Dateien in die Momentaufnahme des Experiments eingeschlossen werden, die nicht wirklich ein Teil des Quellcodes sind. Platzieren Sie diese Datei in dem Verzeichnis, und fügen Sie die Dateinamen hinzu, um es zu ignorieren. Für die `.amlignore`-Datei werden die gleiche Syntax und die gleichen Muster wie für die `.gitignore`-Datei verwendet. Wenn beide Dateien vorhanden sind, hat die `.amlignore`-Datei Vorrang.
Pipeline|Verwenden Sie ein anderes Unterverzeichnis für jeden Schritt, oder erstellen Sie eine Ignorierdatei.
Jupyter Notebooks| Sie verwenden wahrscheinlich ein Verzeichnis, das mehr als 300MB an Daten oder Dateien enthält. Verschieben Sie Ihr Notebook mit den folgenden Schritten in ein neues, leeres Unterverzeichnis.  <br> 1. Erstellen Sie einen neuen Ordner.<br> 2. Verschieben Sie das Jupyter-Notebook in den leeren Ordner. <br> 3. Führen Sie den Code erneut aus.

## <a name="where-to-write-files"></a>Speicherort zum Schreiben von Dateien

Aufgrund der Isolation von Trainingsexperimenten werden die während Ausführungen auftretenden Änderungen an Dateien nicht unbedingt außerhalb Ihrer Umgebung beibehalten.
Wenn Ihr Skript die lokalen zu berechnenden Dateien ändert, werden die Änderungen für Ihre nächste Ausführung nicht beibehalten und auch nicht automatisch an den Clientcomputer zurückgeleitet. Darum sollten die während der ersten Ausführung des Experiments vorgenommenen Änderungen sich nicht auf die Änderungen der zweiten Ausführung auswirken.

Schreiben Sie Dateien an einen der folgenden Speicherorte:
>[!Important]
> Zwei Ordner, *outputs* und *logs*, erhalten eine besondere Behandlung durch Azure Machine Learning. Wenn Sie während des Trainings Dateien in die Ordner `./outputs` und `./logs` schreiben, werden diese Dateien automatisch in Ihren Ausführungsverlauf hochgeladen, damit Sie Zugriff darauf haben, wenn die Ausführung abgeschlossen ist.

 1. Schreiben Sie kleine Dateien in den „./outputs“-Ordner, damit sie als Artefakte im Ausführungsverlauf beibehalten werden. Wenn Sie große Dateien in den „./outputs“-Ordner schreiben, führt dies beim Hochladen der Inhalte des Ordners in den Ausführungsverlauf zu Latenz.

 1. Schreiben Sie große Dateien in den Azure Machine Learning-Datenspeicher. Weitere Informationen finden Sie unter [Zugreifen auf Daten aus Ihren Datenspeichern](how-to-access-data.md).

 1. Schreiben Sie Dateien in den „./logs“-Ordner, um sie als Protokolle im Ausführungsverlauf zu speichern. Die Protokolle werden in Echtzeit hochgeladen, sodass diese Methode für das Streamen von Liveupdates von einer Remoteausführung geeignet ist.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über das [Zugreifen auf Daten aus Ihren Datenspeichern](how-to-access-data.md).

* Erfahren Sie mehr über das [Einrichten von Computezielen für das Modelltraining](how-to-set-up-training-targets.md).

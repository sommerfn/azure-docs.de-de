---
title: Auslösen eines Batch-Auftrags mithilfe von Azure Functions
description: 'Tutorial: Anwenden von OCR auf gescannte Dokumente beim Hinzufügen zu einem Storage-Blob'
services: batch
author: laurenhughes
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 05/30/2019
ms.author: peshultz
ms.custom: mvc
ms.openlocfilehash: d5a5197227ff62ca0c610e2c4e269480690d3faf
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2019
ms.locfileid: "67343089"
---
# <a name="tutorial-trigger-a-batch-job-using-azure-functions"></a>Tutorial: Auslösen eines Batch-Auftrags mithilfe von Azure Functions

In diesem Tutorial erfahren Sie, wie Sie mithilfe von Azure Functions einen Batch-Auftrag auslösen. Wir sehen uns ein Beispiel an, bei dem die optische Zeichenerkennung (OCR) über Azure Batch auf Dokumente angewandt wird, die einem Azure Storage-Blobcontainer hinzugefügt werden. Zur Optimierung der OCR-Verarbeitung wird eine Azure-Funktion konfiguriert, die bei jedem Hinzufügen einer Datei zum Blobcontainer einen Batch-OCR-Auftrag ausführt.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie keins besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* Ein Azure Batch-Konto und ein verknüpftes Azure Storage-Konto. Weitere Informationen zum Erstellen und Verknüpfen von Konten finden Sie unter [Erstellen eines Batch-Kontos](quick-create-portal.md#create-a-batch-account).
* [Batch Explorer](https://azure.github.io/BatchExplorer/)
* [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/)

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-a-batch-pool-and-batch-job-using-batch-explorer"></a>Erstellen eines Batch-Pools und eines Batch-Auftrags mit Batch Explorer

In diesem Abschnitt erstellen Sie mit Batch Explorer den Batch-Pool und den Batch-Auftrag, über den OCR-Tasks ausgeführt werden. 

### <a name="create-a-pool"></a>Erstellen eines Pools

1. Melden Sie sich mit Ihren Azure-Anmeldeinformationen bei Batch Explorer an.
1. Erstellen Sie einen Pool, indem Sie auf der linken Seitenleiste die Option **Pools** und anschließend die Schaltfläche **Hinzufügen** über dem Suchformular auswählen. 
    1. Wählen Sie eine ID und einen Anzeigenamen aus. In diesem Beispiel wird `ocr-pool` verwendet.
    1. Legen Sie den Skalierungstyp auf **Feste Größe** und die Anzahl der dedizierten Knoten auf 3 fest.
    1. Wählen Sie **Ubuntu 18.04-LTS** als Betriebssystem aus.
    1. Wählen Sie `Standard_f2s_v2` als Größe des virtuellen Computers aus.
    1. Aktivieren Sie den Starttask, und fügen Sie den Befehl `/bin/bash -c "sudo update-locale LC_ALL=C.UTF-8 LANG=C.UTF-8; sudo apt-get update; sudo apt-get -y install ocrmypdf"` hinzu. Legen Sie die Benutzeridentität als **Task default user (Admin)** (Standardbenutzer für Tasks (Administrator)) fest, damit Starttasks Befehle mit `sudo` enthalten können.
    1. Klicken Sie auf **OK**.
### <a name="create-a-job"></a>Erstellen eines Auftrags

1. Erstellen Sie einen Auftrag im Pool, indem Sie **Aufträge** auf der linken Seitenleiste und anschließend die Schaltfläche **Hinzufügen** über dem Suchformular auswählen. 
    1. Wählen Sie eine ID und einen Anzeigenamen aus. In diesem Beispiel wird `ocr-job` verwendet.
    1. Legen Sie den Pool auf `ocr-pool` oder den ausgewählten Namen des Pools fest.
    1. Klicken Sie auf **OK**.


## <a name="create-blob-containers"></a>Erstellen von Blobcontainern

Hier erstellen Sie Blobcontainer, in denen die Eingabe- und Ausgabedateien für den OCR-Batch-Auftrag gespeichert werden.

1. Melden Sie sich mit Ihren Azure-Anmeldeinformationen bei Storage-Explorer an.
1. Erstellen Sie unter Verwendung des mit Ihrem Batch-Konto verknüpften Speicherkontos entsprechend den unter [Erstellen eines Blobcontainers](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#create-a-blob-container) beschriebenen Schritten zwei Blobcontainer (einen für Eingabedateien und einen für Ausgabedateien).

In diesem Beispiel hat der Eingabecontainer den Namen `input` und ist der Speicherort, in den alle Dokumente ohne OCR zunächst zur Verarbeitung hochgeladen werden. In den Ausgabecontainer mit dem Namen `output` schreibt der Batch-Auftrag die mit OCR verarbeiteten Dokumente.  
    * In diesem Beispiel hat der Eingabecontainer den Namen `input`, und der Ausgabecontainer heißt `output`.  
    * In den Eingabecontainer werden alle Dokumente ohne OCR zunächst zur Verarbeitung hochgeladen.  
    * In den Ausgabecontainer schreibt der Batch-Auftrag die mit OCR verarbeiteten Dokumente.  

Erstellen Sie in Storage-Explorer eine Shared Access Signature (SAS) für den Ausgabecontainer. Klicken Sie dazu mit der rechten Maustaste auf den Ausgabecontainer, und wählen Sie **Shared Access Signature abrufen** aus. Aktivieren Sie unter **Berechtigungen** die Option **Schreiben**. Es sind keine weiteren Berechtigungen erforderlich.  

## <a name="create-an-azure-function"></a>Erstellen einer Azure Function

In diesem Abschnitt erstellen Sie die Azure-Funktion, die den OCR-Batch-Auftrag auslöst, wenn eine Datei in den Eingabecontainer hochgeladen wird.

1. Führen Sie zum Erstellen der Funktion die Schritte unter [Erstellen einer Funktion, die durch Azure Blob Storage ausgelöst wird](https://docs.microsoft.com/azure/azure-functions/functions-create-storage-blob-triggered-function) aus.
    1. Wenn Sie zur Eingabe eines Speicherkontos aufgefordert werden, geben Sie das gleiche Speicherkonto an, das Sie mit Ihrem Batch-Konto verknüpft haben.
    1. Wählen Sie „.NET“ für **Runtimestapel** aus. Wir schreiben die Funktion in C#, um das Batch .NET SDK zu nutzen.
1. Verwenden Sie nach dem Erstellen der per Blob ausgelösten Funktion die Dateien [`run.csx`](https://github.com/Azure-Samples/batch-functions-tutorial/blob/master/run.csx) und [`function.proj`](https://github.com/Azure-Samples/batch-functions-tutorial/blob/master/function.proj) aus GitHub in der Funktion.
    * `run.csx` wird ausgeführt, wenn ein neues Blob im Eingabeblobcontainer hinzugefügt wird.
    * `function.proj` enthält die externen Bibliotheken im Funktionscode, z. B. das Batch .NET SDK.
1. Ändern Sie die Platzhalterwerte der Variablen in der `Run()`-Funktion in der Datei `run.csx` entsprechend Ihren Batch- und Storage-Anmeldeinformationen. Die Anmeldeinformationen für Ihr Batch- und Storage-Konto finden Sie im Azure-Portal im Abschnitt **Schlüssel** des Batch-Kontos.
    * Rufen Sie die Anmeldeinformationen für Ihr Batch- und Storage-Konto im Azure-Portal im Abschnitt **Schlüssel** des Batch-Kontos ab. 

## <a name="trigger-the-function-and-retrieve-results"></a>Auslösen der Funktion und Abrufen der Ergebnisse

Laden Sie beliebige gescannte Dateien aus dem Verzeichnis [`input_files`](https://github.com/Azure-Samples/batch-functions-tutorial/tree/master/input_files) in GitHub in Ihren Eingabecontainer hoch. Überprüfen Sie in Batch Explorer, ob für jede Datei ein Task in `ocr-pool` hinzugefügt wird. Nach wenigen Sekunden wird die Datei mit angewandter OCR im Ausgabecontainer hinzugefügt. Die Datei ist dann sichtbar und kann über Storage-Explorer abgerufen werden.

Zusätzlich können Sie die Protokolldateien unten im Azure Functions-Web-Editorfenster überprüfen. Hier werden Nachrichten wie die folgende für jede Datei angezeigt, die in den Eingabecontainer hochgeladen wird:

```
2019-05-29T19:45:25.846 [Information] Creating job...
2019-05-29T19:45:25.847 [Information] Accessing input container <inputContainer>...
2019-05-29T19:45:25.847 [Information] Adding <fileName> as a resource file...
2019-06-21T20:02:35.129 [Information] Name of output text file: <outputTxtFile>
2019-06-21T20:02:35.130 [Information] Name of output PDF file: <outputPdfFile>
2019-05-29T19:45:26.200 [Information] Adding OCR task <taskID> for <fileName> <size of fileName>...
```

Um die Ausgabedateien von Storage-Explorer auf den lokalen Computer herunterzuladen, wählen Sie zunächst die gewünschten Dateien und dann im oberen Menüband die Option **Herunterladen** aus. 

> [!TIP]
> Die heruntergeladenen Dateien können durchsucht werden, wenn sie in einem PDF-Reader geöffnet werden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt: 

> [!div class="checklist"]
> * Erstellen von Pools und Aufträgen mithilfe von Batch Explorer
> * Erstellen von Blobcontainern und einer Shared Access Signature (SAS) mithilfe von Storage-Explorer
> * Erstellen einer per Blob ausgelösten Azure-Funktion
> * Hochladen von Eingabedateien in Storage
> * Überwachen der Aufgabenausführung
> * Abrufen von Ausgabedateien

* Weitere Beispiele zur Verwendung der .NET-API zum Planen und Verarbeiten von Batch-Workloads finden Sie in den [Beispielen auf GitHub](https://github.com/Azure-Samples/azure-batch-samples/tree/master/CSharp). 

* Weitere Azure Functions-Auslöser, die Sie zum Ausführen von Batch-Workloads verwenden können, finden Sie in der [Dokumentation zu Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings).

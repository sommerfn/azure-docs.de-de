---
title: Erstellen einer Funktion in Azure, die durch Blob Storage ausgelöst wird | Microsoft-Dokumentation
description: Erstellen Sie mithilfe von Azure Functions eine serverlose Funktion, die aufgerufen wird, wenn Elemente zu Azure Blob Storage hinzugefügt werden.
services: azure-functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.assetid: d6bff41c-a624-40c1-bbc7-80590df29ded
ms.service: azure-functions
ms.topic: quickstart
ms.date: 10/01/2018
ms.author: glenga
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: b8855fa061e07ec77482ca960971db8252608b51
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70096878"
---
# <a name="create-a-function-triggered-by-azure-blob-storage"></a>Erstellen einer Funktion, die durch Azure Blob Storage ausgelöst wird

Erfahren Sie, wie Sie eine Funktion erstellen, die ausgelöst wird, wenn Dateien in Azure Blob Storage hochgeladen oder aktualisiert werden.

![Zeigen Sie die Meldung in den Protokollen an.](./media/functions-create-storage-blob-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Voraussetzungen

+ Laden Sie den [Microsoft Azure Storage-Explorer](https://storageexplorer.com/) herunter, und installieren Sie ihn.
+ Ein Azure-Abonnement. Wenn Sie keins besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-an-azure-function-app"></a>Erstellen einer Azure Function-App

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Die Funktionen-App wurde erfolgreich erstellt.](./media/functions-create-first-azure-function/function-app-create-success.png)

Erstellen Sie als Nächstes in der neuen Funktionen-App eine Funktion.

<a name="create-function"></a>

## <a name="create-a-blob-storage-triggered-function"></a>Erstellen einer Funktion, die durch Blob Storage ausgelöst wird

1. Erweitern Sie die Funktionen-App, und klicken Sie auf die Schaltfläche **+** neben **Functions**. Wenn dies die erste Funktion in Ihrer Funktions-App ist, wählen Sie **Im Portal** und dann **Weiter**. Fahren Sie andernfalls mit Schritt 3 fort.

   ![Schnellstartseite für Funktionen im Azure-Portal](./media/functions-create-storage-blob-triggered-function/function-app-quickstart-choose-portal.png)

1. Klicken Sie auf **More templates** (Weitere Vorlagen) und anschließend auf **Finish and view templates** (Fertig stellen und Vorlagen anzeigen).

    ![Functions-Schnellstartanleitung: Auswählen weiterer Vorlagen](./media/functions-create-storage-blob-triggered-function/add-first-function.png)

1. Geben Sie `blob` in das Suchfeld ein, und wählen Sie dann die Vorlage **Blobtrigger** aus.

1. Klicken Sie auf **Installieren**, wenn Sie dazu aufgefordert werden, um die Azure Storage-Erweiterung sowie alle Abhängigkeiten in der Funktions-App zu installieren. Klicken Sie nach erfolgreichem Abschluss der Installation auf **Weiter**.

    ![Installieren von Bindungserweiterungen](./media/functions-create-storage-blob-triggered-function/functions-create-blob-storage-trigger-portal.png)

1. Verwenden Sie die Einstellungen, die in der Tabelle unter der Abbildung angegeben sind.

    ![Erstellen Sie die Funktion, die durch Blob Storage ausgelöst wird.](./media/functions-create-storage-blob-triggered-function/functions-create-blob-storage-trigger-portal-2.png)

    | Einstellung | Empfohlener Wert | Beschreibung |
    |---|---|---|
    | **Name** | Eindeutig in Ihrer Funktionen-App | Der Name dieser durch Blobs ausgelösten Funktion. |
    | **Path**   | samples-workitems/{name}    | Der Speicherort in Blob Storage, der überwacht wird. Der Dateiname des Blobs wird in der Bindung als _name_-Parameter übergeben.  |
    | **Speicherkontoverbindung** | AzureWebJobsStorage | Sie können die Speicherkontoverbindung verwenden, die bereits von Ihrer Funktionen-App verwendet wird, oder eine neue erstellen.  |

1. Klicken Sie auf **Erstellen**, um die Funktion zu erstellen.

Als Nächstes stellen Sie eine Verbindung mit Ihrem Azure Storage-Konto her und erstellen den Container **samples-workitems**.

## <a name="create-the-container"></a>Erstellen des Containers

1. Klicken Sie in Ihrer Funktion auf **Integrieren**, erweitern Sie **Dokumentation**, und kopieren Sie **Kontoname** und **Kontoschlüssel**. Diese Anmeldeinformationen benötigen Sie für die Verbindung mit dem Speicherkonto. Wenn Sie bereits eine Verbindung zu Ihrem Speicherkonto hergestellt haben, fahren Sie mit Schritt 4 fort.

    ![Beschaffen Sie die Anmeldeinformationen für die Speicherkontenverbindung.](./media/functions-create-storage-blob-triggered-function/functions-storage-account-connection.png)

1. Führen Sie das Tool [Microsoft Azure Storage-Explorer](https://storageexplorer.com/) aus, klicken Sie auf das Verbindungssymbol auf der linken Seite, wählen Sie **Use a storage account name and key** (Name und Schlüssel eines Speicherkontos verwenden) aus, und klicken Sie anschließend auf **Weiter**.

    ![Führen Sie das Tool Storage Account-Explorer aus.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-connect-1.png)

1. Geben Sie den **Kontonamen** und den **Kontoschlüssel** aus Schritt 1 ein, klicken Sie auf **Weiter** und dann auf **Verbinden**. 

    ![Geben Sie die Speicheranmeldeinformationen ein, und stellen Sie eine Verbindung her.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-connect-2.png)

1. Erweitern Sie das angefügte Speicherkonto, klicken Sie mit der rechten Maustaste auf **Blob-Container**, und klicken Sie dann auf **Blob-Container erstellen**. Geben Sie `samples-workitems` ein, und drücken Sie dann die EINGABETASTE.

    ![Erstellen Sie eine Speicherwarteschlange.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-create-blob-container.png)

Nun verfügen Sie über einen Blob-Container und können die Funktion durch Hochladen einer Datei in den Container testen.

## <a name="test-the-function"></a>Testen der Funktion

1. Gehen Sie zurück zum Azure-Portal, navigieren Sie zu Ihrer Funktion, erweitern Sie die **Protokolle** am unteren Rand der Seite, und stellen Sie sicher, dass das Protokollstreaming nicht angehalten ist.

1. Erweitern Sie im Storage-Explorer Ihr Speicherkonto, **Blob-Container** und **samples-workitems**. Klicken Sie auf **Hochladen** und dann auf **Dateien hochladen**.

    ![Laden Sie eine Datei in den Blob-Container hoch.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-upload-file-blob.png)

1. Klicken Sie im Dialogfeld **Dateien hochladen** auf das Feld **Dateien**. Navigieren Sie zu einer Datei auf dem lokalen Computer, z.B. zu einer Bilddatei, und wählen Sie sie aus. Klicken Sie auf **Öffnen** und dann auf **Hochladen**.

1. Gehen Sie zurück zu den Funktionsprotokollen, und überprüfen Sie, ob das Blob gelesen wurde.

   ![Zeigen Sie die Meldung in den Protokollen an.](./media/functions-create-storage-blob-triggered-function/functions-blob-storage-trigger-view-logs.png)

    >[!NOTE]
    > Wenn Ihre Funktionen-App im Standardverbrauchsplan ausgeführt wird, kann es zwischen dem Hinzufügen oder Aktualisieren des Blobs und dem Auslösen der Funktion zu einer Verzögerung von bis zu einigen Minuten kommen. Wenn Sie die Latenzzeit bei Ihren durch Blobs ausgelösten Funktionen gering halten möchten, können Sie die Funktionen-App in einem App Service-Plan ausführen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie haben eine Funktion erstellt, die ausgeführt wird, wenn ein Blob in Blob Storage hinzugefügt oder aktualisiert wird. Weitere Informationen zu Blob Storage-Auslösern finden Sie unter [Azure Functions – Blob Storage-Bindungen](functions-bindings-storage-blob.md).

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]
---
title: 'Zugreifen auf Blobspeicher mit einem Schlüsseltresor: Azure Databricks'
description: In diesem Tutorial erfahren Sie, wie Sie unter Verwendung von Geheimnissen, die in einem Schlüsseltresor gespeichert sind, über Azure Databricks auf Azure Blob Storage zugreifen.
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.topic: tutorial
ms.date: 07/19/2019
ms.openlocfilehash: 7983e18066578e3f036da84c73b6554ead2c40a1
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72791569"
---
# <a name="tutorial-access-azure-blob-storage-from-azure-databricks-using-azure-key-vault"></a>Tutorial: Zugreifen auf Azure Blob Storage über Azure Databricks unter Verwendung von Azure Key Vault

In diesem Tutorial erfahren Sie, wie Sie unter Verwendung von Geheimnissen, die in einem Schlüsseltresor gespeichert sind, über Azure Databricks auf Azure Blob Storage zugreifen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Speicherkontos und eines Blobcontainers
> * Erstellen einer Azure Key Vault-Instanz und Hinzufügen eines Geheimnisses
> * Erstellen eines Azure Databricks-Arbeitsbereichs und Hinzufügen eines Geheimnisbereichs
> * Zugreifen auf Ihren Blobcontainer über Azure Databricks

## <a name="prerequisites"></a>Voraussetzungen

- Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

> [!Note]
> Dieses Tutorial kann nicht mit dem **kostenlosen Azure-Testabonnement** absolviert werden.
> Wenn Sie ein kostenloses Konto haben, rufen Sie Ihr Profil auf, und ändern Sie Ihr Abonnement auf **Nutzungsbasierte Bezahlung**. Weitere Informationen finden Sie unter [Kostenloses Azure-Konto](https://azure.microsoft.com/free/). [Entfernen Sie das dann Ausgabenlimit](https://docs.microsoft.com/azure/billing/billing-spending-limit#why-you-might-want-to-remove-the-spending-limit), und [fordern Sie die Erhöhung des Kontingents](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) für vCPUs in Ihrer Region an. Wenn Sie Ihren Azure Databricks-Arbeitsbereich erstellen, können Sie den Tarif **Testversion (Premium – 14 Tage kostenlosen DBUs)** auswählen, damit Sie über den Arbeitsbereich 14 Tage lang auf kostenlose Premium Azure Databricks-DBUs zugreifen können.

## <a name="create-a-storage-account-and-blob-container"></a>Erstellen eines Speicherkontos und eines Blobcontainers

1. Wählen Sie im Azure-Portal **Ressource erstellen** > **Speicher** aus. Wählen Sie anschließend **Speicherkonto** aus.

   ![Suchen der Azure-Ressource „Speicherkonto“](./media/store-secrets-azure-key-vault/create-storage-account-resource.png)

2. Wählen Sie Ihr Abonnement und Ihre Ressourcengruppe aus, oder erstellen Sie eine neue Ressourcengruppe. Geben Sie anschließend einen Speicherkontonamen ein, und wählen Sie einen Standort aus. Klicken Sie auf **Überprüfen + erstellen**.

   ![Festlegen der Eigenschaften von Speicherkonten](./media/store-secrets-azure-key-vault/create-storage-account.png)

3. Sollte die Überprüfung nicht erfolgreich sein, beheben Sie die vorliegenden Probleme, und versuchen Sie es erneut. Wählen Sie nach erfolgreicher Überprüfung die Option **Erstellen** aus, und warten Sie, bis das Speicherkonto erstellt wurde.

4. Navigieren Sie zu Ihrem neu erstellten Speicherkonto, und wählen Sie auf der Seite **Übersicht** unter **Dienste** die Option **Blobs** aus. Wählen Sie anschließend **+ Container** aus, und geben Sie einen Containernamen ein. Klicken Sie auf **OK**.

   ![Erstellen eines neuen Containers](./media/store-secrets-azure-key-vault/create-blob-storage-container.png)

5. Suchen Sie eine Datei, die Sie in Ihren Blob Storage-Container hochladen möchten. Sollten Sie über keine Datei verfügen, erstellen Sie mithilfe eines Text-Editors eine neue Textdatei mit einigen Informationen. In diesem Beispiel wird eine Datei namens **hw.txt** verwendet, die den Text „hello world“ enthält. Speichern Sie Ihre Textdatei lokal, und laden Sie sie in Ihren Blob Storage-Container hoch.

   ![Hochladen der Datei in den Container](./media/store-secrets-azure-key-vault/upload-txt-file.png)

6. Kehren Sie zu Ihrem Speicherkonto zurück, und wählen Sie unter **Einstellungen** die Option **Zugriffsschlüssel** aus. Kopieren Sie die Werte für **Speicherkontoname** und **Schlüssel1** zur späteren Verwendung in diesem Tutorial in einen Text-Editor.

   ![Suchen der Speicherkonto-Zugriffsschlüssel](./media/store-secrets-azure-key-vault/storage-access-keys.png)

## <a name="create-an-azure-key-vault-and-add-a-secret"></a>Erstellen einer Azure Key Vault-Instanz und Hinzufügen eines Geheimnisses

1. Wählen Sie im Azure-Portal die Option **Ressource erstellen** aus, und geben Sie **Key Vault** in das Suchfeld ein.

   ![Suchfeld für die Erstellung einer Azure-Ressource](./media/store-secrets-azure-key-vault/find-key-vault-resource.png)

2. Die Key Vault-Ressource wird automatisch ausgewählt. Klicken Sie auf **Erstellen**.

   ![Erstellen einer Key Vault-Ressource](./media/store-secrets-azure-key-vault/create-key-vault-resource.png)

3. Geben Sie auf der Seite **Schlüsseltresor erstellen** die folgenden Informationen ein, und behalten Sie bei den restlichen Feldern die Standardwerte bei:

   |Eigenschaft|Description|
   |--------|-----------|
   |NAME|Ein eindeutiger Name für Ihren Schlüsseltresor.|
   |Subscription|Wählen Sie ein Abonnement aus.|
   |Resource group|Wählen Sie eine Ressourcengruppe aus, oder erstellen Sie eine neue.|
   |Location|Wählen Sie einen Standort aus.|

   ![Azure Key Vault-Eigenschaften](./media/store-secrets-azure-key-vault/create-key-vault-properties.png)

3. Klicken Sie nach der Angabe der obigen Informationen auf **Erstellen**. 

4. Navigieren Sie im Azure-Portal zu Ihrem neu erstellten Schlüsseltresor, und wählen Sie **Geheimnisse** aus. Wählen Sie anschließend die Option **+ Generieren/importieren** aus. 

   ![Generieren eines neuen Schlüsseltresorgeheimnisses](./media/store-secrets-azure-key-vault/generate-import-secrets.png)

5. Geben Sie auf der Seite **Geheimnis erstellen** die folgenden Informationen an, und behalten Sie bei den restlichen Feldern die Standardwerte bei:

   |Eigenschaft|Wert|
   |--------|-----------|
   |Uploadoptionen|Manuell|
   |NAME|Anzeigename für Ihren Speicherkontoschlüssel.|
   |Wert|„Schlüssel1“ aus Ihrem Speicherkonto.|

   ![Eigenschaften für das neue Schlüsseltresorgeheimnis](./media/store-secrets-azure-key-vault/create-storage-secret.png)

6. Speichern Sie den Schlüsselnamen zur späteren Verwendung in diesem Tutorial in einem Text-Editor, und wählen Sie **Erstellen** aus. Navigieren Sie anschließend zum Menü **Eigenschaften**. Kopieren Sie die Werte für **DNS-Name** und **Ressourcen-ID** zur späteren Verwendung in diesem Tutorial in einen Text-Editor.

   ![Kopieren von DNS-Name und Ressourcen-ID der Azure Key Vault-Instanz](./media/store-secrets-azure-key-vault/copy-dns-resource.png)

## <a name="create-an-azure-databricks-workspace-and-add-a-secret-scope"></a>Erstellen eines Azure Databricks-Arbeitsbereichs und Hinzufügen eines Geheimnisbereichs

1. Klicken Sie im Azure-Portal auf **Ressource erstellen** > **Analysen** > **Azure Databricks**.

    ![Databricks im Azure-Portal](./media/store-secrets-azure-key-vault/azure-databricks-on-portal.png)

2. Geben Sie unter **Azure Databricks-Dienst** die folgenden Werte an, um einen Databricks-Arbeitsbereich zu erstellen:

   |Eigenschaft  |BESCHREIBUNG  |
   |---------|---------|
   |Arbeitsbereichname     | Geben Sie einen Namen für Ihren Databricks-Arbeitsbereich an.        |
   |Subscription     | Wählen Sie in der Dropdownliste Ihr Azure-Abonnement aus.        |
   |Resource group     | Wählen Sie die Ressourcengruppe aus, in der sich auch Ihr Schlüsseltresor befindet. |
   |Location     | Wählen Sie den Standort aus, an dem sich auch Ihre Azure Key Vault-Instanz befindet. Alle verfügbaren Regionen finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/regions/services/).        |
   |Preisstufe     |  Wählen Sie zwischen **Standard** und **Premium**. Weitere Informationen zu diesen Tarifen, finden Sie unter [Azure Databricks – Preise](https://azure.microsoft.com/pricing/details/databricks/).       |

   ![Eigenschaften des Databricks-Arbeitsbereichs](./media/store-secrets-azure-key-vault/create-databricks-service.png)

   Klicken Sie auf **Erstellen**.

3. Navigieren Sie im Azure-Portal zu Ihrer neu erstellten Azure Databricks-Ressource, und wählen Sie **Arbeitsbereich starten** aus.

   ![Starten des Azure Databricks-Arbeitsbereichs](./media/store-secrets-azure-key-vault/launch-databricks-workspace.png)

4. Nachdem Ihr Azure Databricks-Arbeitsbereich in einem separaten Fenster geöffnet wurde, fügen Sie **#secrets/createScope** an die URL an. Das Format der URL muss wie folgt aussehen: 

   **https://<\location>.azuredatabricks.net/?o=<\id>#secrets/createScope**.

5. Geben Sie einen Bereichsnamen sowie den DNS-Namen und die Ressourcen-ID von Azure Key Vault ein, die Sie zuvor gespeichert haben. Speichern Sie den Bereichsnamen zur späteren Verwendung in diesem Tutorial in einem Text-Editor. Wählen Sie anschließend **Erstellen**.

   ![Erstellen des Geheimnisbereichs im Azure Databricks-Arbeitsbereich](./media/store-secrets-azure-key-vault/create-secret-scope.png)

## <a name="access-your-blob-container-from-azure-databricks"></a>Zugreifen auf Ihren Blobcontainer über Azure Databricks

1. Wählen Sie auf der Startseite Ihres Azure Databricks-Arbeitsbereichs unter **Allgemeine Aufgaben** die Option **Neuer Cluster** aus.

   ![Erstellen eines neuen Azure Databricks-Notebooks](./media/store-secrets-azure-key-vault/create-new-cluster.png)

2. Geben Sie einen Clusternamen ein, und wählen Sie **Cluster erstellen** aus. Die Clustererstellung dauert einige Minuten.

3. Navigieren Sie nach Abschluss der Clustererstellung zur Startseite Ihres Azure Databricks-Arbeitsbereichs, und wählen Sie unter **Allgemeine Aufgaben** die Option **Neues Notebook** aus.

   ![Erstellen eines neuen Azure Databricks-Notebooks](./media/store-secrets-azure-key-vault/create-new-notebook.png)

4. Geben Sie einen Notebooknamen ein, und legen Sie die Sprache auf „Python“ fest. Legen Sie den Cluster auf den Namen des Clusters fest, den Sie im vorherigen Schritt erstellt haben.

5. Führen Sie den folgenden Befehl aus, um Ihren Blob Storage-Container einzubinden. Ändern Sie dabei die Werte für die folgenden Eigenschaften:

   * „your-container-name“ (Name Ihres Containers)
   * „your-storage-account-name“ (Name Ihres Speicherkontos)
   * „mount-name“ (Einbindungsname)
   * „conf-key“ (Konfigurationsschlüssel)
   * „scope-name“ (Bereichsname)
   * „key-name“ (Schlüsselname)

   ```python
   dbutils.fs.mount(
   source = "wasbs://<your-container-name>@<your-storage-account-name>.blob.core.windows.net",
   mount_point = "/mnt/<mount-name>",
   extra_configs = {"<conf-key>":dbutils.secrets.get(scope = "<scope-name>", key = "<key-name>")})
   ```

   * **mount-name** (Einbindungsname) ist ein DBFS-Pfad, der angibt, wo der Blob Storage-Container oder ein Ordner innerhalb des Containers (angegeben in der Quelle) eingebunden wird.
   * **conf-key** (Konfigurationsschlüssel) kann entweder `fs.azure.account.key.<\your-storage-account-name>.blob.core.windows.net` oder `fs.azure.sas.<\your-container-name>.<\your-storage-account-name>.blob.core.windows.net` lauten.
   * **scope-name** (Bereichsname) ist der Name des Geheimnisbereichs, den Sie im vorherigen Abschnitt erstellt haben. 
   * **key-name** (Schlüsselname) ist der Name des Geheimnisses, das Sie für den Speicherkontoschlüssel in Ihrem Schlüsseltresor erstellt haben.

   ![Erstellen einer Blob Storage-Einbindung im Notebook](./media/store-secrets-azure-key-vault/command1.png)

6. Führen Sie den folgenden Befehl aus, um die Textdatei in Ihrem Blob Storage-Container in einen Datenrahmen zu lesen. Ersetzen Sie die Platzhalter in dem Befehl durch Ihren Einbindungsnamen bzw. Ihren Dateinamen:

   ```python
   df = spark.read.text("mnt/<mount-name>/<file-name>")
   ```

   ![Lesen der Datei in einen Datenrahmen](./media/store-secrets-azure-key-vault/command2.png)

7. Verwenden Sie den folgenden Befehl, um den Inhalt Ihrer Datei anzuzeigen:

   ```python
   df.show()
   ```
   ![Anzeigen des Datenrahmens](./media/store-secrets-azure-key-vault/command3.png)

8. Führen Sie den folgenden Befehl aus, um die Einbindung Ihres Blobspeichers aufzuheben:

   ```python
   dbutils.fs.unmount("/mnt/<mount-name>")
   ```

   ![Aufheben der Einbindung des Speicherkontos](./media/store-secrets-azure-key-vault/command4.png)

9. Hinweis: Nach Aufhebung der Bereitstellung kann nicht mehr aus Ihrem Blob Storage-Konto gelesen werden.

   ![Aufheben der Einbindung des Speicherkontos: Fehler](./media/store-secrets-azure-key-vault/command5.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie diese Anwendung nicht mehr benötigen, löschen Sie die gesamte Ressourcengruppe:

1. Wählen Sie im Azure-Portal im Menü auf der linken Seite die Option **Ressourcengruppen** aus, und navigieren Sie zu Ihrer Ressourcengruppe.

2. Wählen Sie **Ressourcengruppe löschen** aus, und geben Sie den Namen Ihrer Ressourcengruppe ein. Wählen Sie anschließend die Option **Löschen**. 

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Tutorial erfahren Sie, wie Sie eine Databricks-Umgebung mit eingeschleustem VNET mithilfe eines für Cosmos DB aktivierten Dienstendpunkts implementieren.
> [!div class="nextstepaction"]
> [Tutorial: Implementieren von Azure Databricks mit einem Cosmos DB-Endpunkt](service-endpoint-cosmosdb.md)

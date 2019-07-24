---
title: Zugreifen auf Azure Storage mithilfe einer vom Windows-VM-System zugewiesenen verwalteten Identität | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie eine systemseitig zugewiesene verwaltete Identität eines virtuellen Windows-Computers verwenden, um auf Azure Storage zuzugreifen.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: daveba
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/12/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: e7d7200dd89d51817a5d146ff4d33e2501ed2826
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2019
ms.locfileid: "68278009"
---
# <a name="tutorial-use-a-windows-vm-system-assigned-managed-identity-to-access-azure-storage"></a>Tutorial: Verwenden der systemseitig zugewiesenen verwalteten Identität eines virtuellen Windows-Computers für den Zugriff auf Azure Storage

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

In diesem Tutorial erfahren Sie, wie Sie eine systemseitig zugewiesene verwaltete Identität für einen virtuellen Windows-Computer verwenden, um auf Azure Storage zuzugreifen. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen eines Blobcontainers im Speicherkonto
> * Gewähren des Zugriffs auf ein Speicherkonto für die systemseitig zugewiesene verwaltete Identität Ihres virtuellen Windows-Computers
> * Erhalten eines Zugriffs und seine Verwendung zum Aufrufen von Azure Storage

> [!NOTE]
> Die Azure Active Directory-Authentifizierung für Azure Storage liegt in der Public Preview-Version vor.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="create-a-storage-account"></a>Speicherkonto erstellen

In diesem Abschnitt erstellen Sie ein Speicherkonto.

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **+ Ressource erstellen**.
2. Wählen Sie die Option **Speicher** und anschließend **Speicherkonto – Blob, Datei, Tabelle, Warteschlange** aus.
3. Geben Sie unter **Name** einen Namen für das Speicherkonto ein.
4. **Bereitstellungsmodell** und **Kontoart** sollten jeweils auf **Resource Manager** und **Storage (universell, Version 1)** festgelegt werden.
5. Stellen Sie sicher, dass **Abonnement** und **Ressourcengruppe** dem entsprechen, was Sie bei der Erstellung Ihrer VM im vorherigen Schritt angegeben haben.
6. Klicken Sie auf **Create**.

    ![Erstellen eines neuen Speicherkontos](./media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-and-upload-a-file-to-the-storage-account"></a>Erstellen eines Blobcontainers und Hochladen einer Datei in das Speicherkonto

Da Dateien Blob Storage erfordern, müssen wir einen Blobcontainer erstellen, in dem die Datei gespeichert wird. Anschließend laden Sie eine Datei in den Blobcontainer im neuen Speicherkonto hoch.

1. Navigieren Sie zurück zum neu erstellten Speicherkonto.
2. Klicken Sie unter **Blob-Dienst** auf **Container**.
3. Klicken Sie oben auf der Seite auf **+ Container**.
4. Geben Sie unter **Neuer Container** einen Namen für den Container ein, und behalten Sie unter **Öffentliche Zugriffsebene** den Standardwert bei.

    ![Erstellen eines Speichercontainers](./media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

5. Erstellen Sie mit einem Editor Ihrer Wahl eine Datei mit dem Titel *hall_ welt.txt* auf dem lokalen Computer. Öffnen Sie die Datei, fügen Sie den Text „Hallo Welt! :)“ (ohne Anführungszeichen) hinzu, und speichern Sie ihn.
6. Laden Sie die Datei in den neu erstellten Container hoch, indem Sie auf den Containernamen und dann auf **Hochladen** klicken.
7. Klicken Sie im Bereich **Blob hochladen** unter **Dateien** auf das Ordnersymbol und navigieren Sie zur Datei **hallo_welt.txt** auf dem lokalen Computer. Wählen Sie dann die Datei aus, und klicken Sie auf **Hochladen**.
    ![Hochladen einer Textdatei](./media/msi-tutorial-linux-vm-access-storage/upload-text-file.png)

## <a name="grant-your-vm-access-to-an-azure-storage-container"></a>Gewähren des Zugriffs auf einen Azure-Speichercontainer für Ihre VM

Sie können die systemseitig zugewiesene verwaltete Identität des virtuellen Computers verwenden, um die Daten in Azure Storage Blob abzurufen.

1. Navigieren Sie zurück zum neu erstellten Speicherkonto.
2. Klicken Sie im linken Bereich auf den Link **Zugriffssteuerung (IAM)** .
3. Klicken Sie oben auf der Seite auf **+ Rollenzuweisung hinzufügen**, um dem virtuellen Computer eine neue Rollenzuweisung hinzuzufügen.
4. Wählen Sie unter **Rolle** in der Dropdownliste **Storage-Blobdatenleser** aus.
5. Wählen Sie in der nächsten Dropdownliste unter **Zugriff zuweisen zu** die Option **Virtueller Computer** aus.
6. Stellen Sie im nächsten Schritt sicher, dass das richtige Abonnement in der Dropdownliste **Abonnement** aufgeführt ist, und legen Sie dann für **Ressourcengruppe** die Option **Alle Ressourcengruppen** fest.
7. Wählen Sie unter **Auswählen** Ihren virtuellen Computer aus, und klicken Sie dann auf **Speichern**.

    ![Zuweisen von Berechtigungen](./media/tutorial-linux-vm-access-storage/access-storage-perms.png)

## <a name="get-an-access-token-and-use-it-to-call-azure-storage"></a>Erhalten eines Zugriffstokens und seine Verwendung zum Aufrufen von Azure Storage 

Azure Storage unterstützt die Azure AD-Authentifizierung nativ, sodass Zugriffstoken, die mit einer verwalteten Identität abgerufen wurden, direkt angenommen werden können. Dieser Umstand ist Teil der Azure Storage-Integration in Azure AD und unterscheidet sich vom Bereitstellen von Anmeldeinformationen in der Verbindungszeichenfolge.

Im Folgenden finden Sie ein .NET-Codebeispiel zum Öffnen einer Verbindung mit Azure Storage mithilfe eines Zugriffstokens und zum anschließenden Lesen der Inhalte der zuvor erstellten Datei. Dieser Code muss auf dem virtuellen Computer ausgeführt werden, um auf Endpunkt für die verwaltete Identität des virtuellen Computers zugreifen zu können. Für die Verwendung des Verfahrens mit Zugriffstoken ist .NET Framework 4.6 oder höher erforderlich. Ersetzen Sie entsprechend den Wert `<URI to blob file>`. Diesen erhalten Sie, wenn Sie zur Datei navigieren, die Sie zuvor erstellt und in Blob Storage hochgeladen haben, und darin die **URL** unter **Eigenschaften** auf der Seite **Übersicht** kopieren.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IO;
using System.Net;
using System.Web.Script.Serialization;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;

namespace StorageOAuthToken
{
    class Program
    {
        static void Main(string[] args)
        {
            //get token
            string accessToken = GetMSIToken("https://storage.azure.com/");

            //create token credential
            TokenCredential tokenCredential = new TokenCredential(accessToken);

            //create storage credentials
            StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

            Uri blobAddress = new Uri("<URI to blob file>");

            //create block blob using storage credentials
            CloudBlockBlob blob = new CloudBlockBlob(blobAddress, storageCredentials);

            //retrieve blob contents
            Console.WriteLine(blob.DownloadText());
            Console.ReadLine();
        }

        static string GetMSIToken(string resourceID)
        {
            string accessToken = string.Empty;
            // Build request to acquire MSI token
            HttpWebRequest request = (HttpWebRequest)WebRequest.Create("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=" + resourceID);
            request.Headers["Metadata"] = "true";
            request.Method = "GET";

            try
            {
                // Call /token endpoint
                HttpWebResponse response = (HttpWebResponse)request.GetResponse();

                // Pipe response Stream to a StreamReader, and extract access token
                StreamReader streamResponse = new StreamReader(response.GetResponseStream());
                string stringResponse = streamResponse.ReadToEnd();
                JavaScriptSerializer j = new JavaScriptSerializer();
                Dictionary<string, string> list = (Dictionary<string, string>)j.Deserialize(stringResponse, typeof(Dictionary<string, string>));
                accessToken = list["access_token"];
                return accessToken;
            }
            catch (Exception e)
            {
                string errorText = String.Format("{0} \n\n{1}", e.Message, e.InnerException != null ? e.InnerException.Message : "Acquire token failed");
                return accessToken;
            }
        }
    }
}
```

Die Antwort enthält den Inhalt der Datei:

`Hello world! :)`

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie eine systemseitig zugewiesene Identität eines virtuellen Windows-Computers für den Zugriff auf Azure Storage aktivieren.  Weitere Informationen zu Azure Storage finden Sie hier:

> [!div class="nextstepaction"]
> [Azure Storage (in englischer Sprache)](/azure/storage/common/storage-introduction)

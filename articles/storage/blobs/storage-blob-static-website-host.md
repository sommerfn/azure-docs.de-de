---
title: 'Tutorial: Hosten einer statischen Website in Blob Storage – Azure Storage'
description: Es wird beschrieben, wie Sie ein Speicherkonto für das Hosten von statischen Websites konfigurieren und eine statische Website in Azure Storage bereitstellen.
author: normesta
ms.service: storage
ms.subservice: blobs
ms.topic: tutorial
ms.date: 10/22/2019
ms.author: normesta
ms.reviewer: dineshm
ms.openlocfilehash: cd3db902d713910de5a8199df85089d62569f9d7
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72757566"
---
<!---Customer intent: I want to host files for a static website in Blob storage and access the website from an Azure endpoint.--->

# <a name="tutorial-host-a-static-website-on-blob-storage"></a>Tutorial: Hosten einer statischen Website in Blob Storage

Dieses Tutorial ist der erste Teil einer Serie. Darin wird beschrieben, wie Sie eine statische Website erstellen und in Azure Storage bereitstellen. Nach Abschluss des Vorgangs verfügen Sie über eine statische Website, auf die Benutzer öffentlich zugreifen können. 

Im ersten Teil der Serie lernen Sie Folgendes:

> [!div class="checklist"]
> * Konfigurieren des Hostens von statischen Websites
> * Bereitstellen einer Hello World-Website

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

> [!NOTE] 
> Erstellen Sie unbedingt ein Speicherkonto vom Typ „Allgemein v2 (GPv2)“. Statische Websites sind in keinem anderen Speicherkontotyp verfügbar.

In diesem Tutorial wird [Visual Studio Code](https://code.visualstudio.com/download) verwendet. Hierbei handelt es sich um ein kostenloses Tool für Programmierer, mit dem die statische Website erstellt und in einem Azure Storage-Konto bereitgestellt werden kann.

Installieren Sie nach Abschluss der Installation von Visual Studio Code die Azure Storage-Erweiterung, die sich in der Vorschauphase befindet. Mit dieser Erweiterung wird die Azure Storage-Verwaltungsfunktionalität in Visual Studio Code integriert. Sie verwenden die Erweiterung zum Bereitstellen Ihrer statischen Website in Azure Storage. Installieren Sie die Erweiterung wie folgt:

1. Starten Sie Visual Studio Code.
2. Klicken Sie in der Symbolleiste auf **Erweiterungen**. Suchen Sie nach *Azure Storage*, und wählen Sie die Erweiterung **Azure Storage** in der Liste aus. Klicken Sie anschließend auf die Schaltfläche **Installieren**, um die Erweiterung zu installieren.

    ![Installieren der Azure Storage-Erweiterung in VS Code](media/storage-blob-static-website-host/install-extension-vs-code.png)

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich zunächst beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="configure-static-website-hosting"></a>Konfigurieren des Hostens von statischen Websites

Im ersten Schritt wird Ihr Speicherkonto für das Hosten einer statischen Website im Azure-Portal konfiguriert. Wenn Sie Ihr Konto für das Hosten von statischen Websites konfigurieren, erstellt Azure Storage automatisch einen Container mit dem Namen *$web*. Der Container *$web* enthält die Dateien für Ihre statische Website. 

1. Wechseln Sie in Ihrem Webbrowser zum [Azure-Portal](https://portal.azure.com/). 
1. Suchen Sie nach Ihrem Speicherkonto, und zeigen Sie die Kontoübersicht an.
1. Wählen Sie die Option **Statische Website**, um die Konfigurationsseite für statische Websites anzuzeigen.
1. Wählen Sie **Aktiviert**, um das Hosten von statischen Websites für das Speicherkonto zu aktivieren.
1. Geben Sie im Feld **Name des Indexdokuments**  die Standardindexseite *index.html* an. Die Standardindexseite wird angezeigt, wenn ein Benutzer zum Stammverzeichnis Ihrer statischen Website navigiert.  
1. Geben Sie im Feld **Pfad zum Fehlerdokument** die Standardfehlerseite *404.html* an. Die Standardfehlerseite wird angezeigt, wenn ein Benutzer versucht, zu einer Seite zu navigieren, die auf Ihrer statischen Website nicht vorhanden ist.
1. Klicken Sie auf **Speichern**. Im Azure-Portal wird jetzt der Endpunkt Ihrer statischen Website angezeigt. 

    ![Aktivieren des Hostens von statischen Websites für ein Speicherkonto](media/storage-blob-static-website-host/enable-static-website-hosting.png)

## <a name="deploy-a-hello-world-website"></a>Bereitstellen einer Hello World-Website

Erstellen Sie als Nächstes eine Hello World-Webseite mit Visual Studio Code, und stellen Sie sie für die statische Website bereit, die in Ihrem Azure Storage-Konto gehostet wird.

1. Erstellen Sie in Ihrem lokalen Dateisystem einen leeren Ordner mit dem Namen *mywebsite*. 
1. Starten Sie Visual Studio Code, und öffnen Sie im Bereich **Explorer** den gerade erstellten Ordner.

    ![Öffnen des Ordners in Visual Studio Code](media/storage-blob-static-website-host/open-folder-vs-code.png)

1. Erstellen Sie die Standardindexdatei im Ordner *mywebsite*, und geben Sie ihr den Namen *index.html*.

    ![Erstellen der Standardindexdatei in Visual Studio Code](media/storage-blob-static-website-host/create-index-file-vs-code.png)

1. Öffnen Sie die Datei *index.html* im Editor, fügen Sie den folgenden Text in die Datei ein, und speichern Sie sie:

    ```
    <h1>Hello World!</h1>
    ```

1. Erstellen Sie die Standardfehlerdatei, und geben Sie ihr den Namen *404.html*.
1. Öffnen Sie die Datei *404.html* im Editor, fügen Sie den folgenden Text in die Datei ein, und speichern Sie sie:

    ```
    <h1>404</h1>
    ```

1. Klicken Sie unter dem Ordner *mywebsite* mit der rechten Maustaste in den Bereich **Explorer**, und wählen Sie **Deploy to Static Website...** (Auf statischer Website bereitstellen...), um Ihre Website bereitzustellen. Sie werden aufgefordert, sich an Azure anzumelden, um eine Liste mit Abonnements abzurufen.

1. Wählen Sie das Abonnement mit dem Speicherkonto aus, für das Sie das Hosten von statischen Websites aktiviert haben. Wählen Sie als Nächstes das Speicherkonto aus, wenn Sie dazu aufgefordert werden.

Visual Studio Code lädt Ihre Dateien jetzt auf Ihren Webendpunkt hoch und zeigt die Statusleiste an, damit Sie den Vorgang verfolgen können. Starten Sie die Website, um sie in Azure anzuzeigen.

![Anzeigen der Bereitstellung von statischen Websites in Azure](media/storage-blob-static-website-host/view-static-website-endpoint.png)

Sie haben das Tutorial erfolgreich abgeschlossen und eine statische Website in Azure bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

Im ersten Teil dieses Tutorials wurde beschrieben, wie Sie Ihr Azure Storage-Konto für das Hosten von statischen Websites konfigurieren und eine statische Website auf einem Azure-Endpunkt erstellen und bereitstellen.

Fahren Sie jetzt mit Teil 2 fort, in dem Sie eine benutzerdefinierte Domäne mit SSL für Ihre statische Website mit Azure CDN konfigurieren.

> [!div class="nextstepaction"]
> [Verwenden von Azure CDN zum Aktivieren einer benutzerdefinierten Domäne mit SSL für eine statische Website](storage-blob-static-website-custom-domain.md)

---
title: Hochladen von Dateien in ein Azure Media Services-Konto mithilfe von REST | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Medieninhalte in Media Services nutzen können, indem Sie Medienobjekte erstellen und hochladen.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: a6f872880b61a5bd9510abda2f15e2edea16e940
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67703875"
---
# <a name="upload-files-into-a-media-services-account-using-rest"></a>Hochladen von Dateien in ein Media Services-Konto mit REST

In Media Services laden Sie Ihre digitalen Dateien in einen Blobcontainer hoch, der einem Medienobjekt zugeordnet ist. Die [Asset](https://docs.microsoft.com/rest/api/media/operations/asset)-Entität kann Videos, Audiodateien, Bilder, Miniaturansichtssammlungen, Texttitel und Untertiteldateien (und die Metadaten zu diesen Dateien) enthalten. Nachdem die Dateien in den Container des Medienobjekts hochgeladen wurden, werden Ihre Inhalte zur weiteren Verarbeitung und zum Streaming sicher in der Cloud gespeichert.

In diesem Artikel erfahren Sie, wie Sie mit REST eine lokale Datei hochladen.

## <a name="prerequisites"></a>Voraussetzungen

Für die in diesem Thema beschriebenen Schritte ist Folgendes erforderlich:

- Lesen Sie [Objekte](assets-concept.md).
- [Konfigurieren Sie Postman für Azure Media Services-REST-API-Aufrufe](media-rest-apis-with-postman.md).
    
    Befolgen Sie dabei unbedingt den letzten Schritt im Thema [Abrufen von Azure AD-Token](media-rest-apis-with-postman.md#get-azure-ad-token). 

## <a name="create-an-asset"></a>Erstellen eines Medienobjekts

In diesem Abschnitt wird veranschaulicht, wie Sie ein neues Medienobjekt erstellen.

1. Wählen Sie **Medienobjekte** -> **Create or update an Asset** (Medienobjekt erstellen oder aktualisieren) aus.
2. Klicken Sie auf **Senden**.

    ![Erstellen eines Medienobjekts](./media/upload-files/postman-create-asset.png)

Es wird eine **Antwort** mit Informationen zum neu erstellten Medienobjekt angezeigt.

## <a name="get-a-sas-url-with-read-write-permissions"></a>Abrufen einer SAS-URL mit Lese-/Schreibberechtigungen 

In diesem Abschnitt wird gezeigt, wie Sie eine SAS-URL, die für das erstellte Medienobjekt generiert wurde, abrufen. Die SAS-URL wurde mit Lese-/Schreibberechtigungen erstellt und kann verwendet werden, um digitale Dateien in den Container des Medienobjekts hochzuladen.

1. Wählen Sie **Medienobjekte** -> **List the Asset URLs** (Medienobjekt-URLs auflisten) aus.
2. Klicken Sie auf **Senden**.

    ![Hochladen einer Datei](./media/upload-files/postman-create-sas-locator.png)

Es wird eine **Antwort** mit Informationen zu den URLs des Medienobjekts angezeigt. Kopieren Sie die erste URL, und verwenden Sie sie zum Hochladen Ihrer Datei.

## <a name="upload-a-file-to-blob-storage-using-the-upload-url"></a>Hochladen einer Datei in den Blobspeicher mithilfe der Upload-URL

Verwenden Sie die Azure Storage-APIs oder SDKs (z.B. [Storage-REST-API](../../storage/common/storage-rest-api-auth.md), [Java SDK](../../storage/blobs/storage-quickstart-blobs-java-v10.md) oder [.NET SDK](../../storage/blobs/storage-quickstart-blobs-dotnet.md).

## <a name="next-steps"></a>Nächste Schritte

[Tutorial: Codieren einer Remotedatei anhand einer URL und Streamen des Videos über – REST](stream-files-tutorial-with-rest.md)

---
title: Bearbeiten von APIs im Azure-Portal | Microsoft-Dokumentation
description: Dieses Tutorial veranschaulicht, wie Sie API Management (APIM) verwenden, um eine API zu bearbeiten.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 11/08/2017
ms.author: apimpm
ms.openlocfilehash: 6be36493fabce07838991c789e111e918a9a826d
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70072158"
---
# <a name="edit-an-api"></a>Bearbeiten von APIs

Die Schritte in diesem Tutorial veranschaulichen, wie Sie API Management (APIM) verwenden, um eine API zu bearbeiten. 

+ Die Bearbeitung erfolgt durch das Hinzufügen, Löschen oder Umbenennen von Vorgängen in der APIM-Instanz. 
+ Sie können auch den Swagger Ihrer API bearbeiten.

## <a name="prerequisites"></a>Voraussetzungen

+ [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md)
+ [Importieren und Veröffentlichen Ihrer ersten API](import-and-publish.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="edit-an-api-in-apim"></a>Bearbeiten von APIs in APIM

![Bearbeiten von APIs](./media/edit-api/edit-api001.png)

1. Klicken Sie auf die Registerkarte **APIs**.
2. Wählen Sie eine der APIs aus, die Sie zuvor importiert haben.
3. Wählen Sie die Registerkarte **Entwurf** aus.
4. Wählen Sie einen Vorgang aus, den Sie bearbeiten möchten.
5. Wählen Sie zum Umbenennen des Vorgangs einen **Stift** im Fenster **Front-End** aus.

## <a name="update-the-swagger"></a>Aktualisieren des Swaggers

Sie können Ihre Back-End-API im Azure-Portal aktualisieren, indem Sie folgende Schritte ausführen:

1. Auswählen von **Alle Vorgänge**
2. Klicken Sie auf den Stift im Fenster **Front-End**.

    ![Bearbeiten von APIs](./media/edit-api/edit-api002.png)

    Ihr API-Swagger wird angezeigt.

    ![Bearbeiten von APIs](./media/edit-api/edit-api003.png)

3. Aktualisieren Sie den Swagger.
4. Klicken Sie auf **Speichern**.

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Beispiele für APIM-Richtlinien](policy-samples.md)
> [Transformieren und Schützen veröffentlichter APIs](transform-api.md)
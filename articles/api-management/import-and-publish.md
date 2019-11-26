---
title: Importieren und Verwalten Ihrer ersten API in Azure API Management | Microsoft-Dokumentation
description: Erfahren sie, wie Sie Ihre erste API mit API Management importieren und veröffentlichen.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 02/24/2019
ms.author: apimpm
ms.openlocfilehash: bae762b4603b2f5f80447a16671fed4e37e62b95
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74108540"
---
# <a name="import-and-publish-your-first-api"></a>Importieren und Veröffentlichen Ihrer ersten API 

In diesem Tutorial erfahren Sie, wie Sie eine Back-End-API mit OpenAPI-Spezifikation aus https://conferenceapi.azurewebsites.net?format=json importieren. Diese Back-End-API wird von Microsoft bereitgestellt und in Azure gehostet. 

Nachdem die Back-End-API in API Management (APIM) importiert wurde, wird die APIM-API zur Fassade für die Back-End-API. Zum Zeitpunkt des Imports der Back-End-API sind die Quell-API und die APIM-API identisch. APIM ermöglicht Ihnen das Anpassen der Fassade gemäß Ihren Anforderungen, ohne die Back-End-API ändern zu müssen. Weitere Informationen finden Sie unter [Transformieren und Schützen Ihrer API](transform-api.md). 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Importieren Ihrer ersten API
> * Testen der API im Azure-Portal
> * Testen der API im Entwicklerportal

![Neue API](./media/api-management-get-started/created-api.png)

## <a name="prerequisites"></a>Voraussetzungen

+ Machen Sie sich mit der [Azure API Management-Terminologie](api-management-terminology.md) vertraut.
+ Absolvieren Sie die folgende Schnellstartanleitung: [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"> </a>Importieren und Veröffentlichen einer Back-End-API

In diesem Abschnitt wird gezeigt, wie Sie eine Back-End-API mit OpenAPI-Spezifikation importieren und veröffentlichen.
 
1. Wählen Sie unter **API MANAGEMENT** die Option **APIs** aus.
2. Wählen Sie **OpenAPI-Spezifikation** in der Liste aus, und klicken Sie im Popupfenster auf **Vollständig**.

    ![Erstellen einer API](./media/api-management-get-started/create-api.png)

    Sie können die API-Werte während der Erstellung oder später über die Registerkarte **Einstellungen** festlegen. Der rote Stern neben einem Feld gibt an, dass das Feld erforderlich ist.

    Verwenden Sie die Werte aus der Datei unten, um Ihre erste API zu erstellen.

    | Einstellung                   | Wert                                              | BESCHREIBUNG                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
    |---------------------------|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | **OpenAPI-Spezifikation** | https://conferenceapi.azurewebsites.net?format=json | Verweist auf den Dienst zum Implementieren der API. API Management leitet Anfragen an diese Adresse weiter.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
    | **Anzeigename**          | *Demo Conference API*                              | Wenn Sie nach dem Eingeben der Dienst-URL die TAB-TASTE drücken, wird dieses Feld von APIM basierend auf den Daten in der JSON-Datei ausgefüllt. <br/>Dieser Name wird im Entwicklerportal angezeigt.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
    | **Name**                  | *demo-conference-api*                              | Stellt einen eindeutigen Namen für die API bereit. <br/>Wenn Sie nach dem Eingeben der Dienst-URL die TAB-TASTE drücken, wird dieses Feld von APIM basierend auf den Daten in der JSON-Datei ausgefüllt.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
    | **Beschreibung**           | Geben Sie eine optionale Beschreibung der API an.        | Wenn Sie nach dem Eingeben der Dienst-URL die TAB-TASTE drücken, wird dieses Feld von APIM basierend auf den Daten in der JSON-Datei ausgefüllt.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
    | **URL-Schema**            | *HTTPS*                                            | Legt fest, welche Protokolle für den Zugriff auf die API verwendet werden können.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | **API-URL-Suffix**        | *conference*                                       | Das Suffix wird an die Basis-URL für den API Management-Dienst angehängt. API Management unterscheidet APIs durch deren Suffix. Daher muss jede API eines bestimmten Herausgebers ein eindeutiges Suffix haben.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
    | **Produkte**              | *Unbegrenzt*                                        | Bei Produkten handelt es sich um API-Zuordnungen. Sie können ein Produkt mit einer Reihe von APIs versehen und sie Entwicklern über das Entwicklerportal zur Verfügung stellen. <br/>Zur Veröffentlichung einer API muss diese einem Produkt (in diesem Beispiel: *Unbegrenzt*) zugeordnet werden. Wenn Sie diese neue API einem Produkt hinzufügen möchten, geben Sie den Produktnamen ein. (Dieser Schritt kann auch später auf der Seite **Einstellungen** ausgeführt werden.) Dieser Schritt kann mehrfach wiederholt werden, um die API mehreren Produkten hinzuzufügen.<br/>Entwickler müssen ein Produkt zunächst abonnieren, um Zugriff auf die API zu erhalten. Wenn sie ein Produkt abonnieren, erhalten sie einen Abonnementschlüssel, der für jede API in diesem Produkt gilt. <br/> Wenn Sie die APIM-Instanz erstellt haben, sind Sie bereits Administrator und haben somit alle Produkte abonniert.<br/> Standardmäßig enthält jede API Management-Instanz zwei Beispielprodukte: **Starter** und **Unbegrenzt**. |
    | **Tags**                  |                                                    | Tags zum Organisieren von APIs. Tags können zum Suchen, Gruppieren oder Filtern verwendet werden.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
    | **Versionsverwaltung für diese API?**     |                                                    | Weitere Informationen zur Versionsverwaltung finden Sie unter [Veröffentlichen mehrerer Versionen Ihrer API](api-management-get-started-publish-versions.md).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |

    >[!NOTE]
    > Zur Veröffentlichung der API muss sie einem Produkt zugeordnet werden. Dies ist auf der Seite **Einstellungen** möglich.

3. Klicken Sie auf **Erstellen**.

> [!TIP]
> Sollten beim Importieren Ihrer eigenen API-Definition Probleme auftreten, sehen Sie sich die [Liste mit bekannten Problemen und Einschränkungen](api-management-api-import-restrictions.md) an.

## <a name="test-the-new-api-in-the-azure-portal"></a>Testen der neuen API im Azure-Portal

![Testen der API](./media/api-management-get-started/01-import-first-api-01.png)

Vorgänge können direkt aus dem Azure-Portal aufgerufen werden. Dies ist ein einfacher Weg, die Vorgänge einer API anzuzeigen und zu testen.

1. Wählen Sie die API aus, die Sie im vorherigen Schritt erstellt haben (auf der Registerkarte **APIs**).
2. Wählen Sie die Registerkarte **Testen** aus.
3. Klicken Sie auf **GetSpeakers**. Auf der Seite werden Felder für Abfrageparameter angezeigt (in diesem Fall keine) sowie Header. Einer der Header ist „Ocp-Apim-Subscription-Key“. Er steht für den Abonnementschlüssel des Produkts, das dieser API zugeordnet ist. Der Schlüssel wird automatisch eingefügt.
4. Klicken Sie auf **Senden**.

    Das Back-End antwortet mit **200 OK** und einigen Daten.

## <a name="next-steps"> </a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Importieren Ihrer ersten API
> * Testen der API im Azure-Portal

Fahren Sie mit dem nächsten Tutorial fort:

> [!div class="nextstepaction"]
> [Erstellen und Veröffentlichen eines Produkts](api-management-howto-add-products.md)

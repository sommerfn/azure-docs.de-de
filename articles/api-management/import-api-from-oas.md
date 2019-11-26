---
title: Importieren einer OpenAPI-Spezifikation mit dem Azure-Portal | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie eine OpenAPI-Spezifikation mit API Management importieren.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 07/12/2019
ms.author: apimpm
ms.openlocfilehash: 2b5bcd0d3bba914b81e305c88a512645c1a1c258
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74108514"
---
# <a name="import-an-openapi-specification"></a>Importieren einer OpenAPI-Spezifikation

In diesem Artikel erfahren Sie, wie Sie eine Back-End-API mit OpenAPI-Spezifikation aus https://conferenceapi.azurewebsites.net?format=json importieren. Diese Back-End-API wird von Microsoft bereitgestellt und unter Azure gehostet. Der Artikel zeigt außerdem, wie Sie die APIM-API testen.

> [!IMPORTANT]
> Dieses [Dokument](https://blogs.msdn.microsoft.com/apimanagement/2018/04/11/important-changes-to-openapi-import-and-export/) enthält wichtige Informationen und Tipps zum OpenAPI-Import.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Importieren einer Back-End-API vom Typ „OpenAPI-Spezifikation“
> * Testen der API im Azure-Portal
> * Testen der API im Entwicklerportal

## <a name="prerequisites"></a>Voraussetzungen

Absolvieren Sie die folgende Schnellstartanleitung: [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"> </a>Importieren und Veröffentlichen einer Back-End-API

1. Wählen Sie unter **API MANAGEMENT** die Option **APIs** aus.
2. Wählen Sie in der Liste **Add a new API** (Neue API hinzufügen) die Option **OpenAPI-Spezifikation**.

    ![OpenAPI-Spezifikation](./media/import-api-from-oas/oas-api.png)
3. Nehmen Sie die entsprechenden Einstellungen vor. Sie können alle API-Werte während der Erstellung festlegen. Alternativ dazu können Sie einige Werte später festlegen, indem Sie die Registerkarte **Einstellungen** verwenden. <br/> Wenn Sie die **TAB-TASTE** drücken, werden einige (oder alle) Felder mit den Informationen aus dem angegebenen Back-End-Dienst gefüllt.

    ![Erstellen einer API](./media/api-management-get-started/create-api.png)

    |Einstellung|Wert|BESCHREIBUNG|
    |---|---|---|
    |**OpenAPI-Spezifikation**|https://conferenceapi.azurewebsites.net?format=json|Verweist auf den Dienst zum Implementieren der API. API Management leitet Anfragen an diese Adresse weiter.|
    |**Anzeigename**|*Demo Conference API*|Wenn Sie nach dem Eingeben der Dienst-URL die TAB-TASTE drücken, wird dieses Feld von APIM basierend auf den Daten in der JSON-Datei ausgefüllt. <br/>Dieser Name wird im Entwicklerportal angezeigt.|
    |**Name**|*demo-conference-api*|Stellt einen eindeutigen Namen für die API bereit. <br/>Wenn Sie nach dem Eingeben der Dienst-URL die TAB-TASTE drücken, wird dieses Feld von APIM basierend auf den Daten in der JSON-Datei ausgefüllt.|
    |**Beschreibung**|Geben Sie eine optionale Beschreibung der API an.|Wenn Sie nach dem Eingeben der Dienst-URL die TAB-TASTE drücken, wird dieses Feld von APIM basierend auf den Daten in der JSON-Datei ausgefüllt.|
    |**API-URL-Suffix**|*conference*|Das Suffix wird an die Basis-URL für den API Management-Dienst angehängt. API Management unterscheidet APIs durch deren Suffix. Daher muss jede API eines bestimmten Herausgebers ein eindeutiges Suffix haben.|
    |**URL-Schema**|*HTTPS*|Legt fest, welche Protokolle für den Zugriff auf die API verwendet werden können. |
    |**Produkte**|*Unbegrenzt*| Veröffentlichen Sie die API, indem Sie sie einem Produkt zuordnen. Geben Sie optional den Produktnamen ein, um diese neue API einem Produkt hinzuzufügen. Dieser Schritt kann mehrfach wiederholt werden, um die API mehreren Produkten hinzuzufügen.<br/>Bei Produkten handelt es sich um API-Zuordnungen. Sie können eine Reihe von APIs einfügen und sie Entwicklern über das Entwicklerportal zur Verfügung stellen. Entwickler müssen ein Produkt zunächst abonnieren, um Zugriff auf die API zu erhalten. Wenn sie ein Produkt abonnieren, erhalten sie einen Abonnementschlüssel, der für jede API in diesem Produkt gilt. Wenn Sie die APIM-Instanz erstellt haben, sind Sie bereits Administrator und haben dadurch standardmäßig alle Produkte abonniert.<br/> Standardmäßig enthält jede API Management-Instanz zwei Beispielprodukte: **Starter** und **Unbegrenzt**. |

4. Klicken Sie auf **Erstellen**.

> [!NOTE]
> Die Einschränkungen beim API-Import sind [in einem anderen Artikel](api-management-api-import-restrictions.md) dokumentiert.

## <a name="test-the-new-api-in-the-azure-portal"></a>Testen der neuen API im Azure-Portal

![Testen der API](./media/api-management-get-started/01-import-first-api-01.png)

Vorgänge können direkt aus dem Azure-Portal aufgerufen werden. Dies ist ein einfacher Weg, die Vorgänge einer API anzuzeigen und zu testen.

1. Wählen Sie die API aus, die Sie im vorherigen Schritt erstellt haben (auf der Registerkarte **APIs**).
2. Wählen Sie die Registerkarte **Testen** aus.
3. Klicken Sie auf **GetSpeakers**. Auf der Seite werden Felder für Abfrageparameter angezeigt (in diesem Fall keine) sowie Header. Einer der Header ist „Ocp-Apim-Subscription-Key“. Er steht für den Abonnementschlüssel des Produkts, das dieser API zugeordnet ist. Der Schlüssel wird automatisch eingefügt.
4. Klicken Sie auf **Senden**.

    Das Back-End antwortet mit **200 OK** und einigen Daten.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Transformieren und Schützen einer veröffentlichten API](transform-api.md)

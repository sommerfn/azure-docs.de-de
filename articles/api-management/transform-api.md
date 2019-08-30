---
title: Transformieren und Schützen Ihrer API mit Azure API Management | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihre API mithilfe von Richtlinien für Kontingente und Drosselung (Beschränken der Aufrufhäufigkeit) schützen.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 02/26/2019
ms.author: apimpm
ms.openlocfilehash: 5dec08bd4bc0a63a419d2bdc63383348a69b02db
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70067467"
---
# <a name="transform-and-protect-your-api"></a>Transformieren und Schützen Ihrer API

Das Tutorial zeigt, wie Sie Ihre API so transformieren, dass keine Informationen zu einem privaten Back-End offengelegt werden. Beispielsweise möchten Sie vielleicht die Informationen zum Technologiestapel ausblenden, der auf dem Back-End ausgeführt wird. Sie können auch die ursprünglich im Textkörper der HTTP-Antwort der API angezeigten URLs ausblenden und stattdessen eine Umleitung an das APIM-Gateway durchführen.

In diesem Tutorial wird außerdem veranschaulicht, wie einfach Sie Ihre Back-End-API schützen können, indem Sie ein Aufruflimit mithilfe von API Management konfigurieren. Beispielsweise können Sie die Anzahl von API-Aufrufen begrenzen, damit die API nicht übermäßig von den Entwicklern verwendet wird. Weitere Informationen finden Sie unter [API Management-Richtlinien](api-management-policies.md).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
>
> -   Transformieren einer API zum Entfernen von Antwortheadern
> -   Ersetzen der ursprünglichen URLs im Textkörper der API-Antwort durch APIM-Gateway-URLs
> -   Schützen einer API durch das Hinzufügen einer Richtlinie für ein Aufruflimit (Drosselung)
> -   Testen der Transformationen

![Richtlinien](./media/transform-api/api-management-management-console.png)

## <a name="prerequisites"></a>Voraussetzungen

-   Machen Sie sich mit der [Azure API Management-Terminologie](api-management-terminology.md) vertraut.
-   Machen Sie sich mit dem [Konzept von Richtlinien in Azure API Management](api-management-howto-policies.md) vertraut.
-   Bearbeiten Sie den folgenden Schnellstart: [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md)
-   Absolvieren Sie außerdem das folgende Tutorial: [Importieren und Veröffentlichen Ihrer ersten API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="transform-an-api-to-strip-response-headers"></a>Transformieren einer API zum Entfernen von Antwortheadern

In diesem Abschnitt wird gezeigt, wie Sie die HTTP-Header ausblenden, die Sie nicht für Ihre Benutzer anzeigen möchten. In diesem Beispiel werden die folgenden Header in der HTTP-Antwort gelöscht:

-   **X-Powered-By**
-   **X-AspNet-Version**

### <a name="test-the-original-response"></a>Testen der ursprünglichen Antwort

So zeigen Sie die ursprüngliche Antwort an

1. Wählen Sie in Ihrer APIM-Dienstinstanz **APIs** aus (unter **API-VERWALTUNG**).
2. Klicken Sie in der API-Liste auf **Demo Conference API**.
3. Klicken Sie oben auf der Seite auf die Registerkarte **Testen**.
4. Wählen Sie den Vorgang **GetSpeakers** aus.
5. Klicken Sie unten auf der Seite auf die Schaltfläche **Senden**.

Die ursprüngliche Antwort sieht in etwa wie folgt aus:

![Richtlinien](./media/transform-api/original-response.png)

### <a name="set-the-transformation-policy"></a>Festlegen der Transformationsrichtlinie

![Festlegen der Richtlinie für ausgehenden Datenverkehr](./media/transform-api/04-ProtectYourAPI-01-SetPolicy-Outbound.png)

1. Wählen Sie **Demo Conference API** aus.
2. Klicken Sie im oberen Seitenbereich auf die Registerkarte **Entwurf**.
3. Wählen Sie **Alle Vorgänge** aus.
4. Klicken Sie im Abschnitt **Ausgehende Verarbeitung** auf das Symbol **</>** .
5. Positionieren Sie den Cursor im **&lt;outbound&gt;** -Element.
6. Klicken Sie im rechten Fenster unter **Transformationsrichtlinien** zweimal auf **+ HTTP-Header festlegen** (um zwei Richtlinienausschnitte einzufügen).

   ![Richtlinien](./media/transform-api/transform-api.png)

7. Ändern Sie Ihren **\<outbound>** -Code, sodass er folgendermaßen aussieht:

       <set-header name="X-Powered-By" exists-action="delete" />
       <set-header name="X-AspNet-Version" exists-action="delete" />

   ![Richtlinien](./media/transform-api/set-policy.png)

8. Klicken Sie auf die Schaltfläche **Save** .

## <a name="replace-original-urls-in-the-body-of-the-api-response-with-apim-gateway-urls"></a>Ersetzen der ursprünglichen URLs im Textkörper der API-Antwort durch APIM-Gateway-URLs

In diesem Abschnitt wird gezeigt, wie Sie die ursprünglich im Textkörper der HTTP-Antwort der API angezeigten URLs ausblenden und stattdessen eine Umleitung an das APIM-Gateway durchführen.

### <a name="test-the-original-response"></a>Testen der ursprünglichen Antwort

So zeigen Sie die ursprüngliche Antwort an

1. Wählen Sie **Demo Conference API** aus.
2. Klicken Sie oben auf der Seite auf die Registerkarte **Testen**.
3. Wählen Sie den Vorgang **GetSpeakers** aus.
4. Klicken Sie unten auf der Seite auf die Schaltfläche **Senden**.

    Wie Sie sehen können, sieht die ursprüngliche Antwort so aus:

    ![Richtlinien](./media/transform-api/original-response2.png)

### <a name="set-the-transformation-policy"></a>Festlegen der Transformationsrichtlinie

1.  Wählen Sie **Demo Conference API** aus.
2.  Wählen Sie **Alle Vorgänge** aus.
3.  Klicken Sie im oberen Seitenbereich auf die Registerkarte **Entwurf**.
4.  Klicken Sie im Abschnitt **Ausgehende Verarbeitung** auf das Symbol **</>** .
5.  Positionieren Sie den Cursor im **&lt;outbound&gt;** -Element.
6.  Klicken Sie im rechten Fenster unter **Transformationsrichtlinien** auf **+ Zeichenfolge im Text suchen und ersetzen**.
7.  Ändern Sie Ihren **find-and-replace**-Code (im Element **\<outbound\>** ) ab, um die URL durch eine URL zu Ihrem APIM-Gateway zu ersetzen. Beispiel:

        <find-and-replace from="://conferenceapi.azurewebsites.net" to="://apiphany.azure-api.net/conference"/>

## <a name="protect-an-api-by-adding-rate-limit-policy-throttling"></a>Schützen einer API durch das Hinzufügen einer Richtlinie für ein Aufruflimit (Drosselung)

In diesem Abschnitt wird gezeigt, wie Sie Ihre Back-End-API schützen, indem Sie Aufruflimits konfigurieren. Beispielsweise können Sie die Anzahl von API-Aufrufen begrenzen, damit die API nicht übermäßig von den Entwicklern verwendet wird. In diesem Beispiel wird das Limit für jede Abonnement-ID auf 3 Aufrufe pro 15 Sekunden festgelegt. Nach 15 Sekunden kann ein Entwickler die API erneut aufrufen.

![Festlegen der Richtlinie für eingehenden Datenverkehr](./media/transform-api/04-ProtectYourAPI-01-SetPolicy-Inbound.png)

1.  Wählen Sie **Demo Conference API** aus.
2.  Wählen Sie **Alle Vorgänge** aus.
3.  Klicken Sie im oberen Seitenbereich auf die Registerkarte **Entwurf**.
4.  Klicken Sie im Abschnitt **Eingehende Verarbeitung** auf das Symbol **</>** .
5.  Positionieren Sie den Cursor im **&lt;inbound&gt;** -Element.
6.  Klicken Sie im rechten Fenster unter **Richtlinien für die Zugriffsbeschränkung** auf **+ Aufrufrate pro Schlüssel einschränken**.
7.  Modifizieren Sie Ihren **rate-limit-by-key**-Code (im **\<inbound\>** -Element) folgendermaßen:

        <rate-limit-by-key calls="3" renewal-period="15" counter-key="@(context.Subscription.Id)" />

## <a name="test-the-transformations"></a>Testen der Transformationen

Zu diesem Zeitpunkt sieht Ihre Richtlinie in etwa wie folgt aus, wenn Sie den Code im Code-Editor betrachten:

    <policies>
        <inbound>
            <rate-limit-by-key calls="3" renewal-period="15" counter-key="@(context.Subscription.Id)" />
            <base />
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <set-header name="X-Powered-By" exists-action="delete" />
            <set-header name="X-AspNet-Version" exists-action="delete" />
            <find-and-replace from="://conferenceapi.azurewebsites.net:443" to="://apiphany.azure-api.net/conference"/>
            <find-and-replace from="://conferenceapi.azurewebsites.net" to="://apiphany.azure-api.net/conference"/>
            <base />
        </outbound>
        <on-error>
            <base />
        </on-error>
    </policies>

Im verbleibenden Teil dieses Abschnitts werden die Richtlinientransformationen getestet, die Sie in diesem Artikel festgelegt haben.

### <a name="test-the-stripped-response-headers"></a>Testen der entfernten Antwortheader

1. Wählen Sie **Demo Conference API** aus.
2. Wählen Sie die Registerkarte **Testen** aus.
3. Klicken Sie auf den Vorgang **GetSpeakers**.
4. Klicken Sie auf **Senden**.

    Wie Sie sehen können, wurden die Header entfernt:

    ![Richtlinien](./media/transform-api/final-response1.png)

### <a name="test-the-replaced-url"></a>Testen der ersetzten URL

1. Wählen Sie **Demo Conference API** aus.
2. Wählen Sie die Registerkarte **Testen** aus.
3. Klicken Sie auf den Vorgang **GetSpeakers**.
4. Klicken Sie auf **Senden**.

    Wie Sie sehen können, wurde die URL ersetzt.

    ![Richtlinien](./media/transform-api/final-response2.png)

### <a name="test-the-rate-limit-throttling"></a>Testen des Aufruflimits (Drosselung)

1. Wählen Sie **Demo Conference API** aus.
2. Wählen Sie die Registerkarte **Testen** aus.
3. Klicken Sie auf den Vorgang **GetSpeakers**.
4. Klicken Sie dreimal hintereinander auf **Senden**.

    Nachdem Sie die Anforderung dreimal gesendet haben, erhalten Sie die Antwort **429 Zu viele Anforderungen**.

5. Warten Sie etwa 15 Sekunden ab, und klicken Sie erneut auf **Senden**. Jetzt sollten Sie die Antwort **200 OK** erhalten.

    ![Drosselung](./media/transform-api/test-throttling.png)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Rate-Limits-and-Quotas/player]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
>
> -   Transformieren einer API zum Entfernen von Antwortheadern
> -   Ersetzen der ursprünglichen URLs im Textkörper der API-Antwort durch APIM-Gateway-URLs
> -   Schützen einer API durch das Hinzufügen einer Richtlinie für ein Aufruflimit (Drosselung)
> -   Testen der Transformationen

Fahren Sie mit dem nächsten Tutorial fort:

> [!div class="nextstepaction"]
> [Überwachen Ihrer API](api-management-howto-use-azure-monitor.md)

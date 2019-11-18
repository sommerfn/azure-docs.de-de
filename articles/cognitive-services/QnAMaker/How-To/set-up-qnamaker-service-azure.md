---
title: Einrichten eines QnA Maker-Diensts – QnA Maker
titleSuffix: Azure Cognitive Services
description: Vor dem Erstellen von QnA Maker-Wissensdatenbanken müssen Sie zuerst einen QnA Maker-Dienst in Azure einrichten. Jeder mit der Berechtigung zum Erstellen neuer Ressourcen in einem Abonnement kann einen QnA Maker-Dienst einrichten.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 6d52062561e3f08a214f3e191706583edc844786
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73794210"
---
# <a name="manage-qna-maker-resources"></a>QnA Maker-Ressourcen

Vor dem Erstellen von QnA Maker-Wissensdatenbanken müssen Sie zuerst einen QnA Maker-Dienst in Azure einrichten. Jeder mit der Berechtigung zum Erstellen neuer Ressourcen in einem Abonnement kann einen QnA Maker-Dienst einrichten.

## <a name="types-of-keys-in-qna-maker"></a>Typen von Schlüsseln in QnA Maker

Ihr QnA Maker-Dienst verwendet zwei Arten von Schlüsseln: **Abonnementschlüssel** und **Endpunktschlüssel**.

![Schlüsselverwaltung](../media/qnamaker-how-to-key-management/key-management.png)

|NAME|Location|Zweck|
|--|--|--|
|Abonnementschlüssel|[Azure-Portal](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)|Diese Schlüssel werden verwendet, um auf die [QnA Maker-Verwaltungsdienst-APIs](https://go.microsoft.com/fwlink/?linkid=2092179) zuzugreifen. Mit diesen APIs können Sie die Fragen und Antworten in Ihrer Wissensdatenbank bearbeiten und Ihre Wissensdatenbank veröffentlichen. Diese Schlüssel werden erstellt, wenn Sie einen neuen QnA Maker-Dienst erstellen.<br><br>Sie finden diese Schlüssel in der Ressource **Cognitive Services** auf der Seite **Schlüssel**.|
|Endpunktschlüssel|[QnA Maker-Portal](http://www.qnamaker.ai)|Diese Schlüssel werden für den Zugriff auf den Endpunkt der veröffentlichten Wissensdatenbank verwendet, um eine Antwort auf eine Benutzerfrage abzurufen. In der Regel verwenden Sie diesen Endpunkt in Ihrem Chatbot oder im Clientanwendungscode, der eine Verbindung mit dem QnA Maker-Dienst herstellt. Diese Schlüssel werden erstellt, wenn Sie Ihre QnA Maker-Wissensdatenbank veröffentlichen.<br><br>Sie finden diese Schlüssel auf der Seite **Diensteinstellungen**. Suchen Sie diese Seite im Menü des Benutzers in der oberen rechten Ecke der Seite im Dropdownmenü.|

## <a name="create-a-new-qna-maker-service"></a>Erstellen eines neuen QnA Maker-Diensts

Mit diesem Verfahren werden die Azure-Ressourcen erstellt, die zum Verwalten des Inhalts der Wissensdatenbank benötigt werden. Nachdem Sie diese Schritte ausgeführt haben, finden Sie die _Abonnementschlüssel_ auf der Seite **Schlüssel** für die Ressource im Azure-Portal.

1. Melden Sie sich beim Azure-Portal an, und [erstellen Sie eine QnA Maker-Ressource](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker).

1. Nachdem Sie die Geschäftsbedingungen gelesen haben, wählen Sie **Erstellen** aus:

    ![Erstellen eines neuen QnA Maker-Diensts](../media/qnamaker-how-to-setup-service/create-new-resource-button.png)

1. Wählen Sie in **QnA Maker** die geeigneten Tarife und Regionen aus:

    ![Erstellen eines neuen QnA Maker-Diensts – Tarif und Regionen](../media/qnamaker-how-to-setup-service/enter-qnamaker-info.png)

    * Geben Sie im Feld **Name** einen eindeutigen Namen zur Identifizierung dieses QnA Maker-Diensts ein. Dieser Name kennzeichnet auch den QnA Maker-Endpunkt, dem Ihre Wissensdatenbanken zugeordnet werden sollen.
    * Wählen Sie das **Abonnement** aus, unter dem die QnA Maker-Ressource bereitgestellt werden soll.
    * Wählen Sie den **Tarif** für die QnA Maker-Verwaltungsdienste (Portal und Verwaltungs-APIs) aus. Lesen Sie [weitere Informationen zu SKU-Preisen](https://aka.ms/qnamaker-pricing).
    * Erstellen Sie eine neue **Ressourcengruppe** (empfohlen), oder verwenden Sie eine vorhandene Ressourcengruppe, in der diese QnA Maker-Ressource bereitgestellt werden soll. QnA Maker erstellt verschiedene Azure-Ressourcen. Wenn Sie eine Ressourcengruppe für diese Ressourcen erstellen, können Sie die Ressourcen anhand des Ressourcengruppennamens leicht finden, verwalten und löschen.
    * Wählen Sie einen **Ressourcengruppenstandort** aus.
    * Wählen Sie den **Tarif für Suche** des Azure Cognitive Search-Diensts aus. Wenn die Option „Free-Tarif“ nicht verfügbar (abgeblendet) ist, bedeutet dies, dass Ihnen schon über Ihr Abonnement ein kostenloser Dienst bereitgestellt wird. In diesem Fall müssen Sie mit dem Basic-Tarif beginnen. Siehe [Azure Cognitive Search – Preise](https://azure.microsoft.com/pricing/details/search/).
    * Wählen Sie den **Suchspeicherort** aus, an dem die Azure Cognitive Search-Indizes bereitgestellt werden sollen. Einschränkungen zu den verpflichtenden Speicherorten für Kundendaten helfen bei der Bestimmung des Speicherorts, den Sie für Azure Cognitive Search wählen.
    * Geben Sie im Feld **App-Name** einen Namen für Ihre Azure App Service-Instanz ein.
    * Standardmäßig wird App Service nach dem Standardtarif (S1) abgerechnet. Sie können den Plan nach der Erstellung ändern. Erfahren Sie mehr über [App Service-Preise](https://azure.microsoft.com/pricing/details/app-service/).
    * Wählen Sie den **Website-Speicherort** aus, an dem App Service bereitgestellt werden soll.

        > [!NOTE]
        > Der **Suchspeicherort** kann vom **Website-Speicherort** abweichen.

    * Wählen Sie, ob Sie **Application Insights** aktivieren möchten. Wenn **Application Insights** aktiviert ist, sammelt QnA Maker Telemetriedaten zu Datenverkehr, Chatprotokollen und Fehlern.
    * Wählen Sie den **App Insights-Speicherort** aus, an dem die Application Insights-Ressource bereitgestellt werden soll.
    * Zur Kosteneinsparung können Sie einige, aber nicht alle Azure-Ressourcen, die für QnA Maker erstellt wurden, [freigeben](#share-existing-services-with-qna-maker). 

1. Nachdem Sie alle Felder überprüft haben, wählen Sie **Erstellen** aus. Der Vorgang kann einige Minuten dauern.

1. Nachdem die Bereitstellung abgeschlossen ist, werden die folgenden neu erstellten Ressourcen in Ihrem Abonnement angezeigt:

   ![Im neuen QnA Maker-Dienst erstellte Ressource](../media/qnamaker-how-to-setup-service/resources-created.png)

    Die Ressource mit dem _Cognitive Services_-Typ enthält Ihre _Abonnementschlüssel_.

## <a name="find-subscription-keys-in-the-azure-portal"></a>Suchen von Abonnementschlüsseln im Azure-Portal

Sie können Ihre Abonnementschlüssel in dem Azure-Portal anzeigen und zurücksetzen, in dem Sie die QnA Maker-Ressource erstellt haben.

1. Wechseln Sie im Azure-Portal zur QnA Maker-Ressource, und wählen Sie die Ressource mit dem _Cognitive Services_-Typ aus:

    ![QnA Maker-Ressourcenliste](../media/qnamaker-how-to-key-management/qnamaker-resource-list.png)

2. Wechseln Sie zu **Schlüssel**:

    ![Abonnementschlüssel](../media/qnamaker-how-to-key-management/subscription-key.PNG)

## <a name="find-endpoint-keys-in-the-qna-maker-portal"></a>Suchen von Endpunktschlüsseln im QnA Maker-Portal

Der Endpunkt befindet sich in demselben Bereich wie die Ressource, da die Endpunktschlüssel verwendet werden, um einen Aufruf an die Wissensdatenbank zu senden.

Endpunktschlüssel können im [QnA Maker-Portal](https://qnamaker.ai) verwaltet werden.

1. Melden Sie sich beim [QnA Maker-Portal](https://qnamaker.ai) an, wechseln Sie zu Ihrem Profil, und wählen Sie **Diensteinstellungen** aus:

    ![Endpunktschlüssel](../media/qnamaker-how-to-key-management/Endpoint-keys.png)

2. Zeigen Sie Ihre Schlüssel an, oder setzen Sie die Schlüssel zurück:

    ![Endpunktschlüssel-Manager](../media/qnamaker-how-to-key-management/Endpoint-keys1.png)

    >[!NOTE]
    >Aktualisieren Sie Ihre Schlüssel, wenn Sie denken, dass sie gefährdet sind. Dazu müssen möglicherweise entsprechende Änderungen an Ihrem Clientanwendungs- oder Botcode vorgenommen werden.

## <a name="share-existing-services-with-qna-maker"></a>Freigeben vorhandener Dienste mit QnA Maker

QnA Maker erstellt verschiedene Azure-Ressourcen. Wenn Sie den Verwaltungsaufwand reduzieren und von der Kostenteilung profitieren möchten, informieren Sie sich anhand der folgenden Tabelle, was Sie freigeben können und was nicht:

|Dienst|Freigabe|`Reason`|
|--|--|--|
|Cognitive Services|X|Programmbedingt nicht möglich.|
|App Service-Plan|✔|Der für einen App Service-Plan zugewiesene Speicherplatz auf dem Datenträger. Wenn andere Apps, die denselben App Service-Plan gemeinsam nutzen, eine erhebliche Menge an Speicherplatz belegen, treten bei der QnAMaker App Service-Instanz Probleme auf.|
|App Service|X|Programmbedingt nicht möglich.|
|Application Insights|✔|Gemeinsame Nutzung möglich.|
|Suchdienst|✔|1. `testkb` ist ein reservierter Name für den QnAMaker-Dienst, der von anderen Benutzern nicht verwendet werden kann.<br>2. Eine Synonymzuordnung nach dem Namen `synonym-map` ist für den QnAMaker-Dienst reserviert.<br>3. Die Anzahl von veröffentlichten Wissensdatenbanken wird durch den Suchdiensttarif begrenzt. Wenn freie Indizes verfügbar sind, können sie von anderen Diensten genutzt werden.|

Erfahren Sie mehr über den [App-Dienst](../../../app-service/index.yml) und den [Suchdienst](../../../search/index.yml).

### <a name="using-a-single-search-service"></a>Verwenden eines einzelnen Suchdiensts

Wenn Sie einen QnA-Dienst und dessen Abhängigkeiten (z.B. Search) über das Portal erstellen, wird automatisch ein Suchdienst erstellt und mit dem QnA Maker-Dienst verknüpft. Nachdem diese Ressourcen erstellt wurden, können Sie die App Service-Einstellung aktualisieren, um einen bereits vorhandenen Suchdienst zu nutzen und den soeben erstellten Suchdienst zu entfernen.

Wenn Sie einen QnA-Dienst über Azure Resource Manager-Vorlagen erstellen, können Sie alle Ressourcen erstellen und die App Service-Erstellung so steuern, dass sie einen vorhandenen Suchdienst nutzt.

## <a name="upgrade-qna-maker"></a>Upgraden von QnA Maker

|Upgrade|`Reason`|
|--|--|
|[Upgrade](#upgrade-qna-maker-sku) der QnA Maker Management-SKU|Ihre Wissensdatenbank soll mehr Fragen und Antworten enthalten.|
|[Upgrade](#upgrade-app-service) der App Service-SKU|Ihre Wissensdatenbank muss mehr Anforderungen aus Ihrer Client-App verarbeiten, z.B. einen Chatbot.|
|[Upgrade](#upgrade-the-azure-cognitive-search-service) des Azure Cognitive Search-Diensts|Sie benötigen voraussichtlich eine Vielzahl von Wissensdatenbanken.|


### <a name="upgrade-qna-maker-sku"></a>Upgraden der QnA Maker-SKU

Wenn Ihre Wissensdatenbank mehr Fragen und Antworten enthalten soll, die über Ihren aktuellen Tarif hinausgehen, upgraden Sie den Tarif Ihres QnA Maker-Diensts.

So führen Sie ein Upgrade der QnA Maker Management-SKU aus:

1. Wechseln Sie im Azure-Portal zu Ihrer QnA Maker-Ressource, und wählen Sie **Tarif** aus.

    ![QnA Maker-Ressource](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource.png)

1. Wählen Sie die passende SKU aus, und drücken Sie **Auswählen**.

    ![QnA Maker-Preise](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-pricing-page.png)

### <a name="upgrade-app-service"></a>Upgraden von App Service

 Wenn Ihre Wissensdatenbank mehr Anforderungen aus Ihrer Client-App verarbeiten muss, upgraden Sie Ihren App Service-Tarif.

Sie können den App Service [zentral hochskalieren](https://docs.microsoft.com/azure/app-service/manage-scale-up) oder horizontal hochskalieren.

Wechseln Sie im Azure-Portal zur App Service-Ressource, und wählen Sie nach Bedarf die Option **Zentral hochskalieren** oder **Horizontal hochskalieren** aus.

![QnA Maker App Service-Staffelung](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-scale.png)

### <a name="upgrade-the-azure-cognitive-search-service"></a>Upgrade des Azure Cognitive Search-Diensts

Wenn Sie voraussichtlich eine Vielzahl von Wissensdatenbanken benötigen, upgraden Sie den Tarif Ihres Azure Cognitive Search-Diensts.

Sie können derzeit kein direktes Upgrade der Azure Search-SKU ausführen. Allerdings können Sie eine neue Azure Search-Ressource mit der gewünschten SKU erstellen, die Daten in der neuen Ressource wiederherstellen und anschließend eine Verknüpfung mit dem QnA Maker-Stapel herstellen. Gehen Sie dazu folgendermaßen vor:

1. Erstellen Sie im Azure-Portal eine neue Azure Search-Ressource, und wählen Sie die gewünschte SKU aus.

    ![QnA Maker-Azure Search-Ressource](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-new.png)

1. Stellen Sie die Indizes aus Ihrer ursprünglichen Azure Search-Ressource in der neuen wieder her. Sehen Sie dazu den [Beispielcode für die Wiederherstellung aus einer Sicherung](https://github.com/pchoudhari/QnAMakerBackupRestore).

1. Nachdem die Daten wiederhergestellt wurden, wechseln Sie zu Ihrer neuen Azure Search-Ressource, wählen Sie **Schlüssel** aus, und notieren Sie sich den **Namen** und den **Administratorschlüssel**:

    ![Schlüssel von QnA Maker-Azure Search](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-keys.png)

1. Um die neue Azure Search-Ressource mit dem QnA Maker-Stapel zu verknüpfen, wechseln Sie zur QnA Maker App Service-Instanz.

    ![QnA Maker App Service-Instanz](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource-list-appservice.png)

1. Wählen Sie **Anwendungseinstellungen** aus, und ändern Sie die Einstellungen in den Feldern **AzureSearchName** und **AzureSearchAdminKey** aus Schritt 3.

    ![QnA Maker App Service-Einstellung](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-settings.png)

1. Starten Sie die App Service-Instanz neu.

    ![Neustarten der QnA Maker App Service-Instanz](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="get-the-latest-runtime-updates"></a>Abrufen der neuesten Runtime-Updates

Die QnA Maker-Runtime ist Teil der Azure App Service-Instanz, die beim [Erstellen eines QnA Maker-Diensts](./set-up-qnamaker-service-azure.md) im Azure-Portal bereitgestellt wird. Updates für die Runtime werden in regelmäßigen Abständen bereitgestellt. Die QnA Maker App Service-Instanz befindet sich nach der Websiteerweiterung vom April 2019 (Version 5+) im automatischen Updatemodus. Dieses Update ist so konzipiert, dass es während Upgrades keine Downtime berücksichtigt.

Sehen Sie sich die aktuelle Version unter https://www.qnamaker.ai/UserSettings an. Wenn Ihre Version älter als Version 5.x ist, müssen Sie App Service neu starten, damit die neuesten Updates angewendet werden:

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem QnA Maker-Dienst (Ressourcengruppe).

    ![Azure-Ressourcengruppe von QnA Maker](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

1. Wählen Sie die App Service-Instanz aus, und öffnen Sie den Abschnitt **Übersicht**.

    ![QnA Maker App Service-Instanz](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

1. Starten Sie App Service neu. Der Updateprozess sollte innerhalb weniger Sekunden abgeschlossen sein. Abhängige Anwendungen oder Bots, in denen dieser QnA Maker-Dienst verwendet wird, stehen während dieses Neustartzeitraums für Endbenutzer nicht zur Verfügung.

    ![Neustarten der QnA Maker App Service-Instanz](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="management-service-region"></a>Verwaltungsdienstregion

Der Verwaltungsdienst von QnA Maker wird nur für das QnA Maker-Portal und für die anfängliche Datenverarbeitung genutzt. Dieser Dienst steht nur in der Region „USA, Westen“ zur Verfügung. In diesem Dienst (USA, Westen) werden keine Kundendaten gespeichert.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über den [App-Dienst](../../../app-service/index.yml) und den [Suchdienst](../../../search/index.yml).

> [!div class="nextstepaction"]
> [Erstellen und Veröffentlichen einer Wissensdatenbank](../Quickstarts/create-publish-knowledge-base.md)

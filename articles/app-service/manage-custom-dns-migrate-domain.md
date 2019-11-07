---
title: Migrieren eines aktiven DNS-Namens – Azure App Service | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen benutzerdefinierte DNS-Domänennamen, der bereits einer Livewebsite zugewiesen ist, ohne Ausfallzeiten zu Azure App Service migrieren.
services: app-service
documentationcenter: ''
author: cephalin
manager: gwallace
tags: top-support-issue
ms.assetid: 10da5b8a-1823-41a3-a2ff-a0717c2b5c2d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/21/2019
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 172003b13807720df2431a3610947b36d8303fed
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73470370"
---
# <a name="migrate-an-active-dns-name-to-azure-app-service"></a>Migrieren eines aktiven DNS-Namens zu Azure App Service

In diesem Artikel erfahren Sie, wie Sie einen aktiven DNS-Namen ohne Ausfallzeiten zu [Azure App Service](../app-service/overview.md) migrieren.

Wenn Sie eine Livewebsite und den dazugehörigen DNS-Domänennamen zu App Service migrieren, wird über diesen DNS-Namen bereits Livedatenverkehr abgewickelt. Damit während der Migration keine Ausfallzeiten bei der DNS-Auflösung auftreten, können Sie den aktiven DNS-Namen vorab an Ihre App Service-App binden.

Wenn ein Ausfall der DNS-Auflösung kein Problem darstellt, finden Sie weitere Informationen unter [Zuordnen eines vorhandenen benutzerdefinierten DNS-Namens zu Azure App Service](app-service-web-tutorial-custom-domain.md).

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie den folgenden Vorbereitungsschritt aus:

- [Vergewissern Sie sich, dass sich Ihre App Service-App nicht im FREE-Tarif befindet.](app-service-web-tutorial-custom-domain.md#checkpricing)

## <a name="bind-the-domain-name-preemptively"></a>Binden des Domänennamens bereits im Vorfeld

Wenn Sie eine benutzerdefinierte Domäne bereits im Vorfeld binden, haben Sie die folgenden beiden Vorgänge ausgeführt, bevor Sie Änderungen an Ihren DNS-Einträgen vornehmen:

- Überprüfen des Domänenbesitzes
- Aktivieren des Domänennamens für Ihre App

Wenn Sie dann Ihren benutzerdefinierten DNS-Namen von der alten Website zur App Service-App migrieren, treten bei der DNS-Auflösung keinerlei Ausfälle auf.

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-domain-verification-record"></a>Erstellen eines Domänenüberprüfungseintrags

Fügen Sie zum Überprüfen des Domänenbesitzes einen TXT-Eintrag hinzu. Der TXT-Eintrag dient zur Zuordnung von _awverify.&lt;Unterdomäne>_ zu _&lt;App-Name>.azurewebsites.net_. 

Welchen TXT-Eintrag Sie benötigen, hängt vom zu migrierenden DNS-Eintrag ab. Beispiele finden Sie in der folgenden Tabelle. (`@` stellt in der Regel die Stammdomäne dar.)

| DNS-Beispieleintrag | TXT-Host | TXT-Wert |
| - | - | - |
| \@ (Stamm) | _awverify_ | _&lt;App-Name&gt;.azurewebsites.net_ |
| www (Unterdomäne) | _awverify.www_ | _&lt;App-Name&gt;.azurewebsites.net_ |
| \* (Platzhalter) | _awverify.\*_ | _&lt;App-Name&gt;.azurewebsites.net_ |

Beachten Sie auf der Seite mit den DNS-Einträgen den Eintragstyp des DNS-Namens, den Sie migrieren möchten. App Service unterstützt Zuordnungen von CNAME- und A-Einträgen.

> [!NOTE]
> Für bestimmte Anbieter, z.B. CloudFlare, ist `awverify.*` kein gültiger Datensatz. Verwenden Sie stattdessen nur `*`.

> [!NOTE]
> Platzhalterdatensätze (`*`) überprüfen keine Unterdomänen mit vorhandenem CNAME-Eintrag. Möglicherweise müssen Sie für jede Unterdomäne explizit einen TXT-Eintrag erstellen.


### <a name="enable-the-domain-for-your-app"></a>Aktivieren der Domäne für Ihre App

Wählen Sie im [Azure-Portal](https://portal.azure.com) im linken Navigationsbereich der App-Seite die Option **Benutzerdefinierte Domänen** aus. 

![Menü „Benutzerdefinierte Domänen“](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

Wählen Sie auf der Seite **Benutzerdefinierte Domänen** neben **Hostname hinzufügen** das Symbol **+** aus.

![Hinzufügen des Hostnamens](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Geben Sie den vollqualifizierten Domänennamen ein, für den Sie den TXT-Eintrag hinzugefügt haben (beispielsweise `www.contoso.com`). Für eine Platzhalterdomäne (Beispiel: \*.contoso.com), können Sie einen beliebigen DNS-Namen verwenden, der der Platzhalterdomäne entspricht. 

Wählen Sie **Überprüfen**.

Die Schaltfläche **Hostnamen hinzufügen** wird aktiviert. 

Vergewissern Sie sich, dass **Typ des Hostnamenseintrags** auf den DNS-Eintragstyp festgelegt ist, den Sie migrieren möchten.

Wählen Sie **Hostnamen hinzufügen**.

![Hinzufügen des DNS-Namens zur App](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

Unter Umständen dauert es eine Weile, bis der neue Hostname auf der Seite **Benutzerdefinierte Domänen** der App angezeigt wird. Aktualisieren Sie den Browser, um die Daten zu aktualisieren.

![Hinzugefügter CNAME-Eintrag](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

Ihr benutzerdefinierter DNS-Name ist nun in Ihrer Azure-App aktiviert. 

## <a name="remap-the-active-dns-name"></a>Neuzuordnen des aktiven DNS-Namens

Nun muss nur noch der aktive DNS-Eintrag neu zugeordnet werden, damit er auf App Service verweist. Momentan verweist er immer noch auf Ihre alte Website.

<a name="info"></a>

### <a name="copy-the-apps-ip-address-a-record-only"></a>Kopieren der IP-Adresse der App (nur A-Eintrag)

Wenn Sie einen CNAME-Eintrag neu zuordnen, überspringen Sie diesen Abschnitt. 

Für die Neuzuordnung eines A-Eintrags benötigen Sie die externe IP-Adresse der App Service-App (zu finden auf der Seite **Benutzerdefinierte Domänen**).

Wählen Sie rechts oben das **X** aus, um die Seite **Hostname hinzufügen** zu schließen. 

Kopieren Sie auf der Seite **Benutzerdefinierte Domänen** die IP-Adresse der App.

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

### <a name="update-the-dns-record"></a>Aktualisieren des DNS-Eintrags

Wählen Sie auf der DNS-Eintragsseite Ihres Domänenanbieters den neu zuzuordnenden DNS-Eintrag aus.

Ordnen Sie für das Stammdomänenbeispiel `contoso.com` den A- oder CNAME-Eintrag wie bei den Beispielen in der folgenden Tabelle zu: 

| FQDN-Beispiel | Eintragstyp | Host | Wert |
| - | - | - | - |
| contoso.com (Stammdomäne) | Eine Datei | `@` | IP-Adresse aus dem Schritt [Kopieren der IP-Adresse der App](#info) |
| www\.contoso.com (Unterdomäne) | CNAME | `www` | _&lt;App-Name&gt;.azurewebsites.net_ |
| \*.contoso.com (Platzhalter) | CNAME | _\*_ | _&lt;App-Name&gt;.azurewebsites.net_ |

Speichern Sie die Einstellungen.

DNS-Abfragen werden umgehend nach der DNS-Verteilung zu Ihrer App Service-App aufgelöst.

## <a name="active-domain-in-azure"></a>Aktive Domäne in Azure

Sie können eine aktive benutzerdefinierte Domäne in Azure zwischen Abonnements oder innerhalb desselben Abonnements migrieren. Eine solche Migration ohne Ausfallzeiten erfordert jedoch, dass die Quell-App und die Ziel-App zu einem bestimmten Zeitpunkt derselben benutzerdefinierten Domäne zugewiesen sind. Daher müssen Sie sicherstellen, dass die beiden Apps nicht in derselben Bereitstellungseinheit (intern als Webspace bezeichnet) bereitgestellt sind. Ein Domänenname kann nur einer App in jeder Bereitstellungseinheit zugewiesen werden.

Sie finden die Bereitstellungseinheit für Ihre App, indem Sie sich den Domänennamen der FTP/S-URL `<deployment-unit>.ftp.azurewebsites.windows.net` ansehen. Überprüfen Sie, und stellen Sie sicher, dass sich die Bereitstellungseinheit der Quell-App von der der Ziel-App unterscheidet. Die Bereitstellungseinheit einer App wird von dem [App Service-Plan bestimmt](overview-hosting-plans.md), in dem sie sich befindet. Er wird nach dem Zufallsprinzip von Azure ausgewählt, wenn Sie den Plan erstellen, und kann nicht geändert werden. Azure stellt nur sicher, dass sich zwei Pläne in derselben Bereitstellungseinheit befinden, wenn Sie [sie in derselben Ressourcengruppe *und* derselben Region erstellen](app-service-plan-manage.md#create-an-app-service-plan), aber es verfügt über keine Logik, um sicherzustellen, dass sich die Pläne in unterschiedlichen Bereitstellungseinheiten befinden. Die einzige Möglichkeit, einen Plan in einer anderen Bereitstellungseinheit zu erstellen, besteht darin, einen Plan in einer neuen Ressourcengruppe oder Region zu erstellen, bis Sie eine andere Bereitstellungseinheit erhalten.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich darüber, wie Sie ein benutzerdefiniertes SSL-Zertifikat an App Service binden.

> [!div class="nextstepaction"]
> [Binden eines SSL-Zertifikats an Azure App Service](configure-ssl-bindings.md)

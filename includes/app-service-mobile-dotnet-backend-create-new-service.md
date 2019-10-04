---
title: include file
description: include file
services: app-service\mobile
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 06/20/2019
ms.author: crdun
ms.custom: include file
ms.openlocfilehash: 72a69359d412a7560472fbb73ec525ab5d4a4fce
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67325794"
---
1. Melden Sie sich beim [Azure-Portal] an.

2. Klicken Sie auf **Ressource erstellen**.

3. Geben Sie in das Suchfeld **Web-App**ein.
    
4. Klicken Sie in der Ergebnisliste über den Marketplace auf **Web-App**.

5. Wählen Sie Ihr **Abonnement** und Ihre **Ressourcengruppe** aus. Dabei können Sie entweder eine bereits vorhandene Ressourcengruppe auswählen _oder_ eine neue erstellen und dabei den gleichen Namen wie für die App verwenden.

6. Geben Sie einen eindeutigen **Namen** für Ihre Web-App ein.

7. Wählen Sie als Standardoption zum **Veröffentlichen** **Code** aus.

8. Im **Laufzeitstapel** müssen Sie unter **ASP.NET** oder **Node** eine Version auswählen. Wenn Sie an einem .NET-Backend arbeiten, wählen Sie eine ASP.NET-Version aus. Wenn Sie aber an einer Node-basierten Anwendung arbeiten, wählen Sie eine Node-Version aus.

9. Wählen Sie ein **Betriebssystem** aus (entweder Linux oder Windows). 

10. Wählen Sie die **Region** aus, für die diese App bereitgestellt werden soll. 

11. Wählen Sie einen passenden **App Service-Plan** aus, und klicken Sie auf **Überprüfen und erstellen**. 

12. Wählen Sie unter **Ressourcengruppe** eine vorhandene Ressourcengruppe aus, _oder_ erstellen Sie eine neue. (Verwenden Sie dabei den gleichen Namen wie für die App.)

13. Klicken Sie auf **Create**. Warten Sie ein paar Minuten, bis der Dienst bereitgestellt wurde, bevor Sie fortfahren. Das Benachrichtigungssymbol (Glocke) in der Portalüberschrift informiert Sie über Statusupdates.

14. Sobald die Bereitstellung abgeschlossen ist, klicken Sie erst auf den Abschnitt **Bereitstellungsdetails** und dann auf die Ressource vom Typ **Microsoft.Web/sites**. Dann werden Sie zu der App Service-Web-App geleitet, die Sie gerade erstellt haben. 

15. Klicken Sie unter **Einstellungen** auf das Blatt **Konfiguration** und unter **Anwendungseinstellungen** auf die Schaltfläche **Neue Anwendungseinstellung**.

16. Geben Sie auf der Seite **Anwendungseinstellung hinzufügen/bearbeiten** den **Namen** **MobileAppsManagement_EXTENSION_VERSION** und den Wert **Latest** (Aktuell) ein. Drücken Sie anschließend auf „OK“.

Nun können Sie diese neu erstellte App Service-Web-App als mobile App verwenden.

<!-- URLs. -->
[Azure-Portal]: https://portal.azure.com/
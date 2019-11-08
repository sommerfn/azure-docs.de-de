---
title: Erstellen einer Azure API Management-Instanz | Microsoft-Dokumentation
description: Führen Sie die Schritte dieses Tutorials zum Erstellen einer neuen Azure API Management-Instanz aus.
services: api-management
documentationcenter: ''
author: vladvino
manager: cflower
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: quickstart
ms.custom: mvc
ms.date: 11/28/2017
ms.author: apimpm
ms.openlocfilehash: a0917f2649f1b780429f78406900c599c5ac853e
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73472011"
---
# <a name="create-a-new-azure-api-management-service-instance"></a>Erstellen einer neuen Azure API Management-Dienstinstanz

Azure API Management (APIM) unterstützt Organisationen beim Veröffentlichen von APIs für externe Entwickler, Partnerentwickler und interne Entwickler, um das volle Potenzial von Daten und Diensten ausschöpfen zu können. API Management bietet die Kernkompetenzen zur Sicherstellung eines erfolgreichen API-Programms in Form von Entwicklerengagement, geschäftlichen Erkenntnissen, Analysen, Sicherheit und Schutz. Mit APIM können Sie moderne API-Gateways für vorhandene Back-End-Dienste, die an einem beliebigen Ort gehostet werden, erstellen und verwalten. Weitere Informationen finden Sie im Thema [Übersicht](api-management-key-concepts.md).

In dieser Schnellstartanleitung werden die Schritte zum Erstellen einer neuen API Management-Instanz mit dem Azure-Portal beschrieben.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

![API Management-Instanz](./media/get-started-create-service-instance/get-started-create-service-instance-created.png)

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-a-new-service"></a>Erstellen eines neuen Diensts

1. Wählen Sie im Menü des Azure-Portals die Option **Ressource erstellen** aus. Die Option **Ressource erstellen** kann auch auf der **Startseite** von Azure ausgewählt werden. 
   
   ![Auswählen von „Ressource erstellen“](./media/get-started-create-service-instance/00-CreateResource-01.png)
   
1. Wählen Sie im Bildschirm **Neu** die Option **Integration** und anschließend **API Management** aus.
   
   ![Neue Azure API Management-Instanz](./media/get-started-create-service-instance/00-CreateResource-02.png)
   
1. Geben Sie im Bildschirm **API Management-Dienst** Einstellungen ein.
   
   ![Neue Instanz](./media/get-started-create-service-instance/get-started-create-service-instance-create-new.png)
   
   | Einstellung                 | Empfohlener Wert                               | Beschreibung                                                                                                                                                                                                                                                                                                                         |
|-------------------------|-----------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                | Eindeutiger Name für Ihren API Management-Dienst | Den Namen können Sie später nicht mehr ändern. Der Name des Diensts wird verwendet, um einen Standarddomänennamen in der Form *{Name}.azure-api.net* zu generieren. Wenn Sie einen benutzerdefinierten Domänennamen verwenden möchten, lesen Sie [Configure a custom domain name](configure-custom-domain.md) (Konfigurieren eines benutzerdefinierten Domänennamens). <br/> Der Dienstname wird verwendet, um auf den Dienst und die entsprechende Azure-Ressource zu verweisen. |
| **Abonnement**        | Ihr Abonnement                             | Das Abonnement, unter dem diese neue Dienstinstanz erstellt wird. Sie können aus den verschiedenen Azure-Abonnements, auf die Sie Zugriff haben, ein Abonnement auswählen.                                                                                                                                                            |
| **Ressourcengruppe**      | *apimResourceGroup*                           | Sie können eine neue oder eine vorhandene Ressource auswählen. Eine Ressourcengruppe ist eine Sammlung von Ressourcen mit gleichem Lebenszyklus, gleichen Berechtigungen und gleichen Richtlinien. [Hier](../azure-resource-manager/resource-group-overview.md#resource-groups)erhalten Sie weitere Informationen.                                                                                                  |
| **Location**            | *USA, Westen*                                    | Wählen Sie die geografische Region in Ihrer Nähe aus. Im Dropdownlistenfeld werden nur die verfügbaren Regionen für den API Management-Dienst angezeigt.                                                                                                                                                                                                          |
| **Name der Organisation**   | Der Name Ihrer Organisation                 | Dieser Name wird an verschiedenen Stellen verwendet, u.a. im Titel des Entwicklerportals und im Absender von Benachrichtigungs-E-Mails.                                                                                                                                                                                                             |
| **Administrator-E-Mail** | *admin\@org.com*                               | Legen Sie die E-Mail-Adresse fest, an die alle Benachrichtigungen von **API Management** gesendet werden.                                                                                                                                                                                                                                              |
| **Preisstufe**        | *Developer*                                   | Legen Sie den Tarif **Entwickler** fest, um den Dienst zu bewerten. Dieser Tarif ist nicht für die Produktion bestimmt. Weitere Informationen zum Skalieren der API Management-Tarife finden Sie unter [Upgrade and scale an API Management instance](upgrade-and-scale.md) (Upgraden und Skalieren einer API Management-Instanz).                                                                                                                                    |

3. Wählen Sie **Erstellen**.

    > [!TIP]
    > Es dauert in der Regel zwischen 20 und 30 Minuten, einen API Management-Dienst zu erstellen. Durch Auswählen von **An Dashboard anheften** ist ein neu erstellter Dienst einfacher zu finden.

[!INCLUDE [api-management-navigate-to-instance](../../includes/api-management-navigate-to-instance.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn die Ressourcengruppe und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie mit den folgenden Schritten entfernen:

1. Suchen Sie im Azure-Portal nach **Ressourcengruppen**, und wählen Sie die entsprechende Option aus. Die Option **Ressourcengruppen** kann auch auf der **Startseite** von Azure ausgewählt werden. 

   ![Ressourcengruppennavigation](./media/get-started-create-service-instance/00-DeleteResource-01.png)

1. Wählen Sie auf der Seite **Ressourcengruppen** Ihre Ressourcengruppe aus.

   ![Ressourcengruppennavigation](./media/get-started-create-service-instance/00-DeleteResource-02.png)

1. Wählen Sie auf der Ressourcengruppenseite die Option **Ressourcengruppe löschen** aus. 
   
1. Geben Sie den Namen Ihrer Ressourcengruppe ein, und wählen Sie **Löschen** aus.

   ![Ressourcengruppe löschen](./media/get-started-create-service-instance/00-DeleteResource-03.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Importieren und Veröffentlichen Ihrer ersten API](import-and-publish.md)

---
title: Verschieben von Azure App Service-Ressourcen
description: Verwenden Sie Azure Resource Manager, um App Service-Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement zu verschieben.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: tomfitz
ms.openlocfilehash: 04ddf644c58434531dde708ee3b6432b1fce8f91
ms.sourcegitcommit: 6eecb9a71f8d69851bc962e2751971fccf29557f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72533529"
---
# <a name="move-guidance-for-app-service-resources"></a>Anleitung zum Verschieben von App Service-Ressourcen

Die Schritte zum Verschieben von App Service-Ressourcen variieren abhängig davon, ob Sie die Ressourcen innerhalb eines Abonnements oder in ein neues Abonnement verschieben.

## <a name="move-in-same-subscription"></a>Verschieben im gleichen Abonnement

Beim Verschieben einer Web-App _innerhalb desselben Abonnements_ können SSL-Zertifikate von Drittanbietern nicht verschoben werden. Sie können eine Web-App jedoch in die neue Ressourcengruppe verschieben, ohne das zugehörige SSL-Zertifikat eines Drittanbieters zu verschieben. Die SSL-Funktionalität Ihrer App wird in diesem Fall nicht beeinträchtigt.

Wenn Sie das SSL-Zertifikat mit der Web-App verschieben möchten, gehen Sie folgendermaßen vor:

1. Löschen Sie das Zertifikat des Drittanbieters aus der Web-App, bewahren Sie aber eine Kopie des Zertifikats auf.
2. Verschieben Sie die Web-App.
3. Laden Sie das Zertifikat des Drittanbieters in die verschobene Web-App hoch.

## <a name="move-across-subscriptions"></a>Verschieben zwischen Abonnements

Beim Verschieben einer Web-App _zwischen Abonnements_ gelten die folgenden Einschränkungen:

- Die Zielressourcengruppe darf keine App Service-Ressourcen besitzen. Zu App Service-Ressourcen zählen:
    - Web-Apps
    - App Service-Pläne
    - Hochgeladene oder importierte SSL-Zertifikate
    - App Service-Umgebungen
- Alle App Service-Ressourcen in der Ressourcengruppe müssen zusammen verschoben werden.
- App Service-Ressourcen können nur aus der Ressourcengruppe verschoben werden, in der sie ursprünglich erstellt wurden. Wenn eine App Service-Ressource nicht mehr in ihrer ursprünglichen Ressourcengruppe enthalten ist, verschieben Sie sie zurück zu ihrer ursprünglichen Ressourcengruppe. Verschieben Sie dann die Ressource zwischen Abonnements.

Wenn Sie sich nicht an die ursprüngliche Ressourcengruppe erinnern, können Sie diese mithilfe der Diagnosefunktion finden. Wählen Sie für Ihre Web-App **Diagnose und Problembehandlung** aus. Wählen Sie dann **Konfiguration und Verwaltung** aus.

![„Diagnose“ auswählen](./media/app-service-move-limitations/select-diagnostics.png)

Wählen Sie **Migrationsoptionen** aus.

![Migrationsoptionen auswählen](./media/app-service-move-limitations/select-migration.png)

Wählen Sie die Option für empfohlene Schritte, um die Web-App zu verschieben.

![„Empfohlene Schritte“ auswählen](./media/app-service-move-limitations/recommended-steps.png)

Sie sehen die empfohlenen Maßnahmen, die Sie ergreifen sollten, bevor Sie die Ressourcen verschieben. Die Informationen enthalten die ursprüngliche Ressourcengruppe für die Web-App.

![Empfehlungen](./media/app-service-move-limitations/recommendations.png)

## <a name="move-app-service-certificate"></a>Verschieben des App Service-Zertifikats

Sie können Ihr App Service-Zertifikat in eine neue Ressourcengruppe oder ein neues Abonnement verschieben. Wenn Ihr App Service-Zertifikat an eine Web-App gebunden ist, müssen Sie einige Schritte ausführen, bevor Sie die Ressourcen in ein neues Abonnement verschieben. Löschen Sie die SSL-Bindung und das private Zertifikat vor dem Verschieben der Ressourcen aus der Web-App. Das App Service-Zertifikat muss nicht gelöscht werden, nur das private Zertifikat in der Web-App.

## <a name="move-support"></a>Unterstützung für das Verschieben

Um zu bestimmen, welche App Service-Ressourcen verschoben werden können, sehen Sie sich den Supportstatus für Folgendes an:

- [Microsoft.AppService](../move-support-resources.md#microsoftappservice)
- [Microsoft.CertificateRegistration](../move-support-resources.md#microsoftcertificateregistration)
- [Microsoft.DomainRegistration](../move-support-resources.md#microsoftdomainregistration)
- [Microsoft.Web](../move-support-resources.md#microsoftweb)

## <a name="next-steps"></a>Nächste Schritte

Befehle zum Verschieben von Ressourcen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../resource-group-move-resources.md).

---
title: Verschieben von Logik-Apps zwischen Abonnements, Ressourcengruppen oder Regionen – Azure Logic Apps
description: Migrieren von Logik-Apps oder Integrationskonten zu anderen Azure-Abonnements, -Ressourcengruppen oder -Standorten (Regionen)
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 07/31/2019
ms.openlocfilehash: 8c3bad32943b83cbfe4c96087f3fef1c51f64bb1
ms.sourcegitcommit: d37991ce965b3ee3c4c7f685871f8bae5b56adfa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72679106"
---
# <a name="move-logic-app-resources-to-other-azure-subscriptions-resource-groups-or-regions"></a>Verschieben von Logik-App-Ressourcen zu anderen Azure-Abonnements, -Ressourcengruppen oder -Regionen

Wenn Sie Ihre Logik-App oder verwandte Ressourcen in ein anderes Azure-Abonnement, eine andere Ressourcengruppe oder Region verschieben möchten, haben Sie verschiedene Möglichkeiten, diese Aufgaben auszuführen, z. B. das Azure-Portal, Azure PowerShell, Azure CLI und die REST-API. Überprüfen Sie vor dem Verschieben von Ressourcen die folgenden Punkte: 

* Sie können nur [bestimmte Logik-App-Ressourcentypen](../azure-resource-manager/move-support-resources.md#microsoftlogic) zwischen Azure-Ressourcengruppen oder -Abonnements verschieben.

* Überprüfen Sie die [Limits](../logic-apps/logic-apps-limits-and-config.md) für die Anzahl von Logik-App-Ressourcen, die Sie in Ihrem Azure-Abonnement und in jeder Azure-Region haben können. Diese Limits beeinflussen, ob Sie bestimmte Ressourcentypen verschieben können, wenn die Region in Abonnements oder Ressourcengruppen gleich bleibt. Beispielsweise können Sie nur ein Integrationskonto im Free-Tarif für jede Azure-Region in jedem Azure-Abonnement haben.

* Wenn Sie Ressourcen verschieben, erstellt Azure neue Ressourcen-IDs. Stellen Sie daher sicher, dass Sie die neuen IDs verwenden, und aktualisieren Sie alle Skripts und Tools, die den verschobenen Ressourcen zugeordnet sind. Nachdem Sie Logik-Apps zwischen Abonnements, Ressourcengruppen oder Regionen verschoben haben, müssen Sie alle OAuth-basierten Verbindungen neu erstellen oder neu autorisieren.

## <a name="prerequisites"></a>Voraussetzungen

* Dasselbe Azure-Abonnement, das zum Erstellen der Logik-App oder des Integrationskontos verwendet wurde, das Sie verschieben möchten.

* Ressourcenbesitzerberechtigungen zum Verschieben und Einrichten der gewünschten Ressourcen. Weitere Informationen zur [rollenbasierten Zugriffssteuerung (RBAC)](../role-based-access-control/built-in-roles.md#owner).

<a name="move-subscription"></a>

## <a name="move-resources-between-subscriptions"></a>Verschieben von Ressourcen zwischen Abonnements

Um eine Ressource, beispielsweise eine Logik-App oder ein Integrationskonto, in ein anderes Azure-Abonnement zu verschieben, können Sie das Azure-Portal, Azure PowerShell, Azure CLI oder die REST-API verwenden. Diese Schritte behandeln das Azure-Portal, das Sie verwenden können, wenn die Region der Ressource unverändert bleibt. Informationen zu anderen Schritten und zur allgemeinen Vorbereitung finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/resource-group-move-resources.md).

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach der Logik-App-Ressource, die Sie verschieben möchten, und wählen Sie sie aus.

1. Wählen Sie auf der Seite **Übersicht** neben **Abonnement** den Link **Ändern** aus.

1. Wählen Sie auf der Seite **Ressourcen verschieben** die Logik-App-Ressource und alle zugehörigen Ressourcen aus, die Sie verschieben möchten.

1. Wählen Sie aus der Liste **Abonnement** das Zielabonnement aus.

1. Wählen Sie aus der Liste **Ressourcengruppe** die Zielressourcengruppe aus. Oder wählen Sie **Neue Gruppe erstellen** aus, um eine andere Ressourcengruppe zu erstellen.

1. Um zu bestätigen, dass Ihnen bekannt ist, dass alle Skripts oder Tools, die den verschobenen Ressourcen zugeordnet sind, erst dann wieder funktionieren, wenn Sie sie mit den neuen Ressourcen-IDs aktualisieren, aktivieren Sie das Kontrollkästchen für die Bestätigung, und klicken Sie dann auf **OK**.

<a name="move-resource-group"></a>

## <a name="move-resources-between-resource-groups"></a>Verschieben von Ressourcen zwischen Ressourcengruppen

Um eine Ressource, beispielsweise eine Logik-App oder ein Integrationskonto, in eine andere Azure-Ressourcengruppe zu verschieben, können Sie das Azure-Portal, Azure PowerShell, Azure CLI oder die REST-API verwenden. Diese Schritte behandeln das Azure-Portal, das Sie verwenden können, wenn die Region der Ressource unverändert bleibt. Informationen zu anderen Schritten und zur allgemeinen Vorbereitung finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/resource-group-move-resources.md).

Bevor Sie Ressourcen tatsächlich zwischen Gruppen verschieben, können Sie testen, ob Sie Ihre Ressource erfolgreich in eine andere Gruppe verschieben können. Weitere Informationen finden Sie unter [Überprüfen Ihrer Verschiebung](../azure-resource-manager/resource-group-move-resources.md#validate-move).

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach der Logik-App-Ressource, die Sie verschieben möchten, und wählen Sie sie aus.

1. Wählen Sie auf der Seite **Übersicht** neben **Ressourcengruppe** den Link **Ändern** aus.

1. Wählen Sie auf der Seite **Ressourcen verschieben** die Logik-App-Ressource und alle zugehörigen Ressourcen aus, die Sie verschieben möchten.

1. Wählen Sie aus der Liste **Ressourcengruppe** die Zielressourcengruppe aus. Oder wählen Sie **Neue Gruppe erstellen** aus, um eine andere Ressourcengruppe zu erstellen.

1. Um zu bestätigen, dass Ihnen bekannt ist, dass alle Skripts oder Tools, die den verschobenen Ressourcen zugeordnet sind, erst dann wieder funktionieren, wenn Sie sie mit den neuen Ressourcen-IDs aktualisieren, aktivieren Sie das Kontrollkästchen für die Bestätigung, und klicken Sie dann auf **OK**.

<a name="move-location"></a>

## <a name="move-resources-between-regions"></a>Verschieben von Ressourcen zwischen Regionen

Wenn Sie eine Logik-App in eine andere Region verschieben möchten, sind Ihre Optionen von der Art und Weise abhängig, wie Sie Ihre Logik-App erstellt haben. Basierend auf der von Ihnen gewählten Option müssen Sie die Verbindungen in ihrer Logik-App neu erstellen oder neu autorisieren.

* Erstellen Sie die Logik-App im Azure-Portal in der neuen Region neu, und konfigurieren Sie die Workfloweinstellungen neu. Um Zeit zu sparen, können Sie die zugrunde liegende Workflowdefinition und deren Verbindungen aus der Quell-App in die Ziel-App kopieren. Um den „Code“ hinter einer Logik-App anzuzeigen, wählen Sie auf der Symbolleiste des Logik-App-Designers die **Codeansicht** aus.

* Mithilfe von Visual Studio und den Azure Logic Apps-Tools für Visual Studio können Sie [Ihre Logik-App](../logic-apps/manage-logic-apps-with-visual-studio.md) aus dem Azure-Portal als [Azure Resource Manager-Vorlage](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md) öffnen und herunterladen. Diese Vorlage ist größtenteils bereit für die Bereitstellung und umfasst die Ressourcendefinitionen für Ihre Logik-App, einschließlich des Workflows selbst und der Verbindungen. Die Vorlage deklariert außerdem Parameter für die bei der Bereitstellung zu verwendenden Werte. Auf diese Weise können Sie einfacher ändern, wo und wie Sie die Logik-App bereitstellen, basierend auf Ihren Anforderungen. Um den Standort und andere erforderliche Informationen für die Bereitstellung anzugeben, können Sie eine gesonderte Parameterdatei verwenden.

* Wenn Sie Ihre Logik-App mit CI/CD-Tools (Continuous Integration/Continuous Delivery) wie Azure Pipelines in Azure DevOps erstellt und bereitgestellt haben, können Sie Ihre App mithilfe dieser Tools in einer anderen Region bereitstellen.

Weitere Informationen zu Bereitstellungsvorlagen für Logik-Apps finden Sie in den folgenden Themen:

* [Übersicht: Automatisieren der Bereitstellung für Azure Logic Apps durch Verwenden von Azure Resource Manager-Vorlagen](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)
* [Suchen, Öffnen und Herunterladen Ihrer Logik-App aus dem Azure-Portal in Visual Studio](../logic-apps/manage-logic-apps-with-visual-studio.md)
* [Erstellen von Azure Resource Manager-Vorlagen für Azure Logic Apps](../logic-apps/logic-apps-create-azure-resource-manager-templates.md)
* [Bereitstellen von Azure Resource Manager-Vorlagen für Azure Logic Apps](../logic-apps/logic-apps-deploy-azure-resource-manager-templates.md)

### <a name="related-resources"></a>Zugehörige Ressourcen

Einige Azure-Ressourcen wie lokale Datengateway-Ressourcen in Azure können in einer Region vorhanden sein, die sich von den Logik-Apps unterscheidet, die diese Ressourcen verwenden. Andere Azure-Ressourcen, wie verknüpfte Integrationskonten, müssen jedoch in derselben Region wie Ihre Logik-Apps vorhanden sein. Stellen Sie basierend auf Ihrem Szenario sicher, dass Ihre Logik-Apps auf die Ressourcen zugreifen können, deren Vorhandensein Ihre Apps in derselben Region erwarten.

Um beispielsweise eine Logik-App mit einem Integrationskonto zu verknüpfen, müssen sich beide Ressourcen in derselben Region befinden. In Szenarien wie einer Notfallwiederherstellung benötigen Sie in der Regel Integrationskonten, die über dieselbe Konfiguration und dieselben Artefakte verfügen. In anderen Szenarien benötigen Sie möglicherweise Integrationskonten mit unterschiedlichen Konfigurationen und Artefakten.

Benutzerdefinierte Connectors in Azure Logic Apps sind für die Ersteller und Benutzer der Connectors sichtbar, die dasselbe Azure-Abonnement und denselben Azure Active Directory-Mandanten haben. Diese Connectors sind in derselben Region verfügbar, in der Logik-Apps bereitgestellt werden. Weitere Informationen finden Sie unter [Freigeben eines benutzerdefinierten Connectors in Ihrer Organisation](https://docs.microsoft.com/connectors/custom-connectors/share).

Die Vorlage, die Sie von Visual Studio erhalten, umfasst nur die Ressourcendefinitionen für Ihre Logik-App und deren Verbindungen. Wenn Ihre Logik-App also andere Ressourcen verwendet, z. B. ein Integrationskonto und B2B-Artefakte, beispielsweise Partner, Vereinbarungen und Schemas, müssen Sie die Vorlage dieses Integrationskontos mithilfe des Azure-Portals exportieren. Diese Vorlage umfasst die Ressourcendefinitionen für sowohl das Integrationskonto als auch die Artefakte. Die Vorlage ist allerdings nicht vollständig parametrisiert. Daher müssen Sie die Werte, die Sie für die Bereitstellung verwenden möchten, manuell parametrisieren.

### <a name="export-templates-for-integration-accounts"></a>Exportieren von Vorlagen für Integrationskonten

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach Ihrem Integrationskonto, und öffnen Sie es.

1. Wählen Sie im Menü Ihres Integrationskontos unter **Einstellungen** den Eintrag **Vorlage exportieren** aus.

1. Wählen Sie auf der Symbolleiste **Herunterladen** aus, und speichern Sie die Vorlage.

1. Öffnen und bearbeiten Sie die Vorlage, um die notwendigen Werte für die Bereitstellung zu parametrisieren.

## <a name="next-steps"></a>Nächste Schritte

[Verschieben von Azure-Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/resource-group-move-resources.md)
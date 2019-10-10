---
title: Hinzufügen von Artefakten zu Integrationsdienstumgebungen (Integration Service Environments, ISEs) in Azure Logic Apps
description: Fügen Sie Logik-Apps, benutzerdefinierte Connectors und Integrationskonten zu Ihrer Integrationsdienstumgebung (Integration Service Environment, ISE) hinzu, um den Zugriff auf virtuelle Azure-Netzwerke (VNETs) zu ermöglichen und dabei trotzdem privat und vom öffentlichen oder globalen Azure-Dienst isoliert zu bleiben.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: 6327f0c14b46ceaadbf7adaa58a70c32b39b7c2a
ms.sourcegitcommit: 4d177e6d273bba8af03a00e8bb9fe51a447196d0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2019
ms.locfileid: "71960490"
---
# <a name="add-artifacts-to-your-integration-service-environment-ise-in-azure-logic-apps"></a>Hinzufügen von Artefakten zu Ihrer Integrationsdienstumgebung (Integration Service Environment, ISE) in Azure Logic Apps

Fügen Sie nach der Erstellung einer [Integrationsdienstumgebung (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) Artefakte wie Logik-Apps, Integrationskonten und Connectors hinzu, damit diese auf die Ressourcen in Ihrem virtuellen Azure-Netzwerk zugreifen können.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Die ISE, die Sie für die Ausführung Ihrer Logik-Apps erstellt haben. Besitzen Sie keine ISE, [erstellen Sie zuerst eine](../logic-apps/connect-virtual-network-vnet-isolated-environment.md).

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-apps"></a>Erstellen von Logik-Apps

Führen Sie die folgenden Schritte aus, um Logik-Apps zu erstellen, die in Ihrer Integrationsdienstumgebung (ISE) ausgeführt werden:

1. Suchen Sie nach Ihrer ISE, und öffnen Sie sie, falls dies erforderlich ist. Wählen Sie im ISE-Menü unter **Einstellungen** die Optionen **Logik-Apps** > **Hinzufügen** aus.

   ![Hinzufügen einer neuen Logik-App zur ISE](./media/add-artifacts-integration-service-environment-ise/add-logic-app-to-ise.png)

   Oder

   Klicken Sie im Hauptmenü von Azure auf **Ressource erstellen** > **Integration** > **Logik-App**.

1. Geben Sie den Namen, das Azure-Abonnement und die Azure-Ressourcengruppe (neu oder vorhanden) an, die Sie für Ihre Logik-App verwenden möchten.

1. Wählen Sie in der Liste **Speicherort** im Abschnitt **Integrationsdienstumgebungen** Ihre ISE aus, z. B.:

   ![Auswählen der Integrationsdienstumgebung](./media/add-artifacts-integration-service-environment-ise/create-logic-app-with-integration-service-environment.png)

   > [!IMPORTANT]
   > Falls Sie Ihre Logik-Apps mit einem Integrationskonto verwenden möchten, muss für diese Logik-Apps und das Integrationskonto dieselbe ISE verwendet werden.

1. Fahren Sie wie gewohnt mit der [Erstellung Ihrer Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md) fort.

   Informationen zu den Unterschieden, wie Trigger und Aktionen funktionieren und wie sie bezeichnet werden, wenn Sie eine Integrationsdienstumgebung verwenden, im Vergleich zum globalen Logic Apps-Dienst, finden Sie unter [„Isoliert“ gegenüber „Global“ in der Übersicht zur Integrationsdienstumgebung](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#difference).

1. Informationen zum Verwalten von Logik-Apps und API-Verbindungen in Ihrer ISE finden Sie unter [Manage your integration service environment (ISE) in Azure Logic Apps](../logic-apps/ise-manage-integration-service-environment.md) (Verwalten Ihrer Integrationsdienstumgebung (Integration Service Environment, ISE) in Azure Logic Apps).

<a name="create-integration-account-environment"></a>

## <a name="create-integration-accounts"></a>Erstellen von Integrationskonten

Basierend auf der während der Erstellung gewählten [ISE-SKU](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level) umfasst Ihre ISE die Nutzung des jeweiligen Integrationskontos ohne weitere Kosten. Logik-Apps, die in einer Integrationsdienstumgebung (ISE) enthalten sind, können nur auf Integrationskonten verweisen, die sich in derselben ISE befinden. Damit ein Integrationskonto mit Logik-Apps in einer ISE funktioniert, muss sowohl für das Integrationskonto als auch für die Logik-Apps *dieselbe Umgebung* als Speicherort verwendet werden. Weitere Informationen zu Integrationskonten und ISEs finden Sie unter [Integrationskonten mit ISE](connect-virtual-network-vnet-isolated-environment-overview.md#create-integration-account-environment).

Führen Sie die folgenden Schritte aus, um ein Integrationskonto zu erstellen, das eine ISE verwendet:

1. Suchen Sie nach Ihrer ISE, und öffnen Sie sie, falls dies erforderlich ist. Wählen Sie im ISE-Menü unter **Einstellungen** die Optionen **Integrationskonten** > **Hinzufügen** aus.

   ![Hinzufügen eines neuen Integrationskontos zur ISE](./media/add-artifacts-integration-service-environment-ise/add-integration-account-to-ise.png)

   Oder

   Wählen Sie im Hauptmenü von Azure die Optionen **Ressource erstellen** > **Integration** > **Integrationskonto** aus.

1. Geben Sie den Namen, das Azure-Abonnement, die Azure-Ressourcengruppe (neu oder vorhanden) und den Tarif für Ihr Integrationskonto an.

1. Wählen Sie in der Liste **Speicherort** im Abschnitt **Integrationsdienstumgebungen** dieselbe ISE aus, die von Ihren Logik-Apps verwendet wird, z. B.:

   ![Auswählen der Integrationsdienstumgebung](./media/add-artifacts-integration-service-environment-ise/create-integration-account-with-integration-service-environment.png)

1. [Verknüpfen Sie Ihre Logik-App wie gewohnt mit Ihrem Integrationskonto.](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md#link-account)

1. Fügen Sie Ihrem Integrationskonto anschließend Artefakte hinzu, etwa [Parteien](../logic-apps/logic-apps-enterprise-integration-partners.md) und [Vereinbarungen](../logic-apps/logic-apps-enterprise-integration-agreements.md).

1. Informationen zum Verwalten von Integrationskonten in Ihrer ISE finden Sie unter [Manage your integration service environment (ISE) in Azure Logic Apps](../logic-apps/ise-manage-integration-service-environment.md) (Verwalten Ihrer Integrationsdienstumgebung (Integration Service Environment, ISE) in Azure Logic Apps.)

<a name="add-ise-connectors-environment"></a>

## <a name="add-ise-connectors"></a>Hinzufügen von ISE-Connectors

Sie können von Microsoft verwaltete Connectors hinzufügen, die für die Verwendung in Ihrer ISE verfügbar sind, dort aber nicht bereitgestellt werden.

1. Wählen Sie im ISE-Menü unter **Einstellungen** die Option **Verwaltete Connectors** aus. Wählen Sie auf der Symbolleiste **Hinzufügen** aus.

   ![Anzeigen verwalteter Connectors](./media/add-artifacts-integration-service-environment-ise/ise-view-managed-connectors.png)

1. Öffnen Sie im Bereich **Neuen verwalteten Connector hinzufügen** die Liste **Connector suchen**. Wenn der gewünschte Connector verfügbar ist, wählen Sie diesen Connector aus, und wählen Sie dann **Erstellen** aus.

   In der Liste werden nur die Connectors angezeigt, die berechtigt sind, aber nicht in ihrer ISE bereitgestellt werden. Connectors, die bereits in der ISE bereitgestellt wurden, sind für die Auswahl nicht verfügbar.

   ![Auswählen eines berechtigten Connectors](./media/add-artifacts-integration-service-environment-ise/add-managed-connector.png)

<a name="create-custom-connectors-environment"></a>

## <a name="create-custom-connectors"></a>Erstellen benutzerdefinierter Connectors

Wenn Sie benutzerdefinierte Connectors in ihrer ISE verwenden möchten, erstellen Sie diese direkt in der ISE.

1. Suchen Sie nach Ihrer ISE, und öffnen Sie sie, falls dies erforderlich ist. Wählen Sie im ISE-Menü unter **Einstellungen** die Optionen **Benutzerdefinierte Connectors** > **Hinzufügen** aus.

   ![Erstellen eines benutzerdefinierten Connectors](./media/add-artifacts-integration-service-environment-ise/add-custom-connector-to-ise.png)

1. Geben Sie den Namen, das Azure-Abonnement und die Azure-Ressourcengruppe (neu oder vorhanden) an, die Sie für den benutzerdefinierten Connector verwenden möchten.

1. Wählen Sie in der Liste **Speicherort** im Abschnitt **Integrationsdienstumgebungen** dieselbe ISE aus, die von Ihren Logik-Apps verwendet wird, und wählen Sie **Erstellen** aus. Beispiel:

   ![Auswählen der Integrationsdienstumgebung](./media/add-artifacts-integration-service-environment-ise/create-custom-connector-with-integration-service-environment.png)

1. Wählen Sie den neuen benutzerdefinierten Connector und anschließend **Bearbeiten** aus. Beispiel:

   ![Auswählen und Bearbeiten eines benutzerdefinierten Connectors](./media/add-artifacts-integration-service-environment-ise/edit-custom-connectors.png)

1. Erstellen Sie anschließend den Connector wie gewohnt auf der Grundlage einer [OpenAPI-Definition](https://docs.microsoft.com/connectors/custom-connectors/define-openapi-definition#import-the-openapi-definition) oder eines [SOAP](https://docs.microsoft.com/connectors/custom-connectors/create-register-logic-apps-soap-connector#2-define-your-connector).

1. Informationen zum Verwalten von benutzerdefinierten Connectors in Ihrer ISE finden Sie unter [Manage your integration service environment (ISE) in Azure Logic Apps](../logic-apps/ise-manage-integration-service-environment.md) (Verwalten Ihrer Integrationsdienstumgebung (Integration Service Environment, ISE) in Azure Logic Apps.)

## <a name="next-steps"></a>Nächste Schritte

* [Verwalten von Integrationsdienstumgebungen](../logic-apps/ise-manage-integration-service-environment.md)

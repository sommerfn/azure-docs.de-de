---
title: Verwalten der Integrationsdienstumgebungen in Azure Logic Apps
description: Überprüfen der Netzwerkintegrität und Verwalten von Logik-Apps, Verbindungen, benutzerdefinierten Connectors und Integrationskonten in Ihrer Integrationsdienstumgebung (Integration Service Environment, ISE) für Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: 6a6b096911ac8596fe29aeb4596f1da6d5266794
ms.sourcegitcommit: fbea2708aab06c19524583f7fbdf35e73274f657
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2019
ms.locfileid: "70967804"
---
# <a name="manage-your-integration-service-environment-ise-in-azure-logic-apps"></a>Verwalten Ihrer Integrationsdienstumgebung (Integration Service Environment, ISE) in Azure Logic Apps

Führen Sie die Schritte in diesem Thema aus, um die Netzwerkintegrität Ihrer [Integrationsdienstumgebung (Integration Service Environment, ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) zu überprüfen und die Logik-Apps, Verbindungen, Integrationskonten und Connectors in Ihrer ISE zu verwalten. Um diese Artefakte Ihrer ISE hinzuzufügen, lesen Sie [Hinzufügen von Artefakten zu Ihrer Integrationsdienstumgebung](../logic-apps/add-artifacts-integration-service-environment-ise.md).

## <a name="view-your-ise"></a>Anzeigen Ihrer Integrationsdienstumgebung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Geben Sie im Suchfeld des Portals „Integrationsdienstumgebungen“ ein, und wählen Sie dann **Integrationsdienstumgebungen** aus.

   ![Suchen von Integrationsdienstumgebungen](./media/ise-manage-integration-service-environment/find-integration-service-environment.png)

1. Wählen Sie in der Ergebnisliste Ihre Integrationsdienstumgebung aus.

   ![Auswählen der Integrationsdienstumgebung](./media/ise-manage-integration-service-environment/select-integration-service-environment.png)

1. Fahren Sie mit den nächsten Abschnitten fort, um Logik-Apps, Verbindungen, Connectors oder Integrationskonten in Ihrer ISE zu finden.

<a name="check-network-health"></a>

## <a name="check-network-health"></a>Überprüfen der Netzwerkintegrität

Wählen Sie im ISE-Menü unter **Einstellungen** die Option **Netzwerkintegrität** aus. In diesem Bereich wird der Integritätsstatus für Ihre Subnetze und ausgehenden Abhängigkeiten von anderen Diensten angezeigt.

![Überprüfen der Netzwerkintegrität](./media/ise-manage-integration-service-environment/ise-check-network-health.png)

<a name="find-logic-apps"></a>

## <a name="manage-your-logic-apps"></a>Verwalten Ihrer Logik-Apps

Sie können die Logik-Apps anzeigen und verwalten, die in ihrer ISE vorhanden sind.

1. Wählen Sie im ISE-Menü unter **Einstellungen** die Option **Logik-Apps** aus.

   ![Anzeigen von Logik-Apps](./media/ise-manage-integration-service-environment/ise-find-logic-apps.png)

1. Wenn Sie Logik-Apps entfernen möchten, die Sie nicht mehr in Ihrer ISE benötigen, markieren Sie diese Logik-Apps und wählen dann **Löschen** aus. Um den Löschvorgang zu bestätigen, wählen Sie **Ja** aus.

<a name="find-api-connections"></a>

## <a name="manage-api-connections"></a>Verwalten von API-Verbindungen

Sie können die Verbindungen anzeigen und verwalten, die von den Logik-Apps erstellt wurden, die in ihrer ISE ausgeführt werden.

1. Wählen Sie im ISE-Menü unter **Einstellungen** die Option **API-Verbindungen** aus.

   ![Anzeigen von API-Verbindungen](./media/ise-manage-integration-service-environment/ise-find-api-connections.png)

1. Um Verbindungen zu entfernen, die Sie in Ihrer ISE nicht mehr benötigen, markieren Sie diese Verbindungen und wählen dann **Löschen** aus. Um den Löschvorgang zu bestätigen, wählen Sie **Ja** aus.

<a name="manage-api-connectors"></a>

## <a name="manage-ise-connectors"></a>Verwalten der ISE-Connectors

Sie können die API-Connectors anzeigen und verwalten, die für Ihre ISE bereitgestellt werden.

1. Wählen Sie im ISE-Menü unter **Einstellungen** die Option **Verwaltete Connectors** aus.

   ![Anzeigen verwalteter Connectors](./media/ise-manage-integration-service-environment/ise-view-managed-connectors.png)

1. Um Connectors zu entfernen, die Sie in Ihrer ISE nicht mehr zur Verfügung stellen möchten, markieren Sie diese Connectors und wählen dann **Löschen** aus. Um den Löschvorgang zu bestätigen, wählen Sie **Ja** aus.

<a name="find-custom-connectors"></a>

## <a name="manage-custom-connectors"></a>Verwalten benutzerdefinierter Connectors

Sie können die benutzerdefinierten Connectors anzeigen und verwalten, die Sie für Ihre ISE bereitgestellt haben.

1. Wählen Sie im ISE-Menü unter **Einstellungen** die Option **Benutzerdefinierte Connectors** aus.

   ![Navigieren zu „Benutzerdefinierte Connectors“](./media/ise-manage-integration-service-environment/ise-find-custom-connectors.png)

1. Um benutzerdefinierte Connectors zu entfernen, die Sie in Ihrer ISE nicht mehr benötigen, markieren Sie diese Connectors und wählen dann **Löschen** aus. Um den Löschvorgang zu bestätigen, wählen Sie **Ja** aus.

<a name="find-integration-accounts"></a>

## <a name="manage-integration-accounts"></a>Verwalten von Integrationskonten

1. Wählen Sie im ISE-Menü unter **Einstellungen** die Option **Integrationskonten** aus.

   ![Suchen von Integrationskonten](./media/ise-manage-integration-service-environment/ise-find-integration-accounts.png)

1. Um nicht mehr benötigte Integrationskonten aus Ihrer ISE zu entfernen, wählen Sie diese Integrationskonten und dann **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie [eine Verbindung mit virtuellen Azure-Netzwerken über isolierte Logik-Apps herstellen können](../logic-apps/connect-virtual-network-vnet-isolated-environment.md).

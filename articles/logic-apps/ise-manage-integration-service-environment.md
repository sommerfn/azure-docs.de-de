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
ms.date: 07/26/2019
ms.openlocfilehash: 8f10e3d3fd7c67d1e803e8f85c9918c91bb81d59
ms.sourcegitcommit: f5cc71cbb9969c681a991aa4a39f1120571a6c2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68517359"
---
# <a name="manage-your-integration-service-environment-ise-in-azure-logic-apps"></a>Verwalten Ihrer Integrationsdienstumgebung (Integration Service Environment, ISE) in Azure Logic Apps

Führen Sie die Schritte in diesem Thema aus, um die Netzwerkintegrität Ihrer [Integrationsdienstumgebung](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) (Integration Service Environment, ISE) zu überprüfen und die Logik-Apps, Verbindungen, Integrationskonten und benutzerdefinierten Connectors in Ihrer ISE zu verwalten.

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

1. Wählen Sie im ISE-Menü unter **Einstellungen** die Option **Logik-Apps** aus.

   ![Suchen nach Logik-Apps](./media/ise-manage-integration-service-environment/ise-find-logic-apps.png)

1. Wenn Sie nicht mehr benötigte Logik-Apps aus Ihrer ISE entfernen möchten, wählen Sie diese Logik-Apps und dann **Löschen** aus.

<a name="find-api-connections"></a>

## <a name="manage-api-connections"></a>Verwalten von API-Verbindungen

1. Um die API-Verbindungen anzuzeigen, die von in Ihrer ISE ausgeführten Logik-Apps erstellt wurden, wählen Sie im ISE-Menü unter **Einstellungen** die Option **API-Verbindungen** aus.

   ![Suchen nach API-Verbindungen](./media/ise-manage-integration-service-environment/ise-find-api-connections.png)

1. Um nicht mehr benötigte Verbindungen aus Ihrer ISE zu entfernen, wählen Sie diese Verbindungen und dann **Löschen** aus.

<a name="find-custom-connectors"></a>

## <a name="manage-custom-connectors"></a>Verwalten benutzerdefinierter Connectors

1. Wenn Sie benutzerdefinierte Connectors anzeigen möchten, die in Ihrer ISE erstellt wurden, wählen Sie im ISE-Menü unter **Einstellungen** die Option **Benutzerdefinierte Connectors** aus.

   ![Navigieren zu „Benutzerdefinierte Connectors“](./media/ise-manage-integration-service-environment/ise-find-custom-connectors.png)

1. Um nicht mehr benötigte Connectors aus Ihrer ISE zu entfernen, wählen Sie diese Connectors und dann **Löschen** aus.

<a name="find-integration-accounts"></a>

## <a name="manage-integration-accounts"></a>Verwalten von Integrationskonten

1. Wählen Sie im ISE-Menü unter **Einstellungen** die Option **Integrationskonten** aus.

   ![Suchen von Integrationskonten](./media/ise-manage-integration-service-environment/ise-find-integration-accounts.png)

1. Um nicht mehr benötigte Integrationskonten aus Ihrer ISE zu entfernen, wählen Sie diese Integrationskonten und dann **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie [eine Verbindung mit virtuellen Azure-Netzwerken über isolierte Logik-Apps herstellen können](../logic-apps/connect-virtual-network-vnet-isolated-environment.md).

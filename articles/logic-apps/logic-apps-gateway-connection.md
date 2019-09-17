---
title: Zugreifen auf lokale Datenquellen aus Azure Logic Apps
description: Herstellen einer Verbindung mit lokalen Datenquellen in Logik-Apps durch Erstellen eines lokalen Datengateways
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: arthii, LADocs
ms.topic: article
ms.date: 07/01/2019
ms.openlocfilehash: 65c1d427939dc39aebece24b923bc4ebfbf136bb
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2019
ms.locfileid: "70861030"
---
# <a name="connect-to-on-premises-data-sources-from-azure-logic-apps"></a>Herstellen einer Verbindung mit lokalen Datenquellen in Azure Logic Apps

Um in Ihren Logik-Apps auf lokale Datenquellen zuzugreifen, erstellen Sie eine lokale Datengatewayressource im Azure-Portal. Ihre Logik-Apps können dann die [lokalen Connectors](../logic-apps/logic-apps-gateway-install.md#supported-connections) verwenden. In diesem Artikel wird gezeigt, wie Sie Ihre Azure-Gatewayressource erstellen, *nachdem* Sie [das Gateway auf Ihren lokalen Computer heruntergeladen und installiert haben](../logic-apps/logic-apps-gateway-install.md). Weitere Informationen zur Funktionsweise des Gateways finden Sie unter [Funktionsweise des Gateways](../logic-apps/logic-apps-gateway-install.md#gateway-cloud-service).

> [!TIP]
> Erwägen Sie zum Herstellen einer Verbindung mit virtuellen Azure-Netzwerken stattdessen die Erstellung einer [*Integrationsdienstumgebung*](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) (Integration Service Environment, ISE). 

Informationen zum Verwenden des Gateways mit anderen Diensten finden Sie in den folgenden Artikeln:

* [Microsoft Power BI | Lokales Datengateway](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
* [Microsoft Flow | Lokales Datengateway](https://flow.microsoft.com/documentation/gateway-manage/)
* [Microsoft Power Apps lokales Datengateway](https://powerapps.microsoft.com/tutorials/gateway-management/)
* [Azure Analysis Services | Lokales Datengateway](../analysis-services/analysis-services-gateway.md)

## <a name="prerequisites"></a>Voraussetzungen

* Das [lokale Datengateway muss bereits auf einem lokalen Computer installiert sein](../logic-apps/logic-apps-gateway-install.md).

* Sie verfügen über [dasselbe Azure-Konto und Azure-Abonnement](../logic-apps/logic-apps-gateway-install.md#requirements), das Sie bei der Installation des lokalen Datengateways verwendet haben.

* Sie haben ihre Gatewayinstallation bisher nicht mit einer anderen Gatewayressource in Azure verknüpft.

  Wenn Sie eine Gatewayressource erstellen, wählen Sie eine Gatewayinstallation aus, die mit ihrer Gatewayressource verknüpft werden soll. Sie können keine bereits verknüpfte Gatewayinstallation auswählen, wenn Sie Gatewayressourcen erstellen.

<a name="create-gateway-resource"></a>

## <a name="create-azure-gateway-resource"></a>Erstellen einer Azure-Gatewayressource

Nachdem Sie das Gateway auf einem lokalen Computer installiert haben, erstellen Sie eine Azure-Ressource für Ihr Gateway. 

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) mit dem gleichen Azure-Konto an, das beim Installieren des Gateways verwendet wurde.

1. Geben Sie im Suchfeld des Azure-Portals den Begriff „lokales Datengateway“ ein, und wählen Sie dann **Lokale Datengateways** aus.

   ![Nach „Lokales Datengateway“ suchen](./media/logic-apps-gateway-connection/find-on-premises-data-gateway.png)

1. Wählen Sie unter **Lokale Datengateways** die Option **Hinzufügen** aus.

   ![Hinzufügen des Datengateways](./media/logic-apps-gateway-connection/add-gateway.png)

1. Geben Sie unter **Verbindungsgateway erstellen** die folgenden Informationen für Ihre Gatewayressource an. Wählen Sie **Erstellen**, wenn Sie fertig sind.

   | Eigenschaft | BESCHREIBUNG |
   |----------|-------------|
   | **Ressourcenname** | Ihr Gatewayressourcenname, der nur Buchstaben, Ziffern, Bindestriche (`-`), Unterstriche (`_`), Klammern (`(`, `)`) und Punkte (`.`) enthalten kann. |
   | **Abonnement** | Ihr Azure-Abonnement. Dieses muss mit der Gatewayinstallation und der Logik-App identisch sein. Das standardmäßige Abonnement basiert auf dem Azure-Konto, das Sie zum Anmelden verwendet haben. |
   | **Ressourcengruppe** | Die [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md), die Sie verwenden möchten. |
   | **Standort** | Die gleiche Region wie der Standort, der während der [Gatewayinstallation](../logic-apps/logic-apps-gateway-install.md) für den Gatewayclouddienst ausgewählt wurde. Andernfalls kann es sein, dass Ihre Gatewayinstallation nicht in der Liste **Installationsname** zur Auswahl angezeigt wird. Der Standort Ihrer Logik-App kann sich vom Standort Ihrer Gatewayressource unterscheiden. |
   | **Installationsname** | Wenn Ihre Gatewayinstallation noch nicht ausgewählt ist, wählen Sie das Gateway aus, das Sie zuvor installiert haben. Zuvor verknüpfte Gatewayinstallationen werden nicht in der Liste angezeigt und stehen nicht zur Auswahl zur Verfügung. |
   |||

   Beispiel:

   ![Angeben von Details zum Erstellen Ihres lokalen Datengateways](./media/logic-apps-gateway-connection/gateway-details.png)

<a name="connect-logic-app-gateway"></a>

## <a name="connect-to-on-premises-data"></a>Herstellen einer Verbindung mit lokalen Daten

Nachdem Sie die Gatewayressource erstellt und Ihr Azure-Abonnement mit dieser Ressource verknüpft haben, können Sie jetzt mit dem Gateway eine Verbindung zwischen Ihrer Logik-App und Ihrer lokalen Datenquelle herstellen.

1. Erstellen oder öffnen Sie im Azure-Portal Ihre Logik-App im Logik-App-Designer.

1. Fügen Sie einen Connector hinzu, der lokale Verbindungen unterstützt, z.B. **SQL Server**.

1. Wählen Sie **Verbinden über lokales Datengateway**. 

1. Wählen Sie für die Option **Gateways** die Gatewayressource aus, die Sie zuvor erstellt haben.

   > [!NOTE]
   > Die Liste der Gateways enthält Gatewayressourcen in anderen Regionen, da sich der Standort Ihrer Logik-App vom Standort Ihrer Gatewayressource unterscheiden kann.

1. Geben Sie einen eindeutigen Verbindungsnamen und andere erforderliche Informationen an, die von der zu erstellenden Verbindung abhängig sind.

   Ein eindeutiger Verbindungsname vereinfacht später das Identifizieren dieser Verbindung. Dies gilt insbesondere dann, wenn Sie mehrere Verbindungen erstellen. Sofern erforderlich, geben Sie auch die qualifizierte Domäne für Ihren Benutzernamen an.
   
   Beispiel:

   ![Erstellen der Verbindung zwischen der Logik-App und dem Datengateway](./media/logic-apps-gateway-connection/logic-app-gateway-connection.png)

1. Wählen Sie **Erstellen**, wenn Sie fertig sind. 

Die Gatewayverbindung kann nun für Ihre Logik-App verwendet werden.

## <a name="edit-connection"></a>Bearbeiten der Verbindung

Zum Aktualisieren der Einstellungen für eine Gatewayverbindung können Sie die Verbindung bearbeiten.

1. Um alle API-Verbindungen nur für Ihre Logik-App zu suchen, wählen Sie im Menü Ihrer Logik-App unter **Entwicklungstools** die Option **API-Verbindungen** aus.
   
   ![Wählen Sie im Menü Ihrer Logik-App „API-Verbindungen“ aus.](./media/logic-apps-gateway-connection/logic-app-find-api-connections.png)

1. Wählen Sie die gewünschte Gatewayverbindung und dann **API-Verbindung bearbeiten** aus.

   > [!TIP]
   > Sollten Ihre Aktualisierungen nicht wirksam werden, [beenden Sie das Gateway-Windows-Dienstkonto für Ihre Gatewayinstallation](../logic-apps/logic-apps-gateway-install.md#restart-gateway), und starten Sie es dann neu.

So suchen Sie alle API-Verbindungen, die mit Ihrem Azure-Abonnement verknüpft sind: 

* Wechseln Sie über das Azure-Hauptmenü zu **Alle Dienste** > **Web** > **API-Verbindungen**.
* Wechseln Sie alternativ über das Azure-Hauptmenü zu **Alle Ressourcen**. Legen Sie Filter **Typ** auf **API-Verbindung** fest.

<a name="change-delete-gateway-resource"></a>

## <a name="delete-gateway-resource"></a>Löschen der Gatewayressource

Um eine andere Gatewayressource zu erstellen, Ihre Gatewayinstallation mit einer anderen Gatewayressource zu verknüpfen oder die Gatewayressource zu entfernen, können Sie die Gatewayressource löschen, ohne dass sich dies auf die Gatewayinstallation auswirkt. 

1. Wählen Sie im Azure-Menü die Option **Alle Ressourcen** aus. Suchen Sie Ihre Gatewayressource, und wählen Sie sie aus.

1. Wenn nicht bereits ausgewählt, wählen Sie in Ihrem Gatewayressourcenmenü **Lokales Datengateway** aus. Wählen Sie auf der Gatewayressourcen-Symbolleiste die Option **Löschen** aus.

   Beispiel:

   ![Gateway löschen](./media/logic-apps-gateway-connection/gateway-delete.png)

<a name="faq"></a>

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Sichern Ihrer Logik-Apps](./logic-apps-securing-a-logic-app.md)
* [Allgemeine Beispiele und Szenarien für Logik-Apps](./logic-apps-examples-and-scenarios.md)

---
title: 'Zugreifen auf lokale Datenquellen: Azure Logic Apps'
description: Herstellen einer Verbindung mit lokalen Datenquellen in Azure Logik-Apps durch Erstellen einer lokalen Azure-Datengatewayressource
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: arthii, LADocs
ms.topic: article
ms.date: 11/06/2019
ms.openlocfilehash: e0354f96036cce968e6b6909a18f97ff48347eda
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73796250"
---
# <a name="connect-to-on-premises-data-sources-from-azure-logic-apps"></a>Herstellen einer Verbindung mit lokalen Datenquellen in Azure Logic Apps

Bevor Sie lokal aus Ihren Logik-Apps auf Datenquellen zugreifen können, müssen Sie eine Azure-Ressource erstellen, nachdem Sie das [*lokale Datengateway* auf einem lokalen Computer installiert haben](../logic-apps/logic-apps-gateway-install.md). Ihre Logik-Apps verwenden dann diese Azure-Gatewayressource in den Triggern und Aktionen, die von den [lokalen Connectors](../connectors/apis-list.md#on-premises-connectors) bereitgestellt werden, die für Azure Logic-Apps verfügbar sind.

In diesem Artikel wird gezeigt, wie Sie Ihre Azure-Gatewayressource für ein zuvor [installiertes Gateway auf Ihren lokalen Computer](../logic-apps/logic-apps-gateway-install.md) erstellen. Weitere Informationen zum Gateway finden Sie unter [So funktioniert das Gateway](../logic-apps/logic-apps-gateway-install.md#gateway-cloud-service).

> [!TIP]
> Erwägen Sie zum Herstellen einer Verbindung mit virtuellen Azure-Netzwerken stattdessen die Erstellung einer [*Integrationsdienstumgebung*](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) (Integration Service Environment, ISE). 

Informationen zum Verwenden des Gateways mit anderen Diensten finden Sie in den folgenden Artikeln:

* [Microsoft Power BI | Lokales Datengateway](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
* [Power Automate für das lokale Datengateway](https://flow.microsoft.com/documentation/gateway-manage/)
* [Lokales Microsoft Power Apps-Datengateway](https://powerapps.microsoft.com/tutorials/gateway-management/)
* [Azure Analysis Services | Lokales Datengateway](../analysis-services/analysis-services-gateway.md)

<a name="supported-connections"></a>

## <a name="supported-data-sources"></a>Unterstützte Datenquellen

In Azure Logic Apps unterstützt das lokale Datengateway [lokale Connectors](../connectors/apis-list.md#on-premises-connectors) für die folgenden Datenquellen:

* BizTalk Server 2016
* Dateisystem
* IBM DB2  
* IBM Informix
* IBM MQ
* MySQL
* Oracle-Datenbank
* PostgreSQL
* SAP
* SharePoint Server
* SQL Server
* Teradata

Azure Logic Apps unterstützt Lese- und Schreibvorgänge über das Datengateway. Allerdings besitzen diese Vorgänge [Limits hinsichtlich Ihrer Nutzlastgröße](https://docs.microsoft.com/data-integration/gateway/service-gateway-onprem#considerations). Obwohl das Gateway selbst keine zusätzlichen Kosten verursacht, gilt das [Logic Apps-Preismodell](../logic-apps/logic-apps-pricing.md) für diese Connectors und andere Vorgänge in Azure Logic Apps.

## <a name="prerequisites"></a>Voraussetzungen

* Sie haben das [lokale Datengateway bereits auf einem lokalen Computer installiert](../logic-apps/logic-apps-gateway-install.md).

* Sie verwenden das [gleiche Azure-Konto und -Abonnement](../logic-apps/logic-apps-gateway-install.md#requirements), das bei der Installation des Datengateways verwendet wurde. Dieses Azure-Konto muss zu einem einzelnen [Azure Active Directory-Mandanten (Azure AD) oder -Verzeichnis](../active-directory/fundamentals/active-directory-whatis.md#terminology) gehören.

* Ihre Gatewayinstallation wurde noch nicht registriert und von einer anderen Azure-Gatewayressource beansprucht.

  Wenn Sie im Azure-Portal eine Gatewayressource erstellen, wählen Sie eine Gatewayinstallation aus, die mit ihrer Gatewayressource und nur mit dieser Gatewayressource verknüpft ist. In Azure Logic Apps verwenden lokale Trigger und Aktionen dann die Gatewayressource, um eine Verbindung mit lokalen Datenquellen herzustellen. In diesen Triggern und Aktionen wählen Sie Ihr Azure-Abonnement und die zugehörige Gatewayressource aus, die Sie verwenden möchten. Jede Gatewayressource ist nur mit einer Gatewayinstallation verknüpft, die nur mit einem Azure-Konto verknüpft ist.

<a name="create-gateway-resource"></a>

## <a name="create-azure-gateway-resource"></a>Erstellen einer Azure-Gatewayressource

Nachdem Sie das Gateway auf einem lokalen Computer installiert haben, erstellen Sie eine Azure-Ressource für Ihr Gateway.

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) mit dem gleichen Azure-Konto an, das beim Installieren des Gateways verwendet wurde.

1. Geben Sie im Suchfeld des Azure-Portals den Begriff „lokales Datengateway“ ein, und wählen Sie dann **Lokale Datengateways** aus.

   ![Nach „Lokales Datengateway“ suchen](./media/logic-apps-gateway-connection/find-on-premises-data-gateway.png)

1. Wählen Sie unter **Lokale Datengateways** die Option **Hinzufügen** aus.

   ![Hinzufügen neuer Azure-Ressource für das Datengateway](./media/logic-apps-gateway-connection/add-azure-data-gateway-resource.png)

1. Geben Sie unter **Verbindungsgateway erstellen** die folgenden Informationen für Ihre Gatewayressource an. Wählen Sie **Erstellen**, wenn Sie fertig sind.

   | Eigenschaft | BESCHREIBUNG |
   |----------|-------------|
   | **Ressourcenname** | Geben Sie einen Namen für Ihre Gatewayressource an, der nur Buchstaben, Ziffern, Bindestriche (`-`), Unterstriche (`_`), Klammern (`(`, `)`) und Punkte (`.`) enthalten darf. |
   | **Abonnement** | Wählen Sie das Azure-Abonnement für das Azure-Konto aus, das für die Gatewayinstallation verwendet wurde. Das standardmäßige Abonnement basiert auf dem Azure-Konto, das Sie zum Anmelden verwendet haben. |
   | **Ressourcengruppe** | Die [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md), die Sie verwenden möchten. |
   | **Location** | Die gleiche Region oder der gleiche Standort, der während der [Gatewayinstallation](../logic-apps/logic-apps-gateway-install.md) für den Gatewayclouddienst ausgewählt wurde. Andernfalls wird Ihre Gatewayinstallation nicht in der Liste **Installationsname** angezeigt. Der Standort Ihrer Logik-App kann sich vom Standort Ihrer Gatewayressource unterscheiden. |
   | **Installationsname** | Wählen Sie eine Gatewayinstallation aus, die in der Liste nur angezeigt wird, wenn die folgenden Bedingungen erfüllt sind: <p><p>– Die Gatewayinstallation verwendet dieselbe Region wie die Gatewayressource, die Sie erstellen möchten. <br>– Die Gatewayinstallation ist nicht mit einer anderen Azure-Gatewayressource verknüpft. <br>– Die Gatewayinstallation ist mit demselben Azure-Konto verknüpft, das Sie zum Erstellen der Gatewayressource verwenden. <br>– Ihr Azure-Konto gehört zu einem einzigen [Azure Active Directory-Mandanten (Azure AD) oder -Verzeichnis](../active-directory/fundamentals/active-directory-whatis.md#terminology) und ist das gleiche Konto, das für die Gatewayinstallation verwendet wurde. <p><p>Weitere Informationen finden Sie im Abschnitt [Häufig gestellte Fragen](#faq). |
   |||

   Im folgenden Beispiel wird eine Gatewayinstallation gezeigt, die sich in derselben Region wie die Gatewayressource befindet und mit demselben Azure-Konto verknüpft ist:

   ![Angeben von Details zum Erstellen der Datengatewayressource](./media/logic-apps-gateway-connection/add-azure-data-gateway-information.png)

<a name="connect-logic-app-gateway"></a>

## <a name="connect-to-on-premises-data"></a>Herstellen einer Verbindung mit lokalen Daten

Nachdem Sie die Gatewayressource erstellt und Ihr Azure-Abonnement mit dieser Ressource verknüpft haben, können Sie jetzt mit dem Gateway eine Verbindung zwischen Ihrer Logik-App und Ihrer lokalen Datenquelle herstellen.

1. Erstellen oder öffnen Sie im Azure-Portal Ihre Logik-App im Logik-App-Designer.

1. Fügen Sie einen Connector hinzu, der lokale Verbindungen unterstützt, z.B. **SQL Server**.

1. Wählen Sie **Verbinden über lokales Datengateway**.

1. Wählen Sie unter **Gateways** in der Liste **Abonnements** Ihr Azure-Abonnement aus, das über die gewünschte Gatewayressource verfügt.

1. Wählen Sie in der Liste **Verbindungsgateway**, in der die verfügbaren Gatewayressourcen in Ihrem ausgewählten Abonnement angezeigt werden, die gewünschte Gatewayressource aus. Jede Gatewayressource ist mit einer einzelnen Gatewayinstallation verknüpft.

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

   ![Löschen der Gatewayressource in Azure](./media/logic-apps-gateway-connection/delete-on-premises-data-gateway-resource.png)

<a name="faq"></a>

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**F:** Warum wird meine Gatewayinstallation nicht angezeigt, wenn ich die Gatewayressource in Azure erstelle? <br/>
**A:** Dieses Problem kann folgende Ursachen haben:

* Bei Ihrem Azure-Konto muss es sich um das gleiche Konto handeln, das mit der Gatewayinstallation auf dem lokalen Computer verknüpft ist. Überprüfen Sie, ob Sie am Azure-Portal mit der gleichen Identität angemeldet sind, die mit der Gatewayinstallation verknüpft ist. Stellen Sie außerdem sicher, dass Ihr Azure-Konto zu einem einzelnen [Azure AD-Mandanten oder -Verzeichnis](../active-directory/fundamentals/active-directory-whatis.md#terminology) gehört und auf den gleichen Azure AD-Mandanten oder das gleiche Verzeichnis festgelegt ist, das bei der Gatewayinstallation verwendet wurde.

* Die Gatewayressource und die Gatewayinstallation müssen die gleiche Region verwenden. Der Standort Ihrer Logik-App kann sich jedoch vom Standort Ihrer Gatewayressource unterscheiden.

* Ihre Gatewayinstallation wurde bereits registriert und von einer anderen Gatewayressource beansprucht. Diese Installationen werden nicht in der Liste **Installationsname** angezeigt. Sehen Sie sich zum Überprüfen der Gatewayregistrierungen im Azure-Portal alle Azure-Ressourcen mit dem Typ **Lokale Datengateways** für *alle* Azure-Abonnements an. Informationen zum Aufheben der Verknüpfung der Gatewayinstallation mit der anderen Gatewayressource finden Sie unter [Löschen der Gatewayressource](#change-delete-gateway-resource).

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Sichern Ihrer Logik-Apps](./logic-apps-securing-a-logic-app.md)
* [Allgemeine Beispiele und Szenarien für Logik-Apps](./logic-apps-examples-and-scenarios.md)

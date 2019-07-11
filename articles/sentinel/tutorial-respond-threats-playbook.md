---
title: Ausführen eines Playbooks in der Vorschauversion von Azure Sentinel | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie ein Playbook in Azure Sentinel ausführen.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: e4afc5c8-ffad-4169-8b73-98d00155fa5a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 52346e2ff9c47e58f2bd040582bee29eaf08bb13
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2019
ms.locfileid: "67621197"
---
# <a name="tutorial-set-up-automated-threat-responses-in-azure-sentinel-preview"></a>Tutorial: Einrichten automatisierter Reaktionen auf Bedrohungen in der Vorschauversion von Azure Sentinel

> [!IMPORTANT]
> Azure Sentinel ist aktuell als Public Preview verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

In diesem Tutorial erfahren Sie, wie Sie in Azure Sentinel mithilfe von Sicherheitsplaybooks automatisierte Reaktionen auf Bedrohungen im Zusammenhang mit sicherheitsbezogenen Problemen festlegen, die von Azure Sentinel erkannt werden.


> [!div class="checklist"]
> * Grundlegendes zu Playbooks
> * Erstellen eines Playbooks
> * Ausführen eines Playbooks


## <a name="what-is-a-security-playbook-in-azure-sentinel"></a>Was ist ein Sicherheitsplaybook in Azure Sentinel?

Bei einem Sicherheitsplaybook handelt es sich um eine Sammlung von Prozeduren, die über Azure Sentinel als Reaktion auf eine Warnung ausgeführt werden können. Ein Sicherheitsplaybook ermöglicht die Automatisierung und Orchestrierung Ihrer Reaktion und kann manuell ausgeführt oder so konfiguriert werden, dass es automatisch ausgeführt wird, wenn bestimmte Warnungen ausgelöst werden. Da Sicherheitsplaybooks in Azure Sentinel auf [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps) basieren, stehen Ihnen die Leistung, Anpassbarkeit und integrierten Vorlagen zur Verfügung, die Sie von Logic Apps gewohnt sind. Die einzelnen Playbooks werden zwar jeweils für das spezifische, von Ihnen ausgewählte Abonnement erstellt, auf der Seite „Playbooks“ werden jedoch alle Playbooks für alle ausgewählten Abonnements angezeigt.

> [!NOTE]
> Aufgrund der Nutzung von Azure Logic Apps fallen bei der Verwendung von Playbooks Gebühren an. Ausführlichere Informationen finden Sie auf der Preisseite von [Azure Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/).

Wenn Sie beispielsweise einen Angriff auf Ihre Netzwerkressourcen befürchten, können Sie eine Warnung für schädliche IP-Adressen festlegen, die auf Ihr Netzwerk zugreifen. Anschließend können Sie ein Playbook mit folgenden Eigenschaften erstellen:
1. Wenn die Warnung ausgelöst wird, wird in ServiceNow oder in einem anderen IT-Ticketsystem ein Ticket erstellt.
2. Es wird eine Nachricht an Ihren Sicherheitskanal in Microsoft Teams oder Slack gesendet, um Ihre Sicherheitsanalysten auf den Vorfall aufmerksam zu machen.
3. Sämtliche Informationen in der Warnung werden an den leitenden Netzwerkadministrator sowie an den leitenden Sicherheitsadministrator gesendet. Die E-Mail enthält auch zwei Schaltflächen: **Blockieren** und **Ignorieren**.
4. Das Playbook wird nach Eingang einer Antwort der Administratoren weiter ausgeführt.
5. Wenn die Administratoren **Blockieren** auswählen, wird die IP-Adresse in der Firewall blockiert, und der Benutzer wird in Azure AD deaktiviert.
6. Wenn die Administratoren **Ignorieren** auswählen, werden die Warnung in Azure Sentinel und der Vorfall in ServiceNow geschlossen.

Sicherheitsplaybooks können manuell oder automatisch ausgeführt werden. Bei der manuellen Ausführung können Sie bei Eingang einer Warnung auswählen, dass ein Playbook als Reaktion auf die ausgewählte Warnung ausgeführt werden soll. Bei der automatischen Ausführung legen Sie im Zuge der Erstellung der Korrelationsregel fest, dass ein Playbook (oder auch mehrere Playbooks) ausgeführt werden soll, wenn die Warnung ausgelöst wird.


## <a name="create-a-security-playbook"></a>Erstellen eines Sicherheitsplaybooks

Gehen Sie wie folgt vor, um in Azure Sentinel ein neues Sicherheitsplaybook zu erstellen:

1. Öffnen Sie das Dashboard **Azure Sentinel**.
2. Wählen Sie unter **Verwaltung** die Option **Playbooks** aus.

   ![Logik-App](./media/tutorial-respond-threats-playbook/playbookimg.png)

3. Klicken Sie auf der Seite **Azure Sentinel - Playbooks (Vorschauversion)** auf die Schaltfläche **Hinzufügen**.

   ![Erstellen einer Logik-App](./media/tutorial-respond-threats-playbook/create-playbook.png) 

4. Geben Sie auf der Seite **Logik-App erstellen** die erforderlichen Informationen für die Erstellung Ihrer neuen Logik-App ein, und klicken Sie anschließend auf **Erstellen**. 

5. Wählen Sie im [**Logik-App-Designer**](../logic-apps/logic-apps-overview.md) die gewünschte Vorlage aus. Wenn Sie sich für eine Vorlage entscheiden, die Anmeldeinformationen erfordert, müssen Sie diese angeben. Alternativ können Sie auch ein neues leeres Playbook erstellen. Wählen Sie **Leere Logik-App** aus. 

   ![Logik-App-Designer](./media/tutorial-respond-threats-playbook/playbook-template.png)

6. Im daraufhin angezeigten Logik-App-Designer können Sie entweder eine neue Logik-App erstellen oder die Vorlage bearbeiten. Weitere Informationen zum Erstellen eines Playbooks mit Logic Apps finden Sie [hier](../logic-apps/logic-apps-create-logic-apps-from-templates.md).

7. Wenn Sie ein leeres Playbook erstellen, geben Sie im Feld **Alle Connectors und Trigger durchsuchen** die Zeichenfolge *Azure Sentinel* ein, und wählen Sie **Beim Auslösen einer Antwort auf eine Azure Sentinel-Warnung** aus. <br>Das neu erstellte Playbook wird in der Liste **Playbooks** angezeigt. Sollte es nicht angezeigt werden, klicken Sie auf **Aktualisieren**. 

7. Nun können Sie definieren, was passieren soll, wenn das Playbook ausgelöst wird. Sie können eine Aktion, eine logische Bedingung, Parameterbedingungen oder Schleifen hinzufügen.

   ![Logik-App-Designer](./media/tutorial-respond-threats-playbook/logic-app.png)

## <a name="how-to-run-a-security-playbook"></a>Ausführen eines Sicherheitsplaybooks

Sie können ein Playbook bei Bedarf ausführen.

So führen Sie ein Playbook bei Bedarf aus:

1. Wählen Sie auf der Seite **Fälle** einen Fall aus, und klicken Sie anschließend auf **View full details** (Details vollständig anzeigen).

2. Klicken Sie auf der Registerkarte **Warnungen** auf die Warnung, für die Sie das Playbook ausführen möchten, scrollen Sie ganz nach rechts, klicken Sie auf **Playbooks anzeigen**, und wählen Sie in der Liste mit den verfügbaren Playbooks für das Abonnement ein Playbook zum **Ausführen** aus. 




## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie ein Playbook in Azure Sentinel ausführen. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln: In diesem Tutorial haben Sie gelernt, wie Sie ein Playbook in Azure Sentinel ausführen. Im [nächsten Artikel](hunting.md) erfahren Sie, wie Sie mithilfe von Azure Sentinel proaktiv nach Bedrohungen suchen.
> [!div class="nextstepaction"]
> [Suchen Sie proaktiv nach Bedrohungen](hunting.md) für Ihr Netzwerk.


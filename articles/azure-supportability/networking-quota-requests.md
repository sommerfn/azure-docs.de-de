---
title: Erhöhung des Grenzwerts für Netzwerke | Microsoft-Dokumentation
description: Erhöhung des Grenzwerts für Netzwerke
author: anavinahar
ms.author: anavin
ms.date: 06/19/2019
ms.topic: article
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 278e9ff68fa20a0a99a6447bb4cf7ac7fddbfb7b
ms.sourcegitcommit: 961468fa0cfe650dc1bec87e032e648486f67651
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72249110"
---
# <a name="networking-limit-increase"></a>Erhöhung des Grenzwerts für Netzwerke

Navigieren Sie im Azure-Portal zum Blatt **Usages + Quota** (Nutzung + Kontingente), um Ihre aktuelle Netzwerkauslastung und Kontingente anzuzeigen. Sie können auch die [Verbrauchs-CLI](https://docs.microsoft.com//cli/azure/network?view=azure-cli-latest#az-network-list-usages), [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermnetworkusage?view=azurermps-6.13.0) oder die [network usage API](https://docs.microsoft.com/rest/api/virtualnetwork/virtualnetworks/listusage) (Netzwerkauslastungs-API) verwenden, um Ihre Netzwerkauslastung und Grenzwerte anzuzeigen.

Über das Blatt **Hilfe und Support** oder über das Blatt **Usages + Quota** (Nutzung + Kontingente) können Sie im Portal eine Erhöhung anfordern.

## <a name="request-networking-quota-increase-at-subscription-level-using-the-help--support-blade"></a>Anfordern einer Netzwerkkontingenterhöhung auf Abonnementebene mithilfe des Blatts **Hilfe und Support**

Führen Sie die folgenden Schritte aus, um im Azure-Portal über das Blatt „Hilfe und Support“ eine Supportanfrage zu erstellen. 

1. Wählen Sie unter https://portal.azure.com die Option **Hilfe und Support** aus.

    ![Hilfe und Support](./media/resource-manager-core-quotas-request/helpsupport.png)
 
2.  Wählen Sie **Neue Supportanfrage** aus. 

    ![Neue Supportanfrage](./media/resource-manager-core-quotas-request/newsupportrequest.png)

3. Wählen Sie in der Dropdownliste „Problemtyp“ die Option **Grenzwerte für Dienste und Abonnements (Kontingente)** aus.

    ![Dropdownliste „Problemtyp“](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

4. Wählen Sie das Abonnement aus, für das ein höheres Kontingent benötigt wird.

    ![Auswählen des Abonnements (neue Supportanfrage)](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
5. Wählen Sie **Netzwerk** in der Dropdownliste **Kontingenttyp** aus. 

    ![Auswählen des Kontingenttyps](./media/networking-quota-request/select-quota-type-network.png)

6. Klicken Sie unter **Problemdetails** auf **Details angeben**, und geben Sie weitere Informationen an, die bei der Verarbeitung Ihrer Anfrage hilfreich sein können.

    ![Angeben von Details](./media/resource-manager-core-quotas-request/provide-details.png)

7. Wählen Sie im Bereich **Quota details** (Kontingentdetails) das Bereitstellungsmodell, einen Ort und die Ressourcen aus, für die Sie eine Kontingenterhöhung anfordern möchten.

    ![Kontingentdetails (Bereitstellungsmodell)](./media/networking-quota-request/quota-details-network.png)

8.  Geben Sie die gewünschten Grenzwerte für das Abonnement ein. Deaktivieren Sie die Ressource in der Dropdownliste „Ressource“, oder klicken Sie auf das X-Symbol, um eine Zeile zu entfernen. Nachdem Sie die gewünschten Kontingente für jede Ressource eingegeben haben, klicken Sie im Bereich mit den Kontingentdetails auf **Speichern und fortfahren**, um die Erstellung der Supportanfrage fortzusetzen.

    ![Neue Grenzwerte](./media/networking-quota-request/network-new-limits.png)


## <a name="request-networking-quota-increase-at-subscription-level-using-usages--quota-blade"></a>Anfordern einer Netzwerkkontingenterhöhung auf Abonnementebene mithilfe des Blatts **Usages + Quota** (Nutzung + Kontingente)

Führen Sie die folgenden Schritte aus, um im Azure-Portal über das Blatt „Nutzung + Kontingente“ eine Supportanfrage zu erstellen. 

1. Klicken Sie unter https://portal.azure.com auf **Abonnements**.

    ![Abonnements](./media/resource-manager-core-quotas-request/subscriptions.png)

2. Wählen Sie das Abonnement aus, für das ein höheres Kontingent benötigt wird.

    ![Wählen Sie das Abonnement aus.](./media/resource-manager-core-quotas-request/select-subscription.png)

3. Wählen Sie **Nutzung + Kontingente** aus.

    ![Auswählen von „Nutzung + Kontingente“](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4. Klicken Sie in der Ecke oben rechts auf **Erhöhung anfordern**.

    ![Anfordern einer Erhöhung](./media/resource-manager-core-quotas-request/request-increase.png)

5. Führen Sie ab Schritt 3 im Abschnitt *Anfordern einer Netzwerkkontingenterhöhung auf Abonnementebene* mithilfe des Blatts **Hilfe und Support** alle Schritte aus.

## <a name="about-networking-limits"></a>Informationen zu Netzwerkgrenzwerten

Weitere Informationen zu Netzwerkgrenzwerten finden Sie im Abschnitt [Netzwerk](../azure-subscription-service-limits.md#networking-limits) der Seite „Grenzwerte“ oder dem Abschnitt zu häufig gestellten Fragen zu Netzwerkgrenzwerten.
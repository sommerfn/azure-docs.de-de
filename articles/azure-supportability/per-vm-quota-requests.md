---
title: Anforderungen zur Erhöhung von Azure-vCPU-Kontingenten pro VM-Serie | Microsoft-Dokumentation
description: Anforderungen zur Erhöhung von vCPU-Kontingenten pro VM-Serie
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: e99bdd92d4a1aab833a95943b22aaabe7f0daa1d
ms.sourcegitcommit: 961468fa0cfe650dc1bec87e032e648486f67651
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72248678"
---
# <a name="vm-series-vcpu-limit-increase"></a>Erhöhung des vCPU-Grenzwerts für die VM-Serie

Resource Manager-vCPU-Kontingente für virtuelle Computer und VM-Skalierungsgruppen werden in jeder Region für jedes Abonnement auf zwei Ebenen erzwungen. 

Die erste Ebene ist der Grenzwert **Regionale vCPUs gesamt** (für alle VM-Serien). Die zweite Ebene ist der Grenzwert **vCPUs pro VM-Serie** (etwa die vCPUs der D-Serie). Bei jeder Bereitstellung eines neuen virtuellen Computers darf die Summe aus neuer und bereits vorhandener vCPU-Nutzung für diese VM-Serie das für diese spezielle VM-Serie genehmigte vCPU-Kontingent nicht überschreiten. Ferner darf die Gesamtzahl der neuen und vorhandenen vCPUs, die insgesamt für alle VM-Serien bereitgestellt werden, das für das Abonnement genehmigte Kontingent „Regionale vCPUs gesamt“ nicht überschreiten. Wird eines dieser Kontingente überschritten, wird die VM-Bereitstellung nicht zugelassen.
Über das Azure-Portal können Sie eine Erhöhung des vCPU-Kontingentlimits für die VM-Serie anfordern. Bei einer Erhöhung des Kontingents für die VM-Serie erhöht sich automatisch auch der Grenzwert „Regionale vCPUs gesamt“ um den gleichen Wert. 

Weitere Informationen zu Kontingenten finden Sie auf den Seiten [vCPU-Kontingente für virtuelle Computer](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) und [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](https://aka.ms/quotalimits). 

Sie können nun eine Erhöhung über das Blatt **Hilfe und Support** oder über das Blatt **Nutzung + Kontingente** im Portal anfordern. 

## <a name="request-per-vm-series-vcpu-quota-increase-at-subscription-level-using-the-help--support-blade"></a>Anfordern einer vCPU-Kontingenterhöhung pro VM-Serie auf Abonnementebene mithilfe des Blatts **Hilfe und Support**

Führen Sie die folgenden Schritte aus, um im Azure-Portal über das Blatt „Hilfe und Support“ eine Supportanfrage zu erstellen. 

1. Wählen Sie unter https://portal.azure.com die Option **Hilfe und Support** aus.

   ![Hilfe und Support](./media/resource-manager-core-quotas-request/helpsupport.png)
 
2.  Wählen Sie **Neue Supportanfrage** aus. 

     ![Neue Supportanfrage](./media/resource-manager-core-quotas-request/newsupportrequest.png)

3. Wählen Sie in der Dropdownliste „Problemtyp“ die Option **Grenzwerte für Dienste und Abonnements (Kontingente)** aus.

   ![Dropdownliste „Problemtyp“](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

4. Wählen Sie das Abonnement aus, für das ein höheres Kontingent benötigt wird.

   ![Auswählen des Abonnements (neue Supportanfrage)](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
5. Wählen Sie in der Dropdownliste **Kontingenttyp** die Option **Compute-VM (cores/vCPUs) subscription limit increases** (Erhöhung der Grenzwerte für Compute-/VM-Abonnements (Kerne/vCPUs)) aus. 

   ![Auswählen des Kontingenttyps](./media/resource-manager-core-quotas-request/select-quota-type.png)

6. Klicken Sie unter **Problemdetails** auf **Details angeben**, und geben Sie weitere Informationen an, die bei der Verarbeitung Ihrer Anfrage hilfreich sein können.

   ![Angeben von Details](./media/resource-manager-core-quotas-request/provide-details.png)

7. Wählen Sie im Bereich **Kontingentdetails** das Bereitstellungsmodell und einen Standort aus.

   ![Kontingentdetails (Bereitstellungsmodell)](./media/resource-manager-core-quotas-request/quota-details.png)

8. Wählen Sie die **SKU-Familien** aus, für die eine Erhöhung erforderlich ist. 

   ![SKU-Familie](./media/resource-manager-core-quotas-request/sku-family.png)

9. Geben Sie die gewünschten Grenzwerte für das Abonnement ein. Wenn Sie eine Zeile entfernen möchten, deaktivieren Sie die SKU über die Dropdownliste für die SKU-Familie, oder klicken Sie auf das X-Symbol. Nachdem Sie die gewünschten Kontingente für die einzelnen SKU-Familien eingegeben haben, klicken Sie im Bereich mit den Kontingentdetails auf **Speichern und fortfahren**, um die Erstellung der Supportanfrage fortzusetzen.

   ![Neue Grenzwerte](./media/resource-manager-core-quotas-request/new-limits.png)


## <a name="request-per-vm-series-vcpu-quota-increase-at-subscription-level-using-usages--quota-blade"></a>Anfordern einer vCPU-Kontingenterhöhung pro VM-Serie auf Abonnementebene mithilfe des Blatts **Nutzung + Kontingente**

Führen Sie die folgenden Schritte aus, um im Azure-Portal über das Blatt „Nutzung + Kontingente“ eine Supportanfrage zu erstellen. 

1. Klicken Sie unter https://portal.azure.com auf **Abonnements**.

   ![Abonnements](./media/resource-manager-core-quotas-request/subscriptions.png)

2. Wählen Sie das Abonnement aus, für das ein höheres Kontingent benötigt wird.

   ![Wählen Sie das Abonnement aus.](./media/resource-manager-core-quotas-request/select-subscription.png)

3. Wählen Sie **Nutzung + Kontingente** aus.

   ![Auswählen von „Nutzung + Kontingente“](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4. Klicken Sie in der Ecke oben rechts auf **Erhöhung anfordern**.

   ![Anfordern einer Erhöhung](./media/resource-manager-core-quotas-request/request-increase.png)

5. Wählen Sie als Kontingenttyp die Option **Compute-VM (cores/vCPUs) subscription limit increases** (Erhöhung der Grenzwerte für Compute-/VM-Abonnements (Kerne/vCPUs)) aus. 

   ![Ausfüllen des Formulars](./media/resource-manager-core-quotas-request/select-quota-type.png)
   
6. Wählen Sie im Bereich **Kontingentdetails** das Bereitstellungsmodell und einen Standort aus.

   ![Blatt für das Kontingentproblem](./media/resource-manager-core-quotas-request/quota-details.png)

7. Wählen Sie die **SKU-Familien** aus, für die eine Erhöhung erforderlich ist.

   ![Ausgewählte SKU-Serie](./media/resource-manager-core-quotas-request/sku-family.png)

8. Geben Sie die gewünschten Grenzwerte für das Abonnement ein. Wenn Sie eine Zeile entfernen möchten, deaktivieren Sie die SKU über die Dropdownliste für die SKU-Familie, oder klicken Sie auf das X-Symbol. Nachdem Sie die gewünschten Kontingente für die einzelnen SKU-Familien eingegeben haben, klicken Sie auf der Seite mit den Problemschritten auf **Speichern und fortfahren**, um die Erstellung der Supportanfrage fortzusetzen.

   ![Neue SKU-Kontingentanforderung](./media/resource-manager-core-quotas-request/new-limits.png)
 

---
title: Klassisches Azure-Bereitstellungsmodell | Microsoft-Dokumentation
description: Klassisches Azure-Bereitstellungsmodell
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/20/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 70ad44eade871d52591014ee24e645b95c52f1e5
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234867"
---
# <a name="classic-deployment-model"></a>Klassisches Bereitstellungsmodell

Das klassisches Bereitstellungsmodell ist ein älteres Bereitstellungsmodell für Azure. Es erzwingt einen globales Kontingentlimit für vCPUs für VMs und VM-Skalierungsgruppen. Das klassische Bereitstellungsmodell wird nicht mehr empfohlen. Stattdessen eignet sich das Resource Manager-Modell. Weitere Informationen über diese beiden Bereitstellungsmodelle und die Vorteile des Resource Managers finden Sie auf der Seite zum Resource Manager-Bereitstellungsmodell. Sobald ein Abonnement erstellt wird, wird diesem das Standardkontingent für vCPUs zugewiesen. Bei jeder Bereitstellung einer neuen VM über das klassische Bereitstellungsmodell werden die neuen und bereits vorhandenen CPUs für alle Regionen addiert. Dabei darf das für das klassische Bereitstellungsmodell zur Verfügung stehende vCPU-Kontingent nicht überschritten werden. Weitere Informationen zu den Kontingenten erhalten Sie auf der [Seite zu Azure-Abonnements und Diensteinschränkungen](https://aka.ms/quotalimits).

Über das Blatt „Hilfe und Support“ oder über das Blatt „Usages + Quota“ (Nutzung und Kontingente) können Sie im Portal eine Erhöhung der Grenzwerte für vCPUs für das klassische Bereitstellungsmodell beantragen.

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

7. Wählen Sie im Panel **Kontingentdetails** die Option „Klassisch“ und einen Standort aus.

   ![Kontingentdetails (Bereitstellungsmodell)](./media/resource-manager-core-quotas-request/quota-details-classic.png)

8. Wählen Sie die **SKU-Familien** aus, für die eine Erhöhung erforderlich ist. 

   ![SKU-Familie](./media/resource-manager-core-quotas-request/sku-family-classic.png)

9. Geben Sie die gewünschten Grenzwerte für das Abonnement ein. Wenn Sie eine Zeile entfernen möchten, deaktivieren Sie die SKU über die Dropdownliste für die SKU-Familie, oder klicken Sie auf das X-Symbol. Nachdem Sie die gewünschten Kontingente für die einzelnen SKU-Familien eingegeben haben, klicken Sie im Bereich mit den Kontingentdetails auf **Speichern und fortfahren**, um die Erstellung der Supportanfrage fortzusetzen.

   ![Neue Grenzwerte](./media/resource-manager-core-quotas-request/new-limits-classic.png)

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
   
6. Klicken Sie unter **Problemdetails** auf **Details angeben**, und geben Sie weitere Informationen an, die bei der Verarbeitung Ihrer Anfrage hilfreich sein können.

   ![Angeben von Details](./media/resource-manager-core-quotas-request/provide-details.png)

7. Wählen Sie im Panel **Kontingentdetails** die Option „Klassisch“ und einen Standort aus.

   ![Kontingentdetails (Bereitstellungsmodell)](./media/resource-manager-core-quotas-request/quota-details-classic.png)

8. Wählen Sie die **SKU-Familien** aus, für die eine Erhöhung erforderlich ist. 

   ![SKU-Familie](./media/resource-manager-core-quotas-request/sku-family-classic.png)

9. Geben Sie die gewünschten Grenzwerte für das Abonnement ein. Wenn Sie eine Zeile entfernen möchten, deaktivieren Sie die SKU über die Dropdownliste für die SKU-Familie, oder klicken Sie auf das X-Symbol. Nachdem Sie die gewünschten Kontingente für die einzelnen SKU-Familien eingegeben haben, klicken Sie im Bereich mit den Kontingentdetails auf **Speichern und fortfahren**, um die Erstellung der Supportanfrage fortzusetzen.

   ![Neue Grenzwerte](./media/resource-manager-core-quotas-request/new-limits-classic.png)


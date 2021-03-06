---
title: Dienstkontingente und -grenzwerte – Azure Batch | Microsoft-Dokumentation
description: Erfahren Sie mehr über Azure Batch-Standardkontingente, Limits und Einschränkungen sowie die Anforderung von Kontingentsteigerungen.
services: batch
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: 28998df4-8693-431d-b6ad-974c2f8db5fb
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2018
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: 5194fa73d246cee039e113207bbde277b06d7130
ms.sourcegitcommit: 7862449050a220133e5316f0030a259b1c6e3004
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/22/2018
ms.locfileid: "53753198"
---
# <a name="batch-service-quotas-and-limits"></a>Batch-Dienst – Kontingente und Limits

Ebenso wie bei anderen Azure-Diensten gelten bei bestimmten Ressourcen in Verbindung mit dem Batch-Dienst Limits. Viele dieser Limits sind Standardkontingente, die von Azure auf Abonnement- oder Kontoebene angewendet werden. In diesem Artikel werden diese Standardwerte erläutert, und Sie erfahren, wie Sie Kontingenterhöhungen anfordern können.

Bedenken Sie diese Kontingente beim Entwerfen und Skalieren Ihrer Batchworkloads. Wenn Ihr Pool beispielsweise nicht die von Ihnen vorgegebene Anzahl von Computeknoten erreicht, haben Sie möglicherweise die Kernkontingentgrenze für Ihr Batch-Konto erreicht.

Sie können mehrere Batch-Workloads in einem Batch-Konto ausführen oder Ihre Workloads auf Batch-Konten in demselben Abonnement, aber verschiedenen Azure-Regionen aufteilen.

Wenn Sie Produktionsworkloads in Batch ausführen möchten, müssen Sie möglicherweise ein oder mehrere Kontingente über den Standardwert erhöhen. Wenn Sie ein Kontingent erhöhen möchten, können Sie kostenlos eine [Anfrage an den Onlinekundensupport](#increase-a-quota) richten.

> [!NOTE]
> Bei einem Kontingent handelt es sich um ein Kreditlimit und keine Kapazitätsgarantie. Wenn Sie einen umfangreichen Kapazitätsbedarf haben, wenden Sie sich an den Azure-Support.
> 
> 

## <a name="resource-quotas"></a>Ressourcenkontingente
[!INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]


### <a name="cores-quotas-in-user-subscription-mode"></a>Kontingente für Kerne im Modus „Benutzerabonnement“

Wenn Sie ein Batch-Konto erstellen, bei dem der Poolzuordnungsmodus auf **Benutzerabonnement** festgelegt ist, werden Kontingente unterschiedlich angewendet. In diesem Modus werden Batch-VMs und andere Ressourcen direkt in Ihrem Abonnement erstellt, wenn ein Pool erstellt wird. Die Kontingente für Kerne von Azure Batch gilt nicht für Konten, die in diesem Modus erstellt werden. Stattdessen werden die Kontingente in Ihrem Abonnement für regionale Computekerne und andere Ressourcen angewendet. Weitere Informationen zu diesen Kontingenten finden Sie unter [Einschränkungen für Azure-Abonnements und -Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md).

## <a name="pool-size-limits"></a>Poolgrößenbeschränkungen

| **Ressource** | **Maximales Limit** |
| --- | --- |
| **Serverknoten im Pool mit [aktivierter Kommunikation zwischen den Knoten](batch-mpi.md)**  ||
| Poolzuordnungsmodus für den Batch-Dienst | 100 |
| Poolzuordnungsmodus für das Batch-Abonnement | 80 |
| **Serverknoten im [mit benutzerdefinierten VM-Images erstellten Pool](batch-custom-images.md)**<sup>1</sup> ||
| Dedizierte Knoten | 2000 |
| Knoten mit niedriger Priorität | 1000 |

<sup>1</sup> Für Pools, bei denen die Kommunikation zwischen den Knoten nicht aktiviert ist

## <a name="other-limits"></a>Andere Limits

| **Ressource** | **Maximales Limit** |
| --- | --- |
| [Gleichzeitige Aufgaben](batch-parallel-node-tasks.md) pro Computeknoten | 4 x Anzahl der Kerne des Knotens |
| [Anwendungen](batch-application-packages.md) pro Batch-Konto | 20 |
| Anwendungspakete pro Anwendung. | 40 |
| Maximale Lebensdauer von Tasks | 180 Tage<sup>1</sup> |

<sup>1</sup> Die maximale Lebensdauer eines Tasks (vom Hinzufügen zum Auftrag bis zum Abschluss) beträgt 180 Tage. Abgeschlossene Tasks bleiben sieben Tage lang erhalten. Daten für Tasks, die nicht innerhalb der maximalen Lebensdauer abgeschlossen wurden, stehen hingegen nicht zur Verfügung.

## <a name="view-batch-quotas"></a>Anzeigen von Batch-Kontingenten

Sie können die Kontingente Ihres Batch-Kontos im [Azure-Portal][portal] anzeigen.

1. Wählen Sie **Batch-Konten** im Portal, und wählen Sie dann das Batch-Konto, das Sie interessiert.
1. Wählen Sie im Menü des Batch-Kontos die Option **Kontingente** aus.
1. Zeigen Sie die Kontingente an, die derzeit für das Batch-Konto gelten.
   
    ![Batch-Kontokontingente][account_quotas]



## <a name="increase-a-quota"></a>Erhöhen eines Kontingents

Führen Sie die folgenden Schritte aus, um über das [Azure-Portal][portal] eine Kontingenterhöhung für Ihr Batch-Konto oder Abonnement anzufordern. Die Art der Kontingenterhöhung richtet sich nach dem Poolzuordnungsmodus Ihres Batch-Kontos.

### <a name="increase-a-batch-cores-quota"></a>Erhöhen des Kernkontingents für Batch 

1. Wählen Sie auf Ihrem Portaldashboard die Kachel **Hilfe und Support** oder das Fragezeichen (**?**) in der oberen rechten Ecke des Portals.
1. Wählen Sie **Neue Supportanfrage** > **Grundlagen** aus.
1. Gehen Sie unter **Grundlegende Einstellungen** wie folgt vor:
   
    a. **Problemtyp** > **Kontingent**
   
    b. Wählen Sie Ihr Abonnement aus.
   
    c. **Kontingenttyp** > **Batch**
   
    d. **Supportplan** > **Kontingentsupport – inbegriffen**
   
    Klicken Sie auf **Weiter**.
1. Gehen Sie unter **Problem** wie folgt vor:
   
    a. Wählen Sie einen **Schweregrad** gemäß der [geschäftlichen Auswirkung][support_sev] aus.
   
    b. Geben Sie unter **Details**jedes Kontingent an, das Sie ändern möchten, sowie den Batch-Kontonamen und das neue Limit.
   
    Klicken Sie auf **Weiter**.
1. Gehen Sie unter **Kontaktinformationen** wie folgt vor:
   
    a. Wählen Sie eine **bevorzugte Kontaktmethode**aus.
   
    b. Überprüfen Sie die erforderlichen Kontaktdetails, und geben Sie sie ein.
   
    Klicken Sie auf **Erstellen** , um die Supportanfrage zu übermitteln.

Nachdem Sie die Supportanfrage übermittelt haben, wird sich der Azure-Support mit Ihnen in Verbindung setzen. Beachten Sie, dass die Bearbeitung der Anfrage bis zu zwei Werktage in Anspruch nehmen kann.

## <a name="related-quotas-for-vm-pools"></a>Verknüpfte Kontingente für VM-Pools

Batch-Pools in der Konfiguration von virtuellen Computern, die in einem virtuellen Azure-Network bereitgestellt wurden, ordnen automatisch zusätzliche Azure-Netzwerkressourcen zu. Die folgenden Ressourcen sind für alle 50 Poolknoten in einem virtuellen Netzwerk erforderlich:

* 1 [Netzwerksicherheitsgruppe](../virtual-network/security-overview.md#network-security-groups)
* 1 [öffentliche IP-Adresse](../virtual-network/virtual-network-ip-addresses-overview-arm.md)
* 1 [Lastenausgleich](../load-balancer/load-balancer-overview.md)

Diese Ressourcen werden in dem Abonnement zugeordnet, das das virtuelle Netzwerk enthält, das beim Erstellen des Batch-Pools bereitgestellt wurde. Diese Ressourcen werden durch die [Ressourcenkontingente](../azure-subscription-service-limits.md) des Abonnements beschränkt. Wenn Sie große Poolbereitstellungen in einem virtuellen Netzwerk planen, überprüfen Sie die Kontingente des Abonnements für diese Ressourcen. Fordern Sie bei Bedarf eine Erhöhung im Azure-Portal an, indem Sie **Hilfe und Support** auswählen.


## <a name="related-topics"></a>Verwandte Themen
* [Erstellen und Verwalten eines Azure Batch-Kontos im Azure-Portal](batch-account-create-portal.md)
* [Übersicht über Azure Batch-Features](batch-api-basics.md)
* [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.png

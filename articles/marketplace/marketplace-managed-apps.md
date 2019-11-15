---
title: 'Azure-Anwendungen: Leitfaden für die Veröffentlichung von Angeboten verwalteter Anwendungen'
description: In diesem Artikel werden die Anforderungen für die Veröffentlichung verwalteter Anwendungen im Marketplace beschrieben.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: qianw211
manager: evansma
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
ms.date: 06/14/2018
ms.author: v-qiwe
ms.openlocfilehash: bdb2a54748c734d7b44dfa0b1f106d37e817f80e
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73822932"
---
# <a name="azure-applications-managed-application-offer-publishing-guide"></a>Azure-Anwendungen: Leitfaden für die Veröffentlichung von Angeboten verwalteter Anwendungen

Verwaltete Anwendungen zählen zu den wichtigsten Verfahren, um eine Lösung im Marketplace zu veröffentlichen. Informieren Sie sich in diesem Handbuch über die Anforderungen für dieses Angebot. 

Hierbei handelt es sich um Transaktionsangebote, die über den Marketplace bereitgestellt und abgerechnet werden. Die Benutzer sehen hier den Aktionsaufruf „Jetzt kaufen“.

Verwenden der Azure-App: Angebotstyp „Verwaltete App“, wenn die folgenden Bedingungen erforderlich sind:
- Sie stellen entweder eine Abonnement-basierte Lösung für Ihren Kunden mit einer VM oder eine vollständig IaaS-basierte Lösung bereit.
- Sie oder Ihr Kunde setzen voraus, dass die Lösung von einem Partner verwaltet wird.

>[!NOTE]
>Ein Partner kann beispielsweise ein Anbieter für SI oder verwaltete Dienste (MSP) sein.  

## <a name="managed-application-offer"></a>Angebot „Verwaltete App“

|Requirements (Anforderungen) |Details  |
|---------|---------|
|Für das Azure-Abonnement eines Kunden bereitgestellt | Verwaltete Apps müssen im Abonnement des Kunden bereitgestellt werden und können von einem Drittanbieter verwaltet werden. | 
|Abrechnung und Messung    |  Die Ressourcen werden im Azure-Abonnement des Kunden bereitgestellt. Für virtuelle Computer mit nutzungsbasierter Bezahlung (Pay-As-You-Go, PAYGO) wird die Transaktion mit dem Kunden über Microsoft und die Abrechnung über das Azure-Abonnement des Kunden (PAYGO) abgewickelt. <br> Im Fall von Bring-Your-Own-License rechnet Microsoft die angefallenen Infrastrukturkosten im Kundenabonnement ab, während Sie Ihre Softwarelizenzgebühren direkt mit dem Kunden abrechnen.        |
|Azure-kompatible virtuelle Festplatte (VHD)    |   VMs müssen unter Windows oder Linux erstellt werden.<ul> <ul> <li>Weitere Informationen zum Erstellen einer Linux-VHD finden Sie unter [Von Azure unterstützte Distributionen von Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros).</li> <li>Weitere Informationen zum Erstellen einer Windows-VHD finden Sie unter [Erstellen einer Azure-kompatiblen VHD](./cloud-partner-portal/virtual-machine/cpp-create-vhd.md).</li> </ul> |

>[!NOTE]
> Verwaltete Apps müssen im gesamten Marketplace bereitgestellt werden können. In Bezug auf die Kundenkommunikation ist zu beachten, dass Sie interessierte Kunden erreichen können, nachdem der gemeinsame Zugriff auf Leads aktiviert ist.  

>[!Note]
>Die Nutzung des Cloud Solution Provider (CSP)-Partnerkanals ist jetzt verfügbar.  Unter [Cloud Solution Providers](./cloud-solution-providers.md) finden Sie weitere Informationen zum Vermarkten Ihres Angebots über die Microsoft CSP-Partnerkanäle.

## <a name="next-steps"></a>Nächste Schritte
Falls Sie dies noch nicht getan haben, 

- [Registrieren Sie](https://azuremarketplace.microsoft.com/sell) beim Marketplace.

Wenn Sie registriert sind und ein neues Angebot erstellen oder an einem vorhandenen arbeiten,

- [Melden Sie sich beim Cloud-Partnerportal an](https://cloudpartner.azure.com), um Ihr Angebot zu erstellen oder zu vervollständigen.

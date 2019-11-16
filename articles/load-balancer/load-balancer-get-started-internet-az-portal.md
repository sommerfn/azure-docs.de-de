---
title: Erstellen eines Load Balancer mit zonenredundantem Front-End – Azure-Portal
titleSuffix: Azure Load Balancer
description: Erfahren Sie, wie Sie im Azure-Portal eine öffentlichen Load Balancer Standard-Instanz mit zonenredundantem öffentlichem IP-Adressen-Front-End erstellen.
services: load-balancer
documentationcenter: na
author: asudbring
manager: kumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2018
ms.author: allensu
ms.openlocfilehash: 4896030b1980defc79ff7a7ccd24ee22202ce167
ms.sourcegitcommit: b1a8f3ab79c605684336c6e9a45ef2334200844b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74049068"
---
#  <a name="create-a-standard-load-balancer-with-zone-redundant-frontend-using-azure-portal"></a>Erstellen einer Load Balancer Standard-Instanz mit zonenredundantem Front-End über das Azure-Portal

In diesem Artikel wird die Erstellung eines öffentlichen [Load Balancer Standard](https://aka.ms/azureloadbalancerstandard) mit zonenredundantem Front-End mithilfe einer öffentlichen IP-Standard-Adresse erläutert. Eine einzelne Front-End-IP-Adresse auf einem standardmäßigen Load Balancer ist standardmäßig zonenredundant.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

> [!NOTE]
>  Unterstützung für Verfügbarkeitszonen ist für ausgewählte Azure-Ressourcen und -Regionen sowie VM-Größenkategorien verfügbar. Weitere Informationen zu den ersten Schritten sowie zu den Azure-Ressourcen, -Regionen und VM-Größenkategorien, die mit Verfügbarkeitszonen verwendet werden können, finden Sie unter [Overview of Availability Zones in Azure (Preview) (Übersicht über Verfügbarkeitszonen in Azure (Vorschauversion))](https://docs.microsoft.com/azure/availability-zones/az-overview). Wenn Sie Unterstützung benötigen, können Sie über [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) Kontakt aufnehmen oder [ein Azure-Supportticket erstellen](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

## <a name="log-in-to-azure"></a>Anmelden an Azure 

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="create-a-zone-redundant-load-balancer"></a>Erstellen eines zonenredundanten Lastenausgleichs

1. Navigieren Sie in einem Browser zum Azure-Portal [https://portal.azure.com](https://portal.azure.com), und melden Sie sich mit Ihrem Azure-Konto an.
2. Klicken Sie links oben auf dem Bildschirm auf **Ressource erstellen** > **Netzwerk** > **Lastenausgleich**.
3. Geben Sie auf der Seite **Lastenausgleich erstellen** unter **Name** **myLoadBalancer** ein.
4. Wählen Sie unter **Typ** die Option **Öffentlich** aus.
5. Wählen Sie unter „SKU“ **Standard**  aus.
6. Klicken Sie auf **Öffentliche IP-Adresse** und **Neu erstellen**, und geben Sie auf der Seite **Öffentliche IP-Adresse erstellen** unter „Name“ **myPublicIPStandard** ein.
    >[!NOTE] 
    > Die in diesem Schritt erstellte öffentliche IP-Adresse ist von der Standard-SKU und standardmäßig zonenredundant. 
8. Wählen Sie unter **Standort** **East US2** (USA, Osten 2) aus, und klicken Sie dann auf **OK**. Die Bereitstellung des Load Balancers wird dann gestartet und dauert einige Minuten.

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr zu [Standard-Lastenausgleich und Verfügbarkeitszonen](load-balancer-standard-availability-zones.md).




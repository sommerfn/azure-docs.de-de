---
title: Hinzufügen oder Entfernen einer Subnetzdelegierung in einem virtuellen Azure-Netzwerk
titlesuffix: Azure Virtual Network
description: Erfahren Sie, wie Sie ein delegiertes Subnetz für einen Dienst in Azure hinzufügen oder entfernen.
services: virtual-network
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/01/2019
ms.author: kumud
ms.openlocfilehash: 5fa340fc3c839d74f292f551b73184ea4df1c0f1
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72175950"
---
# <a name="add-or-remove-a-subnet-delegation"></a>Hinzufügen oder Entfernen einer Subnetzdelegierung

Bei der Subnetzdelegierung erhält der Dienst explizite Berechtigungen, um dienstspezifische Ressourcen im Subnetz zu erstellen, indem beim Bereitstellen des Diensts ein eindeutiger Bezeichner verwendet wird. Dieser Artikel beschreibt das Hinzufügen oder Entfernen eines delegierten Subnetzes für einen Azure-Dienst.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="create-the-virtual-network"></a>Erstellen des virtuellen Netzwerks

In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk und das Subnetz, das Sie später an einen Azure-Dienst delegieren.

1. Wählen Sie oben links auf dem Bildschirm **Ressource erstellen** > **Netzwerk** > **Virtuelles Netzwerk** aus.
1. Geben Sie in **Virtuelles Netzwerk erstellen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | NAME | Geben Sie *MyVirtualNetwork* ein. |
    | Adressraum | Geben Sie *10.0.0.0/16* ein. |
    | Subscription | Wählen Sie Ihr Abonnement aus.|
    | Resource group | Wählen Sie **Neue erstellen** aus, geben Sie *myResourceGroup* ein, und wählen Sie **OK** aus. |
    | Location | Wählen Sie **EastUS** aus.|
    | Subnetzname | Geben Sie *mySubnet* ein. |
    | Subnetzadressbereich | Geben Sie *10.0.0.0/24* ein. |
    |||
1. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie dann **Erstellen** aus.

## <a name="permissons"></a>Berechtigungen

Wenn Sie das Subnetz, das Sie an den Azure-Dienst delegieren möchten, nicht persönlich erstellt haben, benötigen Sie die folgende Berechtigung: `Microsoft.Network/virtualNetworks/subnets/write`.

Die integrierte Rolle [Netzwerkmitwirkender](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) enthält ebenfalls die erforderlichen Berechtigungen.

## <a name="delegate-a-subnet-to-an-azure-service"></a>Delegieren eines Subnetzes an einen Azure-Dienst

In diesem Abschnitt delegieren Sie das Subnetz, das Sie im vorhergehenden Abschnitt erstellt haben, an einen Azure-Dienst.

1. Geben Sie in der Suchleiste des Portals *myVirtualNetwork* ein. Wenn **myVirtualNetwork** in den Suchergebnissen angezeigt wird, können Sie den Begriff auswählen.
2. Wählen Sie in den Suchergebnissen *myVirtualNetwork* aus.
3. Wählen Sie unter **EINSTELLUNGEN** **Subnetze** und dann **mySubnet** aus.
4. Treffen Sie auf der Seite *mySubnet* für die LIste **Subnetzdelegierung** eine Auswahl unter den Diensten, die unter **Subnetz an einen Dienst delegieren** aufgelistet sind (beispielsweise **Microsoft.DBforPostgreSQL/serversv2**).  

## <a name="remove-subnet-delegation-from-an-azure-service"></a>Entfernen einer Subnetzdelegierung aus einem Azure-Dienst

1. Geben Sie in der Suchleiste des Portals *myVirtualNetwork* ein. Wenn **myVirtualNetwork** in den Suchergebnissen angezeigt wird, können Sie den Begriff auswählen.
2. Wählen Sie in den Suchergebnissen *myVirtualNetwork* aus.
3. Wählen Sie unter **EINSTELLUNGEN** **Subnetze** und dann **mySubnet** aus.
4. Wählen Sie auf der Seite *mySubnet* für die Liste **Subnetzdelegierung** unter den Diensten, die unter **Subnetz an einen Dienst delegieren** **Keins** aus. 

## <a name="next-steps"></a>Nächste Schritte
- Informationen zum [Verwalten von Subnetzen in Azure](virtual-network-manage-subnet.md).

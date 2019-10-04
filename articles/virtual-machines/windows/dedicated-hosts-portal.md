---
title: Bereitstellen von dedizierten Azure-Hosts über das Azure-Portal | Microsoft-Dokumentation
description: Stellen Sie VMs auf dedizierten Hosts über das Azure-Portal bereit.
services: virtual-machines-windows
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/26/2019
ms.author: cynthn
ms.openlocfilehash: 0a3cb76d1e28559727957c1007b0992fb8e90f2b
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68700218"
---
# <a name="preview-deploy-vms-to-dedicated-hosts-using-the-portal"></a>Vorschau: Bereitstellen von VMs auf dedizierten Hosts über das Portal

Dieser Artikel führt Sie durch die Erstellung eines [dedizierten Azure-Hosts](dedicated-hosts.md) zum Hosten Ihrer virtuellen Computer (VMs). 

[!INCLUDE [virtual-machines-common-dedicated-hosts-portal](../../../includes/virtual-machines-common-dedicated-hosts-portal.md)]

## <a name="create-a-vm"></a>Erstellen einer VM

1. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen**.
1. Wählen Sie auf der Seite **Neu** unter **Beliebt** die Option **Windows Server 2016 Datacenter** aus.
1. Stellen Sie auf der Registerkarte **Grundlagen** unter **Projektdetails** sicher, dass das richtige Abonnement ausgewählt ist, und wählen Sie dann *myDedicatedHostsRG* als **Ressourcengruppe** aus. 
1. Geben Sie unter **Instanzdetails** *myVM* als **Namen des virtuellen Computers** ein, und wählen Sie *USA, Osten* als Ihre **Ort** aus.
1. Wählen Sie unter **Verfügbarkeitsoptionen** die Option **Verfügbarkeitszone** aus, und wählen Sie in der Dropdownliste *1* aus.
1. Wählen Sie für die Größe **Größe ändern** aus. Wählen Sie in der Liste der verfügbaren Größen eine der Esv3-Serie aus, z. B. **Standard E2s v3**. Möglicherweise müssen Sie den Filter löschen, um alle verfügbaren Größen anzuzeigen.
1. Geben Sie unter **Administratorkonto** einen Benutzernamen wie *azureuser* an sowie ein Kennwort. Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](faq.md#what-are-the-password-requirements-when-creating-a-vm) erfüllen.
1. Wählen Sie unter **Regeln für eingehende Ports** die Option **Ausgewählte Ports zulassen** aus, und wählen Sie dann **RDP (3389)** aus der Dropdownliste aus.
1. Wählen Sie oben auf der Seite die Registerkarte **Erweitert** und im Abschnitt **Host** den Eintrag *myHostGroup* für **Hostgruppe**  und *myHost* für den **Host** aus. 
    ![Auswählen von Hostgruppe und Host](./media/dedicated-hosts-portal/advanced.png)
1. Belassen Sie die übrigen Standardeinstellungen, und wählen Sie dann die Schaltfläche **Überprüfen + erstellen** am unteren Rand der Seite aus.
1. Wenn eine Meldung über die erfolgreiche Validierung angezeigt wird, wählen Sie **Erstellen** aus.


## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie in der Übersicht zu [dedizierten Hosts](dedicated-hosts.md). 

- [Hier](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md) finden Sie eine Beispielvorlage, die sowohl Zonen als auch Fehlerdomänen für maximale Resilienz in einer Region verwendet.

- Sie können dedizierte Hosts auch über [Azure PowerShell](dedicated-hosts-powershell.md) bereitstellen.
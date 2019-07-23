---
title: Verwalten von SQL Server-VMs in Azure über das Azure-Portal | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie im Azure-Portal auf die Ressource „Virtuelle SQL-Computer“ für eine in Azure gehostete SQL Server-VM zugreifen.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/13/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: e2a807bbd6baeb2f14a6d36f5d98a28d48725449
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67082039"
---
# <a name="manage-sql-server-vms-in-azure-using-the-azure-portal"></a>Verwalten von SQL Server-VMs in Azure über das Azure-Portal

Es gibt ein neuen Zugriffspunkt für die Verwaltung Ihrer SQL Server-VMs in Azure mithilfe des [Azure-Portals](https://portal.azure.com). 

Die Ressource **Virtuelle SQL-Computer** ist jetzt ein unabhängiger Verwaltungsdienst, mit dem Sie alle Ihre SQL Server-VMs gleichzeitig anzeigen und die Einstellungen für SQL Server ändern können: 

![Ressource „Virtuelle SQL-Computer“](media/virtual-machines-windows-sql-manage-portal/sql-vm-manage.png)


## <a name="remarks"></a>Anmerkungen

- Die Ressource **Virtuelle SQL-Computer** ist die empfohlene Methode zum Anzeigen und Verwalten Ihre SQL Server-VMs. Derzeit unterstützt die Ressource **Virtuelle SQL-Computer** jedoch noch nicht die Verwaltung von SQL Server-VMs, deren [Support abgelaufen](virtual-machines-windows-sql-server-2008-eos-extend-support.md) ist. Um die Einstellungen für Ihre SQL Server-VMs mit abgelaufenem Support zu verwalten, verwenden Sie stattdessen die veraltete [Registerkarte „SQL Server-Konfiguration“](#access-sql-server-configuration-tab). 
- Die Ressource **Virtuelle SQL-Computer** ist nur für SQL Server-VMs verfügbar, die [mit dem SQL-VM-Ressourcenanbieter registriert](virtual-machines-windows-sql-ahb.md#register-sql-server-vm-with-the-sql-vm-resource-provider) wurden. 


## <a name="access-sql-virtual-machine-resource"></a>Zugreifen auf die Ressource „Virtuelle SQL-Computer“
Um auf die Ressource „Virtuelle SQL-Computer“ zuzugreifen, führen Sie folgende Schritte aus:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com). 
1. Wählen Sie **Alle Dienste** aus. 
1. Geben Sie im Suchfeld als Suchbegriff `SQL virtual machines` ein.
1. (Optional): Wählen Sie das Sternsymbol neben **Virtuelle SQL-Computer** aus, um diese Option Ihrem Menü „Favoriten“ hinzuzufügen. 
1. Wählen Sie **Virtuelle SQL-Computer** aus. 

   ![Ermitteln von virtuellen SQL-Computern in allen Diensten](media/virtual-machines-windows-sql-manage-portal/sql-vm-search.png)

1. Damit listen Sie alle SQL Server-VMs auf, die im Abonnement verfügbar sind. Wählen Sie die zu verwaltende VM aus, um die Ressource **Virtuelle SQL-Computer** zu öffnen. Verwenden Sie das Suchfeld, wenn Ihre SQL Server-VM nicht direkt angezeigt wird. 

![Alle verfügbaren SQL-VMs](media/virtual-machines-windows-sql-manage-portal/all-sql-vms.png)

Durch Auswählen der SQL Server-VM wird die Ressource **Virtuelle SQL-Computer** geöffnet: 


![Ressource „Virtuelle SQL-Computer“](media/virtual-machines-windows-sql-manage-portal/sql-vm-resource.png)

  > [!TIP]
  > Die Ressource **Virtuelle SQL-Computer** ist für SQL Server-Einstellungen vorgesehen. Wählen Sie den Namen des virtuellen Computers im Feld **Virtueller Computer** aus, um zu den Einstellungen dieser spezifischen VM zu navigieren, die jedoch nicht nur für SQL Server gelten. 

## <a name="access-sql-server-configuration-tab"></a>Zugreifen auf die Registerkarte „SQL Server-Konfiguration“
Die Registerkarte „SQL Server-Konfiguration“ ist veraltet. Sie stellt im Moment jedoch die einzige Möglichkeit dar, um virtuelle SQL Server-Computer mit [Ende des Supports (EOS)](virtual-machines-windows-sql-server-2008-eos-extend-support.md) sowie SQL Server-VMs zu verwalten, die nicht mit dem [SQL-VM-Ressourcenanbieter](virtual-machines-windows-sql-ahb.md#register-sql-server-vm-with-the-sql-vm-resource-provider) registriert wurden.

Für den Zugriff auf die veraltete Registerkarte „SQL Server-Konfiguration“ müssen Sie zur Ressource **Virtuelle Computer** navigieren. Führen Sie hierzu folgende Schritte aus:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com). 
1. Wählen Sie **Alle Dienste** aus. 
1. Geben Sie im Suchfeld als Suchbegriff `virtual machines` ein.
1. (Optional): Wählen Sie das Sternsymbol neben **Virtuelle Computer** aus, um diese Option Ihrem Menü „Favoriten“ hinzuzufügen. 
1. Wählen Sie **Virtuelle Computer** aus. 

   ![Suchen virtueller Computer](media/virtual-machines-windows-sql-manage-portal/vm-search.png)

1. Damit listen Sie alle virtuellen Computer im Abonnement auf. Wählen Sie die zu verwaltende VM aus, um die Ressource **Virtuelle Computer** zu öffnen. Verwenden Sie das Suchfeld, wenn Ihre SQL Server-VM nicht direkt angezeigt wird. 
1. Wählen Sie **SQL Server-Konfiguration** im Bereich **Einstellungen** aus, um Ihre SQL Server-Instanzen zu verwalten. 

![SQL Server-Konfiguration](media/virtual-machines-windows-sql-manage-portal/sql-vm-configuration.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln: 

* [Übersicht über SQL Server auf einem virtuellen Windows-Computer](virtual-machines-windows-sql-server-iaas-overview.md)
* [Häufig gestellte Fragen zu SQL Server auf virtuellen Windows-Computern in Azure](virtual-machines-windows-sql-server-iaas-faq.md)
* [Preisinformationen für Azure-VMs mit SQL Server](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server on Azure Virtual Machine release notes](virtual-machines-windows-sql-server-iaas-release-notes.md) (SQL Server auf virtuellem Azure-Computer – Versionshinweise)



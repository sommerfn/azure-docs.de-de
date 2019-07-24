---
title: Ausführen eines direkten Upgrades der SQL Server-Edition auf einem virtuellen Azure-Computer | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Edition Ihrer SQL Server-VM in Azure ändern.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: jroth
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/26/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 05d2170fe9e6055179bf49d803d4739ddc05be89
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67607354"
---
# <a name="how-to-perform-an-in-place-upgrade-of-sql-server-edition-on-azure-vm"></a>Ausführen eines direkten Upgrades der SQL Server-Edition auf einem virtuellen Azure-Computer

In diesem Artikel wird beschrieben, wie Sie die Edition von SQL Server für einen vorhandenen SQL Server auf einem virtuellen Windows-Computer in Azure ändern. 

Die Edition von SQL Server richtet sich nach dem Product Key und wird bei der Installation angegeben. Die Edition bestimmt, welche [Features](/sql/sql-server/editions-and-components-of-sql-server-2017) innerhalb des SQL Server-Produkts verfügbar sind. Sie können die SQL Server-Edition mit den Installationsmedien ändern und entweder ein Downgrade durchführen, um die Kosten zu senken, oder ein Upgrade durchführen, um weitere Features zu aktivieren.

Wenn Sie die Edition von SQL Server mithilfe der Installationsmedien nach der Registrierung beim SQL-VM-Ressourcenanbieter aktualisiert haben, sollten Sie anschließend zum entsprechenden Aktualisieren der Azure-Abrechnung die Eigenschaft für die SQL Server-Edition der SQL-VM-Ressource wie folgt festlegen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
1. Navigieren Sie zu Ihrer SQL Server-VM-Ressource. 
1. Klicken Sie unter **Einstellungen** auf **Konfigurieren**, und wählen Sie dann die gewünschte Edition von SQL Server aus der Dropdownliste unter **Edition** aus. 

   ![Ändern von Editionsmetadaten](media/virtual-machines-windows-sql-change-edition/edition-change-in-portal.png)

1. Lesen Sie die Warnung, die Sie darüber informiert, dass Sie zuerst die SQL Server-Edition ändern müssen und dass die Editionseigenschaft der SQL Server-Edition entsprechen muss. 
1. Wählen Sie **Übernehmen** aus, um die Änderungen der Editionsmetadaten anzuwenden. 


## <a name="prerequisites"></a>Voraussetzungen

Für eine direkte Änderung der Edition von SQL Server benötigen Sie Folgendes: 

- Ein [Azure-Abonnement](https://azure.microsoft.com/free/).
- Eine Windows-[SQL Server-VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision), die beim [SQL-VM-Ressourcenanbieter](virtual-machines-windows-sql-register-with-resource-provider.md) registriert ist.
- Setupmedien mit der gewünschten Edition von SQL Server. Kunden, die über [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default) verfügen, können Ihre Installationsmedien vom [Volume Licensing Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx) erhalten. Kunden ohne Software Assurance können die Setupmedien aus einem SQL Server-VM-Image mit der gewünschten Edition im Marketplace verwenden.


## <a name="upgrade-edition"></a>Upgrade der Edition

  > [!WARNING]
  > - **Durch ein Upgrade der SQL Server-Edition werden der Dienst für SQL Server sowie alle zugeordneten Dienste, z.B. Analysis Services und R Services, neu gestartet.** 

Für ein Upgrade der SQL Server-Edition beschaffen Sie sich die SQL Server-Setupmedien für die gewünschte Edition, und führen Sie dann die folgenden Schritte aus:

1. Starten Sie „Setup.exe“ von den SQL Server-Installationsmedien. 
1. Navigieren Sie zu **Wartung**, und wählen Sie die Option **Editionsupgrade** aus. 

   ![Upgrade der Edition von SQL Server](media/virtual-machines-windows-sql-change-edition/edition-upgrade.png)

1. Klicken Sie auf **Weiter**, bis Sie zur Seite **Die Edition kann jetzt aktualisiert werden** gelangen, und wählen Sie dann **Update** aus. Das Setupfenster kann einige Minuten hängen, während die Änderung wirksam wird, und Sie sehen dann eine Seite **Abgeschlossen**, auf der bestätigt wird, dass das Editionsupgrade abgeschlossen ist. 

Nachdem Sie ein Upgrade der SQL Server-Edition durchgeführt haben, sollten Sie die Eigenschaft „Edition“ des virtuellen SQL-Computers im Azure-Portal wie oben gezeigt ändern. Dadurch werden die Metadaten und die Abrechnung für diesen virtuellen Computer aktualisiert.

## <a name="downgrade-edition"></a>Downgrade der Edition

  > [!WARNING]
  > - **Ein Downgrade der SQL Server-Edition erfordert das vollständige Deinstallieren von SQL Server, wodurch zusätzliche Ausfallzeiten entstehen können**. 


Für ein Downgrade der SQL Server-Edition müssen Sie SQL Server vollständig deinstallieren und dann mit den Setupmedien für die gewünschte Edition neu installieren. 

Führen Sie für ein Downgrade der SQL Server-Edition die folgenden Schritte aus:

1. Sichern Sie alle Datenbanken, einschließlich der Systemdatenbanken. 
1. Verschieben Sie die Systemdatenbanken (Master, Modell und MSDB) an einen neuen Speicherort. 
1. Führen Sie eine vollständige Deinstallation von SQL Server und aller zugeordneten Dienste durch. 
1. Starten Sie den virtuellen Computer neu. 
1. Installieren Sie SQL Server mithilfe der Medien mit der gewünschten SQL Server-Edition.
1. Installieren Sie die neuesten Service Packs und kumulativen Updates.  
1. Ersetzen Sie die neuen Systemdatenbanken, die während der Installation erstellt wurden, durch die Systemdatenbanken, die Sie zuvor an einen anderen Speicherort verschoben hatten. 

Nachdem Sie ein Downgrade der SQL Server-Edition durchgeführt haben, sollten Sie die Eigenschaft „Edition“ des virtuellen SQL-Computers im Azure-Portal wie oben gezeigt ändern. Dadurch werden die Metadaten und die Abrechnung für diesen virtuellen Computer aktualisiert.

## <a name="remarks"></a>Anmerkungen

 - Die Editionseigenschaft für die SQL Server-VM muss der auf dem virtuellen Computer installierten SQL Server-Edition für alle virtuellen SQL-Computer, einschließlich der beiden Lizenztypen PAYG und BYOL, entsprechen.
 - Wenn Sie Ihre SQL Server-VM-Ressource verwerfen, kehren Sie wieder zur hartcodierten Editionseinstellung des Images zurück.
  - Die Möglichkeit, die Edition zu ändern, ist ein Feature des SQL-VM-Ressourcenanbieters. Wird ein Marketplace-Image über das Azure-Portal bereitgestellt, wird automatisch eine SQL Server-VM mit dem Ressourcenanbieter registriert. Kunden, die SQL Server selbst installieren, müssen [ihre SQL Server-VM jedoch manuell registrieren](virtual-machines-windows-sql-register-with-resource-provider.md).
- Beim Hinzufügen einer SQL Server-VM zu einer Verfügbarkeitsgruppe muss die VM erneut erstellt werden. Daher werden VMs, die einer Verfügbarkeitsgruppe hinzugefügt wurden, auf die Standardedition zurückgesetzt, und die Edition muss erneut geändert werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln: 

* [Übersicht über SQL Server auf einem virtuellen Windows-Computer](virtual-machines-windows-sql-server-iaas-overview.md)
* [Häufig gestellte Fragen zu SQL Server auf virtuellen Windows-Computern in Azure](virtual-machines-windows-sql-server-iaas-faq.md)
* [Preisinformationen für Azure-VMs mit SQL Server](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server on Azure Virtual Machine release notes](virtual-machines-windows-sql-server-iaas-release-notes.md) (SQL Server auf virtuellem Azure-Computer – Versionshinweise)



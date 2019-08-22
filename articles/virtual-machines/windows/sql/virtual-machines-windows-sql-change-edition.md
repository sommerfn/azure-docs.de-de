---
title: Ausführen eines direkten Upgrades einer SQL Server-Edition auf einer Azure-VM | Microsoft-Dokumentation
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
ms.openlocfilehash: 04e447b7d8da1c8769239aee7650fe3bc5585590
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2019
ms.locfileid: "68855229"
---
# <a name="perform-an-in-place-upgrade-of-a-sql-server-edition-on-an-azure-vm"></a>Ausführen eines direkten Upgrades einer SQL Server-Edition auf einer Azure-VM

In diesem Artikel wird beschrieben, wie Sie die Edition von SQL Server auf einem virtuellen Windows-Computer in Azure ändern. 

Die Edition von SQL Server richtet sich nach dem Product Key und wird bei der Installation angegeben. Die Edition bestimmt, welche [Features](/sql/sql-server/editions-and-components-of-sql-server-2017) im SQL Server-Produkt verfügbar sind. Sie können die SQL Server-Edition mit den Installationsmedien ändern und entweder ein Downgrade durchführen, um die Kosten zu senken, oder ein Upgrade durchführen, um weitere Features zu aktivieren.

Wenn Sie die Edition von SQL Server mithilfe der Installationsmedien nach der Registrierung beim SQL-VM-Ressourcenanbieter aktualisiert haben, sollten Sie anschließend zum entsprechenden Aktualisieren der Azure-Abrechnung die Eigenschaft für die SQL Server-Edition der SQL-VM-Ressource wie folgt festlegen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
1. Navigieren Sie zu Ihrer SQL Server-VM-Ressource. 
1. Wählen Sie **Konfigurieren** unter **Einstellungen** aus. Wählen Sie dann in der Dropdownliste unter **Edition** die gewünschte Edition von SQL Server aus. 

   ![Ändern von Editionsmetadaten](media/virtual-machines-windows-sql-change-edition/edition-change-in-portal.png)

1. Nehmen Sie die Warnung zur Kenntnis, dass Sie zuerst die SQL Server-Edition ändern müssen und dass die Eigenschaft „Edition“ der SQL Server-Edition entsprechen muss. 
1. Wählen Sie **Übernehmen** aus, um die Änderungen der Editionsmetadaten anzuwenden. 


## <a name="prerequisites"></a>Voraussetzungen

Für eine direkte Änderung der Edition von SQL Server benötigen Sie Folgendes: 

- Ein [Azure-Abonnement](https://azure.microsoft.com/free/).
- Eine [SQL Server-VM unter Windows](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision), die beim [SQL-VM-Ressourcenanbieter](virtual-machines-windows-sql-register-with-resource-provider.md) registriert ist.
- Setupmedien mit der gewünschten Edition von SQL Server. Kunden mit [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default) können Ihre Installationsmedien im [Microsoft Business Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx) abrufen. Kunden ohne Software Assurance können die Setupmedien in einem SQL Server-VM-Image aus Azure Marketplace mit der gewünschten Edition verwenden.


## <a name="upgrade-an-edition"></a>Upgrade einer Edition

> [!WARNING]
> Durch ein Upgrade der SQL Server-Edition werden der Dienst für SQL Server sowie alle zugeordneten Dienste, z. B. Analysis Services und R Services, neu gestartet. 

Für ein Upgrade von SQL Server beschaffen Sie sich die SQL Server-Setupmedien für die gewünschte Edition, und führen Sie dann die folgenden Schritte aus:

1. Rufen Sie in den SQL Server-Installationsmedien „Setup.exe“ auf. 
1. Navigieren Sie zu **Wartung**, und wählen Sie die Option **Editionsupgrade** aus. 

   ![Auswahl für das Upgrade der Edition von SQL Server](media/virtual-machines-windows-sql-change-edition/edition-upgrade.png)

1. Klicken Sie auf **Weiter**, bis Sie zur Seite **Die Edition kann jetzt aktualisiert werden** gelangen, und wählen Sie dann **Update** aus. Das Setupfenster reagiert möglicherweise einige Minuten nicht mehr, während die Änderung wirksam wird. Auf der Seite **Abgeschlossen** wird bestätigt, dass das Upgrade Ihrer Edition beendet wurde. 

Nach dem Upgrade der SQL Server-Edition ändern Sie die Eigenschaft „Edition“ der SQL Server-VM im Azure-Portal wie zuvor beschrieben. Dadurch werden die mit dieser VM verbundenen Metadaten und Abrechnungen aktualisiert.

## <a name="downgrade-an-edition"></a>Downgrade einer Edition

Für ein Downgrade der SQL Server-Edition müssen Sie SQL Server vollständig deinstallieren und dann mithilfe der Setupmedien für die gewünschte Edition neu installieren.

> [!WARNING]
> Das Deinstallieren von SQL Server kann zu zusätzlichen Ausfallzeiten führen. 

Führen Sie für ein Downgrade der SQL Server-Edition die folgenden Schritte aus:

1. Sichern Sie alle Datenbanken, einschließlich der Systemdatenbanken. 
1. Verschieben Sie die Systemdatenbanken (Master, Modell und MSDB) an einen neuen Speicherort. 
1. Führen Sie eine vollständige Deinstallation von SQL Server und aller zugeordneten Dienste durch. 
1. Starten Sie den virtuellen Computer neu. 
1. Installieren Sie SQL Server mithilfe der Medien mit der gewünschten SQL Server-Edition.
1. Installieren Sie die neuesten Service Packs und kumulativen Updates.  
1. Ersetzen Sie die neuen Systemdatenbanken, die während der Installation erstellt wurden, durch die Systemdatenbanken, die Sie zuvor an einen anderen Speicherort verschoben hatten. 

Nach dem Downgrade der SQL Server-Edition ändern Sie die Eigenschaft „Edition“ der SQL Server-VM im Azure-Portal wie zuvor beschrieben. Dadurch werden die mit dieser VM verbundenen Metadaten und Abrechnungen aktualisiert.

## <a name="remarks"></a>Anmerkungen

- Die Eigenschaft „Edition“ der SQL Server-VM muss mit der Edition der SQL Server-Instanz übereinstimmen, die für alle SQL Server-VMs installiert ist, einschließlich der Lizenztypen „Nutzungsbasierte Bezahlung“ und „Bring Your Own License (BYOL)“.
- Wenn Sie Ihre SQL Server-VM-Ressource verwerfen, kehren Sie wieder zur hartcodierten Editionseinstellung des Images zurück.
- Die Möglichkeit, die Edition zu ändern, ist ein Feature des SQL-VM-Ressourcenanbieters. Bei der Bereitstellung eines Azure Marketplace-Images über das Azure-Portal wird eine SQL Server-VM automatisch beim Ressourcenanbieter registriert. Kunden, die SQL Server selbst installieren, müssen [ihre SQL Server-VM jedoch manuell registrieren](virtual-machines-windows-sql-register-with-resource-provider.md).
- Das Hinzufügen einer SQL Server-VM zu einem Verfügbarkeitssatz erfordert die Neuerstellung der VM. Alle einer Verfügbarkeitsgruppe hinzugefügten VMs werden auf die Standardedition zurückgesetzt, und die Edition muss erneut geändert werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln: 

* [Übersicht über SQL Server auf einem virtuellen Windows-Computer](virtual-machines-windows-sql-server-iaas-overview.md)
* [Häufig gestellte Fragen zu SQL Server auf virtuellen Windows-Computern](virtual-machines-windows-sql-server-iaas-faq.md)
* [Preisinformationen zu SQL Server auf einer Windows-VM](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server auf Azure-VMs – Versionshinweise](virtual-machines-windows-sql-server-iaas-release-notes.md)



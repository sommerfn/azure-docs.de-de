---
title: Verwenden von Azure-Schnellstartvorlagen zum Konfigurieren von Always On-Verfügbarkeitsgruppen für SQL Server auf einer Azure VM
description: Verwenden Sie Azure-Schnellstartvorlagen, um den Windows-Failovercluster zu erstellen, SQL Server-VMs in diesen einzubinden, den Listener zu erstellen und den internen Lastenausgleich in Azure zu konfigurieren.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/04/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 2b44b51da11bc1c51fcbc60992a9b5b870daf02e
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70100580"
---
# <a name="use-azure-quickstart-templates-to-configure-an-always-on-availability-group-for-sql-server-on-an-azure-vm"></a>Verwenden von Azure-Schnellstartvorlagen zum Konfigurieren von Always On-Verfügbarkeitsgruppen für SQL Server auf einer Azure VM
In diesem Artikel erfahren Sie, wie Sie die Bereitstellung einer Always On-Verfügbarkeitsgruppenkonfiguration für virtuelle SQL Server-Computer in Azure mithilfe von Azure-Schnellstartvorlagen teilweise automatisieren. Bei diesem Prozess werden zwei Azure-Schnellstartvorlagen verwendet: 

   | Vorlage | BESCHREIBUNG |
   | --- | --- |
   | [101-sql-vm-ag-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-ag-setup) | Erstellt den Windows-Failovercluster und bindet die SQL Server-VMs in diesen Cluster ein. |
   | [101-sql-vm-aglistener-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-aglistener-setup) | Erstellt den Verfügbarkeitsgruppenlistener und konfiguriert den internen Lastenausgleich. Diese Vorlage kann nur verwendet werden, wenn der Windows-Failovercluster mit der Vorlage **101-sql-vm-ag-setup** erstellt wurde. |
   | &nbsp; | &nbsp; |

Andere Aufgaben der Verfügbarkeitsgruppenkonfiguration müssen manuell ausgeführt werden – etwa die Erstellung der Verfügbarkeitsgruppe und des internen Lastenausgleichs. Dieser Artikel enthält die Abfolge der automatisierten und manuellen Schritte.
 

## <a name="prerequisites"></a>Voraussetzungen 
Wenn Sie die Einrichtung einer Always On-Verfügbarkeitsgruppe mithilfe von Schnellstartvorlagen automatisieren möchten, muss Folgendes vorhanden sein: 
- Ein [Azure-Abonnement](https://azure.microsoft.com/free/).
- Eine Ressourcengruppe mit einem Domänencontroller. 
- Mindestens ein in eine Domäne eingebundener [virtueller Computer in Azure mit der Enterprise Edition von SQL Server 2016 (oder höher)](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision), der sich in der gleichen Verfügbarkeitsgruppe oder -zone befindet und die [beim SQL-VM-Ressourcenanbieter registriert](virtual-machines-windows-sql-register-with-resource-provider.md) wurde.  
- Zwei verfügbare (von keiner Entität verwendete ) IP-Adressen, eine für den internen Lastenausgleich und eine für den Verfügbarkeitsgruppenlistener im gleichen Subnetz wie die Verfügbarkeitsgruppe. Wenn ein vorhandener Lastenausgleich verwendet wird, wird nur eine verfügbare IP-Adresse benötigt.  

## <a name="permissions"></a>Berechtigungen
Die folgenden Berechtigungen sind erforderlich, um die Always On-Verfügbarkeitsgruppe mithilfe von Azure-Schnellstartvorlagen zu konfigurieren: 

- Ein vorhandenes Domänenbenutzerkonto mit der Berechtigung zum **Erstellen von Computerobjekten** in der Domäne.  Beispielsweise verfügt ein Domänenadministratorkonto in der Regel über ausreichende Berechtigungen (Beispiel: account@domain.com). _Dieses Konto muss auch Teil der lokalen Administratorgruppe auf allen virtuellen Computern sein, um den Cluster zu erstellen._
- Das Domänenbenutzerkonto, das den SQL Server-Dienst steuert. 


## <a name="step-1-create-the-failover-cluster-and-join-sql-server-vms-to-the-cluster-by-using-a-quickstart-template"></a>Schritt 1: Erstellen des Failoverclusters und Einbinden von SQL Server-VMs in den Cluster mithilfe einer Schnellstartvorlage 
Nachdem Sie Ihre SQL Server-VMs beim neuen SQL-VM-Ressourcenanbieter registriert haben, können Sie Ihre SQL Server-VMs in *SqlVirtualMachineGroups* einbinden. Diese Ressource definiert die Metadaten des Windows-Failoverclusters. Zu den Metadaten zählen die Version, Edition, der vollqualifizierte Domänennamen, die Active Directory-Konten zum Verwalten des Clusters und des SQL Server-Diensts und das Speicherkonto als Cloudzeuge. 

Beim Hinzufügen der SQL Server-VMs zu *SqlVirtualMachineGroups* wird ein Bootstrapvorgang für den Windows-Failoverclusterdienst ausgeführt, um den Cluster zu erstellen und die SQL Server-VMs in den Cluster einzubinden. Dieser Schritt wird mit der Schnellstartvorlage  **101-sql-vm-ag-setup** automatisiert. Sie können sie mithilfe der folgenden Schritte implementieren:

1. Wechseln Sie zur Schnellstartvorlage [**101-sql-vm-ag-setup**](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-ag-setup). Wählen Sie dann **Bereitstellung in Azure** aus, um die Schnellstartvorlage im Azure-Portal zu öffnen.
1. Füllen Sie die Pflichtfelder aus, um die Metadaten für den Windows-Failovercluster zu konfigurieren. Sie können die optionalen Felder leer lassen.

   Die folgende Tabelle enthält die für die Vorlage erforderlichen Werte: 

   | **Feld** | Wert |
   | --- | --- |
   | **Abonnement** |  Das Abonnement, in dem sich Ihre SQL Server-VMs befinden. |
   |**Ressourcengruppe** | Die Ressourcengruppe, in der sich Ihre SQL Server-VMs befinden. | 
   |**Failover Cluster Name** (Name des Failoverclusters) | Der gewünschte Name für Ihren neuen Windows-Failovercluster. |
   | **Existing Vm List** (Liste vorhandener VMs) | Die SQL Server-VMs, die der Verfügbarkeitsgruppe (und damit dem neuen Cluster) angehören sollen. Trennen Sie die einzelnen Werte jeweils durch ein Komma und ein Leerzeichen. (Beispiel: *SQLVM1, SQLVM2*). |
   | **SQL Server-Version** | Die SQL Server-Version Ihrer SQL Server-VMs. Wählen Sie sie in der Dropdownliste aus. Derzeit werden nur SQL Server 2016- und SQL Server 2017-Images unterstützt. |
   | **Existing Fully Qualified Domain Name** (Vorhandener vollqualifizierter Domänenname) | Der vorhandene FQDN für die Domäne, in der sich Ihre SQL Server-VMs befinden. |
   | **Existing Domain Account** (Vorhandenes Domänenkonto) | Ein vorhandenes Domänenbenutzerkonto mit der Berechtigung zum **Erstellen von Computerobjekten** in der Domäne als [CNO](/windows-server/failover-clustering/prestage-cluster-adds) wird während der Vorlagenbereitstellung erstellt. Beispielsweise verfügt ein Domänenadministratorkonto in der Regel über ausreichende Berechtigungen (Beispiel: account@domain.com). *Dieses Konto muss auch Teil der lokalen Administratorgruppe auf allen virtuellen Computern sein, um den Cluster zu erstellen.*| 
   | **Domain Account Password** (Domänenkontokennwort) | Das Kennwort für das zuvor erwähnte Domänenbenutzerkonto. | 
   | **Existing Sql Service Account** (Vorhandenes SQL-Dienstkonto) | Das Domänenbenutzerkonto, das den [SQL Server-Dienst](/sql/database-engine/configure-windows/configure-windows-service-accounts-and-permissions) bei der Bereitstellung der Verfügbarkeitsgruppe steuert (Beispiel: account@domain.com). |
   | **Sql Service Password** (SQL-Dienstkennwort) | Das Kennwort für das Domänenbenutzerkonto zum Steuern des SQL Server-Diensts. |
   | **Cloud Witness Name** (Name des Cloudzeugen) | Ein neues Azure Storage-Konto, das für den Cloudzeugen erstellt und verwendet wird. Sie können diesen Namen ändern. |
   | **\_artifacts Location** („_artifacts“-Speicherort) | Dieses Feld ist standardmäßig festgelegt und sollte nicht geändert werden. |
   | **\_artifacts Location Sas Token** (SAS-Token für „_artifacts“-Speicherort): | Dieses Feld bleibt absichtlich leer. |
   | &nbsp; | &nbsp; |

1. Wenn Sie den Geschäftsbedingungen zustimmen, aktivieren Sie das Kontrollkästchen **Ich stimme den oben genannten Geschäftsbedingungen zu**. Klicken Sie dann auf **Kaufen**, um die Bereitstellung der Schnellstartvorlage abzuschließen. 
1. Um Ihre Bereitstellung zu überwachen, klicken Sie entweder im oberen Navigationsbanner auf das Glockensymbol **Benachrichtigungen**, oder wechseln Sie im Azure-Portal zu **Ressourcengruppe**. Wählen Sie unter **Einstellungen** die Option **Bereitstellungen** und dann die Bereitstellung **Microsoft.Template** aus. 

>[!NOTE]
> Während der Vorlagenbereitstellung angegebene Anmeldeinformationen werden nur für die Dauer der Bereitstellung gespeichert. Nach Abschluss der Bereitstellung werden diese Kennwörter entfernt. Wenn Sie dem Cluster weitere SQL Server-VMs hinzufügen, werden Sie aufgefordert, sie erneut anzugeben. 


## <a name="step-2-manually-create-the-availability-group"></a>Schritt 2: Manuelles Erstellen der Verfügbarkeitsgruppe 
Erstellen Sie die Verfügbarkeitsgruppe wie gewohnt manuell mithilfe von [SQL Server Management Studio](/sql/database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio), [PowerShell](/sql/database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell) oder [Transact-SQL](/sql/database-engine/availability-groups/windows/create-an-availability-group-transact-sql). 

>[!IMPORTANT]
> Erstellen Sie noch *keinen* Listener, da dies in Schritt 4 mithilfe der Vorlage **101-sql-vm-aglistener-setup** automatisch erfolgt. 

## <a name="step-3-manually-create-the-internal-load-balancer"></a>Schritt 3: Manuelles Erstellen des internen Lastenausgleichs
Für den Always On-Verfügbarkeitsgruppenlistener ist eine interne Azure Load Balancer-Instanz erforderlich. Der interne Lastenausgleich stellt eine Floating IP-Adresse für den Verfügbarkeitsgruppenlistener bereit, um Failovervorgänge und Verbindungswiederherstellungen zu beschleunigen. Wenn die SQL Server-VMs in einer Verfügbarkeitsgruppe Teil des gleichen Verfügbarkeitssatzes sind, können Sie einen Lastenausgleich im Tarif „Basic“ verwenden. Andernfalls benötigen einen Lastenausgleich im Tarif „Standard“. 

> [!IMPORTANT]
> Der interne Load Balancer muss sich im selben virtuellen Netzwerk befinden wie die SQL Server VM-Instanzen. 

Sie müssen nur den internen Load Balancer erstellen. Die übrige Konfiguration (wie Back-End-Pool, Integritätstest und Lastenausgleichsregeln) erfolgt in Schritt 4 der Schnellstartvorlage **101-sql-vm-aglistener-setup**. 

1. Öffnen Sie im Azure-Portal die Ressourcengruppe mit den virtuellen SQL Server-Computern. 
2. Wählen Sie in der Ressourcengruppe **Hinzufügen** aus.
3. Suchen Sie nach **Load Balancer**. Wählen Sie in den Suchergebnissen den (von **Microsoft** veröffentlichten) **Load Balancer** aus.
4. Klicken Sie auf dem Blatt **Load Balancer** auf **Erstellen**.
5. Konfigurieren Sie den Load Balancer unter **Lastenausgleich erstellen** wie folgt:

   | Einstellung | Wert |
   | --- | --- |
   | **Name** |Geben Sie einen Namen ein, der den Load Balancer angibt. Geben Sie z. B. **sqlLB** ein. |
   | **Typ** |**Intern:** Die meisten Implementierungen verwenden einen internen Lastenausgleich, wodurch Anwendungen innerhalb des gleichen virtuellen Netzwerks eine Verbindung mit der Verfügbarkeitsgruppe herstellen können.  </br> **Extern:** Dieser Typ ermöglicht Anwendungen, über eine öffentliche Internetverbindung eine Verbindung mit der Verfügbarkeitsgruppe herzustellen. |
   | **Virtuelles Netzwerk** | Wählen Sie das virtuelle Netzwerk aus, in dem sich die SQL Server-Instanzen befinden. |
   | **Subnetz** | Wählen Sie das Subnetz aus, in dem sich die SQL Server-Instanzen befinden. |
   | **IP-Adresszuweisung** |**Statisch** |
   | **Private IP-Adresse** | Geben Sie eine verfügbare IP-Adresse aus dem Subnetz an. |
   | **Abonnement** |Dieses Feld wird unter Umständen angezeigt, wenn Sie über mehrere Abonnements verfügen. Wählen Sie das Abonnement aus, mit dem diese Ressource verknüpft werden soll. Hierbei handelt es sich üblicherweise um das gleiche Abonnement wie bei allen Ressourcen für die Verfügbarkeitsgruppe. |
   | **Ressourcengruppe** |Wählen Sie die Ressourcengruppe aus, in der sich die SQL Server-Instanzen befinden. |
   | **Location** |Wählen Sie die Azure-Region aus, in der sich die SQL Server-Instanzen befinden. |
   | &nbsp; | &nbsp; |

6. Klicken Sie auf **Erstellen**. 


>[!IMPORTANT]
> Die öffentliche IP-Adressressource für die einzelnen SQL Server-VMs muss über eine Standard-SKU verfügen, um mit dem Load Balancer „Standard“ kompatibel zu sein. Um die SKU der öffentlichen IP-Ressource Ihrer VM zu ermitteln, navigieren Sie zu Ihrer **Ressourcengruppe**, und wählen Sie für die SQL Server-VM die Ressource **Öffentliche IP-Adresse** aus. Der Wert befindet sich im Bereich **Übersicht** unter **SKU**. 

## <a name="step-4-create-the-availability-group-listener-and-configure-the-internal-load-balancer-by-using-the-quickstart-template"></a>Schritt 4: Erstellen des Verfügbarkeitsgruppenlisteners und Konfigurieren des internen Lastenausgleichs mithilfe der Schnellstartvorlage

Erstellen Sie den Verfügbarkeitsgruppenlistener, und konfigurieren Sie den internen Lastenausgleich automatisch mithilfe der Schnellstartvorlage **101-sql-vm-aglistener-setup**. Die Vorlage stellt die Ressource Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups/AvailabilityGroupListener bereit. Die Schnellstartvorlage **101-sql-vm-aglistener-setup** führt über den SQL-VM-Ressourcenanbieter folgende Aktionen aus:

- Eine neue Front-End-IP-Ressource (basierend auf der bei der Bereitstellung angegebenen IP-Adresse) für den Listener wird erstellt. 
- Konfiguriert die Netzwerkeinstellungen für den Cluster und internen Lastenausgleich. 
- Konfiguriert den Back-End-Pool für den internen Lastenausgleich, den Integritätstest und die Lastenausgleichsregeln.
- Erstellt den Verfügbarkeitsgruppenlistener mit der angegebenen IP-Adresse und dem angegebenen Namen.
 
>[!NOTE]
> Sie können **101-sql-vm-aglistener-setup** nur verwenden, wenn der Windows-Failovercluster mit der Vorlage **101-sql-vm-ag-setup** erstellt wurde.
   
   
Gehen Sie folgendermaßen vor, um den internen Lastenausgleich zu konfigurieren und den Verfügbarkeitsgruppenlistener zu erstellen:
1. Navigieren Sie zur Schnellstartvorlage [101-sql-vm-aglistener-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-aglistener-setup), und wählen Sie **Bereitstellung in Azure**  aus, um die Schnellstartvorlage im Azure-Portal zu starten.
1. Füllen Sie die Pflichtfelder aus, um den internen Lastenausgleich zu konfigurieren, und erstellen Sie den Verfügbarkeitsgruppenlistener. Sie können die optionalen Felder leer lassen. 

   Die folgende Tabelle enthält die für die Vorlage erforderlichen Werte: 

   | **Feld** | Wert |
   | --- | --- |
   |**Ressourcengruppe** | Die Ressourcengruppe, in der sich Ihre SQL Server-VMs und die Verfügbarkeitsgruppe befinden. | 
   |**Existing Failover Cluster Name** (Name des vorhandenen Failoverclusters) | Der Name des Clusters, in den Ihre SQL Server-VMs eingebunden sind. |
   | **Existing Sql Availability Group** (Vorhandene SQL-Verfügbarkeitsgruppe)| Der Name der Verfügbarkeitsgruppe, der Ihre SQL Server-VMs angehören. |
   | **Existing Vm List** (Liste vorhandener VMs) | Die Namen der SQL Server-VMs, die der zuvor erwähnten Verfügbarkeitsgruppe angehören. Trennen Sie die Namen jeweils durch ein Komma und ein Leerzeichen. (Beispiel: *SQLVM1, SQLVM2*). |
   | **Listener** | Der DNS-Name, den Sie dem Listener zuweisen möchten. Die Vorlage gibt standardmäßig den Namen „aglistener“ an, den Sie jedoch ändern können. Der Name sollte 15 Zeichen nicht überschreiten. |
   | **Listenerport** | Der Port, den der Listener verwenden soll. In der Regel sollte dieser Port den Standardwert 1433 haben. Dies ist die Portnummer, die von der Vorlage angegeben wird. Wenn aber Ihr Standardport geändert wurde, muss der Listenerport stattdessen auf den geänderten Wert festgelegt werden. | 
   | **Listener IP** | Die IP-Adresse, die der Listener verwenden soll. Diese Adresse wird während der Vorlagenbereitstellung erstellt. Daher sollten Sie eine angeben, die noch nicht verwendet wird.  |
   | **Existing Subnet** (Vorhandenes Subnetz) | Der Name des internen Subnetzes Ihrer SQL Server-VMs (Beispiel: *default*). Diesen Wert können Sie wie folgt ermittelt: Navigieren Sie zu Ihrer **Ressourcengruppe**, und wählen Sie Ihr virtuelles Netzwerk aus. Wählen Sie anschließend im Bereich **Einstellungen** die Option **Subnetze** aus, und kopieren Sie den Wert unter **Name**. |
   | **Existing Internal Load Balancer** (Vorhandener interner Lastenausgleich) | Der Name des internen Lastenausgleichs, den Sie in Schritt 3 erstellt haben. |
   | **Testport** | Der Testport, der vom internen Lastenausgleich verwendet werden soll. Die Vorlage verwendet standardmäßig den Wert 59999, den Sie jedoch ändern können. |
   | &nbsp; | &nbsp; |

1. Wenn Sie den Geschäftsbedingungen zustimmen, aktivieren Sie das Kontrollkästchen **Ich stimme den oben genannten Geschäftsbedingungen zu**. Klicken Sie auf **Kaufen**, um die Bereitstellung der Schnellstartvorlage abzuschließen. 
1. Um Ihre Bereitstellung zu überwachen, klicken Sie entweder im oberen Navigationsbanner auf das Glockensymbol **Benachrichtigungen**, oder wechseln Sie im Azure-Portal zu **Ressourcengruppe**. Wählen Sie unter **Einstellungen** die Option **Bereitstellungen** und dann die Bereitstellung **Microsoft.Template** aus. 

>[!NOTE]
>Sollte nach der Hälfte des Bereitstellungsvorgangs ein Fehler auftreten, müssen Sie [den neu erstellten Listener manuell mithilfe von PowerShell entfernen](#remove-the-availability-group-listener), bevor Sie die Schnellstartvorlage **101-sql-vm-aglistener-setup** erneut bereitstellen. 

## <a name="remove-the-availability-group-listener"></a>Entfernen des Verfügbarkeitsgruppenlisteners
Wenn Sie den durch die Vorlage konfigurierten Verfügbarkeitsgruppenlistener später entfernen möchten, muss dies über den SQL-VM-Ressourcenanbieter erfolgen. Da der Listener über den SQL-VM-Ressourcenanbieter registriert wurde, reicht das Löschen über SQL Server Management Studio nicht aus. 

Die beste Methode besteht darin, ihn über den SQL-VM-Ressourcenanbieter zu löschen, indem Sie in PowerShell den folgenden Codeausschnitt verwenden. Dadurch werden die Metadaten des Verfügbarkeitsgruppenlisteners aus dem SQL-VM-Ressourcenanbieter entfernt. Außerdem wird der Listener physisch aus der Verfügbarkeitsgruppe gelöscht. 

```PowerShell
# Remove the availability group listener
# example: Remove-AzResource -ResourceId '/subscriptions/a1a11a11-1a1a-aa11-aa11-1aa1a11aa11a/resourceGroups/SQLAG-RG/providers/Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups/Cluster/availabilitygrouplisteners/aglistener' -Force
Remove-AzResource -ResourceId '/subscriptions/<SubscriptionID>/resourceGroups/<resource-group-name>/providers/Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups/<cluster-name>/availabilitygrouplisteners/<listener-name>' -Force
```
 
## <a name="common-errors"></a>Häufige Fehler
In diesem Abschnitt werden einige bekannte Probleme und mögliche Lösungen behandelt. 

### <a name="availability-group-listener-for-availability-group-ag-name-already-exists"></a>Availability group listener for availability group '\<AG-Name>' already exists (Der Verfügbarkeitsgruppenlistener für die Verfügbarkeitsgruppe „<Name der Verfügbarkeitsgruppe>“ ist bereits vorhanden.)
Die ausgewählte Verfügbarkeitsgruppe, die in der Azure-Schnellstartvorlage für den Verfügbarkeitsgruppenlistener verwendet wird, enthält bereits einen Listener. Er befindet sich entweder physisch innerhalb der Verfügbarkeitsgruppe, oder seine Metadaten verbleiben innerhalb des SQL-VM-Ressourcenanbieters. Entfernen Sie den Listener mithilfe von [PowerShell](#remove-the-availability-group-listener), bevor Sie die Schnellstartvorlage **101-sql-vm-aglistener-setup** erneut bereitstellen. 

### <a name="connection-only-works-from-primary-replica"></a>Connection only works from primary replica (Die Verbindung funktioniert nur vom primären Replikat aus.)
Dieses Verhalten ist wahrscheinlich auf eine fehlgeschlagene Bereitstellung der Vorlage **101-sql-vm-aglistener-setup** zurückzuführen, die die Konfiguration des internen Lastenausgleichs in einen inkonsistenten Zustand versetzt hat. Vergewissern Sie sich, dass die Verfügbarkeitsgruppe im Back-End-Pool aufgeführt ist und dass Regeln für den Integritätstest und für den Lastenausgleich vorhanden sind. Wenn etwas fehlt, hat die Konfiguration des internen Load Balancers ein inkonsistenter Zustand. 

Entfernen Sie zur Behebung dieses Problems den Listener mithilfe von [PowerShell](#remove-the-availability-group-listener), löschen Sie den internen Lastenausgleich im Azure-Portal, und beginnen Sie erneut bei Schritt 3. 

### <a name="badrequest---only-sql-virtual-machine-list-can-be-updated"></a>BadRequest - Only SQL virtual machine list can be updated (Fehlerhafte Anforderung: Nur die SQL-VM-Liste kann aktualisiert werden.)
Dieser Fehler kann beim Bereitstellen der Vorlage **101-sql-vm-aglistener-setup** auftreten, wenn der Listener zwar über SQL Server Management Studio (SSMS), aber nicht aus dem SQL-VM-Ressourcenanbieter gelöscht wurde. Wenn Sie den Listener über SSMS löschen, werden die Metadaten des Listener nicht aus dem SQL-VM-Ressourcenanbieter entfernt. Der Listener muss über [PowerShell](#remove-the-availability-group-listener) aus dem Ressourcenanbieter gelöscht werden. 

### <a name="domain-account-does-not-exist"></a>Domain account does not exist (Domänenkonto nicht vorhanden)
Dieser Fehler kann zwei Gründe haben. Entweder ist das angegebene Domänenkonto nicht vorhanden, oder es fehlen die [UPN-Daten](/windows/desktop/ad/naming-properties#userprincipalname) (User Principal Name, Benutzerprinzipalname). Die Vorlage **101-sql-vm-ag-setup** erwartet ein Domänenkonto im UPN-Format (d. h. *user@domain.com* ), aber bei einigen Domänenkonten fehlt dieses möglicherweise. Dies geschieht meist, wenn ein lokaler Benutzer zum ersten Domänenadministratorkonto migriert wurde, als der Server zu einem Domänencontroller heraufgestuft wurde, oder wenn ein Benutzer über PowerShell erstellt wurde. 

Überprüfen Sie, ob das Konto vorhanden ist. Falls nicht, kann die zweite Situation auftreten. Führen Sie folgende Schritte aus, um dieses Problem zu beheben:

1. Öffnen Sie auf dem Domänencontroller das Fenster **Active Directory-Benutzer und -Computer** über die Option **Extras** im **Server-Manager**. 
2. Navigieren Sie zum Konto, indem Sie im linken Bereich **Benutzer** auswählen.
3. Klicken Sie mit der rechten Maustaste auf das Konto, und wählen Sie **Eigenschaften** aus.
4. Klicken Sie auf die Registerkarte **Konto**. Wenn das Feld **Benutzeranmeldename** leer ist, ist dies die Ursache des Fehlers. 

    ![Ein leeres Benutzerkonto deutet auf einen fehlenden UPN hin.](media/virtual-machines-windows-sql-availability-group-quickstart-template/account-missing-upn.png)

5. Geben Sie als **Benutzeranmeldename** den Namen des Benutzers ein, und wählen Sie in der Dropdownliste die passende Domäne aus. 
6. Wählen Sie **Übernehmen** aus, um die Änderungen zu speichern, und schließen Sie das Dialogfeld, indem Sie **OK** auswählen. 

Nachdem Sie diese Änderungen vorgenommen haben, versuchen Sie erneut, die Azure-Schnellstartvorlage bereitzustellen. 



## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln: 

* [Was ist SQL Server auf virtuellen Azure-Computern? (Windows)](virtual-machines-windows-sql-server-iaas-overview.md)
* [Häufig gestellte Fragen zu SQL Server auf virtuellen Windows-Computern in Azure](virtual-machines-windows-sql-server-iaas-faq.md)
* [Preisinformationen für virtuelle Azure-Computer mit SQL Server](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server auf Azure-VMs – Versionshinweise](virtual-machines-windows-sql-server-iaas-release-notes.md)
* [Ändern des Lizenzierungsmodells für eine SQL Server-VM in Azure](virtual-machines-windows-sql-ahb.md)




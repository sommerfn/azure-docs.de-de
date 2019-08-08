---
title: Erstellen eines Hostpools für die Vorschauversion von Windows Virtual Desktop zum Überprüfen von Dienstupdates – Azure
description: Erfahren Sie, wie Sie einen Hostpool für die Überwachung von Dienstupdates erstellen, bevor Updates in der Produktion bereitgestellt werden.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 05/08/2019
ms.author: helohr
ms.openlocfilehash: 732192e481f88c0cd3c78e9dac43970eb5be72e6
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68840520"
---
# <a name="tutorial-create-a-host-pool-to-validate-service-updates"></a>Tutorial: Erstellen eines Hostpools zum Überprüfen von Dienstupdates

Hostpools sind eine Sammlung identischer virtueller Computer innerhalb von Mandantenumgebungen mit Windows Virtual Desktop (Vorschauversion). Vor der Bereitstellung von Hostpools in Ihrer Produktionsumgebung empfehlen wir dringend, dass Sie einen Überprüfungshostpool erstellen. Updates werden zuerst auf Überprüfungshostpools angewendet, sodass Sie Dienstupdates überwachen können, bevor Sie sie in Ihrer Produktionsumgebung bereitstellen. Ohne einen Überprüfungshostpool können Sie keine Änderungen erkennen, die Fehler einführen, was zu Ausfallzeiten für Benutzer in der Produktionsumgebung führen könnte.

Um sicherzustellen, dass Ihre Apps mit den neuesten Updates funktionieren, sollte der Überprüfungshostpool Hostpools in Ihrer Produktionsumgebung so ähnlich wie möglich sein. Benutzer sollten so häufig eine Verbindung mit dem Überprüfungshostpool herstellen, wie sie es auch mit dem Produktionshostpool tun. Wenn Sie automatisierte Tests mit Ihrem Hostpool durchführen, sollten Sie auch beim Überprüfungshostpool automatisierte Tests durchführen.

Sie können Probleme beim Überprüfungshostpool entweder mit der [Diagnosefunktion](diagnostics-role-service.md) oder den [Artikeln zur Problembehandlung bei Windows Virtual Desktop](https://docs.microsoft.com/Azure/virtual-desktop/troubleshoot-set-up-overview) debuggen.

>[!NOTE]
> Wir empfehlen, dass Sie den Überprüfungshostpool eingerichtet lassen, um alle zukünftigen Updates zu testen.

Zur Vorbereitung müssen Sie ggf. zunächst das [Windows Virtual Desktop-PowerShell-Modul herunterladen und importieren](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview).

## <a name="create-your-host-pool"></a>Erstellen Ihres Hostpools

Sie können einen Hostpool erstellen, indem Sie die Anweisungen in diesen Artikeln befolgen:
- [Tutorial: Erstellen eines Hostpools mit Azure Marketplace](create-host-pools-azure-marketplace.md)
- [Erstellen eines Hostpools mit einer Azure Resource Manager-Vorlage](create-host-pools-arm-template.md)
- [Erstellen eines Hostpools mit PowerShell](create-host-pools-powershell.md)

## <a name="define-your-host-pool-as-a-validation-host-pool"></a>Definieren Ihres Hostpools als Überprüfungshostpool

Führen Sie die folgenden PowerShell-Cmdlets aus, um den neuen Hostpool als Überprüfungshostpool zu definieren. Ersetzen Sie die Werte in Anführungszeichen durch die für Ihre Sitzung relevanten Werte:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
Set-RdsHostPool -TenantName $myTenantName -Name "contosoHostPool" -ValidationEnv $true
```

Führen Sie das folgende PowerShell-Cmdlet aus, um zu bestätigen, dass die Überprüfungseigenschaft festgelegt ist. Ersetzen Sie die Werte in Anführungszeichen durch die für Ihre Sitzung relevanten Werte.

```powershell
Get-RdsHostPool -TenantName $myTenantName -Name "contosoHostPool"
```

Die Ergebnisse des Cmdlets sollte der folgenden Ausgabe ähneln:

```
    TenantName          : contoso 
    TenantGroupName     : Default Tenant Group
    HostPoolName        : contosoHostPool
    FriendlyName        :
    Description         :
    Persistent          : False 
    CustomRdpProperty   : use multimon:i:0;
    MaxSessionLimit     : 10
    LoadBalancerType    : BreadthFirst
    ValidationEnv       : True
    Ring                :
```

## <a name="update-schedule"></a>Zeitplan für Updates

In der Vorschauphase kommen Dienstupdates ungefähr monatlich vor. Wenn schwerwiegende Probleme vorliegen, werden kritische Updates in häufigeren Abständen bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie einen Überprüfungshostpool erstellt haben, können Sie erfahren, wie Sie ein Verwaltungstool zur Verwaltung von Microsoft Virtual Desktop-Ressourcen bereitstellen und eine Verbindung damit herstellen.

> [!div class="nextstepaction"]
> [Tutorial: Bereitstellen eines Verwaltungstools](./manage-resources-using-ui.md)

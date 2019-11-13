---
title: 'Problembehandlung von Windows Virtual Desktop: Übersicht – Azure'
description: Eine Übersicht zur Problembehandlung bei der Einrichtung einer Windows Virtual Desktop-Mandantenumgebung.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 04/08/2019
ms.author: helohr
ms.openlocfilehash: aa4254673d426579a5e5f0e90e389db7c6cadff0
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73607346"
---
# <a name="troubleshooting-overview-feedback-and-support"></a>Problembehandlung: Übersicht, Feedback und Support

Dieser Artikel gibt einen Überblick über die Probleme, die bei der Einrichtung einer Windows Virtual Desktop-Mandantenumgebung auftreten können, und beschreibt Möglichkeiten, diese Probleme zu lösen.

## <a name="provide-feedback"></a>Feedback geben

In der [Windows Virtual Desktop Tech Community](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) können Sie sich mit dem Produktteam und aktiven Communitymitgliedern über den Windows Virtual Desktop-Dienst austauschen.

## <a name="escalation-tracks"></a>Eskalationspfade

Verwenden Sie die folgende Tabelle, um Probleme zu identifizieren und zu beheben, die beim Einrichten einer Mandantenumgebung mit dem Remotedesktopclient auftreten können. Nachdem Sie Ihren Mandanten eingerichtet haben, können Sie den neuen [Diagnosedienst](https://docs.microsoft.com/azure/virtual-desktop/diagnostics-role-service) verwenden, um Probleme für häufige Szenarien zu identifizieren.

>[!NOTE]
> Wir betreiben ein Tech Community-Forum, das Sie besuchen können, um Probleme mit dem Produktteam und aktiven Communitymitgliedern zu diskutieren. Besuchen Sie die [Windows Virtual Desktop Tech Community](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop), um die folgenden Probleme zu diskutieren: 

| **Problem**                                                            | **Vorgeschlagene Lösung**  |
|----------------------------------------------------------------------|-------------------------------------------------|
| Erstellen eines Mandanten                                                    | Wenn ein Azure-Ausfall auftritt, wenden Sie sich an den [Azure-Support](https://azure.microsoft.com/support/options/). In anderen Fällen **öffnen Sie eine Supportanfrage für Windows Virtual Desktop (Compute)** .|
| Zugreifen auf Marketplace-Vorlagen im Azure-Portal       | Wenn es zu einem Azure-Ausfall kommt, wenden Sie sich an [Azure-Support](https://azure.microsoft.com/support/options/). <br> <br> Azure Marketplace virtuelle Windows-Desktop-Vorlagen sind kostenlos verfügbar.|
| Zugreifen auf Azure Resource Manager-Vorlagen aus GitHub                                  | Weitere Informationen finden Sie im Abschnitt „Erstellen von Windows Virtual Desktop-Sitzungshost-VMs“ von [Mandanten- und Hostpoolerstellung](troubleshoot-set-up-issues.md). Wenn das Problem immer noch nicht gelöst ist, wenden Sie sich an das [GitHub-Supportteam](https://github.com/contact). <br> <br> Wenn der Fehler nach dem Zugriff auf die Vorlage in GitHub auftritt, wenden Sie sich an den [Azure-Support](https://azure.microsoft.com/support/options/).|
| Einstellungen für den Sitzungshostpool von Azure Virtual Network (VNET) und Express Route               | Wenden Sie sich an den **Azure-Support (Netzwerke)** . |
| Sitzungshostpool-VM-Erstellung, wenn keine mit Windows Virtual Desktop bereitgestellten Azure Resource Manager-Vorlagen verwendet werden | Wenden Sie sich an den **Azure-Support (Compute)** . <br> <br> Informationen zu Problemen mit den Vorlagen von Azure Resource Manager, die mit Windows Virtual Desktop bereitgestellt werden, finden Sie im Abschnitt „Erstellen eines Windows Virtual Desktop-Mandanten“ unter [Mandanten- und Hostpoolerstellung](troubleshoot-set-up-issues.md). |
| Verwalten der Windows Virtual Desktop-Sitzungshostumgebung über das Azure-Portal    | Wenden Sie sich an den **Azure-Support**. <br> <br> Bei Verwaltungsproblemen bei der Verwendung von Remote Desktop Services/Windows Virtual Desktop PowerShell lesen Sie [Windows Virtual Desktop PowerShell](troubleshoot-powershell.md), oder **öffnen Sie eine Supportanfrage für Windows Virtual Desktop (Compute)** . |
| Verwalten einer Windows Virtual Desktop-Konfiguration, die an Hostpools und Anwendungsgruppen (App-Gruppen) gebunden ist      | Weitere Informationen finden Sie unter [Windows Virtual Desktop PowerShell](troubleshoot-powershell.md), oder **öffnen Sie eine Supportanfrage für Windows Virtual Desktop (Compute)** . <br> <br> Wenn Probleme mit der grafischen Beispielbenutzeroberfläche (GUI) verbunden sind, wenden Sie sich an die Yammer-Community.|
| Fehlfunktion von Remotedesktopclients beim Start                                                 | Weitere Informationen finden Sie unter [Remote Desktop-Clientverbindungen](troubleshoot-client-connection.md). Wenn das Problem weiterhin besteht, **öffnen Sie eine Supportanfrage für Windows Virtual Desktop (Compute)** .  <br> <br> Wenn es sich um ein Netzwerkproblem handelt, müssen Ihre Benutzer sich an ihren Netzwerkadministrator wenden. |
| Verbunden, aber kein Feed                                                                 | Führen Sie die Problembehandlung anhand von „Benutzer stellt eine Verbindung her, aber es wird nichts angezeigt (kein Feed)“ im Abschnitt [Remotedesktop-Clientverbindungen](troubleshoot-client-connection.md) aus. <br> <br> Wenn Ihre Benutzer einer App-Gruppe zugewiesen wurden, **öffnen Sie eine Supportanfrage für Windows Virtual Desktop (Compute)** . |
| Feedermittlungsprobleme aufgrund des Netzwerks                                            | Ihre Benutzer müssen sich an ihren Netzwerkadministrator wenden. |
| Verbinden von Clients                                                                    | Weitere Informationen finden Sie unter [Remotedesktop-Clientverbindungen](troubleshoot-client-connection.md). Wenn das Ihr Problem nicht löst, finden Sie weitere Informationen unter [Konfiguration des virtuellen Sitzungshostcomputers](troubleshoot-vm-configuration.md). |
| Reaktionsfähigkeit von Remoteanwendungen oder Desktop                                      | Wenn Probleme mit einer bestimmten Anwendung oder einem Produkt verknüpft sind, wenden Sie sich an das Team, das für das betreffende Produkt zuständig ist. |
| Lizenzierungsmeldungen oder -fehler                                                          | Wenn Probleme mit einer bestimmten Anwendung oder einem Produkt verknüpft sind, wenden Sie sich an das Team, das für das betreffende Produkt zuständig ist. |

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur Problembehandlung beim Erstellen eines Mandanten- und Hostpools in einer Windows Virtual Desktop-Umgebung finden Sie unter [Mandanten- und Hostpoolerstellung](troubleshoot-set-up-issues.md).
- Informationen zur Problembehandlung bei der Konfiguration eines virtuellen Computers (VM) in Windows Virtual Desktop finden Sie unter [Konfiguration des virtuellen Sitzungshostcomputers](troubleshoot-vm-configuration.md).
- Informationen zur Problembehandlung bei Problemen mit Windows Virtual Desktop-Clientverbindungen finden Sie unter [Remotedesktop-Clientverbindungen](troubleshoot-client-connection.md).
- Informationen zur Problembehandlung bei der Verwendung von PowerShell mit Windows Virtual Desktop finden Sie unter [Windows Virtual Desktop – PowerShell](troubleshoot-powershell.md).
- Weitere Informationen zum Dienst finden Sie unter [Windows Virtual Desktop-Umgebung](https://docs.microsoft.com/azure/virtual-desktop/environment-setup).
- Ein Tutorial zur Problembehandlung finden Sie unter [Tutorial: Problembehandlung von Bereitstellungen der Resource Manager-Vorlage](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-tutorial-troubleshoot).
- Informationen zur Überwachung von Aktionen finden Sie unter [Überwachen von Vorgängen mit Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
- Weitere Informationen zu Aktionen zum Bestimmen von Fehlern während der Bereitstellung finden Sie unter [Anzeigen von Bereitstellungsvorgängen mit dem Azure-Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-operations).

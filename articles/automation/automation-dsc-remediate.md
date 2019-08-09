---
title: Beheben von Problemen mit nicht konformen Azure Automation State Configuration-Servern
description: Es wird beschrieben, wie Sie Konfigurationen bedarfsabhängig erneut auf Server anwenden, für die sich der Konfigurationszustand geändert hat.
services: automation
ms.service: automation
ms.subservice: dsc
author: mgreenegit
ms.author: migreene
ms.topic: conceptual
ms.date: 07/17/2019
manager: nirb
ms.openlocfilehash: a6c7639cb4988eb13dfaa1c151085cda6e53b5d3
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68611357"
---
# <a name="remediate-non-compliant-dsc-servers"></a>Korrigieren nicht konformer DSC-Server

Wenn Server mit Azure Automation State Configuration registriert wurden, wird der „Konfigurationsmodus“ auf „ApplyOnly“, „ApplyandMonitor“ oder „ApplyAndAutoCorrect“ festgelegt.
Wenn der Modus nicht auf „AutoCorrect“ festgelegt ist, verbleiben Server, für die aus irgendeinem Grund der konforme Zustand nicht mehr besteht, im nicht konformen Zustand, bis der Fehler manuell behoben wird.

Azure Compute verfügt über das Feature „Befehl ausführen“, mit dem Kunden Skripts auf virtuellen Computern ausführen können.
Dieses Dokument enthält Beispielskripts für dieses Feature, die genutzt werden können, wenn Abweichungen bei der Konfiguration manuell korrigiert werden sollen.

## <a name="correct-drift-of-windows-virtual-machines-using-powershell"></a>Korrigieren von Abweichungen virtueller Windows-Computer mit PowerShell

Eine Schritt-für-Schritt-Anleitung zur Verwendung des Features „Befehl ausführen“ auf virtuellen Windows-Computern finden Sie in der Dokumentation auf der Seite [Ausführen von PowerShell-Skripts in Ihrer Windows-VM mit „Befehl ausführen“](/azure/virtual-machines/windows/run-command).

Verwenden Sie das Cmdlet `Update-DscConfiguration`, um zu erzwingen, dass ein Azure Automation State Configuration-Knoten die aktuelle Konfiguration herunterlädt und anwendet.
Ausführliche Informationen finden Sie in der Dokumentation zum Cmdlet [Update-DscConfiguration](/powershell/module/psdesiredstateconfiguration/update-dscconfiguration).

```powershell
Update-DscConfiguration -Wait -Verbose
```

## <a name="correct-drift-of-linux-virtual-machines"></a>Korrigieren von Abweichungen virtueller Linux-Computer

Für Linux-Server ist derzeit keine vergleichbare Funktion verfügbar.
Die einzige Möglichkeit besteht darin, den Registrierungsvorgang zu wiederholen.
Für Azure-Knoten können Abweichungen über das Portal oder mit Az Automation-Cmdlets korrigiert werden.
Ausführliche Informationen zu diesem Prozess sind auf der Seite [Onboarding von Computern zur Verwaltung durch Azure Automation DSC](/azure/automation/automation-dsc-onboarding#azure-portal) dokumentiert.
Für Hybridknoten kann die Korrektur von Abweichungen mit den beigefügten Python-Skripts durchgeführt werden.
Weitere Informationen finden Sie in der Dokumentation unter [PowerShell DSC for Linux Repo](https://github.com/Microsoft/PowerShell-DSC-for-Linux#performing-dsc-operations-from-the-linux-computer) (Repository für PowerShell DSC für Linux).

## <a name="next-steps"></a>Nächste Schritte

- Eine PowerShell-Cmdlet-Referenz ist unter [Azure Automation State Configuration-Cmdlets](/powershell/module/azurerm.automation/#automation) verfügbar.
- Ein Verwendungsbeispiel für Azure Automation State Configuration in einer Continuous Deployment-Pipeline finden Sie unter [Continuous Deployment mit Azure Automation State Configuration und Chocolatey](automation-dsc-cd-chocolatey.md).

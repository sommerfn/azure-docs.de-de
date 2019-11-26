---
title: Weitere Informationen zu den Orchestrierungsmodi für VM-Skalierungsgruppen in Azure
description: Erfahren Sie mehr über die Orchestrierungsmodi für VM-Skalierungsgruppen in Azure.
services: virtual-machine-scale-sets
documentationcenter: ''
author: shandilvarun
manager: gwallace
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.topic: article
ms.date: 10/23/2019
ms.author: vashan
ms.openlocfilehash: 063b3210877c06edf7eeddab37c50ed84033098a
ms.sourcegitcommit: f7f70c9bd6c2253860e346245d6e2d8a85e8a91b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73065728"
---
# <a name="orchestration-mode-preview"></a>Orchestrierungsmodus (Vorschau)

VM-Skalierungsgruppen bieten eine logische Gruppierung von plattformverwalteten virtuellen Computern. Mit Skalierungsgruppen erstellen Sie ein Konfigurationsmodell für virtuelle Computer, fügen automatisch zusätzliche Instanzen basierend auf der CPU- oder Speicherauslastung hinzu oder entfernen sie und führen automatisch ein Upgrade auf die neueste Betriebssystemversion durch. Traditionell ermöglichen Skalierungsgruppen die Erstellung virtueller Computer mit einem VM-Konfigurationsmodell, das zum Zeitpunkt der Erstellung der Skalierungsgruppen bereitgestellt wird, und die Skalierungsgruppen können nur virtuelle Computer verwalten, die implizit basierend auf dem Konfigurationsmodell erstellt werden.

Mit dem Orchestrierungsmodus für Skalierungsgruppen (Vorschauversion) können Sie nun wählen, ob die Skalierungsgruppe VMs orchestrieren soll, die explizit außerhalb eines Konfigurationsmodells für die Skalierungsgruppe erstellt wurden, oder VM-Instanzen, die implizit basierend auf dem Konfigurationsmodell erstellt wurden. Der Orchestrierungsmodus für Skalierungsgruppen hilft Ihnen auch beim Entwurf Ihrer VM-Infrastruktur für Hochverfügbarkeit, indem Sie diese VMs in Fehlerdomänen und Verfügbarkeitszonen bereitstellen.


VM-Skalierungsgruppen unterstützen zwei unterschiedliche Orchestrierungsmodi:

- ScaleSetVM: Instanzen virtueller Computer, die der Skalierungsgruppe hinzugefügt wurden, basieren auf dem Konfigurationsmodell der Skalierungsgruppe. Der Lebenszyklus der Instanz des virtuellen Computers – Erstellung, Aktualisierung, Löschung – wird über die Skalierungsgruppe verwaltet.
- VM (Virtuelle Computer): Virtuelle Computer, die außerhalb der Skalierungsgruppe erstellt wurden, können explizit der Skalierungsgruppe hinzugefügt werden. 
 

> [!IMPORTANT]
> Der Orchestrierungsmodus wird beim Erstellen der Skalierungsgruppe festgelegt und kann später nicht mehr geändert oder aktualisiert werden. 
> 
> Diese Funktion von VM-Skalierungsgruppen befindet sich derzeit in der öffentlichen Vorschauversion.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. 
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


## <a name="orchestration-modes"></a>Orchestrierungsmodi

|                             | „orchestrationMode“: „VM“ (VirtualMachine) | „orchestrationMode“: „ScaleSetVM“ (VirtualMachineScaleSetVM) |
|-----------------------------|--------------------------------------------|--------------------------------------------------------------|
| VM-Konfigurationsmodell      | Keine                                       | Erforderlich |
| Hinzufügen einer neuen VM zu einer Skalierungsgruppe  | VMs werden bei der Erstellung explizit zur Skalierungsgruppe hinzugefügt. | VMs werden implizit erstellt und der Skalierungsgruppe basierend auf dem VM-Konfigurationsmodell, der Anzahl der Instanzen und den AutoScaling-Regeln hinzugefügt. | |
| Löschen eines virtuellen Computers                   | VMs müssen einzeln gelöscht werden, die Skalierungsgruppe wird nicht gelöscht, wenn sie virtuelle Computer enthält. | VMs können einzeln gelöscht werden, das Löschen der Skalierungsgruppe löscht alle VM-Instanzen.  |
| VMs anfügen/trennen           | Nicht unterstützt                              | Nicht unterstützt |
| Lebenszyklus einer Instanz (Erstellung bis Löschung) | VMs und ihre Artefakte (wie Festplatten und NICs) können unabhängig voneinander verwaltet werden. | Instanzen und ihre Artefakte (wie Festplatten und NICs) sind implizit für die Skalierungsgruppeninstanzen, von denen sie erstellt werden. Sie können nicht getrennt oder separat außerhalb der Skalierungsgruppe verwaltet werden. |
| Fehlerdomänen               | Kann Fehlerdomänen definieren. 2 oder 3 basierend auf der regionalen Unterstützung und 5 für Verfügbarkeitszonen. | Kann Fehlerdomänen von 1 bis 5 definieren. |
| Updatedomänen              | Updatedomänen werden automatisch Fehlerdomänen zugeordnet. | Updatedomänen werden automatisch Fehlerdomänen zugeordnet. |
| Verfügbarkeitszonen          | Unterstützt die regionale Bereitstellung oder VMs in einer Verfügbarkeitszone. | Unterstützt regionale Bereitstellung oder mehrere Verfügbarkeitszonen; kann die Strategie zum Sicherstellen eines Zonengleichgewichts definieren. |
| AutoScale                   | Nicht unterstützt                              | Unterstützt |
| Betriebssystemupgrade                  | Nicht unterstützt                              | Unterstützt |
| Modellupdates               | Nicht unterstützt                              | Unterstützt |
| Instanzensteuerung            | Vollständige VM-Steuerung. VMs verfügen über einen vollqualifizierten URI, der die gesamte Bandbreite der Azure VM-Verwaltungsfunktionen unterstützt (wie Azure Policy, Azure Backup und Azure Site Recovery). | VMs sind abhängige Ressourcen der Skalierungsgruppe. Auf Instanzen kann für die Verwaltung nur über die Skalierungsgruppe zugegriffen werden. |
| Instanzenmodell              | Modelldefinition: Microsoft.Compute/VirtualMachines. | Modelldefinition: Microsoft.Compute/VirtualMachineScaleSets/VirtualMachines. |
| Capacity                    | Eine leere Skalierungsgruppe kann erstellt werden. Bis zu 200 VMs können der Skalierungsgruppe hinzugefügt werden. | Skalierungsgruppen können mit einer Instanzenanzahl von 0 bis 1000 definiert werden. |
| Move                        | Unterstützt                                  | Unterstützt |
| Einzelne Platzierungsgruppe = = false | Nicht unterstützt                          | Unterstützt |


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [Übersicht über Verfügbarkeitszonen](availability.md).

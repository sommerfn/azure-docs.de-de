---
title: Erstellen von Konfigurationen aus vorhandenen Servern – Azure Automation
description: Erfahren Sie, wie Sie Konfigurationen aus vorhandenen Servern für Azure Automation erstellen.
keywords: DSC,PowerShell,Konfiguration,Setup,Einrichtung
services: automation
ms.service: automation
ms.subservice: dsc
author: mgreenegit
ms.author: migreene
ms.date: 08/08/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: b8c39ba6c12d43da1b2311ae4d7d85dd13946f25
ms.sourcegitcommit: a6888fba33fc20cc6a850e436f8f1d300d03771f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69559211"
---
# <a name="create-configurations-from-existing-servers"></a>Erstellen von Konfigurationen aus vorhandenen Servern

> Anwendungsbereich: Windows PowerShell 5.1

Erstellen von Konfigurationen aus vorhandenen Servern kann eine Herausforderung darstellen.
Sie möchten nicht unbedingt *alle* Einstellungen, sondern nur die Einstellungen, die Sie interessieren.
Auch dann müssen Sie wissen, in welcher Reihenfolge die Einstellungen angewendet werden müssen, damit die Konfiguration erfolgreich angewendet werden kann.

> [!NOTE]
> Dieser Artikel bezieht sich auf eine Lösung, die von der Open Source-Community verwaltet wird.
> Support ist nur in Form der GitHub-Zusammenarbeit erhältlich, nicht von Microsoft.

## <a name="community-project-reversedsc"></a>Communityprojekt: ReverseDSC

Eine von der Community verwaltete Lösung namens [ReverseDSC](https://github.com/microsoft/reversedsc) wurde erstellt, um in diesem Bereich zu arbeiten, beginnend mit SharePoint.

Die Lösung baut auf der [SharePointDSC-Ressource](https://github.com/powershell/sharepointdsc) auf und erweitert sie so, dass das [Sammeln von Informationen](https://github.com/Microsoft/sharepointDSC.reverse#how-to-use) von vorhandenen SharePoint-Servern orchestriert wird.
Die neueste Version verfügt über mehrere [Extraktionsmodi](https://github.com/Microsoft/SharePointDSC.Reverse/wiki/Extraction-Modes), um die Stufe der einzuschließenden Informationen zu bestimmen.

Das Ergebnis der Verwendung der Lösung ist das Generieren von [Konfigurationsdaten](https://github.com/Microsoft/sharepointDSC.reverse#configuration-data) zur Verwendung mit SharePointDSC-Konfigurationsskripts.

Sobald die Datendateien einmal generiert sind, können Sie sie mit [DSC-Konfigurationsskripts](/powershell/dsc/overview/overview) verwenden, um MOF-Dateien zu generieren und die [MOF-Dateien in Azure Automation hochzuladen](/azure/automation/tutorial-configure-servers-desired-state#create-and-upload-a-configuration-to-azure-automation).
Danach registrieren Sie Ihre Server entweder von [lokal](/azure/automation/automation-dsc-onboarding#physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azureaws) oder [in Azure](/azure/automation/automation-dsc-onboarding#azure-virtual-machines) um Konfigurationen abzurufen.

Um ReverseDSC zu testen, besuchen Sie den [PowerShell-Katalog](https://www.powershellgallery.com/packages/ReverseDSC/), und laden Sie die Lösung herunter, oder klicken Sie auf „Projektwebsite“, um die [Dokumentation](https://github.com/Microsoft/sharepointDSC.reverse) anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

- [Windows PowerShell DSC – Übersicht](/powershell/dsc/overview/overview)
- [DSC-Ressourcen](/powershell/dsc/resources/resources)
- [Konfigurieren des lokalen Konfigurations-Managers](/powershell/dsc/managing-nodes/metaconfig)

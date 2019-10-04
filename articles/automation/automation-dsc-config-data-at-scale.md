---
title: Konfigurationsdaten nach Bedarf – Azure Automation
description: Erfahren Sie, wie Sie Daten skaliert für die Zustandskonfiguration in Azure Automation konfigurieren.
keywords: DSC,PowerShell,Konfiguration,Setup,Einrichtung
services: automation
ms.service: automation
ms.subservice: dsc
author: mgreenegit
ms.author: migreene
ms.date: 08/08/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: f6bb89370c85389d6c41306ed224d27d710cd7c8
ms.sourcegitcommit: a6888fba33fc20cc6a850e436f8f1d300d03771f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69559189"
---
# <a name="configuration-data-at-scale"></a>Konfigurationsdaten nach Bedarf

> Anwendungsbereich: Windows PowerShell 5.1

Das Verwalten von Hunderten oder Tausenden von Servern kann eine Herausforderung darstellen.
Kunden haben Feedback gegeben, dass die eigentliche Verwaltung der [Konfigurationsdaten](/powershell/dsc/configurations/configdata) der schwierigste Aspekt ist.
Das Organisieren von Informationen über logische Konstrukte wie Speicherort, Typ und Umgebung.

> [!NOTE]
> Dieser Artikel bezieht sich auf eine Lösung, die von der Open Source-Community verwaltet wird.
> Support ist nur in Form der GitHub-Zusammenarbeit erhältlich, nicht von Microsoft.

## <a name="community-project-datum"></a>Communityprojekt: Datum

Eine von der Community verwaltete Lösung namens [Datum](https://github.com/gaelcolas/Datum) wurde erstellt, um diese Herausforderung zu bewältigen.
Datum basiert auf großartigen Ideen anderer Plattformen für die Konfigurationsverwaltung und implementiert denselben Typ von Lösung für PowerShell DSC.
Die Informationen sind [in Textdateien organisiert](https://github.com/gaelcolas/Datum#3-intended-usage), die auf logischen Ideen basieren.
Beispiele wären:

- Einstellungen, die Global gelten sollen.
- Einstellungen, die für alle Server an einem Standort gelten sollen.
- Einstellungen, die für alle Datenbankserver gelten sollen.
- Individuelle Servereinstellungen

Diese Informationen sind im von Ihnen bevorzugten Dateiformat organisiert (JSON, YAML oder PSD1).
Anschließend werden Cmdlets bereitgestellt, um Konfigurationsdatendateien zu generieren, indem die [Informationen aus jeder Datei in einer einzelnen Ansicht eines Servers oder einer Serverrolle konsolidiert werden](https://github.com/gaelcolas/Datum#datum-tree).

Sobald die Datendateien einmal generiert sind, können Sie sie mit [DSC-Konfigurationsskripts](/powershell/dsc/configurations/write-compile-apply-configuration) verwenden, um MOF-Dateien zu generieren und die [MOF-Dateien in Azure Automation hochzuladen](/azure/automation/tutorial-configure-servers-desired-state#create-and-upload-a-configuration-to-azure-automation).
Danach registrieren Sie Ihre Server entweder von [lokal](/azure/automation/automation-dsc-onboarding#physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azureaws) oder [in Azure](/azure/automation/automation-dsc-onboarding#azure-virtual-machines) um Konfigurationen abzurufen.

Um Datum zu testen, besuchen Sie den [PowerShell-Katalog](https://www.powershellgallery.com/packages/datum/), und laden Sie die Lösung herunter, oder klicken Sie auf „Projektwebsite“, um die [Dokumentation](https://github.com/gaelcolas/Datum#2-getting-started--concepts) anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

- [Windows PowerShell DSC – Übersicht](/powershell/dsc/overview/overview)
- [DSC-Ressourcen](/powershell/dsc/resources/resources)
- [Konfigurieren des lokalen Konfigurations-Managers](/powershell/dsc/managing-nodes/metaconfig)

---
title: Anleitung zum Onboarding der Microsoft Azure-Sicherheitscodeanalyse
description: Dieser Artikel behandelt die Installation der Erweiterung „Sicherheitscodeanalyse“.
author: vharindra
manager: sukhans
ms.author: terrylan
ms.date: 07/31/2019
ms.topic: article
ms.service: security
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: 8a8eca73205d4f8f33d4d069fda72638af61d355
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68718173"
---
# <a name="how-to-onboarding-and-installing"></a>Anleitung: Onboarding und Installation

Voraussetzungen für die Verwendung der Microsoft-Sicherheitscodeanalyse
  - Berechtigtes Angebot für Microsoft Unified Support-Dienste (Details siehe unten)
  - Eine Azure DevOps-Organisation
  - Berechtigungen zum Installieren von Erweiterungen für die Azure DevOps-Organisation
  - Quellcode, der mit einer in der Cloud gehosteten Azure DevOps-Pipeline synchronisiert werden kann


## <a name="onboarding-microsoft-security-code-analysis-extension"></a>Onboarding der Erweiterung „Microsoft-Sicherheitscodeanalyse“

- Wenn Sie bereits über eines der folgenden Supportangebote verfügen, wenden Sie sich einfach an Ihren Technical Account Manager, und erwerben oder tauschen Sie Stunden, um Zugriff auf die Erweiterung zu erhalten.
   - Unified Support – Advanced- und Performance-Tarif, Premier Support für Entwickler, Premier Support für Partner oder Premier Support für Enterprise
- Wenn Sie über einen der folgenden Supportdienste oder über keinen Supportplan für Microsoft verfügen, müssen Sie ein Upgrade auf ein berechtigtes Supportangebot durchführen:
   - Azure-Support für Partner, Azure Basic, Azure Developer, Azure Standard, Azure Professional Direct oder Unified Support – Core-Tarif
- Zum Erwerb eines berechtigten Supportangebots besuchen Sie bitte unsere [Homepage für Supportdienste](https://www.microsoft.com/enterprise/services/support).
- Nachdem Sie einen Supportvertrag abgeschlossen haben, wenden Sie sich an Ihren Technical Account Manager, der Ihnen beim Einstieg und beim Erfassen aller erforderlichen Details helfen kann.
 
>[!NOTE]
> Nur registrierte Partner im Microsoft Partner Network sind berechtigt, Premier Support für Partner zu erwerben. Erwerben Sie andernfalls eines der zuvor genannten berechtigten Supportangebote.

## <a name="installing-microsoft-security-code-analysis-extension"></a>Installieren der Erweiterung „Microsoft-Sicherheitscodeanalyse“

1. Nachdem die Erweiterung für Ihre Azure DevOps-Organisation freigegeben wurde, navigieren Sie zur Seite Ihrer Azure DevOps-Organisation (z. B. http://dev.azure.com/contoso).
2. Klicken Sie oben rechts auf das Warenkorbsymbol neben Ihrem Namen und dann auf „Erweiterungen verwalten“. 
3. Klicken Sie auf die Erweiterung „Microsoft-Sicherheitscodeanalyse“, und starten Sie den Assistenten für die Azure DevOps-Benutzeroberfläche, um die Installation zu starten.
4. Wählen Sie in der Dropdownliste die Azure DevOps-Organisation aus, für die die Erweiterung installiert werden soll.
5. Klicken Sie auf „Installieren“. Nach Abschluss der Installation können Sie die Erweiterung verwenden.

>[!NOTE]
> Fahren Sie mit den Installationsschritten fort, auch wenn Sie keinen Zugriff haben. Den Zugriff können Sie während des Vorgangs vom Administrator Ihrer Azure DevOps-Organisation anfordern.
>
Nach der Installation der Erweiterung werden die Buildaufgaben für die sichere Entwicklung angezeigt und können Ihren Azure-Pipelines hinzugefügt werden.

## <a name="adding-specific-build-tasks-to-your-devops-pipeline"></a>Hinzufügen spezifischer Buildaufgaben zu Ihrer DevOps-Pipeline

1. Öffnen Sie das Teamprojekt in Ihrer Azure DevOps-Organisation.
2. Navigieren Sie unter **Pipelines** zur Registerkarte **Builds**. 
3. Wählen Sie die Pipeline aus, der Sie die Buildaufgaben der Erweiterung hinzufügen möchten. 
   - Neu: Klicken Sie auf **Neu**, und führen Sie die beschriebenen Schritte zum Erstellen einer neuen Pipeline aus.
   - Bearbeiten: Wählen Sie die Pipeline aus, und klicken Sie auf **Bearbeiten**, um mit der Bearbeitung einer vorhandenen Pipeline zu beginnen.
4. Klicken Sie auf „+“, um zum Bereich **Aufgaben hinzufügen** zu navigieren.
5. Suchen Sie die Buildaufgabe, die Sie hinzufügen möchten, in der Liste oder über das Suchfeld, und klicken Sie dann auf **Hinzufügen**. 
6. Nun können Sie die für die Aufgabe erforderlichen Parameter angeben.
>[!NOTE]
>Datei-oder Verzeichnispfade sind relativ zum Stammverzeichnis Ihres Quellrepositorys. Parameter, die die Ausgabeordner oder -dateien angeben, werden durch den allgemeinen Speicherort ersetzt, der im Build-Agent definiert wurde.

7. Setzen Sie einen neuen Build in die Warteschlange.
> [!TIP]
>  - Um die Analyse nach dem Build auszuführen, platzieren Sie die Buildaufgaben der Microsoft-Sicherheitscodeanalyse nach dem Schritt „Buildartefakte veröffentlichen“ des Builds. Auf diese Weise können der Build beendet und Ergebnisse veröffentlicht werden, bevor statische Analysetools ausgeführt werden.
>  - Aktivieren Sie immer die Option **Bei Fehler fortsetzen** der Buildaufgaben für die sichere Entwicklung. Auch wenn bei einem der Tools Fehler auftreten, können die anderen Tools ausgeführt werden. Zwischen den Tools bestehen keine Abhängigkeiten.
>  - Bei den Buildaufgaben der Microsoft-Sicherheitscodeanalyse treten nur Fehler auf, wenn das Tool nicht erfolgreich ausgeführt wird. Es treten jedoch keine Fehler auf, wenn das Tool Probleme im Code identifiziert. Sie können den Build so konfigurieren, dass er unterbrochen wird, wenn ein Tool Probleme im Code identifiziert. Verwenden Sie dazu die Buildaufgabe für die **Nachanalyse**.
>  - Einige Azure DevOps-Buildaufgaben werden nicht unterstützt, wenn sie über eine Releasepipeline ausgeführt werden. Dies ist eine Einschränkung von Azure DevOps. Aufgaben, die Artefakte über eine Releasepipeline veröffentlichen, werden nicht unterstützt.
>  - Eine Liste der vordefinierten Variablen in Azure DevOps Team Build, die Sie als Parameter angeben können, finden Sie unter [Azure DevOps-Buildvariablen](https://docs.microsoft.com/azure/devops/pipelines/build/variables?tabs=batch&view=vsts).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Konfigurieren der Buildaufgaben finden Sie in der [Konfigurationsanleitung](security-code-analysis-customize.md).

Wenn Sie weitere Fragen zur Erweiterung und zu den angebotenen Tools haben, [lesen Sie unsere FAQs-Seite.](security-code-analysis-faq.md)



---
title: Übersicht über die Microsoft Azure-Sicherheitscodeanalyse
description: Dieser Artikel bietet eine Übersicht über die Erweiterung „Sicherheitscodeanalyse“.
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
ms.openlocfilehash: 283d63bafc583f2ac9da3294644aaebd17d7c950
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68718169"
---
# <a name="about-microsoft-security-code-analysis"></a>Informationen zur Microsoft-Sicherheitscodeanalyse

Mit der **Erweiterung „Microsoft-Sicherheitscodeanalyse“** können Teams die Sicherheitscodeanalyse nahtlos in ihre Azure DevOps-CI/CD-Pipelines integrieren, wie dies von Experten von [Secure Development Lifecycle (SDL)](https://www.microsoft.com/securityengineering/sdl/practices) bei Microsoft empfohlen wird. Die Sicherheit wird durch eine konsistente Benutzeroberfläche vereinfacht, die die Komplexität der Ausführung verschiedener Tools abstrahiert. Mit einer NuGet-basierten Bereitstellung der Tools müssen Teams die Installation und Aktualisierung der Tools nicht mehr verwalten. Über Schnittstellen mit Befehlszeilen und grundlegenden Buildaufgaben können alle Benutzer, vom versierten Toolexperten bis zum alltäglichen Entwickler, so wenig oder so viel Kontrolle wie gewünscht über die Tools haben. Teams können außerdem leistungsstarke Funktionen für die Nachbearbeitung nutzen, z. B. Veröffentlichen von Protokollen zur Aufbewahrung, Generieren von umsetzbaren entwicklerorientierten Berichten und Konfigurieren von Buildunterbrechungen bei Regressionen.

## <a name="why-microsoft-security-code-analysis"></a>Gründe für die Verwendung der Microsoft-Sicherheitscodeanalyse

### <a name="security-simplified"></a>Vereinfachte Sicherheit

Zum Hinzufügen der Tools der Sicherheitscodeanalyse zu Ihrer Azure DevOps-Pipeline werden einfach neue Aufgaben hinzugefügt. Diese können Sie anpassen oder mit den Standardeinstellungen verwenden. Die Aufgaben werden als Teil der DevOps-Pipeline ausgeführt und generieren Protokolle, in denen alle möglichen Arten von Ergebnissen aufgeführt sind.

### <a name="clean-builds"></a>Bereinigte Builds

Nachdem Sie die anfänglichen Probleme behoben haben, die von den Tools gemeldet wurden, können Sie die Erweiterung so konfigurieren, dass die Builds bei neuen Problemen unterbrochen werden. Noch nie war das Einrichten von Continuous Integration-Builds bei jedem Pull Request so einfach!

### <a name="set-it-and-forget-it"></a>Einmal festlegen und dann vergessen

Die Buildaufgaben und -tools können so festgelegt werden, dass sie auf dem neuesten Stand bleiben (und sind dies auch standardmäßig). Wenn eine aktualisierte Version des Tools vorliegt, muss sie nicht heruntergeladen und installiert werden. Dies erfolgt automatisch über die Erweiterung. 

>>>
### <a name="under-the-hood"></a>Im Hintergrund

Die Buildaufgaben der Microsoft-Sicherheitscodeanalyse abstrahieren die Komplexität folgender Vorgänge:
  - Ausführen von statischen Sicherheitsanalysetools
  - Verarbeiten der Ergebnisse aus Protokolldateien zum Erstellen eines Zusammenfassungsberichts oder Unterbrechen des Builds
>>>

## <a name="security-code-analysis-toolset"></a>Toolset der Sicherheitscodeanalyse

Mit der Erweiterung „Microsoft-Sicherheitscodeanalyse“ stehen Ihnen die neuesten Versionen wichtiger Analysetools zur Verfügung. Die Erweiterung umfasst sowohl Microsoft-interne Tools als auch Open-Source-Tools. Die Tools werden automatisch auf den in der Cloud gehosteten Agent heruntergeladen, nachdem Sie die Pipeline mithilfe der entsprechenden Buildaufgabe konfiguriert und ausgeführt haben. Nachfolgend sind die Tools aufgeführt, die derzeit in der Erweiterung verfügbar sind. Bleiben Sie auf dem Laufenden, und senden Sie uns Vorschläge für Tools, die hinzugefügt werden können.

### <a name="anti-malware-scanner"></a>Schadsoftwarescanner

Die Buildaufgabe für den Schadsoftwarescanner ist nun in der Erweiterung „Microsoft-Sicherheitscodeanalyse“ enthalten. Er muss auf einem Build-Agent ausgeführt werden, auf dem Windows Defender bereits installiert ist. Weitere Informationen finden Sie auf der [Defender-Website](https://aka.ms/defender). 

### <a name="binskim"></a>BinSkim

BinSkim ist ein einfacher PE-Scanner (Portable Executable, portierbar ausführbar), mit dem Compiler- und Linker-Einstellungen sowie andere sicherheitsrelevante binäre Merkmale überprüft werden. Mit der Buildaufgabe wird ein Befehlszeilenwrapper um die Anwendung „BinSkim.exe“ erstellt. BinSkim ist ein Open-Source-Tool. Weitere Informationen finden Sie unter [BinSkim auf GitHub](https://github.com/Microsoft/binskim).

### <a name="credential-scanner"></a>Credential Scanner

Kennwörter und andere Geheimnisse, die im Quellcode gespeichert sind, stellen derzeit ein erhebliches Problem dar. Credential Scanner ist ein proprietäres statisches Analysetool, mit dem Anmeldeinformationen, Geheimnisse, Zertifikate und andere vertrauliche Inhalte im Quellcode und der Buildausgabe erkannt werden.

### <a name="microsoft-security-risk-detection"></a>Microsoft Security Risk Detection

Security Risk Detection ist der einzigartige cloudbasierte Fuzzing-Dienst von Microsoft zum Identifizieren von ausnutzbaren Sicherheitsfehlern in der Software. Dieser Dienst erfordert einen separaten Onboardingprozess. Weitere Informationen finden Sie unter [MSRD unter docs.microsoft.com](https://docs.microsoft.com/security-risk-detection/).

### <a name="roslyn-analyzers"></a>Roslyn Analyzers

Das in den Compiler integrierte statische Analysetool von Microsoft zum Analysieren von verwaltetem Code (C# und VB). Weitere Informationen finden Sie unter [Roslyn Analyzers unter docs.microsoft.com](https://docs.microsoft.com/dotnet/standard/analyzers/).

### <a name="tslint"></a>TSLint

Bei TSLint handelt es sich um ein erweiterbares statisches Analysetool, mit dem TypeScript-Code auf Lesbarkeit, Wartbarkeit und Funktionsfehler geprüft wird. Das Tool wird in modernen Editoren und Buildsystemen weitgehend unterstützt und kann mit Ihren eigenen Lint-Regeln, -Konfigurationen und -Formatierern angepasst werden. TSLint ist ein Open-Source-Tool. Weitere Informationen finden Sie unter [TSLint auf GitHub](https://github.com/palantir/tslint).

## <a name="analysis-and-post-processing-of-results"></a>Analyse und Nachbearbeitung von Ergebnissen

Die Erweiterung „Microsoft-Sicherheitscodeanalyse“ umfasst außerdem drei hilfreiche Nachbearbeitungsaufgaben, mit denen Sie die mit den Aufgaben der Sicherheitstools gefundenen Ergebnisse verarbeiten und analysieren können. Sie werden in der Regel in der Pipeline nach allen anderen Toolaufgaben hinzugefügt.

### <a name="publish-security-analysis-logs"></a>Veröffentlichen von Sicherheitsanalyseprotokollen
Über die Buildaufgabe zum Veröffentlichen von Sicherheitsanalyseprotokollen werden die Protokolldateien der während des Builds ausgeführten Sicherheitstools zur Untersuchung und Nachverfolgung beibehalten.

Die Protokolle können in den Artefakten des Azure-Servers (als ZIP-Datei) veröffentlicht oder über den privaten Build-Agent in eine zugängliche Dateifreigabe kopiert werden.

### <a name="security-report"></a>Sicherheitsbericht
Mit der Buildaufgabe für den Sicherheitsbericht werden die Protokolldateien analysiert, die mit den während des Builds ausgeführten Sicherheitstools erstellt wurden, und es wird eine Zusammenfassungsberichtsdatei mit allen mit den Analysetools gefundenen Problemen erstellt.

Die Aufgabe kann so konfiguriert werden, dass Ergebnisse für spezifische oder für alle Tools erfasst werden. Außerdem können Sie festlegen, welche Fehlerstufe (Fehler oder Fehler und Warnungen) gemeldet wird.

### <a name="post-analysis-build-break"></a>Nachanalyse (Buildunterbrechung)
Mit der Buildaufgabe für die Nachanalyse kann der Kunde eine Buildunterbrechung einfügen, sodass der Build mit einem Fehler abgebrochen wird, wenn Analysetools Ergebnisse oder Probleme im Code melden.

Die Aufgabe kann so konfiguriert werden, dass der Build bei Problemen unterbrochen wird, die mit spezifischen oder mit allen Aufgaben gefunden wurden, und außerdem basierend auf dem Schweregrad der gefundenen Probleme (Fehler oder Fehler und Warnungen) festgelegt werden.

>[!NOTE]
>Einzelne Buildaufgaben werden entwurfsbedingt ausgeführt, solange das Tool erfolgreich abgeschlossen wird, unabhängig davon, ob Ergebnisse vorhanden oder nicht vorhanden sind, sodass der Build bis zum Abschluss ausgeführt werden kann und alle Tools ausgeführt werden können.

## <a name="next-steps"></a>Nächste Schritte

Anweisungen zum Onboarding und zur Installation der Sicherheitscodeanalyse finden Sie in der Anleitung zu [Onboarding und Installation](security-code-analysis-onboard.md).

Weitere Informationen zum Konfigurieren der Buildaufgaben finden Sie in der [Konfigurationsanleitung](security-code-analysis-customize.md).

Wenn Sie weitere Fragen zur Erweiterung und zu den angebotenen Tools haben, [lesen Sie unsere FAQs-Seite.](security-code-analysis-faq.md)
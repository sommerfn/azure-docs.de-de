---
title: Verwenden des Einstellungsbereichs für Azure Application Insights Profiler | Microsoft-Dokumentation
description: Anzeigen des Profiler-Status und Starten von Profilerstellungssitzungen
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: cweining
ms.author: cweining
ms.date: 08/06/2018
ms.reviewer: mbullwin
ms.openlocfilehash: b383ef8c92325b0ad6561bee9b654c78e4054338
ms.sourcegitcommit: 8e271271cd8c1434b4254862ef96f52a5a9567fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72820479"
---
# <a name="configure-application-insights-profiler"></a>Konfigurieren von Application Insights Profiler

## <a name="updated-profiler-agent"></a>Aktualisierter Profiler-Agent
Die Triggerfunktionen funktionieren nur mit Version 2.6 oder höher des Profiler-Agents. Wenn Sie eine Azure App Service ausführen, wird Ihr Agent automatisch aktualisiert. Sie können sehen, welche Version des Agents ausgeführt wird, wenn Sie die Kudu-URL für Ihre Website aufrufen und „\DiagnosticServices“ am Ende anfügen, wie hier gezeigt: https://yourwebsite.scm.azurewebsites.net/diagnosticservices. Der Application Insights Profiler-Webauftrag solle Version 2.6 oder höher haben. Sie können ein Upgrade erzwingen, indem Sie Ihre Web-App neu starten. 

Wenn Sie den Profiler auf einem virtuellen Computer oder in einem Clouddienst ausführen, müssen Sie die Windows Azure-Diagnoseerweiterung (WAD) Version 16.0.4 oder höher installiert haben. Sie können die Version von WAD überprüfen, indem Sie sich bei Ihrem virtuellen Computer anmelden und dieses Verzeichnis anzeigen: C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\1.16.0.4. Der Verzeichnisname ist die Version von der WAD, die installiert ist. Der Azure-VM-Agent aktualisiert WAD automatisch, wenn neue Versionen verfügbar sind.

## <a name="profiler-settings-page"></a>Seite mit Profiler-Einstellungen

Um den Einstellungsbereich von Azure Application Insights Profiler zu öffnen, wechseln Sie zum Bereich „Leistung“ von Application Insights, und wählen Sie dann die Schaltfläche **Profiler konfigurieren** aus.

![Link zum Öffnen der Seite mit Profiler-Einstellungen][configure-profiler-entry]

Hiermit wird eine Seite geöffnet, die in etwa wie folgt aussieht:

![Seite mit Profiler-Einstellungen][configure-profiler-page]

Die Seite **Application Insights Profiler konfigurieren** enthält diese Funktionen:

| | |
|-|-|
Jetzt Profil erstellen | Startet die Profilerstellungssitzungen für alle Apps, die mit dieser Instanz von Application Insights verknüpft sind.
Trigger | Ermöglicht Ihnen das Konfigurieren von Triggern, die die Ausführung des Profilers bewirken. 
Letzte Profilerstellungssitzungen | Zeigt Informationen zu vergangenen Profilerstellungssitzungen an.

## <a name="profile-now"></a>Jetzt Profil erstellen
Mit dieser Option können Sie bei Bedarf eine Profilerstellungssitzung starten. Wenn Sie auf diesen Link klicken, beginnen alle Profiler-Agents, die Daten an diese Application Insights-Instanz senden, mit der Erfassung eines Profils. Nach 5 bis 10 Minuten wird die Profilsitzung in der Liste unten angezeigt.

Damit ein Benutzer eine Profilersitzung manuell auslösen kann, muss er in seiner Rolle mindestens über Schreibzugriff für die Application Insights-Komponente verfügen. In den meisten Fällen erhalten Sie diesen Zugriff automatisch, und es sind keine weiteren Arbeitsschritte erforderlich. Wenn Sie Probleme haben, müssen Sie die entsprechende Rolle für den Abonnementbereich, d. h. „Mitwirkender der Application Insights-Komponente“, hinzufügen. [Weitere Informationen zur Rollenzugriffssteuerung mithilfe der Azure-Überwachung](https://docs.microsoft.com/azure/azure-monitor/app/resources-roles-access-control).

## <a name="trigger-settings"></a>Triggereinstellungen
![Triggereinstellungen-Flyout][trigger-settings-flyout]

Wenn Sie in der Menüleiste auf die Schaltfläche „Trigger“ klicken, wird das Feld mit den „Triggereinstellungen“ geöffnet. Sie können einen Trigger einrichten, der die Profilerstellung startet, wenn der Prozentsatz der CPU- oder Arbeitsspeicherverwendung das festgelegte Niveau erreicht.

| | |
|-|-|
Schaltfläche „Ein/Aus“ | Ein: Profiler kann von diesem Trigger gestartet werden. Aus: Profiler wird von diesem Trigger nicht gestartet.
Schwellenwert für Arbeitsspeicher | Wenn dieser Prozentsatz des Arbeitsspeichers verwendet wird, wird der Profiler gestartet.
Duration | Legt die Zeitspanne fest, die der Profiler bei Auslösung ausgeführt wird.
Abkühlen | Legt die Zeitspanne fest, die der Profiler wartet, bevor nach seiner Auslösung die Arbeitsspeicher- oder CPU-Verwendung erneut überprüft wird.

## <a name="recent-profiling-sessions"></a>Letzte Profilerstellungssitzungen
In diesem Abschnitt werden Informationen zu den letzten Profilerstellungssitzungen angezeigt. Eine Profilerstellungssitzung stellt den Zeitraum dar, in dem der Profiler-Agent ein Profil auf einem der Computer, auf dem Ihre Anwendung gehostet wird, erfasst hat. Sie können die Profile aus einer Sitzung öffnen, indem Sie auf eine der Zeilen klicken. Für jede Sitzung wird Folgendes angezeigt:

| | |
|-|-|
Ausgelöst von | Wie die Sitzung gestartet wurde, entweder von einem Trigger, mit „Jetzt Profil erstellen“ oder per „Standard-Stichprobennahme“. 
App-Name | Name der Anwendung, für die ein Profil erstellt wurde.
Computerinstanz | Name des Computers, auf dem der Profiler-Agent ausgeführt wurde.
Timestamp | Uhrzeit, zu der das Profil erfasst wurde.
Ablaufverfolgungen | Anzahl der Ablaufverfolgungen, die einzelnen Anforderungen angefügt wurden.
CPU % | Prozentsatz der CPU, der während der Ausführung des Profilers verwendet wurde.
Arbeitsspeicher % | Prozentsatz des Arbeitsspeichers, der während der Ausführung des Profilers verwendet wurde.

## <a id="profileondemand"></a> Verwenden von Webleistungstests zum Generieren von Datenverkehr an Ihre Anwendung

Sie können Profiler manuell mit einem einzigen Mausklick auslösen. Nehmen wir an, dass Sie einen Webleistungstest ausführen. Sie benötigen Ablaufverfolgungen, um besser zu verstehen, wie Ihre Web-App unter Last ausgeführt wird. Die Kontrolle über den Zeitpunkt der Erfassung von Ablaufverfolgungen ist entscheidend, da Sie wissen, wann der Auslastungstest ausgeführt wird. Bei einem zufälligen Intervall für die Stichprobenentnahme verpassen Sie diesen Zeitpunkt aber möglicherweise.

Die folgenden Abschnitte veranschaulichen dieses Szenario:

### <a name="step-1-generate-traffic-to-your-web-app-by-starting-a-web-performance-test"></a>Schritt 1: Generieren von Datenverkehr für Ihre Web-App durch das Starten eines Webleistungstests

Wenn Ihre Web-App bereits über eingehenden Datenverkehr verfügt oder wenn Sie einfach nur manuell Datenverkehr generieren möchten, überspringen Sie diesen Abschnitt, und fahren Sie mit Schritt 2 fort.

1. Wählen Sie im Application Insights-Portal **Konfigurieren** > **Leistungstests** aus. 

1. Wählen Sie die Schaltfläche **Neu** aus, um einen neuen Leistungstest zu starten.

   ![Neuen Leistungstest erstellen][create-performance-test]

1. Konfigurieren Sie im Bereich **Neuer Leistungstest** die Testziel-URL. Übernehmen Sie alle Standardeinstellungen, und wählen Sie dann **Test ausführen** aus, um die Ausführung des Auslastungstests zu starten.

    ![Konfigurieren von Auslastungstests][configure-performance-test]

    Der neue Test wird zunächst in die Warteschlange eingereiht und erhält dann den Status *In Bearbeitung*.

    ![Auslastungstest übermittelt und in die Warteschlange gestellt][load-test-queued]

    ![Ausführung des Auslastungstests in Bearbeitung][load-test-in-progress]

### <a name="step-2-start-a-profiler-on-demand-session"></a>Schritt 2: Starten einer bedarfsgesteuerten Profiler-Sitzung

1. Wenn der Auslastungstest ausgeführt wird, starten Sie Profiler, um Ablaufverfolgungen in der Web-App zu erfassen, während diese die Last empfängt.

1. Wechseln Sie zum Bereich **Profiler konfigurieren**.


### <a name="step-3-view-traces"></a>Schritt 3: Ablaufverfolgungen anzeigen

Wenn die Profiler-Ausführung abgeschlossen ist, folgen Sie den Anweisungen zur Benachrichtigung, um zum Bereich „Leistung“ zu wechseln und Ablaufverfolgungen anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte
[Aktivieren von Profiler und Anzeigen von Ablaufverfolgungen](profiler-overview.md?toc=/azure/azure-monitor/toc.json)

[profiler-on-demand]: ./media/profiler-settings/Profiler-on-demand.png
[configure-profiler-entry]: ./media/profiler-settings/configure-profiler-entry.png
[configure-profiler-page]: ./media/profiler-settings/configureBlade.png
[trigger-settings-flyout]: ./media/profiler-settings/CPUTrigger.png
[create-performance-test]: ./media/profiler-settings/new-performance-test.png
[configure-performance-test]: ./media/profiler-settings/configure-performance-test.png
[load-test-queued]: ./media/profiler-settings/load-test-queued.png
[load-test-in-progress]: ./media/profiler-settings/load-test-inprogress.png
[enable-app-insights]: ./media/profiler-settings/enable-app-insights-blade-01.png
[update-site-extension]: ./media/profiler-settings/update-site-extension-01.png
[change-and-save-appinsights]: ./media/profiler-settings/change-and-save-appinsights-01.png
[app-settings-for-profiler]: ./media/profiler-settings/appsettings-for-profiler-01.png
[check-for-extension-update]: ./media/profiler-settings/check-extension-update-01.png
[profiler-timeout]: ./media/profiler-settings/profiler-timeout.png

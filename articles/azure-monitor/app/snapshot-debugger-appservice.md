---
title: Aktivieren des Momentaufnahmedebuggers für .NET-Apps in Azure App Service | Microsoft-Dokumentation
description: Aktivieren des Momentaufnahmedebuggers für .NET-Apps in Azure App Service
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: brahmnes
ms.author: bfung
ms.date: 03/07/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 0f6eb6376075337edd7656e4bc83b5b7fddde479
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72899891"
---
# <a name="enable-snapshot-debugger-for-net-apps-in-azure-app-service"></a>Aktivieren des Momentaufnahmedebuggers für .NET-Apps in Azure App Service

Der Momentaufnahmedebugger kann derzeit für ASP.NET- und ASP.NET Core-Apps verwendet werden, die unter Azure App Service in Windows-Dienstplänen ausgeführt werden.

## <a id="installation"></a> Aktivieren des Momentaufnahmedebuggers
Befolgen Sie die unten angegebenen Anweisungen, um den Momentaufnahmedebugger für eine App zu aktivieren. Wenn Sie eine andere Art von Azure-Dienst ausführen, helfen Ihnen diese Anweisungen zum Aktivieren des Momentaufnahmedebuggers auf anderen unterstützten Plattformen weiter:
* [Azure Cloud Services](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric-Dienste](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure Virtual Machines und VM-Skalierungsgruppen](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Lokale virtuelle oder physische Computer](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)

Wenn Sie eine Vorschauversion von .NET Core verwenden, führen Sie zuerst die Anweisungen unter [Aktivieren des Momentaufnahmedebuggers für andere Umgebungen](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json) aus, um das NuGet-Paket [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) in die Anwendung zu integrieren, und führen Sie dann die übrigen nachstehenden Anweisungen aus. 

Der Application Insights-Momentaufnahmedebugger ist als Teil der App Services-Runtime zwar bereits vorinstalliert, muss aber noch aktiviert werden, um Momentaufnahmen für Ihre App Service-Apps zu erhalten. Führen Sie die unten angegebenen Schritte zum Aktivieren des Momentaufnahmedebuggers aus, nachdem Sie eine App bereitgestellt haben (auch wenn Sie das Application Insights SDK in den Quellcode eingefügt haben).

1. Wechseln Sie zum Bereich **App Services** im Azure-Portal.
2. Navigieren Sie zum Bereich **Einstellungen > Application Insights**.

   ![Aktivieren von App Insights im App Services-Portal](./media/snapshot-debugger/applicationinsights-appservices.png)

3. Befolgen Sie die Anweisungen im Bereich zum Erstellen einer neuen Ressource, oder wählen Sie eine vorhandene App Insights-Ressource aus, um Ihre App zu überwachen. Stellen Sie außerdem sicher, dass beide Schalter für den Momentaufnahmedebugger auf **Ein** festgelegt sind.

   ![Hinzufügen der App Insights-Websiteerweiterung][Enablement UI]

4. Der Momentaufnahmedebugger wurde mithilfe einer App Services-App-Einstellung aktiviert.

    ![App-Einstellung für den Momentaufnahmedebugger][snapshot-debugger-app-setting]

## <a name="disable-snapshot-debugger"></a>Deaktivieren des Momentaufnahmedebuggers

Führen Sie die gleichen Schritte wie zum **Aktivieren des Momentaufnahmedebuggers** aus, legen Sie jedoch beide Schalter für den Momentaufnahmedebugger auf **Aus** fest.
Es wird empfohlen, den Momentaufnahmedebugger für alle Ihre Apps zu aktivieren, um die Diagnose von Anwendungsausnahmen zu erleichtern.

## <a name="next-steps"></a>Nächste Schritte

- Generieren Sie Datenverkehr für Ihre Anwendung, der eine Ausnahme auslösen kann. Warten Sie dann 10 bis 15 Minuten, bis die Momentaufnahmen an die Application Insights-Instanz gesendet werden.
- Weitere Informationen finden Sie unter [Momentaufnahmen](snapshot-debugger.md?toc=/azure/azure-monitor/toc.json#view-snapshots-in-the-portal) im Azure-Portal.
- Hilfe bei der Behandlung von Problemen mit dem Momentaufnahmedebugger finden Sie unter [Problembehandlung für Momentaufnahmedebugger](snapshot-debugger-troubleshoot.md?toc=/azure/azure-monitor/toc.json).

[Enablement UI]: ./media/snapshot-debugger/enablement-ui.png
[snapshot-debugger-app-setting]:./media/snapshot-debugger/snapshot-debugger-app-setting.png


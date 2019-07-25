---
title: Diagnostizieren von häufigen Fehlern im Zusammenhang mit Codepaketen mit Service Fabric | Microsoft-Dokumentation
description: Beheben von häufigen Fehlern im Zusammenhang mit Codepaketen mit Azure Service Fabric
services: service-fabric
documentationcenter: .net
author: grzuber
manager: gkhanna
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/09/2019
ms.author: grzuber
ms.openlocfilehash: 320a55e8b14648b1d7e256855582ab31846a63cf
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2019
ms.locfileid: "68249217"
---
# <a name="diagnose-common-code-package-errors-by-using-service-fabric"></a>Diagnostizieren von häufigen Fehlern im Zusammenhang mit Codepaketen mit Service Fabric

In diesem Artikel wird beschrieben, was es bedeutet, wenn ein Codepaket unerwartet beendet wird. Er bietet Einblicke in mögliche Ursachen für häufige Fehlercodes sowie Schritte zur Problembehandlung.

## <a name="when-does-a-process-or-container-terminate-unexpectedly"></a>Wann wird ein Prozess oder ein Container unerwartet beendet?

Wenn Azure Service Fabric eine Anforderung zum Starten eines Codepakets empfängt, wird die Umgebung auf dem lokalen System gemäß den in der App und in Dienstmanifesten festgelegten Konfigurationsoptionen vorbereitet. Zu diesen Vorbereitungen gehören z.B. das Reservieren von Netzwerkendpunkten oder -ressourcen, das Konfigurieren von Firewallregeln oder das Einrichten von Einschränkungen für die Ressourcenkontrolle. 

Nachdem die Umgebung ordnungsgemäß konfiguriert wurde, versucht Service Fabric, das Codepaket anzuzeigen. Dieser Schritt gilt als erfolgreich, wenn das Betriebssystem oder die Containerruntime meldet, dass der Prozess oder der Container erfolgreich aktiviert wurde. Wenn die Aktivierung nicht erfolgreich war, sollte eine Integritätsmeldung in SFX angezeigt werden, die der folgenden Meldung ähnelt:

```
There was an error during CodePackage activation. Service host failed to activate. Error: 0xXXXXXXXX
```

Nachdem das Codepaket erfolgreich aktiviert wurde, beginnt Service Fabric, dessen Lebensdauer zu überwachen. An diesem Punkt kann ein Prozess oder Container jederzeit und aus verschiedenen Gründen beendet werden. Es kann z.B. ein Fehler beim Initialisieren einer DLL vorliegen, oder dem Betriebssystem steht kein Desktopheapspeicher zur Verfügung. Wenn Ihr Codepaket beendet wurde, sollte die folgende Integritätsmeldung in SFX angezeigt werden:

```
The process/container terminated with exit code: XXXXXXXX. Please look at your application logs/dump or debug your code package for more details. For information about common termination errors, please visit https://aka.ms/service-fabric-termination-errors
```

Der Exitcode in dieser Integritätsmeldung ist der einzige Hinweis auf den Grund für die Beendigung des Prozesses oder Containers. Er kann durch eine beliebige Ebene des Stapels generiert werden. Dieser Exitcode kann sich z.B. auf einen Betriebssystemfehler oder ein .NET-Problem beziehen, oder er wurde möglicherweise durch Ihren Code ausgelöst. Verwenden Sie diesen Artikel als Ausgangspunkt für die Diagnose der Quelle der Beendigungsexitcodes und mögliche Lösungen. Beachten Sie jedoch, dass es sich hierbei um allgemeine Lösungen für gängige Szenarien handelt, die sich möglicherweise nicht auf den Fehler beziehen, den Sie sehen.

## <a name="how-can-i-tell-if-service-fabric-terminated-my-code-package"></a>Woran erkenne ich, ob Service Fabric mein Codepaket beendet hat?

Service Fabric ist möglicherweise aus mehreren Gründen für das Beenden Ihres Codepakets verantwortlich. Vielleicht soll das Codepaket für den Lastenausgleich auf einem anderen Knoten platziert werden. Sie können anhand der Exitcodes in der folgenden Tabelle überprüfen, ob Service Fabric Ihr Codepaket beendet hat.

>[!NOTE]
> Wenn Ihr Prozess oder Container mit einem anderen Code als den in der folgenden Tabelle aufgeführten Codes beendet wird, ist Service Fabric nicht für das Beenden verantwortlich.

Exitcode | BESCHREIBUNG
--------- | -----------
7147 | Gibt an, dass Service Fabric den Prozess oder Container durch Senden eines „STRG+C“-Signals ordnungsgemäß beendet hat.
7148 | Gibt an, dass Service Fabric den Prozess oder Container beendet hat. In einigen Fällen gibt dieser Fehlercode an, dass der Prozess oder der Container nicht rechtzeitig reagiert hat, nachdem ein „STRG+C“-Signal gesendet wurde und daher beendet werden musste.


## <a name="other-common-error-codes-and-their-potential-fixes"></a>Weitere häufige Fehlercodes und deren potenzielle Korrekturen

Exitcode | Hexadezimalwert | Kurzbeschreibung | Grundursache | Mögliche Lösung
--------- | --------- | ----------------- | ---------- | -------------
3221225794 | 0xc0000142 | STATUS_DLL_INIT_FAILED | Dieser Fehler bedeutet manchmal, dass der Computer über keinen Desktopheapspeicher mehr verfügt. Diese Ursache ist besonders wahrscheinlich, wenn zahlreiche Prozesse Ihrer Anwendung auf dem Knoten ausgeführt werden. | Wenn Ihr Programm so konfiguriert ist, dass es nicht auf „STRG+C“-Signale antwortet, können Sie die Einstellung **EnableActivateNoWindow** im Clustermanifest aktivieren. Wenn Sie diese Einstellung aktivieren, wird das Codepaket ohne ein GUI-Fenster ausgeführt, und es werden keine „STRG+C“-Signale empfangen. Durch diese Aktion wird auch die Menge des Desktopheapspeichers reduziert, den jeder Prozess beansprucht. Sie können die Größe des Desktopheaps Ihres Knotens erhöhen, wenn Ihr Codepaket „STRG+C“-Signale empfangen muss.
3762504530 | 0xe0434352 | – | Dieser Wert stellt einen Fehlercode für einen Ausnahmefehler von verwaltetem Code (also .NET) dar. | Dieser Exitcode gibt an, dass Ihre Anwendung eine Ausnahme ausgelöst hat, die nicht verarbeitet wurde und den Prozess beendet hat. Debuggen Sie als ersten Schritt bei der Ermittlung des Auslösers dieses Fehlers Ihre Anwendungsprotokolle und Absturzabbilder.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur [Diagnose weiterer häufiger Szenarien](service-fabric-diagnostics-common-scenarios.md).
* Eine ausführlichere Übersicht über Azure Monitor-Protokolle und ihre Inhalte finden Sie unter [Azure Monitor: Übersicht.](../operations-management-suite/operations-management-suite-overview.md)
* Erfahren Sie mehr über die [Warnungen](../log-analytics/log-analytics-alerts.md) von Azure Monitor-Protokollen, die bei der Erkennung und Diagnose hilfreich sein können.
* Machen Sie sich mit den Funktionen zur [Protokollsuche und -abfrage](../log-analytics/log-analytics-log-searches.md) in Azure Monitor-Protokollen vertraut, die als Bestandteil von Azure Monitor-Protokollen angeboten werden.

---
title: Diagnostizieren von Fehlern im Zusammenhang mit Codepaketen mit Azure Service Fabric | Microsoft-Dokumentation
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
ms.openlocfilehash: 235952388d2c044cc141b3020c67944c4250ea3d
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2019
ms.locfileid: "67276579"
---
# <a name="diagnose-common-code-package-errors-with-service-fabric"></a>Diagnostizieren häufiger Fehler im Zusammenhang mit Codepaketen mit Service Fabric

In diesem Artikel wird veranschaulicht, was es bedeutet, wenn ein Codepaket unerwartet beendet wird. Außerdem bietet er einen Einblick in mögliche Ursachen für häufig auftretende Fehlercodes und Schritte für die Problembehandlung, mit denen das Problem möglicherweise behoben werden kann.

## <a name="when-does-a-process-or-container-terminate-unexpectedly"></a>Wann wird ein Prozess oder ein Container unerwartet beendet?

Wenn Service Fabric eine Anforderung zum Starten eines Codepakets empfängt, wird die Umgebung auf dem lokalen System basierend auf den in der App und Dienstmanifesten festgelegten Konfigurationsoptionen vorbereitet. Zu diesen Vorbereitungen gehören das Reservieren von Netzwerkendpunkten oder -ressourcen, das Konfigurieren von Firewallregeln oder das Einrichten von Einschränkungen für die Ressourcenkontrolle. Sobald die Umgebung ordnungsgemäß konfiguriert wurde, versucht Service Fabric, das Codepaket anzuzeigen. Dieser Schritt gilt als erfolgreich, wenn das Betriebssystem oder die Containerruntime meldet, dass der Prozess oder der Container erfolgreich aktiviert wurde. Wenn die Aktivierung nicht erfolgreich war, sollte die folgende Integritätsmeldung in SFX angezeigt werden:

```
There was an error during CodePackage activation. Service host failed to activate. Error: 0xXXXXXXXX
```

Sobald das Codepaket erfolgreich aktiviert wurde, beginnt Service Fabric, dessen Lebensdauer zu überwachen. An diesem Punkt kann ein Prozess oder Container jederzeit und aus verschiedenen Gründen beendet werden. Es kann ein Fehler beim Initialisieren der DLL vorliegen, dem Betriebssystem steht kein Desktopheapspeicher zur Verfügung usw. Wenn Ihr Codepaket beendet wurde, sollte folgende Integritätsmeldung in SFX angezeigt werden:

```
The process/container terminated with exit code: XXXXXXXX. Please look at your application logs/dump or debug your code package for more details. For information about common termination errors, please visit https://aka.ms/service-fabric-termination-errors
```

Nur der in dieser Integritätsmeldung vom Prozess oder Container bereitgestellte Exitcode gibt Aufschluss darüber, warum das Codepaket beendet wurde. Außerdem hätte dieses von jeder Ebene des Stapels generiert werden können. Es ist schwierig nachzuvollziehen, ob dieser Exitcode beispielsweise im Zusammenhang mit einem Betriebssystemfehler bzw. einem Problem mit .NET aufgetreten ist oder von Ihrem Code ausgelöst wurde. Dieser Artikel kann daher als Startpunkt für die Diagnose der Ursachen für Exitcodes und mögliche Lösungen sein und daran erinnern, dass es sich hierbei um allgemeine Lösungen für allgemeine Szenarios handelt und möglicherweise nicht für den Ihnen angezeigten Fehler gilt.

## <a name="how-can-i-tell-if-service-fabric-terminated-my-code-package"></a>Woran erkenne ich, ob Service Fabric mein Codepaket beendet hat?

Service Fabric ist möglicherweise aus mehreren Gründen für das Beenden Ihres Codepakets verantwortlich. Vielleicht soll das Codepaket für den Lastenausgleich auf einem anderen Knoten platziert werden. Wenn Ihnen einer der Exitcodes in der folgenden Tabelle angezeigt wird, wurde Ihr Codepaket von Service Fabric beendet:

>[!NOTE]
> Service Fabric ist nicht für das Beenden verantwortlich, wenn Ihr Prozess oder Container mit einem anderen als den in der folgenden Tabelle aufgeführten Exitcodes beendet wird:

Exitcode | BESCHREIBUNG
--------- | -----------
7147 | Diese Fehlercodes geben an, dass Service Fabric den Prozess oder Container durch Senden eines „STRG+C“-Signals ordnungsgemäß beendet hat.
7148 | Diese Fehlercodes geben an, dass Service Fabric den Prozess oder Container beendet hat. In einigen Fällen kann dieser Fehlercode angeben, dass der Prozess oder der Container nicht rechtzeitig reagiert hat, nachdem ein „STRG+C“-Signal gesendet wurde und daher beendet werden musste.


## <a name="other-common-error-codes-and-their-potential-fixes"></a>Weitere häufige Fehlercodes und deren potenzielle Korrekturen

Exitcode | Hexadezimalwert | Kurze Beschreibung | Grundursache | Mögliche Lösung
--------- | --------- | ----------------- | ---------- | -------------
3221225794 | 0xc0000142 | STATUS_DLL_INIT_FAILED | Dieser Fehler bedeutet möglicherweise, dass der Computer über keinen Desktopheapspeicher mehr verfügt. Diese Ursache ist besonders wahrscheinlich, wenn viele Prozesse Ihrer Anwendung auf dem Knoten ausgeführt werden. | Wenn Ihr Programm so konfiguriert ist, dass es nicht auf „STRG+C“-Signale antwortet, können Sie die Einstellung „EnableActivateNoWindow“ im Clustermanifest aktivieren. Das Aktivieren dieser Einstellung würde bedeuten, dass Ihr Codepaket ohne GUI-Fenster ausgeführt wird und keine „STRG+C“-Signale empfängt, aber andererseits die von jedem Prozess genutzte Menge an Desktopheapspeicher verringert. Sie können die Größe des Desktopheaps Ihres Knotens erhöhen, wenn Ihr Codepaket „STRG+C“-Signale empfangen muss.
3762504530 | 0xe0434352 | – | Bei diesem Wert handelt es sich um einen Fehlercode für einen Ausnahmefehler von verwaltetem Code (.NET). | Wenn Ihnen dieser Exitcode angezeigt wird, hat Ihre Anwendung eine Ausnahme ausgelöst, die nicht verarbeitet wurde und den Prozess beendet hat. Das Debuggen der Protokolle und Sicherungsdateien Ihrer Anwendung sollte der erste Schritt beim Bestimmen der Fehlerursache sein.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur [Diagnose weiterer häufiger Szenarios](service-fabric-diagnostics-common-scenarios.md)
* Detailliertere Übersicht über Azure Monitor-Protokolle und die zugehörigen Optionen unter [Was sind Azure Monitor-Protokolle?](../operations-management-suite/operations-management-suite-overview.md)
* Erfahren Sie mehr über die [Warnungen](../log-analytics/log-analytics-alerts.md) von Azure Monitor-Protokolle, die bei der Erkennung und Diagnose hilfreich sein können.
* Machen Sie sich mit den Funktionen zur [Protokollsuche und -abfrage](../log-analytics/log-analytics-log-searches.md) in Azure Monitor-Protokolle vertraut.

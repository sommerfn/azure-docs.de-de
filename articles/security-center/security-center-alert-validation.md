---
title: Warnungsüberprüfung (EICAR-Testdatei) in Azure Security Center | Microsoft-Dokumentation
description: In diesem Dokument erfahren Sie, wie Sie die Sicherheitswarnungen in Azure Security Center überprüfen.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: f8f17a55-e672-4d86-8ba9-6c3ce2e71a57
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 709ac3a9f5e7cf0bd13a6e387f0b80caf2608fe0
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73748464"
---
# <a name="alert-validation-eicar-test-file-in-azure-security-center"></a>Warnungsüberprüfung (EICAR-Testdatei) in Azure Security Center
In diesem Dokument erfahren Sie, wie Sie überprüfen, ob Ihr System ordnungsgemäß für Azure Security Center-Warnungen konfiguriert ist.

## <a name="what-are-security-alerts"></a>Was sind Sicherheitswarnungen?
Warnungen sind die Benachrichtigungen, die Security Center generiert, wenn Bedrohungen für Ihre Ressourcen erkannt werden. Security Center priorisiert die Warnungen und listet sie zusammen mit den Informationen auf, die erforderlich sind, um das Problem schnell zu untersuchen. Security Center stellt außerdem Empfehlungen zur Reaktion auf einen Angriff bereit.
Weitere Informationen finden Sie unter [Sicherheitswarnungen in Azure Security Center](security-center-alerts-overview.md) und [Verwalten von und Reagieren auf Sicherheitswarnungen](security-center-managing-and-responding-alerts.md).

## <a name="alert-validation"></a>Warnungsüberprüfung

* [Windows](#validate-windows)
* [Linux](#validate-linux)
* [Kubernetes](#validate-kubernetes)

## Überprüfen von Warnungen auf virtuellen Windows-Computern <a name="validate-windows"></a>

Nach der Installation des Security Center-Agents auf Ihrem Computer führen Sie auf dem Computer, auf dem sich die angegriffene Ressource für die Warnung befinden soll, die folgenden Schritte aus:

1. Kopieren Sie eine ausführbare Datei (beispielsweise **calc.exe**) auf den Desktop des Computers oder in ein beliebiges anderes Verzeichnis, und benennen Sie sie in **ASC_AlertTest_662jfi039N.exe** um.
1. Öffnen Sie die Eingabeaufforderung, und führen Sie die Datei mit einem Argument (beliebiger Pseudoname) aus. Beispiel: ```ASC_AlertTest_662jfi039N.exe -foo```
1. Warten Sie fünf bis zehn Minuten, und öffnen Sie die Security Center-Warnungen. Eine Warnung ähnlich wie im [Beispiel](#alert-validate) unten sollte angezeigt werden:

> [!NOTE]
> Vergewissern Sie sich bei der Prüfung dieser Testwarnung für Windows, dass **Arguments Auditing Enabled** (Argumentüberprüfung aktiviert) auf **TRUE** festgelegt ist. Sollte die Option auf **FALSE** festgelegt sein, müssen Sie die Überprüfung von Befehlszeilenargumenten aktivieren. Verwenden Sie den folgenden Befehl, um die Einstellung zu aktivieren:
>
>```reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"```

## Überprüfen von Warnungen auf virtuellen Linux-Computern <a name="validate-linux"></a>

Nach der Installation des Security Center-Agents auf Ihrem Computer führen Sie auf dem Computer, auf dem sich die angegriffene Ressource für die Warnung befinden soll, die folgenden Schritte aus:
1. Kopieren Sie eine ausführbare Datei an einem geeigneten Speicherort, und benennen Sie sie in **./asc_alerttest_662jfi039n** um. Beispiel:

    ```cp /bin/echo ./asc_alerttest_662jfi039n```

1. Öffnen Sie die Eingabeaufforderung, und führen Sie diese Datei aus:

    ```./asc_alerttest_662jfi039n testing eicar pipe```

1. Warten Sie fünf bis zehn Minuten, und öffnen Sie die Security Center-Warnungen. Eine Warnung ähnlich wie im [Beispiel](#alert-validate) unten sollte angezeigt werden:

### Beispiel für eine Warnung <a name="alert-validate"></a>

![Beispiel für eine Warnungsüberprüfung](./media/security-center-alert-validation/security-center-alert-validation-fig2.png) 


## Überprüfen von Warnungen unter Kubernetes <a name="validate-kubernetes"></a>

Wenn Sie die Security Center-Previewfunktion der Integration von Azure Kubernetes Service verwenden, führen Sie den folgenden kubectl-Befehl aus, um zu testen, ob Ihre Warnungen funktionieren:

```kubectl get pods --namespace=asc-alerttest-662jfi039n```

Weitere Informationen zur Integration von Azure Kubernetes Service und Azure Security Center finden Sie in [diesem Artikel](azure-kubernetes-service-integration.md).

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurden Sie in den Prozess der Warnungsüberprüfung eingeführt. Nachdem Sie sich mit dieser Überprüfung vertraut gemacht haben, können Sie mit den folgenden Artikeln fortfahren:

* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts): Erfahren Sie, wie Sie Warnungen verwalten und auf Sicherheitsvorfälle in Security Center reagieren.
* [Überwachen der Sicherheitsintegrität in Azure Security Center:](security-center-monitoring.md) Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verstehen der Sicherheitswarnungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type): Hier finden Sie Informationen zu den unterschiedlichen Arten von Sicherheitswarnungen.
* [Azure Security Center – Handbuch zur Problembehandlung](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide): Hier erfahren Sie, wie Sie allgemeine Probleme in Security Center behandeln.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](https://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.

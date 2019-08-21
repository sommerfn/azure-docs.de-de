---
title: 'Azure Service Fabric: Konfigurieren eines vorhandenen Azure Service Fabric-Clusters zum Aktivieren der Unterstützung für verwaltete Identitäten | Microsoft-Dokumentation'
description: In diesem Artikel erfahren Sie, wie Sie einen vorhandenen Azure Service Fabric-Cluster für die Unterstützung verwalteter Identitäten konfigurieren.
services: service-fabric
author: athinanthny
ms.service: service-fabric
ms.topic: article
ms.date: 07/25/2019
ms.author: atsenthi
ms.openlocfilehash: dc9d7ba9499f2c98370b14fd88d38dffc65480e5
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68964035"
---
# <a name="configure-an-existing-azure-service-fabric-cluster-to-enable-managed-identity-support"></a>Aktivieren eines vorhandenen Azure Service Fabric-Clusters für die Unterstützung verwalteter Identitäten
Um auf die Funktion für verwaltete Identitäten für Azure Service Fabric-Anwendungen zuzugreifen, müssen Sie zunächst den **Tokendienst für verwaltete Identitäten** im Cluster aktivieren. Dieser Dienst ist für die Authentifizierung von Service Fabric-Anwendungen anhand ihrer verwalteten Identitäten und für das Abrufen von Zugriffstoken in ihrem Namen zuständig. Nachdem der Dienst aktiviert wurde, wird er im Service Fabric Explorer im Abschnitt **System** im linken Bereich unter dem Namen **fabric:/System/ManagedIdentityTokenService** angezeigt.

> [!NOTE]
> Zum Aktivieren des **Tokendiensts für verwaltete Identitäten** ist mindestens Version 6.5.658.9590 der Service Fabric-Runtime erforderlich.  
> 
> Sie finden die Service Fabric-Version eines Clusters im Azure-Portal. Öffnen Sie dazu dort die Clusterressource, und überprüfen Sie die Eigenschaft **Service Fabric-Version** im Abschnitt **Zusammenfassung**.
> 
> Wenn sich der Cluster im **manuellen** Upgrademodus befindet, müssen Sie ihn zunächst auf Version 6.5.658.9590 oder höher aktualisieren.


## <a name="enable-the-managed-identity-token-service-in-an-existing-cluster"></a>Aktivieren des Tokendiensts für verwaltete Identitäten in einem vorhandenen Cluster
Um den Tokendienst für verwaltete Identitäten in einem vorhandenen Cluster zu aktivieren, müssen Sie ein Clusterupgrade initiieren und dabei zwei Änderungen angeben: Aktivieren Sie den Tokendienst für verwaltete Identitäten, und fordern Sie einen Neustart jedes einzelnen Knotens an. Fügen Sie dazu die folgenden beiden Codeausschnitte in der Azure Resource Manager-Vorlage hinzu:

```json
"fabricSettings": [
    {
        "name": "ManagedIdentityTokenService",
        "parameters": [
            {
                "name": "IsEnabled",
                "value": "true"
            }
        ]
    }
]
```

Damit die Änderungen wirksam werden, müssen Sie auch die Upgraderichtlinie ändern, um auf jedem Knoten einen Neustart der Service Fabric-Runtime zu erzwingen, wenn das Upgrade den Cluster durchläuft. Mit diesem Neustart wird sichergestellt, dass der neu aktivierte Systemdienst auf jedem Knoten gestartet und ausgeführt wird. Im folgenden Codeausschnitt ist `forceRestart` die wesentliche Einstellung. Verwenden Sie Ihre vorhandenen Werte für die übrigen Einstellungen.  

```json
"upgradeDescription": {
    "forceRestart": true,
    "healthCheckRetryTimeout": "00:45:00",
    "healthCheckStableDuration": "00:05:00",
    "healthCheckWaitDuration": "00:05:00",
    "upgradeDomainTimeout": "02:00:00",
    "upgradeReplicaSetCheckTimeout": "1.00:00:00",
    "upgradeTimeout": "12:00:00"
}
```

> [!NOTE]
> Vergessen Sie nach dem erfolgreichen Abschluss des Upgrades nicht, ein Rollback für die Einstellung `forceRestart` auszuführen, um die Auswirkungen nachfolgender Upgrades zu minimieren. 

## <a name="errors-and-troubleshooting"></a>Fehler und Troubleshooting

Wenn die Bereitstellung mit der folgenden Meldung zu einem Fehler führt, bedeutet dies, dass im Cluster eine zu niedrige Service Fabric-Version ausgeführt wird:

```json
{
    "code": "ParameterNotAllowed",
    "message": "Section 'ManagedIdentityTokenService' and Parameter 'IsEnabled' is not allowed."
}
```

## <a name="next-steps"></a>Nächste Schritte
* [Bereitstellen einer Azure Service Fabric-Anwendung mit einer systemseitig zugewiesenen verwalteten Identität](./how-to-deploy-service-fabric-application-system-assigned-managed-identity.md)
* [Bereitstellen einer Azure Service Fabric-Anwendung mit einer benutzerseitig zugewiesenen verwalteten Identität](./how-to-deploy-service-fabric-application-user-assigned-managed-identity.md)
* [Nutzen der verwalteten Identität einer Service Fabric-Anwendung aus dem Dienstcode](./how-to-managed-identity-service-fabric-app-code.md)
* [Gewähren des Zugriffs auf andere Azure-Ressourcen für eine Azure Service Fabric-Anwendung](./how-to-grant-access-other-resources.md)

## <a name="related-articles"></a>Verwandte Artikel
* Überprüfen der [Unterstützung für verwaltete Identitäten](./concepts-managed-identity.md) in Azure Service Fabric

* [Aktivieren der Unterstützung für verwaltete Identitäten in einem vorhandenen Azure Service Fabric-Cluster](./configure-existing-cluster-enable-managed-identity-token-service.md)

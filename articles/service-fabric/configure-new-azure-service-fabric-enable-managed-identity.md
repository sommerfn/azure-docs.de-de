---
title: 'Azure Service Fabric: Bereitstellen eines neuen Azure Service Fabric-Clusters mit Unterstützung der verwalteten Identität | Microsoft-Dokumentation'
description: In diesem Artikel erfahren Sie, wie Sie einen neuen Service Fabric-Cluster mit aktivierter verwalteter Identität erstellen.
services: service-fabric
author: athinanthny
ms.service: service-fabric
ms.topic: article
ms.date: 07/25/2019
ms.author: atsenthi
ms.openlocfilehash: 24fcdaf612a26109194524733e1fb9069dc664e0
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68965573"
---
# <a name="create-a-new-azure-service-fabric-cluster-with-managed-identity-support"></a>Erstellen eines neuen Azure Service Fabric-Clusters mit Unterstützung der verwalteten Identität

Um auf das Feature für verwaltete Identitäten für Azure Service Fabric-Anwendungen zugreifen zu können, müssen Sie zunächst den Tokendienst für verwaltete Identitäten im Cluster aktivieren. Dieser Dienst ist für die Authentifizierung von Service Fabric-Anwendungen anhand ihrer verwalteten Identitäten und für den Abruf von Zugriffstoken in deren Auftrag zuständig. Nachdem der Dienst aktiviert wurde, wird er in Service Fabric Explorer im Abschnitt **System** im linken Bereich unter dem Namen **fabric:/System/ManagedIdentityTokenService** neben anderen Systemdiensten angezeigt.

> [!NOTE]
> Zum Aktivieren des **Tokendiensts für verwaltete Identitäten** ist mindestens Version 6.5.658.9590 der Service Fabric-Runtime erforderlich.  

## <a name="enable-the-managed-identity-token-service"></a>Aktivieren des Tokendiensts für verwaltete Identitäten 
Sie können den folgenden Codeausschnitt in einer Azure Resource Manager-Vorlage verwenden, um den Tokendienst für verwaltete Identitäten bei der Clustererstellung zu aktivieren:

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

## <a name="errors"></a>Fehler

Sollte bei der Bereitstellung der folgende Fehler auftreten, verfügt der Cluster nicht über die erforderliche Service Fabric-Version. (Die niedrigste unterstützte Runtimeversion ist 6.5 CU2.)



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
* Machen Sie sich mit der [Unterstützung der verwalteten Identität](./concepts-managed-identity.md) in Azure Service Fabric vertraut.

* [Aktivieren Sie die Unterstützung der verwalteten Identität in einem bereits vorhandenen Azure Service Fabric-Cluster.](./configure-existing-cluster-enable-managed-identity-token-service.md)

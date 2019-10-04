---
title: 'Azure Service Fabric: Konfigurieren der Anmeldeinformationen des Containerrepositorys | Microsoft-Dokumentation'
description: Konfigurieren der Anmeldeinformationen des Repositorys zum Herunterladen von Images aus der Containerregistrierung
services: service-fabric
documentationcenter: .net
author: arya
manager: gkhanna
ms.assetid: b93d31e5-9e4c-4405-b266-c0efa4643d97
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 8/1/2019
ms.author: arya
ms.openlocfilehash: cfe212a150da0e5828f48de3bf2692ab2a44c672
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2019
ms.locfileid: "69656869"
---
# <a name="configure-repository-credentials-for-your-application-to-download-container-images"></a>Konfigurieren der Anmeldeinformationen des Repositorys für Ihre Anwendung zum Herunterladen von Containerimages

Konfigurieren Sie die Authentifizierung der Containerregistrierung, indem Sie dem `ContainerHostPolicies`-Element der Datei „ApplicationManifest.xml“ ein `RepositoryCredentials`-Element hinzufügen. Fügen Sie das Konto und Kennwort für die Containerregistrierung „myregistry.azurecr.io“ hinzu, damit der Dienst das Containerimage aus dem Repository herunterladen kann.

```xml
<ServiceManifestImport>
    ...
    <Policies>
        <ContainerHostPolicies CodePackageRef="Code">
            <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
            <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
        </ContainerHostPolicies>
    </Policies>
    ...
</ServiceManifestImport>
```

Es wird empfohlen, das Repositorykennwort mithilfe eines Verschlüsselungszertifikats zu verschlüsseln, das auf allen Knoten des Clusters bereitgestellt wird. Bei der Bereitstellung des Dienstpakets für den Cluster durch Service Fabric wird das Verschlüsselungszertifikat zum Entschlüsseln des Verschlüsselungstexts verwendet. Das Cmdlet „Invoke-ServiceFabricEncryptText“ wird zum Verschlüsseln des Texts für das Kennwort verwendet, das der Datei „ApplicationManifest.xml“ hinzugefügt wird.
Weitere Informationen zu Zertifikaten und zur Verschlüsselungssemantik finden Sie unter [Verwalten von Geheimnissen](service-fabric-application-secret-management.md).

## <a name="configure-cluster-wide-credentials"></a>Konfigurieren von Anmeldeinformationen für den gesamten Cluster

Mit Service Fabric können Sie Anmeldeinformationen konfigurieren, die für den gesamten Cluster gelten und von Anwendungen als Standardanmeldeinformationen für Repositorys verwendet werden können.

Dieses Feature kann aktiviert oder deaktiviert werden, indem Sie in der Datei „ApplicationManifest.xml“ dem `ContainerHostPolicies`-Element das Attribut `UseDefaultRepositoryCredentials` mit dem Wert `true` oder `false` hinzufügen.

```xml
<ServiceManifestImport>
    ...
    <Policies>
        <ContainerHostPolicies CodePackageRef="Code" UseDefaultRepositoryCredentials="true">
            <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
        </ContainerHostPolicies>
    </Policies>
    ...
</ServiceManifestImport>
```

In Service Fabric werden dann die Standardanmeldeinformationen für Repositorys verwendet, die im ClusterManifest im Abschnitt `Hosting` angegeben werden können.  Wenn `UseDefaultRepositoryCredentials` auf `true` festgelegt ist, liest Service Fabric die folgenden Werte aus dem ClusterManifest:

* DefaultContainerRepositoryAccountName (Zeichenfolge)
* DefaultContainerRepositoryPassword (Zeichenfolge)
* IsDefaultContainerRepositoryPasswordEncrypted (Boolescher Wert)
* DefaultContainerRepositoryPasswordType (Zeichenfolge) – Wird ab Version 6.4 der Runtime unterstützt

Hier ist ein Beispiel dafür angegeben, was in der Datei „ClusterManifestTemplate.json“ im Abschnitt `Hosting` hinzugefügt werden kann. Der `Hosting`-Abschnitt kann bei der Clustererstellung oder später in einem Konfigurationsupgrade hinzugefügt werden. Weitere Informationen finden Sie unter [Ändern von Azure Service Fabric-Clustereinstellungen](service-fabric-cluster-fabric-settings.md) und [Verwalten von Azure Service Fabric-Anwendungsgeheimnissen](service-fabric-application-secret-management.md).

```json
"fabricSettings": [
    ...,
    {
        "name": "Hosting",
        "parameters": [
          {
            "name": "EndpointProviderEnabled",
            "value": "true"
          },
          {
            "name": "DefaultContainerRepositoryAccountName",
            "value": "someusername"
          },
          {
            "name": "DefaultContainerRepositoryPassword",
            "value": "somepassword"
          },
          {
            "name": "IsDefaultContainerRepositoryPasswordEncrypted",
            "value": "false"
          },
          {
            "name": "DefaultContainerRepositoryPasswordType",
            "value": "PlainText"
          }
        ]
      },
]
```

## <a name="leveraging-the-managed-identity-of-the-virtual-machine-scale-set-by-using-managed-identity-service-msi"></a>Nutzen der verwalteten Identität der VM-Skalierungsgruppe mithilfe des Diensts für verwaltete Identitäten (MSI)

Service Fabric unterstützt die Verwendung von Token als Anmeldeinformationen zum Herunterladen von Images für Ihre Container.  Diese Funktion nutzt die verwaltete Identität der zugrunde liegenden VM-Skalierungsgruppe, um sich bei der Registrierung zu authentifizieren, sodass keine Benutzeranmeldeinformationen mehr verwaltet werden müssen.  Weitere Informationen zu MSI finden Sie unter [Verwaltete Dienstidentität](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).  Zur Verwendung dieser Funktion sind die folgenden Schritte erforderlich:

1.  Stellen Sie sicher, dass „Systemseitig zugewiesene verwaltete Identität“ für den virtuellen Computer aktiviert ist (siehe Screenshot unten).

    ![Erstellen einer Identität für die VM-Skalierungsgruppe](./media/configure-container-repository-credentials/configure-container-repository-credentials-acr-iam.png)

2.  Anschließend erteilen Sie dem virtuellen Computer (der VM-Skalierungsgruppe) Berechtigungen zum Abrufen und Lesen von Images aus der Registrierung.  Wechseln Sie über das Azure-Blatt zur Zugriffssteuerung (IAM) Ihrer ACR, und erteilen Sie Ihrem virtuellen Computer (der VM-Skalierungssgruppe) die richtigen Berechtigungen wie unten gezeigt:

    ![Hinzufügen des VM-Prinzipals zu ACR](./media/configure-container-repository-credentials/configure-container-repository-credentials-vmss-identity.png)

3.  Nachdem Sie die Schritte oben abgeschlossen haben, ändern Sie die Datei „applicationmanifest.xml“.  Suchen Sie das Tag mit der Bezeichnung „ContainerHostPolicies“, und fügen Sie das Attribut `‘UseTokenAuthenticationCredentials=”true”` hinzu.

    ```json
      <ServiceManifestImport>
          <ServiceManifestRef ServiceManifestName="NodeServicePackage" ServiceManifestVersion="1.0"/>
      <Policies>
        <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" UseTokenAuthenticationCredentials="true">
          <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
        </ContainerHostPolicies>
        <ResourceGovernancePolicy CodePackageRef="NodeService.Code" MemoryInMB="256"/>
      </Policies>
      </ServiceManifestImport>
    ```

    > [!NOTE]
    > Wenn die Flags `UseDefaultRepositoryCredentials` und `UseTokenAuthenticationCredentials` beide auf „true“ festgelegt sind, tritt ein Fehler während der Bereitstellung auf.

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über die [Authentifizierung der Containerregistrierung](/azure/container-registry/container-registry-authentication).

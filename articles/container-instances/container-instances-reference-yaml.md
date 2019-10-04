---
title: Azure Container Instances – YAML-Referenz
description: Referenz für die von Azure Container Instances zum Konfigurieren einer Containergruppe unterstützte YAML-Datei
services: container-instances
author: dlepow
manager: gwallace
ms.service: container-instances
ms.topic: article
ms.date: 08/12/2019
ms.author: danlep
ms.openlocfilehash: 2e6be18371cf3ff96d1ce91d4dde26ff1f14021b
ms.sourcegitcommit: 83df2aed7cafb493b36d93b1699d24f36c1daa45
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2019
ms.locfileid: "71179993"
---
# <a name="yaml-reference-azure-container-instances"></a>YAML-Referenz: Azure Container Instances

Dieser Artikel behandelt die Syntax und Eigenschaften für die von Azure Container Instances zum Konfigurieren einer [Containergruppe](container-instances-container-groups.md) unterstützte YAML-Datei. Verwenden Sie eine YAML-Datei, um die Gruppenkonfiguration in den Befehl [az container create][az-container-create] in der Azure CLI einzugeben. 

Eine YAML-Datei ist eine bequeme Möglichkeit, um eine Containergruppe für reproduzierbare Bereitstellungen zu konfigurieren. Dies ist eine präzise Alternative zur Verwendung einer [Resource Manager-Vorlage](/azure/templates/Microsoft.ContainerInstance/2018-10-01/containerGroups) oder des Azure Container Instances SDKs zum Erstellen oder Aktualisieren einer Containergruppe.

> [!NOTE]
> Diese Referenz gilt für YAML-Dateien für Azure Container Instances-REST-API-Version `2018-10-01`.

## <a name="schema"></a>Schema 

Im Folgenden finden Sie das Schema für die YAML-Datei, einschließlich Kommentaren zur Hervorhebung von wichtigen Eigenschaften. Eine Beschreibung der Eigenschaften in diesem Schema finden Sie im Abschnitt [Eigenschaftswerte](#property-values).

```yml
name: string  # Name of the container group
apiVersion: '2018-10-01'
location: string
tags: {}
identity: 
  type: string
  userAssignedIdentities: {}
properties: # Properties of container group
  containers: # Array of container instances in the group
  - name: string # Name of an instance
    properties: # Properties of an instance
      image: string # Container image used to create the instance
      command:
      - string
      ports: # Exposed ports on the instance
      - protocol: string
        port: integer
      environmentVariables:
      - name: string
        value: string
        secureValue: string
      resources: # Resource requirements of the instance
        requests:
          memoryInGB: number
          cpu: number
          gpu:
            count: integer
            sku: string
        limits:
          memoryInGB: number
          cpu: number
          gpu:
            count: integer
            sku: string
      volumeMounts: # Array of volume mounts for the instance
      - name: string
        mountPath: string
        readOnly: boolean
      livenessProbe:
        exec:
          command:
          - string
        httpGet:
          path: string
          port: integer
          scheme: string
        initialDelaySeconds: integer
        periodSeconds: integer
        failureThreshold: integer
        successThreshold: integer
        timeoutSeconds: integer
      readinessProbe:
        exec:
          command:
          - string
        httpGet:
          path: string
          port: integer
          scheme: string
        initialDelaySeconds: integer
        periodSeconds: integer
        failureThreshold: integer
        successThreshold: integer
        timeoutSeconds: integer
  imageRegistryCredentials: # Credentials to pull a private image
  - server: string
    username: string
    password: string
  restartPolicy: string
  ipAddress: # IP address configuration of container group
    ports:
    - protocol: string
      port: integer
    type: string
    ip: string
    dnsNameLabel: string
  osType: string
  volumes: # Array of volumes available to the instances
  - name: string
    azureFile:
      shareName: string
      readOnly: boolean
      storageAccountName: string
      storageAccountKey: string
    emptyDir: {}
    secret: {}
    gitRepo:
      directory: string
      repository: string
      revision: string
  diagnostics:
    logAnalytics:
      workspaceId: string
      workspaceKey: string
      logType: string
      metadata: {}
  networkProfile: # Virtual network profile for container group
    id: string
  dnsConfig: # DNS configuration for container group
    nameServers:
    - string
    searchDomains: string
    options: string
```

## <a name="property-values"></a>Eigenschaftswerte

In den folgenden Tabellen sind die Werte beschrieben, die Sie im Schema festlegen müssen.

<a id="Microsoft.ContainerInstance/containerGroups" />

### <a name="microsoftcontainerinstancecontainergroups-object"></a>Microsoft.ContainerInstance/containerGroups-Objekt

|  NAME | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  name | Zeichenfolge | Ja | Der Name der Containergruppe. |
|  apiVersion | enum | Ja | 01.10.2018 |
|  location | Zeichenfolge | Nein | Der Ressourcenspeicherort. |
|  tags | object | Nein | Die Ressourcentags. |
|  identity | object | Nein | Die Identität der Containergruppe, sofern konfiguriert. - [-Objekt](#ContainerGroupIdentity) |
|  properties | object | Ja | [ContainerGroupProperties-Objekt](#ContainerGroupProperties) |


<a id="ContainerGroupIdentity" />

### <a name="containergroupidentity-object"></a>ContainerGroupIdentity-Objekt

|  NAME | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  type | enum | Nein | Der für die Containergruppe verwendete Identitätstyp. Der Typ „SystemAssigned, UserAssigned“ umfasst sowohl eine implizit erstellte Identität als auch einen Satz von Benutzern zugewiesener Identitäten. Der Typ „None“ entfernt alle Identitäten aus der Containergruppe. – SystemAssigned, UserAssigned, SystemAssigned, UserAssigned, None |
|  userAssignedIdentities | object | Nein | Die Liste der der Containergruppe zugeordneten Benutzeridentitäten. Die Schlüsselverweise des Benutzeridentitäts-Wörterbuchs sind Azure Resource Manager-Ressourcen-IDs im Format: '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ManagedIdentity/userAssignedIdentities/{identityName}'. |


<a id="ContainerGroupProperties" />

### <a name="containergroupproperties-object"></a>ContainerGroupProperties-Objekt

|  NAME | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  containers | array | Ja | Die Container innerhalb der Containergruppe. - [Container-Objekt](#Container) |
|  imageRegistryCredentials | array | Nein | Die Anmeldeinformationen für die Imageregistrierung, mit denen die Containergruppe erstellt wird. - [ImageRegistryCredential-Objekt](#ImageRegistryCredential) |
|  restartPolicy | enum | Nein | Neustartrichtlinie für alle Container innerhalb der Containergruppe. - `Always` Immer neu starten- `OnFailure` Neustart bei Fehler- `Never` Nie neu starten. – Always, OnFailure, Never |
|  ipAddress | object | Nein | Der IP-Adressentyp der Containergruppe. - [IpAddress-Objekt](#IpAddress) |
|  osType | enum | Ja | Der für die Container in der Containergruppe erforderliche Betriebssystemtyp. – Windows oder Linux |
|  volumes | array | Nein | Die Liste der Volumes, die von Containern in dieser Containergruppe eingebunden werden können. - [Volume-Objekt](#Volume) |
|  Diagnose | object | Nein | Die Diagnoseinformationen für eine Containergruppe. - [ContainerGroupDiagnostics-Objekt](#ContainerGroupDiagnostics) |
|  networkProfile | object | Nein | Die Netzwerkprofilinformationen für eine Containergruppe. - [ContainerGroupNetworkProfile-Objekt](#ContainerGroupNetworkProfile) |
|  dnsConfig | object | Nein | Die DNS-Konfigurationsinformationen für eine Containergruppe. - [DnsConfiguration-Objekt](#DnsConfiguration) |


<a id="Container" />

### <a name="container-object"></a>Container-Objekt

|  NAME | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  name | Zeichenfolge | Ja | Der vom Benutzer bereitgestellte Name der Containerinstanz. |
|  properties | object | Ja | Der Eigenschaften der Containerinstanz. - [ContainerProperties-Objekt](#ContainerProperties) |


<a id="ImageRegistryCredential" />

### <a name="imageregistrycredential-object"></a>ImageRegistryCredential-Objekt

|  NAME | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  server | Zeichenfolge | Ja | Der Docker-Imageregistrierungsserver ohne ein Protokoll wie „HTTP“ oder „HTTPS“. |
|  username | Zeichenfolge | Ja | Der Benutzername für die private Registrierung. |
|  password | Zeichenfolge | Nein | Das Kennwort für die private Registrierung. |


<a id="IpAddress" />

### <a name="ipaddress-object"></a>IpAddress-Objekt

|  NAME | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  ports | array | Ja | Die Liste der in der Containergruppe verfügbar gemachten Ports. - [Port-Objekte](#Port) |
|  type | enum | Ja | Gibt an, ob die IP dem öffentlichen Internet oder dem privaten VNET verfügbar gemacht ist. – Public oder Private |
|  ip | Zeichenfolge | Nein | Die dem öffentlichen Internet verfügbar gemachte IP-Adresse. |
|  dnsNameLabel | Zeichenfolge | Nein | Die DNS-Namensbezeichnung für die IP-Adresse. |


<a id="Volume" />

### <a name="volume-object"></a>Volume-Objekt

|  NAME | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  name | Zeichenfolge | Ja | Der Name des Volumes. |
|  azureFile | object | Nein | Das Azure File-Volume. - [AzureFileVolume-Objekt](#AzureFileVolume) |
|  emptyDir | object | Nein | Das leere Verzeichnis-Volume. |
|  secret | object | Nein | Das geheime Volume. |
|  gitRepo | object | Nein | Das Git-Repository-Volume. - [GitRepoVolume-Objekt](#GitRepoVolume) |


<a id="ContainerGroupDiagnostics" />

### <a name="containergroupdiagnostics-object"></a>ContainerGroupDiagnostics-Objekt

|  NAME | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  logAnalytics | object | Nein | Containergruppen-Protokollanalyseinformationen. - [LogAnalytics-Objekt](#LogAnalytics) |


<a id="ContainerGroupNetworkProfile" />

### <a name="containergroupnetworkprofile-object"></a>ContainerGroupNetworkProfile-Objekt

|  NAME | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  id | Zeichenfolge | Ja | Der Bezeichner für ein Netzwerkprofil. |


<a id="DnsConfiguration" />

### <a name="dnsconfiguration-object"></a>DnsConfiguration-Objekt

|  NAME | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  nameServers | array | Ja | Die DNS-Server für die Containergruppe. – string |
|  searchDomains | Zeichenfolge | Nein | Die DNS-Suchdomänen für das Nachschlagen von Hostnamen in der Containergruppe. |
|  options | Zeichenfolge | Nein | Die DNS-Optionen für die Containergruppe. |


<a id="ContainerProperties" />

### <a name="containerproperties-object"></a>ContainerProperties-Objekt

|  NAME | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  image | Zeichenfolge | Ja | Der Name des zum Erstellen der Containerinstanz verwendeten Images. |
|  command | array | Nein | Die innerhalb der Containerinstanz auszuführenden Befehle im EXEC-Format. – string |
|  ports | array | Nein | Die in der Containerinstanz verfügbar gemachten Ports. - [ContainerPort-Objekt](#ContainerPort) |
|  environmentVariables | array | Nein | Die in der Containerinstanz festzulegenden Umgebungsvariablen. - [EnvironmentVariable-Objekt](#EnvironmentVariable) |
|  ressourcen | object | Ja | Die Ressourcenanforderungen der Containerinstanz. - [ResourceRequirements-Objekt](#ResourceRequirements) |
|  volumeMounts | array | Nein | Die für die Containerinstanz verfügbaren Volumebereitstellungen. - [VolumeMount-Objekt](#VolumeMount) |
|  livenessProbe | object | Nein | Der Livetest. - [ContainerProbe-Objekt](#ContainerProbe) |
|  readinessProbe | object | Nein | Der Bereitschaftstest. - [ContainerProbe-Objekt](#ContainerProbe) |


<a id="Port" />

### <a name="port-object"></a>Port-Objekt

|  NAME | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  protocol | enum | Nein | Das dem Port zugeordnete Protokoll. – TCP oder UDP |
|  port | integer | Ja | Die Portnummer. |


<a id="AzureFileVolume" />

### <a name="azurefilevolume-object"></a>AzureFileVolume-Objekt

|  NAME | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  shareName | Zeichenfolge | Ja | Der Name der als Volume bereitzustellenden Azure File-Freigabe. |
|  readOnly | boolean | Nein | Das Flag, das anzeigt, ob die als Volume bereitgestellte Azure File-Freigabe schreibgeschützt ist. |
|  storageAccountName | Zeichenfolge | Ja | Der Name des Speicherkontos, das die Azure File-Freigabe enthält. |
|  storageAccountKey | Zeichenfolge | Nein | Der Zugriffsschlüssel des Speicherkontos, der für den Zugriff auf die Azure File-Freigabe verwendet wird. |


<a id="GitRepoVolume" />

### <a name="gitrepovolume-object"></a>GitRepoVolume-Objekt

|  NAME | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  Verzeichnis | Zeichenfolge | Nein | Zielverzeichnisname. Darf „..“ weder enthalten noch damit beginnen.  Wenn „.“ angegeben wird, ist das Volumeverzeichnis das Git-Repository.  Andernfalls, falls angegeben, enthält das Volume das Git-Repository in einem Unterverzeichnis mit dem angegebenen Namen. |
|  repository | Zeichenfolge | Ja | Repository-URL |
|  revision | Zeichenfolge | Nein | Commit-Hash für die angegebene Revision. |


<a id="LogAnalytics" />

### <a name="loganalytics-object"></a>LogAnalytics-Objekt

|  NAME | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  workspaceId | Zeichenfolge | Ja | Die Arbeitsbereichs-ID für Log Analytics. |
|  workspaceKey | Zeichenfolge | Ja | Der Arbeitsbereichsschlüssel für Log Analytics. |
|  logType | enum | Nein | Der zu verwendende Protokolltyp. – ContainerInsights oder ContainerInstanceLogs |
|  metadata | object | Nein | Metadaten für Log Analytics. |


<a id="ContainerPort" />

### <a name="containerport-object"></a>ContainerPort-Objekt

|  NAME | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  protocol | enum | Nein | Das dem Port zugeordnete Protokoll. – TCP oder UDP |
|  port | integer | Ja | Die innerhalb der Containergruppe verfügbar gemachte Portnummer. |


<a id="EnvironmentVariable" />

### <a name="environmentvariable-object"></a>EnvironmentVariable-Objekt

|  NAME | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  name | Zeichenfolge | Ja | Der Name der Umgebungsvariablen. |
|  value | Zeichenfolge | Nein | Der Wert der Umgebungsvariablen. |
|  secureValue | Zeichenfolge | Nein | Der Wert der sicheren Umgebungsvariablen. |


<a id="ResourceRequirements" />

### <a name="resourcerequirements-object"></a>ResourceRequirements-Objekt

|  NAME | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  requests | object | Ja | Die Ressourcenanforderungen dieser Containerinstanz. - [ResourceRequests-Objekt](#ResourceRequests) |
|  Grenzwerte | object | Nein | Die Ressourcenlimits dieser Containerinstanz. - [ResourceLimits-Objekt](#ResourceLimits) |


<a id="VolumeMount" />

### <a name="volumemount-object"></a>VolumeMount-Objekt

|  NAME | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  name | Zeichenfolge | Ja | Der Name der Volumebereitstellung. |
|  mountPath | Zeichenfolge | Ja | Der Pfad innerhalb des Containers, in dem das Volume bereitgestellt werden soll. Darf keinen Doppelpunkt (:) enthalten. |
|  readOnly | boolean | Nein | Das Flag, das anzeigt, ob die Volumebereitstellung schreibgeschützt ist. |


<a id="ContainerProbe" />

### <a name="containerprobe-object"></a>ContainerProbe-Objekt

|  NAME | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  exec | object | Nein | Der zu testende Ausführungsbefehl – [ContainerExec-Objekt](#ContainerExec) |
|  httpGet | object | Nein | Die zu testenden HTTP GET-Einstellungen – [ContainerHttpGet-Objekt](#ContainerHttpGet) |
|  initialDelaySeconds | integer | Nein | Die anfänglichen Verzögerungssekunden. |
|  periodSeconds | integer | Nein | Die Zeitraumsekunden. |
|  failureThreshold | integer | Nein | Der Fehlerschwellenwert. |
|  successThreshold | integer | Nein | Der Schwellenwert für erfolgreiche Tests. |
|  timeoutSeconds | integer | Nein | Die Timeoutsekunden. |


<a id="ResourceRequests" />

### <a name="resourcerequests-object"></a>ResourceRequests-Objekt

|  NAME | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  memoryInGB | number | Ja | Die Arbeitsspeicheranforderung dieser Containerinstanz in GB. |
|  cpu | number | Ja | Die CPU-Anforderungen dieser Containerinstanz. |
|  gpu | object | Nein | Die GPU-Anforderungen dieser Containerinstanz. - [GpuResource-Objekt](#GpuResource) |


<a id="ResourceLimits" />

### <a name="resourcelimits-object"></a>ResourceLimits-Objekt

|  NAME | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  memoryInGB | number | Nein | Das Arbeitsspeicherlimit dieser Containerinstanz in GB. |
|  cpu | number | Nein | Das CPU-Limit dieser Containerinstanz. |
|  gpu | object | Nein | Das GPU-Limit dieser Containerinstanz. - [GpuResource-Objekt](#GpuResource) |


<a id="ContainerExec" />

### <a name="containerexec-object"></a>ContainerExec-Objekt

|  NAME | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  command | array | Nein | Die innerhalb des Containers auszuführenden Befehle. – string |


<a id="ContainerHttpGet" />

### <a name="containerhttpget-object"></a>ContainerHttpGet-Objekt

|  NAME | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  path | Zeichenfolge | Nein | Der zu testende Pfad. |
|  port | integer | Ja | Die zu testende Portnummer. |
|  scheme | enum | Nein | Das Schema. – HTTP oder HTTPS |


<a id="GpuResource" />

### <a name="gpuresource-object"></a>GpuResource-Objekt

|  NAME | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  count | integer | Ja | Die Anzahl der GPU-Ressourcen. |
|  sku | enum | Ja | Die SKU der GPU-Ressource. – K80, P100, V100 |


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie im Tutorial [Bereitstellen einer Gruppe mit mehreren Containern mithilfe einer YAML-Datei](container-instances-multi-container-yaml.md).

Beispiele für die Verwendung einer YAML-Datei zum Bereitstellen von Containergruppen in einem [virtuellen Netzwerk](container-instances-vnet.md) oder für das [Einbinden eines externen Volumes](container-instances-volume-azure-files.md).

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create


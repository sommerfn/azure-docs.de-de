---
title: Azure Connected Machine Agent CLI-Schnittstelle
description: Referenzdokumentation für die Azure Connected Machine Agent CLI
author: bobbytreed
manager: carmonm
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
ms.topic: reference
ms.date: 11/04/2019
ms.author: robreed
ms.openlocfilehash: d35c5e283f2e1e2f8afd431d83775167dc2a531a
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510395"
---
# <a name="azure-connected-machine-agent-cli-interface"></a>Azure Connected Machine Agent CLI-Schnittstelle

Das `Azcmagent`-Tool (Azure Connected Machine Agent) wird verwendet, um eine Nicht-Azure-Computerverbindung mit Azure zu konfigurieren und diesbezügliche Probleme zu behandeln.

Der Agent selbst ist ein Daemonprozess namens `himdsd` unter Linux und ein Windows-Dienst namens `himds` unter Windows.

Bei normalem Betrieb wird `azcmagent connect` verwendet, um eine Verbindung zwischen diesem Computer und Azure herzustellen, und `azcmagent disconnect`, wenn Sie feststellen, dass Sie diese Verbindung nicht mehr benötigen. Die anderen Befehle dienen der Problembehandlung oder sind für andere Sonderfälle gedacht.

## <a name="options"></a>Optionen

```none
  -h, --help      help for azcmagent
  -v, --verbose   Increase logging verbosity to show all logs
```

## <a name="see-also"></a>SIEHE AUCH

* [azcmagent connect-](#azcmagent-connect): Verbindet diesen Computer mit Azure.
* [azcmagent disconnect](#azcmagent-disconnect): Trennt diesen Computer von Azure.
* [azcmagent reconnect](#azcmagent-reconnect): Verbindet diesen Computer erneut mit Azure.
* [azcmagent show](#azcmagent-show): Ruft Computermetadaten und den Agent-Status ab. Dies ist vor allem für die Problembehandlung nützlich.
* [azcmagent version](#azcmagent-version): Zeigt die Version des Hybrid Management-Agents an.

## <a name="azcmagent-connect"></a>azcmagent connect:

Verbindet diesen Computer mit Azure.

### <a name="synopsis"></a>Zusammenfassung

Erstellt eine Ressource in Azure, die diesen Computer darstellt.

Dabei werden die angegebenen Authentifizierungsoptionen verwendet, um eine Ressource in Azure Resource Manager zu erstellen, die diesen Computer darstellt. Die Ressource befindet sich im angeforderten Abonnement und der Ressourcengruppe, und die Daten zum Computer werden in der durch den location-Parameter angegebenen Azure-Region gespeichert.
Der Ressourcenstandardname ist der Hostname dieses Computers, wenn dieser nicht überschrieben wird.

Ein Zertifikat, das der vom System zugewiesenen Identität dieses Computers entspricht, wird dann heruntergeladen und lokal gespeichert. Sobald dieser Schritt abgeschlossen ist, beginnen der **Azure Connected Machine Metadata Service** und der Gastkonfigurations-Agent die Synchronisierung mit der Azure-Cloud.

Authentifizierungsoptionen:

* Zugriffstoken `azcmagent connect --access-token <> --subscription-id <> --resource-group <> --location <>`
* Dienstprinzipal-ID und Geheimnis `azcmagent connect --service-principal-id <> --service-principal-secret <> --tenant-id <tenantid> --subscription-id <> --resource-group <> --location <>`
* Geräteanmeldung (interaktiv) `azcmagent connect --tenant-id <> --subscription-id <> --resource-group <> --location <>`

### <a name="syntax"></a>Syntax

```none
azcmagent connect [flags]
```

### <a name="options"></a>Optionen

```none
      --access-token string               Access token
  -h, --help                              help for connect
  -l, --location string                   Location of the resource [Required]
      --physical-location string          Physical location of the resource
  -g, --resource-group string             Name of the resource group. [Required]
  -n, --resource-name string              Name of the resource. Defaults to Host Name
  -i, --service-principal-id string       Service Principal Id
  -p, --service-principal-secret string   Service Principal Secret
  -s, --subscription-id string            Subscription Id [Required]
  -t, --tags string                       Tags for resource
      --tenant-id string                  Tenant Id
```

## <a name="azcmagent-disconnect"></a>azcmagent disconnect:

Trennt diesen Computer von Azure.

### <a name="synopsis"></a>Zusammenfassung

Löscht die Ressource in Azure, die diesen Server darstellt.

Dieser Befehl verwendet die angegebenen Authentifizierungsoptionen, um eine Azure Resource Manager-Ressource zu entfernen, die diesen Computer darstellt. Anschließend werden der **Azure Connected Machine Metadata Service** und der Gastkonfigurations-Agent getrennt. Mit diesem Befehl werden die Dienste nicht beendet oder entfernt: Entfernen Sie zu diesem Zweck das Paket.

Dieser Befehl erfordert höhere Berechtigungen als die Rolle „Azure Connected Machine-Onboarding“.

Sobald ein Computer getrennt ist, verwenden Sie `azcmagent connect`und nicht `azcmagent reconnect`, wenn Sie für ihn eine neue Ressource in Azure erstellen möchten.

Authentifizierungsoptionen:

* Zugriffstoken `azcmagent disconnect --access-token <>`
* Dienstprinzipal-ID und Geheimnis `azcmagent disconnect --service-principal-id <> --service-principal-secret <> --tenant-id <tenantid>`
* Interaktive Geräteanmeldung `azcmagent disconnect --tenant-id <>`

### <a name="syntax"></a>Syntax

```none
azcmagent disconnect [flags]
```

### <a name="options"></a>Optionen

```none
      --access-token string               Access token
  -h, --help                              help for disconnect
  -r, --resource-group string             Name of the resource group
  -n, --resource-name string              Name of the resource
  -i, --service-principal-id string       Service Principal Id
  -p, --service-principal-secret string   Service Principal Secret
  -s, --subscription-id string            Subscription Id
  -t, --tenant-id string                  Tenant Id
```

## <a name="azcmagent-reconnect"></a>azcmagent reconnect:

Verbindet diesen Computer erneut mit Azure.

### <a name="synopsis"></a>Zusammenfassung

Erneutes Verbinden eines Computers mit ungültigen Anmeldeinformationen mit Azure.

Wenn ein Computer bereits über eine Ressource in Azure verfügt, sich aber nicht bei ihr authentifizieren kann, kann er mithilfe dieses Befehls erneut verbunden werden. Dies ist möglich, wenn ein Computer so lange ausgeschaltet war, dass das Zertifikat abgelaufen ist (mindestens 45 Tage).

Wenn ein Computer mit `azcmagent disconnect` getrennt wurde, verwenden Sie stattdessen `azcmagent connect`.

Dieser Befehl verwendet die angegebenen Authentifizierungsoptionen, um neue Anmeldeinformationen abzurufen, die der Azure Resource Manager-Ressource entsprechen, die diesen Computer darstellt.

Dieser Befehl erfordert höhere Berechtigungen als die Rolle **Azure Connected Machine-Onboarding**.

Authentifizierungsoptionen

* Zugriffstoken `azcmagent reconnect --access-token <>`
* Dienstprinzipal-ID und Geheimnis `azcmagent reconnect --service-principal-id <> --service-principal-secret <> --tenant-id <tenantid>`
* Interaktive Geräteanmeldung `azcmagent reconnect --tenant-id <>`

### <a name="syntax"></a>Syntax

```none
azcmagent reconnect [flags]
```

### <a name="options"></a>Optionen

```none
      --access-token string               Access token
  -h, --help                              help for reconnect
  -l, --location string                   Location of the resource
  -g, --resource-group string             Name of the resource group.
  -n, --resource-name string              Name of the resource. Defaults to Host Name
  -i, --service-principal-id string       Service Principal Id
  -p, --service-principal-secret string   Service Principal Secret
  -s, --subscription-id string            Subscription Id
      --tenant-id string                  tenant id
```

## <a name="azcmagent-show"></a>azcmagent show:

Ruft Computermetadaten und den Agent-Status ab. Dies ist vor allem für die Problembehandlung nützlich.

### <a name="synopsis"></a>Zusammenfassung

Ruft Computermetadaten und den Agent-Status ab. Dies ist vor allem für die Problembehandlung nützlich.


### <a name="syntax"></a>Syntax

```
azcmagent show [flags]
```

### <a name="options"></a>Optionen

```
  -h, --help   help for show
```

## <a name="azcmagent-version"></a>azcmagent version:

Zeigt die Version des Hybrid Management-Agents an.

### <a name="synopsis"></a>Zusammenfassung

Zeigt die Version des Hybrid Management-Agents an.

### <a name="syntax"></a>Syntax

```none
azcmagent version [flags]
```

### <a name="options"></a>Optionen

```none
  -h, --help   help for version
```

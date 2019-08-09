---
title: Authentifizierungsmethoden für Azure Security Center für IoT | Microsoft-Dokumentation
description: Lernen Sie die verschiedenen Authentifizierungsmethoden kennen, die für den Azure Security Center für IoT-Dienst verfügbar sind.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 10b38f20-b755-48cc-8a88-69828c17a108
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: 16f7f91e02d118d9f9a295ebb79a6cd0187dd9fd
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68596474"
---
# <a name="security-agent-authentication-methods"></a>Authentifizierungsmethoden des Sicherheits-Agents 

In diesem Artikel werden die verschiedenen Authentifizierungsmethoden erläutert, die Sie mit dem AzureIoTSecurity-Agent für die Authentifizierung beim IoT Hub verwenden können.

Für jedes Gerät, das im IoT Hub in Azure Security Center für IoT integriert ist, ist ein Sicherheitsmodul erforderlich. Zur Authentifizierung des Geräts kann Azure Security Center für IoT eine von zwei Methoden verwenden. Wählen Sie die Methode aus, die sich am besten für Ihre vorhandene IoT-Lösung eignet. 

> [!div class="checklist"]
> * Option „SecurityModule“
> * Geräteoption

## <a name="authentication-methods"></a>Authentifizierungsmethoden

Für den AzureIoTSecurity-Agent gibt es die beiden folgenden Authentifizierungsmethoden:

 - Authentifizierungsmodus **SecurityModule**<br>
   Der Agent wird unabhängig von der Geräteidentität mithilfe der Sicherheitsmodulidentität authentifiziert.
   Verwenden Sie diesen Authentifizierungstyp, wenn der Sicherheits-Agent eine dedizierte Authentifizierungsmethode über Sicherheitsmodul (nur symmetrischer Schlüssel) verwenden soll.
        
 - Authentifizierungsmodus **Gerät**<br>
    Bei dieser Methode wird der Sicherheits-Agent zunächst mit der Geräteidentität authentifiziert. Nach der ersten Authentifizierung führt der Azure Security Center für IoT-Agent den Aufruf **REST** am IoT Hub durch und verwendet dabei die REST-API mit den Authentifizierungsdaten des Geräts. Der Azure Security Center für IoT-Agent fordert dann die Authentifizierungsmethode und -daten des Sicherheitsmoduls vom IoT Hub an. Im letzten Schritt führt der Azure Security Center für IoT-Agent eine Authentifizierung für das Azure Security Center für IoT-Modul aus.
    
    Verwenden Sie diesen Authentifizierungstyp, wenn der Sicherheits-Agent eine vorhandene Geräteauthentifizierungsmethode (selbstsigniertes Zertifikat oder symmetrischer Schlüssel) erneut verwenden soll. 

Informationen zur Konfiguration finden Sie unter [Security agent installation parameters (Installationsparameter für den Sicherheits-Agent)](#security-agent-installation-parameters).
                                
## <a name="authentication-methods-known-limitations"></a>Bekannte Einschränkungen von Authentifizierungsmethoden

- Der Authentifizierungsmodus **SecurityModule** unterstützt nur die Authentifizierung mit symmetrischen Schlüsseln.
- Der Authentifizierungsmodus **Gerät** unterstützt kein von der Zertifizierungsstelle signiertes Zertifikat.  

## <a name="security-agent-installation-parameters"></a>Installationsparameter für den Sicherheits-Agent

Beim [Bereitstellen eines Sicherheits-Agents](how-to-deploy-agent.md) müssen Authentifizierungsdetails als Argumente angegeben werden.
Diese Argumente werden in der folgenden Tabelle gezeigt.


|Linux-Parametername | Windows-Parametername | Kurzform für Parameter |BESCHREIBUNG|Optionen|
|---------------------|---------------|---------|---------------|---------------|
|authentication-identity|AuthenticationIdentity|aui|Authentifizierungsidentität| **SecurityModule** oder **Device**|
|authentication-method|AuthenticationMethod|aum|Authentifizierungsmethode|**SymmetricKey** oder **SelfSignedCertificate**|
|file-path|FilePath|f|Vollständiger Pfad der Datei, die das Zertifikat oder den symmetrischen Schlüssel enthält| |
|host-name|HostName|hn|Vollqualifizierter Domänenname (FQDN) des IoT Hubs|Beispiel: ContosoIotHub.azure-devices.net|
|device-id|deviceId|di|Geräte-ID|Beispiel: MyDevice1|
|certificate-location-kind|CertificateLocationKind|cl|Speicherort des Zertifikats|**LocalFile** oder **Store**|
|


Wenn Sie das Installationsskript des Sicherheits-Agents verwenden, wird die folgende Konfiguration automatisch ausgeführt. Um die Authentifizierung des Sicherheits-Agents manuell zu bearbeiten, bearbeiten Sie die Konfigurationsdatei. 

## <a name="change-authentication-method-after-deployment"></a>Ändern der Authentifizierungsmethode nach der Bereitstellung

Wenn Sie einen Sicherheits-Agent mit einem Installationsskript bereitstellen, wird automatisch eine Konfigurationsdatei erstellt.

Um Authentifizierungsmethoden nach der Bereitstellung zu ändern, muss die Konfigurationsdatei manuell bearbeitet werden.


### <a name="c-based-security-agent"></a>C#-basierter Sicherheits-Agent

Bearbeiten Sie _Authentication.config_ mit den folgenden Parametern:

```xml
<Authentication>
  <add key="deviceId" value=""/>
  <add key="gatewayHostname" value=""/>
  <add key="filePath" value=""/>
  <add key="type" value=""/>
  <add key="identity" value=""/>
  <add key="certificateLocationKind" value="" />
</Authentication>
```

### <a name="c-based-security-agent"></a>C-basierter Sicherheits-Agent

Bearbeiten Sie _LocalConfiguration.json_ mit den folgenden Parametern:

```json
"Authentication" : {
    "Identity" : "",
    "AuthenticationMethod" : "",
    "FilePath" : "",
    "DeviceId" : "",
    "HostName" : ""
}
```

## <a name="see-also"></a>Weitere Informationen
- [Security agents overview (Sicherheits-Agents (Übersicht))](security-agent-architecture.md)
- [Deploy security agent (Bereitstellen eines Sicherheits-Agents)](how-to-deploy-agent.md)
- [Access raw security data (Zugreifen auf Sicherheitsrohdaten)](how-to-security-data-access.md)

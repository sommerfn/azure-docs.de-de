---
title: Azure Service Fabric-Reverseproxy – sichere Kommunikation | Microsoft-Dokumentation
description: Konfigurieren eines Reverseproxys für eine sichere End-to-End-Kommunikation.
services: service-fabric
documentationcenter: .net
author: kavyako
manager: vipulm
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 08/10/2017
ms.author: kavyako
ms.openlocfilehash: e915e689f09ba7f5c92958ebf8531aa67eef4493
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2019
ms.locfileid: "72933953"
---
# <a name="connect-to-a-secure-service-with-the-reverse-proxy"></a>Herstellen einer Verbindung mit einem sicheren Dienst mit dem Reverseproxy

In diesem Artikel wird erläutert, wie eine sichere Verbindung zwischen dem Reverseproxy und Diensten hergestellt und so ein sicherer End-to-End-Kanal ermöglicht wird. Weitere Informationen zum Reverseproxy finden Sie unter [Reverseproxy in Azure Service Fabric](service-fabric-reverseproxy.md).

Die Herstellung einer Verbindung mit sicheren Diensten wird nur unterstützt, wenn der Reverseproxy für das Lauschen von HTTPS konfiguriert ist. In diesem Artikel wird davon ausgegangen, dass dies der Fall ist.
Informationen zum Konfigurieren des Reverseproxys in Service Fabric finden Sie unter [Setup reverse proxy in Azure Service Fabric](service-fabric-reverseproxy-setup.md) (Reverseproxy-Setup in Azure Service Fabric).

## <a name="secure-connection-establishment-between-the-reverse-proxy-and-services"></a>Herstellung einer sicheren Verbindung zwischen dem Reverseproxy und Diensten 

### <a name="reverse-proxy-authenticating-to-services"></a>Reverseproxy authentifiziert sich bei Diensten:
Der Reverseproxy identifiziert sich bei Diensten selbst über sein Zertifikat. Für Azure-Cluster wird das Zertifikat mit der ***reverseProxyCertificate***-Eigenschaft im [Ressourcentypenabschnitt](../azure-resource-manager/resource-group-authoring-templates.md) [**Microsoft.ServiceFabric/clusters**](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/clusters) der Resource Manager-Vorlage angegeben. Für eigenständige Cluster wird der Cluster entweder mit der ***ReverseProxyCertificate***- oder der ***ReverseProxyCertificateCommonNames***-Eigenschaft im Abschnitt **Security** der Datei „ClusterConfig.json“ angegeben. Weitere Informationen finden Sie unter [Aktivieren des Reverseproxys auf eigenständigen Clustern](service-fabric-reverseproxy-setup.md#enable-reverse-proxy-on-standalone-clusters). 

Die Dienste können die Logik implementieren, um das vom Reverseproxy bereitgestellte Zertifikat zu überprüfen. Die Dienste können die Details des akzeptierten Clientzertifikats als Konfigurationseinstellungen im Konfigurationspaket angeben. Dieses kann zur Laufzeit gelesen und zum Überprüfen des vom Reverseproxy bereitgestellten Zertifikats verwendet werden. Informationen zum Hinzufügen der Konfigurationseinstellungen finden Sie unter [Verwalten von Anwendungsparametern](service-fabric-manage-multiple-environment-app-configuration.md). 

### <a name="reverse-proxy-verifying-the-services-identity-via-the-certificate-presented-by-the-service"></a>Reverseproxy überprüft die Identität des Diensts über das vom Dienst bereitgestellte Zertifikat:
Der Reverseproxy unterstützt für die Ausführung der Serverzertifikatüberprüfung der von den Diensten bereitgestellten Zertifikate die folgenden Richtlinien: „Keine“ (None), „ServiceCommonNameAndIssuer“ und „ServiceCertificateThumbprints“.
Um die vom Reverseproxy zu verwendende Richtlinie auszuwählen, geben Sie **ApplicationCertificateValidationPolicy** im Abschnitt **ApplicationGateway/Http** unter [fabricSettings](service-fabric-cluster-fabric-settings.md) an.

Der nächste Abschnitt enthält die Konfigurationsdetails für die einzelnen Optionen.

### <a name="service-certificate-validation-options"></a>Optionen für die Dienstzertifikatüberprüfung 

- **Keine:** Der Reverseproxy überspringt die Überprüfung des an den Proxy übermittelten Dienstzertifikats und stellt die sichere Verbindung her. Dies ist das Standardverhalten.
Geben Sie **ApplicationCertificateValidationPolicy** mit dem Wert **None** im Abschnitt [**ApplicationGateway/Http**](./service-fabric-cluster-fabric-settings.md#applicationgatewayhttp) ein.

   ```json
   {
   "fabricSettings": [
             ...
             {
               "name": "ApplicationGateway/Http",
               "parameters": [
                 {
                   "name": "ApplicationCertificateValidationPolicy",
                   "value": "None"
                 }
               ]
             }
           ],
           ...
   }
   ```

- **ServiceCommonNameAndIssuer**: Der Reverseproxy überprüft das vom Dienst bereitgestellte Zertifikat anhand des allgemeinen Namens des Zertifikats und des Fingerabdrucks des unmittelbaren Ausstellers: Geben Sie **ApplicationCertificateValidationPolicy** mit dem Wert **ServiceCommonNameAndIssuer** im Abschnitt [**ApplicationGateway/Http**](./service-fabric-cluster-fabric-settings.md#applicationgatewayhttp) an.

   ```json
   {
   "fabricSettings": [
             ...
             {
               "name": "ApplicationGateway/Http",
               "parameters": [
                 {
                   "name": "ApplicationCertificateValidationPolicy",
                   "value": "ServiceCommonNameAndIssuer"
                 }
               ]
             }
           ],
           ...
   }
   ```

   Um die Liste der allgemeinen Namen des Diensts und der Fingerabdrücke des Ausstellers anzugeben, fügen Sie wie weiter unten dargestellt einen [**ApplicationGateway/Http/ServiceCommonNameAndIssuer**](./service-fabric-cluster-fabric-settings.md#applicationgatewayhttpservicecommonnameandissuer)-Abschnitt unter **fabricSettings** ein. Im Array **parameters** können mehrere Paare von allgemeinen Namen des Zertifikats und Fingerabdrücken des Ausstellers hinzugefügt werden. 

   Wenn der Endpunkt, mit dem der Reverseproxy eine Verbindung herstellt, ein Zertifikat bereitstellt, dessen allgemeiner Name und Fingerabdruck des Ausstellers mit einem der hier angegebenen Werte übereinstimmt, wird der SSL-Kanal eingerichtet. 
   Bei Nichtübereinstimmung der Zertifikatdetails gibt der Reverseproxy für die Anforderung des Clients eine Fehlermeldung mit dem Statuscode 502 (Ungültiges Gateway) aus. Zudem enthält die HTTP-Statuszeile den Ausdruck „Invalid SSL Certificate“. 

   ```json
   {
   "fabricSettings": [
             ...
             {
               "name": "ApplicationGateway/Http/ServiceCommonNameAndIssuer",
               "parameters": [
                 {
                   "name": "WinFabric-Test-Certificate-CN1",
                   "value": "b3 44 9b 01 8d 0f 68 39 a2 c5 d6 2b 5b 6c 6a c8 22 b4 22 11"
                 },
                 {
                   "name": "WinFabric-Test-Certificate-CN2",
                   "value": "b3 44 9b 01 8d 0f 68 39 a2 c5 d6 2b 5b 6c 6a c8 22 11 33 44"
                 }
               ]
             }
           ],
           ...
   }
   ```

- **ServiceCertificateThumbprints**: Der Reverseproxy überprüft das an den Proxy übermittelte Dienstzertifikat anhand des zugehörigen Fingerabdrucks. Diese Option können Sie auswählen, wenn die Dienste mit selbstsignierten Zertifikaten konfiguriert sind: Geben Sie **ApplicationCertificateValidationPolicy** mit dem Wert **ServiceCertificateThumbprints** im Abschnitt [**ApplicationGateway/Http**](./service-fabric-cluster-fabric-settings.md#applicationgatewayhttp) an.

   ```json
   {
   "fabricSettings": [
             ...
             {
               "name": "ApplicationGateway/Http",
               "parameters": [
                 {
                   "name": "ApplicationCertificateValidationPolicy",
                   "value": "ServiceCertificateThumbprints"
                 }
               ]
             }
           ],
           ...
   }
   ```

   Geben Sie zudem die Fingerabdrücke mit einem **ServiceCertificateThumbprints**-Eintrag im Abschnitt **ApplicationGateway/Http** an. Mehrere Fingerabdrücke können, wie im Folgenden dargestellt, als eine durch Trennzeichen getrennte Liste im Wertfeld angegeben werden:

   ```json
   {
   "fabricSettings": [
             ...
             {
               "name": "ApplicationGateway/Http",
               "parameters": [
                   ...
                 {
                   "name": "ServiceCertificateThumbprints",
                   "value": "78 12 20 5a 39 d2 23 76 da a0 37 f0 5a ed e3 60 1a 7e 64 bf,78 12 20 5a 39 d2 23 76 da a0 37 f0 5a ed e3 60 1a 7e 64 b9"
                 }
               ]
             }
           ],
           ...
   }
   ```

   Wenn der Fingerabdruck des Serverzertifikats in diesem Konfigurationseintrag aufgeführt ist, stellt der Reverseproxy die SSL-Verbindung her. Andernfalls beendet er die Verbindung und gibt für die Anforderung des Clients eine Fehlermeldung mit dem Statuscode 502 (Ungültiges Gateway) aus. Zudem enthält die HTTP-Statuszeile den Ausdruck „Invalid SSL Certificate“.

## <a name="endpoint-selection-logic-when-services-expose-secure-as-well-as-unsecured-endpoints"></a>Logik der Endpunktauswahl, wenn Dienste sichere und nicht sichere Endpunkte bereitstellen
Service Fabric unterstützt die Konfiguration mehrerer Endpunkte für einen Dienst. Weitere Informationen finden Sie unter [Angeben von Ressourcen in einem Dienstmanifest](service-fabric-service-manifest-resources.md).

Der Reverseproxy wählt einen der Endpunkte zum Weiterleiten der Anforderung basierend auf dem Abfrageparameter **ListenerName** im [Dienst-URI](./service-fabric-reverseproxy.md#uri-format-for-addressing-services-by-using-the-reverse-proxy) aus. Wird der Parameter **ListenerName** nicht angegeben, kann der Reverseproxy einen beliebigen Endpunkt in der Liste der Endpunkte auswählen. Abhängig von den für den Dienst konfigurierten Endpunkten kann der ausgewählte Endpunkt ein HTTP- oder ein HTTPS-Endpunkt sein. Unter Umständen gibt es Szenarien oder Anforderungen, bei denen der Reverseproxy im „ausschließlich sicheren Modus“ ausgeführt werden soll. Das bedeutet, dass der sichere Reverseproxy keine Anforderungen an nicht sichere Endpunkte weiterleiten soll. Um für den Reverseproxy den ausschließlich sicheren Modus festzulegen, geben Sie den Konfigurationseintrag **SecureOnlyMode** im Abschnitt [**ApplicationGateway/Http**](./service-fabric-cluster-fabric-settings.md#applicationgatewayhttp) mit dem Wert **true** an.   

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
                ...
              {
                "name": "SecureOnlyMode",
                "value": true
              }
            ]
          }
        ],
        ...
}
```

> [!NOTE]
> Wenn im **SecureOnlyMode** der Client ein **ListenerName**-Element angegeben hat, das einem HTTP-Endpunkt (ungesichert) entspricht, gibt der Reverseproxy für die Anforderung eine Fehlermeldung mit dem HTTP-Statuscode 404 (Nicht gefunden) aus.

## <a name="setting-up-client-certificate-authentication-through-the-reverse-proxy"></a>Einrichten der Clientzertifikatauthentifizierung über den Reverseproxy
Die SSL-Terminierung erfolgt auf dem Reverseproxy, und alle Daten des Clientzertifikats gehen verloren. Damit die Dienste die Clientzertifikatauthentifizierung durchführen, legen Sie die Einstellung **ForwardClientCertificate** im Abschnitt [**ApplicationGateway/Http**](./service-fabric-cluster-fabric-settings.md#applicationgatewayhttp) fest.

1. Wenn **ForwardClientCertificate** auf **false** festgelegt ist, fordert der Reverseproxy das Clientzertifikat während des SSL-Handshakes mit dem Client nicht an.
Dies ist das Standardverhalten.

2. Wenn **ForwardClientCertificate** auf **true** festgelegt ist, fordert der Reverseproxy das Clientzertifikat während des SSL-Handshakes mit dem Client an.
Dann leitet er die Daten des Clientzertifikats in einem benutzerdefinierten HTTP-Header mit dem Namen **X-Client-Certificate** weiter. Der Headerwert ist die Base64-codierte Zeichenfolge im PEM-Format des Clientzertifikats. Der Dienst kann die Anforderung nach dem Überprüfen der Zertifikatdaten mit dem entsprechenden Statuscode annehmen oder ablehnen.
Wenn der Client kein Zertifikat bereitstellt, leitet der Reverseproxy einen leeren Header weiter. Die weitere Verarbeitung erfolgt durch den Dienst.

> [!NOTE]
> Der Reverseproxy führt lediglich die Weiterleitung durch. Er führt keine Überprüfung des Clientzertifikats durch.


## <a name="next-steps"></a>Nächste Schritte
* [Einrichten und Konfigurieren eines Reverseproxys in einem Cluster](service-fabric-reverseproxy-setup.md)
* Weitere Informationen finden Sie unter [Konfigurieren des Reverseproxys für die Verbindung mit sicheren Diensten](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/Reverse-Proxy-Sample#configure-reverse-proxy-to-connect-to-secure-services).
* Ein Beispiel für die HTTP-Kommunikation zwischen Diensten finden Sie im [Beispielprojekt auf GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).
* [Remoteprozeduraufrufe mit Reliable Services-Remoting](service-fabric-reliable-services-communication-remoting.md)
* [Web-API, die OWIN in Reliable Services verwendet](service-fabric-reliable-services-communication-webapi.md)
* [Verwalten von Clusterzertifikaten](service-fabric-cluster-security-update-certs-azure.md)

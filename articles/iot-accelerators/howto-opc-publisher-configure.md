---
title: Konfigurieren von OPC Publisher – Azure | Microsoft-Dokumentation
description: In diesem Artikel wird das Konfigurieren von OPC Publisher zum Angeben von Datenänderungen des OPC UA-Knotens, OPC UA-Ereignissen für die Veröffentlichung und des Telemetrieformats beschrieben.
author: dominicbetts
ms.author: dobett
ms.date: 06/10/2019
ms.topic: overview
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 0db00f670dfcc526d3fc34d41ce731df4c6573ec
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73824151"
---
# <a name="configure-opc-publisher"></a>Konfigurieren von OPC Publisher

Sie können OPC Publisher so konfigurieren, dass Folgendes angegeben wird:

- Die zu veröffentlichenden Änderungen der OPC UA-Knotendaten
- Die zu veröffentlichenden OPC UA-Ereignisse
- Das Format der Telemetriedaten

Sie können OPC Publisher mithilfe von Konfigurationsdateien oder über Methodenaufrufe konfigurieren.

## <a name="use-configuration-files"></a>Verwenden von Konfigurationsdateien

In diesem Abschnitt werden die Optionen zum Konfigurieren der Veröffentlichung von OPC UA-Knoten mit Konfigurationsdateien beschrieben.

### <a name="use-a-configuration-file-to-configure-publishing-data-changes"></a>Verwenden einer Konfigurationsdatei zum Konfigurieren der Veröffentlichung von Datenänderungen

Die zu veröffentlichenden OPC UA-Knoten können am einfachsten mit einer Konfigurationsdatei konfiguriert werden. Das Konfigurationsdateiformat ist in der Datei [publishednodes.json](https://github.com/Azure/iot-edge-opc-publisher/blob/master/opcpublisher/publishednodes.json) im Repository angegeben.

Die Syntax der Konfigurationsdateien wurde im Laufe der Zeit geändert. OPC Publisher liest weiterhin alte Formate, konvertiert sie jedoch in das aktuelle Format, wenn die Konfiguration dauerhaft gespeichert wird.

Im folgenden Beispiel sehen Sie das Format der Konfigurationsdatei:

```json
[
  {
    "EndpointUrl": "opc.tcp://testserver:62541/Quickstarts/ReferenceServer",
    "UseSecurity": true,
    "OpcNodes": [
      {
        "Id": "i=2258",
        "OpcSamplingInterval": 2000,
        "OpcPublishingInterval": 5000,
        "DisplayName": "Current time"
      }
    ]
  }
]
```

### <a name="use-a-configuration-file-to-configure-publishing-events"></a>Verwenden einer Konfigurationsdatei zum Konfigurieren der Veröffentlichung von Ereignissen

Zum Veröffentlichen von OPC UA-Ereignissen wird die gleiche Konfigurationsdatei wie für Datenänderungen verwendet.

Das folgende Beispiel zeigt, wie Sie die Veröffentlichung für Ereignisse konfigurieren, die über den [SimpleEvents-Server](https://github.com/OPCFoundation/UA-.NETStandard/tree/master/SampleApplications/Workshop/SimpleEvents/Server) generiert werden. Den SimpleEvents-Server finden Sie im [OPC Foundation-Repository](https://github.com/OPCFoundation/UA-.NETStandard).

```json
[
  {
    "EndpointUrl": "opc.tcp://testserver:62563/Quickstarts/SimpleEventsServer",
    "OpcEvents": [
      {
        "Id": "i=2253",
        "DisplayName": "SimpleEventServerEvents",
        "SelectClauses": [
          {
            "TypeId": "i=2041",
            "BrowsePaths": [
              "EventId"
            ]
          },
          {
            "TypeId": "i=2041",
            "BrowsePaths": [
              "Message"
            ]
          },
          {
            "TypeId": "nsu=http://opcfoundation.org/Quickstarts/SimpleEvents;i=235",
            "BrowsePaths": [
              "/2:CycleId"
            ]
          },
          {
            "TypeId": "nsu=http://opcfoundation.org/Quickstarts/SimpleEvents;i=235",
            "BrowsePaths": [
              "/2:CurrentStep"
            ]
          }
        ],
        "WhereClause": [
          {
            "Operator": "OfType",
            "Operands": [
              {
                "Literal": "ns=2;i=235"
              }
            ]
          }
        ]
      }
    ]
  }
]
```

## <a name="use-method-calls"></a>Verwenden von Methodenaufrufen

In diesem Abschnitt werden die Methodenaufrufe beschrieben, die Sie zum Konfigurieren von OPC Publisher verwenden können.

### <a name="configure-using-opc-ua-method-calls"></a>Konfigurieren mithilfe von OPC UA-Methodenaufrufen

OPC Publisher umfasst einen OPC UA-Server, auf den über Port 62222 zugegriffen werden kann. Wenn als Hostname **publisher** festgelegt ist, lautet der Endpunkt-URI `opc.tcp://publisher:62222/UA/Publisher`.

Dieser Endpunkt macht die folgenden vier Methoden verfügbar:

- PublishNode
- UnpublishNode
- GetPublishedNodes
- IoT HubDirectMethod

### <a name="configure-using-iot-hub-direct-method-calls"></a>Konfigurieren mithilfe von direkten IoT Hub-Methodenaufrufen

OPC Publisher implementiert die folgenden direkten IoT Hub-Methodenaufrufe:

- PublishNodes
- UnpublishNodes
- UnpublishAllNodes
- GetConfiguredEndpoints
- GetConfiguredNodesOnEndpoint
- GetDiagnosticInfo
- GetDiagnosticLog
- GetDiagnosticStartupLog
- ExitApplication
- GetInfo

Das Format der JSON-Nutzlast für die Anforderung und die Antworten der Methode sind in [opcpublisher/HubMethodModel.cs](https://github.com/Azure/iot-edge-opc-publisher/blob/master/opcpublisher/HubMethodModel.cs) definiert.

Wenn Sie eine unbekannte Methode für das Modul aufrufen, wird als Antwort eine Zeichenfolge zurückgegeben, die angibt, dass die Methode nicht implementiert ist. Sie können eine unbekannte Methode als eine Möglichkeit aufrufen, das Modul zu pingen.

### <a name="configure-username-and-password-for-authentication"></a>Konfigurieren des Benutzernamens und Kennworts für die Authentifizierung

Der Authentifizierungsmodus kann über einen direkten IoT Hub-Methodenaufruf festgelegt werden. Die Nutzlast muss die **OpcAuthenticationMode**-Eigenschaft sowie den Benutzernamen und das Kennwort enthalten:

```csharp
{
    "EndpointUrl": "<Url of the endpoint to set authentication settings>",
    "OpcAuthenticationMode": "UsernamePassword",
    "Username": "<Username>",
    "Password": "<Password>"
    ...
}
```

Das Kennwort wird vom IoT Hub-Workloadclient verschlüsselt und in der Konfiguration von Publisher gespeichert. Um wieder zur anonymen Authentifizierung zu wechseln, verwenden Sie die Methode mit der folgenden Nutzlast:

```csharp
{
    "EndpointUrl": "<Url of the endpoint to set authentication settings>",
    "OpcAuthenticationMode": "Anonymous"
    ...
}
```

Wenn die **OpcAuthenticationMode**-Eigenschaft in der Nutzlast nicht festgelegt ist, bleiben die Authentifizierungseinstellungen in der Konfiguration unverändert.

## <a name="configure-telemetry-publishing"></a>Konfigurieren der Veröffentlichung von Telemetriedaten

Wenn OPC Publisher eine Benachrichtigung über die Änderung von Werten in einem veröffentlichten Knoten empfängt, wird eine Nachricht im JSON-Format generiert und an IoT Hub gesendet.

Sie können den Inhalt dieser JSON-formatierten Nachricht mithilfe einer Konfigurationsdatei konfigurieren. Wenn keine Konfigurationsdatei mit der Option `--tc` angegeben wurde, wird eine Standardkonfiguration verwendet, die mit dem [Solution Accelerator für verbundene Factorys](https://github.com/Azure/azure-iot-connected-factory) kompatibel ist.

Wenn OPC Publisher für Batchnachrichten konfiguriert ist, werden diese als gültiges JSON-Array gesendet.

Die Telemetriedaten werden von den folgenden Quellen abgeleitet:

- OPC Publisher-Knotenkonfiguration für den Knoten
- **MonitoredItem**-Objekt des OPC UA-Stapels, für den OPC Publisher eine Benachrichtigung erhalten hat
- Das an diese Benachrichtigung übergebene Argument, das Details zur Änderung der Datenwerte enthält

Die Telemetriedaten, die in der JSON-formatierten Nachricht enthalten sind, stellen eine Auswahl aus wichtigen Eigenschaften dieser Objekte dar. Wenn Sie weitere Eigenschaften benötigen, müssen Sie die OPC Publisher-Codebasis ändern.

Die Syntax der Konfigurationsdatei sieht wie folgt aus:

```json
// The configuration settings file consists of two objects:
// 1) The 'Defaults' object, which defines defaults for the telemetry configuration
// 2) An array 'EndpointSpecific' of endpoint specific configuration
// Both objects are optional and if they are not specified, then publisher uses
// its internal default configuration, which generates telemetry messages compatible
// with the Microsoft Connected factory Preconfigured Solution (https://github.com/Azure/azure-iot-connected-factory).

// A JSON telemetry message for Connected factory looks like:
//  {
//      "NodeId": "i=2058",
//      "ApplicationUri": "urn:myopcserver",
//      "DisplayName": "CurrentTime",
//      "Value": {
//          "Value": "10.11.2017 14:03:17",
//          "SourceTimestamp": "2017-11-10T14:03:17Z"
//      }
//  }

// The 'Defaults' object in the sample below, are similar to what publisher is
// using as its internal default telemetry configuration.
{
    "Defaults": {
        // The first two properties ('EndpointUrl' and 'NodeId' are configuring data
        // taken from the OpcPublisher node configuration.
        "EndpointUrl": {

            // The following three properties can be used to configure the 'EndpointUrl'
            // property in the JSON message send by publisher to IoT Hub.

            // Publish controls if the property should be part of the JSON message at all.
            "Publish": false,

            // Pattern is a regular expression, which is applied to the actual value of the
            // property (here 'EndpointUrl').
            // If this key is ommited (which is the default), then no regex matching is done
            // at all, which improves performance.
            // If the key is used you need to define groups in the regular expression.
            // Publisher applies the regular expression and then concatenates all groups
            // found and use the resulting string as the value in the JSON message to
            //sent to IoT Hub.
            // This example mimics the default behaviour and defines a group,
            // which matches the conplete value:
            "Pattern": "(.*)",
            // Here some more exaples for 'Pattern' values and the generated result:
            // "Pattern": "i=(.*)"
            // defined for Defaults.NodeId.Pattern, will generate for the above sample
            // a 'NodeId' value of '2058'to be sent by publisher
            // "Pattern": "(i)=(.*)"
            // defined for Defaults.NodeId.Pattern, will generate for the above sample
            // a 'NodeId' value of 'i2058' to be sent by publisher

            // Name allows you to use a shorter string as property name in the JSON message
            // sent by publisher. By default the property name is unchanged and will be
            // here 'EndpointUrl'.
            // The 'Name' property can only be set in the 'Defaults' object to ensure
            // all messages from publisher sent to IoT Hub have a similar layout.
            "Name": "EndpointUrl"

        },
        "NodeId": {
            "Publish": true,

            // If you set Defaults.NodeId.Name to "ni", then the "NodeId" key/value pair
            // (from the above example) will change to:
            //      "ni": "i=2058",
            "Name": "NodeId"
        },

        // The MonitoredItem object is configuring the data taken from the MonitoredItem
        // OPC UA object for published nodes.
        "MonitoredItem": {

            // If you set the Defaults.MonitoredItem.Flat to 'false', then a
            // 'MonitoredItem' object will appear, which contains 'ApplicationUri'
            // and 'DisplayNode' proerties:
            //      "NodeId": "i=2058",
            //      "MonitoredItem": {
            //          "ApplicationUri": "urn:myopcserver",
            //          "DisplayName": "CurrentTime",
            //      }
            // The 'Flat' property can only be used in the 'MonitoredItem' and
            // 'Value' objects of the 'Defaults' object and will be used
            // for all JSON messages sent by publisher.
            "Flat": true,

            "ApplicationUri": {
                "Publish": true,
                "Name": "ApplicationUri"
            },
            "DisplayName": {
                "Publish": true,
                "Name": "DisplayName"
            }
        },
        // The Value object is configuring the properties taken from the event object
        // the OPC UA stack provided in the value change notification event.
        "Value": {
            // If you set the Defaults.Value.Flat to 'true', then the 'Value'
            // object will disappear completely and the 'Value' and 'SourceTimestamp'
            // members won't be nested:
            //      "DisplayName": "CurrentTime",
            //      "Value": "10.11.2017 14:03:17",
            //      "SourceTimestamp": "2017-11-10T14:03:17Z"
            // The 'Flat' property can only be used for the 'MonitoredItem' and 'Value'
            // objects of the 'Defaults' object and will be used for all
            // messages sent by publisher.
            "Flat": false,

            "Value": {
                "Publish": true,
                "Name": "Value"
            },
            "SourceTimestamp": {
                "Publish": true,
                "Name": "SourceTimestamp"
            },
            // 'StatusCode' is the 32 bit OPC UA status code
            "StatusCode": {
                "Publish": false,
                "Name": "StatusCode"
                // 'Pattern' is ignored for the 'StatusCode' value
            },
            // 'Status' is the symbolic name of 'StatusCode'
            "Status": {
                "Publish": false,
                "Name": "Status"
            }
        }
    },

    // The next object allows to configure 'Publish' and 'Pattern' for specific
    // endpoint URLs. Those will overwrite the ones specified in the 'Defaults' object
    // or the defaults used by publisher.
    // It is not allowed to specify 'Name' and 'Flat' properties in this object.
    "EndpointSpecific": [
        // The following shows how a endpoint specific configuration can look like:
        {
            // 'ForEndpointUrl' allows to configure for which OPC UA server this
            // object applies and is a required property for all objects in the
            // 'EndpointSpecific' array.
            // The value of 'ForEndpointUrl' must be an 'EndpointUrl' configured in
            // the publishednodes.json confguration file.
            "ForEndpointUrl": "opc.tcp://<your_opcua_server>:<your_opcua_server_port>/<your_opcua_server_path>",
            "EndpointUrl": {
                // We overwrite the default behaviour and publish the
                // endpoint URL in this case.
                "Publish": true,
                // We are only interested in the URL part following the 'opc.tcp://' prefix
                // and define a group matching this.
                "Pattern": "opc.tcp://(.*)"
            },
            "NodeId": {
                // We are not interested in the configured 'NodeId' value, 
                // so we do not publish it.
                "Publish": false
                // No 'Pattern' key is specified here, so the 'NodeId' value will be
                // taken as specified in the publishednodes configuration file.
            },
            "MonitoredItem": {
                "ApplicationUri": {
                    // We already publish the endpoint URL, so we do not want
                    // the ApplicationUri of the MonitoredItem to be published.
                    "Publish": false
                },
                "DisplayName": {
                    "Publish": true
                }
            },
            "Value": {
                "Value": {
                    // The value of the node is important for us, everything else we
                    // are not interested in to keep the data ingest as small as possible.
                    "Publish": true
                },
                "SourceTimestamp": {
                    "Publish": false
                },
                "StatusCode": {
                    "Publish": false
                },
                "Status": {
                    "Publish": false
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun wissen, wie OPC Publisher konfiguriert wird, empfiehlt es sich im nächsten Schritt mit [Ausführen von OPC Publisher](howto-opc-publisher-run.md) fortzufahren.

---
title: WebSocket-Unterstützung in Azure Application Gateway
description: WebSocket wird von Application Gateway nativ für alle Gatewaygrößen unterstützt. Es gibt keine vom Benutzer konfigurierbaren Einstellungen.
author: vhorne
ms.author: amsriva
ms.service: application-gateway
services: application-gateway
ms.topic: conceptual
ms.date: 11/16/2019
ms.openlocfilehash: baa02c4d946a121f26f421af99835ae2bea18847
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2019
ms.locfileid: "74130334"
---
# <a name="overview-of-websocket-support-in-application-gateway"></a>Übersicht über die WebSocket-Unterstützung in Application Gateway

WebSocket wird von Application Gateway nativ für alle Gatewaygrößen unterstützt. Die WebSocket-Unterstützung kann von Benutzern nicht selektiv aktiviert oder deaktiviert werden. 

Das in [RFC6455](https://tools.ietf.org/html/rfc6455) standardisierte WebSocket-Protokoll ermöglicht die Vollduplexkommunikation zwischen einem Server und einem Client über eine TCP-Verbindung mit langer Laufzeit. Dieses Feature ermöglicht wiederum mehr Interaktivität bei der Kommunikation zwischen dem Webserver und dem Client, da die Kommunikation auch ohne die bei HTTP-basierten Implementierungen erforderlichen Abfragen bidirektional sein kann. Im Vergleich zu HTTP zeichnet sich WebSocket durch einen geringen Mehraufwand aus. Außerdem können sie die gleiche TCP-Verbindung für mehrere Anforderungen/Antworten verwenden, was eine effizientere Ressourcennutzung zur Folge hat. WebSocket-Protokolle sind für die Nutzung der herkömmlichen HTTP-Ports 80 und 443 konzipiert.

WebSocket-Datenverkehr kann mit einem standardmäßigen HTTP-Listener an den Ports 80 und 443 empfangen werden. Der empfangene WebSocket-Datenverkehr wird dann unter Verwendung des entsprechenden Back-End-Pools (gemäß Angabe in Anwendungsgatewayregeln) an den WebSocket-fähigen Back-End-Server weitergeleitet. Der Back-End-Server muss auf die Anwendungsgatewaytests reagieren. Diese werden im Abschnitt [Integritätstest – Übersicht](application-gateway-probe-overview.md) erläutert. Application Gateway-Integritätstests sind auf HTTP/HTTPS beschränkt. Jeder Back-End-Server muss auf HTTP-Tests für das Anwendungsgateway reagieren, damit WebSocket-Datenverkehr an den Server weitergeleitet wird.

WebSocket wird in Anwendungen verwendet, die von schneller Kommunikation in Echtzeit profitieren, wie etwa Chat-, Dashboard- und Spiele-Apps.

## <a name="how-does-websocket-work"></a>Funktionsweise von WebSocket

Um eine WebSocket-Verbindung einzurichten, wird ein spezifischer HTTP-basierter Handshake zwischen dem Client und dem Server ausgetauscht. Im Erfolgsfall wird mithilfe der zuvor hergestellten TCP-Verbindung ein „Upgrade“ des Protokolls der Anwendungsschicht von HTTP auf WebSockets ausgeführt. Sobald dies erfolgt ist, spielt HTTP keinerlei Rolle mehr. Daten können von beiden Endpunkten mithilfe des WebSocket-Protokolls gesendet oder empfangen werden, bis die WebSocket-Verbindung geschlossen wird. 

![websocket](./media/application-gateway-websocket/websocket.png)

### <a name="listener-configuration-element"></a>Listenerkonfigurationselement

Zur Unterstützung von WebSocket-Datenverkehr kann ein vorhandener HTTP-Listener verwendet werden. Es folgt ein Codeausschnitt des „httpListeners“-Elements in einer Beispielvorlagendatei. Zur Unterstützung von WebSocket-Datenverkehr benötigen Sie sowohl den HTTP- als auch den HTTPS-Listener. Analog dazu können Sie über das Portal oder mithilfe von Azure PowerShell ein Anwendungsgateway mit Listenern für die Ports 80 und 443 erstellen, um WebSocket-Datenverkehr zu unterstützen.

```json
"httpListeners": [
        {
            "name": "appGatewayHttpsListener",
            "properties": {
                "FrontendIPConfiguration": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendIPConfigurations/DefaultFrontendPublicIP"
                },
                "FrontendPort": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendPorts/appGatewayFrontendPort443'"
                },
                "Protocol": "Https",
                "SslCertificate": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/sslCertificates/appGatewaySslCert1'"
                },
            }
        },
        {
            "name": "appGatewayHttpListener",
            "properties": {
                "FrontendIPConfiguration": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendIPConfigurations/appGatewayFrontendIP'"
                },
                "FrontendPort": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendPorts/appGatewayFrontendPort80'"
                },
                "Protocol": "Http",
            }
        }
    ],
```

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a>Konfigurieren von „backendAddressPool“, „backendHttpSettings“ und Routingregel

„backendAddressPool“ dient zum Definieren eines Back-End-Pools mit WebSocket-fähigen Servern. „backendHttpSettings“ wird mit den Back-End-Ports 80 und 443 definiert. Der Wert für das Anforderungstimeout in den HTTP-Einstellungen gilt auch für die WebSocket-Sitzung. Für die Routingregel ist keine Änderung erforderlich. Sie wird verwendet, um den geeigneten Listener mit dem entsprechenden Back-End-Adresspool zu verknüpfen. 

```json
"requestRoutingRules": [{
    "name": "<ruleName1>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/httpListeners/appGatewayHttpsListener')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/ContosoServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }
    }

}, {
    "name": "<ruleName2>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/httpListeners/appGatewayHttpListener')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/ContosoServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }

    }
}]
```

## <a name="websocket-enabled-backend"></a>WebSocket-fähiges Back-End

WebSocket kann nur verwendet werden, wenn das Back-End über einen HTTP-/HTTPS-Webserver verfügt, der an dem konfigurierten Port (üblicherweise 80/443) ausgeführt wird. Dies ist erforderlich, da der anfängliche Handshake des WebSocket-Protokolls HTTP-basiert sein und über ein Headerfeld verfügen muss, das ein Upgrade auf das WebSocket-Protokoll angibt. Es folgt ein Beispiel eines Headers:

```
    GET /chat HTTP/1.1
    Host: server.example.com
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
    Origin: https://example.com
    Sec-WebSocket-Protocol: chat, superchat
    Sec-WebSocket-Version: 13
```

Ein weiterer Grund: Der Back-End-Integritätstest des Anwendungsgateways unterstützt nur das HTTP-/HTTPS-Protokoll. Wenn der Back-End-Server nicht auf HTTP- oder HTTPS-Tests reagiert, wird er aus dem Back-End-Pool entfernt.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich im Anschluss an die WebSocket-Unterstützung unter [Erstellen eines Anwendungsgateways](quick-create-powershell.md) über die ersten Schritte mit einer WebSocket-fähigen Webanwendung.
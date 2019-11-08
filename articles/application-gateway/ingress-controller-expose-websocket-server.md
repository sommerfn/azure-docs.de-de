---
title: Verfügbarmachen eines WebSocket-Servers für Application Gateway
description: Dieser Artikel enthält Informationen darüber, wie Sie einen WebSocket-Server für Application Gateway mit dem Eingangscontroller für AKS-Cluster bereitstellen.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 01fde82e69917f59f6519524c4c8828feb84a4f9
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73795967"
---
# <a name="expose-a-websocket-server-to-application-gateway"></a>Verfügbarmachen eines WebSocket-Servers für Application Gateway

Wie in der Dokumentation zu Application Gateway v2 beschrieben, ist [native Unterstützung für die WebSocket- und HTTP/2-Protokolle](https://docs.microsoft.com/azure/application-gateway/overview#websocket-and-http2-traffic) verfügbar. Beachten Sie, dass sowohl für Application Gateway als auch für die Kubernetes-Eingangsressource keine vom Benutzer konfigurierbare Einstellung vorhanden ist, um die WebSocket-Unterstützung selektiv zu aktivieren oder zu deaktivieren.

Die unten abgebildete YAML-Bereitstellungsdatei von Kubernetes zeigt die minimale Konfiguration, die zum Bereitstellen eines WebSocket-Servers verwendet wird. Dies entspricht der Bereitstellung eines regulären Webservers:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: websocket-server
spec:
  selector:
    matchLabels:
      app: ws-app
  replicas: 2
  template:
    metadata:
      labels:
        app: ws-app
    spec:
      containers:
        - name: websocket-app
          imagePullPolicy: Always
          image: your-container-repo.azurecr.io/websockets-app
          ports:
            - containerPort: 8888
      imagePullSecrets:
        - name: azure-container-registry-credentials

---

apiVersion: v1
kind: Service
metadata:
  name: websocket-app-service
spec:
  selector:
    app: ws-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8888

---

apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: websocket-repeater
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
spec:
  rules:
    - host: ws.contoso.com
      http:
        paths:
          - backend:
              serviceName: websocket-app-service
              servicePort: 80
```

Wenn alle Voraussetzungen erfüllt sind und Sie über ein Application Gateway verfügen, das von einer Kubernetes-Erfassungsressource in Ihrem AKS gesteuert wird, würde die oben angeführte Bereitstellung zu einem an Port 80 der öffentlichen IP-Adresse Ihres Application Gateways und der `ws.contoso.com`-Domäne verfügbaren WebSockets-Server führen.

Der folgende cURL-Befehl testet die WebSocket-Serverbereitstellung:
```sh
curl -i -N -H "Connection: Upgrade" \
        -H "Upgrade: websocket" \
        -H "Origin: http://localhost" \
        -H "Host: ws.contoso.com" \
        -H "Sec-Websocket-Version: 13" \
        -H "Sec-WebSocket-Key: 123" \
        http://1.2.3.4:80/ws
```

## <a name="websocket-health-probes"></a>WebSocket-Integritätstests

Wenn die Bereitstellung nicht explizit Integritätstests definiert, versucht Application Gateway, einen HTTP GET-Vorgang auf dem WebSocket-Serverendpunkt durchzuführen.
Abhängig von der Serverimplementierung ([hier finden Sie eine interessante Implementierung](https://github.com/gorilla/websocket/blob/master/examples/chat/main.go)) können WebSocket-spezifische Header erforderlich sein (beispielsweise `Sec-Websocket-Version`).
Da Application Gateway keine WebSocket-Header hinzufügt, lautet die Integritätstestantwort von Application Gateway von Ihrem WebSocket-Server wahrscheinlich `400 Bad Request`.
Dies führt dazu, dass Application Gateway Ihre Pods als fehlerhaft markiert, was schließlich zu einem Fehler `502 Bad Gateway` für die Consumer des WebSocket-Servers führt.
Um dies zu vermeiden, müssen Sie dem Server möglicherweise einen HTTP GET-Handler für eine Integritätsprüfung hinzufügen (z.B. `/health` mit der Rückgabe `200 OK`).

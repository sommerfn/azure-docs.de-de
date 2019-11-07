---
title: Aktivieren von cookiebasierter Affinität mit Application Gateway
description: Dieser Artikel enthält Informationen zum Aktivieren von cookiebasierter Affinität mit Application Gateway.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 10/23/2019
ms.author: caya
ms.openlocfilehash: cb1a403888f8dca78d1af7395095aa2bac39935e
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510511"
---
# <a name="enable-cookie-based-affinity-with-an-application-gateway"></a>Aktivieren von cookiebasierter Affinität mit Application Gateway
Wie in der [Azure Application Gateway-Dokumentation](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#http-settings) beschrieben, unterstützt Application Gateway cookiebasierte Affinität, d.h. das Gateway kann nachfolgenden Datenverkehr von einer Benutzersitzung an den gleichen Server zur Verarbeitung weiterleiten.

## <a name="example"></a>Beispiel
```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: guestbook
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/cookie-based-affinity: "true"
spec:
  rules:
  - http:
      paths:
      - backend:
          serviceName: frontend
          servicePort: 80
```

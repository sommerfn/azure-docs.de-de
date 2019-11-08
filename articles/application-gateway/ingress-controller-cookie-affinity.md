---
title: Aktivieren von cookiebasierter Affinität mit Application Gateway
description: Dieser Artikel enthält Informationen zum Aktivieren von cookiebasierter Affinität mit Application Gateway.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: adacd805e736524fd7956c4bbc0ad402980b4cd0
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73795980"
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

---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 09/12/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 1c2525b352c25f470814ce909a8d10ff821d9e32
ms.sourcegitcommit: dd69b3cda2d722b7aecce5b9bd3eb9b7fbf9dc0a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2019
ms.locfileid: "70961619"
---
Generieren des Zertifizierungsstellenzertifikats

  ```
  ipsec pki --gen --outform pem > caKey.pem
  ipsec pki --self --in caKey.pem --dn "CN=VPN CA" --ca --outform pem > caCert.pem
  ```

Drucken Sie das Zertifizierungsstellenzertifikat im Base64-Format. Dies ist das Format, das von Azure unterstützt wird. Sie laden dieses Zertifikat im Rahmen der [Schritte für die P2S-Konfiguration](../articles/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md) in Azure hoch.

  ```
  openssl x509 -in caCert.pem -outform der | base64 -w0 ; echo
  ```

Generieren Sie das Benutzerzertifikat.

  ```
  export PASSWORD="password"
  export USERNAME="client"

  ipsec pki --gen --outform pem > "${USERNAME}Key.pem"
  ipsec pki --pub --in "${USERNAME}Key.pem" | ipsec pki --issue --cacert caCert.pem --cakey caKey.pem --dn "CN=${USERNAME}" --san "${USERNAME}" --flag clientAuth --outform pem > "${USERNAME}Cert.pem"
  ```

Generieren Sie ein p12-Paket mit dem Benutzerzertifikat. Dieses Paket wird in den nächsten Schritten bei der Arbeit mit den Clientkonfigurationsdateien verwendet.

  ```
  openssl pkcs12 -in "${USERNAME}Cert.pem" -inkey "${USERNAME}Key.pem" -certfile caCert.pem -export -out "${USERNAME}.p12" -password "pass:${PASSWORD}"
  ```
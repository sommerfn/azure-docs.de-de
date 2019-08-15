---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 01/16/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 98172c2c487488a72bbfdd3a8205ac7d8668db60
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/15/2019
ms.locfileid: "69035786"
---
Generieren des Zertifizierungsstellenzertifikats

  ```
  ipsec pki --gen --outform pem > caKey.pem
  ipsec pki --self --in caKey.pem --dn "CN=VPN CA" --ca --outform pem > caCert.pem
  ```

Drucken Sie das Zertifizierungsstellenzertifikat im Base64-Format. Dies ist das Format, das von Azure unterstützt wird. Sie werden dies später in Azure als Teil der P2S-Konfiguration hochladen.

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
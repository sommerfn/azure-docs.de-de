---
title: Konfigurieren der Clientauthentifizierung von eingehenden Aufrufen – Azure Event Grid-IoT Edge | Microsoft-Dokumentation
description: Konfigurieren Sie API-Protokolle, die von Event Grid in IoT Edge verfügbar gemacht werden.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/03/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: b5456130e89bf77e2c2ba41880323e38f6b27f4c
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2019
ms.locfileid: "72991839"
---
# <a name="configure-client-authentication-of-incoming-calls"></a>Konfigurieren der Clientauthentifizierung von eingehenden Aufrufen

Dieser Leitfaden enthält Beispiele zu möglichen Konfigurationen der Clientauthentifizierung für das Event Grid-Modul. Das Event Grid-Modul unterstützt zwei Typen der Clientauthentifizierung:

* SAS-Schlüssel-basiert (Shared Access Signature)
* Zertifikatbasiert

Informationen zu den möglichen Konfigurationen finden Sie im Leitfaden zu [Sicherheit und Authentifizierung](security-authentication.md).

## <a name="enable-certificate-based-client-authentication-no-self-signed-certificates"></a>Aktivieren der zertifikatbasierten Clientauthentifizierung, keine selbstsignierten Zertifikate

```json
 {
  "Env": [
    "inbound:clientAuth:sasKeys:enabled=false",
    "inbound:clientAuth:clientCert:enabled=true",
    "inbound:clientAuth:clientCert:source=IoTEdge",
    "inbound:clientAuth:clientCert:allowUnknownCA=false"
  ]
}
 ```

## <a name="enable-certificate-based-client-authentication-allow-self-signed-certificates"></a>Aktivieren der zertifikatbasierten Clientauthentifizierung, selbstsignierte Zertifikate zulassen

```json
 {
  "Env": [
    "inbound:clientAuth:sasKeys:enabled=false",
    "inbound:clientAuth:clientCert:enabled=true",
    "inbound:clientAuth:clientCert:source=IoTEdge",
    "inbound:clientAuth:clientCert:allowUnknownCA=true"
  ]
}
```

>[!NOTE]
>Legen Sie die Eigenschaft **inbound:clientAuth:clientCert:allowUnknownCA** nur in Testumgebungen auf **true** fest, da Sie u. U. normalerweise selbstsignierte Zertifikate verwenden. Für Produktionsworkloads empfiehlt es sich, diese Eigenschaft auf **false** festzulegen und Zertifikate von einer Zertifizierungsstelle zu beziehen.

## <a name="enable-certificate-based-and-sas-key-based-client-authentication"></a>Aktivieren der zertifikatbasierten und der SAS-Schlüssel-basierten Clientauthentifizierung

```json
 {
  "Env": [
    "inbound:clientAuth:sasKeys:enabled=true",
    "inbound:clientAuth:sasKeys:key1=<some-secret1-here>",
    "inbound:clientAuth:sasKeys:key2=<some-secret2-here>",
    "inbound:clientAuth:clientCert:enabled=true",
    "inbound:clientAuth:clientCert:source=IoTEdge",
    "inbound:clientAuth:clientCert:allowUnknownCA=true"
  ]
}
 ```

>[!NOTE]
>Die SAS-Schlüssel-basierte Clientauthentifizierung ermöglicht es einem Nicht-IoT Edge-Modul, Verwaltungs- und Runtimevorgänge auszuführen. Hierfür müssen natürlich die API-Ports außerhalb des IoT Edge-Netzwerks zugänglich sein.

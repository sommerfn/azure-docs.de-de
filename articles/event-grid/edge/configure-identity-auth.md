---
title: Konfigurieren der Identität – Azure Event Grid-IoT Edge | Microsoft-Dokumentation
description: Konfigurieren der Identität des Event Grid-Moduls
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/05/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 336b6157128468169264d6ffa9564da4d9338aae
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2019
ms.locfileid: "72991819"
---
# <a name="configure-identity-for-the-event-grid-module"></a>Konfigurieren der Identität für das Event Grid-Modul

Dieser Artikel enthält Beispiele für die möglichen Identitätskonfigurationen für ein Event Grid-Modul. In der Standardeinstellung präsentiert das Event Grid-Modul sein Identitätszertifikat entsprechend der Konfiguration durch den IoT-Sicherheitsdaemon. Ein Identitätszertifikat wird vom Event Grid-Modul in seinen ausgehenden Aufrufen präsentiert, d. h. wenn Ereignisse übermittelt werden. Ein Abonnent eines Event Grid-Ereignisses kann dann validieren, dass es sich tatsächlich um das Event Grid-Modul handelt, welches das Ereignis gesendet hat, bevor er das Ereignis akzeptiert.

Informationen zu den möglichen Konfigurationen finden Sie im Leitfaden zu [Sicherheit und Authentifizierung](security-authentication.md).

## <a name="always-present-identity-certificate"></a>Immer vorhandenes Identitätszertifikat
Dies ist eine Beispielkonfiguration, bei der bei ausgehenden Aufrufen immer ein Identitätszertifikat präsentiert wird. 

```json
 {
  "Env": [
    "outbound:clientAuth:clientCert:enabled=true",
    "outbound:clientAuth:clientCert:source=IoTEdge"
  ]
}
 ```

## <a name="dont-present-identity-certificate"></a>Kein Identitätszertifikat wird präsentiert
Dies ist eine Beispielkonfiguration, bei der bei ausgehenden Aufrufen kein Identitätszertifikat präsentiert wird. 

```json
 {
  "Env": [
    "outbound:clientAuth:clientCert:enabled=false"
  ]
}
 ```

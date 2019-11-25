---
title: Konfigurieren von API-Protokollen – Azure Event Grid IoT Edge | Microsoft-Dokumentation
description: Konfigurieren Sie API-Protokolle, die von Event Grid in IoT Edge verfügbar gemacht werden.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: ''
ms.date: 10/03/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: f39968476f2fecf655e6c69bea2ff19304d2b465
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2019
ms.locfileid: "72991843"
---
# <a name="configure-event-grid-api-protocols"></a>Konfigurieren von Event Grid API-Protokollen

Dieser Leitfaden enthält Beispiele zu möglichen Protokollkonfigurationen für ein Event Grid-Modul. Das Event Grid-Modul macht die API für ihre Verwaltungs- und Laufzeitvorgänge verfügbar. Die folgende Tabelle enthält die Protokolle und Ports.

| Protocol | Port | BESCHREIBUNG |
| ---------------- | ------------ | ------------ |
| HTTP | 5888 | Standardmäßig deaktiviert. Nur während Tests nützlich. Nicht geeignet für Produktionsworkloads.
| HTTPS | 4438 | Standard

Informationen zu allen möglichen Konfigurationen finden Sie im Leitfaden zu [Sicherheit und Authentifizierung](security-authentication.md).

## <a name="expose-https-to-iot-modules-on-the-same-edge-network"></a>Verfügbarmachen von HTTPS in IoT-Modulen im selben Edge-Netzwerk

```json
 {
  "Env": [
    "inbound:serverAuth:tlsPolicy=strict",
    "inbound:serverAuth:serverCert:source=IoTEdge"
  ]
}
 ```

## <a name="enable-https-to-other-iot-modules-and-non-iot-workloads"></a>Aktivieren von HTTPS für andere IoT-Module und Nicht-IoT-Workloads

```json
 {
  "Env": [
    "inbound:serverAuth:tlsPolicy=strict",
    "inbound:serverAuth:serverCert:source=IoTEdge"
  ],
  "HostConfig": {
    "PortBindings": {
      "4438/tcp": [
        {
          "HostPort": "4438"
        }
      ]
    }
  }
}
 ```

>[!NOTE]
> Im Abschnitt **PortBindings** können Sie interne Ports den Ports des Containerhosts zuordnen. Diese Funktion ermöglicht es, das Event Grid-Modul von außerhalb des IoT Edge-Containernetzwerks zu erreichen, wenn das IoT Edge-Gerät öffentlich erreichbar ist.

## <a name="expose-http-and-https-to-iot-modules-on-the-same-edge-network"></a>Verfügbarmachen von HTTP- und HTTPS-Modulen für IoT-Module im selben Edge-Netzwerk

```json
 {
  "Env": [
    "inbound:serverAuth:tlsPolicy=enabled",
    "inbound:serverAuth:serverCert:source=IoTEdge"
  ]
}
 ```

## <a name="enable-http-and-https-to-other-iot-modules-and-non-iot-workloads"></a>Aktivieren von HTTP und HTTPS für andere IoT-Module und Nicht-IoT-Workloads

```json
 {
  "Env": [
    "inbound:serverAuth:tlsPolicy=enabled",
    "inbound:serverAuth:serverCert:source=IoTEdge"
  ],
  "HostConfig": {
    "PortBindings": {
      "4438/tcp": [
        {
          "HostPort": "4438"
        }
      ],
      "5888/tcp": [
        {
          "HostPort": "5888"
        }
      ]
    }
  }
}
 ```

>[!NOTE]
> Standardmäßig ist jedes IoT-Modul Teil der IoT Edge-Runtime, die vom Bridgenetzwerk erstellt wird. Sie ermöglicht verschiedenen IoT-Modulen im selben Netzwerk, miteinander zu kommunizieren. **PortBindings** gestattet es Ihnen, einen internen Containerport dem Hostcomputer zuzuordnen, wodurch jeder in die Lage versetzt wird, von außerhalb auf den Port des Event Grid-Moduls zugreifen zu können.

>[!IMPORTANT]
> Zwar können die Ports von außerhalb des IoT Edge-Netzwerks zugänglich gemacht werden, doch die Clientauthentifizierung erzwingt, wer tatsächlich Aufrufe an das Modul durchführen darf.

---
title: Konfigurieren der Authentifizierung für Webhookabonnenten – Azure Event Grid-IoT Edge | Microsoft-Dokumentation
description: Konfigurieren der Authentifizierung für Webhookabonnenten
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/06/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 97ed1e2ad84d895e9da0d96cd070e14acb46385d
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2019
ms.locfileid: "72991831"
---
# <a name="configure-webhook-subscriber-authentication"></a>Konfigurieren der Authentifizierung für Webhookabonnenten

Dieser Leitfaden enthält Beispiele zu möglichen Konfigurationen für Webhookabonnenten für ein Event Grid-Modul. Standardmäßig werden nur HTTPS-Endpunkte für Webhookabonnenten akzeptiert. Wenn der Abonnent ein selbstsigniertes Zertifikat präsentiert, wird dieses vom Event Grid-Modul abgelehnt.

## <a name="allow-only-https-subscriber"></a>Nur HTTPS-Abonnenten zulassen

```json
 {
  "Env": [
    "outbound:webhook:httpsOnly=true",
    "outbound:webhook:skipServerCertValidation=false",
    "outbound:webhook:allowUnknownCA=false"
  ]
}
 ```

## <a name="allow-https-subscriber-with-self-signed-certificate"></a>HTTPS-Abonnenten mit selbstsigniertem Zertifikat zulassen

```json
 {
  "Env": [
    "outbound:webhook:httpsOnly=true",
    "outbound:webhook:skipServerCertValidation=false",
    "outbound:webhook:allowUnknownCA=true"
  ]
}
 ```

>[!NOTE]
>Legen Sie die Eigenschaft `outbound:webhook:allowUnknownCA` nur in Testumgebungen auf `true` fest, da Sie u. U. normalerweise selbstsignierte Zertifikate verwenden. Für Produktionsworkloads empfiehlt es sich, sie auf **false** festzulegen.

## <a name="allow-https-subscriber-but-skip-certificate-validation"></a>HTTPS-Abonnenten zulassen, aber Zertifikatüberprüfung überspringen

```json
 {
  "Env": [
    "outbound:webhook:httpsOnly=true",
    "outbound:webhook:skipServerCertValidation=true",
    "outbound:webhook:allowUnknownCA=false"
  ]
}
 ```

>[!NOTE]
>Legen Sie die Eigenschaft `outbound:webhook:skipServerCertValidation` nur in Testumgebungen auf `true` fest, da Sie möglicherweise kein Zertifikat präsentieren, das authentifiziert werden muss. Für Produktionsworkloads empfiehlt es sich, sie auf **false** festzulegen.

## <a name="allow-both-http-and-https-with-self-signed-certificates"></a>Sowohl HTTP als auch HTTPS mit selbstsignierten Zertifikaten zulassen

```json
 {
  "Env": [
    "outbound:webhook:httpsOnly=false",
    "outbound:webhook:skipServerCertValidation=false",
    "outbound:webhook:allowUnknownCA=true"
  ]
}
 ```

>[!NOTE]
>Legen Sie die Eigenschaft `outbound:webhook:httpsOnly` nur in Testumgebungen auf `false` fest, da Sie möglicherweise erst einen HTTP-Abonnenten anzeigen möchten. Für Produktionsworkloads empfiehlt es sich, sie auf **true** festzulegen.

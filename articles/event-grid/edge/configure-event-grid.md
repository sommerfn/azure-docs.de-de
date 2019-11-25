---
title: Konfiguration – Azure Event Grid IoT Edge | Microsoft-Dokumentation
description: Konfiguration in Event Grid in IoT Edge.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/03/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: a9bac9a84e1ba034b011691ae82a1cb67eb71af0
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2019
ms.locfileid: "72991815"
---
# <a name="event-grid-configuration"></a>Event Grid-Konfiguration

Event Grid bietet viele Konfigurationen, die über die Umgebung geändert werden können. Im folgenden Abschnitt finden Sie eine Referenz für alle verfügbaren Optionen und ihre Standardwerte.

## <a name="tls-configuration"></a>TLS-Konfiguration

Informationen zur Clientauthentifizierung im Allgemeinen finden Sie unter [Sicherheit and Authentifizierung](security-authentication.md). Beispiele für die Verwendung finden Sie in [diesem Artikel](configure-api-protocol.md).

| Eigenschaftenname | BESCHREIBUNG |
| ---------------- | ------------ |
|`inbound:serverAuth:tlsPolicy`| TLS-Richtlinie des Event Grid-Moduls. Der Standardwert ist nur „HTTPS“.
|`inbound:serverAuth:serverCert:source`| Quelle des Serverzertifikats, das vom Event Grid-Modul für seine TLS-Konfiguration verwendet wird. Der Standardwert ist „IoT Edge“.

## <a name="incoming-client-authentication"></a>Eingehende Clientauthentifizierung

Informationen zur Clientauthentifizierung im Allgemeinen finden Sie unter [Sicherheit and Authentifizierung](security-authentication.md). Beispiele finden Sie in [diesem Artikel](configure-client-auth.md).

| Eigenschaftenname | BESCHREIBUNG |
| ---------------- | ------------ |
|`inbound:clientAuth:clientCert:enabled`| Zum De-/Aktivieren der zertifikatbasierten Clientauthentifizierung. Der Standardwert lautet „true“.
|`inbound:clientAuth:clientCert:source`| Quelle für die Überprüfung von Clientzertifikaten. Der Standardwert ist „IoT Edge“.
|`inbound:clientAuth:clientCert:allowUnknownCA`| Richtlinie zum Zulassen eines selbstsignierten Clientzertifikats. Der Standardwert lautet „true“.
|`inbound:clientAuth:sasKeys:enabled`| Zum De-/Aktivieren der SAS-Schlüssel-basierten Clientauthentifizierung. Der Standardwert ist „Aus“.
|`inbound:clientAuth:sasKeys:key1`| Einer der Werte, um eingehende Anforderungen zu überprüfen.
|`inbound:clientAuth:sasKeys:key2`| Optionaler zweiter Wert, um eingehende Anforderungen zu überprüfen.

## <a name="outgoing-client-authentication"></a>Ausgehende Clientauthentifizierung
Informationen zur Clientauthentifizierung im Allgemeinen finden Sie unter [Sicherheit and Authentifizierung](security-authentication.md). Beispiele finden Sie in [diesem Artikel](configure-identity-auth.md).

| Eigenschaftenname | BESCHREIBUNG |
| ---------------- | ------------ |
|`outbound:clientAuth:clientCert:enabled`| Zum De-/Aktivieren des Anfügens eines Identitätszertifikats für ausgehende Anforderungen. Der Standardwert lautet „true“.
|`outbound:clientAuth:clientCert:source`| Quelle zum Abrufen des ausgehenden Zertifikats des Event Grid-Moduls. Der Standardwert ist „IoT Edge“.

## <a name="webhook-event-handlers"></a>Webhook-Ereignishandler

Informationen zur Clientauthentifizierung im Allgemeinen finden Sie unter [Sicherheit and Authentifizierung](security-authentication.md). Beispiele finden Sie in [diesem Artikel](configure-webhook-subscriber-auth.md).

| Eigenschaftenname | BESCHREIBUNG |
| ---------------- | ------------ |
|`outbound:webhook:httpsOnly`| Richtlinie zur Steuerung, ob nur HTTPS-Abonnenten zulässig sind. Der Standardwert ist „true“ (nur HTTPS).
|`outbound:webhook:skipServerCertValidation`| Flag zur Steuerung, ob das Zertifikat des Abonnenten überprüft werden soll. Der Standardwert lautet „true“.
|`outbound:webhook:allowUnknownCA`| Richtlinie zur Steuerung, ob ein selbstsigniertes Zertifikat von einem Abonnenten vorgewiesen werden kann. Der Standardwert lautet „true“. 

## <a name="delivery-and-retry"></a>Übermittlung und Wiederholung

Informationen zu dieser Funktion im Allgemeinen finden Sie unter [Übermittlung und Wiederholung](delivery-retry.md).

| Eigenschaftenname | BESCHREIBUNG |
| ---------------- | ------------ |
| `broker:defaultMaxDeliveryAttempts` | Die maximale Anzahl der Versuche zur Übermittlung eines Ereignisses. Standardwert: 30.
| `broker:defaultEventTimeToLiveInSeconds` | Gültigkeitsdauer (Time-to-Live, TTL) in Sekunden, nach der ein Ereignis gelöscht wird, wenn es nicht übermittelt wird. Der Standardwert beträgt **7200** Sekunden.

## <a name="output-batching"></a>Ausgabebatches

Weitere Informationen zu dieser Funktion im Allgemeinen finden Sie unter [Übermittlung und Ausgabebatchverarbeitung](delivery-output-batching.md).

| Eigenschaftenname | BESCHREIBUNG |
| ---------------- | ------------ |
| `api:deliveryPolicyLimits:maxBatchSizeInBytes` | Der zulässige Höchstwert für den Knopf `ApproxBatchSizeInBytes`. Der Standardwert ist `1_058_576`.
| `api:deliveryPolicyLimits:maxEventsPerBatch` | Der zulässige Höchstwert für den Knopf `MaxEventsPerBatch`. Der Standardwert ist `50`.
| `broker:defaultMaxBatchSizeInBytes` | Maximale Größe der Übermittlungsanforderung, wenn nur `MaxEventsPerBatch` angegeben ist. Der Standardwert ist `1_058_576`.
| `broker:defaultMaxEventsPerBatch` | Maximale Anzahl der einem Batch hinzuzufügenden Ereignisse, wenn nur `MaxBatchSizeInBytes` angegeben ist. Der Standardwert ist `10`.

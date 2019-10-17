---
title: include file
description: include file
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 10/07/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: f7167cbb26e69941cade01ab8c0b8d9dc633f0d2
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72168379"
---
Beachten Sie bei der Arbeit mit benutzerdefinierten IPsec-Richtlinien die folgenden Anforderungen:

* **IKE**: Für IKE können Sie einen beliebigen Parameter aus der IKE-Verschlüsselung, einen beliebigen Parameter aus der IKE-Integrität sowie einen beliebigen Parameter aus der DH-Gruppe auswählen.
* **IPsec**: Für IPsec können Sie einen beliebigen Parameter aus der IPsec-Verschlüsselung, einen beliebigen Parameter aus der IPsec-Integrität sowie PFS auswählen. Falls einer der Parameter für die IPsec-Verschlüsselung oder die IPsec-Integrität GCM ist, müssen die Parameter für beide Einstellungen GCM sein.

>[!NOTE]
> Bei benutzerdefinierten IPsec-Richtlinien besteht kein Konzept von Antwortdienst und Initiator (im Gegensatz zu IPsec-Standardrichtlinien). Beide Seiten (lokale und Azure-VPN-Gateways) verwenden die gleichen Einstellungen für IKE Phase 1 und IKE Phase 2. Es werden beide Protokolle – IKEv1 und IKEv2 – unterstützt. Azure nur als Beantworter wird nicht unterstützt.
>

**Verfügbare Einstellungen und Parameter**

| Einstellung | Parameter |
|--- |--- |
| IKE-Verschlüsselung | AES256, AES192, AES128 |
| IKE-Integrität | SHA384, SHA256, SHA1 |
| DH-Gruppe | DHGroup24, ECP384, ECP256, DHGroup14, DHGroup2048, DHGroup2 |
| IPsec-Verschlüsselung | GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128 |
| IPsec-Integrität | GCMASE256, GCMAES192, GCMAES128, SHA256, SHA1 |
| PFS-Gruppe | PFS24, ECP384, ECP256, PFS2048, PFS2 |

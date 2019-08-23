---
title: include file
description: include file
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 08/21/2019
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 42e965b188db2b84579ab322fbe19781000dff7e
ms.sourcegitcommit: a3a40ad60b8ecd8dbaf7f756091a419b1fe3208e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69894139"
---
## <a name="storage"></a>Storage

|  |  |
|---------|---------|
| [Zulässige SKUs für Speicherkonten und virtuelle Computer](../articles/governance/policy/samples/allowed-skus-storage.md) | Erfordert, dass Speicherkonten und virtuelle Computer genehmigte SKUs verwenden. Verwendet integrierte Richtlinien, um die Verwendung von genehmigten SKUs zu gewährleisten. Sie geben ein Array von genehmigten SKUs für virtuelle Computer und ein Array von genehmigten SKUs für Speicherkonten an. |
| [Zulässige Speicherkonten-SKUs](../articles/governance/policy/samples/allowed-storage-account-skus.md) | Erfordert, dass Speicherkonten eine genehmigte SKU verwenden. Sie geben ein Array von zulässigen SKUs an. |
| [Verweigern des kalten Zugriffstierings für Speicherkonten](../articles/governance/policy/samples/deny-cool-access-tiering.md) | Untersagt die Verwendung des kalten Zugriffstierings für Blob-Speicherkonten.  |
| [Gewährleisten eines HTTPS-Datenverkehrs ausschließlich für Speicherkonten](../articles/governance/policy/samples/ensure-https-storage-account.md) | Erfordert, dass Speicherkonten HTTPS-Datenverkehr verwenden.  |
| [Gewährleisten der Verschlüsselung von Speicherdateien](../articles/governance/policy/samples/ensure-storage-file-encryption.md) | Erfordert, dass die Dateiverschlüsselung für Speicherkonten aktiviert ist.  |
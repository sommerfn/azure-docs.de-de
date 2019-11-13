---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/06/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: e3b3d944508a4261b78def0b3bee13f7395a8bf0
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73749007"
---
Führen Sie folgende Schritte durch, um die Zugriffsschlüssel oder Verbindungszeichenfolge Ihres Speicherkontos aus dem Azure-Portal anzuzeigen oder zu kopieren:

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
2. Suchen Sie nach Ihrem Speicherkonto.
3. Wählen Sie im Abschnitt **Einstellungen** der Speicherkontoübersicht die Option **Zugriffsschlüssel**. Daraufhin werden Ihre Zugriffsschlüssel zusammen mit der jeweiligen vollständigen Verbindungszeichenfolge angezeigt.
4. Suchen Sie unter **key1** nach dem Wert für **Schlüssel**, und klicken Sie dann auf die Schaltfläche **Kopieren**, um den Kontoschlüssel zu kopieren.
5. Alternativ können Sie die gesamte Verbindungszeichenfolge kopieren. Suchen Sie unter **key1** nach dem Wert für die **Verbindungszeichenfolge**, und klicken Sie dann auf die Schaltfläche **Kopieren**, um die Verbindungszeichenfolge zu kopieren.

    ![Ein Screenshot, der zeigt, wie Sie Zugriffsschlüssel im Azure-Portal anzeigen](media/storage-view-keys-include/portal-connection-string.png)

Sie können einen der beiden Schlüssel verwenden, um auf Azure Storage zuzugreifen. Im Allgemeinen empfiehlt es sich jedoch, den ersten Schlüssel zu verwenden und die Verwendung des zweiten Schlüssels bei der Rotation von Schlüsseln zu reservieren.

---
title: include file
description: include file
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 09/04/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 4604616cd4f2d6c75c272586df1331fc405061cb
ms.sourcegitcommit: 97605f3e7ff9b6f74e81f327edd19aefe79135d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70737490"
---
## <a name="error-conditionheadersnotsupported-from-a-web-application-using-azure-files-from-browser"></a>Fehler „ConditionHeadersNotSupported“ bei einer Webanwendung, die Azure Files über Browser verwendet

Der Fehler „ConditionHeadersNotSupported“ wird angezeigt, wenn versucht wird, über eine Anwendung, die bedingte Header verwendet (z.B. ein Webbrowser), auf in Azure Files gehostete Inhalte zuzugreifen und der Zugriff fehlschlägt. Der Fehler besagt, dass bedingte header nicht unterstützt werden.

![Azure Files-Fehler bei bedingten Headern](media/storage-files-condition-headers/conditionalerror.png)

### <a name="cause"></a>Ursache

Bedingte Header werden noch nicht unterstützt. Anwendungen, die diese Header implementieren, müssen die vollständige Datei jedes Mal anfordern, wenn sie darauf zugreifen.

### <a name="workaround"></a>Problemumgehung

Wenn eine neue Datei hochgeladen wird, lautet der Wert für die Eigenschaft „cache-control“ standardmäßig „no-cache“. Um zu erzwingen, dass die Anwendung die Datei jedes Mal anfordert, muss der Wert für die Eigenschaft „cache-control“ der Datei von „no-cache“ auf „no-cache, no-store, must-revalidate“ aktualisiert werden. Dies kann über [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) geschehen.

![Änderung des Inhaltscaches von Storage-Explorer für bedingte Header von Azure Files](media/storage-files-condition-headers/storage-explorer-cache.png)
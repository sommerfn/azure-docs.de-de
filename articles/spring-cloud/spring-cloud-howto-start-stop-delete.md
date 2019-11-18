---
title: Starten, Beenden und Löschen einer Azure Spring Cloud-Anwendung | Microsoft-Dokumentation
description: Starten, Beenden und Löschen einer Azure Spring Cloud-Anwendung
author: jpconnock
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: jeconnoc
ms.openlocfilehash: 9f537ab425428728137e04713e434d8dc09e065a
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73607721"
---
# <a name="how-to-start-stop-and-delete-your-azure-spring-cloud-application"></a>Starten, Beenden und Löschen einer Azure Spring Cloud-Anwendung

In diesem Leitfaden wird erläutert, wie Sie den Zustand einer Anwendung in Azure Spring Cloud mithilfe des Azure-Portals oder der CLI ändern.

## <a name="using-the-azure-portal"></a>Verwenden des Azure-Portals

Nach dem Bereitstellen einer Anwendung können Sie sie über das Azure-Portal **starten**, **beenden** und **löschen**.

1. Navigieren Sie im Azure-Portal zu Ihrer Azure Spring Cloud-Dienstinstanz.
1. Wählen Sie die Registerkarte **Anwendungsdashboard** aus.
1. Wählen Sie die Anwendung aus, deren Zustand Sie ändern möchten.
2. Auf der Seite **Übersicht** der Anwendung finden Sie Schaltflächen zum **Starten/Beenden**, **Neustarten** und **Löschen** der Anwendung.

## <a name="using-the-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle

> [!NOTE]
> Sie können optionale Parameter verwenden und Standardwerte mit der Azure CLI konfigurieren. Weitere Informationen finden Sie in der [Referenzdokumentation](spring-cloud-cli-reference.md).  

Installieren Sie die Spring Cloud-Erweiterung für die Azure-Befehlszeilenschnittstelle:

```azurecli
az extension add --name spring-cloud
```

* So starten Sie Ihre Anwendung:
    ```azurecli
    az spring-cloud app start -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

* So beenden Sie Ihre Anwendung:
    ```azurecli
    az spring-cloud app stop -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

* So starten Sie Ihre Anwendung neu:
    ```azurecli
    az spring-cloud app restart -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

* So löschen Sie Ihre Anwendung:
    ```azurecli
    az spring-cloud app delete -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

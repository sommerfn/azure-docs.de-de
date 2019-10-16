---
title: Starten, Beenden und Löschen einer Azure Spring Cloud-Anwendung | Microsoft-Dokumentation
description: Starten, Beenden und Löschen einer Azure Spring Cloud-Anwendung
services: spring-cloud
author: v-vasuke
manager: jeconnoc
editor: ''
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/07/2019
ms.author: v-vasuke
ms.openlocfilehash: f7f76644d13c20704d2c3bd908176ac452df2a20
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72038439"
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

* So starten Sie Ihre Anwendung:
    ```Azure CLI
    az spring-cloud app start -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

* So beenden Sie Ihre Anwendung:
    ```Azure CLI
    az spring-cloud app stop -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

* So starten Sie Ihre Anwendung neu:
    ```Azure CLI
    az spring-cloud app restart -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

* So löschen Sie Ihre Anwendung:
    ```Azure CLI
    az spring-cloud app delete -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

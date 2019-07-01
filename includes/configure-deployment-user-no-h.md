---
title: include file
description: include file
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/14/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: ebea8bfd69a4df605142ab82f3efbc7d97d34529
ms.sourcegitcommit: 22c97298aa0e8bd848ff949f2886c8ad538c1473
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2019
ms.locfileid: "67143978"
---
Konfigurieren Sie in Azure Cloud Shell mit dem Befehl [`az webapp deployment user set`](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set) Anmeldeinformationen für die Bereitstellung. Bei der FTP-Bereitstellung und der lokalen Git-Bereitstellung für eine Web-App verwenden Sie diesen Bereitstellungsbenutzer. Der Benutzername und das Kennwort gelten auf Kontoebene. _Sie unterscheiden sich von den Anmeldeinformationen Ihres Azure-Abonnements._

Ersetzen Sie im folgenden Beispiel *\<username>* und *\<password>* (einschließlich der spitzen Klammern) durch einen neuen Benutzernamen und ein neues Kennwort. Der Benutzername muss in Azure eindeutig sein. Das Kennwort muss mindestens acht Zeichen lang sein und zwei der folgenden drei Elemente enthalten: Buchstaben, Zahlen und Symbole.

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

Sie erhalten eine JSON-Ausgabe, in der das Kennwort als `null` angezeigt wird. Wenn Sie den Fehler `'Conflict'. Details: 409` erhalten, müssen Sie den Benutzernamen ändern. Wenn Sie den Fehler `'Bad Request'. Details: 400` erhalten, müssen Sie ein sichereres Kennwort verwenden. Der Bereitstellungsbenutzername darf bei lokalen Git-Pushübertragungen kein @-Zeichen enthalten.

Sie konfigurieren diesen Bereitstellungsbenutzer nur einmal. Sie können ihn für alle Azure-Bereitstellungen verwenden.

> [!NOTE]
> Notieren Sie sich den Benutzernamen und das Kennwort. Sie benötigen sie später für die Bereitstellung der Web-App.
>
>

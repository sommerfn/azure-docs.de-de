---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/18/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 5e605e4ad987db16b98649b32dc96fa1620b1564
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "69011990"
---
Die Clientbibliothek [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) für .NET (Vorschauversion) vereinfacht den Prozess des Abrufens und Erneuerns eines Tokens aus dem Code. Mit der Clientbibliothek zur App-Authentifizierung wird die Authentifizierung automatisch verwaltet. Mit der Bibliothek werden die Anmeldeinformationen des Entwicklers für die Authentifizierung bei der lokalen Entwicklung verwendet. Während der lokalen Entwicklung sind Entwickleranmeldeinformationen sicherer, da Sie keine Azure AD-Anmeldeinformationen erstellen oder Anmeldeinformationen zwischen Entwicklern freigeben müssen. Wenn die Lösung später in Azure bereitgestellt wird, wechselt die Bibliothek automatisch zur Verwendung der Anwendungsanmeldeinformationen.

Zur Verwendung der Bibliothek für die App-Authentifizierung in einer Azure Storage-Anwendung installieren Sie das neueste Vorschaupaket von [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) sowie die neueste Version der [allgemeinen Azure Storage-Clientbibliothek für .NET](https://www.nuget.org/packages/Microsoft.Azure.Storage.Common/) und der [Clientbibliothek für Azure Blob Storage für .NET](https://www.nuget.org/packages/Microsoft.Azure.Storage.Blob/). Fügen Sie dem Code folgende **using**-Anweisungen hinzu:

```csharp
using Microsoft.Azure.Services.AppAuthentication;
using Microsoft.Azure.Storage.Auth;
using Microsoft.Azure.Storage.Blob;
```

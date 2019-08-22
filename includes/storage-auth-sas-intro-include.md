---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/25/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 3af8077627d56ce44c5e2959e2c79b967b09d9e5
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "69011938"
---
Eine Shared Access Signature (SAS) ermöglicht Ihnen, eingeschränkten Zugriff auf Container und Blobs in Ihrem Speicherkonto zu gewähren. Wenn Sie eine SAS erstellen, geben Sie ihre Einschränkungen an, einschließlich der Azure Storage-Ressourcen, auf die Clients zugreifen dürfen, welche Berechtigungen sie für diese Ressourcen haben und wie lange die SAS gültig ist.

Jede SAS wird mit einem Schlüssel signiert. Zum Signieren einer SAS stehen zwei Möglichkeiten zur Verfügung:

- Mit einem Schlüssel, der mit Azure Active Directory-Anmeldeinformationen (Azure AD) erstellt wurde. Eine mit Azure AD-Anmeldeinformationen signierte SAS ist eine SAS für die *Benutzerdelegierung*.
- Mit dem Speicherkontoschlüssel. Sowohl eine *Dienst-SAS* als auch eine *Konto-SAS* wird mit dem Speicherkontoschlüssel signiert.

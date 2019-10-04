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
ms.openlocfilehash: f3b9f6c27fb8d423350eac5d286c9859ad6fbd37
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70104254"
---
## <a name="about-the-user-delegation-sas-preview"></a>Informationen zur SAS für die Benutzerdelegierung (Vorschau)

Ein SAS-Token für den Zugriff auf einen Container oder ein Blob kann entweder mit Azure AD-Anmeldeinformationen oder einem Kontoschlüssel geschützt werden. Eine mit Azure AD-Anmeldeinformationen gesicherte SAS wird als SAS für die Benutzerdelegierung bezeichnet, da das OAuth 2.0-Token, das zum Signieren der SAS verwendet wird, im Namen des Benutzers angefordert wird.

Microsoft empfiehlt als bewährte Methode, nach Möglichkeit Azure AD-Anmeldeinformationen anstelle des Kontoschlüssels zu verwenden, der leichter kompromittiert werden kann. Wenn Ihr Anwendungsentwurf Shared Access Signatures erfordert, verwenden Sie Azure AD-Anmeldeinformationen, um eine SAS für die Benutzerdelegierung zu erstellen und damit die Sicherheit zu erhöhen. Weitere Informationen zur SAS für die Benutzerdelegierung finden Sie unter [Erstellen einer SAS für die Benutzerdelegierung](/rest/api/storageservices/create-user-delegation-sas).

> [!NOTE]
> Diese Vorschau der SAS für die Benutzerdelegierung ist nur für die Verwendung außerhalb der Produktion bestimmt.

> [!CAUTION]
> Jeder Client, der über eine gültige SAS verfügt, kann auf Daten in Ihrem Speicherkonto zugreifen, sofern von dieser SAS zugelassen. Es ist wichtig, eine SAS vor böswilliger oder unbeabsichtigter Verwendung zu schützen. Verteilen Sie eine SAS mit Diskretion, und halten Sie einen Plan für den Widerruf einer kompromittierten SAS bereit.

Weitere Informationen zu SAS (Shared Access Signatures) finden Sie unter [Gewähren von eingeschränktem Zugriff auf Azure Storage-Ressourcen mithilfe von SAS (Shared Access Signature)](../articles/storage/common/storage-sas-overview.md).

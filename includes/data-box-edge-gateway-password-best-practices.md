---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: 86b18c2a6fc3fbf342f5b7a4b6b563432c605f73
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/05/2019
ms.locfileid: "68781453"
---
Beachten Sie diese bewährten Methoden:

- Es empfiehlt sich, alle Kennwörter an einem sicheren Ort zu speichern, sodass Sie ein Kennwort nicht zurücksetzen müssen, wenn Sie es vergessen haben. Der Verwaltungsdienst kann vorhandene Kennwörter nicht abrufen. Er kann diese nur über das Azure-Portal zurücksetzen. Wenn Sie ein Kennwort zurücksetzen, dürfen Sie nicht vergessen, zuvor alle Benutzer zu benachrichtigen.
- Auf die Windows PowerShell-Oberfläche Ihres Geräts können Sie remote über HTTP zugreifen. Aus Sicherheitsgründen empfiehlt es sich, HTTP nur in vertrauenswürdigen Netzwerken zu verwenden.
- Gerätekennwörter müssen sicher und gut geschützt sein. Berücksichtigen Sie die [bewährten Methoden für Kennwörter](https://docs.microsoft.com/azure/security/fundamentals/identity-management-best-practices#enable-password-management).
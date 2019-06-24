---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: 86d1cf5e103bcbb13782aa7a2a84092aa426d670
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2019
ms.locfileid: "67178730"
---
Beachten Sie diese bewährten Methoden:

- Es empfiehlt sich, alle Kennwörter an einem sicheren Ort zu speichern, sodass Sie ein Kennwort nicht zurücksetzen müssen, wenn Sie es vergessen haben. Der Verwaltungsdienst kann vorhandene Kennwörter nicht abrufen. Er kann diese nur über das Azure-Portal zurücksetzen. Wenn Sie ein Kennwort zurücksetzen, dürfen Sie nicht vergessen, zuvor alle Benutzer zu benachrichtigen.
- Auf die Windows PowerShell-Oberfläche Ihres Geräts können Sie remote über HTTP zugreifen. Aus Sicherheitsgründen empfiehlt es sich, HTTP nur in vertrauenswürdigen Netzwerken zu verwenden.
- Gerätekennwörter müssen sicher und gut geschützt sein. Berücksichtigen Sie die [bewährten Methoden für Kennwörter](https://docs.microsoft.com/azure/security/azure-security-identity-management-best-practices#enable-password-management).
---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 4bd890adcaa4982c52366faed0f2975729290360
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73612069"
---
Die folgende Tabelle enthält die Grenzwerte, die für Ressourcen des Azure IoT Hub Device Provisioning-Diensts gelten.

| Resource | Begrenzung |
| --- | --- |
| Maximale Anzahl von Gerätebereitstellungsdiensten pro Azure-Abonnement | 10 |
| Maximale Anzahl von Registrierungen (Enrollments) | 1\.000.000 |
| Maximale Anzahl von Registrierungen (Registrations) | 1\.000.000 |
| Maximale Anzahl von Registrierungsgruppen | 100 |
| Maximale Anzahl von Zertifizierungsstellen | 25 |
| Maximale Anzahl der verknüpften IoT-Hubs | 50 |
| Maximale Nachrichtengröße | 96 KB|


> [!NOTE]
> Sie können sich an den [Microsoft-Support](https://azure.microsoft.com/support/options/) wenden, um die Anzahl von Instanzen in Ihrem Abonnement erhöhen zu lassen.

> [!NOTE]
> Sie können sich an den [Microsoft-Support](https://azure.microsoft.com/support/options/) wenden, um die Anzahl der Registrierungen für Ihren Bereitstellungsdienst zu erhöhen.

Der Device Provisioning Service drosselt Anforderungen, wenn die folgenden Kontingente überschritten werden.

| Drosselung | Wert pro Einheit |
| --- | --- |
| Vorgänge | 200/Minute/Dienst |
| Geräteregistrierungen | 200/Minute/Dienst |
| Abrufvorgang für Geräte | 5/10 Sekunden/Gerät |

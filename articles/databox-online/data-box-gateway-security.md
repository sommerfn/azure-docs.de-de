---
title: Azure Data Box Gateway – Sicherheit | Microsoft-Dokumentation
description: Beschreibt die Sicherheits- und Datenschutzfeatures, die Ihr virtuelles Azure Data Box Gateway-Gerät, Ihren Dienst und Ihre Daten sowohl lokal als auch in der Cloud schützen.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: 230d1a28ba15a8736e46c02cb08217a28fc18599
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "64695186"
---
# <a name="azure-data-box-gateway-security-and-data-protection"></a>Azure Data Box Gateway: Sicherheit und Schutz von Daten

Sicherheit ist bei der Einführung neuer Technologien immer ein zentrales Anliegen. Das gilt insbesondere, wenn die Technologie mit vertraulichen oder proprietären Daten verwendet wird. Azure Data Box Gateway trägt dazu bei, dass Ihre Daten nur von autorisierten Entitäten angezeigt, geändert oder gelöscht werden können.

In diesem Artikel werden die Sicherheitsmerkmale von Azure Data Box Gateway beschrieben, mit denen die einzelnen Komponenten der Lösung und die darin gespeicherten Daten geschützt werden.

Die Lösung Data Box Gateway besteht aus vier Hauptkomponenten, die miteinander interagieren:

- **In Azure gehosteter Data Box Gateway-Dienst:** Die Verwaltungsressource zum Erstellen des Geräteauftrags, zum Konfigurieren des Geräts sowie zum anschließenden Nachverfolgen des Auftrags bis zur Erfüllung
- **Data Box Gateway-Gerät:** Das virtuelle Gerät, das Sie im Hypervisor des angegebenen Systems bereitgestellt haben. Dieses virtuelle Gerät wird verwendet, um Ihre lokalen Daten in Azure zu importieren.
- **Mit dem Gerät verbundene Clients/Hosts:** Die Clients in Ihrer Infrastruktur, die mit dem Data Box Gateway-Gerät verbunden werden und zu schützende Daten enthalten
- **Cloudspeicher:** Der Speicherort auf der Azure-Cloudplattform, an dem die Daten gespeichert werden. Hierbei handelt es sich in der Regel um das Speicherkonto, das mit der erstellten Data Box Gateway-Ressource verknüpft ist.


## <a name="data-box-gateway-service-protection"></a>Schutz des Data Box Gateway-Diensts

Der Data Box Gateway-Dienst ist ein in Azure gehosteter Verwaltungsdienst. Der Dienst wird zum Konfigurieren und Verwalten des Geräts verwendet.

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-service-protection.md)]

## <a name="data-box-gateway-device-protection"></a>Schutz des Data Box Gateway-Geräts

Das Data Box Gateway-Gerät ist ein virtuelles Gerät, das im Hypervisor eines angegebenen lokalen Systems bereitgestellt wird. Das Gerät hilft dabei, Daten an Azure senden. Für Ihr Gerät gilt Folgendes:

- Es benötigt einen Aktivierungsschlüssel für den Zugriff auf den Data Box Edge/Data Box Gateway-Dienst.
- Es ist jederzeit durch ein Gerätekennwort geschützt.
<!---  secure boot enabled.
- Runs Windows Defender Device Guard. Device Guard allows you to run only trusted applications that you define in your code integrity policies.-->

### <a name="protect-the-device-via-activation-key"></a>Schützen des Geräts mittels Aktivierungsschlüssel

Dem Data Box Gateway-Dienst, den Sie in Ihrem Azure-Abonnement erstellt haben, können nur autorisierte Data Box Gateway-Geräte hinzugefügt werden. Um ein Gerät zu autorisieren, müssen Sie es mithilfe eines Aktivierungsschlüssels für den Data Box Gateway-Dienst aktivieren.

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-activation-key.md)]

Weitere Informationen finden Sie unter [Abrufen des Aktivierungsschlüssels](data-box-gateway-deploy-prep.md#get-the-activation-key).

### <a name="protect-the-device-via-password"></a>Schützen des Geräts mittels Kennwort

Mit Kennwörtern wird sichergestellt, dass nur autorisierte Benutzer Zugriff auf Ihre Daten haben. Data Box Gateway-Geräte sind nach dem Start gesperrt.

Ihre Möglichkeiten:

- Stellen Sie über einen Browser eine Verbindung mit der lokalen Webbenutzeroberfläche des Geräts her, und geben Sie ein Kennwort an, um sich bei dem Gerät anzumelden.
- Stellen Sie eine HTTP-Remoteverbindung mit der PowerShell-Schnittstelle des Geräts her. Die Remoteverwaltung ist standardmäßig aktiviert. Anschließend können Sie das Gerätekennwort angeben, um sich bei dem Gerät anzumelden. Weitere Informationen finden Sie unter [Herstellen einer Remoteverbindung mit Ihrem Data Box Gateway-Gerät](data-box-gateway-connect-powershell-interface.md#connect-to-the-powershell-interface).

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-password-best-practices.md)]
- Verwenden Sie zum [Ändern des Kennworts](data-box-gateway-manage-access-power-connectivity-mode.md#manage-device-access) die lokale Webbenutzeroberfläche. Sollten Sie das Kennwort ändern, benachrichtigen Sie alle Benutzer mit Remotezugriff, damit bei ihnen keine Anmeldefehler auftreten.


## <a name="protect-your-data"></a>Schützen Ihrer Daten

In diesem Abschnitt werden die Data Box Gateway-Sicherheitsfeatures für Daten während der Übertragung und für gespeicherte Daten beschrieben.

### <a name="protect-data-at-rest"></a>Schutz gespeicherter Daten

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-data-rest.md)]

### <a name="protect-data-in-flight"></a>Schützen von Daten während der Übertragung

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-data-flight.md)]

### <a name="protect-data-via-storage-accounts"></a>Schützen von Daten über Speicherkonten

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-protect-data-storage-accounts.md)]
- Rotieren und [synchronisieren Sie Ihre Speicherkontoschlüssel](data-box-gateway-manage-shares.md#sync-storage-keys) regelmäßig, um zu vermeiden, dass nicht autorisierte Benutzer auf Ihr Speicherkonto zugreifen können.

## <a name="manage-personal-information"></a>Verwalten persönlicher Informationen

Der Data Box Gateway-Dienst erfasst persönliche Informationen in folgenden Szenarien:

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-manage-personal-data.md)]

Eine Anleitung zum Anzeigen der Liste mit den Benutzern, die auf eine Freigabe zugreifen oder sie löschen können, finden Sie unter [Verwalten von Freigaben auf Data Box Gateway](data-box-gateway-manage-shares.md).

Weitere Informationen finden Sie im [Trust Center in der Microsoft-Datenschutzrichtlinie](https://www.microsoft.com/trustcenter).

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen Ihres Data Box Gateway-Geräts](data-box-gateway-deploy-prep.md)

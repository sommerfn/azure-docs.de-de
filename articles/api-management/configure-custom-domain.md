---
title: Konfigurieren eines benutzerdefinierten Domänennamens für Ihre Azure API Management-Instanz | Microsoft-Dokumentation
description: In diesem Thema erfahren Sie, wie Sie einen benutzerdefinierten Domänennamen für Ihre Azure API Management-Instanz konfigurieren.
services: api-management
documentationcenter: ''
author: vladvino
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 08/12/2019
ms.author: apimpm
ms.openlocfilehash: ba2288fa4add6df6b7861b5f8a99bcd3ee8b4c12
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73472191"
---
# <a name="configure-a-custom-domain-name"></a>Konfigurieren eines benutzerdefinierten Domänennamens

Wenn Sie eine Azure API-Instanz (API Management) erstellen, wird sie von Azure einer Unterdomäne von azure-api.net zugewiesen (z. B. `apim-service-name.azure-api.net`). Sie können Ihre API Management-Endpunkte aber unter Ihrem eigenen benutzerdefinierten Domänennamen verfügbar machen (z. B. **contoso.com**). In diesem Tutorial erfahren Sie, wie Sie Endpunkten, die durch eine API Management-Instanz verfügbar gemacht werden, einen vorhandenen benutzerdefinierten DNS-Namen zuordnen.

> [!WARNING]
> Kunden, die das Anheften von Zertifikaten nutzen möchten, um die Sicherheit ihrer Anwendungen zu erhöhen, müssen einen benutzerdefinierten Domänennamen und ein von ihnen verwaltetes Zertifikat (nicht das Standardzertifikat) verwenden. Kunden, die stattdessen das Standardzertifikat anheften, übernehmen eine harte Abhängigkeit für die Eigenschaften des nicht von ihnen verwalteten Zertifikats. Dies ist kein empfohlenes Verfahren.

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen der in diesem Artikel beschriebenen Schritte benötigen Sie Folgendes:

-   Ein aktives Azure-Abonnement.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

-   Eine API Management-Instanz. Weitere Informationen finden Sie unter [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md).
-   Einen benutzerdefinierten Domänennamen, der sich in Ihrem Besitz oder im Besitz Ihrer Organisation befindet. Dieses Thema enthält keine Anleitung zum Erwerben eines benutzerdefinierten Domänennamens.
-   Einen CNAME-Eintrag, der auf einem DNS-Server gehostet wird, der den benutzerdefinierten Domänennamen dem Standarddomänennamen Ihrer API Management-Instanz zuordnet. Dieses Thema enthält keine Anleitung zum Hosten eines CNAME-Eintrags.
-   Sie benötigen ein gültiges Zertifikat mit einem öffentlichen und privaten Schlüssel (.pfx). Der Antragstellername oder der alternative Antragstellername (Subject Alternative Name, SAN) muss dem Domänennamen entsprechen, damit die API Management-Instanz auf sichere Weise URLs über SSL verfügbar machen kann.

## <a name="use-the-azure-portal-to-set-a-custom-domain-name"></a>Festlegen eines benutzerdefinierten Domänennamens über das Azure-Portal

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) zu Ihrer API Management-Instanz.
1. Klicken Sie auf **Benutzerdefinierte Domänen**.

    Es stehen mehrere Endpunkte zur Verfügung, denen Sie einen benutzerdefinierten Domänennamen zuweisen können. Derzeit sind folgende Endpunkte verfügbar:

    - **Gateway** (Standardwert: `<apim-service-name>.azure-api.net`),
    - **Portal** (Standardwert: `<apim-service-name>.portal.azure-api.net`)
    - **Verwaltung** (Standardwert: `<apim-service-name>.management.azure-api.net`)
    - **SCM** (Standardwert: `<apim-service-name>.scm.azure-api.net`).

    > [!NOTE]
    > Nur der **Gateway**endpunkt steht im Tarif „Consumption“ (Verbrauch) zur Konfiguration zur Verfügung.
    > Sie können alle oder nur bestimmte Endpunkte aktualisieren. Kunden aktualisieren üblicherweise **Gateway** (URL zum Aufrufen der über API Management verfügbar gemachten API) und **Portal** (URL des Entwicklerportals).
    > Die Endpunkte **Verwaltung** und **SCM** werden nur intern von Besitzern der API Management-Instanz verwendet, weshalb ihnen seltener ein benutzerdefinierter Domänenname zugewiesen wird.
    > Der **Premium**-Tarif unterstützt mehrere Hostnamen für den **Gateway**endpunkt.

1. Wählen Sie den Endpunkt aus, den Sie aktualisieren möchten.
1. Klicken Sie im Fenster auf der rechten Seite auf **Benutzerdefiniert**.

    - Geben Sie unter **Benutzerdefinierter Domänenname** den gewünschten Namen an. Beispiel: `api.contoso.com`.
    - Wählen Sie unter **Zertifikat** ein Zertifikat aus „Key Vault“ aus. Wenn das Zertifikat mit einem Kennwort geschützt ist, können Sie auch eine gültige PFX-Datei hochladen und deren **Kennwort** angeben.

    > [!NOTE]
    > Platzhalterdomänennamen (etwa `*.contoso.com`) werden in allen Tarifen mit Ausnahme des Verbrauchstarifs unterstützt.

    > [!TIP]
    > Wir empfehlen Ihnen die Nutzung von Azure Key Vault zur Verwaltung von Zertifikaten. Legen Sie hierfür die Option „Automatische Rotation“ fest.
    > Sollten Sie das benutzerdefinierte SSL-Domänenzertifikat mit Azure Key Vault verwalten, stellen Sie sicher, dass das Zertifikat [als ein _Zertifikat_](https://docs.microsoft.com/rest/api/keyvault/CreateCertificate/CreateCertificate) und nicht als ein _Geheimnis_ in Key Vault eingefügt wird.
    >
    > Zum Abrufen des SSL-Zertifikats benötigt API Management die Liste und die Berechtigung zum Abrufen geheimer Schlüssel für die Azure Key Vault-Instanz, die das Zertifikat enthält. Wenn Sie Azure-Portal verwenden, werden alle notwendigen Konfigurationsschritte automatisch durchgeführt. Wenn Sie Befehlszeilentools oder eine Verwaltungs-API verwenden, müssen diese Berechtigungen manuell erteilt werden. Dazu sind zwei Schritte erforderlich. Verwenden Sie zuerst die Seite „Verwaltete Identitäten“ in Ihrer API Management-Instanz, um sicherzustellen, dass die verwaltete Instanz aktiviert ist. Notieren Sie sich die Prinzipal-ID, die auf der Seite angezeigt wird. Weisen Sie dieser Prinzipal-ID anschließend die Berechtigungsliste und die Berechtigung zum Abrufen geheimer Schlüssel für die Azure Key Vault-Instanz mit dem Zertifikat zu.
    >
    > Wenn für das Zertifikat die automatische Rotation eingerichtet ist, wählt API Management automatisch die neuste Version aus, ohne dass der Dienst ausfällt (wenn Ihr API Management-Tarif über eine SLA verfügt, also bei allen Tarifen mit Ausnahme des Developer-Tarifs).

1. Klicken Sie auf „Übernehmen“.

    > [!NOTE]
    > Die Zuweisung des Zertifikats kann je nach Größe der Bereitstellung 15 Minuten und länger dauern. Bei der Entwickler-SKU tritt eine Ausfallzeit auf, bei der SKU „Basic“ und höheren SKUs jedoch nicht.

[!INCLUDE [api-management-custom-domain](../../includes/api-management-custom-domain.md)]

## <a name="dns-configuration"></a>DNS-Konfiguration

Bei der DNS-Konfiguration für Ihren benutzerdefinierten Domänennamen haben Sie zwei Möglichkeiten:

-   Konfigurieren eines CNAME-Eintrags, der auf den Endpunkt Ihres konfigurierten benutzerdefinierten Domänennamens verweist
-   Konfigurieren eines A-Eintrags, der auf die Gateway-IP-Adresse für API Management verweist

> [!NOTE]
> Die IP-Adresse der API Management-Instanz ist zwar statisch, kann sich aber dennoch in bestimmten Szenarien ändern. Daher empfiehlt es sich, beim Konfigurieren der benutzerdefinierten Domäne CNAME zu verwenden. Berücksichtigen Sie dies bei der Wahl der DNS-Konfigurationsmethode. Weitere Informationen finden Sie in den [häufig gestellten Fragen zu API Management](api-management-faq.md#how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services).

## <a name="next-steps"></a>Nächste Schritte

[Upgrade and scale an API Management instance](upgrade-and-scale.md) (Upgraden und Skalieren einer API Management-Instanz)

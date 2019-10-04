---
title: Wann und wie verwenden Sie einen Azure Multi-Factor Authentication-Anbieter? – Azure Active Directory
description: Wann sollten Sie einen Auth-Anbieter mit Azure MFA verwenden?
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 399fccf9aaaeb9e252527e80a6549ee286bb1898
ms.sourcegitcommit: c71306fb197b433f7b7d23662d013eaae269dc9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2019
ms.locfileid: "68369376"
---
# <a name="when-to-use-an-azure-multi-factor-authentication-provider"></a>Erste Schritte mit einem Azure Multi-Factor Authentication-Anbieter

Die zweistufige Überprüfung ist standardmäßig für globale Administratoren, die mit Azure Active Directory arbeiten, und für Office 365-Benutzer verfügbar. Falls Sie die [erweiterten Funktionen](howto-mfa-mfasettings.md) nutzen möchten, müssen Sie die Vollversion von Azure Multi-Factor Authentication (MFA) erwerben.

Ein Azure Multi-Factor Authentication-Anbieter wird verwendet, um Features zu nutzen, die von Azure MFA für Benutzer bereitgestellt werden, die **keine Lizenzen haben**.

> [!NOTE]
> Ab dem 1. September 2018 können keine neuen Authentifizierungsanbieter mehr erstellt werden. Vorhandene Authentifizierungsanbieter können ggf. weiterhin verwendet und aktualisiert werden, aber Migration ist nicht mehr möglich. Die mehrstufige Authentifizierung ist in Azure AD Premium-Lizenzen weiterhin als Feature verfügbar.

## <a name="caveats-related-to-the-azure-mfa-sdk"></a>Einschränkungen im Zusammenhang mit dem Azure MFA SDK

Beachten Sie, dass das SDK veraltet ist und nur noch bis zum 14. November 2018 funktioniert. Nach diesem Zeitpunkt führen Aufrufe an das SDK zu Fehlern.

## <a name="what-is-an-mfa-provider"></a>Was ist ein MFA-Anbieter?

Es gibt zwei Arten von Authentifizierungsanbietern. Der Unterschied besteht darin, wie Ihr Azure-Abonnement in Rechnung gestellt wird. Bei der Option pro Authentifizierung wird die Anzahl der Authentifizierungen berechnet, die für Ihren Mandanten in einem Monat ausgeführt werden. Diese Option ist am besten geeignet, wenn sich mehrere Benutzer nur gelegentlich authentifizieren. Bei der Option pro Benutzer wird die Anzahl der Personen in Ihrem Mandanten berechnet, die in einem Monat die zweistufige Überprüfung ausführen. Diese Option empfiehlt sich, wenn Sie über einige Benutzer mit Lizenzen verfügen, MFA aber auf weitere Benutzer außerhalb Ihrer Lizenzierungsgrenzwerte erweitern müssen.

## <a name="manage-your-mfa-provider"></a>Verwalten Ihres MFA-Anbieters

Das Nutzungsmodell (pro aktiviertem Benutzer oder pro Authentifizierung) kann nach der Erstellung eines MFA-Anbieters nicht mehr geändert werden.

Sofern die Anzahl erworbenen Lizenzen für alle Benutzer ausreicht, die für MFA aktiviert sind, können Sie den MFA-Anbieter auch ganz löschen.

Falls Ihr MFA-Anbieter nicht mit einem Azure AD-Mandanten verknüpft ist oder Sie den neuen MFA-Anbieter mit einem anderen Azure AD-Mandanten verknüpfen, werden Benutzereinstellungen und Konfigurationsoptionen nicht übernommen. Darüber hinaus müssen vorhandene Azure MFA-Server unter Verwendung von Aktivierungsanmeldeinformationen des MFA-Anbieters erneut aktiviert werden. Wenn Sie die MFA-Server erneut aktivieren, um sie mit dem MFA-Anbieter zu verknüpfen, hat das keinerlei Auswirkungen auf die Authentifizierung per Telefonanruf oder SMS, aber Benachrichtigungen der mobilen App funktionieren erst wieder, wenn die Benutzer die mobile App erneut aktivieren.

### <a name="removing-an-authentication-provider"></a>Entfernen eines Authentifizierungsanbieters

> [!CAUTION]
> Beim Löschen eines Authentifizierungsanbieters wird keine Bestätigung angezeigt. **Löschen** ist ein endgültiger Prozess.

Authentifizierungsanbieter finden Sie hier: **Azure-Portal** > **Azure Active Directory** > **MFA** > **Anbieter**. Klicken Sie auf einen aufgeführten Anbieter, um die zugehörigen Details und Konfigurationen anzuzeigen.

Überprüfen Sie vor dem Entfernen eines Authentifizierungsanbieters zunächst alle benutzerdefinierten Einstellungen, die ggf. für Ihren Anbieter konfiguriert wurden. Überlegen Sie sich, welche Einstellungen von Ihrem Anbieter zu allgemeinen MFA-Einstellungen migriert werden müssen, und migrieren Sie sie. 

Mit Anbietern verknüpfte MFA-Server müssen unter Verwendung der Anmeldeinformationen reaktiviert werden, die unter **Azure-Portal** > **Azure Active Directory** > **MFA** > **Servereinstellungen** generiert wurden. Vor der Reaktivierung müssen auf Azure MFA-Servern in Ihrer Umgebung folgende Dateien aus dem Verzeichnis `\Program Files\Multi-Factor Authentication Server\Data\` gelöscht werden:

- caCert
- cert
- groupCACert
- groupKey
- groupName
- licenseKey
- pkey

![Löschen eines Authentifizierungsanbieters über das Azure-Portal](./media/concept-mfa-authprovider/authentication-provider-removal.png)

Wenn Sie sich vergewissert haben, dass alle Einstellungen migriert wurden, können Sie unter **Azure-Portal** > **Azure Active Directory** > **MFA** > **Anbieter** die Auslassungspunkte **...** und anschließend **Löschen** auswählen.

> [!WARNING]
> Beim Löschen eines Authentifizierungsanbieters werden alle diesem Anbieter zugeordneten Berichtsinformationen gelöscht. Es empfiehlt sich gegebenenfalls, Aktivitätsberichte vor dem Löschen des Anbieters zu speichern.

> [!NOTE]
> Benutzer mit älteren Versionen von Microsoft Authenticator-App und Azure MFA-Server müssen ihre App möglicherweise neu registrieren.

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren von Azure Multi-Factor Authentication-Einstellungen](howto-mfa-mfasettings.md)

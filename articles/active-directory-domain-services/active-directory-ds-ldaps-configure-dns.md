---
title: Konfigurieren des DNS für den Zugriff auf eine verwaltete Domäne per LDAPS über das Internet | Microsoft-Dokumentation
description: Konfigurieren des DNS für den Zugriff auf eine verwaltete Azure Active Directory Domain Services-Domäne per LDAPS über das Internet
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: a47f0f3e-2578-422a-a421-034f66de38f5
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: iainfou
ms.openlocfilehash: 1c5a119c686bc879b11b8fa87a9c81ba36b0beb4
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2019
ms.locfileid: "67474023"
---
# <a name="configure-dns-to-access-an-azure-ad-domain-services-managed-domain-using-secure-ldap-ldaps"></a>Konfigurieren des DNS für den Zugriff auf eine verwaltete Azure Active Directory Domain Services-Domäne per Secure LDAP (LDAPS)

## <a name="before-you-begin"></a>Voraussetzungen
Führen Sie [Aufgabe 3: Aktivieren von sicherem LDAP für die verwaltete Domäne über das Azure-Portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md) durch.

## <a name="task-4-configure-dns-to-access-the-managed-domain-from-the-internet"></a>Aufgabe 4: Konfigurieren von DNS für den Zugriff auf die verwaltete Domäne über das Internet
> [!TIP]
> **Optionale Aufgabe** : Wenn Sie nicht über eine Internetverbindung mit LDAPS auf die verwaltete Domäne zugreifen möchten, überspringen Sie diese Konfigurationsaufgabe.
>
>

Vor dem Ausführen dieser Aufgabe führen Sie die in [Aufgabe 3](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md) beschriebenen Schritte aus.

Nachdem Sie den Zugriff per Secure LDAP über das Internet aktiviert haben, müssen Sie das DNS aktualisieren, damit die Clientcomputer die verwaltete Domäne finden können. Unter **EXTERNE IP-ADRESSE FÜR LDAPS-ZUGRIFF** wird auf der Registerkarte **Eigenschaften** eine externe IP-Adresse angezeigt.

Konfigurieren Sie Ihren externen DNS-Anbieter so, dass der DNS-Name der verwalteten Domäne (beispielsweise „ldaps.contoso100.com“) auf diese externe IP-Adresse verweist. Erstellen Sie beispielsweise den folgenden DNS-Eintrag:

    ldaps.contoso100.com  -> 52.165.38.113

Das ist alles! Sie können jetzt über das Internet und sicheres LDAP eine Verbindung mit der verwalteten Domäne herstellen.

> [!WARNING]
> Denken Sie daran, dass die Clientcomputer dem Aussteller des Zertifikats für LDAPS vertrauen müssen, damit die Verbindung mit der verwalteten Domäne über LDAPS erfolgreich hergestellt werden kann. Wenn Sie eine für die Öffentlichkeit vertrauenswürdige Zertifizierungsstelle verwenden, ist kein weiterer Schritt erforderlich, da Clientcomputer diesen Zertifikatausstellern vertrauen. Wenn Sie ein selbstsigniertes Zertifikat verwenden, müssen Sie den öffentlichen Teil des selbstsignierten Zertifikats im vertrauenswürdigen Zertifikatpeicher auf den Clientcomputern installieren.
>
>

## <a name="next-step"></a>Nächster Schritt
[Aufgabe 5: Binden an die verwaltete Domäne und Sperren des Secure LDAP-Zugriffs](active-directory-ds-ldaps-bind-lockdown.md)

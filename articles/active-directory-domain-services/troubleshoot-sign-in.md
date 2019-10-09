---
title: Troubleshooting für Probleme bei der Anmeldung in Azure AD Domain Services | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie häufige Probleme und Fehler bei der Benutzeranmeldung in Azure Active Directory Domain Services beheben.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: troubleshooting
ms.date: 10/02/2019
ms.author: iainfou
ms.openlocfilehash: aa03e388019bf696324ea7af6062ec98386df5fa
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71827046"
---
# <a name="troubleshoot-account-sign-in-problems-with-an-azure-ad-domain-services-managed-domain"></a>Troubleshooting für Probleme bei der Anmeldung bei einer mit Azure AD Domain Services verwalteten Domäne

Die häufigsten Gründe, warum ein Benutzerkonto, sich nicht bei einer in Azure AD DS verwalteten Domäne anmelden kann, sind die folgenden Szenarien:

* [Das Konto ist noch nicht mit Azure AD DS synchronisiert.](#account-isnt-synchronized-into-azure-ad-ds-yet)
* [Azure AD DS verfügt nicht über die zur Anmeldung des Kontos nötigen Kennworthashes.](#azure-ad-ds-doesnt-have-the-password-hashes)
* [Das Konto ist gesperrt.](#the-account-is-locked-out)

> [!TIP]
> Azure AD DS kann die Anmeldeinformationen für Konten außerhalb des Azure AD-Mandanten nicht synchronisieren. Externe Benutzer können sich nicht bei der Azure AD DS-verwalteten Domäne anmelden.

## <a name="account-isnt-synchronized-into-azure-ad-ds-yet"></a>Das Konto ist noch nicht mit Azure AD DS synchronisiert

Je nach Größe Ihres Verzeichnisses kann es einige Zeit dauern, bis die Benutzerkonten und Anmeldeinformationshashes in Azure AD DS verfügbar sind. Bei großen Verzeichnissen kann diese anfängliche unidirektionale Synchronisierung von Azure AD einige Stunden und bis zu einem oder zwei Tagen dauern. Achten Sie darauf, dass Sie lange genug warten, bevor Sie erneut versuchen, die Authentifizierung durchzuführen.

Bei Hybridumgebungen, die Azure AD Connect verwenden, um lokale Verzeichnisdaten nach Azure AD zu synchronisieren, sollten Sie sichergehen, dass Sie die neuste Version von Azure AD Connect ausführen und dass [Azure AD Connect so konfiguriert ist, dass nach der Aktivierung von Azure AD DS eine vollständige Synchronisierung durchgeführt wird][azure-ad-connect-phs]. Wenn Sie Azure AD DS deaktivieren und dann erneut aktivieren, müssen Sie die folgenden Schritte ausführen.

Wenn weiterhin Probleme mit Konten auftreten, die nicht durch Azure AD Connect synchronisiert werden, starten Sie den Azure AD Sync Service neu. Öffnen Sie auf dem Computer, auf dem Azure AD Connect installiert ist, ein Eingabeaufforderungsfenster und führen Sie folgende Befehle aus:

```console
net stop 'Microsoft Azure AD Sync'
net start 'Microsoft Azure AD Sync'
```

## <a name="azure-ad-ds-doesnt-have-the-password-hashes"></a>Azure AD DS hat keine Kennworthashes

Azure AD generiert oder speichert erst dann Kennworthashes in dem für die NTLM- oder Kerberos-Authentifizierung erforderlichen Format, wenn Sie Azure AD DS für Ihren Mandanten aktivieren. Aus Sicherheitsgründen speichert Azure AD Kennwörter nicht als Klartext. Daher kann Azure AD diese NTLM- oder Kerberos-Kennworthashes nicht automatisch auf der Grundlage bereits vorhandener Anmeldeinformationen von Benutzern generieren.

### <a name="hybrid-environments-with-on-premises-synchronization"></a>Hybridumgebungen mit lokaler Synchronisierung

Für Hybridumgebungen, in denen Azure AD Connect für die Synchronisierung von einer lokalen AD DS-Umgebung verwendet wird, können Sie die erforderlichen NTLM- oder Kerberos-Kennworthashes lokal in Azure AD generieren und synchronisieren. Nachdem Sie Ihre mit Azure AD DC verwaltete Domäne erstellt haben, [aktivieren sie die Kennworthashsynchronisierung zu Azure Active Directory Domain Services][azure-ad-connect-phs]. Ohne diesen Schritt zur Kennworthashsynchronisierung können Sie sich nicht mit Azure AD DS bei einem Konto anmelden. Wenn Sie Azure AD DS deaktivieren und dann erneut aktivieren, müssen Sie die Schritte erneut ausführen.

Weitere Informationen finden Sie im Artikel [Kennworthashsynchronisierung für Azure AD DS][phs-process].

### <a name="cloud-only-environments-with-no-on-premises-synchronization"></a>Cloud-Umgebungen ohne lokale Synchronisierung

Mit Azure AD DS verwaltete Domänen ohne lokale Synchronisierung, nur mit Konten in Azure AD, müssen auch die erforderlichen NTLM- oder Kerberos-Kennworthashes generieren. Wenn ein Nur-Cloud-Konto sich nicht anmelden kann, wurde nach der Aktivierung von Azure AD DS eine erfolgreiche Kennwortänderung durchgeführt?

* **Nein, das Kennwort wurde nicht geändert.**
    * [Ändern Sie das Kennwort für das Konto][enable-user-accounts], um die erforderlichen Kennworthashes zu generieren, und warten Sie 15 Minuten, bevor Sie versuchen, sich erneut anzumelden.
    * Wenn Sie Azure AD DS deaktivieren und dann erneut aktivieren, muss jedes Konto die Schritte erneut ausführen, um sein Kennwort zu ändern und die erforderlichen Kennworthashes zu generieren.
* **Ja, das Kennwort wurde geändert.**
    * Versuchen Sie, sich mit dem Format *UPN* anzumelden, z. B. `driley@contoso.com`, anstelle des Formats *SAMAccountName*, z. B. `CONTOSO\deeriley`.
    * Der *SAMAccountName* wird möglicherweise automatisch für Benutzer generiert, deren UPN-Präfix übermäßig lang ist oder deren Präfix mit dem eines anderen Benutzers in der verwalteten Domäne übereinstimmt. Das Format *UPN* ist innerhalb eines Azure AD-Mandanten garantiert eindeutig.

## <a name="the-account-is-locked-out"></a>Das Konto ist gesperrt

Benutzerkonten werden in Azure AD DS gesperrt, wenn ein festgelegter Schwellenwert für fehlgeschlagene Anmeldeversuche erreicht wurde. Das Verhalten bei Kontosperrungen ist so konzipiert, dass Sie vor wiederholt versuchten Brute-Force-Anmeldungen geschützt werden, die auf einen automatisierten digitalen Angriff hinweisen können.

Wenn in 2 Minuten 5 Versuche mit ungültigen Kennwörtern vorliegen, wird das Konto standardmäßig für 30 Minuten gesperrt.

Weitere Informationen zum Beheben von Problemen bezüglich Kontosperrungen finden Sie unter [Troubleshooting für Probleme mit Kontosperrungen in Azure AD DS][troubleshoot-account-lockout].

## <a name="next-steps"></a>Nächste Schritte

Sollten beim Verknüpfen Ihres VM mit der durch Azure AD DS verwalteten Domäne weitere Probleme auftreten, [öffnen Sie ein Supportticket für Azure Active Directory][azure-ad-support].

<!-- INTERNAL LINKS -->
[troubleshoot-account-lockout]: troubleshoot-account-lockout.md
[azure-ad-connect-phs]: active-directory-ds-getting-started-password-sync-synced-tenant.md
[enable-user-accounts]:  tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds
[phs-process]: ../active-directory/hybrid/how-to-connect-password-hash-synchronization.md#password-hash-sync-process-for-azure-ad-domain-services
[azure-ad-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md

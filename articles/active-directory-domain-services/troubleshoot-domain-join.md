---
title: Problembehandlung beim Einbinden einer Domäne mit Azure AD Domain Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie häufige Probleme beheben können, wenn Sie versuchen, eine VM in eine Domäne einzubinden oder eine Anwendung mit Azure Active Directory Domain Services zu verbinden, und keine Verbindung mit der verwalteten Domäne herstellen oder sich dort nicht authentifizieren können.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: troubleshooting
ms.date: 10/02/2019
ms.author: iainfou
ms.openlocfilehash: 1016fbc1478ec713d50a2f04bcc80d08288b03f3
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71827235"
---
# <a name="troubleshoot-domain-join-problems-with-an-azure-ad-domain-services-managed-domain"></a>Behandeln von Problemen beim Einbinden in eine von Azure AD Domain Services verwaltete Domäne

Wenn Sie versuchen, einen virtuellen Computer (VM) in eine von Azure Active Directory Domain Services (AD DS) verwaltete Domäne einzubinden oder eine Anwendung mit dieser Domäne zu verbinden, wird eventuell eine Fehlermeldung angezeigt, dass dies nicht möglich ist. Informationen zur Behandlung von Problemen beim Einbinden in die Domäne finden Sie unter den folgenden Punkten:

* Wenn Sie keine Authentifizierungsaufforderung erhalten, kann die VM oder Anwendung keine Verbindung mit der von Azure AD DS verwalteten Domäne herstellen.
    * Beginnen Sie mit der Behandlung von [Konnektivitätsproblemen beim Einbinden in die Domäne](#connectivity-issues-for-domain-join).
* Wenn während der Authentifizierung ein Fehler auftritt, wurde die Verbindung mit der von Azure AD DS verwalteten Domäne erfolgreich hergestellt.
    * Beginnen Sie mit der Behandlung von [Problemen mit Anmeldeinformationen während des Einbindens in die Domäne](#credentials-related-issues-during-domain-join).

## <a name="connectivity-issues-for-domain-join"></a>Konnektivitätsprobleme beim Einbinden in die Domäne

Wenn die VM die von Azure AD DS verwaltete Domäne nicht finden kann, liegt normalerweise ein Netzwerkverbindungs- oder Konfigurationsproblem vor. Überprüfen Sie die folgenden Schritte zur Problembehandlung, um das Problem zu finden und zu beheben:

1. Stellen Sie sicher, dass die VM mit demselben virtuellen Netzwerk (oder einem virtuellen Peernetzwerk) verbunden ist, das für Azure AD DS aktiviert wurde. Andernfalls kann die VM die Domäne nicht finden und eine Verbindung mit ihr herstellen, um sich darin einzubinden.
    * Wenn die VM nicht mit demselben virtuellen Netzwerk verbunden ist, vergewissern Sie sich, dass der Status für das Peering virtueller Netzwerke oder die VPN-Verbindung *Aktiv* oder *Verbunden* lautet, damit der Datenverkehr ordnungsgemäß weitergeleitet werden kann.
1. Versuchen Sie, die Domäne unter Angabe des Domänennamens der von Azure AD DS verwaltete Domäne, z.B. `ping contoso.com`, zu pingen.
    * Wenn die Ping-Antwort fehlschlägt, versuchen Sie, die IP-Adressen für die Domäne zu pingen, die auf der Übersichtsseite im Portal für Ihre von Azure AD DS verwaltete Domäne angezeigt werden, z.B. `ping 10.0.0.4`.
    * Wenn Sie die IP-Adresse erfolgreich pingen können, aber nicht die Domäne, ist der DNS möglicherweise falsch konfiguriert. Stellen Sie sicher, dass Sie die DNS-Server der von Azure AD DS verwalteten Domäne für das virtuelle Netzwerk konfiguriert haben.
1. Versuchen Sie, den DNS-Resolvercache auf dem virtuellen Computer zu leeren, z.B. `ipconfig /flushdns`.

### <a name="network-security-group-nsg-configuration"></a>Konfiguration der Netzwerksicherheitsgruppe (NSG)

Wenn Sie eine von Azure AD DS verwaltete Domäne erstellen, wird auch eine Netzwerksicherheitsgruppe mit den entsprechenden Regeln für einen erfolgreichen Domänenvorgang erstellt. Wenn Sie zusätzliche Netzwerksicherheitsgruppen-Regeln bearbeiten oder erstellen, können Sie versehentlich Ports blockieren, die für die Bereitstellung von Verbindungs- und Authentifizierungsdiensten von Azure AD DS benötigt werden. Diese Netzwerksicherheitsgruppen-Regeln können Probleme verursachen – z.B., die Kennwortsynchronisierung wird nicht abgeschlossen, Benutzer können sich nicht anmelden, oder es gibt Probleme beim Einbinden in die Domäne.

Wenn weiterhin Verbindungsprobleme auftreten, überprüfen Sie die folgenden Schritte zur Problembehandlung:

1. Überprüfen Sie den Integritätsstatus Ihrer von Azure AD DS verwalteten Domäne im Azure-Portal. Wenn eine Warnung für *AADDS001* angezeigt wird, blockiert eine Netzwerksicherheitsgruppen-Regel den Zugriff.
1. Überprüfen Sie die [erforderlichen Ports und Netzwerksicherheitsgruppen-Regeln][network-ports]. Stellen Sie sicher, dass Netzwerksicherheitsgruppen-Regeln, die auf die VM oder das virtuelle Netzwerk angewendet werden, von dem aus Sie eine Verbindung herstellen, diese Netzwerkports nicht blockieren.
1. Nachdem Konfigurationsprobleme bei der Netzwerksicherheitsgruppe behoben wurden, wird die Warnung *AADDS001* auf der Seite „Integrität“ in ungefähr 2 Stunden nicht mehr angezeigt. Wenn die Netzwerkkonnektivität jetzt verfügbar ist, versuchen Sie, die VM erneut in die Domäne einzubinden.

## <a name="credentials-related-issues-during-domain-join"></a>Probleme mit Anmeldeinformationen während des Einbindens in die Domäne

Wenn ein Dialogfeld angezeigt wird, das Anmeldeinformationen zum Einbinden in die von Azure AD DS verwaltete Domäne anfordert, kann die VM eine Verbindung mit der Domäne über das virtuelle Azure-Netzwerk herstellen. Der Prozess des Einbindens in die Domäne schlägt beim Authentifizieren bei der Domäne oder der Autorisierung fehl, sodass dieser Prozess unter Verwendung der von Ihnen bereitgestellten Anmeldeinformationen nicht abgeschlossen werden kann.

Sehen Sie sich die folgenden Schritte zur Problembehandlung an, um Probleme im Zusammenhang mit Anmeldeinformationen zu beheben:

1. Geben Sie die Anmeldeinformationen im UPN-Format an, z. B. als `dee@contoso.onmicrosoft.com`. Stellen Sie sicher, dass dieser UPN in Azure AD ordnungsgemäß konfiguriert wird.
    * Der *SAMAccountName* für Ihr Konto wird möglicherweise automatisch generiert, wenn mehrere Benutzer in Ihrem Mandanten das gleiche UPN-Präfix verwenden oder wenn Ihr UPN-Präfix übermäßig lang ist. Das Format *SAMAccountName* für Ihr Konto ist möglicherweise anders als Sie erwarten bzw. unterscheidet sich von dem, was Sie in Ihrer lokalen Domäne verwenden.
1. Verwenden Sie die Anmeldeinformationen für ein Benutzerkonto, das zur Gruppe *AAD DC-Administratoren* gehört, um VMs in die von Azure AD DS verwaltete Domäne einzubinden.
1. Stellen Sie sicher, dass Sie die [Kennwortsynchronisierung aktiviert][enable-password-sync] und so lange gewartet haben, bis die erste Kennwortsynchronisierung abgeschlossen war.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie sich über die Active Directory-Prozesse im Rahmen des Vorgangs zum Einbinden in eine Domäne genauer informieren möchten, lesen Sie [Probleme beim Einbinden und bei der Authentifizierung][join-authentication-issues].

Sollten beim Einbinden Ihrer VM in die von Azure AD DS verwaltete Domäne weiterhin Probleme auftreten, [suchen Sie Hilfe, und öffnen Sie ein Supportticket für Azure Active Directory][azure-ad-support].

<!-- INTERNAL LINKS -->
[enable-password-sync]: tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds
[network-ports]: network-considerations.md#network-security-groups-and-required-ports
[azure-ad-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md

<!-- EXTERNAL LINKS -->
[join-authentication-issues]: /previous-versions/windows/it-pro/windows-2000-server/cc961817(v=technet.10)

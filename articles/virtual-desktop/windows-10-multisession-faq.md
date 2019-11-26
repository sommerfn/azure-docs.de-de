---
title: 'Häufig gestellte Fragen zu Windows 10 Enterprise mit mehreren Sitzungen: Azure'
description: Enthält die häufig gestellten Fragen und bewährten Methoden für die Verwendung von Windows 10 Enterprise mit mehreren Sitzungen für Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 08/28/2019
ms.author: helohr
ms.openlocfilehash: f1ba54547b947e18d2d42520c0fb51a0855fb37c
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72901608"
---
# <a name="windows-10-enterprise-multi-session-faq"></a>Häufig gestellte Fragen zu Windows 10 Enterprise mit mehreren Sitzungen

In diesem Artikel werden häufig gestellte Fragen beantwortet, und Sie erhalten Informationen zu den bewährten Methoden für Windows 10 Enterprise mit mehreren Sitzungen.
 
## <a name="what-is-windows-10-enterprise-multi-session"></a>Was ist Windows 10 Enterprise (mehrere Sitzungen)? 

Windows 10 Enterprise (mehrere Sitzungen) (bisher als Windows 10 Enterprise for Virtual Desktops (EVD) bezeichnet) ist ein neuer Remotedesktop-Sitzungshost, der mehrere gleichzeitige interaktive Sitzungen ermöglicht. Zuvor war dies nur mit Windows Server möglich. Mit dieser Funktion erhalten Benutzer eine vertraute Windows 10-Benutzeroberfläche. Die IT-Abteilung kann von den Kostenvorteilen profitieren, die sich durch die Ausführung mehrerer Sitzungen ergeben, und anstelle von RDS-Clientzugriffslizenzen (Client Access Licenses, CALs) die vorhandene Windows-Lizenzierung pro Benutzer verwenden. Weitere Informationen zu Lizenzen und Preisen finden Sie unter [Windows Virtual Desktop – Preise](https://azure.microsoft.com/pricing/details/virtual-desktop/). 
 
## <a name="how-many-users-can-simultaneously-have-an-interactive-session-on-windows-10-enterprise-multi-session"></a>Wie viele Benutzer können unter Windows 10 Enterprise (mehrere Sitzungen) gleichzeitig eine interaktive Sitzung ausführen?

Wie viele interaktive Sitzungen gleichzeitig aktiv sein können, hängt von den Hardwareressourcen Ihres Systems (vCPU, Arbeitsspeicher, Datenträger und vGPU), der Verwendung der Apps durch Benutzer während der Sitzungsanmeldung und der Workload Ihres Systems ab. Wir empfehlen Ihnen, die Leistung Ihres Systems zu überprüfen, um zu ermitteln, wie viele Benutzer unter Windows 10 Enterprise (mehrere Sitzungen) möglich sind. Weitere Informationen finden Sie unter [Windows Virtual Desktop – Preise](https://azure.microsoft.com/pricing/details/virtual-desktop/). 
 
## <a name="why-does-my-application-report-windows-10-enterprise-multi-session-as-a-server-operating-system"></a>Warum meldet meine Anwendung Windows 10 Enterprise (mehrere Sitzungen) als Serverbetriebssystem?

Windows 10 Enterprise (mehrere Sitzungen) ist eine virtuelle Edition von Windows 10 Enterprise. Einer der Unterschiede ist, dass dieses Betriebssystem für [ProductType](https://docs.microsoft.com/windows/desktop/cimwin32prov/win32-operatingsystem) den Wert „3“ meldet. Dies ist der gleiche Wert wie für Windows Server. Mit dieser Eigenschaft wird die Kompatibilität des Betriebssystems mit vorhandenen RDSH-Verwaltungstools, RDSH-Multisession-fähigen Anwendungen und vorwiegend auf Low-Level-Basis vorgenommenen Systemleistungsoptimierungen für RDSH-Umgebungen sichergestellt. Bei einigen Anwendungsinstallationsprogrammen kann es sein, dass die Installation unter Windows 10 Enterprise (mehrere Sitzungen) blockiert wird. Dies hängt davon ab, ob erkannt wird, dass „ProductType“ auf „Client“ festgelegt ist. Wenn Ihre App nicht installiert werden kann, sollten Sie sich an den Anbieter Ihrer Anwendung wenden, um eine aktualisierte Version zu erhalten. 
 
## <a name="can-i-run-windows-10-enterprise-multi-session-on-premises"></a>Kann ich Windows 10 Enterprise (mehrere Sitzungen) lokal ausführen?

Windows 10 Enterprise (mehrere Sitzungen) kann nicht in lokalen Produktionsumgebungen ausgeführt werden, weil die Anwendung für den Dienst Windows Virtual Desktop für Azure optimiert ist. Es ist ein Verstoß gegen den Lizenzvertrag, Windows 10 Enterprise (mehrere Sitzungen) außerhalb von Azure zu Produktionszwecken auszuführen. Windows 10 Enterprise (mehrere Sitzungen) kann für lokale Schlüsselverwaltungsdienste (Key Management Services, KMS) nicht aktiviert werden.
 
## <a name="how-do-i-customize-the-windows-10-enterprise-multi-session-image-for-my-organization"></a>Wie kann ich das Image von Windows 10 Enterprise (mehrere Sitzungen) für meine Organisation anpassen?

Sie können in Azure einen virtuellen Computer (VM) mit Windows 10 Enterprise (mehrere Sitzungen) starten und anpassen, indem Sie Branchenanwendungen installieren, Sysprep und die Generalisierung ausführen und dann über das Azure-Portal ein Image erstellen.  
 
Erstellen Sie zunächst in Azure mit Windows 10 Enterprise (mehrere Sitzungen) einen virtuellen Computer. Anstatt die VM in Azure zu starten, können Sie die VHD direkt herunterladen. Anschließend können Sie die heruntergeladene VHD verwenden, um auf einem Windows 10-PC eine neue VM der ersten Generation mit Hyper-V-Aktivierung zu erstellen.

Passen Sie das Image an Ihre Anforderungen an, indem Sie Branchenanwendungen installieren und Sysprep für das Image ausführen. Laden Sie das Image nach erfolgter Anpassung mit der enthaltenen VHD in Azure hoch. Beschaffen Sie anschließend Windows Virtual Desktop über den Azure Marketplace, und nutzen Sie den Dienst, um einen neuen Hostpool mit dem angepassten Image bereitzustellen.
 
## <a name="how-do-i-manage-windows-10-enterprise-multi-session-after-deployment"></a>Wie verwalte ich Windows 10 Enterprise (mehrere Sitzungen) nach der Bereitstellung?

Sie können ein beliebiges unterstütztes Konfigurationstool verwenden. Wir empfehlen Ihnen aber, System Center Configuration Manager 1906 zu nutzen, weil diese Anwendung über Unterstützung für Windows 10 Enterprise (mehrere Sitzungen) verfügt. Wir arbeiten derzeit an der Unterstützung von Microsoft Intune.
 
## <a name="can-windows-10-enterprise-multi-session-be-azure-active-directory-ad-joined"></a>Kann Windows 10 Enterprise (mehrere Sitzungen) in Azure AD (Azure Active Directory) eingebunden werden?

Für Windows 10 Enterprise (mehrere Sitzungen) wird derzeit die Hybrideinbindung in Azure AD unterstützt. Nachdem Windows 10 Enterprise (mehrere Sitzungen) in die Domäne eingebunden wurde, können Sie das vorhandene Gruppenrichtlinienobjekt verwenden, um die Azure AD-Registrierung zu aktivieren. Weitere Informationen finden Sie unter [Planen der Implementierung einer Azure Active Directory-Hybrideinbindung](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-plan).
 
## <a name="where-can-i-find-the-windows-10-enterprise-multi-session-image"></a>Wo finde ich das Image für Windows 10 Enterprise (mehrere Sitzungen)?

Windows 10 Enterprise (mehrere Sitzungen) ist über den Azure-Katalog erhältlich. Navigieren Sie zum Azure-Portal, und suchen Sie nach dem Release „Windows 10 Enterprise for Virtual Desktops“. Ein in Office Pro Plus integriertes Image erhalten Sie, indem Sie im Azure-Portal nach „Microsoft Windows 10 + Office 365 ProPlus“ suchen.

## <a name="which-windows-10-enterprise-multi-session-image-should-i-use"></a>Welches Image für Windows 10 Enterprise (mehrere Sitzungen) sollte ich verwenden?

Der Azure-Katalog enthält mehrere Releases, z. B. Version 1809 und Version 1903 von Windows 10 Enterprise (mehrere Sitzungen). Wir empfehlen Ihnen die Verwendung der neuesten Version, um eine höhere Leistung und Zuverlässigkeit zu erzielen.
 
## <a name="which-windows-10-enterprise-multi-session-versions-are-supported"></a>Welche Versionen von Windows 10 Enterprise (mehrere Sitzungen) werden unterstützt?

Version 1809 und höher von Windows 10 Enterprise (mehrere Sitzungen) werden unterstützt und sind im Azure-Katalog verfügbar. Für diese Releases wird der gleiche Supportrichtlinienansatz wie für Windows 10 Enterprise verfolgt. Dies bedeutet, dass für das Frühlingsrelease 18 Monate und für das Herbstrelease 30 Monate lang Support geleistet wird.
 
## <a name="which-profile-management-solution-should-i-use-for-windows-10-enterprise-multi-session"></a>Welche Profilverwaltungslösung sollte ich für Windows 10 Enterprise (mehrere Sitzungen) verwenden?

Wir empfehlen Ihnen die Verwendung von FSLogix-Profilcontainern, wenn Sie Windows 10 Enterprise in nicht persistenten Umgebungen oder anderen Szenarien konfigurieren, für die ein zentral gespeichertes Profil benötigt wird. Mit FSLogix wird sichergestellt, dass das Benutzerprofil für jede Benutzersitzung verfügbar und aktuell ist. Darüber hinaus empfehlen wir Ihnen die Verwendung Ihres FSLogix-Profilcontainers zum Speichern eines Benutzerprofils auf einer SMB-Freigabe mit den passenden Berechtigungen. Sie können Benutzerprofile bei Bedarf aber auch im Azure-Seitenblobspeicher speichern. Benutzer von Windows Virtual Desktop können FSLogix nutzen, ohne dass zusätzliche Kosten anfallen.
 
Weitere Informationen zum Konfigurieren eines FSLogix-Profilcontainers finden Sie unter [Konfigurieren des FSLogix-Profilcontainers](create-host-pools-user-profile.md#configure-the-fslogix-profile-container).  

## <a name="which-license-do-i-need-to-access-windows-10-enterprise-multi-session"></a>Welche Lizenz benötige ich für den Zugriff auf Windows 10 Enterprise (mehrere Sitzungen)?

Eine vollständige Liste mit den passenden Lizenzen finden Sie unter [Windows Virtual Desktop – Preise](https://azure.microsoft.com/pricing/details/virtual-desktop/).
 
## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Windows Virtual Desktop und Windows 10 Enterprise (mehrere Sitzungen):

- Lesen Sie die [Dokumentation zu Windows Virtual Desktop (Vorschauversion)](overview.md).
- Besuchen Sie unsere [Windows Virtual Desktop Tech Community](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop).
- Richten Sie Ihre Windows Virtual Desktop-Bereitstellung ein, indem Sie die [Tutorials zu Windows Virtual Desktop](tenant-setup-azure-active-directory.md) verwenden.

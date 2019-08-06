---
title: 'Azure Active Directory: Häufig gestellte Fragen zur Geräteverwaltung | Microsoft-Dokumentation'
description: 'Azure Active Directory: Häufig gestellte Fragen zur Geräteverwaltung.'
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: troubleshooting
ms.date: 06/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: ravenn
ms.collection: M365-identity-device-management
ms.openlocfilehash: fbba3f1b753738de57aa311387e522bae1b7b523
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2019
ms.locfileid: "68499792"
---
# <a name="azure-active-directory-device-management-faq"></a>Azure Active Directory: Häufig gestellte Fragen zur Geräteverwaltung

### <a name="q-i-registered-the-device-recently-why-cant-i-see-the-device-under-my-user-info-in-the-azure-portal-or-why-is-the-device-owner-marked-as-na-for-hybrid-azure-active-directory-azure-ad-joined-devices"></a>F: Ich habe das Gerät vor Kurzem registriert. Warum kann ich das Gerät nicht in meinen Benutzerinformationen im Azure-Portal sehen? Oder warum ist der Gerätebesitzer für in Azure Active Directory (Azure AD) eingebundene Hybridgeräte als „N/V“ markiert?

**A:** Windows 10-Geräte, die in Azure AD eingebundene Hybridgeräte sind, werden nicht unter den **BENUTZER-Geräten** angezeigt.
Verwenden Sie die Ansicht **Alle Geräte** im Azure-Portal. Sie können auch das PowerShell-Cmdlet [Get-MsolDevice](https://docs.microsoft.com/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) verwenden.

Nur die folgenden Geräte werden unter den **BENUTZER-Geräten** aufgeführt:

- Alle persönlichen Geräte, die keine in Azure AD eingebundenen Hybridgeräte sind. 
- Alle Geräte, die keine Windows 10- oder Windows Server 2016-Geräte sind.
- Alle Geräte ohne Windows. 

---

### <a name="q-how-do-i-know-what-the-device-registration-state-of-the-client-is"></a>F: Wie ermittle ich den Geräteregistrierungsstatus des Clients?

**A:** Navigieren Sie im Azure-Portal zu **Alle Geräte**. Suchen Sie für das Gerät anhand der Geräte-ID. Überprüfen Sie den Wert in der Spalte „Jointyp“. Möglicherweise wurde das Gerät zurückgesetzt oder ein Reimaging durchgeführt. Daher ist es wichtig, den Geräteregistrierungsstatus auf dem Gerät zu überprüfen:

- Führen Sie für Geräte mit Windows 10, Windows Server 2016 oder höher `dsregcmd.exe /status` aus.
- Führen Sie für kompatible Betriebssystemversionen `%programFiles%\Microsoft Workplace Join\autoworkplace.exe` aus.

---

### <a name="q-i-see-the-device-record-under-the-user-info-in-the-azure-portal-and-i-see-the-state-as-registered-on-the-device-am-i-set-up-correctly-to-use-conditional-access"></a>F: Der Gerätedatensatz wird in meinen Benutzerinformationen im Azure-Portal angezeigt. Und ich sehe den Status als auf dem Gerät registriert. Sind diese Einstellungen für den bedingten Zugriff richtig?

**A:** Der Verknüpfungsstatus des Geräts, der in **„deviceID“** angezeigt wird, muss mit dem Status in Azure AD übereinstimmen und alle Bewertungskriterien für bedingten Zugriff erfüllen. Weitere Informationen finden Sie unter [„Vorschreiben der Verwendung verwalteter Geräte für den Zugriff auf Cloud-Apps mithilfe des bedingten Zugriffs“](../conditional-access/require-managed-devices.md).

---

### <a name="q-why-do-my-users-see-an-error-message-saying-your-organization-has-deleted-the-device-or-your-organization-has-disabled-the-device-on-their-windows-10-devices-"></a>F: Warum sehen meine Benutzer auf ihren Windows 10-Geräten eine Fehlermeldung, die besagt, dass die Organisation das Gerät gelöscht oder deaktiviert hat?

**A:** Benutzer erhalten auf Windows 10-Geräten, die in Azure AD eingebunden oder registriert sind, ein [Primäres Aktualisierungstoken](concept-primary-refresh-token.md) (Primary Refresh Token, PRT), das das einmalige Anmelden ermöglicht. Die Gültigkeit des PRT basiert auf der Gültigkeit des Geräts selbst. Benutzern wird diese Meldung angezeigt, wenn das Gerät in Azure AD entweder gelöscht oder deaktiviert wurde, ohne dass die Aktion vom Gerät selbst initiiert wurde. Ein Gerät kann in einem der folgenden Szenarien in Azure AD gelöscht oder deaktiviert werden: 

- Der Benutzer deaktiviert das Gerät im Meine Apps-Portal. 
- Ein Administrator (oder Benutzer) löscht oder deaktiviert das Gerät im Azure-Portal oder über PowerShell.
- Nur in Azure AD Hybrid eingebundene Geräte: Ein Administrator entfernt die Geräte-OE aus dem Synchronisierungsbereich, was dazu führt, dass die Geräte aus Azure AD gelöscht werden.

Weiter unten finden Sie Informationen dazu, wie diese Aktionen korrigiert werden können.

---

### <a name="q-i-disabled-or-deleted-my-device-in-the-azure-portal-or-by-using-windows-powershell-but-the-local-state-on-the-device-says-its-still-registered-what-should-i-do"></a>F: Ich habe mein Gerät im Azure-Portal oder mithilfe von Windows PowerShell deaktiviert oder gelöscht. Laut lokalem Status auf dem Gerät ist es aber immer noch registriert. Wie sollte ich vorgehen?

**A:** Dieser Vorgang ist von vornherein vorgesehen. In diesem Fall hat das Gerät keinen Zugriff auf Ressourcen in der Cloud. Administratoren können diese Aktion für veraltete, verlorene oder gestohlene Geräte ausführen, um nicht autorisierten Zugriff zu verhindern. Wenn diese Aktion unbeabsichtigt ausgeführt wurde, müssen Sie das Gerät erneut aktivieren oder registrieren, wie unten beschrieben.

- Wenn das Gerät in Azure AD deaktiviert wurde, kann ein Administrator mit ausreichenden Berechtigungen es über das Azure AD-Portal wieder aktivieren.  

 - Wenn das Gerät in Azure AD gelöscht wurde, müssen Sie es neu registrieren. Zur erneuten Registrierung müssen Sie eine manuelle Aktion auf dem Gerät durchführen. Anweisungen zur erneuten Registrierung basierend auf dem Gerätestatus finden Sie unten. 

      Um in Azure AD Hybrid eingebundene Windows 10- und Windows Server 2016/2019-Geräte erneut zu registrieren, führen Sie die folgenden Schritte aus:

      1. Öffnen Sie die Eingabeaufforderung als Administrator.
      1. Geben Sie `dsregcmd.exe /debug /leave` ein.
      1. Melden Sie sich ab und erneut an, um den geplanten Task auszulösen, der das Gerät erneut in Azure AD registriert. 

      Gehen Sie bei kompatiblen Windows-Betriebssystemversionen, die in Azure AD Hybrid eingebunden sind, folgendermaßen vor:

      1. Öffnen Sie die Eingabeaufforderung als Administrator.
      1. Geben Sie `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"` ein.
      1. Geben Sie `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"` ein.

      Gehen Sie bei in Azure AD eingebundenen Windows 10-Geräten folgendermaßen vor:

      1. Öffnen Sie die Eingabeaufforderung als Administrator.
      1. Geben Sie `dsregcmd /forcerecovery` ein (Hinweis: Sie müssen Administrator sein, um diese Aktion auszuführen).
      1. Klicken Sie im angezeigten Dialogfeld auf „Anmelden“, und fahren Sie mit dem Anmeldevorgang fort.
      1. Melden Sie sich vom Gerät ab, und melden Sie sich erneut an, um die Wiederherstellung abzuschließen.

      Gehen Sie bei in Azure AD registrierten Windows 10-Geräten folgendermaßen vor:

      1. Wechseln Sie zu **Einstellungen** > **Konten** > **Auf Geschäfts-, Schul- oder Unikonto zugreifen**. 
      1. Wählen Sie das Konto aus, und klicken Sie auf **Trennen**.
      1. Klicken Sie auf „+ Verbinden“, und registrieren Sie das Gerät erneut, indem Sie den Anmeldevorgang durchlaufen.

---

### <a name="q-why-do-i-see-duplicate-device-entries-in-the-azure-portal"></a>F: Warum sehe ich doppelte Geräteeinträge im Azure-Portal?

**A:**

- Wenn unter Windows 10 und Windows Server 2016 wiederholt versucht wird, dasselbe Gerät zu entfernen und erneut hinzuzufügen, können doppelte Einträge auftreten. 
- Jeder Windows-Benutzer, der **Geschäfts-, Schul- oder Unikonto hinzufügen** verwendet, erstellt einen neuen Gerätedatensatz mit demselben Gerätenamen.
- Für kompatible Windows-Versionen, die über die automatische Registrierung in die lokale Active Directory-Domäne eingebunden sind, wird ein neuer Gerätedatensatz mit demselben Gerätenamen für jeden Domänenbenutzer erstellt, der sich beim Gerät anmeldet. 
- Ein in Azure AD eingebundener Computer, der gelöscht, neu installiert und mit demselben Namen wieder eingebunden wurde, wird als anderer Datensatz mit demselben Gerätenamen angezeigt.

---

### <a name="q-does-windows-10-device-registration-in-azure-ad-support-tpms-in-fips-mode"></a>F: Unterstützt die Windows 10-Geräteregistrierung in Azure AD TPMs im FIPS-Modus?

**A:** Nein, derzeit unterstützt die Geräteregistrierung unter Windows 10 für alle Gerätezustände – Hybrid Azure AD Join, Azure AD Join und Azure AD Registered – keine TPMs im FIPS-Modus. Für eine erfolgreiche Anmeldung oder Registrierung bei Azure AD muss der FIPS-Modus für die TPMs auf diesen Geräten deaktiviert werden.

---

**F: Warum kann ein Benutzer weiterhin Ressourcen von einem Gerät aufrufen, das ich im Azure-Portal deaktiviert habe?**

**A:** Das Widerrufen dauert bis zu einer Stunde.

>[!NOTE] 
>Für registrierte Geräte wird empfohlen, das Gerät zu löschen, um sicherzustellen, dass Benutzer nicht auf die Ressourcen zugreifen können. Weitere Informationen finden Sie unter [Was ist die Geräteregistrierung?](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune). 

---

## <a name="azure-ad-join-faq"></a>Häufig gestellte Fragen zu Azure AD Join

### <a name="q-how-do-i-unjoin-an-azure-ad-joined-device-locally-on-the-device"></a>F: Wie entferne ich ein in Azure AD eingebundenes Gerät lokal auf dem Gerät?

**A:** 
- Für in Azure AD eingebundene Hybridgeräte muss die automatische Registrierung deaktiviert sein. Der geplante Task registriert das Gerät also nicht erneut. Öffnen Sie als nächstes die Eingabeaufforderung als Administrator, und geben Sie `dsregcmd.exe /debug /leave` ein. Oder führen Sie diesen Befehl als Skript für mehrere Geräte aus, um die Einbindung für diese Geräte gleichzeitig aufzuheben.
- Stellen Sie bei ausschließlich in Azure AD eingebundenen Geräten sicher, dass Sie über ein lokales Offlineadministratorkonto verfügen. Sie können sich nicht mit Ihren Azure AD-Anmeldeinformationen anmelden. Navigieren Sie als Nächstes zu **Einstellungen** > **Konten** > **Auf Geschäfts-, Schul- oder Unikonto zugreifen**. Wählen Sie Ihr Konto aus, und klicken Sie auf **Trennen**. Befolgen Sie die Anweisungen, und geben Sie die Anmeldeinformationen für den lokalen Administrator an, wenn Sie aufgefordert werden. Starten Sie das Gerät neu, um den Vorgang zur Aufhebung einer Einbindung abzuschließen.

---

### <a name="q-can-my-users-sign-in-to-azure-ad-joined-devices-that-are-deleted-or-disabled-in-azure-ad"></a>F: Können sich meine Benutzer bei in Azure AD eingebundenen Geräten anmelden, die in Azure AD gelöscht oder deaktiviert wurden?

**A:** Ja. Windows verfügt über eine Zwischenspeicherfunktion für Benutzernamen und Kennwörter, die es Benutzern, die sich zuvor angemeldet haben, ermöglicht, auch ohne Netzwerkverbindung schnell auf den Desktop zuzugreifen. 

Ein in Azure AD gelöschtes oder deaktiviertes Gerät wird vom Windows-Gerät nicht erkannt. Benutzer, die sich zuvor angemeldet haben, greifen also weiterhin mit dem im Cache gespeicherten Benutzernamen und Kennwort auf den Desktop zu. Wenn das Gerät gelöscht oder deaktiviert wird, können die Benutzer jedoch nicht auf durch den gerätebasierten bedingten Zugriff geschützte Ressourcen zugreifen. 

Benutzer, die sich zuvor nicht angemeldet haben, können nicht auf das Gerät zugreifen. Es wurde kein zwischengespeicherter Benutzernamen oder Kennwort für sie aktiviert. 

---

### <a name="q-can-a-disabled-or-deleted-user-sign-in-to-an-azure-ad-joined-devices"></a>F: Kann sich ein deaktivierter oder gelöschter Benutzer bei in Azure AD eingebundenen Geräten anmelden?

**A:** Ja, aber nur für einen begrenzten Zeitraum. Ein in Azure AD gelöschter oder deaktivierter Benutzer wird vom Windows-Gerät nicht sofort erkannt. Benutzer, die sich zuvor angemeldet haben, greifen also mit dem im Cache gespeicherten Benutzernamen und Kennwort auf den Desktop zu. 

In der Regel ist das Gerät in weniger als vier Stunden über den Benutzerstatus informiert. Dann blockiert Windows den Zugriff dieser Benutzer auf den Desktop. Wie der Benutzer gelöscht oder in Azure AD deaktiviert ist, werden alle seine Token widerrufen. Daher kann der Benutzer auf keine Ressourcen mehr zugreifen. 

Gelöschte oder deaktivierte Benutzer, die sich zuvor nicht angemeldet haben, können nicht auf das Gerät zugreifen. Es wurde kein zwischengespeicherter Benutzernamen oder Kennwort für sie aktiviert. 

---

### <a name="q-why-do-my-users-have-issues-on-azure-ad-joined-devices-after-changing-their-upn"></a>F: Warum haben meine Benutzer nach dem Ändern des UPN Probleme mit in Azure AD eingebundenen Geräten?

**A:** Derzeit werden UPN-Änderungen nicht vollständig auf in Azure AD eingebundenen Geräten unterstützt. Also tritt bei der Authentifizierung mit Azure AD nach den UPN-Änderungen ein Fehler auf. Infolgedessen haben Benutzer Probleme mit SSO und bedingtem Zugriff auf ihren Geräten. Zu diesem Zeitpunkt müssen sich die Benutzer über die Kachel „Anderer Benutzer“ mit ihrem neuen UPN bei Windows anmelden, um dieses Problem zu lösen. Wir arbeiten derzeit an einer Lösung für das Problem. Allerdings tritt das Problem bei Benutzern, die sich mit Windows Hello for Business anmelden, nicht auf. 

---

### <a name="q-my-users-cant-search-printers-from-azure-ad-joined-devices-how-can-i-enable-printing-from-those-devices"></a>F: Meine Benutzer können über in Azure AD eingebundene Geräte keine Drucker suchen. Wie kann ich das Drucken über diese Geräte aktivieren?

**A:** Informationen zum Bereitstellen von Druckern für in Azure AD eingebundene Geräte finden Sie unter [Bereitstellen von Windows Server Hybrid Cloud Print mit Vorauthentifizierung](https://docs.microsoft.com/windows-server/administration/hybrid-cloud-print/hybrid-cloud-print-deploy). Sie benötigen einen lokalen Windows-Server, um das Drucken in Hybrid Clouds bereitzustellen. Aktuell ist kein cloudbasierter Druckdienst verfügbar. 

---

### <a name="q-how-do-i-connect-to-a-remote-azure-ad-joined-device"></a>F: Wie kann ich eine Verbindung mit einem in Azure AD eingebundenen Remotegerät herstellen?

**A:** Informationen finden Sie unter [Herstellen einer Verbindung mit einem in Azure AD eingebundenen Remotecomputer](https://docs.microsoft.com/windows/client-management/connect-to-remote-aadj-pc).

---

### <a name="q-why-do-my-users-see-you-cant-get-there-from-here"></a>F: Warum wird meinen Benutzern angezeigt: *Von hier aus haben Sie darauf keinen Zugriff*?

**A:** Haben Sie bestimmte Regeln für bedingten Zugriff konfiguriert, um einen bestimmten Gerätestatus zu erfordern? Wenn das Gerät die Kriterien nicht erfüllt, wird der Benutzer blockiert und diese Meldung angezeigt. Überprüfen Sie die Regeln für bedingten Zugriff. Stellen Sie sicher, dass das Gerät die Kriterien erfüllt, um die Meldung zu vermeiden.

---

### <a name="q-why-dont-some-of-my-users-get-azure-multi-factor-authentication-prompts-on-azure-ad-joined-devices"></a>F: Warum erhalten einige Benutzer in keine Azure Multi-Factor Authentication-Eingabeaufforderungen auf in Azure AD eingebundenen Geräten?

**A:** Ein Benutzer kann ein Gerät mit Multi-Factor Authentication in Azure AD einbinden oder registrieren. Dann wird das Gerät zu einem vertrauenswürdigen zweiten Faktor für diesen Benutzer. Wann immer sich derselbe Benutzer bei dem Gerät anmeldet und auf eine Anwendung zugreift, betrachtet Azure AD das Gerät als einen zweiten Faktor. Es ermöglicht dem Benutzer den nahtlosen Zugriff auf Anwendungen ohne zusätzliche Multi-Factor Authentication-Eingabeaufforderungen. 

Dieses Verhalten:

- Gilt für in Azure AD eingebundene und bei Azure AD registrierte Geräte, jedoch nicht für über Azure AD Hybrid Join eingebundene Geräte.
- Gilt nicht für andere Benutzer, die sich bei diesem Gerät anmelden. So erhalten alle anderen Benutzer, die auf dieses Gerät zugreifen, eine Multi-Factor Authentication. Dann können sie auf Anwendungen zugreifen, für die Multi-Factor Authentication erforderlich ist.

---

### <a name="q-why-do-i-get-a-username-or-password-is-incorrect-message-for-a-device-i-just-joined-to-azure-ad"></a>F: Warum wird die Meldung *Benutzername oder Kennwort ist falsch* für ein Gerät angezeigt, das ich vor Kurzem in Azure AD eingebunden habe?

**A:** Häufige Ursachen für dieses Szenario sind:

- Ihre Benutzeranmeldeinformationen sind nicht mehr gültig.
- Ihr Computer kann nicht mit Azure Active Directory kommunizieren. Suchen Sie nach Netzwerkkonnektivitätsproblemen.
- Für Verbundanmeldungen muss der Verbundserver aktive und zugängliche WS-Trust-Endpunkte unterstützen. 
- Sie haben die Passthrough-Authentifizierung aktiviert. Daher muss Ihr temporäres Kennwort geändert werden, wenn Sie sich anmelden.

---

### <a name="q-why-do-i-see-the-oops-an-error-occurred-dialog-when-i-try-to-azure-ad-join-my-pc"></a>F: Warum wird das Dialogfeld *Entschuldigung... Es ist ein Fehler aufgetreten!* angezeigt, wenn ich versuche, in Azure AD meinen PC einzubinden?

**A:** Dieser Fehler tritt bei der Einrichtung der Azure Active Directory-Registrierung bei Intune auf. Stellen Sie sicher, dass dem Benutzer, der ein Einbinden in Azure AD versucht, die richtige Intune-Lizenz zugewiesen wurde. Weitere Informationen finden Sie unter [Einrichten der Registrierung für Windows-Geräte](https://docs.microsoft.com/intune/windows-enroll).  

---

### <a name="q-why-did-my-attempt-to-azure-ad-join-a-pc-fail-although-i-didnt-get-any-error-information"></a>F: Warum konnte ich meinen PC nicht in Azure AD einbinden, obwohl ich keine Fehlerinformationen erhalten habe?

**A:** Wahrscheinlich haben Sie sich mit dem lokalen integrierten Administratorkonto beim Gerät angemeldet. Erstellen Sie ein anderes lokales Konto, bevor Sie Azure Active Directory Join verwenden, um die Einrichtung abzuschließen. 

---

### <a name="qwhat-are-the-ms-organization-p2p-access-certificates-present-on-our-windows-10-devices"></a>F: Welche MS-Organisation-P2P-Access-Zertifikate sind auf unseren Windows 10-Geräten vorhanden?

**A:** Die MS-Organisation-P2P-Access-Zertifikate werden von Azure AD sowohl für in Azure AD eingebundene Geräte als auch für in Azure AD eingebundene Hybridgeräte ausgestellt. Diese Zertifikate werden verwendet, um die Vertrauensstellung zwischen Geräten desselben Mandanten für Remotedesktopszenarien zu ermöglichen. Ein Zertifikat wird für das Gerät und ein weiteres für den Benutzer ausgestellt. Das Gerätezertifikat befindet sich in `Local Computer\Personal\Certificates` und gilt für einen Tag. Dieses Zertifikat wird erneuert (durch Ausstellung eines neuen Zertifikats), wenn das Gerät noch in Azure AD aktiv ist. Das Benutzerzertifikat befindet sich in `Current User\Personal\Certificates` und dieses Zertifikat ist ebenfalls für einen Tag gültig, wird aber auf Anfrage ausgestellt, wenn ein Benutzer eine Remotedesktopsitzung mit einem anderen in Azure AD eingebundenen Gerät versucht. Es wird bei Ablauf nicht erneuert. Beide Zertifikate werden über das MS-Organisation-P2P-Access-Zertifikat in `Local Computer\AAD Token Issuer\Certificates` ausgegeben. Dieses Zertifikat wird von Azure AD bei der Registrierung des Geräts ausgegeben. 

---

### <a name="qwhy-do-i-see-multiple-expired-certificates-issued-by-ms-organization-p2p-access-on-our-windows-10-devices-how-can-i-delete-them"></a>F: Warum sehe ich mehrere abgelaufene Zertifikate, die von MS-Organisation-P2P-Access auf unseren Windows 10-Geräten ausgestellt wurden? Wie kann ich diese löschen?

**A:** Es wurde ein Problem unter Windows 10 Version 1709 und niedriger festgestellt, bei dem abgelaufene MS-Organisation-P2P-Access-Zertifikate aufgrund von kryptographischen Problemen weiterhin im Computerspeicher vorhanden waren. Ihre Benutzer könnten Probleme mit der Netzwerkkonnektivität haben, wenn Sie VPN-Clients (z. B. Cisco AnyConnect) verwenden, die die große Anzahl abgelaufener Zertifikate nicht verarbeiten können. Dieses Problem wurde in der Version Windows 10 1803 behoben, um solche abgelaufenen MS-Organisation-P2P-Access-Zertifikate automatisch zu löschen. Sie können dieses Problem beheben, indem Sie Ihre Geräte auf Windows 10 1803 aktualisieren. Wenn Sie nicht aktualisieren können, können Sie diese Zertifikate ohne negative Auswirkungen löschen.  

---

## <a name="hybrid-azure-ad-join-faq"></a>Häufig gestellte Fragen zu Azure AD Hybrid Join

### <a name="q-where-can-i-find-troubleshooting-information-to-diagnose-hybrid-azure-ad-join-failures"></a>F: Wo finde ich Problembehandlungsinformationen für die Diagnose von Azure AD Hybrid Join-Fehlern?

**A:** Informationen zur Problembehandlung finden Sie in diesen Artikeln:

- [Beheben von Problemen mit Geräten unter Windows 10 und Windows Server 2016 mit Hybrideinbindung in Azure Active Directory](troubleshoot-hybrid-join-windows-current.md)
- [Beheben von Problemen mit Geräten mit Hybrideinbindung in Azure Active Directory](troubleshoot-hybrid-join-windows-legacy.md)
 
### <a name="q-why-do-i-see-a-duplicate-azure-ad-registered-record-for-my-windows-10-hybrid-azure-ad-joined-device-in-the-azure-ad-devices-list"></a>F: Warum wird in der Liste mit den Azure AD-Geräten für mein in Azure AD eingebundenes Windows 10-Hybridgerät ein doppelter Azure AD-Registrierungseintrag angezeigt?

**A:** Wenn Benutzer ihr Konto den Apps auf einem in die Domäne eingebundenen Gerät hinzufügen, wird ggf. eine Frage der Art **Soll das Konto Windows hinzugefügt werden?** angezeigt. Wenn Sie in der Eingabeaufforderung **Ja** eingeben, wird das Gerät in Azure AD registriert. Der Vertrauenstyp wird als in Azure AD registriert gekennzeichnet. Nachdem Sie Azure AD Hybrid Join in Ihrer Organisation aktiviert haben, wird das Gerät auch in die Azure AD-Hybridumgebung eingebunden. Dann werden zwei Gerätestatus für dasselbe Gerät angezeigt. 

Azure AD Hybrid Join hat Vorrang vor dem Azure AD-Registrierungsstatus. Ihr Gerät wird also für alle Auswertungen in Bezug auf die Authentifizierung und den bedingten Zugriff als Azure AD-Hybrideinbindung angesehen. Sie können den Azure AD-Registrierungseintrag für das Gerät daher ohne Weiteres aus dem Azure AD-Portal löschen. Erfahren Sie mehr zum [Vermeiden oder Bereinigen dieses zweifachen Status auf einem Windows 10-Computer](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-plan#review-things-you-should-know). 

---

### <a name="q-why-do-my-users-have-issues-on-windows-10-hybrid-azure-ad-joined-devices-after-changing-their-upn"></a>F: Warum haben meine Benutzer nach dem Ändern des UPN Probleme mit in Azure AD eingebundenen Windows 10-Hybridgeräten?

**A:** Derzeit werden UPN-Änderungen nicht vollständig auf in Azure AD eingebundenen Hybridgeräten unterstützt. Benutzer können sich zwar am Gerät anmelden und auf ihre lokalen Anwendungen zugreifen, bei der Authentifizierung mit Azure AD tritt nach einer UPN-Änderung aber ein Fehler auf. Infolgedessen haben Benutzer Probleme mit SSO und bedingtem Zugriff auf ihren Geräten. Derzeit müssen Sie das Gerät von Azure AD trennen (führen Sie „dsregcmd /leave“ mit erhöhten Rechten aus) und sich wieder anmelden (geschieht automatisch), um das Problem zu lösen. Wir arbeiten derzeit an einer Lösung für das Problem. Allerdings tritt das Problem bei Benutzern, die sich mit Windows Hello for Business anmelden, nicht auf. 

---

### <a name="q-do-windows-10-hybrid-azure-ad-joined-devices-require-line-of-sight-to-the-domain-controller-to-get-access-to-cloud-resources"></a>F: Benötigen Azure AD Hybrid Join-Geräte unter Windows 10 Sichtverbindung zum Domänencontroller, um auf die Ressourcen in der Cloud zugreifen zu können?

**A:** Nein, außer wenn das Kennwort des Benutzers geändert wird. Nachdem die Einrichtung von Azure AD Hybrid Join unter Windows 10 abgeschlossen ist und sich der Benutzer mindestens einmal angemeldet hat, benötigt das Gerät keine Sichtverbindung zum Domänencontroller, um auf die Cloud-Ressourcen zuzugreifen. Windows 10 kann das einmalige Anmelden bei Azure AD-Anwendungen für jeden beliebigen Standort mit Internetverbindung einrichten, solange kein Kennwort geändert wird. Für Benutzer, die sich mit Windows Hello for Business anmelden, ist selbst nach einer Kennwortänderung weiterhin das einmalige Anmelden bei Azure AD-Anwendungen verfügbar, auch wenn sie keine Sichtverbindung zu ihrem Domänencontroller haben. 

---

### <a name="q-what-happens-if-a-user-changes-their-password-and-tries-to-login-to-their-windows-10-hybrid-azure-ad-joined-device-outside-the-corporate-network"></a>F: Was passiert, wenn ein Benutzer sein Kennwort ändert und versucht, sich bei seinem in Azure AD eingebundenen Windows 10-Hybridgerät außerhalb des Unternehmensnetzwerks anzumelden?

**A:** Wenn ein Kennwort außerhalb des Unternehmensnetzwerks geändert wird (z.B. durch die Verwendung von Azure AD SSPR), schlägt die Benutzeranmeldung mit dem neuen Kennwort fehl. Für in Azure AD eingebundene Hybridgeräte ist das lokale Active Directory die primäre Autorität. Wenn ein Gerät sich nicht in Sichtweite des Domänencontrollers befindet, kann es das neue Kennwort nicht validieren. Daher muss der Benutzer eine Verbindung mit dem Domänencontroller herstellen (entweder über VPN oder im Unternehmensnetzwerk), bevor er sich mit seinem neuen Kennwort bei dem Gerät anmelden kann. Andernfalls kann er sich aufgrund der Fähigkeit der zwischengespeicherten Anmeldung in Windows nur mit seinem alten Kennwort anmelden. Das alte Kennwort wird jedoch von Azure AD bei Tokenanforderungen ungültig gemacht. Auf diese Weise wird die SSO-Anmeldung verhindert und alle gerätebasierten Richtlinien für bedingten Zugriff schlagen fehl. Dieses Problem tritt nicht auf, wenn Sie Windows Hello for Business verwenden. 

---

## <a name="azure-ad-register-faq"></a>Häufig gestellte Fragen zur Azure AD-Registrierung

### <a name="q-can-i-register-android-or-ios-byod-devices"></a>F: Kann ich Android- oder iOS-BYOD-Geräte registrieren?

**A:** Ja, aber nur mit dem Azure-Dienst zur Geräteregistrierung und wenn Sie Hybrid-Kunde sind. Es wird nicht mit dem lokalen Geräteregistrierungsdienst in Active Directory Federation Services (AD FS) unterstützt.

### <a name="q-how-can-i-register-a-macos-device"></a>F: Wie kann ich ein macOS-Gerät registrieren?

**A:** Führen Sie die folgenden Schritte aus:

1.  [Erstellen Sie eine Konformitätsrichtlinie](https://docs.microsoft.com/intune/compliance-policy-create-mac-os).
1.  [Definieren Sie eine Richtlinie zum bedingten Zugriff für macOS-Geräte](../active-directory-conditional-access-azure-portal.md). 

**Hinweise:**

- Die in der Richtlinie zum bedingten Zugriff enthaltenen Benutzer benötigen für den Zugriff auf Ressourcen eine [unterstützte Version von Office für macOS](../conditional-access/technical-reference.md#client-apps-condition). 
- Beim ersten Zugriffsversuch werden die Benutzer aufgefordert, das Gerät über das Unternehmensportal zu registrieren.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [bei Azure AD registrierten Geräten](concept-azure-ad-register.md)
- Weitere Informationen zu [in Azure AD eingebundene Geräte](concept-azure-ad-join.md)
- Weitere Informationen zu [in Hybrid Azure AD eingebundene Geräte](concept-azure-ad-join-hybrid.md)

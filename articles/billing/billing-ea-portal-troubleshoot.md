---
title: Problembehandlung für den Zugriff auf das Azure EA-Portal
description: In diesem Artikel werden einige häufige Probleme beschrieben, die im Zusammenhang mit einem Azure Enterprise Agreement (EA) im Azure EA-Portal auftreten können.
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/09/2019
ms.topic: troubleshooting
ms.service: billing
manager: boalcsva
ms.openlocfilehash: a2214ef0dd3b7c4f706ebbb529b6c44e956f9242
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70900878"
---
# <a name="troubleshoot-azure-ea-portal-access"></a>Problembehandlung für den Zugriff auf das Azure EA-Portal

In diesem Artikel werden einige häufige Probleme beschrieben, die im Zusammenhang mit einem Azure Enterprise Agreement (EA) auftreten können. Das Azure EA-Portal dient zum Verwalten von Enterprise Agreement-Benutzern und -Kosten. Die folgenden Probleme können auftreten, wenn Sie den Zugriff auf das Azure EA-Portal konfigurieren oder aktualisieren.

## <a name="issues-adding-an-admin-to-an-enrollment"></a>Probleme beim Hinzufügen eines Administrators zu einer Registrierung

Für Enterprise-Registrierungen stehen verschiedene Arten von Authentifizierungsebenen zur Verfügung. Bei nicht ordnungsgemäßer Anwendung von Authentifizierungsebenen treten möglicherweise Probleme auf, wenn Sie versuchen, sich beim Azure EA-Portal anzumelden.

Das Azure EA-Portal wird verwendet, um Benutzern mit verschiedenen Authentifizierungsebenen Zugriff zu gewähren. Ein Unternehmensadministrator kann die Authentifizierungsebene aktualisieren, um die Sicherheitsanforderungen seiner Organisation zu erfüllen.

### <a name="authentication-level-types"></a>Arten von Authentifizierungsebenen

- Nur Microsoft-Konto: Für Organisationen, die Benutzer über Microsoft-Konten verwenden, erstellen und verwalten möchten.
- Geschäfts-, Schul- oder Unikonto: Für Organisationen, die Active Directory mit Cloudverbund eingerichtet haben und bei denen sich alle Konten in einem einzelnen Mandanten befinden.
- Geschäfts- oder Schulkonto – Mandanten-übergreifend: Für Organisationen, die Active Directory mit Cloudverbund eingerichtet haben und über Konten in mehreren Mandanten verfügen.
- Mischkonto: Diese Option ermöglicht das Hinzufügen von Benutzern mit einem Microsoft-Konto und/oder einem Geschäfts-, Schul- oder Unikonto.

Das erste Geschäfts-, Schul- oder Unikonto, das der Registrierung hinzugefügt wird, bestimmt die _Standarddomäne_ (oder _Masterdomäne_). Wenn Sie ein Geschäfts-, Schul- oder Unikonto mit einem anderen Mandanten hinzufügen möchten, müssen Sie die Authentifizierungsebene unter der Registrierung in die mandantenübergreifende Authentifizierung ändern.

So aktualisieren Sie die Authentifizierungsebene:

1. Melden Sie sich beim Azure EA-Portal als Unternehmensadministrator an.
2. Klicken Sie im linken Navigationsbereich auf **Verwalten**.
3. Klicken Sie auf die Registerkarte **Registrierung**.
4. Wählen Sie unter **Registrierungsdetails** die Option **Authentifizierungsebene** aus.
5. Klicken Sie auf das Stiftsymbol.
6. Klicken Sie auf **Speichern**.

![Beispiel mit Authentifizierungsebenen ](./media/billing-ea-portal-troubleshoot/create-ea-authentication-level-types.png)

Für Microsoft-Konten muss auf [https://signup.live.com](https://signup.live.com/) eine zugehörige ID erstellt werden.

Geschäfts-, Schul- oder Unikonten stehen für Organisationen zur Verfügung, die Active Directory mit Verbund eingerichtet haben und bei denen sich alle Konten in einem einzelnen Mandanten befinden. Benutzer können mit der Verbundbenutzerauthentifizierung für Geschäfts-, Schul- oder Unikonten hinzugefügt werden, wenn das interne Active Directory des Unternehmens einem Verbund angehört.

Falls Ihre Organisation keinen Active Directory-Verbund verwendet, können Sie Ihre Geschäfts-, Schul- oder Uni-E-Mail-Adresse nicht verwenden. Registrieren oder erstellen Sie stattdessen eine neue E-Mail-Adresse, und registrieren Sie sie als Microsoft-Konto.

## <a name="unable-to-access-the-azure-ea-portal"></a>Zugriff auf das Azure EA-Portal nicht möglich

Sollte bei der Anmeldung beim Azure EA-Portal eine Fehlermeldung angezeigt werden, führen Sie die folgenden Problembehandlungsschritte aus:

- Vergewissern Sie sich, dass Sie die korrekte Azure EA-Portal-URL verwenden: https://ea.azure.com.
- Ermitteln Sie, ob Ihr Zugriff auf das Azure EA-Portal als Geschäfts-, Schul- oder Unikonto oder als Microsoft-Konto hinzugefügt wurde.
  - Geben Sie bei Verwendung eines Geschäftskontos Ihre geschäftliche E-Mail-Adresse und Ihr geschäftliches Kennwort ein. Ihr geschäftliches Kennwort erhalten Sie von Ihrer Organisation. Bei Problemen mit dem Kennwort können Sie sich zwecks Kennwortzurücksetzung an Ihre IT-Abteilung wenden.
  - Geben Sie bei Verwendung eines Microsoft-Kontos die E-Mail-Adresse und das Kennwort für Ihr Microsoft-Konto ein. Sollten Sie das Kennwort Ihres Microsoft-Kontos vergessen haben, können Sie es unter [https://account.live.com/password/reset](https://account.live.com/password/reset) zurücksetzen.
- Verwenden Sie für die Anmeldung eine InPrivate- oder Inkognito-Browsersitzung, damit keine Cookies oder zwischengespeicherten Informationen aus vorherigen oder vorhandenen Sitzungen gespeichert werden. Löschen Sie den Browsercache, und öffnen Sie https://ea.azure.com in einem InPrivate- oder Inkognito-Fenster.
- Sollte bei Verwendung eines Microsoft-Kontos ein Fehler vom Typ _Ungültiger Benutzer_ auftreten, besitzen Sie möglicherweise mehrere Microsoft-Konten. Bei dem für die Anmeldung verwendeten Konto handelt es sich nicht um die primäre E-Mail-Adresse.
Ein Fehler vom Typ _Ungültiger Benutzer_ kann aber auch auftreten, wenn beim Hinzufügen des Benutzers zur Registrierung der falsche Kontotyp verwendet wurde (beispielsweise ein Geschäfts-, Schul- oder Unikonto anstelle eines Microsoft-Kontos). In diesem Fall muss ein anderer EA-Administrator das richtige Konto hinzufügen, oder Sie müssen sich an den [Support](https://support.microsoft.com/supportforbusiness/productselection?sapId=cf791efa-485b-95a3-6fad-3daf9cd4027c) wenden.

## <a name="no-activation-email-received"></a>Keine Aktivierungs-E-Mail erhalten

Vom Azure EA-Portal wird eine E-Mail mit dem Absender *waep@microsoft.com* gesendet. Sollten Sie keine Aktivierungs-E-Mail erhalten haben, durchsuchen Sie Ihren Spam- oder Junk-Ordner nach der E-Mail. Der Betreff lautet _Invitation to View/Manage the Microsoft Azure service_ (Einladung zum Anzeigen/Verwalten des Microsoft Azure-Diensts). Sie wird an jeden neu hinzugefügten EA-Administrator gesendet.

Sollten Sie wissen, dass Sie als EA-Administrator eingerichtet wurden, müssen Sie nicht auf die Aktivierungs-E-Mail warten, um sich beim Azure EA-Portal anzumelden. Stattdessen können Sie sich unter https://ea.azure.com mit Ihrer E-Mail-Adresse (Microsoft-Konto oder Geschäfts-, Schul- oder Unikonto) und Ihrem Kennwort anmelden.

Den primären Alias können Sie unter [https://account.live.com](https://account.live.com) überprüfen. Klicken Sie anschließend auf **Ihre Infos** > **Anmeldung bei Microsoft verwalten**. Befolgen Sie die Anweisungen, um eine alternative E-Mail-Adresse zu verifizieren und einen Code für den Zugriff auf vertrauliche Informationen zu erhalten. Geben Sie den Sicherheitscode ein. Falls Sie keine zweistufige Authentifizierung einrichten möchten, wählen Sie **Später einrichten** aus.

Auf der daraufhin angezeigten Seite **Anmeldung bei Microsoft verwalten** können Sie Ihre Kontoaliase anzeigen. Vergewissern Sie sich, dass der primäre Alias der Alias ist, den Sie für die Anmeldung beim Azure EA-Portal verwenden. Falls nicht, können Sie ihn als primären Alias festlegen. Alternativ können Sie den primären Alias für das Azure EA-Portal verwenden.

## <a name="next-steps"></a>Nächste Schritte

- Azure EA-Portaladministratoren finden unter [Azure EA-Portalverwaltung](billing-ea-portal-administration.md) Informationen zu allgemeinen Verwaltungsaufgaben.

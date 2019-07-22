---
title: Einlösen von Einladungen in B2B-Zusammenarbeit – Azure Active Directory | Microsoft-Dokumentation
description: Beschreibt das Einlösen von Einladungen in Azure AD B2B-Zusammenarbeit für Endbenutzer (einschließlich der Zustimmung zu Datenschutzrichtlinien).
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisol
ms.collection: M365-identity-device-management
ms.openlocfilehash: a80eaa134130195fce00ee6a4d68851e478c4532
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67052473"
---
# <a name="azure-active-directory-b2b-collaboration-invitation-redemption"></a>Azure Active Directory B2B-Zusammenarbeit: Einlösen von Einladungen

Dieser Artikel beschreibt die Möglichkeiten, wie Gastbenutzer auf Ihre Ressourcen zugreifen können, und den Einwilligungsprozess, auf den sie stoßen werden. Wenn Sie eine Einladungs-E-Mail an den Gast senden, enthält die Einladung einen Link, den der Gast einlösen kann, um Zugriff auf Ihre App oder Ihr Portal zu erhalten. Die Einladungs-E-Mail ist nur eine der Möglichkeiten, wie Gäste Zugriff auf Ihre Ressourcen erhalten können. Alternativ können Sie Ihrem Verzeichnis Gäste hinzufügen und ihnen einen direkten Link zu dem Portal oder der App zur Verfügung stellen, das bzw. die Sie freigeben möchten. Unabhängig von der verwendeten Methode werden Gäste schrittweise durch einen erstmaligen Einwilligungsprozess geführt. Dieser Prozess stellt sicher, dass Ihre Gäste den Datenschutzbestimmungen zustimmen und alle [Nutzungsbedingungen](https://docs.microsoft.com/azure/active-directory/governance/active-directory-tou) akzeptieren, die Sie eingerichtet haben.

Wenn Sie Ihrem Verzeichnis einen Gastbenutzer hinzufügen, hat das Gastbenutzerkonto einen Zustimmungsstatus (in PowerShell einsehbar), der zunächst auf **PendingAcceptance** festgelegt ist. Diese Einstellung bleibt bestehen, bis der Gast Ihre Einladung annimmt und Ihrer Datenschutzerklärung und Ihren Nutzungsbedingungen zustimmt. Danach ändert sich der Zustimmungsstatus in **Accepted** (Akzeptiert), und die Zustimmungsseiten werden dem Gast nicht mehr angezeigt.

## <a name="redemption-through-the-invitation-email"></a>Einlösung über die Einladungs-E-Mail

Wenn Sie Ihrem Verzeichnis [über das Azure-Portal](https://docs.microsoft.com/azure/active-directory/b2b/b2b-quickstart-add-guest-users-portal) einen Gastbenutzer hinzufügen, wird dabei eine Einladungs-E-Mail an den Gast gesendet. Sie können auch Einladungs-E-Mails senden, wenn Sie Ihrem Verzeichnis [mit PowerShell](https://docs.microsoft.com/azure/active-directory/b2b/b2b-quickstart-invite-powershell) Gastbenutzer hinzufügen. Hier ist eine Beschreibung der Erfahrungen des Gasts, wenn er den Link in der E-Mail einlöst.

1. Der Gast erhält eine [Einladungs-E-Mail](https://docs.microsoft.com/azure/active-directory/b2b/invitation-email-elements), die über **Microsoft-Einladungen** gesendet wird.
2. Der Gast klickt in der E-Mail auf **Erste Schritte**.
3. Wenn der Gast kein Azure AD-Konto, kein Microsoft-Konto (MSA) und kein E-Mail-Konto in einer Verbundorganisation besitzt, wird er aufgefordert, ein MSA zu erstellen (es sei denn, die Funktion [Einmaliger Passcode](https://docs.microsoft.com/azure/active-directory/b2b/one-time-passcode) ist aktiviert, die kein MSA erfordert).
4. Der Gast wird schrittweise durch die im Folgenden beschriebene [Zustimmungsbenutzeroberfläche](#consent-experience-for-the-guest) geführt.

## <a name="redemption-through-a-direct-link"></a>Einlösung über einen direkten Link

Alternativ zur Einladungs-E-Mail können Sie einem Gast einen direkten Link zu Ihrer App oder Ihrem Portal zur Verfügung stellen. Zunächst müssen Sie den Gastbenutzer über das [Azure-Portal](https://docs.microsoft.com/azure/active-directory/b2b/b2b-quickstart-add-guest-users-portal) oder [PowerShell](https://docs.microsoft.com/azure/active-directory/b2b/b2b-quickstart-invite-powershell) zu Ihrem Verzeichnis hinzufügen. Dann können Sie eine der [anpassbaren Möglichkeiten zum Bereitstellen von Anwendungen für Benutzer](https://docs.microsoft.com/azure/active-directory/manage-apps/end-user-experiences) verwenden, einschließlich direkter Anmeldelinks. Wenn ein Gast anstelle der Einladungs-E-Mail einen direkten Link verwendet, wird er dennoch schrittweise durch die Benutzeroberfläche für die erste Zustimmung geführt.

> [!IMPORTANT]
> Der direkte Link muss mandantenspezifisch sein. Mit anderen Worten: Er muss eine Mandanten-ID oder eine überprüfte Domäne enthalten, damit der Gast in Ihrem Mandanten, in dem sich die freigegebene App befindet, authentifiziert werden kann. Eine allgemeine URL wie https://myapps.microsoft.com funktioniert für einen Gast nicht, da sie für die Authentifizierung an seinen Basismandanten weitergeleitet wird. Hier sind einige Beispiele für direkte Links mit Mandantenkontext:
 > - Zugriffsbereich für Apps: https://myapps.microsoft.com/?tenantid=&lt;Mandanten-ID&gt; 
 > - Zugriffsbereich für Apps für eine überprüfte Domäne: https://myapps.microsoft.com/&lt;überprüfte Domäne&gt;
 > - Azure-Portal: https://portal.azure.com/&lt;Mandanten-ID&gt;
 > - Einzelne App: Lesen Sie, wie ein [direkter Anmeldelink](../manage-apps/end-user-experiences.md#direct-sign-on-links) verwendet wird.

Es gibt einige Fälle, in denen die Einladungs-E-Mail über einen direkten Link empfohlen wird. Wenn diese Sonderfälle für Ihre Organisation relevant sind, empfiehlt es sich, für die Einladung von Benutzern eine Methode zu verwenden, bei der weiterhin eine Einladungs-E-Mail gesendet wird:
 - Der Benutzer verfügt nicht über ein Azure AD-Konto, ein MSA oder ein E-Mail-Konto in einer Verbundorganisation. Sofern Sie nicht die Funktion für einen einmaligen Passcode verwenden, muss der Gast die Einladungs-E-Mail einlösen, um durch die Schritte zur Erstellung eines MSA geführt zu werden.
 - Manchmal verfügt das eingeladene Benutzerobjekt aufgrund eines Konflikts mit einem Kontaktobjekt (beispielsweise ein Outlook-Kontaktobjekt) möglicherweise über keine E-Mail-Adresse. In diesem Fall muss der Benutzer auf die Einlösungs-URL in der Einladungs-E-Mail klicken.
 - Der Benutzer meldet sich möglicherweise mit einem Alias für die eingeladene E-Mail-Adresse an. (Ein Alias ist eine zusätzliche E-Mail-Adresse, die einem E-Mail-Konto zugeordnet ist.) In diesem Fall muss der Benutzer auf die Einlösungs-URL in der Einladungs-E-Mail klicken.

## <a name="consent-experience-for-the-guest"></a>Einwilligungsbenutzeroberfläche für den Gast

Wenn sich ein Gast zum ersten Mal anmeldet, um auf Ressourcen in einer Partnerorganisation zuzugreifen, wird er schrittweise durch die folgenden Seiten geführt. 

1. Der Gast überprüft die Seite **Berechtigungen überprüfen**, die die Datenschutzerklärung der einladenden Organisation beschreibt. Der Benutzer muss der Nutzung seiner Informationen gemäß den Datenschutzrichtlinien der Organisation **zustimmen**, um fortfahren zu können.

   ![Screenshot: Seite „Berechtigungen überprüfen“](media/redemption-experience/review-permissions.png) 

   > [!NOTE]
   > Informationen dazu, wie Sie als Mandantenadministrator einen Link zu den Datenschutzbestimmungen Ihrer Organisation einrichten, finden Sie unter [Vorgehensweise: Hinzufügen der Datenschutzinformationen mit Azure Active Directory](https://aka.ms/adprivacystatement).

2. Wenn Nutzungsbedingungen konfiguriert sind, öffnet und überprüft der Gast die Nutzungsbedingungen und wählt dann **Ich stimme zu** aus. 

   ![Screenshot mit neuen Nutzungsbedingungen](media/redemption-experience/terms-of-use-accept.png) 

   > [!NOTE]
   > Sie können die [Nutzungsbedingungen](../governance/active-directory-tou.md) unter **Verwalten** > **Organisationsbeziehungen** > **Nutzungsbedingungen** konfigurieren.

3. Sofern nicht anders angegeben, wird der Gast zum Zugriffsbereich für Apps weitergeleitet, der die Anwendungen auflistet, auf die der Gast zugreifen kann.

   ![Screenshot: Zugriffsbereich für Apps](media/redemption-experience/myapps.png) 

In Ihrem Verzeichnis ändert sich der Wert **Invitation accepted** (Einladung angenommen) des Gasts in **Yes** (Ja). Wenn ein MSA erstellt wurde, wird als **Quelle** des Gasts **Microsoft-Konto** angezeigt. Weitere Informationen zu den Eigenschaften des Gastbenutzerkontos finden Sie unter [Eigenschaften eines Benutzers von Azure AD B2B-Zusammenarbeit](user-properties.md). 

## <a name="next-steps"></a>Nächste Schritte

- [Was ist die Azure AD B2B-Zusammenarbeit?](what-is-b2b.md)
- [Add Azure Active Directory B2B collaboration users in the Azure portal](add-users-administrator.md) (Hinzufügen von Azure Active Directory B2B-Zusammenarbeitsbenutzern über das Azure-Portal)
- [How do information workers add B2B collaboration users to Azure Active Directory?](add-users-information-worker.md) (Wie fügen Information-Worker B2B-Zusammenarbeitsbenutzer zu Azure Active Directory hinzu?)
- [Azure Active Directory B2B collaboration API and customization](customize-invitation-api.md#powershell) (Azure Active Directory B2B-Zusammenarbeit: API und Anpassung)
- [Leave an organization as a guest user](leave-the-organization.md) (Verlassen einer Organisation als Gastbenutzer)

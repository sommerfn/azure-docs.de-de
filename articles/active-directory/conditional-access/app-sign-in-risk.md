---
title: Schnellstart – Blockieren des Zugriffs, wenn ein Sitzungsrisiko beim bedingten Azure Active Directory-Zugriff erkannt wird | Microsoft-Dokumentation
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie eine Azure Active Directory-Richtlinie (Azure AD) für bedingten Zugriff basierend auf Sitzungsrisiken konfigurieren können, um Anmeldungen zu blockieren.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: quickstart
ms.date: 12/14/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 619f1ea3bae001d25eb520f43da33ca94a3160c8
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72880333"
---
# <a name="quickstart-block-access-when-a-session-risk-is-detected-with-azure-active-directory-conditional-access"></a>Schnellstart: Blockieren des Zugriffs, wenn ein Sitzungsrisiko beim bedingten Azure Active Directory-Zugriff erkannt wird  

Zum Schutz Ihrer Umgebung sollten Sie verhindern, dass sich verdächtige Benutzer anmelden können. [Azure Active Directory (Azure AD) Identity Protection](../active-directory-identityprotection.md) analysiert alle Anmeldungen und berechnet die Wahrscheinlichkeit dafür, dass ein Anmeldeversuch nicht vom rechtmäßigen Besitzer eines Benutzerkontos durchgeführt wurde. Die Wahrscheinlichkeit (gering, mittel, hoch) wird in Form eines berechneten Wertes mit der Bezeichnung [Risikostufen für die Anmeldung](conditions.md#sign-in-risk) angegeben. Durch Festlegen der Bedingung zum Anmelderisiko können Sie eine Richtlinie für bedingten Zugriff konfigurieren, um auf bestimmte Risikostufen für die Anmeldung zu reagieren.

In dieser Schnellstartanleitung erfahren Sie, wie Sie eine [Richtlinie für bedingten Zugriff](../active-directory-conditional-access-azure-portal.md) konfigurieren, mit der eine Anmeldung blockiert wird, wenn eine konfigurierte Risikostufe für die Anmeldung erkannt wird.

![Richtlinie erstellen](./media/app-sign-in-risk/1000.png)

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Für die Durchführung des Szenarios im Rahmen dieses Tutorials benötigen Sie Folgendes:

- **Zugriff auf eine Azure AD Premium P2-Edition**: Der bedingte Zugriff ist zwar eine Funktion von Azure AD Premium P1. Dennoch benötigen Sie eine P2-Edition, da für das Szenario in dieser Schnellstartanleitung Identity Protection erforderlich ist.
- **Identity Protection**: Für das Szenario in dieser Schnellstartanleitung muss Identity Protection aktiviert sein. Informationen zum Aktivieren von Identity Protection finden Sie unter [Aktivieren von Azure Active Directory Identity Protection](../identity-protection/overview-identity-protection.md).
- **Tor Browser**: Mit dem [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en) können Sie Ihre Privatsphäre online schützen. Identity Protection erkennt eine Anmeldung von einem Tor Browser als Anmeldungen von anonymen IP-Adressen. Anmeldungen dieser Art weisen eine mittlere Risikostufe auf. Weitere Informationen finden Sie unter [Azure Active Directory risk detections](../reports-monitoring/concept-risk-events.md) (Azure Active Directory-Risikoerkennungen).  
- **Ein Testkonto mit dem Namen Alain Charon**: Informationen zum Erstellen eines Testkontos finden Sie unter [Hinzufügen von cloudbasierten Benutzern](../fundamentals/add-users-azure-active-directory.md#add-a-new-user).

## <a name="test-your-sign-in"></a>Testen Ihrer Anmeldung

Mit diesem Schritt soll sichergestellt werden, dass Sie mit dem Tor Browser über das Testkonto auf Ihren Mandanten zugreifen können.

**So testen Sie Ihre Anmeldung**

1. Melden Sie sich bei Ihrem [Azure-Portal](https://portal.azure.com) als **Alain Charon** an.
1. Melden Sie sich ab.

## <a name="create-your-conditional-access-policy"></a>Erstellen Ihrer Richtlinie für bedingten Zugriff

Für das Szenario in dieser Schnellstartanleitung wird eine Anmeldung über einen Tor-Browser verwendet, um eine erkannte Risikoerkennung vom Typ **Anmeldungen von anonymen IP-Adressen** zu generieren. Diese Risikoerkennung weist eine mittlere Risikostufe auf. Reagieren Sie auf diese Risikoerkennung, indem Sie die Bedingung für das Anmelderisiko auf „Mittel“ festlegen. In einer Produktionsumgebung sollten Sie die Bedingung für das Anmelderisiko entweder auf „Hoch“ oder auf „Mittel“ und „Hoch“ festlegen.

In diesem Abschnitt wird gezeigt, wie Sie die erforderliche Richtlinie für bedingten Zugriff erstellen. Legen Sie in Ihrer Richtlinie Folgendes fest:

| Einstellung | Wert |
| --- | --- |
| Benutzer und Gruppen | Alain Charon  |
| Cloud-Apps | Alle Cloud-Apps |
| Anmelderisiko | Mittel |
| Gewährung | Zugriff blockieren |

![Richtlinie erstellen](./media/app-sign-in-risk/130.png)

**So konfigurieren Sie die Richtlinie für bedingten Zugriff:**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als globaler Administrator, Sicherheitsadministrator oder Administrator für bedingten Zugriff an.
1. Klicken Sie im Azure-Portal auf der linken Navigationsleiste auf **Azure Active Directory**.

   ![Azure Active Directory](./media/app-sign-in-risk/02.png)

1. Klicken Sie auf der Seite **Azure Active Directory** im Abschnitt **Sicherheit** auf **Bedingter Zugriff**.

   ![Bedingter Zugriff](./media/app-sign-in-risk/03.png)

1. Klicken Sie auf der Seite **Bedingter Zugriff** auf der Symbolleiste am oberen Rand auf **Hinzufügen**.

   ![NAME](./media/app-sign-in-risk/108.png)

1. Geben Sie auf der Seite **Neu** im Textfeld **Name** den Namen **Zugriff für mittlere Risikostufe blockieren** ein.

   ![NAME](./media/app-sign-in-risk/104.png)

1. Klicken Sie im Abschnitt **Zuweisung** auf **Benutzer und Gruppen**.

   ![Benutzer und Gruppen](./media/app-sign-in-risk/06.png)

1. Gehen Sie auf der Seite **Benutzer und Gruppen** folgendermaßen vor:

   ![Bedingter Zugriff](./media/app-sign-in-risk/107.png)

   1. Klicken Sie auf **Benutzer und Gruppen auswählen**, und wählen Sie dann **Benutzer und Gruppen** aus.
   1. Klicken Sie auf **Auswählen**.
   1. Wählen Sie auf der Seite **Auswählen** den Eintrag **Alain Charon** aus, und klicken Sie dann auf **Auswählen**.
   1. Klicken Sie auf der Seite **Benutzer und Gruppen** auf **Fertig**.
1. Klicken Sie auf **Cloud-Apps**.

   ![Cloud-Apps](./media/app-sign-in-risk/08.png)

1. Gehen Sie auf der Seite **Cloud-Apps** folgendermaßen vor:

   ![Bedingter Zugriff](./media/app-sign-in-risk/109.png)

   1. Klicken Sie auf **Alle Cloud-Apps**.
   1. Klicken Sie auf **Fertig**.
1. Klicken Sie auf **Bedingungen**.

   ![Zugriffssteuerung](./media/app-sign-in-risk/19.png)

1. Gehen Sie auf der Seite **Bedingungen** folgendermaßen vor:

   ![Risikostufe für die Anmeldung](./media/app-sign-in-risk/21.png)

   1. Klicken Sie auf **Anmelderisiko**.
   1. Legen Sie **Konfigurieren** auf **Ja** fest.
   1. Legen Sie die Risikostufe für die Anmeldung auf **Mittel** fest.
   1. Klicken Sie auf **Auswählen**.
   1. Klicken Sie auf der Seite **Bedingungen** auf **Fertig**.
1. Klicken Sie im Abschnitt **Zugriffssteuerungen** auf **Gewähren**.

   ![Zugriffssteuerung](./media/app-sign-in-risk/10.png)

1. Gehen Sie auf der Seite **Gewähren** folgendermaßen vor:

   ![Bedingter Zugriff](./media/app-sign-in-risk/105.png)

   1. Wählen Sie **Zugriff blockieren** aus.
   1. Klicken Sie auf **Auswählen**.
1. Klicken Sie im Abschnitt **Richtlinie aktivieren** auf **Ein**.

   ![Richtlinie aktivieren](./media/app-sign-in-risk/18.png)

1. Klicken Sie auf **Create**.

## <a name="evaluate-a-simulated-sign-in"></a>Auswerten einer simulierten Anmeldung

Nachdem Sie nun Ihre Richtlinie für bedingten Zugriff konfiguriert haben, möchten Sie wahrscheinlich wissen, ob sie erwartungsgemäß funktioniert. Verwenden Sie im ersten Schritt das **What-If-Richtlinientool** des bedingten Zugriffs, um eine Anmeldung für Ihren Testbenutzer zu simulieren. Die Simulation schätzt die Auswirkungen dieser Anmeldungen auf Ihre Richtlinien ab und generiert einen Simulationsbericht.  

Wenn Sie das **What-If-Richtlinientool** für dieses Szenario verwenden, muss die Richtlinie **Zugriff für mittlere Risikostufe blockieren** unter **Anzuwendende Richtlinien** angezeigt werden.

![Benutzer](./media/app-sign-in-risk/117.png)

**So werten Sie die Richtlinie für bedingten Zugriff aus:**

1. Klicken Sie auf der Seite [Bedingter Zugriff – Richtlinien](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) im Menü am oberen Rand auf **What If**.  

   ![What If](./media/app-sign-in-risk/14.png)

1. Klicken Sie auf **Benutzer**. Wählen Sie auf der Seite **Benutzer** den Eintrag **Alan Charon** aus, und klicken Sie dann auf **Auswählen**.

   ![Benutzer](./media/app-sign-in-risk/116.png)

1. Wählen Sie für das **Anmelderisiko** die Option **Mittel** aus.

   ![Benutzer](./media/app-sign-in-risk/119.png)

1. Klicken Sie auf **What If**.

## <a name="test-your-conditional-access-policy"></a>Testen der Richtlinie für bedingten Zugriff

Im vorherigen Abschnitt haben Sie gelernt, wie Sie eine simulierte Anmeldung auswerten. Zusätzlich zu einer Simulation sollten Sie auch Ihre Richtlinie für bedingten Zugriff testen, um sicherzustellen, dass sie wie erwartet funktioniert.

Melden Sie sich zum Testen der Richtlinie mit dem Tor Browser bei Ihrem [Azure-Portal](https://portal.azure.com) als **Alan Charon** an. Ihr Anmeldeversuch sollte durch Ihre Richtlinie für bedingten Zugriff blockiert werden.

![Multi-Factor Authentication](./media/app-sign-in-risk/118.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie den Testbenutzer, wenn er nicht mehr benötigt wird, und löschen Sie auch den Tor Browser und die Richtlinie für bedingten Zugriff:

- Wenn Sie nicht wissen, wie ein Azure AD-Benutzer gelöscht wird, lesen Sie [Löschen von Benutzern aus Azure AD](../fundamentals/add-users-azure-active-directory.md#delete-a-user).
- Zum Löschen Ihrer Richtlinie wählen Sie die Richtlinie aus, und klicken Sie dann in der Symbolleiste für den Schnellzugriff auf **Löschen**.

   ![Multi-Factor Authentication](./media/app-sign-in-risk/33.png)

- Eine Anleitung zum Entfernen von Tor Browser finden Sie unter [Uninstalling (Deinstallieren)](https://tb-manual.torproject.org/uninstalling/).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Festlegen, dass die Nutzungsbedingungen akzeptiert werden müssen](require-tou.md)
> [Require MFA for specific apps (Festlegen, dass für bestimmte Apps MFA verwendet werden muss)](app-based-mfa.md)

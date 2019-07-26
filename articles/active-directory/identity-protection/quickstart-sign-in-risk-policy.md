---
title: Schnellstart – Blockieren des Zugriffs mit Azure Active Directory Identity Protection, wenn ein Sitzungsrisiko beim bedingten Azure Active Directory-Zugriff erkannt wird | Microsoft-Dokumentation
description: In diesem Schnellstart erfahren Sie, wie Sie eine Azure Active Directory (Azure AD) Identity Protection-Richtlinie für bedingten Zugriff bei einem Anmelderisiko basierend auf Sitzungsrisiken konfigurieren können, um Anmeldungen zu blockieren.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: quickstart
ms.date: 09/13/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1bb1e29735a860f5dc3b6ce8996af9fcd4962871
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2019
ms.locfileid: "68335302"
---
# <a name="quickstart-block-access-when-a-session-risk-is-detected-with-azure-active-directory-identity-protection"></a>Schnellstart: Blockieren des Zugriffs mit Azure Active Directory Identity Protection, wenn ein Sitzungsrisiko erkannt wird  

Zum Schutz Ihrer Umgebung sollten Sie verhindern, dass sich verdächtige Benutzer anmelden können. Azure Active Directory (Azure AD) Identity Protection analysiert alle Anmeldungen und berechnet die Wahrscheinlichkeit dafür, dass ein Anmeldeversuch nicht vom rechtmäßigen Besitzer eines Benutzerkontos durchgeführt wurde. Die Wahrscheinlichkeit (gering, mittel, hoch) wird in Form eines berechneten Wertes mit der Bezeichnung „Risikostufen für die Anmeldung“ angegeben. Durch Festlegen der Bedingung zum Anmelderisiko können Sie eine Richtlinie für bedingten Zugriff bei Anmelderisiko konfigurieren, um auf bestimmte Risikostufen für die Anmeldung zu reagieren. 

In diesem Schnellstart erfahren Sie, wie Sie eine Richtlinie für bedingten Zugriff bei Anmelderisiko konfigurieren, mit der eine Anmeldung blockiert wird, wenn eine mittlere oder höhere Risikostufe für die Anmeldung erkannt wird. 

![Richtlinie erstellen](./media/quickstart-sign-in-risk-policy/1004.png)

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen 

Für die Durchführung des Szenarios im Rahmen dieses Tutorials benötigen Sie Folgendes:

- **Zugriff auf eine Azure AD Premium P2-Edition** –Azure AD Identity Protection ist eine Azure AD Premium P2-Feature. 
- **Identity Protection**: Für das Szenario in dieser Schnellstartanleitung muss Identity Protection aktiviert sein. Informationen zum Aktivieren von Identity Protection finden Sie unter [Aktivieren von Azure Active Directory Identity Protection](../identity-protection/enable.md).
- **Tor Browser**: Mit dem [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en) können Sie Ihre Privatsphäre online schützen. Identity Protection erkennt eine Anmeldung von einem Tor Browser als **Anmeldungen von anonymen IP-Adressen**. Anmeldungen dieser Art weisen eine mittlere Risikostufe auf. Weitere Informationen finden Sie unter [Azure Active Directory-Risikoereignisse](../reports-monitoring/concept-risk-events.md).  
- **Ein Testkonto mit dem Namen Alain Charon**: Informationen zum Erstellen eines Testkontos finden Sie unter [Hinzufügen eines neuen Benutzers](../fundamentals/add-users-azure-active-directory.md#add-a-new-user).

## <a name="test-your-sign-in"></a>Testen Ihrer Anmeldung 

Mit diesem Schritt soll sichergestellt werden, dass Sie mit dem Tor Browser über das Testkonto auf Ihren Mandanten zugreifen können.

**So testen Sie Ihre Anmeldung**

1. Melden Sie sich bei Ihrem [Azure-Portal](https://portal.azure.com) als **Alain Charon** an.
2. Melden Sie sich ab. 

## <a name="create-your-conditional-access-policy"></a>Erstellen Ihrer Richtlinie für bedingten Zugriff 

Für das Szenario in dieser Schnellstartanleitung wird eine Anmeldung über einen Tor Browser verwendet, um ein erkanntes Risikoereignis vom Typ **Anmeldungen von anonymen IP-Adressen** zu generieren. Dieses Risikoereignis weist eine mittlere Risikostufe auf. Reagieren Sie auf dieses Risikoereignis, indem Sie die Bedingung für das Anmelderisiko auf „Mittel“ festlegen. 

In diesem Abschnitt wird gezeigt, wie Sie die erforderliche Richtlinie für bedingten Zugriff bei Anmelderisiko erstellen. Legen Sie in Ihrer Richtlinie Folgendes fest:

|Einstellung |Wert|
|---     | --- |
| Benutzer  | Alain Charon  |
| Bedingungen | Anmelderisiko, Mittel und höher |
| Kontrollen | Zugriff blockieren |

![Richtlinie erstellen](./media/quickstart-sign-in-risk-policy/201.png)

**So konfigurieren Sie die Richtlinie für bedingten Zugriff**

1. Melden Sie sich im [Azure-Portal](https://portal.azure.com) als globaler Administrator an.
2. Rufen Sie die Seite [Azure AD Identity Protection](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/Overview) auf.
3. Klicken Sie auf der Seite **Azure AD Identity Protection** im Abschnitt **Konfigurieren** auf **Richtlinie zum Anmelderisiko**.
4. Klicken Sie auf der Seite „Richtlinie“ im Abschnitt **Zuweisungen** auf **Benutzer**.
5. Auf der Seite **Benutzer** klicken Sie auf **Benutzer auswählen**.
6. Wählen Sie auf der Seite **Benutzer auswählen** den Eintrag **Alain Charon** aus, und klicken Sie dann auf **Auswählen**.
7. Auf der Seite **Benutzer** klicken Sie auf **Fertig**. 
8. Klicken Sie auf der Seite „Richtlinie“ im Abschnitt **Zuweisungen** auf **Bedingungen**.
9. Klicken Sie auf der Seite **Bedingungen** auf **Anmelderisiko**.
10. Wählen Sie auf der Seite **Anmelderisiko** **Mittel und höher**, und klicken Sie dann auf **Auswählen**. 
11. Klicken Sie auf der Seite **Bedingungen** auf **Fertig**.
12. Klicken Sie auf der Seite „Richtlinie“ im Abschnitt **Steuerelemente** auf **Zugriff**.
13. Klicken Sie auf der Seite **Zugriff** auf **Zugriff erlauben**, wählen Sie **Erfordert mehrstufige Authentifizierung** und anschließend auf **Auswählen**.
14. Klicken Sie auf der Seite „Richtlinie“ auf **Speichern**.  

## <a name="test-your-conditional-access-policy"></a>Testen der Richtlinie für bedingten Zugriff

Melden Sie sich zum Testen der Richtlinie mit dem Tor Browser bei Ihrem [Azure-Portal](https://portal.azure.com) als **Alan Charon** an. Ihr Anmeldeversuch sollte durch Ihre Richtlinie für bedingten Zugriff blockiert werden.

![Multi-Factor Authentication](./media/quickstart-sign-in-risk-policy/203.png)


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie den Testbenutzer, wenn er nicht mehr benötigt wird, und löschen Sie auch den Tor-Browser und die Richtlinie für bedingten Zugriff bei Anmelderisiko:

- Wenn Sie nicht wissen, wie ein Azure AD-Benutzer gelöscht wird, lesen Sie [Löschen oder Hinzufügen von Benutzern](../fundamentals/add-users-azure-active-directory.md#delete-a-user).
- Eine Anleitung zum Entfernen von Tor Browser finden Sie unter [Uninstalling (Deinstallieren)](https://tb-manual.torproject.org/uninstalling/).

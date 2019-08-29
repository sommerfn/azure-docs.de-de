---
title: 'Basisrichtlinie: Anfordern von MFA für die Dienstverwaltung (Vorschau) – Azure Active Directory'
description: Conditonal Access Richtlinie erfordert MFA für Azure Resource Manager
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: e8095b4fa6e52b7c34cedaea35b129ab68dddc65
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69532956"
---
# <a name="baseline-policy-require-mfa-for-service-management-preview"></a>Basisrichtlinie: Anfordern von MFA für die Dienstverwaltung (Vorschau)

Möglicherweise nutzen Sie viele verschiedene Azure-Dienste in Ihrer Organisation. Diese Dienste können über die Azure Resource Manager-API verwaltet werden:

* Azure-Portal
* Azure PowerShell
* Azure-Befehlszeilenschnittstelle

Benutzer, die Dienste mit Azure Resource Manager verwalten, verfügen über weitreichende Berechtigungen. Beispielsweise können mit Azure Resource Manager mandantenweite Konfigurationen wie Diensteinstellungen und die Abonnementabrechnung geändert werden. Die einstufige Authentifizierung ist für eine Vielzahl von Bedrohungen anfällig, beispielsweise für Phishing- und Kennwort-Spray-Angriffe. Aus diesem Grund muss die Identität von Benutzern, die auf Azure Resource Manager zugreifen und Konfigurationen aktualisieren möchten, unbedingt überprüft werden, indem der Zugriff erst nach einer Multi-Factor Authentication gewährt wird.

**Anfordern von MFA für die Dienstverwaltung** ist eine [Basisrichtlinie](concept-baseline-protection.md), die festlegt, dass jeder Benutzer, der auf das Azure-Portal, auf Azure PowerShell oder auf die Azure CLI zugreift, eine Multi-Factor Authentication (MFA) durchführen muss. Diese Richtlinie gilt für alle Benutzer mit Zugriff auf Azure Resource Manager und nicht nur für Administratoren.

Sobald die Richtlinie in einem Mandanten aktiviert ist, werden alle Benutzer, die sich bei Azure-Verwaltungsressourcen anmelden, aufgefordert, eine Multi-Factor Authentication durchzuführen. Wenn der Benutzer nicht für die Multi-Factor Authentication registriert ist, muss er sich mithilfe der Microsoft Authenticator-App registrieren, um fortzufahren.

Um eine interaktive Anmeldung mithilfe von [Azure Powershell](https://docs.microsoft.com/powershell/azure/authenticate-azureps) auszuführen, verwenden Sie das [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount)-Cmdlet.

```PowerShell
Connect-AzAccount
```

Bei der Ausführung wird von diesem Cmdlet eine Tokenzeichenfolge bereitgestellt. Kopieren Sie diese Zeichenfolge, und fügen Sie sie in einem Browser unter [https://microsoft.com/devicelogin](https://microsoft.com/devicelogin) ein, um sich anzumelden. Ihre PowerShell-Sitzung wird für die Verbindungsherstellung mit Azure authentifiziert.

Um eine interaktive Anmeldung mithilfe der [Azure CLI](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest) vorzunehmen, führen Sie den Befehl[az login](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az-login) aus.

```azurecli
az login
```

Die CLI öffnet Ihren Standardbrowser, sofern sie dazu in der Lage ist, und lädt eine Anmeldeseite. Andernfalls müssen Sie eine Browserseite öffnen und die Anweisungen zur Befehlszeile befolgen, um einen Autorisierungscode einzugeben, nachdem Sie in Ihrem Browser zu [https://aka.ms/devicelogin](https://aka.ms/devicelogin) navigiert sind. Melden Sie sich anschließend im Browser mit Ihren Anmeldeinformationen an.

## <a name="deployment-considerations"></a>Überlegungen zur Bereitstellung

Die Richtlinie **Anfordern von MFA für die Dienstverwaltung** betrifft alle Benutzer von Azure Resource Manager. Daher sind verschiedene Aspekte zu berücksichtigen, um eine reibungslose Bereitstellung sicherzustellen. Beispielsweise müssen Benutzer und Dienstprinzipale in Azure AD identifiziert werden, die MFA nicht ausführen dürfen oder sollen, sowie die von Ihrer Organisation verwendeten Anwendungen und Clients, die keine moderne Authentifizierung unterstützen.

## <a name="enable-the-baseline-policy"></a>Aktivieren der Basisrichtlinie

Die **Basisrichtlinie: Anfordern von MFA für die Dienstverwaltung (Vorschau)** ist vorkonfiguriert und wird im Azure-Portal auf dem Blatt „Bedingter Zugriff“ ganz oben angezeigt.

Gehen Sie wie folgt vor, um diese Richtlinie zu aktivieren und Ihre Administratoren zu schützen:

1. Melden Sie sich beim **Azure-Portal** als globaler Administrator, Sicherheitsadministrator oder Administrator für bedingten Zugriff an.
1. Navigieren Sie zu **Azure Active Directory** > **Bedingter Zugriff**.
1. Wählen Sie in der Liste der Richtlinien **Basisrichtlinie: Anfordern von MFA für die Dienstverwaltung (Vorschau)** aus.
1. Legen Sie **Richtlinie aktivieren** auf **Richtlinie sofort verwenden** fest.
1. Klicken Sie auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter

* [Conditional Access grundlegende Schutzrichtlinien](concept-baseline-protection.md)
* [Fünf Schritte zum Sichern Ihrer Identitätsinfrastruktur](../../security/fundamentals/steps-secure-identity.md)
* [Was ist Conditional Access in Azure Active Directory?](overview.md)
---
title: Identity Protection und B2B-Benutzer – Azure Active Directory
description: Verwenden von Identity Protection mit B2B-Benutzern – Funktionsweise und bekannte Einschränkungen
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 10/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 95ea7eb470a5880bc88b3df903d33854f363e974
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72881311"
---
# <a name="identity-protection-and-b2b-users"></a>Identity Protection und B2B-Benutzer

Azure AD B2B-Zusammenarbeit bietet Organisationen die Möglichkeit, risikobasierte Richtlinien für B2B-Benutzer mithilfe von Identity Protection zu erzwingen. Diese Richtlinien können auf zwei Arten konfiguriert werden:

- Administratoren können die integrierten risikobasierten Richtlinien von Identity Protection konfigurieren, die für alle Apps mit Gastbenutzern gelten.
- Administratoren können eigene Richtlinien für den bedingten Zugriff konfigurieren und das Anmelderisiko als Bedingung verwenden, die auch Gastbenutzer umfasst.

## <a name="how-is-risk-evaluated-for-b2b-collaboration-users"></a>Auswertung des Risikos für Benutzer von B2B-Zusammenarbeit

Das Benutzerrisiko für Benutzer von B2B-Zusammenarbeit wird in deren Basisverzeichnis ausgewertet. Das Echtzeit-Anmelderisiko für diese Benutzer wird im Ressourcenverzeichnis ausgewertet, wenn sie versuchen, auf die Ressource zuzugreifen.

## <a name="limitations-of-identity-protection-for-b2b-collaboration-users"></a>Einschränkungen von Identity Protection für Benutzer von B2B-Zusammenarbeit

Es gibt Einschränkungen bei der Implementierung von Identity Protection für Benutzer von B2B-Zusammenarbeit in einem Ressourcenverzeichnis, weil sich ihre Identität im eigenen Basisverzeichnis befindet. Die wichtigsten Einschränkungen sind folgende:

- Wenn ein Gastbenutzer die Richtlinie zum Benutzerrisiko von Identity Protection auslöst, um eine Kennwortzurücksetzung zu erzwingen, **wird er gesperrt**. Diese Sperrung ist darauf zurückzuführen, dass Kennwörter nicht im Ressourcenverzeichnis zurückgesetzt werden können.
- **Gastbenutzer werden im Bericht „Riskante Benutzer“ nicht angezeigt**. Dieser Sichtbarkeitsverlust ist darauf zurückzuführen, dass die Risikobewertung im Basisverzeichnis des B2B-Benutzers erfolgt.
- Administratoren **können einen riskanten Benutzer von B2B-Zusammenarbeit in ihrem Ressourcenverzeichnis nicht verwerfen oder bereinigen**. Dieser Funktionsverlust ist darauf zurückzuführen, dass Administratoren im Ressourcenverzeichnis keinen Zugriff auf das Basisverzeichnis des B2B-Benutzers haben.

### <a name="why-cant-i-remediate-risky-b2b-collaboration-users-in-my-directory"></a>Warum kann ich riskante Benutzer von B2B-Zusammenarbeit in meinem Verzeichnis nicht bereinigen?

Die Risikobewertung und Bereinigung für B2B-Benutzer erfolgt in deren Basisverzeichnis. Aus diesem Grund werden die Gastbenutzer im Bericht „Riskante Benutzer“ im Ressourcenverzeichnis nicht angezeigt und Administratoren im Ressourcenverzeichnis können keine sichere Kennwortzurücksetzung für diese Benutzer erzwingen.

### <a name="what-do-i-do-if-a-b2b-collaboration-user-was-blocked-due-to-a-risk-based-policy-in-my-organization"></a>Was kann ich tun, wenn ein Benutzer von B2B-Zusammenarbeit aufgrund einer risikobasierten Richtlinie in meiner Organisation gesperrt wurde?

Wenn ein riskanter B2B-Benutzer in Ihrem Verzeichnis durch die risikobasierte Richtlinie gesperrt wird, muss der Benutzer dieses Risiko im eigenen Basisverzeichnis bereinigen. Benutzer können ihr Risiko bereinigen, indem sie eine sichere Kennwortzurücksetzung im eigenen Basisverzeichnis ausführen. Wenn die Self-Service-Kennwortzurücksetzung im Basisverzeichnis der Benutzer nicht aktiviert ist, müssen diese sich an das IT-Personal der eigenen Organisation wenden, damit ein Administrator das Risiko manuell verwirft oder das Kennwort zurücksetzt.

### <a name="how-do-i-prevent-b2b-collaboration-users-from-being-impacted-by-risk-based-policies"></a>Wie verhindere ich, dass Benutzer von B2B-Zusammenarbeit durch risikobasierte Richtlinien betroffen sind?

Durch das Ausschließen von B2B-Benutzern aus den risikobasierten Richtlinien für den bedingten Zugriff Ihrer Organisation wird verhindert, dass B2B-Benutzer von der Risikobewertung betroffen sind oder dadurch gesperrt werden. Zum Ausschließen dieser B2B-Benutzer erstellen Sie eine Gruppe in Azure AD, die alle Gastbenutzer Ihrer Organisation enthält. Fügen Sie diese Gruppe anschließend als Ausschlusselement für Ihre integrierten Identity Protection-Richtlinien zum Benutzerrisiko und zum Anmelderisiko sowie für alle Richtlinien für bedingten Zugriff zu, die das Anmelderisiko als Bedingung verwenden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Artikel zur Azure AD B2B-Zusammenarbeit:

- [Was ist die Azure AD B2B-Zusammenarbeit?](../b2b/what-is-b2b.md)

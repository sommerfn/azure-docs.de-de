---
title: Empfohlene Sicherheitsmaßnahmen
description: Bei der Verwendung der delegierten Azure-Ressourcenverwaltung ist es wichtig, die Sicherheits-und Zugriffssteuerung zu beachten.
author: JnHs
ms.service: lighthouse
ms.author: jenhayes
ms.date: 07/11/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: 08a54313885c7d38117c242f01c2780796f38a08
ms.sourcegitcommit: e0a1a9e4a5c92d57deb168580e8aa1306bd94723
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72286183"
---
# <a name="recommended-security-practices"></a>Empfohlene Sicherheitsmaßnahmen

Bei der Verwendung der delegierten Azure-Ressourcenverwaltung ist es wichtig, die Sicherheits-und Zugriffssteuerung zu beachten. Benutzer in Ihrem Mandanten haben direkten Zugriff auf Kundenabonnements und Ressourcengruppen, weshalb Sie Maßnahmen ergreifen sollten, um die Sicherheit Ihres Mandanten aufrechtzuerhalten. Außerdem sollten Sie sicherstellen, dass Sie nur den Zugriff zulassen, der für die effektive Verwaltung der Ressourcen ihrer Kunden erforderlich ist. Dieses Thema enthält Empfehlungen, die Ihnen dabei helfen sollen.

## <a name="require-azure-multi-factor-authentication"></a>Anfordern von Azure Multi-Factor Authentication

[Azure Multi-Factor Authentication](../../active-directory/authentication/concept-mfa-howitworks.md) (auch als zweistufige Überprüfung bezeichnet) trägt dazu bei, Angreifer daran zu hindern, Zugriff auf ein Konto zu erlangen, indem mehrere Authentifizierungsschritte erforderlich sind. Sie sollten Multi-Factor Authentication für alle Benutzer in Ihrem Dienstanbietermandanten anfordern, einschließlich aller Benutzer, die Zugriff auf Kundenressourcen haben werden.

Wir schlagen vor, dass Sie Ihre Kunden auffordern, Azure Multi-Factor Authentication auch in ihren Mandanten zu implementieren.

## <a name="assign-permissions-to-groups-using-the-principle-of-least-privilege"></a>Zuweisen von Berechtigungen an Gruppen unter Verwendung des Prinzips der geringsten Rechte

Um die Verwaltung zu vereinfachen, empfiehlt es sich, Azure AD-Benutzergruppen für jede Rolle zu verwenden, die erforderlich ist, um die Ressourcen ihrer Kunden zu verwalten. Auf diese Weise können Sie der Gruppe einzelne Benutzer nach Bedarf hinzufügen oder diese daraus entfernen, anstatt dem jeweiligen Benutzer Berechtigungen direkt zuzuweisen.

Beachten Sie beim Erstellen Ihrer Berechtigungsstruktur, dass Sie das Prinzip der geringsten Rechte befolgen, damit Benutzer nur über die Berechtigungen verfügen, die zum Durchführen Ihrer Aufgaben erforderlich sind, um die Wahrscheinlichkeit von unbeabsichtigten Fehlern zu verringern.

Beispielsweise könnten Sie eine Struktur wie die folgende verwenden:

|Gruppenname  |type  |principalId  |Rollendefinition  |Rollendefinitions-ID  |
|---------|---------|---------|---------|---------|
|Architekten     |Benutzergruppe         |\<principalId\>         |Mitwirkender         |b24988ac-6180-42a0-ab88-20f7382dd24c  |
|Bewertung     |Benutzergruppe         |\<principalId\>         |Leser         |acdd72a7-3385-48ef-bd42-f606fba81ae7  |
|VM-Spezialisten     |Benutzergruppe         |\<principalId\>         |VM-Mitwirkender         |9980e02c-c2be-4d73-94e8-173b1dc7cf3c  |
|Automation     |Dienstprinzipalname (SPN)         |\<principalId\>         |Mitwirkender         |b24988ac-6180-42a0-ab88-20f7382dd24c  |

Nachdem Sie diese Gruppen erstellt haben, können Sie Benutzer nach Bedarf zuweisen. Fügen Sie nur die Benutzer hinzu, die wirklich Zugriff benötigen. Achten Sie darauf, dass Sie die Gruppenmitgliedschaft regelmäßig überprüfen und alle Benutzer entfernen, die nicht mehr angemessen oder erforderlich sind.

Bedenken Sie, dass jede Gruppe (bzw. jeder Benutzer oder Dienstprinzipal), wenn Sie [das Onboarding von Kunden über ein öffentliches verwaltetes Dienstangebot durchführen](../how-to/publish-managed-services-offers.md), die Sie einschließen, über dieselben Berechtigungen für jeden Kunden verfügt, der den Plan kauft. Um verschiedene Gruppen für die Arbeit mit den einzelnen Kunden zuzuweisen, müssen Sie einen gesonderten privaten Plan veröffentlichen, der exklusiv für jeden Kunden ist, oder Sie müssen Kunden einzeln aufnehmen, indem Sie Azure Resource Manager-Vorlagen verwenden. Beispielsweise könnten Sie einen öffentlichen Plan veröffentlichen, der nur über sehr eingeschränkten Zugriff verfügt, und dann mit dem Kunden direkt zusammenarbeiten, um dessen Ressourcen für zusätzlichen Zugriff zu integrieren, indem Sie eine angepasste Azure Resource Manager-Vorlage verwenden, die nach Bedarf zusätzlichen Zugriff gewährt.


## <a name="next-steps"></a>Nächste Schritte

- [Bereitstellen der Azure Multi-Factor Authentication](../../active-directory/authentication/howto-mfa-getstarted.md).
- Erfahren Sie über [Mandantenübergreifende Verwaltungsmöglichkeiten](cross-tenant-management-experience.md).

---
title: Was ist die Azure AD-Berechtigungsverwaltung? (Vorschau) – Azure Active Directory
description: Verschaffen Sie sich einen Überblick über die Berechtigungsverwaltung von Azure Active Directory, und lesen Sie, wie Sie mit ihr den Zugriff auf Gruppen, Anwendungen und SharePoint Online-Websites für interne und externe Benutzer verwalten können.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 09/03/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: a467856550bf2deaab931b3fe2f54b7986f12f8a
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72430298"
---
# <a name="what-is-azure-ad-entitlement-management-preview"></a>Was ist die Azure AD-Berechtigungsverwaltung? (Vorschau)

> [!IMPORTANT]
> Die Berechtigungsverwaltung von Azure Active Directory (Azure AD) befindet sich derzeit in der öffentlichen Vorschau.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Mitarbeiter in Organisationen benötigen für ihre Aufgaben Zugriff auf verschiedene Gruppen, Anwendungen und Websites. Die Verwaltung dieses Zugriffs ist nicht einfach. In den meisten Fällen gibt es keine strukturierte Liste aller Ressourcen, die ein Benutzer für ein Projekt benötigt. Der Projektmanager weiß über die erforderlichen Ressourcen, die beteiligten Personen und die Projektdauer Bescheid. Jedoch verfügt der Projektmanager in der Regel nicht über Berechtigungen zum Genehmigen oder Verweigern des Zugriffs anderer Personen. Dieses Szenario wird komplizierter, wenn Sie versuchen, mit externen Personen oder Unternehmen zusammenzuarbeiten.

Die Berechtigungsverwaltung von Azure Active Directory (Azure AD) kann Ihnen helfen, den Zugriff auf Gruppen, Anwendungen und SharePoint Online-Websites für interne Benutzer und Benutzer außerhalb der Organisation zu verwalten.

Dieses Video bietet einen Überblick über die Berechtigungsverwaltung und ihren geschäftlichen Nutzen:

>[!VIDEO https://www.youtube.com/embed/_Lss6bFrnQ8]

## <a name="why-use-entitlement-management"></a>Argumente für die Berechtigungsverwaltung

Die Verwaltung des Zugriffs auf Ressourcen stellt Unternehmen häufig vor Herausforderungen:

- Benutzer wissen möglicherweise nicht, welchen Zugriff sie benötigen.
- Benutzern bereitet es möglicherweise Schwierigkeiten, die richtigen Personen oder Ressourcen zu finden.
- Wenn Benutzer den richtigen Zugriff auf eine Ressource erhalten haben, verfügen sie möglicherweise länger über diesen Zugriff, als für die Geschäftszwecke erforderlich ist.

Diese Probleme sind für Benutzer noch größer, die Zugriff aus einem anderen Verzeichnis benötigen, z.B. externe Benutzer in Lieferkettenorganisationen oder bei Geschäftspartnern. Beispiel:

- Organisationen kennen möglicherweise nicht alle der Personen in anderen Verzeichnissen und können die unbekannten Personen nicht einladen.
- Selbst wenn Organisationen diese Benutzer einladen können, versäumen sie es möglicherweise, den Zugriff sämtlicher Benutzer einheitlich zu verwalten.

Diese Herausforderungen können mit der Azure AD-Berechtigungsverwaltung leichter bewältigt werden.

## <a name="what-can-i-do-with-entitlement-management"></a>Welche Möglichkeiten bietet mir die Berechtigungsverwaltung?

Einige Funktionen der Berechtigungsverwaltung:

- Erstellen von Paketen verwandter Ressourcen, die Benutzer anfordern können
- Definieren von Regeln für das Anfordern von Ressourcen und den Ablauf des Zugriffs
- Steuern des Lebenszyklus für den Zugriff interner und externer Benutzer
- Delegieren der Verwaltung von Ressourcen
- Festlegen von genehmigenden Personen für Anforderungen
- Erstellen von Berichten zum Nachverfolgen des Verlaufs

Einen Überblick über Identitätsgovernance und Berechtigungsverwaltung bietet Ihnen das folgende Video von der Ignite 2018:

>[!VIDEO https://www.youtube.com/embed/aY7A0Br8u5M]

## <a name="what-resources-can-i-manage"></a>Welche Ressourcen kann ich verwalten?

Sie können mit der Berechtigungsverwaltung die folgenden Typen von Ressourcen verwalten:

- Azure AD-Sicherheitsgruppen
- Office 365-Gruppen
- Azure AD-Enterprise-Anwendungen einschließlich SaaS-Anwendung und benutzerdefinierter integrierter Anwendungen, die Verbund oder Bereitstellung unterstützen
- SharePoint Online-Websitesammlungen und -Websites

Sie können auch den Zugriff auf andere Ressourcen steuern, die auf Azure AD-Sicherheitsgruppen oder Office 365-Gruppen basieren.  Beispiel:

- Sie können Benutzerlizenzen für Microsoft Office 365 mit einer Azure AD-Sicherheitsgruppe in einem Zugriffspaket und Konfigurieren der [gruppenbasierten Lizenzierung](../users-groups-roles/licensing-groups-assign.md) für diese Gruppe vergeben.
- Sie können Benutzern mithilfe einer Azure AD-Sicherheitsgruppe in einem Zugriffspaket und Erstellen einer [Azure-Rollenzuweisung](../../role-based-access-control/role-assignments-portal.md) für diese Gruppe Zugriff zum Verwalten von Azure-Ressourcen erteilen.

## <a name="what-are-access-packages-and-policies"></a>Was sind Zugriffspakete und Zugriffspaketrichtlinien?

Die Berechtigungsverwaltung beinhaltet das Konzept von *Zugriffspaketen*. Ein Zugriffspaket ist ein Bündel aller Ressourcen, die ein Benutzer benötigt, um an einem Projekt zu arbeiten oder seine Aufgaben auszuführen. Die Ressourcen umfassen den Zugriff auf Gruppen, Anwendungen oder Websites. Zugriffspakete dienen zum Steuern des Zugriffs für die internen Mitarbeiter und auch für Benutzer außerhalb Ihrer Organisation. Zugriffspakete sind in Containern definiert, die als *Kataloge* bezeichnet werden.

Zugriffspakete enthalten außerdem eine oder mehrere *Richtlinien*. In einer Richtlinie sind die Regeln oder Leitlinien für den Zugriff auf ein Zugriffspaket definiert. Das Aktivieren eine Richtlinie erzwingt, dass nur den richtigen Benutzern Zugriff auf die richtigen Ressourcen und während des richtigen Zeitraums gewährt wird.

![Zugriffspakete und Zugriffspaketrichtlinien](./media/entitlement-management-overview/elm-overview-access-package.png)

Mit einem Zugriffspaket und seinen Richtlinien definiert der Zugriffspaketmanager Folgendes:

- Ressourcen
- Die Rollen, die Benutzer für die Ressourcen benötigen
- Interne Benutzer und Partnerorganisationen externer Benutzer, die zum Anfordern von Zugriff berechtigt sind
- Den Genehmigungsprozess und die Benutzer, die den Zugriff genehmigen oder verweigern können
- Die Dauer des Zugriffs durch den Benutzer

Im folgenden Diagramm wird ein Beispiel für die verschiedenen Elemente der Berechtigungsverwaltung gezeigt. In ihm werden zwei Beispielzugriffspakete dargestellt.

- **Zugriffspaket 1** enthält als Ressource eine einzelne Gruppe. Der Zugriff wird mit einer Richtlinie definiert, die einer Gruppe von Benutzern im Verzeichnis das Anfordern des Zugriffs ermöglicht.
- **Zugriffspaket 2** enthält als Ressourcen eine Gruppe, eine Anwendung und eine SharePoint Online-Website. Der Zugriff wird mit zwei verschiedenen Richtlinien definiert. Die erste Richtlinie ermöglicht einer Gruppe von Benutzern im Verzeichnis das Anfordern des Zugriffs. Die zweite Richtlinie ermöglicht Benutzern in einem externen Verzeichnis das Anfordern des Zugriffs.

![Übersicht über die Berechtigungsverwaltung](./media/entitlement-management-overview/elm-overview.png)

## <a name="terminology"></a>Begriff

Machen Sie sich zum besseren Verständnis der Berechtigungsverwaltung und der dazugehörigen Dokumentation mit den folgenden Begriffen vertraut.

| Begriff oder Konzept | BESCHREIBUNG |
| --- | --- |
| Berechtigungsverwaltung | Ein Dienst, der Zugriffspakete zuweist, widerruft und verwaltet. |
| Zugriffspaket | Ein Ressourcenpaket, das von einem Team oder Projekt benötigt wird und Richtlinien unterliegt. Zugriffspakete sind immer in einem Katalog enthalten. |
| Zugriffsanforderung | Die Anforderung des Zugriffs auf die Ressourcen in einem Zugriffspaket. Eine Anforderung durchläuft in der Regel einen Workflow. |
| policy | Ein Satz von Regeln, die den Zugriffslebenszyklus definieren. Sie legen beispielsweise fest, wie Benutzer Zugriff erhalten, wer den Zugriff genehmigen darf und wie lange Benutzer Zugriff haben. Beispielrichtlinien umfassen Mitarbeiterzugriff und externen Zugriff. |
| catalog | Ein Container verwandter Ressourcen und Zugriffspakete. |
| Katalog „Allgemein“ | Ein integrierter Katalog, der immer verfügbar ist. Um dem Katalog „Allgemein“ Ressourcen hinzuzufügen, sind bestimmte Berechtigungen erforderlich. |
| resource | Ein Asset oder Dienst (z.B. eine Office-Gruppe, eine Sicherheitsgruppe, eine Anwendung oder eine SharePoint Online-Website), für den einem Benutzer Berechtigungen erteilt werden können. |
| Ressourcentyp | Der Typ der Ressource, z.B. Gruppen, Anwendungen und SharePoint Online-Websites. |
| Ressourcenrolle | Eine Sammlung von Berechtigungen, die einer Ressource zugeordnet sind. |
| Ressourcenverzeichnis | Ein Verzeichnis, das mindestens eine Ressource enthält, die freigegeben (geteilt) werden soll. |
| Zugewiesene Benutzer | Eine Zuweisung eines Zugriffspakets für einen Benutzer, damit der Benutzer über alle Ressourcenrollen dieses Zugriffspakets verfügt. |
| enable | Das Verfügbarmachen eines Zugriffspakets für Anforderungen von Benutzern. |

## <a name="license-requirements"></a>Lizenzanforderungen

[!INCLUDE [Azure AD Premium P2 license](../../../includes/active-directory-p2-license.md)]

Spezielle Clouds, z.B. Azure Government, Azure Deutschland und Azure China 21Vianet, können in dieser Vorschau derzeit nicht verwendet werden.

### <a name="which-users-must-have-licenses"></a>Welche Benutzer benötigen Lizenzen?

Ihr Mandant muss über mindestens so viele Azure AD Premium P2-Lizenzen verfügen wie Sie aktive Mitgliedsbenutzer haben. Aktive Mitglieder in der Berechtigungsverwaltung umfassen die folgenden Benutzer:

- Benutzer, die eine Anforderung für ein Zugriffspaket initiieren oder genehmigen.
- Benutzer, denen ein Zugriffspaket zugewiesen ist. 
- Benutzer, die Zugriffspakete verwalten.

Als Teil der Lizenzen für Mitgliedsbenutzer können Sie auch einer bestimmten Anzahl von Gastbenutzern die Interaktion mit der Berechtigungsverwaltung gestatten. Informationen zum Berechnen der Anzahl von Gastbenutzern, die Sie einbeziehen können, finden Sie im [Leitfaden zur Lizenzierung von Azure Active Directory B2B-Kollaboration](../b2b/licensing-guidance.md).

Informationen zum Zuweisen von Lizenzen zu Ihren Benutzern finden Sie unter [Zuweisen oder Entfernen von Lizenzen im Azure Active Directory-Portal](../fundamentals/license-users-groups.md).

## <a name="next-steps"></a>Nächste Schritte

- [Tutorial: Erstellen Ihres ersten Zugriffspakets](entitlement-management-access-package-first.md)
- [Gängige Szenarios](entitlement-management-scenarios.md)

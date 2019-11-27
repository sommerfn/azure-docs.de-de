---
title: Bereitstellen von Privileged Identity Management (PIM) – Azure AD | Microsoft-Dokumentation
description: Beschreibt die Planung der Bereitstellung von Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: eef096322c8a8cfbf1618447529d46f6fbfd13b1
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74021849"
---
# <a name="deploy-azure-ad-privileged-identity-management-pim"></a>Bereitstellen von Azure AD Privileged Identity Management (PIM)

In dieser Schritt-für-Schritt-Anleitung wird beschrieben, wie Sie die Bereitstellung von Privileged Identity Management (PIM) in Ihrer Azure Active Directory-Organisation (Azure AD-Organisation) planen können.

> [!TIP]
> In diesem Artikel sind einige Elemente folgendermaßen gekennzeichnet:
> 
> :heavy_check_mark: **Microsoft-Empfehlung**
> 
> Hierbei handelt es sich um allgemeine Empfehlungen, die nur umgesetzt werden sollten, wenn sie Ihren individuellen Unternehmensanforderungen entsprechen.

## <a name="learn-about-privileged-identity-management"></a>Informationen zu Privileged Identity Management

Azure AD Privileged Identity Management unterstützt Sie beim Verwalten von privilegierten Administratorrollen in Azure AD, Azure-Ressourcen und anderen Microsoft-Onlinediensten. In einer Welt, in der privilegierte Identitäten zugewiesen und vergessen werden, bietet Privileged Identity Management Lösungen wie Just-In-Time-Zugriff, Workflows für die Anforderungsgenehmigung und vollständig integrierte Zugriffsüberprüfungen, damit Sie schädliche Aktivitäten von privilegierten Rollen in Echtzeit identifizieren, aufdecken und verhindern können. Durch die Bereitstellung von Privileged Identity Management zum Verwalten Ihrer privilegierten Rollen in der gesamten Organisation können Sie Risiken minimieren und gleichzeitig wertvolle Einblicke in die Aktivitäten Ihrer privilegierten Rollen erhalten.

### <a name="business-value-of-privileged-identity-management"></a>Geschäftlicher Nutzen von Privileged Identity Management

**Risikomanagement**: Sichern Sie Ihre Organisation durch Erzwingen des Prinzip des [Zugriffs mit den geringsten Rechten](/windows-server/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models) und Just-in-Time-Zugriff. Da die Anzahl von dauerhaften Zuweisungen von Benutzern an privilegierte Rollen minimiert und Genehmigungen und mehrstufige Authentifizierung für erhöhte Rechte erzwungen werden, können Sie Sicherheitsrisiken im Zusammenhang mit privilegiertem Zugriff in Ihrer Organisation deutlich reduzieren. Das Erzwingen der geringsten Rechte und von Just-In-Time-Zugriff ermöglicht Ihnen auch das Anzeigen eines Verlaufs des Zugriffs auf privilegierte Rollen und das Ausfindigmachen von Sicherheitsproblemen, während sie auftreten.

**Umsetzung von Compliance und Governance**: Durch die Bereitstellung von Privileged Identity Management wird eine Umgebung für kontinuierliche Identitätsgovernance geschaffen. Durch Just-In-Time-Rechteerweiterungen für privilegierte Identitäten kann Privileged Identity Management privilegierte Zugriffsaktivitäten in Ihrer Organisation nachverfolgen. Außerdem können Sie Benachrichtigungen für alle Zuweisungen von dauerhaften und berechtigten Rollen innerhalb Ihrer Organisation anzeigen und erhalten. Anhand der Zugriffsüberprüfung können Sie unnötige privilegierte Identitäten regelmäßig überprüfen und entfernen und sicherstellen, dass Ihre Organisation die strengsten Identität-, Zugriffs- und Sicherheitstandards einhält.

**Kostensenkung**: Reduzieren Sie Kosten, indem Ineffizienzen, menschliche Fehler und Sicherheitsprobleme durch die korrekte Bereitstellung von Privileged Identity Management beseitigt werden. Das Endergebnis ist eine Reduzierung von Cyberverbrechen im Zusammenhang mit privilegierten Identitäten, die eine kostspielige und schwierige Wiederherstellung erfordern. Privileged Identity Management unterstützt Ihre Organisation auch beim Senken von Kosten für die Überprüfung von Zugriffsinformationen, wenn es um die Einhaltung von Vorschriften und Standards geht.

Weitere Informationen finden Sie unter [Was ist Azure AD Privileged Identity Management?](pim-configure.md).

### <a name="licensing-requirements"></a>Lizenzanforderungen

Um Privileged Identity Management verwenden zu können, muss Ihr Verzeichnis über eine der folgenden kostenpflichtigen Lizenzen oder Testlizenzen verfügen:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5
- Microsoft 365 M5

Weitere Informationen finden Sie unter [Lizenzanforderungen für die Verwendung von PIM](subscription-requirements.md).

### <a name="key-terminology"></a>Wesentliche Terminologie

| Begriff oder Konzept | BESCHREIBUNG |
| --- | --- |
| Berechtigt | Eine Rollenzuweisung, bei der ein Benutzer mindestens eine Aktion ausführen muss, um die Rolle nutzen zu können. Wenn ein Benutzer zu einer Rolle berechtigt ist, kann er die Rolle aktivieren, wenn er privilegierte Aufgaben ausführen muss. Es gibt keinen Unterschied hinsichtlich des Zugriffs zwischen einer permanenten und einer berechtigten Rollenzuweisung. Der einzige Unterschied ist, dass einige Benutzer den Zugriff nicht jederzeit benötigen. |
| aktivieren | Das Ausführen mindestens einer Aktion zum Verwenden einer Rolle, zu der der Benutzer berechtigt ist. Beispiele für Aktionen sind eine erfolgreiche Multi-Factor Authentication-Überprüfung (MFA), die Angabe einer geschäftlichen Begründung oder das Anfordern einer Genehmigung von den angegebenen genehmigenden Personen. |
| Just-in-Time-Zugriff (JIT) | Ein Modell, bei dem Benutzer temporäre Berechtigungen zum Ausführen privilegierter Aufgaben erhalten. Dieses Modell verhindert, dass böswillige oder nicht autorisierte Benutzer nach dem Ablauf der Berechtigungen Zugriff erhalten. Der Zugriff wird nur gewährt, wenn Benutzer ihn benötigen. |
| Prinzip des Zugriffs mit den geringsten Rechten | Eine empfohlene Sicherheitsmethode, bei der alle Benutzer nur die zum Ausführen der Aufgaben, für die sie autorisiert sind, mindestens erforderlichen Berechtigungen erhalten. Diese Methode minimiert die Anzahl von globalen Administratoren, indem stattdessen spezifische Administratorrollen für bestimmte Szenarien verwendet werden. |

Weitere Informationen finden Sie unter [Terminologie](pim-configure.md#terminology).

### <a name="high-level-overview-of-how-privileged-identity-management-works"></a>Allgemeine Übersicht über die Funktionsweise von Privileged Identity Management

1. Privileged Identity Management wird eingerichtet, damit Benutzer für privilegierte Rollen berechtigt sind.
1. Wenn ein berechtigter Benutzer seine privilegierte Rolle verwenden muss, aktiviert er die Rolle in Privileged Identity Management.
1. Je nach den für die Rolle konfigurierten Privileged Identity Management-Einstellungen muss der Benutzer bestimmte Schritte ausführen (z. B. die mehrstufige Authentifizierung durchführen, eine Genehmigung einholen oder einen Grund angeben).
1. Nachdem der Benutzer seine Rolle erfolgreich aktiviert hat, kann er sie während eines vorkonfigurierten Zeitraums nutzen.
1. Administratoren können einen Verlauf aller Privileged Identity Management-Aktivitäten im Überwachungsprotokoll anzeigen. Außerdem können Administratoren mithilfe von Privileged Identity Management-Funktionen wie Zugriffsüberprüfungen und Warnungen ihre Azure AD-Organisationen noch besser schützen und Complianceanforderungen einhalten.

Weitere Informationen finden Sie unter [Was ist Azure AD Privileged Identity Management?](pim-configure.md).

### <a name="roles-that-can-be-managed-by-privileged-identity-management"></a>Von Privileged Identity Management verwaltbare Rollen

**Azure AD-Rollen**: Dabei handelt es sich um Rollen in Azure Active Directory (z. B. „Globaler Administrator“, „Exchange-Administrator“ und „Sicherheitsadministrator“). Unter [Berechtigungen der Administratorrolle in Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md) erfahren Sie mehr über die Rollen und ihre Funktionen. [Administratorrollen nach Administratoraufgabe in Azure Active Directory](../users-groups-roles/roles-delegate-by-task.md) hilft Ihnen dabei, Ihren Administratoren geeignete Rollen zuzuweisen.

**Azure-Ressourcenrollen**: Diese Rollen sind mit einer Azure-Ressource, einer Ressourcengruppe, einem Abonnement oder einer Verwaltungsgruppe verknüpft. Privileged Identity Management bietet Just-in-Time-Zugriff auf integrierte Rollen wie „Besitzer“, „Benutzerzugriffsadministrator“ und „Mitwirkender“ sowie auf [benutzerdefinierte Rollen](../../role-based-access-control/custom-roles.md). Weitere Informationen zu Azure-Ressourcenrollen finden Sie unter [Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)](../../role-based-access-control/overview.md).

Weitere Informationen finden Sie unter [Nicht in PIM verwaltbare Rollen](pim-roles.md).

## <a name="plan-your-deployment"></a>Planen der Bereitstellung

Dieser Abschnitt konzentriert sich auf die Aufgaben, die Sie vor dem Bereitstellen von Privileged Identity Management in Ihrer Organisation ausführen müssen. Das Befolgen der Anweisungen und das Verstehen der Konzepte in diesem Abschnitt ist wichtig, da es sich um einen Leitfaden zum Erstellen des besten Plans speziell für die privilegierten Identitäten Ihrer Organisation handelt.

### <a name="identify-your-stakeholders"></a>Identifizieren der Beteiligten

Der folgende Abschnitt hilft Ihnen beim Identifizieren aller Projektbeteiligten, die genehmigen, überprüfen oder informiert bleiben müssen. Er enthält separate Tabellen für die Bereitstellung von Privileged Identity Management für Azure AD-Rollen und Privileged Identity Management für Azure-Ressourcenrollen. Fügen Sie der folgenden Tabelle je nach Bedarf Projektbeteiligte für Ihre Organisation hinzu.

- SO = Dieses Projekt genehmigen
- R = Dieses Projekt überprüfen und Beiträge bereitstellen
- I = Über dieses Projekt informiert

#### <a name="stakeholders-privileged-identity-management-for-azure-ad-roles"></a>Beteiligte: Privileged Identity Management für Azure AD-Rollen

| NAME | Role | Aktion |
| --- | --- | --- |
| Name und E-Mail-Adresse | **Identitätsarchitekt oder globaler Azure-Administrator**<br/>Ein Vertreter des Identitätsverwaltungsteams, der definiert, wie diese Änderung an der Kerninfrastruktur für die Identitätsverwaltung in Ihrer Organisation ausgerichtet ist. | SO/R/I |
| Name und E-Mail-Adresse | **Dienstbesitzer/Linienmanager**<br/>Ein Vertreter der IT-Besitzer eines Diensts oder eine Gruppe von Diensten. Er ist der Hauptentscheidungsträger und in erster Linie verantwortlich für die Einführung von Privileged Identity Management für sein Team. | SO/R/I |
| Name und E-Mail-Adresse | **Sicherheitsbesitzer**<br/>Ein Vertreter des Sicherheitsteams, der abzeichnen kann, dass der Plan die Sicherheitsanforderungen Ihrer Organisation erfüllt. | SO/R |
| Name und E-Mail-Adresse | **IT-Supportmanager/Helpdesk**<br/>Ein Vertreter der IT-Supportorganisation, der zu den Unterstützungsmöglichkeiten dieser Änderung aus einer Helpdesk-Perspektive Stellung nehmen kann. | R/I |
| Name und E-Mail-Adresse für Pilotbenutzer | **Benutzer mit privilegierter Rolle**<br/>Die Gruppe von Benutzern, für die privilegierte Identitätsverwaltung implementiert wird. Sie müssen wissen, wie ihre Rollen nach der Implementierung von Privileged Identity Management aktiviert werden. | I |

#### <a name="stakeholders-privileged-identity-management-for-azure-resource-roles"></a>Beteiligte: Privileged Identity Management für Azure-Ressourcenrollen

| NAME | Role | Aktion |
| --- | --- | --- |
| Name und E-Mail-Adresse | **Abonnement/Ressourcenbesitzer**<br/>Ein Vertreter der IT-Besitzer jedes Abonnements oder jeder Ressource, für das oder die Sie Privileged Identity Management bereitstellen möchten | SO/R/I |
| Name und E-Mail-Adresse | **Sicherheitsbesitzer**<br/>Ein Vertreter des Sicherheitsteams, der abzeichnen kann, dass der Plan die Sicherheitsanforderungen Ihrer Organisation erfüllt. | SO/R |
| Name und E-Mail-Adresse | **IT-Supportmanager/Helpdesk**<br/>Ein Vertreter der IT-Supportorganisation, der zu den Unterstützungsmöglichkeiten dieser Änderung aus einer Helpdesk-Perspektive Stellung nehmen kann. | R/I |
| Name und E-Mail-Adresse für Pilotbenutzer | **Benutzer mit RBAC-Rolle**<br/>Die Gruppe von Benutzern, für die privilegierte Identitätsverwaltung implementiert wird. Sie müssen wissen, wie ihre Rollen nach der Implementierung von Privileged Identity Management aktiviert werden. | I |

### <a name="enable-privileged-identity-management"></a>Aktivieren von Privileged Identity Management

Im Rahmen des Planungsprozesses müssen Sie zuerst Privileged Identity Management zustimmen und aktivieren. Folgen Sie dazu den Anweisungen im Artikel [Einstieg in die Verwendung von PIM](pim-getting-started.md). Durch das Aktivieren von Privileged Identity Management erhalten Sie Zugriff auf einige Funktionen, die speziell für die Unterstützung Ihrer Bereitstellung vorgesehen sind.

Wenn Sie Privileged Identity Management für Azure-Ressourcen bereitstellen möchten, sollten Sie den Anweisungen im Artikel [Ermitteln von Azure-Ressourcen zur Verwaltung in PIM](pim-resource-roles-discover-resources.md) folgen. Nur Besitzer von Ressourcen, Ressourcengruppen und Abonnements können diese in Privileged Identity Management ermitteln. Wenn Sie als globaler Administrator versuchen, Privileged Identity Management für Azure-Ressourcen bereitzustellen, können Sie [die Zugriffsrechte zum Verwalten aller Azure-Abonnements erhöhen](../../role-based-access-control/elevate-access-global-admin.md?toc=%2fazure%2factive-directory%2fprivileged-identity-management%2ftoc.json), um sich selbst Zugriff auf alle Azure-Ressourcen im Verzeichnis für die Ermittlung zu erteilen. Wir raten jedoch dazu, die Genehmigung aller Ihrer Abonnementbesitzer einzuholen, bevor Sie deren Ressourcen mit Privileged Identity Management verwalten.

### <a name="enforce-principle-of-least-privilege"></a>Durchsetzen des Prinzips der geringsten Rechte

Stellen Sie unbedingt sicher, dass Sie das Prinzip der geringsten Rechte in Ihrer Organisation für Ihre Azure AD-Rollen und Ihre Azure-Ressourcenrollen durchgesetzt haben.

#### <a name="azure-ad-roles"></a>Azure AD-Rollen

Für Azure AD-Rollen weisen Organisation die Rolle „Globaler Administrator“ häufig einer großen Anzahl von Administratoren zu, obwohl die meisten Administratoren nur eine oder zwei bestimmte Administratorrollen benötigen. Auch die Zuweisung von privilegierten Rollen wird häufig nicht verwaltet.

> [!NOTE]
> Häufige Probleme bei der Rollendelegierung:
>
> - Der für Exchange verantwortliche Administrator erhält die Rolle „Globaler Administrator“, obwohl er für seine täglichen Aufgaben nur die Rolle „Exchange-Administrator“ benötigt.
> - Die Rolle „Globale Administrator“ wird einem Office-Administrator zugewiesen, da der Administrator, sowohl Sicherheits- als auch SharePoint-Administratorrollen benötigt und es einfacher ist, nur eine Rolle zu delegieren.
> - Dem Administrator wurde zum Ausführen einer Aufgabe vor längerer Zeit eine Sicherheitsadministratorrolle zugewiesen, die nicht mehr entfernt wurde.

Führen Sie diese Schritte aus, um das Prinzip der geringsten Rechte für Ihre Azure AD-Rollen zu erzwingen.

1. Einblicke in die Granularität der Rollen erhalten Sie durch Lesen und Verstehen von [Verfügbare Rollen](../users-groups-roles/directory-assign-admin-roles.md#available-roles). Sie und Ihr Team sollten außerdem den Artikel [Administratorrollen nach Administratoraufgabe in Azure Active Directory](../users-groups-roles/roles-delegate-by-task.md) lesen, in dem die am wenigsten privilegierte Rolle für bestimmte Aufgaben erläutert wird.

1. Erstellen Sie eine Liste der Benutzer mit privilegierten Rollen in Ihrer Organisation. Mit dem [Privileged Identity Management-Assistenten](pim-security-wizard.md#run-the-wizard) können Sie eine Seite wie die folgende erstellen.

    ![Bereich zum Ermitteln privilegierter Rollen, der anzeigt, wer über privilegierte Rollen verfügt](./media/pim-deployment-plan/discover-privileged-roles-users.png)

1. Ermitteln Sie für alle globalen Administratoren in Ihrer Organisation, warum sie die Rolle benötigen. Basierend auf der bisherigen Dokumentation sollten Sie eine Person aus der Rolle „Globaler Administrator“ entfernen, wenn deren Aufgaben von einer oder mehreren genau abgestimmten Administratorrollen ausgeführt werden kann, und entsprechende Zuweisungen in Azure Active vornehmen. (Bezugswert: Bei Microsoft gibt es derzeit nur ungefähr 10 Administratoren mit der Rolle „Globaler Administrator“. Weitere Informationen finden Sie unter [Verwenden von Azure AD Privileged Identity Management für erhöhte Zugriffsrechte](https://www.microsoft.com/itshowcase/Article/Content/887/Using-Azure-AD-Privileged-Identity-Management-for-elevated-access)).

1. Für alle anderen Azure AD-Rollen überprüfen Sie die Liste der Zuweisungen, identifizieren Administratoren, die die Rolle nicht mehr benötigen, und entfernen diese aus ihren Zuweisungen.

Wenn Sie die letzten beiden Schritte automatisieren möchten, können Sie Zugriffsüberprüfungen in Privileged Identity Management verwenden. Durch Ausführen der unter [Erstellen einer Zugriffsüberprüfung für Azure AD-Rollen in PIM](pim-how-to-start-security-review.md) beschriebenen Schritte können Sie eine Zugriffsüberprüfung für alle Azure AD-Rollen mit mindestens einem Mitglied einrichten.

![Erstellen eines Zugriffsüberprüfungsbereich für Azure AD-Rollen](./media/pim-deployment-plan/create-access-review.png)

Legen Sie die Prüfer auf **Mitglieder (selbst)** selbst. Dadurch wird eine E-Mail an alle Mitglieder der Rolle gesendet, damit diese bestätigen, ob sie den Zugriff benötigen. Außerdem sollten Sie in den erweiterten Einstellung die Option **Bei Genehmigung Grund anfordern** aktivieren, damit Benutzer angeben können, warum sie die Rolle benötigen. Auf der Grundlage dieser Informationen können Sie Benutzer aus unnötigen Rollen entfernen und im Fall von globalen Administratoren genauer abgestimmte Administratorrollen delegieren.

Bei Zugriffsüberprüfungen werden E-Mails gesendet, um Personen dazu aufzufordern, ihren Zugriff auf die Rollen zu überprüfen. Wenn Sie privilegierte Konten haben, mit denen keine E-Mail-Adressen verknüpft sind, achten Sie darauf, für diese Konten das Feld für die sekundäre E-Mail-Adresse auszufüllen. Weitere Informationen finden Sie im Artikel zum [proxyAddresses-Attribut in Azure AD](https://support.microsoft.com/help/3190357/how-the-proxyaddresses-attribute-is-populated-in-azure-ad).

#### <a name="azure-resource-roles"></a>Azure-Ressourcenrollen

Für Azure-Abonnements und _Ressourcen können Sie einen ähnlichen Zugriffsüberprüfungsprozess einrichten, um die Rollen in den einzelnen Abonnements oder Ressourcen zu überprüfen. Das Ziel dieses Prozesses besteht in der Minimierung der Zuweisungen an „Besitzer“ und „Benutzerzugriffsadministrator“ für die einzelnen Abonnements oder Ressourcen sowie in der Entfernung unnötiger Zuweisungen. Organisationen delegieren solche Aufgaben jedoch häufig an den Besitzer eines Abonnements oder einer Ressource, da sie ein besseres Verständnis der bestimmten Rollen (insbesondere benutzerdefinierten Rollen) haben.

Wenn Sie als IT-Administrator mit der Rolle „Globaler Administrator“ versuchen, Privileged Identity Management für Azure-Ressourcen in Ihrer Organisation bereitzustellen, können Sie [die Zugriffsrechte zum Verwalten aller Azure-Abonnements erhöhen](../../role-based-access-control/elevate-access-global-admin.md?toc=%2fazure%2factive-directory%2fprivileged-identity-management%2ftoc.json), um Zugriff auf alle Abonnements zu erhalten. Anschließend können Sie die Besitzer der einzelnen Abonnements ermitteln und in Zusammenarbeit mit ihnen unnötige Zuweisungen entfernen und die Zuweisung der Besitzerrolle minimieren.

Benutzer mit der Rolle „Besitzer“ für ein Azure-Abonnement können auch [Zugriffsüberprüfungen für Azure-Ressourcen](pim-resource-roles-start-access-review.md) nutzen, um unnötige Rollenzuweisungen mit einem ähnlichen Vorgang wie oben für Azure AD-Rollen beschrieben zu überprüfen und zu entfernen.

### <a name="decide-which-role-assignments-should-be-protected-by-privileged-identity-management"></a>Entscheiden, welche Rollenzuweisungen von Privileged Identity Management geschützt werden sollen

Nach dem Bereinigen der Zuweisungen privilegierter Rollen in Ihrer Organisation müssen Sie entscheiden, welche Rollen mit Privileged Identity Management geschützt werden sollen.

Wenn eine Rolle von Privileged Identity Management geschützt wird, müssen berechtigte Benutzer, die ihr zugewiesen sind, die Rechte erhöhen, um die von der Rolle gewährten Berechtigungen zu nutzen. Die Erhöhung der Rechte kann auch das Einholen einer Genehmigung, das Durchführen einer mehrstufigen Authentifizierung und/oder das Angeben eines Grunds für die Aktivierung umfassen. Privileged Identity Management kann Erhöhungen von Rechten auch über Benachrichtigungen sowie über die Privileged Identity Management- und Azure AD-Überwachungsereignisprotokolle nachverfolgen.

Die Auswahl der mit Privileged Identity Management zu schützenden Rollen kann schwierig sein und unterscheidet sich je nach Organisation. Dieser Abschnitt enthält unsere empfohlenen bewährten Methoden für Azure AD-Rollen und Azure-Ressourcenrollen.

#### <a name="azure-ad-roles"></a>Azure AD-Rollen

Das Priorisieren des Schutzes von Azure AD-Rollen mit der höchsten Anzahl von Berechtigungen ist wichtig. Basierend auf den Nutzungsmustern aller Privileged Identity Management-Kunden ergeben sich die folgenden 10 wichtigsten von Privileged Identity Management verwalteten Azure AD-Rollen:

1. Globaler Administrator
1. Sicherheitsadministrator
1. Benutzeradministrator
1. Exchange-Administrator
1. SharePoint-Administrator
1. Intune-Administrator
1. Sicherheitsleseberechtigter
1. Dienstadministrator
1. Rechnungsadministrator
1. Skype for Business-Administrator

> [!TIP]
> :heavy_check_mark: **Microsoft-Empfehlung**: Verwalten Sie zuerst alle globalen Administratoren und Sicherheitsadministratoren mithilfe von Privileged Identity Management, weil diese bei einer Kompromittierung den meisten Schaden anrichten können.

Es ist wichtig zu berücksichtigen, welche Daten und Berechtigungen für Ihre Organisation am sensibelsten sind. Beispielsweise sollten einige Organisationen ihre Power BI-Administratorrolle oder ihre Teams-Administratorrolle mit Privileged Identity Management schützen, weil diese auf Daten zugreifen und/oder zentrale Workflows ändern können.

Wenn Rollen vorhanden sind, denen Gastbenutzer zugewiesen sind, sind sie besonders anfällig für Angriffe.

> [!TIP]
> :heavy_check_mark: **Microsoft-Empfehlung**: Verwalten Sie alle Rollen für Gastbenutzer mit Privileged Identity Management, um Risiken im Zusammenhang mit kompromittierten Gastbenutzerkonten zu reduzieren.

Leserrollen wie „Verzeichnis lesen“, „Nachrichtencenter-Leser“ und „Sicherheitsleseberechtigter“ gelten bisweilen als weniger wichtig als andere Rollen, da sie keine Schreibberechtigung haben. Allerdings ist uns bekannt, dass einige Kunden auch diese Rollen schützen, da Angreifer, die Zugriff auf diese Konten erlangt haben, möglicherweise sensible Daten wie beispielsweise personenbezogene Informationen lesen können. Dies sollten Sie berücksichtigen, wenn Sie entscheiden, ob Leserrollen in Ihrer Organisation mit Privileged Identity Management verwaltet werden müssen.

#### <a name="azure-resource-roles"></a>Azure-Ressourcenrollen

Bei der Entscheidung, welche Rollenzuweisungen mithilfe von Privileged Identity Management für Azure-Ressourcen verwaltet werden sollen, müssen Sie zunächst die Abonnements/Ressourcen identifizieren, die für Ihre Organisation am wichtigsten sind. Beispiele für solche Abonnements/Ressourcen sind:

- Ressourcen, welche die sensibelsten Daten hosten
- Ressourcen, von denen zentrale kundenorientierte Anwendungen abhängig sind

Wenn Sie sich als globaler Administrator unsicher sind, welche Abonnements/Ressourcen am wichtigsten sind, wenden Sie sich an Abonnementbesitzer in Ihrer Organisation, um eine Liste der Ressourcen zusammenzustellen, die von jedem Abonnement verwaltet werden. Gruppieren Sie dann in Zusammenarbeit mit den Abonnementbesitzern die Ressourcen basierend auf dem Schweregrad im Fall einer Kompromittierung (niedrig, mittel, hoch). Priorisieren Sie die Verwaltung von Ressourcen mit Privileged Identity Management basierend auf diesem Schweregrad.

> [!TIP]
> :heavy_check_mark: **Microsoft-Empfehlung**: Arbeiten Sie mit Abonnement-/Ressourcenbesitzern von kritischen Diensten zusammen, um den Privileged Identity Management-Workflow für alle Rollen in sensiblen Abonnements/Ressourcen einzurichten.

Privileged Identity Management für Azure-Ressourcen unterstützt zeitgebundene Dienstkonten. Behandeln Sie Dienstkonten genauso, wie Sie ein normales Benutzerkonto behandeln würden.

Bei Abonnements/Ressourcen, die nicht so kritisch sind, müssen Sie Privileged Identity Management nicht für alle Rollen einrichten. Die Rollen „Besitzer“ und „Benutzerzugriffsadministrator“ sollten Sie jedoch weiterhin mit Privileged Identity Management schützen.

> [!TIP]
> :heavy_check_mark: **Microsoft-Empfehlung**: Verwalten Sie die Rollen „Besitzer“ und „Benutzerzugriffsadministrator“ aller Abonnements/Ressourcen mit Privileged Identity Management.

### <a name="decide-which-role-assignments-should-be-permanent-or-eligible"></a>Entscheiden, welche Rollenzuweisungen permanent oder berechtigt sein sollen

Nachdem Sie die Liste der von Privileged Identity Management zu verwaltenden Rollen festgelegt haben, müssen Sie entscheiden, welche Benutzer die berechtigte Rolle anstatt der dauerhaft aktiven Rolle erhalten sollen. Dauerhaft aktive Rollen sind die normalen über Azure Active Directory und Azure-Ressourcen zugewiesenen Rollen, während berechtigte Rollen nur in Privileged Identity Management zugewiesen werden können.

> [!TIP]
> :heavy_check_mark: **Microsoft-Empfehlung**: Legen Sie für Azure AD-Rollen und Azure-Ressourcenrollen keine anderen dauerhaft aktiven Zuweisungen fest, als die empfohlenen [zwei Konten für den Notfallzugriff](../users-groups-roles/directory-emergency-access.md), welche über die permanente Rolle „Globaler Administrator“ verfügen müssen.

Wir empfehlen zwar, überhaupt keinen ständigen Administratorzugriff bereitzustellen, für Organisationen ist es jedoch mitunter schwierig, dies sofort umzusetzen. Folgende Aspekte sind bei dieser Entscheidung zu berücksichtigen:

- Häufigkeit der Rechteerhöhung: Wenn der Benutzer die privilegierte Zuweisung nur einmal benötigt, sollte er nicht die permanente Zuweisung haben. Wenn der Benutzer andererseits die Rolle für seine tägliche Arbeit benötigt und seine Produktivität durch die Verwendung von Privileged Identity Management erheblich verringert würde, kann er für die permanente Rolle in Betracht gezogen werden.
- Für Ihre Organisation spezifische Fälle: Wenn die Person, welche die berechtigte Rolle erhält, aus einem sehr entfernten Team stammt oder eine hochrangige Führungskraft ist, sodass die Kommunikation und die Durchsetzung der Erhöhung der Rechte schwierig ist, kann die Person für die permanente Rolle in Betracht gezogen werden.

> [!TIP]
> :heavy_check_mark: **Microsoft-Empfehlung**: Richten Sie laufende Zugriffsüberprüfungen für Benutzer mit permanenten Rollenzuweisungen ein (falls vorhanden). Im letzten Abschnitt dieses Bereitstellungsplans erfahren Sie mehr über laufende Zugriffsüberprüfungen.

### <a name="draft-your-privileged-identity-management-settings"></a>Entwerfen Ihrer Privileged Identity Management-Einstellungen

Vor dem Implementieren Ihrer Privileged Identity Management-Lösung empfiehlt es sich, einen Entwurf der Privileged Identity Management-Einstellungen für jede privilegierte Rolle zu erstellen, die in Ihrer Organisation verwendetet wird. Dieser Abschnitt enthält einige Beispiele für Privileged Identity Management-Einstellungen für bestimmte Rollen (diese dienen nur als Referenz und können für Ihre Organisation anders ausfallen). Jede dieser Einstellungen wird nach den Tabellen mit den Empfehlungen von Microsoft ausführlich erläutert.

#### <a name="privileged-identity-management-settings-for-azure-ad-roles"></a>Privileged Identity Management-Einstellungen für Azure AD-Rollen

| Role | Anfordern von MFA | Benachrichtigung | Vorfallsticket | Genehmigung anfordern | Genehmigende Person | Aktivierungsdauer | Permanenter Administrator |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Globaler Administrator | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | Andere globale Administratoren | 1 Stunde | Konten für den Notfallzugriff |
| Exchange-Administrator | :heavy_check_mark: | :heavy_check_mark: | :x: | :x: | Keine | 2 Stunden | Keine |
| Helpdeskadministrator | :x: | :x: | :heavy_check_mark: | :x: | Keine | 8 Stunden | Keine |

#### <a name="privileged-identity-management-settings-for-azure-resource-roles"></a>Privileged Identity Management-Einstellungen für Azure-Ressourcenrollen

| Role | Anfordern von MFA | Benachrichtigung | Genehmigung anfordern | Genehmigende Person | Aktivierungsdauer | Aktiver Administrator | Ablauf der Aktivierung | Ablauf der Berechtigung |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Besitzer von wichtigen Abonnements | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | Andere Besitzer des Abonnements | 1 Stunde | Keine | – | 3 Monate |
| Benutzerzugriffsadministrator von weniger wichtigen Abonnements | :heavy_check_mark: | :heavy_check_mark: | :x: | Keine | 1 Stunde | Keine | – | 3 Monate |
| Mitwirkender von virtuellen Computern | :x: | :heavy_check_mark: | :x: | Keine | 3 Stunden | Keine | – | 6 Monate |

In der folgenden Tabelle sind die einzelnen Einstellungen beschrieben.

| Einstellung | BESCHREIBUNG |
| --- | --- |
| Role | Name der Rolle, für die Sie die Einstellungen definieren. |
| Anfordern von MFA | Gibt an, ob der berechtigte Benutzer vor dem Aktivieren der Rolle eine mehrstufige Authentifizierung durchführen muss.<br/><br/> :heavy_check_mark: **Microsoft-Empfehlung**: Erzwingen Sie die mehrstufige Authentifizierung für alle Administratorrollen, insbesondere wenn die Rollen Gastbenutzer enthalten. |
| Benachrichtigung | Ist diese Einstellung auf „true“ festgelegt, erhalten globale Administratoren, Administratoren für privilegierte Rollen und Sicherheitsadministratoren in der Organisation eine E-Mail-Benachrichtigung, wenn ein berechtigter Benutzer die Rolle aktiviert.<br/><br/>**Hinweis:** Einige Organisationen haben keine E-Mail-Adressen mit ihren Administratorkonten verknüpft. Um diese E-Mail-Benachrichtigungen zu erhalten müssen Sie eine alternative E-Mail-Adresse festlegen, damit Administratoren diese E-Mails erhalten. |
| Vorfallsticket | Gibt an, ob der berechtigte Benutzer beim Aktivieren seiner Rolle eine Vorfallsticketnummer erfassen muss. Mit dieser Einstellung kann eine Organisation jede Aktivierung anhand einer internen Vorfallnummer identifizieren, um unerwünschte Aktivierungen zu verringern.<br/><br/> :heavy_check_mark: **Microsoft-Empfehlung**: Nutzen Sie Incidentticketnummern, um Privileged Identity Management mit Ihrem internen System zu verknüpfen. Dies ist insbesondere nützlich für genehmigende Personen, die Kontext für die Aktivierung benötigen. |
| Genehmigung anfordern | Gibt an, ob der berechtigte Benutzer eine Genehmigung zum Aktivieren der Rolle einholen muss.<br/><br/> :heavy_check_mark: **Microsoft-Empfehlung**: Richten Sie die Genehmigung für Rollen mit der höchsten Berechtigung ein. Basierend auf den Nutzungsmustern aller Privileged Identity Management-Kunden handelt es sich bei den Rollen „Globaler Administrator“, „Benutzeradministrator“, „Exchange-Administrator“, „Sicherheitsadministrator“ und „Kennwortadministrator“ um die am häufigsten verwendeten Rollen mit eingerichteter Genehmigung. |
| Genehmigende Person | Wenn zum Aktivieren der berechtigten Rolle eine Genehmigung erforderlich ist, listen Sie die Personen auf, von denen die Anforderung genehmigt werden muss. Standardmäßig legt Privileged Identity Management alle Benutzer als genehmigende Person fest, die Administrator für privilegierte Rollen sind, unabhängig davon, ob es sich um eine permanente oder berechtigte Zuweisung handelt.<br/><br/>**Hinweis:** Wenn ein Benutzer sowohl für eine Azure AD-Rolle berechtigt als auch eine genehmigende Person der Rolle ist, kann er sich nicht selbst genehmigen.<br/><br/> :heavy_check_mark: **Microsoft-Empfehlung**: Wählen Sie als genehmigende Personen jene aus, die am besten über eine bestimmte Rolle und deren häufige Benutzer Bescheid wissen, anstatt eines globalen Administrators. |
| Aktivierungsdauer | Die Zeitspanne, in der ein Benutzer in der Rolle aktiviert ist, bevor sie abläuft. |
| Permanenter Administrator | Liste der Benutzer, die ein permanenter Administrator für die Rolle sein werden (d. h. sie nie aktivieren müssen).<br/><br/> :heavy_check_mark: **Microsoft-Empfehlung**: Legen Sie keine ständigen Administratoren für Rollen fest, mit Ausnahme von globalen Administratoren. Mehr dazu erfahren Sie in diesem Plan in den Abschnitten dazu, wer berechtigte und wer dauerhaft aktive Rollenzuweisungen erhalten sollte. |
| Aktiver Administrator | Für Azure-Ressourcen ist „Aktiver Administrator“ die Liste der Benutzer, die zur Verwendung der Rolle nie eine Aktivierung durchführen müssen. Dies wird nicht als permanenter Administrator wie bei Azure AD-Rollen bezeichnet, da Sie eine Ablaufzeit festlegen können, nach der ein Benutzer diese Rolle verliert. |
| Ablauf der Aktivierung | Eine aktive Rollenzuweisung für Azure-Ressourcenrollen läuft nach diesem konfigurierten Zeitraum ab. Sie können zwischen 15 Tagen, 1 Monat, 3 Monaten, 6 Monaten, 1 Jahr und dauerhafter Aktivität wählen. |
| Ablauf der Berechtigung | Eine berechtigte Rollenzuweisung für Azure-Ressourcenrollen läuft nach diesem konfigurierten Zeitraum ab. Sie können zwischen 15 Tagen, 1 Monat, 3 Monaten, 6 Monaten, 1 Jahr und dauerhafter Berechtigung wählen. |

## <a name="implement-your-solution"></a>Implementieren Ihrer Lösung

Die Grundlage für eine gründliche Planung ist die Basis, auf der Sie eine Anwendung in Azure Active Directory erfolgreich bereitstellen können.  Sie bietet intelligente Sicherheit und Integration, die das Onboarding vereinfacht und den Zeitaufwand für erfolgreiche Bereitstellungen reduziert.  Mit dieser Kombination wird sichergestellt, dass Ihre Anwendung problemlos integriert und gleichzeitig Ausfallzeiten für Ihre Endbenutzer vorgebeugt wird.

### <a name="identify-test-users"></a>Identifizieren von Testbenutzern

Verwenden Sie diesen Abschnitt, um einen Satz von Benutzern und Gruppen identifizieren, um die Implementierung zu überprüfen. Identifizieren Sie basierend auf den Einstellungen, die Sie im Planungsabschnitt ausgewählt haben, die Benutzer, die Sie für jede Rolle testen möchten.

> [!TIP]
> :heavy_check_mark: **Microsoft-Empfehlung**: Legen Sie Dienstbesitzer der einzelnen Azure AD-Rollen als Testbenutzer fest, damit sie mit dem Prozess vertraut werden und die Einführung intern unterstützen können.

Identifizieren Sie in dieser Tabelle die Testbenutzer, die überprüfen, ob die Einstellungen für die einzelnen Rollen funktionieren.

| Rollenname | Testbenutzer |
| --- | --- |
| &lt;Rollenname&gt; | &lt;Benutzer zum Testen der Rolle&gt; |
| &lt;Rollenname&gt; | &lt;Benutzer zum Testen der Rolle&gt; |

### <a name="test-implementation"></a>Testimplementierung

Nachdem Sie nun die Testbenutzer identifiziert haben, konfigurieren Sie in diesem Schritt Privileged Identity Management für Ihre Testbenutzer. Wenn Ihre Organisation einen Privileged Identity Management-Workflow in Ihre eigene interne Anwendung integrieren möchte, statt Privileged Identity Management im Azure-Portal zu verwenden, werden alle Vorgänge in Privileged Identity Management auch über unsere [Graph-API](https://docs.microsoft.com/graph/api/resources/privilegedidentitymanagement-root) unterstützt.

#### <a name="configure-privileged-identity-management-for-azure-ad-roles"></a>Konfigurieren von Privileged Identity Management für Azure AD-Rollen

1. [Konfigurieren Sie die Einstellungen für Azure AD-Rollen](pim-how-to-change-default-settings.md) basierend auf Ihrer Planung.

1. Navigieren Sie zu **Azure AD-Rollen**, klicken Sie auf **Rollen**, und wählen Sie dann die soeben konfigurierte Rolle aus.

1. Wenn die Gruppe von Testbenutzern bereits ein permanenter Administrator ist, können Sie sie als berechtigt festlegen. Dazu suchen Sie nach den Benutzern und konvertieren sie von permanent in berechtigt, indem Sie auf die drei Punkte in ihrer Zeile klicken. Wenn sie die Rollenzuweisungen noch nicht haben, können Sie [neue berechtigte Zuweisung erstellen](pim-how-to-add-role-to-user.md#make-a-user-eligible-for-a-role).

1. Wiederholen Sie die Schritte 1 bis 3 für alle Rollen, die Sie testen möchten.

1. Nachdem Sie die Testbenutzer eingerichtet haben, müssen Sie ihnen den Link mit den Anweisungen zum [Aktivieren ihrer Azure AD-Rolle](pim-how-to-activate-role.md) senden.

#### <a name="configure-privileged-identity-management-for-azure-resource-roles"></a>Konfigurieren von Privileged Identity Management für Azure-Ressourcenrollen

1. [Konfigurieren Sie die Einstellungen für die Azure-Ressourcenrolle](pim-resource-roles-configure-role-settings.md) für eine Rolle in einem Abonnement oder einer Ressource, die Sie testen möchten.

1. Navigieren Sie zu **Azure-Ressourcen** für dieses Abonnement, klicken Sie auf **Rollen**, und wählen Sie die soeben konfigurierte Rolle aus.

1. Wenn die Gruppe von Testbenutzern bereits ein aktiver Administrator ist, können Sie sie als berechtigt festlegen. Dazu suchen Sie nach den Benutzern und [aktualisieren ihre Rollenzuweisung](pim-resource-roles-assign-roles.md#update-or-remove-an-existing-role-assignment). Wenn sie die Rolle noch nicht haben, können Sie [eine neue Rolle zuweisen](pim-resource-roles-assign-roles.md#assign-a-role).

1. Wiederholen Sie die Schritte 1 bis 3 für alle Rollen, die Sie testen möchten.

1. Nachdem Sie die Testbenutzer eingerichtet haben, müssen Sie ihnen den Link mit den Anweisungen zum [Aktivieren ihrer Azure-Ressourcenrolle](pim-resource-roles-activate-your-roles.md) senden.

Überprüfen Sie in dieser Phase, ob die gesamte von Ihnen für die Rollen festgelegte Konfiguration ordnungsgemäß funktioniert. Verwenden Sie die folgende Tabelle zum Dokumentieren Ihrer Tests. Nutzen Sie diese Phase auch zur Optimierung der Kommunikation mit betroffenen Benutzern.

| Role | Erwartetes Verhalten während der Aktivierung | Tatsächliche Ergebnisse |
| --- | --- | --- |
| Globaler Administrator | (1) Mehrstufige Authentifizierung erfordern<br/>(2) Genehmigung erfordern<br/>(3) Genehmigende Person erhält Benachrichtigung und kann genehmigen<br/>(4) Rolle läuft nach voreingestellter Zeit ab |  |
| Besitzer von Abonnement *X* | (1) Mehrstufige Authentifizierung erfordern<br/>(2) Berechtigte Zuweisung läuft nach konfiguriertem Zeitraum |  |

### <a name="communicate-privileged-identity-management-to-affected-stakeholders"></a>Informieren der betroffenen Projektbeteiligten über Privileged Identity Management

Die Bereitstellung von Privileged Identity Management bringt zusätzliche Schritte für Benutzer von privilegierten Rollen mit sich. Zwar werden Sicherheitsprobleme im Zusammenhang mit privilegierten Identitäten durch Privileged Identity Management erheblich reduziert, die Änderung muss jedoch vor der mandantenweiten Bereitstellung effektiv kommuniziert werden. Je nach Anzahl der betroffenen Administratoren entscheiden sich Organisationen häufig für die Erstellung eines internen Dokuments, eines Videos oder einer E-Mail zu dieser Änderung. Folgende Informationen sind in diesen Mitteilungen häufig enthalten:

- Was ist PIM?
- Was ist der Vorteil für die Organisation?
- Wer ist betroffen?
- Wann wird PIM eingeführt?
- Welche zusätzlichen Schritte müssen Benutzer zum Aktivieren ihrer Rolle ausführen?
    - Senden Sie Link zu unserer Dokumentation:
    - [Aktivieren von Azure AD-Verzeichnisrollen in PIM](pim-how-to-activate-role.md)
    - [Aktivieren von Azure-Ressourcenrollen in PIM](pim-resource-roles-activate-your-roles.md)
- Kontaktinformationen oder Helpdesk-Link für Probleme im Zusammenhang mit PIM

> [!TIP]
> :heavy_check_mark: **Microsoft-Empfehlung**: Vereinbaren Sie eine Zeit mit den Mitgliedern Ihres Helpdesk-/Supportteams, um sie Schritt für Schritt durch den Privileged Identity Management-Workflow zu führen (falls Ihre Organisation über ein internes IT-Supportteam verfügt). Stellen Sie ihnen die entsprechende Dokumentationen sowie Ihre Kontaktdaten zur Verfügung.

### <a name="move-to-production"></a>Überführen in die Produktion

Nachdem Sie Ihre Tests erfolgreich abgeschlossen haben, verschieben Sie Privileged Identity Management in die Produktionsumgebung. Dazu wiederholen Sie alle Schritte der Testphase für alle Benutzer jeder Rolle, die Sie in Ihrer Privileged Identity Management-Konfiguration definiert haben. Bei Privileged Identity Management für Azure AD-Rollen entscheiden sich Organisationen häufig dazu, Privileged Identity Management zuerst für globale Administratoren zu testen und einzuführen, bevor die Tests und Rollouts für andere Rollen erfolgen. Bei Azure-Ressourcen(rollen) dagegen nehmen Organisationen die Tests und Rollouts von Privileged Identity Management in der Regel für jeweils ein Azure-Abonnement vor.

### <a name="in-the-case-a-rollback-is-needed"></a>Falls ein Rollback erforderlich ist

Wenn Privileged Identity Management in der Produktionsumgebung nicht wie gewünscht funktioniert, können Ihnen die folgenden Rollback-Schritte bei der Zurücksetzung auf einen bekannten fehlerfreien Zustand vor der Einrichtung von Privileged Identity Management helfen:

#### <a name="azure-ad-roles"></a>Azure AD-Rollen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
1. Öffnen Sie **Azure AD Privileged Identity Management**.
1. Klicken Sie auf **Azure AD-Rollen** und dann auf **Rollen**.
1. Klicken Sie für jede von Ihnen konfigurierte Rolle für alle Benutzer mit einer berechtigten Zuweisung auf die Auslassungspunkte ( **...** ).
1. Klicken Sie auf die Option **Als permanent festlegen**, um die Zuweisung dauerhaft einzurichten.

#### <a name="azure-resource-roles"></a>Azure-Ressourcenrollen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
1. Öffnen Sie **Azure AD Privileged Identity Management**.
1. Klicken Sie auf **Azure-Ressourcen**, und klicken Sie dann auf ein Abonnement oder eine Ressource, für das bzw. die Sie ein Rollback ausführen möchten.
1. Klicken Sie auf **Rollen**.
1. Klicken Sie für jede von Ihnen konfigurierte Rolle für alle Benutzer mit einer berechtigten Zuweisung auf die Auslassungspunkte ( **...** ).
1. Klicken Sie auf die Option **Als permanent festlegen**, um die Zuweisung dauerhaft einzurichten.

## <a name="next-steps-after-deploying"></a>Nächste Schritte nach der Bereitstellung

Die erfolgreiche Bereitstellung von Privileged Identity Management in der Produktionsumgebung ist ein wichtiger Schritt im Hinblick auf den Schutz der privilegierten Identitäten Ihrer Organisation. Durch die Bereitstellung von Privileged Identity Management erhalten Sie zusätzliche Privileged Identity Management-Features, die Sie für die Sicherheit und Compliance verwenden sollten.

### <a name="use-privileged-identity-management-alerts-to-safeguard-your-privileged-access"></a>Verwenden von Privileged Identity Management-Warnungen zum Schützen des privilegierten Zugriffs

Verwenden Sie die integrierte Warnungsfunktion von Privileged Identity Management, um Ihren Mandanten besser zu schützen. Weitere Informationen finden Sie unter [Sicherheitswarnungen](pim-how-to-configure-security-alerts.md#security-alerts). Diese Warnungen umfassen Folgendes: Administratoren verwenden keine privilegierten Rollen, Rollen sind außerhalb von Privileged Identity Management zugewiesen, Rollen werden zu häufig aktiviert und mehr. Zum vollständigen Schutz Ihrer Organisation sollten Sie die Liste der Warnungen regelmäßig prüfen und die Probleme beheben. So können Sie Ihre Warnungen anzeigen und beheben:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
1. Öffnen Sie **Azure AD Privileged Identity Management**.
1. Klicken Sie auf **Azure AD-Rollen** und dann auf **Warnungen**.

> [!TIP]
> :heavy_check_mark: **Microsoft-Empfehlung**: Kümmern Sie sich um alle Warnungen mit hohem Schweregrad sofort. Über Warnungen mit mittlerem und geringem Schweregrad sollten Sie auf dem Laufenden bleiben und Änderungen vornehmen, wenn Sie glauben, dass ein Sicherheitsrisiko vorliegt.

Wenn bestimmte Warnungen nicht nützlich sind oder für Ihre Organisation nicht gelten, können Sie solche Warnungen jederzeit auf der Warnungsseite schließen. Diese Aktion kann später jederzeit auf der Seite „Azure AD-Einstellungen“ rückgängig gemacht werden.

### <a name="set-up-recurring-access-reviews-to-regularly-audit-your-organizations-privileged-identities"></a>Einrichten laufender Zugriffsüberprüfungen zur regelmäßigen Überprüfung der privilegierten Identitäten Ihrer Organisation

Zugriffsüberprüfungen sind die beste Methode, um Benutzer mit privilegierten Rollen oder bestimmte Prüfer zu fragen, ob sie die privilegierte Identität jeweils benötigen. Zugriffsüberprüfungen eignen sich hervorragend dazu, die Angriffsfläche zu reduzieren und konform zu bleiben. Weitere Informationen zum Starten einer Zugriffsüberprüfung finden Sie unter [Starten einer Zugriffsüberprüfung für Azure AD-Verzeichnisrollen in PIM](pim-how-to-start-security-review.md) und [Starten einer Zugriffsüberprüfung für Azure-Ressourcenrollen in PIM](pim-resource-roles-start-access-review.md). Für einige Organisationen ist das Durchführen regelmäßiger Zugriffsüberprüfungen erforderlich, um Gesetze und Vorschriften einzuhalten. Für andere wiederum sind Zugriffsüberprüfungen die beste Möglichkeit, das Prinzip der geringsten Rechte innerhalb der Organisation durchzusetzen.

> [!TIP]
> :heavy_check_mark: **Microsoft-Empfehlung**: Richten Sie vierteljährliche Zugriffsüberprüfungen für alle Azure AD-Rollen und Azure-Ressourcenrollen ein.

In den meisten Fällen ist der Prüfer für Azure AD-Rollen der Benutzer selbst. Für Azure-Ressourcenrollen dagegen ist der Prüfer der Besitzer des Abonnements, in dem sich die Rolle befindet. Unternehmen haben jedoch häufig privilegierte Konten, die nicht mit der E-Mail-Adresse einer bestimmten Person verknüpft sind. In diesen Fällen liest niemand die E-Mails und überprüft den Zugriff.

> [!TIP]
> :heavy_check_mark: **Microsoft-Empfehlung**: Fügen Sie eine sekundäre E-Mail-Adresse für alle Konten mit Zuweisungen privilegierter Rollen hinzu, die nicht mit einer regelmäßig überprüften E-Mail-Adresse verknüpft sind.

### <a name="get-the-most-out-of-your-audit-log-to-improve-security-and-compliance"></a>Optimale Nutzung des Überwachungsprotokolls zur Verbesserung der Sicherheit und Compliance

Mithilfe des Überwachungsprotokolls bleiben Sie auf dem Laufenden und halten Richtlinien ein. Privileged Identity Management speichert derzeit einen 30-tägigen Verlauf des gesamten Verlaufs Ihrer Organisation im Überwachungsprotokoll einschließlich der folgenden Informationen:

- Aktivierung/Deaktivierung berechtigter Rollen
- Rollenzuweisungsaktivitäten innerhalb und außerhalb von Privileged Identity Management
- Änderungen an Rolleneinstellungen
- Anforderungs-/Genehmigungs-/Verweigerungsaktivitäten für die Rollenaktivierung mit eingerichteter Genehmigung
- Warnungsaktualisierungen

Zugriff auf diese Überwachungsprotokolle haben Sie als globaler Administrator und als Administrator für privilegierte Rollen. Weitere Informationen finden Sie unter [Anzeigen des Überwachungsverlaufs für Azure AD-Verzeichnisrollen in PIM](pim-how-to-use-audit-log.md) und [Anzeigen von Aktivitäten und Überwachungsverlauf für Azure-Ressourcenrollen in PIM](azure-pim-resource-rbac.md).

> [!TIP]
> :heavy_check_mark: **Microsoft-Empfehlung**: Mindestens ein Administrator sollte wöchentlich alle Überwachungsereignisse lesen und monatlich alle Überwachungsereignisse exportieren.

Wenn Sie Ihre Überwachungsereignisse für einen längeren Zeitraum automatisch speichern möchten, wird das Privileged Identity Management-Überwachungsprotokoll automatisch mit den [Azure AD-Überwachungsprotokollen](../reports-monitoring/concept-audit-logs.md) synchronisiert.

> [!TIP]
> :heavy_check_mark: **Microsoft-Empfehlung**: Richten Sie [Azure-Protokollüberwachung](../reports-monitoring/concept-activity-logs-azure-monitor.md) ein, um Überwachungsereignisse zu Sicherheits- und Compliancezwecken in einem Azure Storage-Konto zu archivieren.

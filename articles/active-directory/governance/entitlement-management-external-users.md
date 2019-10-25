---
title: 'Steuern des Zugriffs für externe Benutzer in der Azure AD-Berechtigungsverwaltung (Vorschau): Azure Active Directory'
description: Enthält Informationen zu den Einstellungen, die Sie angeben können, um den Zugriff für externe Benutzer in der Azure Active Directory-Berechtigungsverwaltung zu steuern.
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
ms.date: 10/15/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: bcf4a0272e21a1fba3cf9adbd9158492e4318578
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72452908"
---
# <a name="govern-access-for-external-users-in-azure-ad-entitlement-management-preview"></a>Steuern des Zugriffs für externe Benutzer in der Azure AD-Berechtigungsverwaltung (Vorschau)

> [!IMPORTANT]
> Die Berechtigungsverwaltung von Azure Active Directory (Azure AD) befindet sich derzeit in der öffentlichen Vorschau.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Für die Azure AD-Berechtigungsverwaltung wird [Azure AD Business-to-Business (B2B)](../b2b/what-is-b2b.md) genutzt, um mit Personen außerhalb Ihrer Organisation in einem anderen Verzeichnis zusammenzuarbeiten. Externe Benutzer verwenden Azure AD B2B für die Authentifizierung gegenüber ihrem Basisverzeichnis, aber sie werden in Ihrem Verzeichnis dargestellt. Die Darstellung in Ihrem Verzeichnis ermöglicht dem Benutzer den Zugriff auf Ihre Ressourcen.

In diesem Artikel werden die Einstellungen beschrieben, die Sie angeben können, um den Zugriff für externe Benutzer zu steuern.

## <a name="how-entitlement-management-can-help"></a>Vorteile der Berechtigungsverwaltung

Wenn Sie [Azure AD B2B](../b2b/what-is-b2b.md)-Einladungen verwenden, müssen Sie bereits die E-Mail-Adressen der externen Gastbenutzer kennen, die Sie in das Ressourcenverzeichnis aufnehmen möchten, um mit ihnen zusammenzuarbeiten. Dies lässt sich leicht durchführen, wenn Sie an einem kleineren oder kurzfristigen Projekt arbeiten und bereits alle Teilnehmer kennen. Es ist jedoch schwieriger, wenn Sie mit vielen Benutzern arbeiten möchten oder wenn die Teilnehmer im Lauf der Zeit wechseln.  Angenommen, Sie arbeiten mit einer anderen Organisation zusammen und haben einen Ansprechpartner in dieser Organisation, im Laufe der Zeit benötigen jedoch weitere Benutzer in dieser Organisation ebenfalls Zugriff.

Mit der Berechtigungsverwaltung können Sie eine Richtlinie definieren, die Benutzern aus von Ihnen angegebenen Organisationen das selbstständige Anfordern eines Zugriffspakets ermöglicht. Sie können ein Ablaufdatum für den Zugriff angeben und festlegen, ob eine Genehmigung erforderlich ist. Wenn eine Genehmigung erforderlich ist, können Sie auch einen oder mehrere Benutzer der externen Organisation für Ihr Verzeichnis einladen und als genehmigende Personen festlegen, da diese wahrscheinlich am besten wissen, welche externen Benutzer in ihrer eigenen Organisation Zugriff benötigen. Nachdem Sie das Zugriffspaket konfiguriert haben, können Sie der Kontaktperson (Projektsponsor) in der externen Organisation den Link zum Zugriffspaket senden. Diese Kontaktperson kann den Link für andere Benutzer in der externen Organisation freigeben, und diese können mit diesem Link das Zugriffspaket anfordern. Benutzer in dieser Organisation, die bereits in Ihr Verzeichnis eingeladen wurden, können diesen Link ebenfalls verwenden.

Wenn eine Anforderung genehmigt wurde, gewährt die Berechtigungsverwaltung dem Benutzer den erforderlichen Zugriff. Dies kann das Einladen des Benutzers umfassen, falls er sich noch nicht in Ihrem Verzeichnis befindet. Azure AD erstellt automatisch ein B2B-Gastkonto für den Benutzer. Beachten Sie, dass ein Administrator möglicherweise durch das Festlegen einer [B2B-Zulassungs- oder -Verweigerungsliste](../b2b/allow-deny-list.md), die Einladungen an andere Organisationen zulässt oder blockiert, festgelegt hat, welche Organisationen für die Zusammenarbeit zugelassen werden.  Wenn die Zulassungs- oder Verweigerungsliste den Benutzer nicht zulässt, wird er nicht eingeladen.

Da der Zugriff des Benutzers nicht unbefristet sein soll, geben Sie in der Richtlinie ein Ablaufdatum, z.B. 180 Tage, an. Nach 180 Tagen wird der gesamte per Zugriffspaket gewährte Zugriff durch die Berechtigungsverwaltung entfernt, sofern er nicht verlängert wird. Wenn der Benutzer, der über die Berechtigungsverwaltung eingeladen wurde, nach Ende der Zuweisung über keine weiteren Zuweisungen von Zugriffspaketen verfügt, wird die Anmeldung über sein Gastkonto standardmäßig 30 Tage lang gesperrt, und anschließend wird das Konto entfernt. Auf diese Weise lassen sich unnötige Konten verhindern. Wie in den folgenden Abschnitten beschrieben, können diese Einstellungen konfiguriert werden.

## <a name="how-access-works-for-external-users"></a>Funktionsweise des Zugriffs für externe Benutzer

Im folgenden Diagramm und den folgenden Schritten erhalten Sie einen Überblick darüber, wie externen Benutzern der Zugriff auf ein Zugriffspaket gewährt wird.

![Diagramm: Lebenszyklus externer Benutzer](./media/entitlement-management-external-users/external-users-lifecycle.png)

1. Sie erstellen in Ihrem Verzeichnis ein Zugriffspaket, das die Richtlinie [Für nicht in Ihrem Verzeichnis befindliche Benutzer](entitlement-management-access-package-create.md#for-users-not-in-your-directory) enthält.

1. Sie senden einen [Link für das Portal „Mein Zugriff“](entitlement-management-access-package-settings.md) an Ihre Kontaktperson in der externen Organisation, den diese zur Anforderung des Zugriffspakets an die Benutzer weitergeben kann.

1. Ein externer Benutzer (in diesem Beispiel **Requestor A**) verwendet den Link für das Portal „Mein Zugriff“, um [Zugriff auf das Zugriffspaket anzufordern](entitlement-management-request-access.md).

1. Eine genehmigende Person [genehmigt die Anforderung](entitlement-management-request-approve.md) (oder sie wird automatisch genehmigt).

1. Für die Anforderung wird in den [Übermittlungsstatus](entitlement-management-process.md) gewechselt.

1. Per B2B-Einladungsprozess wird in Ihrem Verzeichnis ein Gastbenutzerkonto erstellt (in diesem Beispiel **Requestor A (Guest)** ). Wenn eine [Zulassungs- oder Verweigerungsliste](../b2b/allow-deny-list.md) definiert ist, wird die Listeneinstellung angewendet.

1. Dem Gastbenutzer wird der Zugriff auf alle Ressourcen des Zugriffspakets zugewiesen. Es kann einige Zeit dauern, bis Änderungen an Azure AD und anderen Microsoft Online Services oder verbundenen SaaS-Anwendungen vorgenommen werden. Weitere Informationen finden Sie im Abschnitt zur [Anwendung von Änderungen](entitlement-management-access-package-resources.md#when-changes-are-applied).

1. Der externe Benutzer erhält eine E-Mail mit dem Hinweis, dass der Zugriff [bereitgestellt](entitlement-management-process.md) wurde.

1. Für den Zugriff auf die Ressourcen kann der externe Benutzer entweder in der E-Mail auf den Link klicken oder versuchen, direkt auf die Verzeichnisressourcen zuzugreifen, um den Einladungsprozess abzuschließen.

1. Abhängig von den Richtlinieneinstellungen läuft die Zuweisung des Zugriffspakets für den externen Benutzer nach einer bestimmten Zeit ab, sodass der Zugriff für ihn nicht mehr möglich ist.

1. Je nach Lebenszyklus der Einstellungen des externen Benutzers gilt Folgendes: Wenn der externe Benutzer nicht mehr über Zuweisungen von Zugriffspaketen verfügt, wird seine Anmeldung blockiert und das Gastbenutzerkonto aus Ihrem Verzeichnis entfernt.

## <a name="manage-the-lifecycle-of-external-users"></a>Verwalten des Lebenszyklus von externen Benutzern

Sie können auswählen, was passiert, wenn ein externer Benutzer, der per Genehmigung der Anforderung eines Zugriffspakets eingeladen wurde, nicht mehr über Zuweisungen von Zugriffspaketen verfügt. Dies kann passieren, wenn der Benutzer alle Zuweisungen von Zugriffspaketen aufgibt oder seine letzte Zuweisung eines Zugriffspakets abläuft. Wenn ein externer Benutzer nicht mehr über Zuweisungen von Zugriffspaketen verfügt, wird für ihn die Anmeldung bei Ihrem Verzeichnis standardmäßig blockiert. Nach 30 Tagen wird das Gastbenutzerkonto aus Ihrem Verzeichnis entfernt.

**Erforderliche Rolle:** Globaler Administrator oder Benutzeradministrator

1. Klicken Sie im Azure-Portal auf **Azure Active Directory** und dann auf **Identity Governance**.

1. Klicken Sie im linken Menü im Abschnitt **Berechtigungsverwaltung** auf **Einstellungen**.

1. Klicken Sie auf **Edit**.

    ![Einstellungen für die Verwaltung des Lebenszyklus von externen Benutzern](./media/entitlement-management-external-users/settings-external-users.png)

1. Wählen Sie im Abschnitt **Lebenszyklus von externen Benutzern verwalten** die jeweiligen Einstellungen für externe Benutzer aus.

1. Wenn ein externer Benutzer seine letzte Zuweisung von Zugriffspaketen verliert und Sie die Anmeldung beim Verzeichnis blockieren möchten, müssen Sie die Option **Anmelden von externen Benutzern bei diesem Verzeichnis blockieren** auf **Ja** festlegen.

1. Wenn ein externer Benutzer seine letzte Zuweisung von Zugriffspaketen verliert und Sie sein Gastbenutzerkonto in Ihrem Verzeichnis entfernen möchten, müssen Sie **Externen Benutzer entfernen** auf **Ja** festlegen.

    > [!NOTE]
    > Im Rahmen der Berechtigungsverwaltung werden nur Konten entfernt, für die eine Einladung per Berechtigungsverwaltung erfolgt ist. Beachten Sie außerdem Folgendes: Für einen Benutzer wird auch dann die Anmeldung blockiert und die Entfernung aus Ihrem Verzeichnis durchgeführt, wenn dieser Benutzer Ressourcen in Ihrem Verzeichnis hinzugefügt wurde, die nicht den Zuweisungen von Zugriffspaketen unterliegen. Wenn der Gast bereits in Ihrem Verzeichnis vorhanden war, bevor die Zuweisungen von Zugriffspaketen durchgeführt wurden, wird er nicht entfernt. Falls der Gast aber per Zuweisung eines Zugriffspakets eingeladen und anschließend auch einer OneDrive for Business- oder SharePoint Online-Website zugewiesen wurde, wird er trotzdem entfernt.

1. Wenn Sie das Gastbenutzerkonto in Ihrem Verzeichnis entfernen möchten, können Sie die Anzahl von Tagen festlegen, die ablaufen sollen, bevor die Entfernung durchgeführt wird. Falls Sie das Gastbenutzerkonto entfernen möchten, sobald die letzte Zuweisung von Zugriffspaketen abgelaufen ist, legen Sie **Anzahl von Tagen, bevor ein externer Benutzer aus diesem Verzeichnis entfernt wird** auf **0** fest.

1. Klicken Sie auf **Speichern**.

## <a name="enable-a-catalog-for-external-users"></a>Aktivieren eines Katalogs für externe Benutzer

Beim Erstellen eines [neuen Katalogs](entitlement-management-catalog-create.md) können Sie eine Einstellung verwenden, um für Benutzer in externen Verzeichnissen das Anfordern von Zugriffspaketen im Katalog zu ermöglichen. Legen Sie **Für externe Benutzer aktiviert** auf **Nein** fest, wenn Sie nicht möchten, dass externe Benutzer über Berechtigungen zum Anfordern von Zugriffspaketen im Katalog verfügen.

**Erforderliche Rolle:** Globaler Administrator, Benutzeradministrator oder Katalogbesitzer

![Bereich „Neuer Katalog“](./media/entitlement-management-shared/new-catalog.png)

Sie können diese Einstellung auch noch ändern, nachdem Sie den Katalog erstellt haben.

![Bearbeiten von Katalogeinstellungen](./media/entitlement-management-shared/catalog-edit.png)

## <a name="next-steps"></a>Nächste Schritte

- [Für Benutzer, die sich nicht in Ihrem Verzeichnis befinden](entitlement-management-access-package-create.md#for-users-not-in-your-directory)
- [Erstellen und Verwalten eines Katalogs von Ressourcen](entitlement-management-catalog-create.md)
- [Delegierung und Rollen](entitlement-management-delegate.md)

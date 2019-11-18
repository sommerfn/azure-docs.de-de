---
title: Anzeigen und Verwalten von Kunden und delegierten Ressourcen
description: Als Dienstanbieter, der die delegierte Azure-Ressourcenverwaltung verwendet, können Sie alle Ihre delegierten Kundenressourcen und -abonnements anzeigen, indem Sie im Azure-Portal zu „Meine Kunden“ wechseln.
ms.date: 10/23/2019
ms.topic: overview
ms.openlocfilehash: ed1f2e5044fe5058acc4f8b155b346dd46c51b28
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2019
ms.locfileid: "74131239"
---
# <a name="view-and-manage-customers-and-delegated-resources"></a>Anzeigen und Verwalten von Kunden und delegierten Ressourcen

Dienstanbieter, die die [delegierte Azure-Ressourcenverwaltung](../concepts/azure-delegated-resource-management.md) verwenden, können die Seite **Meine Kunden** im [Azure-Portal](https://portal.azure.com) verwenden, um delegierte Kundenressourcen und -abonnements anzuzeigen. Zwar beziehen wir uns hier auf Dienstanbieter und Kunden, doch können Unternehmen, die mehrere Mandanten verwalten, denselben Prozess verwenden, um ihre Verwaltungserfahrung zu konsolidieren.

Um auf die Seite **Meine Kunden** im Azure-Portal zuzugreifen, wählen Sie **Alle Dienste** aus, und suchen Sie dann nach **Meine Kunden**, und wählen Sie es aus. Sie können die Seite auch finden, indem Sie im Suchfeld am oberen Rand des Azure-Portals „Meine Kunden“ eingeben.

Beachten Sie, dass im obersten Bereich der Seite **Meine Kunden** nur Informationen zu Kunden angezeigt werden, die Abonnements oder Ressourcengruppen delegiert haben. Wenn Sie mit anderen Kunden arbeiten (z. B. durch das [CSP-Programm (Programm für Cloud-Lösungsanbieter)](https://docs.microsoft.com/partner-center/csp-overview)), werden hier keine Informationen zu diesen Kunden angezeigt, es sei denn, Sie integrieren deren Ressourcen für die delegierte Ressourcenverwaltung. Unten auf der Seite werden im Abschnitt **Cloud Solution Provider (Vorschauversion)** Abrechnungsinformationen und Ressourcen für Ihre CSP-Kunden angezeigt, die die [Microsoft-Kundenvereinbarung](https://docs.microsoft.com/partner-center/confirm-customer-agreement) signiert haben und sich im Azure-Plan befinden, unabhängig davon, ob ein Onboarding für die delegierte Azure-Ressourcenverwaltung stattgefunden hat. Weitere Informationen finden Sie unter [Erste Schritte mit Ihrem Abrechnungskonto für eine Microsoft-Partnervereinbarung](https://docs.microsoft.com/azure/billing/mpa-overview).

> [!NOTE]
> Ihre Kunden können Informationen zu Dienstanbietern anzeigen, indem sie im Azure-Portal zu **Dienstanbieter** navigieren. Weitere Informationen finden Sie unter [Anzeigen und Verwalten von Dienstanbietern](view-manage-service-providers.md).

## <a name="view-and-manage-customer-details"></a>Anzeigen und Verwalten von Kundendetails

Um Kundendetails anzuzeigen, wählen Sie im linken Bereich der Seite **Meine Kunden** die Option **Kunden** aus.

Für jeden Kunden werden der Name des Kunden, die Kunden-ID (Mandanten-ID) und das Angebot, das mit dem Engagement verknüpft ist, angezeigt. In der Spalte **Delegierungen** wird die Anzahl der delegierten Abonnements und/oder die Anzahl der delegierten Ressourcengruppen angezeigt.

> [!IMPORTANT]
> Um eine Delegierung anzuzeigen, muss den Benutzern im Onboardingprozess die Rolle [Leser](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#reader) (oder eine andere integrierte Rolle mit Lesezugriff) erteilt worden sein.

Mithilfe von Filtern am oberen Rand der Seite können Sie Ihre Kundeninformationen sortieren und gruppieren oder nach bestimmten Kunden, Angeboten oder Schlüsselwörtern filtern.

Folgende Informationen können Sie auf dieser Seite anzeigen:

- Um alle mit einem Kunden verbundenen Abonnements, Angebote und Delegierungen anzuzeigen, wählen Sie den Namen des Kunden aus.
- Um weitere Details zu einem Angebot und seinen Delegierungen anzuzeigen, wählen Sie den Angebotsnamen aus.
- Um weitere Details zu Rollenzuweisungen für delegierte Abonnements oder Ressourcengruppen anzuzeigen, wählen Sie den Eintrag in der Spalte **Delegierungen** aus.

## <a name="view-delegations"></a>Anzeigen von Delegierungen

Delegierungen zeigen das Abonnement/die Ressourcengruppe an, das/die delegiert wurde, zusammen mit den Benutzern und Berechtigungen, die Zugriff darauf haben. Um diese Informationen anzuzeigen, wählen Sie im linken Bereich der Seite **Meine Kunden** die Option **Delegierungen** aus.

Mithilfe von Filtern am oberen Rand der Seite können Sie Ihre Zugriffszuweisungsinformationen sortieren und gruppieren oder nach bestimmten Kunden, Angeboten oder Schlüsselwörtern filtern.

Die den einzelnen Delegierungen zugeordneten Benutzer und Berechtigungen werden in der Spalte **Rollenzuweisungen** angezeigt. Sie können jeden Eintrag auswählen, um die vollständige Liste der Benutzer, Gruppen und Dienstprinzipale anzuzeigen, denen der Zugriff auf das Abonnement oder die Ressourcengruppe gewährt wurde. Von dort aus können Sie einen bestimmten Benutzer, eine bestimmte Gruppe oder einen bestimmten Dienstprinzipalnamen auswählen, um weitere Details zu erhalten.

## <a name="work-in-the-context-of-a-delegated-subscription"></a>Arbeiten im Kontext eines delegierten Abonnements

Sie können direkt im Kontext eines delegierten Abonnements innerhalb des Azure-Portals arbeiten, ohne das Verzeichnis wechseln zu müssen, in dem Sie arbeiten. Gehen Sie dazu wie folgt vor:

1. Wählen Sie das Symbol **Verzeichnis + Abonnement** am oberen Rand des Azure-Portals aus.
2. Stellen Sie im Filter **Globales Abonnement** sicher, dass nur das Kontrollkästchen für das delegierte Abonnement aktiviert ist. Sie können das Dropdownfeld **Aktuell + Delegierte Verzeichnisse** verwenden, um nur Abonnements innerhalb eines bestimmten Verzeichnisses anzuzeigen. (Verwenden Sie nicht die Option **Verzeichnis wechseln**, weil dadurch das Verzeichnis geändert wird, in dem Sie angemeldet sind.)

Wenn Sie dann auf einen Dienst zugreifen, der [mandantenübergreifende Verwaltungserfahrungen](../concepts/cross-tenant-management-experience.md) unterstützt, wird der Dienst standardmäßig den Kontext des delegierten Abonnements verwenden, das Sie ausgewählt haben. Dies können Sie ändern, indem Sie die obigen Schritte ausführen und das Kontrollkästchen **Alle auswählen** aktivieren (oder indem Sie ein oder mehrere Abonnements auswählen, in denen Sie stattdessen arbeiten möchten).

> [!NOTE]
> Wenn Ihnen der Zugriff auf eine oder mehrere Ressourcengruppen gewährt wurde, können Sie, anstatt auf ein vollständiges Abonnement zuzugreifen, das Abonnement auswählen, zu dem diese Ressourcengruppe gehört. Dann arbeiten Sie im Kontext dieses Abonnements, können aber nur auf die vorgesehenen Ressourcengruppen zugreifen.

Sie können auch auf Funktionen im Zusammenhang mit delegierten Abonnements oder Ressourcengruppen aus Diensten heraus zugreifen, die mandantenübergreifende Verwaltungserfahrungen unterstützen, indem Sie das Abonnement oder die Ressourcengruppe aus diesem Dienst heraus auswählen.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie über [Mandantenübergreifende Verwaltungsmöglichkeiten](../concepts/cross-tenant-management-experience.md).
- Erfahren Sie, wie Ihre Kunden [Dienstanbieter anzeigen und verwalten können](view-manage-service-providers.md), indem sie im Azure-Portal zu **Dienstanbieter** navigieren.

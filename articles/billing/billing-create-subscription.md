---
title: Erstellen eines zusätzlichen Azure-Abonnements | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein neues Azure-Abonnement im Azure-Portal erstellen.
services: billing
documentationcenter: ''
author: amberb
manager: amberb
editor: ''
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/28/2018
ms.author: banders
ms.openlocfilehash: acb41de8344d1467e86c38c7c99e8ad4ab517ead
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72375828"
---
# <a name="create-an-additional-azure-subscription"></a>Erstellen eines zusätzlichen Azure-Abonnements

Sie können ein zusätzliches Abonnement für Ihr Abrechnungskonto unter dem [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/), der [Microsoft-Kundenvereinbarung](https://azure.microsoft.com/pricing/purchase-options/microsoft-customer-agreement/) oder der [Microsoft Partnervereinbarung](https://www.microsoft.com/licensing/news/introducing-microsoft-partner-agreement) im Azure-Portal erstellen. Mit einem zusätzlichen Abonnement können Sie beispielsweise Abonnementgrenzwerte vermeiden, separate Umgebungen für die Sicherheit schaffen oder Daten aus Compliancegründen isolieren.

Wenn Sie über ein Abrechnungskonto des Microsoft Online Service-Programms (MOSP) verfügen, können Sie im [Azure-Registrierungsportal](https://account.azure.com/signup?offer=ms-azr-0003p) zusätzliche Abonnements erstellen.

Weitere Informationen zu Abrechnungskonten und zum Identifizieren des Typs Ihres Abrechnungskontos finden Sie unter [Anzeigen Ihrer Abrechnungskonten im Azure-Portal](billing-view-all-accounts.md).

## <a name="permission-required-to-create-azure-subscriptions"></a>Erforderliche Berechtigungen zum Erstellen von Azure-Abonnements

Zum Erstellen von Abonnements benötigen Sie die folgenden Berechtigungen:

|Abrechnungskonto  |Berechtigung  |
|---------|---------|
|Enterprise Agreement (EA) |  Rolle „Kontobesitzer“ für die Enterprise Agreement-Registrierung. Weitere Informationen finden Sie unter [Informationen zu Azure Enterprise Agreement-Administratorrollen in Azure](billing-understand-ea-roles.md).    |
|Microsoft-Kundenvereinbarung (Microsoft Customer Agreement, MCA) |  Rolle „Besitzer“ oder „Mitwirkender“ im Rechnungsabschnitt, Abrechnungsprofil oder Abrechnungskonto. Oder die Rolle „Azure-Abonnementersteller“ im Rechnungsabschnitt.  Weitere Informationen finden Sie unter [Rollen und Aufgaben für die Abonnementabrechnung](billing-understand-mca-roles.md#subscription-billing-roles-and-tasks).    |
|Microsoft-Partnervereinbarung (MPA) |   Rolle „Globaler Administrator“ und „Administrator-Agent“ in der Partnerorganisation. Weitere Informationen finden Sie unter [Partner Center –Zuweisen von Rollen und Berechtigungen zu Benutzern](https://docs.microsoft.com/partner-center/permissions-overview).  |

## <a name="create-a-subscription-in-the-azure-portal"></a>Erstellen eines Abonnements im Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach **Abonnements**.

   ![Screenshot der Suche im Portal nach Abonnements](./media/billing-create-subscription/billing-search-subscription-portal.png)

1. Wählen Sie **Hinzufügen**.

   ![Screenshot der Schaltfläche „Hinzufügen“ in der Ansicht „Abonnements“](./media/billing-create-subscription/subscription-add.png)

1. Wenn Sie über Zugriff auf mehrere Abrechnungskonten verfügen, wählen Sie das Abrechnungskonto aus, für das Sie das Abonnement erstellen möchten.

1. Füllen Sie das Formular aus, und klicken Sie auf **Erstellen**. In den folgenden Tabellen sind die Felder im Formular für die einzelnen Abrechnungskontotypen aufgelistet.

**Enterprise Agreement**

|Feld  |Definition  |
|---------|---------|
|NAME     | Anhand des Anzeigenamens können Sie das Abonnement im Azure-Portal einfach identifizieren.  |
|Angebot     | Wählen Sie „EA Dev/Test“ aus, wenn Sie dieses Abonnement für Entwicklungs- oder Testworkloads verwenden möchten. Für alle anderen Workloads verwenden Sie „Microsoft Azure Enterprise“. Das DevTest-Angebot muss für Ihr Registrierungskonto aktiviert sein, damit Sie EA Dev/Test-Abonnements erstellen können.|

**Microsoft-Kundenvereinbarung**

|Feld  |Definition  |
|---------|---------|
|Abrechnungsprofil     | Die Gebühren für Ihr Abonnement werden dem ausgewählten Abrechnungsprofil in Rechnung gestellt. Wenn Sie nur auf ein Abrechnungsprofil zugreifen können, wird die Auswahl abgeblendet dargestellt.     |
|Rechnungsabschnitt     | Die Gebühren für Ihr Abonnement werden in diesem Abschnitt der Rechnung des Abrechnungsprofils angezeigt. Wenn Sie nur auf einen Rechnungsabschnitt zugreifen können, wird die Auswahl abgeblendet dargestellt.  |
|Plan     | Wählen Sie „Microsoft Azure-Plan für Dev/Test“ aus, wenn Sie dieses Abonnement für Entwicklungs- oder Testworkloads verwenden möchten. Für alle anderen Workloads verwenden Sie „Microsoft Azure-Plan“. Wenn nur ein Plan für das Abrechnungsprofil aktiviert ist, wird die Auswahl abgeblendet dargestellt.  |
|NAME     | Anhand des Anzeigenamens können Sie das Abonnement im Azure-Portal einfach identifizieren.  |

**Microsoft-Partnervereinbarung**

|Feld  |Definition  |
|---------|---------|
|Kunde    | Das Abonnement wird für den von Ihnen ausgewählten Kunden erstellt. Wenn Sie über nur einen Kunden verfügen, wird die Auswahl abgeblendet dargestellt.  |
|Reseller    | Der Handelspartner, der die Dienste für den Kunden bereitstellt. Dies ist ein optionales Feld, das nur für indirekte Anbieter im CSP-Modell mit zwei Ebenen anwendbar ist. |
|NAME     | Anhand des Anzeigenamens können Sie das Abonnement im Azure-Portal einfach identifizieren.  |

## <a name="create-an-additional-azure-subscription-programmatically"></a>Programmgesteuertes Erstellen eines zusätzlichen Azure-Abonnements

Sie können zusätzliche Abonnements auch programmgesteuert erstellen. Weitere Informationen finden Sie unter [Programmgesteuertes Erstellen von Azure Enterprise-Abonnements](../azure-resource-manager/programmatically-create-subscription.md).

## <a name="next-steps"></a>Nächste Schritte

- [Hinzufügen oder Ändern von Azure-Abonnementadministratoren](billing-add-change-azure-subscription-administrator.md)
- [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/resource-group-move-resources.md?toc=/azure/billing/TOC.json)
- [Erstellen von Verwaltungsgruppen zum Organisieren und Verwalten von Ressourcen](../governance/management-groups/create.md?toc=/azure/billing/TOC.json)
- [Kündigen Ihres Abonnements für Azure](billing-how-to-cancel-azure-subscription.md)

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

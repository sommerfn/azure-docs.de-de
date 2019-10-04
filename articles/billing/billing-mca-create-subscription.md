---
title: Erstellen eines zusätzlichen Azure-Abonnements für Ihr Abrechnungskonto
description: Erfahren Sie, wie Sie ein neues Azure-Abonnement im Azure-Portal erstellen.
author: amberbhargava
manager: amberb
editor: banders
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/01/2019
ms.author: banders
ms.openlocfilehash: e1fafafe5f67d1775ca80a1f7c2aff4dec9e0bc4
ms.sourcegitcommit: d4c9821b31f5a12ab4cc60036fde00e7d8dc4421
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71709587"
---
# <a name="create-an-additional-azure-subscription-for-microsoft-customer-agreement"></a>Erstellen eines zusätzlichen Azure-Abonnements für eine Microsoft-Kundenvereinbarung

Erstellen Sie weitere Abonnements für Ihr Abrechnungskonto, um separate Umgebungen für Entwicklung und Tests einzurichten, um die Sicherheit zu erhöhen oder Daten aus Compliancegründen zu isolieren.

Dieser Artikel bezieht sich auf ein Abrechnungskonto für eine Microsoft-Kundenvereinbarung. [Überprüfen Sie, ob Sie Zugriff auf eine Microsoft-Kundenvereinbarung haben.](#check-access) Wenn Sie Abonnements für andere Typen von Abrechnungskonten erstellen möchten, lesen Sie [Erstellen eines zusätzlichen Azure-Abonnements im Azure-Portal](billing-create-subscription.md).

Um ein Abonnement erstellen zu können, muss Ihnen die Rolle **Rechnungsabschnitt (Besitzer)** , **Rechnungsabschnitt (Mitwirkender)** oder **Azure-Abonnementersteller** zugewiesen sein. Weitere Informationen finden Sie unter [Rollen und Aufgaben für die Abonnementabrechnung](billing-understand-mca-roles.md#subscription-billing-roles-and-tasks). Wenn Sie anderen Benutzern die Berechtigung zum Erstellen von Azure-Abonnements für Ihr Abrechnungskonto erteilen möchten, lesen Sie den Abschnitt [Zuweisen der Berechtigung zum Erstellen von Azure-Abonnements für andere Benutzer](#give-others-permission).

## <a name="create-a-subscription"></a>Erstellen eines Abonnements

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Suchen Sie nach **Abonnements**.

   ![Screenshot der Suche im Portal nach Abonnements](./media/billing-mca-create-subscription/billing-search-subscriptions.png)

3. Wählen Sie **Hinzufügen** aus.

4. Wenn Sie über Zugriff auf mehrere Abrechnungskonten verfügen, wählen Sie das Abrechnungskonto für Ihre Microsoft-Kundenvereinbarung aus.

   ![Screenshot der Seite „Abonnement erstellen“](./media/billing-mca-create-subscription/billing-mca-create-azure-subscription.png)

5. Wählen Sie ein Abrechnungsprofil aus. Die Gebühren für Ihr Abonnement werden dem ausgewählten Abrechnungsprofil in Rechnung gestellt. Wenn Sie nur auf ein Abrechnungsprofil zugreifen können, wird die Auswahl abgeblendet dargestellt.

6. Wählen Sie einen Rechnungsabschnitt aus. Die Gebühren für Ihr Abonnement werden diesem Abschnitt der Rechnung des Rechnungsprofils in Rechnung gestellt. Wenn Sie nur auf einen Rechnungsabschnitt zugreifen können, wird die Auswahl abgeblendet dargestellt.

7. Wählen Sie einen Plan für das Abonnement aus. Wählen Sie **Microsoft Azure-Plan für Dev/Test** aus, wenn Sie dieses Abonnement für Entwicklungs- oder Testworkloads verwenden möchten. Für alle anderen Workloads verwenden Sie **Microsoft Azure-Plan**. Wenn Sie nur auf einen Plan zugreifen können, wird die Auswahl abgeblendet dargestellt.

8. Geben Sie einen Namen für das Abonnement ein. Anhand des Namens können Sie das Abonnement im Azure-Portal einfach identifizieren.

9. Klicken Sie auf **Erstellen**.

## <a name="give-others-permission"></a>Anderen Berechtigung erteilen

Fügen Sie in einem Rechnungsabschnitt Benutzer als Azure-Abonnementersteller hinzu, um diesen Benutzern die Berechtigung zum Erstellen von Azure-Abonnements zu erteilen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Suchen Sie nach **Kostenverwaltung + Abrechnung**.

   ![Screenshot der Suche im Portal nach Abonnements](./media/billing-mca-create-subscription/billing-search-cost-management-billing.png)

3. Navigieren Sie zum Rechnungsabschnitt. Abhängig von Ihren Zugriffsberechtigungen müssen Sie möglicherweise ein Abrechnungskonto oder ein Abrechnungsprofil auswählen. Wählen Sie im Abrechnungskonto oder -profil die Option **Rechnungsabschnitte** aus, und wählen Sie dann in der Liste einen Rechnungsabschnitt aus. Alle Abonnements, die von den Benutzern erstellt wurden, werden diesem Rechnungsabschnitt in Rechnung gestellt.
   
   ![Screenshot der Auswahl von Rechnungsabschnitten](./media/billing-mca-create-subscription/mca-select-invoice-sections.png)        

4. Wählen Sie oben links die Option **Zugriffsverwaltung (IAM)** aus.

5. Wählen Sie oben auf der Seite die Option **Hinzufügen** aus.

6. Wählen Sie die Rolle **Azure-Abonnementersteller** aus.

7. Geben Sie die E-Mail-Adresse des Benutzers ein, dem Sie Zugriff erteilen möchten.

8. Wählen Sie **Speichern** aus.

## <a name="check-access"></a>Zugriff überprüfen
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

Wenn Sie Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte

- [Zuweisen der Berechtigung zum Erstellen von Azure-Ressourcen für andere Benutzer mithilfe integrierter Rollen](../role-based-access-control/built-in-roles.md#built-in-role-descriptions)
- [Erstellen eines virtuellen Windows-Computers](../virtual-machines/windows/quick-create-portal.md)
- [Erstellen eines virtuellen Linux-Computers](../virtual-machines/linux/quick-create-portal.md)
- [Erstellen von Verwaltungsgruppen zum Organisieren und Verwalten von Ressourcen](../governance/management-groups/create.md?toc=/azure/billing/TOC.json)

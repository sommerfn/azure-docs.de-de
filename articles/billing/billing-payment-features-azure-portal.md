---
title: Aufgaben für Kontoadministratoren im Azure-Portal
description: Hier erfahren Sie, wie Sie Zahlungsvorgänge im Azure-Portal ausführen.
author: bandersmsft
manager: jureid
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/05/2019
ms.author: banders
ms.openlocfilehash: 510765778166f007501cfcb21e242c539200ce2f
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73722141"
---
# <a name="account-administrator-tasks-in-the-azure-portal"></a>Aufgaben für Kontoadministratoren im Azure-Portal

In diesem Artikel erfahren Sie, wie Sie folgende Aufgaben im Azure-Portal ausführen:
- Verwalten der Zahlungsmethoden Ihres Abonnements
- Entfernen des Ausgabenlimits Ihres Abonnements
- Hinzufügen von Guthaben zu Ihrem Azure in Open-Abonnement

Diese Aufgaben können nur vom Kontoadministrator ausgeführt werden. 

## <a name="navigate-to-your-subscriptions-payment-methods"></a>Navigieren zu den Zahlungsmethoden Ihres Abonnements

1. Melden Sie sich als Kontoadministrator beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach **Kostenverwaltung + Abrechnung**.
    
    ![Screenshot: Suchen nach „Kostenverwaltung + Abrechnung“ ](./media/billing-payment-features-azure-portal/search-bar.png)

1. Wählen Sie in der Liste **Meine Abonnements** das Abonnement aus, dem Sie die Kreditkarte hinzufügen möchten.
    ![Screenshot: Raster „Meine Abonnements“ in der Übersicht](./media/billing-payment-features-azure-portal/cost-management-billing-overview-x.png)

1. Wählen Sie die Option **Zahlungsmethoden**.

    ![Screenshot: Ausgewähltes Blatt „Zahlungsmethoden“](./media/billing-payment-features-azure-portal/subscription-payment-methods-blade.png)

Hier können Sie eine neue Kreditkarte hinzufügen, die aktive Zahlungsmethode ändern, Kreditkartendetails bearbeiten und Kreditkarten löschen. 

### <a name="change-active-payment-method"></a>Ändern der aktiven Zahlungsmethode

Sie können die aktive Zahlungsmethode ändern, indem Sie eine neue Kreditkarte hinzufügen oder eine bereits gespeicherte Kreditkarte auswählen. So legen Sie eine neue Kreditkarte als aktive Zahlungsmethode fest:

1. Wählen Sie links oben das Pluszeichen (+) aus, um eine Kreditkarte hinzuzufügen.
    
    ![Screenshot: Pluszeichen](./media/billing-payment-features-azure-portal/subscription-payment-methods-plus.png)

1. Geben Sie im Formular auf der rechten Seite Kreditkartendetails ein.

    ![Screenshot: Formular zum Hinzufügen einer Kreditkarte](./media/billing-payment-features-azure-portal/subscription-add-payment-method-x.png)

1. Aktivieren Sie oberhalb des Formulars das Kontrollkästchen **Diese Zahlungsmethode als aktive Methode festlegen**, um diese Karte als Ihre aktive Zahlungsmethode festzulegen. Diese Karte wird zum aktiven Zahlungsmittel für alle Abonnements, indem dieselbe Karte wie für das ausgewählte Abonnement verwendet wird.

    ![Screenshot: Kontrollkästchen, um die Karte als aktive Zahlungsmethode festzulegen](./media/billing-payment-features-azure-portal/subscription-make-active-payment-method-x.png)

1. Klicken Sie auf **Weiter**.

So legen Sie die aktive Zahlungsmethode auf eine bereits gespeicherte Kreditkarte fest:

1. Aktivieren Sie das Kontrollkästchen neben der Karte, die Sie als aktive Zahlungsmethode festlegen möchten.

    ![Screenshot: Aktiviertes Kontrollkästchen neben der Kreditkarte](./media/billing-payment-features-azure-portal/subscription-checked-payment-method-x.png)

1. Klicken Sie auf der Befehlsleiste auf **Als aktiv festlegen**.

    ![Screenshot: Schaltfläche „Als aktiv festlegen“](./media/billing-payment-features-azure-portal/subscription-checked-payment-method-set-active.png)

### <a name="edit-credit-card-details"></a>Bearbeiten von Kreditkartendetails

Wenn Sie Kreditkartendetails wie das Ablaufdatum oder die Adresse bearbeiten möchten, klicken Sie auf die Kreditkarte, die Sie bearbeiten möchten. Rechts wird ein Kreditkartenformular angezeigt.

![Screenshot: Ausgewählte Kreditkarte](./media/billing-payment-features-azure-portal/subscription-edit-payment-method-x.png)

Aktualisieren Sie die Kreditkartendetails, und klicken Sie anschließend auf **Speichern**.

### <a name="remove-a-credit-card-from-the-account"></a>Entfernen einer Kreditkarte aus dem Konto

1. Aktivieren Sie das Kontrollkästchen neben der Karte, die Sie löschen möchten.

    ![Screenshot: Aktiviertes Kontrollkästchen neben der Kreditkarte](./media/billing-payment-features-azure-portal/subscription-checked-payment-method-x.png)

1. Klicken Sie auf der Befehlsleiste auf **Löschen**.

    ![Screenshot: Schaltfläche „Löschen“](./media/billing-payment-features-azure-portal/subscription-checked-payment-method-delete.png)

Wenn Ihre Kreditkarte die aktive Zahlungsmethode für eines Ihrer Microsoft-Abonnements ist, können Sie sie nicht aus Ihrem Azure-Konto entfernen. Ändern Sie die aktive Zahlungsmethode für alle Abonnements, die mit dieser Kreditkarte verknüpft sind, und versuchen Sie es noch mal.

### <a name="switch-to-invoice-payment"></a>Umstellen auf Zahlung per Rechnung

Wenn Sie zur Zahlung per Rechnung (Scheck/Überweisung) berechtigt sind, können Sie Ihr Abonnement im Azure-Portal auf Zahlung per Rechnung (Scheck/Überweisung) umstellen.

1. Wählen Sie auf der Befehlsleiste die Option **Zahlung per Rechnung** aus.

    ![Screenshot: Ausgewähltes Blatt „Zahlungsmethoden“](./media/billing-payment-features-azure-portal/subscription-payment-methods-pay-by-invoice.png)

1. Geben Sie die Adresse für die Zahlungsmethode „Rechnung“ ein.
1. Klicken Sie auf **Weiter**.

Informationen zum Beantragen der Zahlung per Rechnung finden Sie unter [Zahlen für Ihr Azure-Abonnement auf Rechnung](billing-how-to-pay-by-invoice.md).

### <a name="edit-invoice-payment-address"></a>Bearbeiten der Adresse für die Zahlung per Rechnung

Wenn Sie die Adresse für die Zahlungsmethode „Rechnung“ bearbeiten möchten, klicken Sie in der Liste mit den Zahlungsmethoden für Ihr Abonnement auf **Rechnung**. Daraufhin wird auf der rechten Seite das Adressformular geöffnet. 

## <a name="remove-spending-limit"></a>Entfernen des Ausgabenlimits

Mit dem Ausgabenlimit in Azure wird verhindert, dass Sie Ihr Guthaben überschreiten. Wenn für Ihr Abonnement eine gültige Zahlungsmethode festgelegt wurde, können Sie das Ausgabenlimit jederzeit entfernen. Bei Abonnementtypen mit Guthaben für mehrere Monate (beispielsweise Visual Studio Enterprise und Visual Studio Professional) kann das Ausgabenlimit zu Beginn des nächsten Abrechnungszeitraums reaktiviert werden.

Für Abonnements mit Verpflichtungsplänen oder mit nutzungsbasierter Bezahlung ist kein Ausgabenlimit verfügbar. Die vollständige Liste mit Azure-Abonnementtypen sowie Informationen zur Verfügbarkeit des Ausgabenlimits finden Sie [hier](https://azure.microsoft.com/support/legal/offer-details/).

1. Melden Sie sich als Kontoadministrator beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach **Kostenverwaltung + Abrechnung**.

    ![Screenshot: Suchen nach „Kostenverwaltung + Abrechnung“ ](./media/billing-payment-features-azure-portal/search-bar.png)

1. Wählen Sie in der Liste **Meine Abonnements** Ihr Visual Studio Enterprise-Abonnement aus.
    
    ![Screenshot: Raster „Meine Abonnements“ in der Übersicht](./media/billing-payment-features-azure-portal/cost-management-overview-msdn-x.png)

1. Klicken Sie in der Abonnementübersicht auf das orangefarbene Banner, um das Ausgabenlimit zu entfernen.
    
    ![Screenshot: Banner zum Entfernen des Ausgabenlimits](./media/billing-payment-features-azure-portal/msdn-remove-spending-limit-banner-x.png)

1. Wählen Sie aus, ob Sie das Ausgabenlimit dauerhaft oder nur für den aktuellen Abrechnungszeitraum entfernen möchten.

   ![Screenshot: Blatt zum Entfernen des Ausgabenlimits](./media/billing-payment-features-azure-portal/remove-spending-limit-blade-x.png)

1. Klicken Sie auf **Zahlungsmethode auswählen**, um eine Zahlungsmethode für Ihr Abonnement auszuwählen. Diese wird als aktive Zahlungsmethode für Ihr Abonnement verwendet.

1. Klicken Sie auf **Fertig stellen**.

## <a name="add-credits-to-azure-in-open-subscription"></a>Hinzufügen von Guthaben zu einem Azure in Open-Abonnement

Wenn Sie über ein Azure in Open License-Abonnement verfügen, können Sie Ihrem Abonnement im Azure-Portal Guthaben hinzufügen, indem Sie einen Product Key einlösen oder Guthaben mit einer Kreditkarte erwerben.

1. Melden Sie sich als Kontoadministrator beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach **Kostenverwaltung + Abrechnung**.

    ![Screenshot: Suchen nach „Kostenverwaltung + Abrechnung“ ](./media/billing-payment-features-azure-portal/search-bar.png)

1. Wählen Sie in der Liste **Meine Abonnements** Ihr Azure in Open-Abonnement aus.
   
    ![Screenshot: Raster „Meine Abonnements“ in der Übersicht](./media/billing-payment-features-azure-portal/cost-management-overview-aio-x.png)

1. Wählen Sie **Bonitätsgeschichte** aus.
    
    ![Screenshot: Bonitätsgeschichte](./media/billing-payment-features-azure-portal/aio-credit-history-blade.png)

1. Wählen Sie links oben das Pluszeichen (+) aus, um Guthaben hinzuzufügen.

    ![Screenshot: Schaltfläche zum Hinzufügen von Guthaben](./media/billing-payment-features-azure-portal/aio-credit-history-plus.png)

1. Wählen Sie im Dropdownfeld einen Zahlungsmethodentyp aus. Sie können entweder einen Product Key hinzufügen oder Guthaben mit einer Kreditkarte erwerben.
    
    ![Screenshot: Dropdownfeld für die Zahlungsmethode auf dem Blatt „Guthaben hinzufügen“](./media/billing-payment-features-azure-portal/add-credits-select-payment-method.png)

1. Wenn Sie „Product Key“ ausgewählt haben, gehen Sie wie folgt vor:
    - Geben Sie den Product Key ein.
    - Klicken Sie auf **Überprüfen**.

1. Wenn Sie „Kreditkarte“ ausgewählt haben, gehen Sie wie folgt vor:
    - Klicken Sie auf **Zahlungsmethode auswählen**, um eine Kreditkarte hinzuzufügen oder eine vorhandene Kreditkarte auszuwählen.
    - Geben Sie an, wie viel Guthaben Sie hinzufügen möchten.

1. Klicken Sie auf **Anwenden**.

## <a name="troubleshooting"></a>Problembehandlung
Virtuelle Karten oder Prepaidkarten werden nicht unterstützt. Wenn beim Hinzufügen oder Aktualisieren einer gültigen Kreditkarte Fehler auftreten, versuchen Sie, den Browser im privaten Modus zu öffnen.

## <a name="next-steps"></a>Nächste Schritte
- Informieren Sie sich ausführlicher über das [Vermeiden unerwarteter Gebühren bei der Azure-Abrechnung und -Kostenverwaltung](billing-getting-started.md).

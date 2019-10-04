---
title: E-Mail zu überfälligen Zahlungen von Azure | Microsoft-Dokumentation
description: Informationen zur Zahlung, wenn das Azure-Abonnement einen überfälligen Saldo aufweist.
services: ''
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
tags: billing
ms.assetid: d0b88c92-fb9d-4d12-931b-c26104ad63e9
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: banders
ms.openlocfilehash: cd3c29c4b65f45863647599ae2447584845f27c8
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2019
ms.locfileid: "71300281"
---
# <a name="resolve-past-due-balance-for-your-azure-subscription"></a>Begleichen überfälliger Zahlungen für Ihr Azure-Abonnement

Dieser Artikel gilt für Kunden mit einem Microsoft Online Services-Programmkonto.

Wenn Ihre Zahlung nicht eingegangen ist oder wir die Zahlung nicht verarbeiten können, erhalten Sie ggf. eine E-Mail, oder im Azure-Portal oder Kontocenter wird eine Benachrichtigung angezeigt.
Als [Kontoadministrator](billing-subscription-transfer.md#whoisaa) können Sie die ausstehenden Gebühren im [Azure-Portal](https://portal.azure.com) begleichen. Wenn Sie per Rechnung zahlen, senden Sie Ihre Zahlung an den Empfänger, der unten auf der Rechnung aufgeführt ist.

> [!IMPORTANT]
> * Wenn Sie über mehrere Abonnements verfügen, für die dieselbe Kreditkarte genutzt wird und die alle überfällige Zahlungen aufweisen, müssen Sie die gesamte ausstehende Summe auf einmal begleichen.
> * Das Zahlungsmittel, das Sie zum Begleichen der ausstehenden Gebühren verwenden, wird zur neuen aktiven Zahlungsmethode für alle Abonnements, für die die fehlgeschlagene Zahlungsmethode genutzt wurde.

## <a name="resolve-past-due-balance-in-the-azure-portal"></a>Begleichen überfälliger Zahlungen im Azure-Portal

1. Melden Sie sich als Kontoadministrator am [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach **Kostenverwaltung + Abrechnung**.
1. Auf der Seite „Übersicht“ wird eine Liste mit Ihren Abonnements angezeigt. Klicken Sie auf den Link **Saldo ausgleichen** , wenn der Abonnementstatus „Überfällig“ lautet.
    ![Screenshot: Link „Saldo ausgleichen“](./media/billing-azure-subscription-past-due-balance/settle-balance-entry-point.png)
1. Die Gesamtsumme umfasst die ausstehenden Beträge aller Microsoft-Dienste, für die die fehlgeschlagene Zahlungsmethode genutzt wurde.
1. Wählen Sie eine Zahlungsmethode aus, um den Saldo auszugleichen. Diese Zahlungsmethode wird zur aktiven Methode für alle Abonnements, für die derzeit die fehlgeschlagene Zahlungsmethode verwendet wird.
    ![Screenshot: Link zum Auswählen der Zahlungsmethode](./media/billing-azure-subscription-past-due-balance/settle-balance-screen.png)
1. Falls die ausgewählte Zahlungsmethode ebenfalls ausstehende Gebühren für Microsoft-Dienste aufweist, wird dies für die Summe der ausstehenden Beträge berücksichtigt. Sie müssen auch diese ausstehenden Gebühren begleichen.
1. Klicken Sie auf **Bezahlen**.

## <a name="troubleshoot-declined-credit-card"></a>Problembehandlung bei Ablehnung der Kreditkarte

Falls die Belastung Ihrer Kreditkarte von Ihrem Finanzinstitut abgelehnt wurde, wenden Sie sich zur Behebung des Problems an das Finanzinstitut. Stellen Sie in Rücksprache mit Ihrer Bank Folgendes sicher:
- Internationale Transaktionen sind für die Karte aktiviert.
- Die Karte verfügt über ein ausreichendes Kreditlimit oder Guthaben, um den Saldo zu begleichen.
- Wiederkehrende Zahlungen sind für die Karte aktiviert.

## <a name="not-getting-billing-email-notifications"></a>Sie erhalten keine E-Mail-Benachrichtigungen zur Abrechnung?

Wenn Sie Kontoadministrator sind, [überprüfen Sie, welche E-Mail-Adresse für Benachrichtigungen verwendet wird](billing-how-to-change-azure-account-profile.md). Es wird empfohlen, dass Sie eine E-Mail-Adresse verwenden, die Sie regelmäßig überprüfen. Wenn die E-Mail-Adresse richtig ist, überprüfen Sie Ihren Spam-Ordner.

## <a name="if-i-forget-to-pay-what-happens"></a>Was geschieht, wenn ich eine Zahlung vergesse?

Der Dienst wird gekündigt, und Ihre Ressourcen sind nicht mehr verfügbar. Alle Ihre Azure-Daten werden 90 Tage nach dem Kündigen des Diensts gelöscht. Weitere Informationen finden Sie in [Microsoft Trust Center – Wie wir Ihre Daten verwalten](https://go.microsoft.com/fwLink/p/?LinkID=822930&clcid=0x409).

Wenn Sie sicher sind, dass Ihre Zahlung verarbeitet wurde, Ihr Abonnement jedoch immer noch deaktiviert ist, wenden Sie sich an den [Azure-Support](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).


## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

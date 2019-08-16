---
title: Vorauszahlen der Azure App Service-Stempelgebühr (gesondert) mit reservierter Kapazität
description: Hier erfahren Sie, wie Sie die Azure App Service-Stempelgebühr (gesondert) mit reservierter Kapazität vorauszahlen und so Geld sparen können.
services: billing
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 08/02/2019
ms.author: banders
ms.openlocfilehash: f122ec2474c09cdd6c9ada4ddc59b1adb44f619f
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/05/2019
ms.locfileid: "68779993"
---
# <a name="prepay-for-azure-app-service-isolated-stamp-fee-with-reserved-capacity"></a>Vorauszahlen der Azure App Service-Stempelgebühr (gesondert) mit reservierter Kapazität

Sie können bei den Azure App Service-Stempelgebühren (gesondert) sparen, indem Sie Ihre Stempelnutzung für drei Jahre im Voraus bezahlen. Um reservierte Kapazität für die Stempelgebühr (gesondert) zu erwerben, müssen Sie sowohl die Azure-Region, in der der Stempel bereitgestellt wird, als auch die Anzahl der zu erwerbenden Stempel auswählen.

Wenn Sie eine Reservierung erwerben, wird die Nutzung der Stempelgebühr (gesondert), die den Reservierungsattributen entspricht, nicht mehr zu den Preisen für die nutzungsbasierte Bezahlung abgerechnet. Die Reservierung wird automatisch auf die Anzahl gesonderter Stempel angewendet, die dem reservierten Kapazitätsspektrum und der Region entsprechen. Es ist nicht erforderlich, einem gesonderten Stempel eine Reservierung zuzuweisen. Die Reservierung gilt nicht für Worker. Jede andere dem Stempel zugeordnete Ressource wird also separat abgerechnet.

Nach Ablauf der reservierten Kapazität werden gesonderte Stempel zwar weiterhin ausgeführt, dann aber zu den Preisen für die nutzungsbasierte Bezahlung abgerechnet. Reservierungen werden nicht automatisch verlängert.

## <a name="determine-the-right-reservation-to-purchase"></a>Bestimmen der richtigen Reservierung für den Erwerb

Wenn Sie eine Reservierung erwerben, bezahlen Sie vorab für die dauerhafte Nutzung reservierter Mengen innerhalb der nächsten drei Jahre. Ermitteln Sie anhand Ihrer Nutzungsdaten, wie viele Stempel für App Service (isoliert) Sie konsistent nutzen und voraussichtlich in Zukunft nutzen werden.

Machen Sie sich außerdem damit vertraut, wie der gesonderte Stempel Linux- oder Windows-Verbrauchseinheiten ausgibt.

- Ein leerer gesonderter Stempel gibt standardmäßig die Windows-Verbrauchseinheit aus – also beispielsweise ohne bereitgestellte Worker. Diese Verbrauchseinheit wird weiter ausgegeben, wenn Windows-Worker für den Stempel bereitgestellt werden.
- Wenn Sie einen Linux-Worker bereitstellen, ändert sich die Verbrauchseinheit in die Verbrauchseinheit für Linux-Stempel.
- Wurden sowohl Linux- als auch Windows-Worker bereitgestellt, gibt der Stempel die Windows-Verbrauchseinheit aus.

Die Verbrauchseinheit kann sich also im Laufe der Lebensdauer des Stempels zwischen Windows und Linux ändern.

Erwerben Sie Windows-Stempelreservierungen, wenn für den Stempel mindestens ein Windows-Worker vorhanden ist. Linux-Stempelreservierung sollten nur erworben werden, wenn Sie _ausschließlich_ Linux-Worker für den Stempel verwenden möchten.

## <a name="buy-isolated-stamp-reserved-capacity"></a>Erwerben von reservierter Kapazität für gesonderte Stempel

Reservierte Kapazität für gesonderte Stempel kann über das [Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/documentation/filters/%7B%22reservedResourceType%22%3A%22AppService%22%7D) erworben werden. Für den Erwerb von reservierter Kapazität müssen Sie über die Besitzerrolle für mindestens ein Enterprise- oder Einzelabonnement mit nutzungsbasierten Tarifen verfügen.

- Bei Enterprise-Abonnements muss im [EA-Portal](https://ea.azure.com/) die Option **Reservierte Instanzen hinzufügen** aktiviert werden. Sollte diese Einstellung deaktiviert sein, müssen Sie EA-Administrator sein.
- Für das Cloud Solution Provider-Programm (CSP) können nur die Administrator- oder Vertriebs-Agents reservierte Kapazität für SQL Data Warehouse erwerben.

**Vorgehensweise für den Erwerb:**

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/documentation/filters/%7B%22reservedResourceType%22%3A%22AppService%22%7D).
1. Wählen Sie ein Abonnement aus. Wählen Sie in der Liste **Abonnement** das Abonnement aus, das für die Bezahlung der reservierten Kapazität verwendet wird. Die Zahlungsmethode für das Abonnement wird mit Vorauszahlungen für die reservierte Kapazität belastet. Der Abonnementtyp muss „Enterprise Agreement“ (Angebotsnummern: MS-AZR-0017P oder MS-AZR-0148P) oder „Nutzungsbasierte Zahlung“ (Angebotsnummern: MS-AZR-0003P oder MS-AZR-0023P) oder ein CSP-Abonnement sein.
    - Bei einem Enterprise-Abonnement werden die Gebühren vom Verpflichtungsguthaben der Reservierung abgezogen oder als Überschreitung belastet.
    - Bei einem Abonnement mit nutzungsbasierter Zahlung wird die Kreditkarte mit den Gebühren belastet, oder die Gebühren werden für die Zahlung auf Rechnung in Rechnung gestellt.
1. Wählen Sie unter **Bereich** einen Abonnementbereich aus.
    - **Einzelne Ressourcengruppe**: Wendet den Reservierungsrabatt nur auf die entsprechenden Ressourcen in der ausgewählten Ressourcengruppe an.
    - **Einzelnes Abonnement**: Wendet den Reservierungsrabatt auf die entsprechenden Ressourcen im ausgewählten Abonnement an.
    - **Gemeinsam genutzt**: Wendet den Reservierungsrabatt auf die entsprechenden Ressourcen in berechtigten Abonnements innerhalb des Abrechnungskontexts an. Für Kunden mit einem Enterprise Agreement ist der Abrechnungskontext die Registrierung. Für Kunden mit individuellen Abonnements mit nutzungsbasierten Tarifen handelt es sich beim Abrechnungsbereich um alle berechtigten Abonnements, die vom Kontoadministrator erstellt wurden.
1. Wählen Sie unter **Region** eine Azure-Region aus, die durch die reservierte Kapazität abgedeckt ist, und fügen Sie die Reservierung dem Warenkorb hinzu.
1. Wählen Sie einen isolierten Plantyp aus, und klicken Sie anschließend auf **Auswählen**.  
    ![Beispiel ](./media/billing-prepay-app-service-isolated-stamp/app-service-isolated-stamp-select.png)
1. Geben Sie die Anzahl der zu reservierenden Stempel für App Service (isoliert) ein. Wenn Sie also beispielsweise die Menge „3“ angeben, erhalten Sie drei reservierte Stempel pro Region. Klicken Sie auf **Weiter: Review + Buy** (Weiter: Überprüfen und kaufen).
1. Überprüfen Sie die Angaben, und klicken Sie auf **Jetzt kaufen**.

Nach dem Erwerb können Sie unter [Reservierungen](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) jederzeit den Status des Erwerbs anzeigen und überwachen.

## <a name="cancel-exchange-or-refund-reservations"></a>Stornieren, Umtauschen oder Rückerstatten von Reservierungen

Reservierungen können unter bestimmten Einschränkungen storniert, umgetauscht oder rückerstattet werden. Weitere Informationen finden Sie unter [Self-Service-Umtausch und -Rückerstattungen für Azure-Reservierungen](billing-azure-reservations-self-service-exchange-and-refund.md).

## <a name="discount-application-shown-in-usage-data"></a>Anwendung des Rabatts in den Nutzungsdaten

Ihre Nutzungsdaten haben für die Nutzung, für die der Reservierungsrabatt gilt, effektiv den Preis „0“. Die Nutzungsdaten zeigen den Reservierungsrabatt für jede Stempelinstanz in jeder Reservierung.

Weitere Informationen zur Anzeige von Reservierungsrabatt in Nutzungsdaten für EA-Kunden (Enterprise Agreement) finden Sie unter [Abrufen von Reservierungskosten und Nutzung laut Enterprise Agreement](billing-understand-reserved-instance-usage-ea.md). Informationen für andere Kunden finden Sie unter [Informationen zur Azure-Reservierungsnutzung bei Ihrem einzelnen Abonnement mit nutzungsbasierter Bezahlung](billing-understand-reserved-instance-usage.md).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:
  - [Was sind Azure-Reservierungen?](billing-save-compute-costs-reservations.md)
  - [How reservation discounts apply to Azure App Service Isolated Stamps](billing-reservation-discount-app-service-isolated-stamp.md) (Anwendung von Reservierungsrabatten auf Azure App Service-Stempelgebühren (gesondert))
  - [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](billing-understand-reserved-instance-usage-ea.md)

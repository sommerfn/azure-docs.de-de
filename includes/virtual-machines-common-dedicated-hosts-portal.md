---
title: include file
description: include file
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 07/25/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 262880997c6b065dc5293a18d9a07c52ac836f37
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73590813"
---
> [!IMPORTANT]
> Azure Dedicated Host – der Dienst für dedizierte Azure-Hosts – ist derzeit als Public Preview verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>
> **Bekannte Einschränkungen der Vorschau**
> - VM-Skalierungsgruppen werden zurzeit auf dedizierten Hosts nicht unterstützt.
> - Das erste Vorschaurelease unterstützt die folgende VM-Serien: DSv3 und ESv3. 


## <a name="create-a-host-group"></a>Erstellen einer Hostgruppe

Eine **Hostgruppe** ist eine neue Ressource, die eine Sammlung dedizierter Hosts darstellt. Sie erstellen eine Hostgruppe in einer Region und einer Verfügbarkeitszone und fügen ihr Hosts hinzu. Bei der Planung für Hochverfügbarkeit stehen zusätzliche Optionen zur Verfügung. Sie können eine oder beide der folgenden Optionen mit Ihren dedizierten Hosts verwenden: 
- Ausdehnen über mehrere Verfügbarkeitszonen. In diesem Fall muss in jeder Zone, die Sie verwenden möchten, eine Hostgruppe vorhanden sein.
- Ausdehnen über mehrere Fehlerdomänen, die physischen Racks zugeordnet sind. 
 
In beiden Fällen müssen Sie die Anzahl der Fehlerdomänen für Ihre Hostgruppe angeben. Wenn Ihre Gruppe keine Fehlerdomänen umfassen soll, verwenden Sie die Anzahl 1 von Fehlerdomänen. 

Sie können auch sowohl Verfügbarkeitszonen als auch Fehlerdomänen verwenden. 

In diesem Beispiel erstellen Sie eine Hostgruppe mit einer Verfügbarkeitszone und zwei Fehlerdomänen. 


1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
1. Wählen Sie links oben **Ressource erstellen** aus.
1. Suchen Sie nach **Host group** (Hostgruppe), und wählen Sie dann in den Ergebnissen **Host Groups (preview)** (Hostgruppen (Vorschauversion)) aus.

    ![Ergebnis der Suche nach „Host group“.](./media/virtual-machines-common-dedicated-hosts-portal/host-group.png)
1. Wählen Sie auf der Seite **Host Groups (preview)** (Hostgruppen (Vorschauversion)) die Option **Erstellen** aus.
1. Wählen Sie das Abonnement aus, das Sie verwenden möchten, und wählen Sie dann **Neu erstellen** aus, um eine neue Ressourcengruppe zu erstellen.
1. Geben Sie *myDedicatedHostsRG* unter **Name** ein, und wählen Sie dann **OK** aus.
1. Geben Sie unter **Host group name** (Name der Hostgruppe) die Zeichenfolge *myHostGroup* ein.
1. Wählen Sie unter **Standort** die Option **USA, Osten** aus.
1. Wählen Sie unter **Verfügbarkeitszone** die Option **1** aus.
1. Wählen Sie für **Fault domain count** (Anzahl von Fehlerdomänen) die Option **2** aus.
1. Wählen Sie **Bewerten + erstellen** aus, und warten Sie auf die Überprüfung.

    ![Hostgruppeneinstellungen](./media/virtual-machines-common-dedicated-hosts-portal/host-group-settings.png)
1. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus, um die Hostgruppe zu erstellen.

Die Erstellung der Hostgruppe sollte nur wenige Sekunden dauern.

## <a name="create-a-dedicated-host"></a>Erstellen eines dedizierten Hosts

Erstellen Sie nun einen dedizierten Host in der Hostgruppe. Zusätzlich zu einem Namen müssen Sie die SKU für den Host angeben. Die Host-SKU erfasst die unterstützte VM-Serie sowie die Hardwaregenerierung für Ihren dedizierten Host.  Während der Vorschau werden die folgenden Host-SKU-Werte unterstützt: „DSv3_Type1“ und „ESv3_Type1“.

Weitere Informationen zu den Host-SKUs und Preisen finden Sie unter [Azure Dedicated Host – Preise](https://aka.ms/ADHPricing).

Beim Festlegen der Anzahl der Fehlerdomänen für die Hostgruppe werden Sie aufgefordert, die Fehlerdomäne für den Host anzugeben.  

1. Wählen Sie links oben **Ressource erstellen** aus.
1. Suchen Sie nach **Dedicated host** (Dedizierter Host), und wählen Sie dann in den Ergebnissen **Dedicated Host (preview)** (Dedicated Host (Vorschauversion)) aus.

    ![Ergebnis der Suche nach „Host group“.](./media/virtual-machines-common-dedicated-hosts-portal/host.png)
1. Wählen Sie auf der Seite **Dedicated Host (preview)** (Dedicated Host (Vorschauversion)) die Option **Erstellen** aus.
1. Wählen Sie das Abonnement aus, das Sie verwenden möchten.
1. Wählen Sie *myDedicatedHostsRG* unter **Ressourcengruppe** aus.
1. Geben Sie unter **Instanzdetails** die Zeichenfolge *myHost* für **Name** ein, und wählen Sie als Standort *USA, Osten* aus.
1. Wählen Sie unter **Hardwareprofil** die Option *Standard Es3 family – Type 1* (Standard-Es3-Familie – Typ 1) unter **Größenfamilie**, *myHostGrup* unter **Hostgruppe** und dann *1* unter **Fehlerdomäne** aus. Behalten Sie in den restlichen Feldern die Standardwerte bei.
1. Wählen Sie nach Abschluss **Bewerten + erstellen** aus, und warten Sie auf die Validierung.

    ![Hosteinstellungen](./media/virtual-machines-common-dedicated-hosts-portal/host-settings.png)
1. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus, um den Host zu erstellen.



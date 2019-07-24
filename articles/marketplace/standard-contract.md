---
title: Standardvertrag | Azure
description: Standardvertrag in Azure Marketplace und AppSource
services: Azure, Marketplace, Compute, Storage, Networking
author: qianw211
ms.service: marketplace
ms.topic: article
ms.date: 07/05/2019
ms.author: ellacroi
ms.openlocfilehash: 80c157423572d356026f257e81d52650ce01d3e8
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2019
ms.locfileid: "67620390"
---
# <a name="standard-contract"></a>Standardvertrag

Zur Vereinfachung des Beschaffungsprozesses für Kunden und zur Verringerung der rechtlichen Komplexität für Softwareanbieter stellt Microsoft die Vorlage „Standardvertrag“ bereit, um eine Transaktion im Marketplace zu erleichtern. Statt benutzerdefinierte Geschäftsbedingungen zu erstellen, können Azure Marketplace-Herausgeber ihre Software wahlweise unter dem Standardvertrag anbieten, den Kunden nur einmal überprüfen und akzeptieren müssen. Der Standardvertrag ist hier zu finden: [https://go.microsoft.com/fwlink/?linkid=2041178](https://go.microsoft.com/fwlink/?linkid=2041178). 

Die Geschäftsbedingungen für ein Angebot werden bei dessen Erstellung im Cloud-Partnerportal auf der Registerkarte „Azure Marketplace“ definiert. Die Option „Standardvertrag“ wird durch Ändern der Einstellung auf „Ja“ aktiviert.

![Aktivieren der Option „Standardvertrag“](media/marketplace-publishers-guide/standard-contract.png)

>[!Note] 
>Wenn Sie auswählen, dass Sie den Standardvertrag verwenden möchten, sind für den Kanal [Cloud Solution Provider](./cloud-solution-providers.md) weiterhin separate Geschäftsbedingungen erforderlich.

## <a name="standard-contract-amendments"></a>Zusatzvereinbarungen für den Standardvertrag

Mithilfe von Zusatzvereinbarungen für den Standardvertrag können Herausgeber der Einfachheit halber den Standardvertrag auswählen und mit angepassten Bedingungen für ihr Produkt oder Unternehmen versehen.  Kunden, die bereits den Microsoft-Standardvertrag geprüft und akzeptiert haben, müssen lediglich die Zusatzvereinbarungen zum Vertrag prüfen.

Für Azure Marketplace-Herausgeber stehen zwei Arten von Zusatzvereinbarungen zur Verfügung:

* Universelle Zusatzvereinbarungen: Diese Zusatzvereinbarungen werden universell auf den Standardvertrag für alle Kunden angewendet. Universelle Zusatzvereinbarungen werden jedem Kunden des Produkts im Rahmen der Kaufabwicklung angezeigt.

![Universelle Zusatzvereinbarungen](media/marketplace-publishers-guide/universal-amendaments.png)

* Benutzerdefinierte Zusatzvereinbarungen: Azure Marketplace ermöglicht auch die Verwendung benutzerdefinierter Zusatzvereinbarungen für bestimmte Mandanten. Hierbei handelt es sich um spezielle Zusatzvereinbarungen für den Standardvertrag, die nur bestimmte Kunden betreffen. Herausgeber können den gewünschten Mandanten auswählen. Für Kunden dieses Mandanten gelten dann beim Produkterwerb der Standardvertrag und die speziellen Zusatzvereinbarungen.

![Benutzerdefinierte Zusatzvereinbarungen](media/marketplace-publishers-guide/custom-amendaments.png)

>[!Note] 
>Kunden, für die benutzerdefinierte Zusatzvereinbarungen gelten, erhalten beim Kauf auch die universellen Zusatzvereinbarung für die Standardbedingungen.

>[!Note]
>Zusatzvereinbarungen für den Standardvertrag werden für folgende Angebotstypen unterstützt: Azure-Anwendungen (Lösungsvorlagen und verwaltete Anwendungen), virtuelle Computer, Container und Containeranwendungen.

### <a name="customer-experience"></a>Darstellung für Kunden

Im Rahmen des Kaufvorgangs werden den Kunden im Azure-Portal die Bedingungen für das Produkt in Form des Microsoft-Standardvertrags und der Zusatzvereinbarungen angezeigt.

![Darstellung für Kunden im Azure-Portal](media/marketplace-publishers-guide/ibiza-customer-experience.png)

### <a name="api"></a>API

Kunden können die Bedingungen eines Angebots mithilfe von `Get-AzureRmMarketplaceTerms` abrufen und akzeptieren. Der Standardvertrag und die zugehörigen Zusatzvereinbarungen werden in der Ausgabe des Cmdlets zurückgegeben.

---

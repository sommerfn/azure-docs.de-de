---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/19/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 6ad5bafe66e6e39503d3987aa6ddd9159c532fa0
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72237355"
---
Die Rechnungsstellung für Azure Storage basiert auf der Nutzung Ihres Speicherkontos. Alle Objekte in einem Speicherkonto werden zusammen als Gruppe abgerechnet. 

Speicherkosten werden gemäß den folgenden Faktoren berechnet: Region/Standort, Kontotyp, Zugriffsebene, Speicherkapazität, Replikationsschema, Speichertransaktionen und Datenausgang.

* **Region** bezieht sich auf die geografische Region, in der sich Ihr Konto befindet.
* **Kontotyp** verweist auf den Typ des Speicherkontos, das Sie verwenden. 
* **Zugriffsebene** bezieht sich auf das Datenverwendungsmuster, das Sie für Ihr Konto vom Typ „general-purpose v2“ oder Ihr Blobspeicherkonto angegeben haben.
* **Speicherkapazität** bezieht sich darauf, wie viel von Ihrer Speicherkontozuweisung zum Speichern von Daten verwendet wird.
* Die **Replikation** bestimmt, wie viele Kopien Ihrer Daten jeweils an welchen Standorten unterhalten werden.
* **Transaktionen** beziehen sich auf alle Lese- und Schreibvorgänge in Azure Storage.
* **Datenausgang** bezieht sich alle Daten, die aus einer Azure-Region übertragen werden. Wenn eine Anwendung, die nicht in der gleichen Region ausgeführt wird, auf die Daten in Ihrem Speicherkonto zugreift, fallen Gebühren für den Datenausgang an. Weitere Informationen zur Verwendung von Ressourcengruppen zum Gruppieren von Daten und Diensten in der gleichen Region zur Beschränkung vom Gebühren für den Datenausgang, finden Sie unter [Was ist eine Azure-Ressourcengruppe?](https://docs.microsoft.com/azure/cloud-adoption-framework/govern/resource-consistency/resource-access-management#what-is-an-azure-resource-group). 

Die Seite [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/) bietet detaillierte Preisinformationen basierend auf Kontotyp, Speicherkapazität, Replikation und Transaktionen. In der [Datenübertragungs-Preisübersicht](https://azure.microsoft.com/pricing/details/data-transfers/) finden Sie detaillierte Preisinformationen für den Datenausgang. Sie können den [Azure Storage-Preisrechner](https://azure.microsoft.com/pricing/calculator/?scenario=data-management) verwenden, um Ihre Kosten zu bestimmen.


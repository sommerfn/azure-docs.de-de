---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/23/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 8908ded31b96aac50db1fc25e92c2c0a8e960453
ms.sourcegitcommit: 3fa4384af35c64f6674f40e0d4128e1274083487
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2019
ms.locfileid: "71219032"
---
Lokal redundanter Speicher (LRS) repliziert Ihre Daten dreimal innerhalb eines einzelnen Rechenzentrums. LRS stellt eine Dauerhaftigkeit von mindestens 99,999999999 % (11 Neunen) für Objekte in einem bestimmten Jahr bereit. LRS ist die kostengünstigste Replikationsmöglichkeit und bietet im Vergleich zu anderen Optionen die geringste Dauerhaftigkeit.

Bei einem Katastrophenfall in einem Rechenzentrum (Feuer, Überschwemmung usw.) gehen ggf. alle Replikate in einem Speicherkonto, das LRS verwendet, verloren oder können nicht mehr wiederhergestellt werden. Um dieses Risiko zu minimieren, wird empfohlen, zonenredundanten Speicher (ZRS), georedundanten Speicher (GRS) oder geozonenredundanten Speicher (GZRS) zu verwenden.

Eine Schreibanforderung an ein LRS-Speicherkonto wird erst dann erfolgreich zurückgegeben, nachdem die Daten in alle drei Replikate geschrieben wurden.

Möglicherweise möchten Sie LRS in den folgenden Szenarios verwenden:

* Wenn Ihre Anwendung Daten speichert, die bei Datenverlust problemlos wiederhergestellt werden können, können Sie sich für LRS entscheiden.
* Einige Anwendungen sind aufgrund von Datenvorschriften auf die Replikation von Daten innerhalb eines Lands/einer Region beschränkt. In einigen Fällen können sich die gekoppelten Regionen, in denen die Daten für GRS-Konten repliziert werden, in einem anderen Land/einer anderen Region befinden. Weitere Informationen zu gekoppelten Regionen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/).

---
title: Migrieren eines Azure CDN-Profils von Verizon Standard zu Verizon Premium | Microsoft-Dokumentation
description: Hier finden Sie ausführliche Informationen zum Migrieren eines Profils von Verizon Standard zu Verizon Premium.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2018
ms.author: magattus
ms.custom: ''
ms.openlocfilehash: 7768dde424aedc295b53512db50c9dfc9db9ab8c
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2018
ms.locfileid: "49091469"
---
# <a name="migrate-an-azure-cdn-profile-from-standard-verizon-to-premium-verizon"></a>Migrieren eines Azure CDN-Profils von Verizon Standard zu Verizon Premium

Wenn Sie zur Verwaltung Ihrer Endpunkte ein Azure CDN-Profil (Content Delivery Network) erstellen, haben Sie die Wahl zwischen vier verschiedenen Produkten. Informationen zu den verschiedenen Produkten und den jeweils verfügbaren Features finden Sie unter [Vergleichen von Azure CDN-Produktfeatures](cdn-features.md).

Wenn Sie zur Verwaltung Ihrer CDN-Endpunkte ein Profil vom Typ **Azure CDN Standard von Verizon** erstellt haben, können Sie ein Upgrade auf ein Profil vom Typ **Azure CDN Premium von Verizon** durchführen. Durch das Upgrade gehen keine Ihrer CDN-Endpunkte oder Daten verloren. 

> [!IMPORTANT]
> Ein Profil vom Typ **Azure CDN Premium von Verizon** kann nach dem Upgrade nicht mehr in ein Profil vom Typ **Azure CDN Standard von Verizon** konvertiert werden.
> 

Wenn Sie ein Profil vom Typ **Azure CDN Standard von Verizon** upgraden möchten, wenden Sie sich an den [Microsoft-Support](https://azure.microsoft.com/support/options/).

## <a name="profile-comparison"></a>Profilvergleich
Profile vom Typ **Azure CDN Premium von Verizon** unterscheiden sich in folgenden wichtigen Punkten von Profilen vom Typ **Azure CDN Standard von Verizon**:
- Für bestimmte Azure CDN-Features (etwa [Komprimierung](cdn-improve-performance.md), [Cacheregeln](cdn-caching-rules.md) und [Geofilterung](cdn-restrict-access-by-country.md)) muss anstelle der Azure CDN-Schnittstelle das Verizon-Portal (über die Schaltfläche **Verwalten**) verwendet werden.
- API: Anders als bei Verizon Standard können Sie diese Features, auf die über das Verizon Premium-Portal zugegriffen wird, nicht über die API steuern. Über die API können jedoch andere allgemeine Features gesteuert werden – beispielsweise das Erstellen/Löschen eines Endpunkts, das Bereinigen/Laden zwischengespeicherter Assets und das Aktivieren/Deaktivieren einer benutzerdefinierten Domäne.
- Preise: Die Verizon Premium-Preisstruktur für Datenübertragungen unterscheidet sich von der Preisstruktur für Verizon Standard. Weitere Informationen finden Sie unter [Azure Content Delivery Network – Preise](https://azure.microsoft.com/pricing/details/cdn/).

Profile vom Typ **Azure CDN Premium von Verizon** bieten folgende Zusatzfeatures:
- [Tokenauthentifizierung:](cdn-token-auth.md) Ermöglicht es Benutzern, ein Token für den Abruf sicherer Ressourcen zu beziehen und zu verwenden.
- [Regel-Engine:](cdn-rules-engine.md) Ermöglicht das Anpassen der Verarbeitung von HTTP-Anforderungen.
- Tools für die erweiterte Analyse:
   - [Detaillierte HTTP-Analyse](cdn-advanced-http-reports.md)
   - [Edgeleistungsanalyse](cdn-edge-performance.md)
   - [Echtzeitanalyse](cdn-real-time-alerts.md)


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Regel-Engine finden Sie in der [Referenz für die Azure CDN-Regel-Engine](cdn-rules-engine-reference.md).


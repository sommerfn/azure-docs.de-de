---
title: Grenzwerte von Azure Analysis Services-Ressourcen und -Objekten | Microsoft-Dokumentation
description: Beschreibt die Grenzwerte von Azure Analysis Services-Ressourcen und -Objekten.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 08/23/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 9fe43602c66af72357e16822ee9d4b5a741d3f86
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2019
ms.locfileid: "72298701"
---
# <a name="analysis-services-resource-and-object-limits"></a>Grenzwerte von Azure Analysis Services-Ressourcen und -Objekten

In diesem Artikel beschreibt Ressourcen- und Modellobjektgrenzwerte.

## <a name="tier-limits"></a>Tarifgrenzwerte

Informationen zu QPU- und Arbeitsspeicherlimits für die Tarife „Developer“, „Basic“ und „Standard“ finden Sie auf der [Seite mit der Azure Analysis Services-Preisübersicht](https://azure.microsoft.com/pricing/details/analysis-services/).

## <a name="object-limits"></a>Objektgrenzwerte

Hierbei handelt um theoretische Grenzwerte. Die Leistung wird bei geringeren Werten beeinträchtigt.

|Object|Maximale Größe/Anzahl|  
|------------|----------------------------|  
|Datenbanken in einer Instanz|16.000|  
|Kombinierte Anzahl an Tabellen und Spalten in einer Datenbank|16.000|  
|Zeilen in einer Tabelle|Unbegrenzt<br /><br /> **Warnung:** Mit der Einschränkung, dass keine einzelne Spalte in der Tabelle mehr als 1.999.999.999.999.997 verschiedene Werte haben kann.|  
|Hierarchien in einer Tabelle|15.999|  
|Ebenen in einer Hierarchie|15.999|  
|Beziehungen|8\.000|  
|Schlüsselspalten in allen Tabellen|15.999|  
|Messungen in Tabellen|2^31-1 = 2.147483.647|  
|Von einer Abfrage zurückgegebene Zellen|2^31-1 = 2.147483.647|  
|Datensatzgröße der Quellabfrage|64 K|  
|Länge des Objektnamens|512 Zeichen|  



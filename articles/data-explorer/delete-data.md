---
title: Löschen von Daten aus dem Azure-Daten-Explorer
description: Dieser Artikel beschreibt Szenarien zum Massenlöschen im Azure-Daten-Explorer, einschließlich endgültiger und beibehaltungsbasierter Löschvorgänge.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 48336b424ef1783d8eb123ac45bfc03b98d5481b
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2019
ms.locfileid: "58756977"
---
# <a name="delete-data-from-azure-data-explorer"></a>Löschen von Daten aus dem Azure-Daten-Explorer

Der Azure-Daten-Explorer unterstützt mehrere Ansätze zum Massenlöschen, die in diesem Artikel behandelt werden. Das Löschen einzelner Datensätze in Echtzeit wird nicht unterstützt, weil er für schnellen Lesezugriff optimiert ist.

* Wenn eine oder mehrere Tabellen nicht mehr benötigt werden, löschen Sie sie über den Befehl „drop table“ bzw. „drop tables“.

    ```Kusto
    .drop table <TableName>

    .drop tables (<TableName1>, <TableName2>,...)
    ```

* Wenn alte Daten nicht mehr erforderlich sind, löschen Sie sie, indem Sie die Beibehaltungsdauer auf Datenbank- oder Tabellenebene ändern.

    Stellen Sie sich eine Datenbank oder Tabelle vor, für die eine Beibehaltungsdauer von 90 Tagen festgelegt ist. Das Unternehmen braucht Veränderung, daher werden nur noch Daten aus den letzten 60 Tage benötigt. In diesem Fall löschen Sie die älteren Daten anhand einer der folgenden Methoden.

    ```Kusto
    .alter-merge database <DatabaseName> policy retention softdelete = 60d

    .alter-merge table <TableName> policy retention softdelete = 60d
    ```

    Weitere Informationen finden Sie unter [Aufbewahrungsrichtlinie](https://docs.microsoft.com/azure/kusto/concepts/retentionpolicy).

Wenn Sie Hilfe bei Datenlöschvorgängen benötigen, erstellen Sie eine Supportanfrage im [Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).

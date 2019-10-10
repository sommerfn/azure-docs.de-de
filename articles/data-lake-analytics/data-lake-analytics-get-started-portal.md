---
title: Erstellen und Ausführen von Abfragen in Azure Data Lake Analytics – Azure-Portal
description: Verwenden Sie das Azure-Portal, um ein Azure Data Lake Analytics-Konto zu erstellen und einen U-SQL-Auftrag zu übermitteln.
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: b1584d16-e0d2-4019-ad1f-f04be8c5b430
ms.topic: conceptual
ms.date: 03/21/2017
ms.openlocfilehash: 46da3750e4d0ac78c5fd9df91ae37670e541302d
ms.sourcegitcommit: 9fba13cdfce9d03d202ada4a764e574a51691dcd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2019
ms.locfileid: "71315760"
---
# <a name="get-started-with-azure-data-lake-analytics-using-the-azure-portal"></a>Erste Schritte mit Azure Data Lake Analytics mithilfe des Azure-Portals
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

In diesem Artikel erfahren Sie, wie Sie über das Azure-Portal Azure Data Lake Analytics-Konten erstellen, Aufträge in [U-SQL](data-lake-analytics-u-sql-get-started.md) definieren und Aufträge an den Data Lake Analytics-Dienst übermitteln.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial wird ein **Azure-Abonnement** benötigt. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-data-lake-analytics-account"></a>Erstellen eines Data Lake Analytics-Kontos

Nun erstellen Sie gleichzeitig ein Data Lake Analytics- und ein Azure Data Lake Storage Gen1-Konto.  Dieser Schritt ist einfach und dauert nur etwa 60 Sekunden.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf **Ressource erstellen** >  **Daten + Analysen** > **Data Lake Analytics**.
3. Wählen Sie Werte für die folgenden Elemente:
   * **Name**: Geben Sie dem Data Lake Analytics-Konto einen Namen (nur Kleinbuchstaben und Zahlen sind zulässig).
   * **Abonnement**: Wählen Sie das Azure-Abonnement aus, das für das Analytics-Konto verwendet wird.
   * **Ressourcengruppe**. Wählen Sie eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue Ressourcengruppe.
   * **Standort**: Wählen Sie ein Azure-Rechenzentrum für das Data Lake Analytics-Konto aus.
   * **Data Lake Storage Gen1:** Führen Sie die Anweisungen zum Erstellen eines neuen Data Lake Storage Gen1-Kontos aus, oder wählen Sie ein vorhandenes Konto aus. 
4. Wählen Sie optional einen Tarif für Ihr Data Lake Analytics-Konto aus.
5. Klicken Sie auf **Create**. 


## <a name="your-first-u-sql-script"></a>Ihr erstes U-SQL-Skript

Der folgende Text ist ein sehr einfaches U-SQL-Skript. Es definiert lediglich ein kleines Dataset im Skript. Anschließend wird dieses Dataset als Datei mit dem Namen `/data.csv` in das Data Lake Storage Gen1-Standardkonto geschrieben.

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

## <a name="submit-a-u-sql-job"></a>Senden eines U-SQL-Auftrags

1. Klicken Sie im Data Lake Analytics-Konto auf **Neuer Auftrag**.
2. Fügen Sie den Text des obigen U-SQL-Skripts ein. Geben Sie einen Namen für den Auftrag ein. 
3. Klicken Sie auf die Schaltfläche **Übermitteln**, um den Auftrag zu starten.   
4. Überwachen Sie den **Status** des Auftrags, und warten Sie, bis sich der Auftragsstatus in **Erfolgreich** ändert.
5. Klicken Sie auf die Registerkarte **Daten** und anschließend auf die Registerkarte **Ausgaben**. Wählen Sie die Ausgabedatei `data.csv` aus, und zeigen Sie die Ausgabedaten an.

## <a name="see-also"></a>Weitere Informationen

* Informationen zu den ersten Schritten der Entwicklung von U-SQL-Anwendungen finden Sie unter [Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
* Informationen zum Erlernen von U-SQL finden Sie unter [Erste Schritte mit der Sprache U-SQL für Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).
* Informationen zu Verwaltungsaufgaben finden Sie unter [Verwalten von Azure Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-manage-use-portal.md).

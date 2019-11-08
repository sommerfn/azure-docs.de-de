---
title: Synchronisieren von Daten aus Azure SQL Database Edge mithilfe der SQL-Datensynchronisierung | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Daten aus Azure SQL Database Edge mithilfe der Azure SQL-Datensynchronisierung synchronisieren.
keywords: SQL Database Edge,Synchronisieren von Daten aus SQL Database Edge,SQL Database Edge-Datensynchronisierung
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: tutorial
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 11/04/2019
ms.openlocfilehash: 857cee30ac4c1002fb7ca57d6be5fa461a14e9ee
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73509194"
---
# <a name="tutorial-sync-data-from-sql-database-edge-to-azure-sql-database-using-sql-data-sync"></a>Tutorial: Synchronisieren von Daten zwischen SQL Database Edge und Azure SQL-Datenbank mithilfe der SQL-Datensynchronisierung

In diesem Tutorial erfahren Sie, wie Sie eine *Synchronisierungsgruppe* der SQL-Datensynchronisierung verwendet, um eine inkrementelle Synchronisierung von Daten zwischen Azure SQL Database Edge und Azure SQL-Datenbank durchzuführen. SQL-Datensynchronisierung ist ein Dienst, der auf Azure SQL-Datenbank basiert und mit dem Sie die ausgewählten Daten bidirektional über mehrere SQL-Datenbanken und SQL Server-Instanzen hinweg synchronisieren können. Weitere Informationen zur Azure SQL-Datensynchronisierung finden Sie unter [Azure SQL-Datensynchronisierung](../sql-database/sql-database-sync-data.md).

Azure SQL Database Edge basiert auf den neuesten Versionen der [Microsoft SQL Server-Datenbank-Engine](/sql/sql-server/sql-server-technical-documentation/). Aus diesem Grund kann jeder Mechanismus für die Datensynchronisierung, der auf eine lokale SQL Server-Instanz angewendet werden kann, auch zum Synchronisieren von Daten einer SQL Database Edge-Instanz genutzt werden, die auf einem Edgegerät ausgeführt wird.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial wird ein Windows-Computer benötigt, der mit dem [Azure SQL Data Sync Agent](../sql-database/sql-database-data-sync-agent.md) konfiguriert ist.

## <a name="before-you-begin"></a>Voraussetzungen

* Erstellen Sie eine Azure SQL-Datenbank-Instanz. Informationen zum Erstellen einer Azure SQL-Datenbank-Instanz mit dem Azure-Portal finden Sie unter [Erstellen einer Einzeldatenbank in Azure SQL-Datenbank](../sql-database/sql-database-single-database-get-started.md?tabs=azure-portal).

* Erstellen Sie die erforderlichen Tabellen und die weiteren benötigten Objekte in Ihrer Azure SQL-Datenbank-Bereitstellung.

* Erstellen Sie die erforderlichen Tabellen und die weiteren benötigten Objekte in Ihrer Azure SQL Database Edge-Bereitstellung. Weitere Informationen finden Sie unter [Verwenden von SQL-Datenbank-DAC-Paketen mit SQL Database Edge](stream-analytics.md).

* Registrieren Sie die Azure SQL Database Edge-Instanz beim Azure SQL Data Sync Agent. Weitere Informationen finden Sie unter [Hinzufügen einer lokalen SQL Server-Datenbank](../sql-database/sql-database-get-started-sql-data-sync.md#add-on-prem).

## <a name="sync-data-between-an-azure-sql-database-and-sql-database-edge"></a>Synchronisieren von Daten zwischen einer Azure SQL-Datenbank-Instanz und SQL Database Edge

Die Einrichtung der Synchronisierung zwischen einer Azure SQL-Datenbank- und einer SQL Database Edge-Instanz mithilfe der SQL-Datensynchronisierung umfasst drei wichtige Schritte.  

1. Verwenden Sie das Azure-Portal, um eine Synchronisierungsgruppe zu erstellen. Informationen zum Erstellen der Synchronisierungsgruppe finden Sie unter [Erstellen einer Synchronisierungsgruppe mit dem Azure-Portal](../sql-database/sql-database-get-started-sql-data-sync.md#create-sync-group). Es ist möglich, mit derselben *Hubdatenbank* verschiedene Synchronisierungsgruppen zu erstellen, um Daten unterschiedlicher SQL Database Edge-Instanzen mit einer oder mehreren SQL-Datenbanken in Azure zu synchronisieren.

2. Fügen Sie Synchronisierungsmitglieder zur Synchronisierungsgruppe hinzu. Informationen zum Hinzufügen von Mitgliedern zur Synchronisierungsgruppe finden Sie unter [Hinzufügen von Mitgliedern zur SQL-Datenbanksynchronisierungsgruppe](../sql-database/sql-database-get-started-sql-data-sync.md#add-sync-members).

3. Konfigurieren Sie die Synchronisierungsgruppe, um die Tabellen auszuwählen, die Teil dieser Synchronisierung sein sollen. Informationen zum Konfigurieren der Synchronisierungsgruppe finden Sie unter [Konfigurieren der Synchronisierungsgruppe](../sql-database/sql-database-get-started-sql-data-sync.md#add-sync-members).

Nachdem Sie die oben genannten Schritte ausgeführt haben, verfügen Sie über eine Synchronisierungsgruppe, die eine Azure SQL-Datenbank- und eine SQL Database Edge-Instanz umfasst.

Weitere Informationen zur SQL-Datensynchronisierung finden Sie in den folgenden Artikeln:

* [Datensynchronisierungs-Agent für die Azure SQL-Datensynchronisierung](../sql-database/sql-database-data-sync-agent.md)

* [Bewährte Methoden für die SQL-Datensynchronisierung](../sql-database/sql-database-best-practices-data-sync.md) und [Behandeln von Problemen mit der SQL-Datensynchronisierung](../sql-database/sql-database-troubleshoot-data-sync.md)

* [Überwachen der SQL-Datensynchronisierung mit Azure Monitor-Protokollen](../sql-database/sql-database-sync-monitor-oms.md)

* [Aktualisieren des Synchronisierungsschemas mit Transact-SQL](../sql-database/sql-database-update-sync-schema.md) oder mit [PowerShell](../sql-database/scripts/sql-database-sync-update-schema.md)

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden von PowerShell zum Synchronisieren zwischen Azure SQL-Datenbank und Azure SQL Database Edge](../sql-database/scripts/sql-database-sync-data-between-azure-onprem.md). Ersetzen Sie im Tutorial die Details zur Datenbank *OnPremiseServer* durch die Details für Azure SQL Database Edge.

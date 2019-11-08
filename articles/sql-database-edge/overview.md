---
title: Was ist Azure SQL Database Edge? | Microsoft-Dokumentation
description: Informationen zu Azure SQL Database Edge
keywords: Einführung in Azure SQL Database Edge,was ist Azure SQL Database Edge,Übersicht über Azure SQL Database Edge
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 11/04/2019
ms.openlocfilehash: 9cbfc17e7412b4d30f082354996721ee7b5d6d5b
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510643"
---
# <a name="what-is-azure-sql-database-edge-preview"></a>Was ist Azure SQL Database Edge (Vorschau)?

Azure SQL Database Edge (Vorschau) ist eine optimierte relationale Datenbank-Engine für IoT- und IoT Edge-Bereitstellungen. Azure SQL Database Edge bietet die Möglichkeit zur Erstellung eines Hochleistungs-Datenspeichers und einer Verarbeitungsschicht für IoT-Anwendungen und -Lösungen. Azure SQL Database Edge bietet Funktionen zum Streamen, Verarbeiten und Analysieren von relationalen und nicht relationalen Daten wie JSON-, Grafik- und Zeitreihendaten und ist damit die richtige Wahl für eine Vielzahl moderner IoT-Anwendungen.

Azure SQL Database Edge basiert auf den aktuellen Versionen der [Microsoft SQL Server-Datenbank-Engine](/sql/sql-server/sql-server-technical-documentation?toc=/azure/sql-database-edge/toc.json), die branchenführende Leistung, Sicherheit und Funktionen für die Abfrageverarbeitung bietet. Da Azure SQL Database Edge auf der gleichen Engine wie SQL Server und Azure SQL-Datenbank basiert, bietet es die gleiche T-SQL-Programmieroberfläche für die einfachere und schnellere Entwicklung von Anwendungen oder Lösungen und vereinfacht gleichzeitig die Anwendungsportierbarkeit zwischen IoT Edge-Geräten, Rechenzentren und der Cloud.

## <a name="deployment-models"></a>Bereitstellungsmodelle

Azure SQL Database Edge ist im Azure Marketplace verfügbar und kann als Modul für [Azure IoT Edge](../iot-edge/about-iot-edge.md) bereitgestellt werden. Weitere Informationen finden Sie unter [Bereitstellen von Azure SQL Database Edge](deploy-portal.md).<br>

![Übersichtsdiagramm zu Azure SQL Database Edge](media/overview/overview.png)

## <a name="editions-of-sql-database-edge"></a>Editionen von SQL Database Edge

SQL Database Edge wird in drei unterschiedlichen Editionen oder Softwareplänen bereitgestellt. Diese Editionen weisen denselben Umfang an Features auf und unterscheiden sich lediglich in Bezug auf die Nutzungsrechte und die Menge an unterstützten CPU-/Arbeitsspeicherressourcen.

   |**Planen**  |**Beschreibung**  |
   |---------|---------|
   |Developer  |  SKU nur für die Entwicklung, es gelten dieselben Drosselungslimits wie bei der unten aufgeführten SKU „Standard“. |
   |Standard   |  Der Plan „Standard“ unterstützt bis zu 4 CPUs und bis zu 32 GB an Arbeitsspeicher für den SQL Database Edge-Container. |
   |Premium    |  Die SKU „Premium“ unterstützt bis zu 8 Kerne und bis zu 64 GB an Arbeitsspeicher für den SQL Database Edge-Container. |

## <a name="pricing-and-availability"></a>Preise und Verfügbarkeit

Azure SQL Database Edge befindet sich zurzeit in der Vorschau. Weitere Informationen zu Preisen und Verfügbarkeit finden Sie unter [Azure SQL Database Edge](https://azure.microsoft.com/services/sql-database-edge/).

> [!IMPORTANT]
> Erläuterungen der funktionellen Unterschiede zwischen Azure SQL Database Edge und SQL Server sowie Unterschiede zwischen den verschiedenen Optionen für Azure SQL Database Edge finden Sie unter [Azure SQL Database Edge – Funktionen](https://azure.microsoft.com/services/sql-database-edge/).

## <a name="streaming-capabilities"></a>Streamingfunktionen  

Azure SQL Database Edge bietet integrierte Streamingfunktionen für die Echtzeitanalyse und die Verarbeitung komplexer Ereignisse. Die integrierte Streamingfunktionalität verwendet dieselben Konstrukte wie [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) und bietet ähnliche Funktionen wie [Azure Stream Analytics auf IoT Edge](../stream-analytics/stream-analytics-edge.md).

Die Streaming-Engine für Azure SQL Database Edge ist auf geringe Latenz, Resilienz, eine effiziente Bandbreitennutzung und Compliance ausgelegt.

## <a name="machine-learning-and-artificial-intelligence-capabilities"></a>Machine Learning-Funktionen und künstliche Intelligenz

Azure SQL Database Edge bietet integrierte ML- und Analysefunktionen durch Integration der ONNX-Runtime (Open Neural Network Exchange), die den Austausch von Deep Learning- und neuronalen Netzwerkmodellen zwischen verschiedenen Frameworks ermöglicht. Weitere Informationen zu ONNX finden Sie [hier](https://onnx.ai/). Die ONNX-Runtime bietet die Flexibilität, Modelle in einer Sprache oder mit den Tools Ihrer Wahl zu entwickeln und anschließend für die Ausführung in SQL Database Edge eine Konvertierung in das ONNX-Format durchzuführen. Weitere Informationen finden Sie unter [Machine Learning und künstliche Intelligenz mit ONNX in SQL Database Edge](onnx-overview.md).

## <a name="working-with-azure-sql-database-edge"></a>Arbeiten mit Azure SQL Database Edge

Azure SQL Database Edge sorgt für einfachere und produktivere Abläufe beim Entwickeln und Verwalten von Anwendungen. Benutzer können auf alle vertrauten Tools und Fähigkeiten zurückgreifen, um herausragende Apps und Lösungen für ihre IoT Edge-Anforderungen zu entwickeln. Benutzer können beispielsweise die folgenden Tools für die Entwicklung in SQL Database Edge nutzen:

- [Azure-Portal](https://portal.azure.com/): Eine webbasierte Anwendung für die Verwaltung aller Azure-Dienste.
- [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms/): Eine kostenlose, herunterladbare Clientanwendung für die Verwaltung beliebiger SQL-Infrastrukturen – von SQL Server bis SQL-Datenbank.
- [SQL Server Data Tools in Visual Studio](/sql/ssdt/download-sql-server-data-tools-ssdt/): Eine kostenlose, herunterladbare Clientanwendung, mit der Sie relationale SQL Server-Datenbanken, SQL-Datenbanken, Integration Services-Pakete, Analysis Services-Datenmodelle und Reporting Services-Berichte erstellen können.
- [Azure Data Studio](/sql/azure-data-studio/what-is/): Ein kostenloses, herunterladbares, plattformübergreifendes Datenbanktool für Datenprofis, die die Microsoft-Familie lokaler und Clouddatenplattformen unter Windows, macOS und Linux nutzen.
- [Visual Studio Code](https://code.visualstudio.com/docs): Ein kostenloser, herunterladbarer Open-Source-Code-Editor für Windows, macOS und Linux. Dieser Editor unterstützt Erweiterungen, etwa die [Erweiterung MSSQLSERVER](https://aka.ms/mssql-marketplace), zum Abfragen von Microsoft SQL Server, Azure SQL-Datenbank und Azure SQL Data Warehouse.


## <a name="next-steps"></a>Nächste Schritte

- Details zu Preisen und Verfügbarkeit finden Sie unter [Azure SQL Database Edge](https://azure.microsoft.com/services/sql-database-edge/).
- Fordern Sie eine Aktivierung von Azure SQL Database Edge für Ihr Abonnement an.
- Hinweise zu den ersten Schritten finden Sie hier:
  - [Bereitstellen von SQL Database Edge über das Azure-Portal](deploy-portal.md)
  - [Machine Learning und künstliche Intelligenz mit Azure SQL Database Edge](onnx-overview.md)

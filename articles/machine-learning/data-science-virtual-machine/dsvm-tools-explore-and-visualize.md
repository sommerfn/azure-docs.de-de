---
title: Tools zum Durchsuchen und Visualisieren von Daten
titleSuffix: Azure Data Science Virtual Machine
description: Tools zum Durchsuchen und Visualisieren von Daten für die Data Science-VM.
keywords: Data Science-Tools, virtuelle Computer für Data Science, Tools für Data Science, Linux Data Science
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: conceptual
ms.date: 10/11/2019
ms.openlocfilehash: a18c374df0cad7994af855089f1096d52fe30ce2
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72330703"
---
# <a name="data-exploration-and-visualization-tools-on-the-azure-data-science-virtual-machine"></a>In Azure Data Science Virtual Machine verfügbare Tools zum Durchsuchen und Visualisieren von Daten

In der Data Science geht es im Kern darum, die jeweiligen Daten zu verstehen. Hierbei helfen Tools zum Durchsuchen und Visualisieren von Daten. Die folgenden Tools, die auf der Data Science Virtual Machine (DSVM) bereitgestellt werden, vereinfachen diesen wesentlichen Schritt.

## <a name="apache-drill"></a>Apache Drill
|    |           |
| ------------- | ------------- |
| Was ist das?   | Open Source-SQL-Abfrage-Engine für Big Data    |
| Unterstützte DSVM-Versionen      | Windows, Linux  |
| Konfiguration und Installation auf der DSVM      |  Installiert unter `/dsvm/tools/drill*` (nur im eingebetteten Modus)   |
| Typische Verwendung      |  Für direkte Datenauswertung, ohne dass Extrahieren, Transformieren und Laden (ETL) erforderlich ist. Abfragen verschiedener Datenquellen und Formate, einschließlich CSV, JSON, relationale Tabellen und Hadoop.     |
| Verwenden und Ausführen      | Desktopverknüpfung  <br/> [Drill in 10 Minutes](https://drill.apache.org/docs/drill-in-10-minutes/) (Drill in zehn Minuten)  |
| Verwandte Tools auf der DSVM      |   Rattle, Weka, SQL Server Management Studio      |

## <a name="power-bi-desktop"></a>Power BI Desktop 
|    |           |
| ------------- | ------------- |
| Was ist das?   | Interaktives Tool für Datenvisualisierung und BI    |
| Unterstützte DSVM-Versionen      | Windows  |
| Typische Verwendung      |  Datenvisualisierung und Dashboarderstellung   |
| Verwenden und Ausführen      | Desktopverknüpfung (`C:\Program Files\Microsoft Power BI Desktop\bin\PBIDesktop.exe`) oder einfach über das **Startmenü** ausführen.      |
| Verwandte Tools auf der DSVM      |   Visual Studio 2019, Visual Studio Code, Juno      |

> [!NOTE]
> Für den Zugriff auf Power BI benötigen Sie ein Microsoft Office 365-Konto.


## <a name="rattle"></a>Rattle
|    |           |
| ------------- | ------------- |
| Was ist das?   |   Eine grafische Benutzeroberfläche für Data Mining, in der R verwendet wird   |
| Unterstützte DSVM-Editionen     | Windows, Linux     |
| Typische Verwendung      | Allgemeines UI-basiertes Data Mining-Tool für R    |
| Verwenden und Ausführen      | UI-Tool. Öffnen Sie unter Windows eine Eingabeaufforderung, führen Sie R aus, und führen Sie dann `rattle()` in R aus. Stellen Sie unter Linux eine Verbindung über X2Go her, starten Sie ein Terminal, führen Sie R aus, und führen Sie dann `rattle()` in R aus. |
| Links zu Beispielen      | [Rattle](https://togaware.com/onepager/) |
| Verwandte Tools auf der DSVM      |LightGBM, Weka, XGBoost   |


## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| Was ist das?   |  Eine Sammlung von Machine Learning-Algorithmen für Data Mining-Aufgaben. Diese Algorithmen können entweder direkt auf ein Dataset angewendet oder über Ihren eigenen Java-Code aufgerufen werden. Weka enthält Tools für Datenvorverarbeitung, Klassifizierung, Regression, Clustering, Zuordnungsregeln und Visualisierung. |
| Unterstützte DSVM-Editionen     | Windows, Linux     |
| Typische Verwendung      | Allgemeines Machine Learning-Tool     |
| Verwenden und Ausführen      | Unter Windows finden Sie Weka im Startmenü. Unter Linux finden Sie Weka nach der Anmeldung mit X2Go unter „Anwendungen > Entwicklung > Weka“. |
| Links zu Beispielen      | [Beispiele für Weka](https://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| Verwandte Tools auf der DSVM      |LightGBM, Rattle, XGBoost   |





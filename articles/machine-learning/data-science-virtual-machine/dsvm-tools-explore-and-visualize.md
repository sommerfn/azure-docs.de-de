---
title: Tools zum Durchsuchen und Visualisieren von Daten – Azure | Microsoft-Dokumentation
description: Tools zum Durchsuchen und Visualisieren von Daten für die Data Science-VM.
keywords: Data Science-Tools, virtuelle Computer für Data Science, Tools für Data Science, Linux Data Science
services: machine-learning
documentationcenter: ''
author: vijetajo
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: vijetaj
ms.openlocfilehash: d60acdf483d418e458f51ef6cf31b17c43b7379f
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70065904"
---
# <a name="data-exploration-and-visualization-tools-on-the-data-science-virtual-machine"></a>Auf der Data Science-VM verfügbare Tools zum Durchsuchen und Visualisieren von Daten

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

## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| Was ist das?   |  Eine Sammlung von Machine Learning-Algorithmen für Data Mining-Aufgaben. Diese Algorithmen können entweder direkt auf ein Dataset angewendet oder über Ihren eigenen Java-Code aufgerufen werden. Weka enthält Tools für Datenvorverarbeitung, Klassifizierung, Regression, Clustering, Zuordnungsregeln und Visualisierung. |
| Unterstützte DSVM-Editionen     | Windows, Linux     |
| Typische Verwendung      | Allgemeines Machine Learning-Tool     |
| Verwenden und Ausführen      | Unter Windows finden Sie Weka im Startmenü. Unter Linux finden Sie Weka nach der Anmeldung mit X2Go unter „Anwendungen > Entwicklung > Weka“. |
| Links zu Beispielen      | [Beispiele für Weka](https://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| Verwandte Tools auf der DSVM      |LightGBM, Rattle, XGBoost   |

## <a name="rattle"></a>Rattle
|    |           |
| ------------- | ------------- |
| Was ist das?   |   Eine grafische Benutzeroberfläche für Data Mining, in der R verwendet wird   |
| Unterstützte DSVM-Editionen     | Windows, Linux     |
| Typische Verwendung      | Allgemeines UI-basiertes Data Mining-Tool für R    |
| Verwenden und Ausführen      | UI-Tool. Öffnen Sie unter Windows eine Eingabeaufforderung, führen Sie R aus, und führen Sie dann `rattle()` in R aus. Stellen Sie unter Linux eine Verbindung über X2Go her, starten Sie ein Terminal, führen Sie R aus, und führen Sie dann `rattle()` in R aus. |
| Links zu Beispielen      | [Rattle](https://togaware.com/onepager/) |
| Verwandte Tools auf der DSVM      |LightGBM, Weka, XGBoost   |

## <a name="power-bi-desktop"></a>Power BI Desktop 
|    |           |
| ------------- | ------------- |
| Was ist das?   | Interaktives Tool für Datenvisualisierung und BI    |
| Unterstützte DSVM-Versionen      | Windows  |
| Typische Verwendung      |  Datenvisualisierung und Dashboarderstellung   |
| Verwenden und Ausführen      | Desktopverknüpfung (`C:\Program Files\Microsoft Power BI Desktop\bin\PBIDesktop.exe`)      |
| Verwandte Tools auf der DSVM      |   Visual Studio 2019, Visual Studio Code, Juno      |


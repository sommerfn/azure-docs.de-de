---
title: Entwicklungstools
titleSuffix: Azure Data Science Virtual Machine
description: Hier erfahren Sie mehr über die in Data Science Virtual Machine verfügbaren Tools und integrierten Entwicklungsumgebungen.
keywords: Data Science-Tools, virtuelle Computer für Data Science, Tools für Data Science, Linux Data Science
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: conceptual
ms.date: 10/3/2019
ms.openlocfilehash: 76a550e95de24bf65b9b6097dd332e535da5b1c4
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72330713"
---
# <a name="development-tools-on-the-azure-data-science-virtual-machine"></a>Entwicklungstools in Azure Data Science Virtual Machine

Auf der Data Science Virtual Machine (DVSM) sind mehrere beliebte Tools in einer hochgradig produktiven integrierten Entwicklungsumgebung (Integrated Development Environment, IDE) gebündelt. Im Anschluss sind einige der Tools aufgeführt, die auf der DSVM zur Verfügung stehen:

## <a name="visual-studio-community-2017"></a>Visual Studio Community 2017

|    |           |
| ------------- | ------------- |
| Was ist das?   | Universelle IDE      |
| Unterstützte DSVM-Versionen      | Windows      |
| Typische Verwendung      | Softwareentwicklung    |
| Konfiguration und Installation auf der DSVM      | Data Science-Workload (Python- und R-Tools), Azure-Workload (Hadoop, Data Lake), Node.js, SQL Server-Tools, [Azure Machine Learning für Visual Studio Code](https://github.com/Microsoft/vs-tools-for-ai)    |
| Verwenden und Ausführen      | Desktopverknüpfung (`C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\Common7\IDE\devenv.exe`). Öffnen Sie Visual Studio auf der Benutzeroberfläche über das Desktopsymbol oder das **Startmenü**. Suchen Sie nach Programmen (Windows-Logo-Taste+S) und anschließend nach **Visual Studio**. Von dort aus können Sie Projekte in Sprachen wie C#, Python, R und Node.js erstellen.   |
| Verwandte Tools auf der DSVM      |     Visual Studio Code, RStudio, Juno  |

> [!NOTE]
> Es kann eine Meldung angezeigt werden, die besagt, dass der Evaluierungszeitraum abgelaufen ist. Geben Sie die Anmeldeinformationen Ihres Microsoft-Kontos ein. Oder erstellen Sie ein neues kostenloses Konto für den Zugriff auf Visual Studio Community.

## <a name="visual-studio-code"></a>Visual Studio Code 

|    |           |
| ------------- | ------------- |
| Was ist das?   | Universelle IDE      |
| Unterstützte DSVM-Versionen      | Windows, Linux     |
| Typische Verwendung      | Code-Editor und Git-Integration   |
| Verwenden und Ausführen      | Desktopverknüpfung (`C:\Program Files (x86)\Microsoft VS Code\Code.exe`) unter Windows, Desktopverknüpfung oder Terminal (`code`) unter Linux    |
| Verwandte Tools auf der DSVM      |     Visual Studio 2017, RStudio, Juno  |

## <a name="rstudio--desktop"></a>RStudio Desktop 

|    |           |
| ------------- | ------------- |
| Was ist das?   | Client-IDE für R   |
| Unterstützte DSVM-Versionen      | Windows, Linux      |
| Typische Verwendung      |  R-Entwicklung     |
| Verwenden und Ausführen      | Desktopverknüpfung (`C:\Program Files\RStudio\bin\rstudio.exe`) unter Windows, Desktopverknüpfung (`/usr/bin/rstudio`) unter Linux      |
| Verwandte Tools auf der DSVM      |   Visual Studio 2017, Visual Studio Code, Juno      |

## <a name="rstudio--server"></a>RStudio Server 

|    |           |
| ------------- | ------------- |
| Was ist das?   | Client-IDE für R   |
| Was ist das?   | Webbasierte IDE für R    |
| Unterstützte DSVM-Versionen      | Linux      |
| Typische Verwendung      |  R-Entwicklung     |
| Verwenden und Ausführen      | Aktivieren Sie den Dienst mit _systemctl enable rstudio-server_, und starten Sie dann den Dienst mit _systemctl start rstudio-server_. Melden Sie sich dann bei RStudio Server unter „http:\//Ihre-vm-ip:8787“ an.       |
| Verwandte Tools auf der DSVM      |   Visual Studio 2017, Visual Studio Code, RStudio Desktop      |

## <a name="juno"></a>Juno 

|    |           |
| ------------- | ------------- |
| Was ist das?   | Client-IDE für Julia   |
| Unterstützte DSVM-Versionen      | Windows, Linux      |
| Typische Verwendung      |  Julia-Entwicklung     |
| Verwenden und Ausführen      | Desktopverknüpfung (`C:\JuliaPro-0.5.1.1\Juno.bat`) unter Windows, Desktopverknüpfung (`/opt/JuliaPro-VERSION/Juno`) unter Linux      |
| Verwandte Tools auf der DSVM      |   Visual Studio 2017, Visual Studio Code, RStudio      |

## <a name="pycharm"></a>PyCharm

|    |           |
| ------------- | ------------- |
| Was ist das?   | Client-IDE für Python    |
| Unterstützte DSVM-Versionen      | Linux      |
| Typische Verwendung      |  Python-Entwicklung     |
| Verwenden und Ausführen      | Desktopverknüpfung (`/usr/bin/pycharm`) unter Linux      |
| Verwandte Tools auf der DSVM      |   Visual Studio 2017, Visual Studio Code, RStudio      |



## <a name="power-bi-desktop"></a>Power BI Desktop 

|    |           |
| ------------- | ------------- |
| Was ist das?   | Interaktives Tool für Datenvisualisierung und BI    |
| Unterstützte DSVM-Versionen      | Windows  |
| Typische Verwendung      |  Datenvisualisierung und Dashboarderstellung   |
| Verwenden und Ausführen      | Desktopverknüpfung (`C:\Program Files\Microsoft Power BI Desktop\bin\PBIDesktop.exe`)      |
| Verwandte Tools auf der DSVM      |   Visual Studio 2017, Visual Studio Code, Juno      |


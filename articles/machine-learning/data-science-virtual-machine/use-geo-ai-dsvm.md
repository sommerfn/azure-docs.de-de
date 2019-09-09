---
title: Verwenden von Geo-KI
titleSuffix: Azure Data Science Virtual Machine
description: Hier erfahren Sie, wie Sie die Data Science Virtual Machine für geografische KI verwenden, um Daten zu analysieren und geodatenbasierte Modelle zu erstellen.
keywords: deep learning, KI, data science-tools, data science-VM, räumliche Analysen
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: conceptual
ms.date: 03/05/2018
ms.openlocfilehash: 2d7532c51e2963c0dc9f8d02e7a0e32864e80f92
ms.sourcegitcommit: 32242bf7144c98a7d357712e75b1aefcf93a40cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70278414"
---
# <a name="using-the-geo-artificial-intelligence-data-science-virtual-machine"></a>Verwendung der Data Science VM für geografische KI

Verwenden Sie die Data Science-VM für geografische KI, um Daten zur Analyse abzurufen, Data Wrangling durchzuführen und Modelle für KI-Anwendungen zu erstellen, die geografische Informationen nutzen. Nachdem Sie Ihre Data Science-VM mit geografischer KI bereitgestellt und sich bei ArcGIS Pro über Ihr ArcGIS-Konto angemeldet haben, können Sie mit der Interaktion mit ArcGIS Desktop und ArcGIS Online beginnen. Sie können auch über Python-Schnittstellen und einer R-Bridge-Sprache auf ArcGIS zugreifen, die auf der Data Science-VM mit geografischer KI vorkonfiguriert ist. Um umfangreiche KI-Anwendungen zu erstellen, kombinieren Sie die Data Science-VM mit geografischer KI mit Machine Learning- und Deep Learning-Frameworks sowie anderer Data Science-Software, die auf der DSVM verfügbar ist.  


## <a name="configuration-details"></a>Konfigurationsdetails

Die Python-Bibliothek [arcpy](https://pro.arcgis.com/en/pro-app/arcpy/main/arcgis-pro-arcpy-reference.htm), die als Schnittstelle zu ArcGIS dient, wird in der globalen Conda-Stammumgebung der Data Science-VM unter ```c:\anaconda``` installiert.

- Wenn Sie Python an einer Eingabeaufforderung ausführen, führen Sie ```activate``` zur Aktivierung in der Python-basierten Conda-Umgebung (root) aus.
- Wenn Sie eine IDE oder Jupyter Notebook verwenden, können Sie die Umgebung oder den Kernel auswählen, um sicherzustellen, dass Sie sich in der richtigen Conda-Umgebung befinden.

R-Bridge für ArcGIS wird als R-Bibliothek mit dem Namen [arcgisbinding](https://github.com/R-ArcGIS/r-bridge) in der eigenständigen Microsoft Machine Learning Server-Hauptinstanz unter ```C:\Program Files\Microsoft\ML Server\R_SERVER``` installiert. Visual Studio, RStudio und Jupyter sind bereits für die Verwendung dieser R-Umgebung vorkonfiguriert und besitzen Zugriff auf die R-Bibliothek ```arcgisbinding```. 


## <a name="geo-ai-data-science-vm-samples"></a>Data Science-VM mit geografischer KI – Beispiele

Zusätzlich zu den auf dem Machine Learning- und Deep Learning-Framework basierenden Beispielen von der standardmäßigen Data Science VM wird auch eine Reihe von Geobeispielen als Teil der Data Science-VM mit geografischer KI bereitgestellt. Diese Beispiele ermöglichen Ihnen eine schnellen Einstieg in die Entwicklung von KI-Anwendungen mithilfe von Geodaten und der ArcGIS-Software:


1. [Erste Schritte mit Geoanalysen mit Python](https://github.com/Azure/DataScienceVM/blob/master/Notebooks/ArcGIS/Python%20walkthrough%20ArcGIS%20Data%20analysis%20and%20ML.ipynb): Ein einführendes Beispiel zeigt, wie Sie mithilfe der von der [arcpy](https://pro.arcgis.com/en/pro-app/arcpy/main/arcgis-pro-arcpy-reference.htm)-Bibliothek bereitgestellten Python-Benutzeroberfläche für ArcGIS mit Geodaten arbeiten. Zudem zeigt es, wie Sie konventionelles Machine Learning mit Geodaten kombinieren und das Ergebnis auf einer Karte in ArcGIS visualisieren können.

2. [Erste Schritte mit Geoanalysen mit R](https://github.com/Azure/DataScienceVM/blob/master/Notebooks/ArcGIS/R%20walkthrough%20ArcGIS%20Data%20analysis%20and%20ML.ipynb): Ein einführendes Beispiel zeigt, wie Sie mithilfe der von der [arcgisbinding](https://github.com/R-ArcGIS/r-bridge)-Bibliothek bereitgestellten R-Benutzeroberfläche für ArcGIS mit Geodaten arbeiten. 

3. [Klassifizierung der Landnutzung auf Pixelebene:](https://github.com/Azure/pixel_level_land_classification) Ein Tutorial, in dem das Erstellen eines neuronalen Deep Learning-Netzmodell gezeigt wird, das ein Luftbild als Eingabe akzeptiert und eine Beschriftung für einen Landstrich zurückgibt. Zu den Bezeichnungen für Geländeformen zählen beispielsweise *forested* (Waldgebiet) und *water* (Wasser). Das Modell gibt eine Beschriftung für jedes Pixel im Bild zurück. 


## <a name="next-steps"></a>Nächste Schritte

Zusätzliche Beispiele, in denen die Data Science-VM verwendet wird, finden Sie hier:

* [Beispiele](dsvm-samples-and-walkthroughs.md)
---
title: Verwendung der Data Science VM für geografische KI – Azure | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die Data Science Virtual Machine für geografische KI verwenden, um Daten zu analysieren und geodatenbasierte Modelle zu erstellen.
keywords: Deep Learning, KI, Data Science-Tools, virtueller Data Science-Computer, räumliche Analysen
services: machine-learning
documentationcenter: ''
author: vijetajo
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/05/2018
ms.author: vijetaj
ms.openlocfilehash: 9bca7089e6137b3780e3d22f50887e880be29d8e
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68565081"
---
# <a name="using-the-geo-artificial-intelligence-data-science-virtual-machine"></a>Verwendung der Data Science VM für geografische KI

Verwenden Sie die Data Science-VM für geografische KI, um Daten zur Analyse abzurufen, Data Wrangling durchzuführen und Modelle für KI-Anwendungen zu erstellen, die geografische Informationen nutzen. Sobald Sie Ihre Data Science-VM mit geografischer KI bereitgestellt und sich bei ArcGIS Pro mit Ihrem ArcGIS-Konto angemeldet haben, können Sie mit der Interaktion mit ArcGIS Desktop und ArcGIS Online beginnen. Sie können auch über Python-Schnittstellen und einer R-Bridge-Sprache auf ArcGIS zugreifen, die auf der Data Science-VM mit geografischer KI vorkonfiguriert ist. Um umfangreiche KI-Anwendungen zu erstellen, kombinieren Sie sie mit Machine Learning- und Deep Learning-Frameworks sowie anderer Data Science-Software, die auf der Data Science-VM verfügbar ist.  


## <a name="configuration-details"></a>Konfigurationsdetails

Die Python-Bibliothek [arcpy](https://pro.arcgis.com/en/pro-app/arcpy/main/arcgis-pro-arcpy-reference.htm), die als Schnittstelle zu ArcGIS dient, wird in der globalen Conda-Stammumgebung der Data Science-VM unter ```c:\anaconda``` installiert. 

- Wenn Sie Python an einer Eingabeaufforderung ausführen, führen Sie ```activate``` zur Aktivierung in der Python-basierten Conda-Stammumgebung aus. 
- Wenn Sie eine IDE oder Jupyter Notebook verwenden, können Sie die Umgebung oder den Kernel auswählen, um sicherzustellen, dass Sie sich in der richtigen Conda-Umgebung befinden. 

R-Bridge für ArcGIS wird als R-Bibliothek mit dem Namen [arcgisbinding](https://github.com/R-ArcGIS/r-bridge) auf der eigenständigen Microsoft Machine Learning Server-Hauptinstanz unter ```C:\Program Files\Microsoft\ML Server\R_SERVER``` installiert. Visual Studio, RStudio und Jupyter sind bereits für die Verwendung dieser R-Umgebung vorkonfiguriert und haben Zugriff auf die R-Bibliothek ```arcgisbinding```. 


## <a name="geo-ai-data-science-vm-samples"></a>Data Science-VM mit geografischer KI – Beispiele

Zusätzlich zu den auf dem Machine Learning- und Deep Learning-Framework basierenden Beispielen von der standardmäßigen Data Science VM wird auch eine Reihe von geografischen Beispielen als Teil der Data Science-VM mit geografischer KI bereitgestellt. Diese Beispiele ermöglichen Ihnen eine schnellen Einstieg in die Entwicklung von KI-Anwendungen mithilfe von geografischen Daten und der ArcGIS-Software. 


1. [Erste Schritte mit geografischen Analysen mit Python:](https://github.com/Azure/DataScienceVM/blob/master/Notebooks/ArcGIS/Python%20walkthrough%20ArcGIS%20Data%20analysis%20and%20ML.ipynb) Ein einführendes Beispiel zeigt, wie Sie mithilfe der von der [arcpy](https://pro.arcgis.com/en/pro-app/arcpy/main/arcgis-pro-arcpy-reference.htm)-Bibliothek bereitgestellten Python-Benutzeroberfläche für ArcGIS mit geografischen Daten arbeiten. Zudem zeigt es, wie Sie konventionelles Machine Learning mit geografischen Daten kombinieren und das Ergebnis auf einer Karte in ArcGIS visualisieren. 

2. [Erste Schritte mit geografischen Analysen mit R:](https://github.com/Azure/DataScienceVM/blob/master/Notebooks/ArcGIS/R%20walkthrough%20ArcGIS%20Data%20analysis%20and%20ML.ipynb) Ein einführendes Beispiel zeigt, wie Sie mithilfe der von der [arcgisbinding](https://github.com/R-ArcGIS/r-bridge)-Bibliothek bereitgestellten R-Benutzeroberfläche für ArcGIS mit geografischen Daten arbeiten. 

3. [Klassifizierung der Landnutzung auf Pixelebene:](https://github.com/Azure/pixel_level_land_classification) Ein Tutorial, in dem das Erstellen eines neuronalen Deep Learning-Netzmodell gezeigt wird, das ein Luftbild als Eingabe akzeptiert und eine Beschriftung für einen Landstrich zurückgibt. Zu Beschriftungen für Landstriche zählen z.B. „Bewaldet“ oder „Wasser“. Das Modell gibt eine Beschriftung für jedes Pixel im Bild zurück. Das Modell wird mit dem Open Source-basierten Deep Learning-Framework [Cognitive Toolkit (CNTK)](https://www.microsoft.com/en-us/cognitive-toolkit/) von Microsoft erstellt. 


## <a name="next-steps"></a>Nächste Schritte

Zusätzliche Beispiele, in denen die Data Science-VM verwendet wird, finden Sie hier:

* [Beispiele](dsvm-samples-and-walkthroughs.md)


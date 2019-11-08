---
title: 'Export Data (Daten exportieren): Modulreferenz'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie das Modul „Export Data“ in Azure Machine Learning verwenden, um Ergebnisse, Zwischendaten und Arbeitsdaten aus Ihren Pipelines in Cloudspeicherzielen außerhalb von Azure Machine Learning zu speichern.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 10/22/2019
ms.openlocfilehash: 5badcbe2a76c41bdc1431abae617e35b7555eb88
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73497893"
---
# <a name="export-data-module"></a>Modul „Export Data“ (Daten exportieren)

In diesem Artikel wird ein Modul in Azure Machine Learning-Designer (Vorschauversion) beschrieben.

Verwenden Sie dieses Modul, um Ergebnisse, Zwischendaten und Arbeitsdaten aus Ihren Pipelines in Cloudspeicherzielen außerhalb von Azure Machine Learning zu speichern. 

Dieses Modul unterstützt das Exportieren von Daten in folgende Clouddatendiensten:

- Azure-Blobcontainer
- Azure-Dateifreigabe
- Azure Data Lake
- Azure Data Lake Gen2

Bevor Sie Ihre Daten exportieren können, müssen Sie zuerst einen Datenspeicher in Ihrem Azure Machine Learning-Arbeitsbereich registrieren. Weitere Informationen hierzu finden Sie unter [Zugreifen auf Daten](../service/how-to-access-data.md).

## <a name="how-to-configure-export-data"></a>Gewusst wie: Konfigurieren von „Export Data“

1. Fügen Sie das Modul **Export Data** zu Ihrer Pipeline im Designer hinzu. Sie finden dieses Modul in der Kategorie **Input and Output** (Eingabe und Ausgabe).

1. Verbinden Sie **Export Data** mit dem Modul, das die zu exportierenden Daten enthält.

1. Wählen Sie **Export Data** aus, um den Bereich **Properties** (Eigenschaften) zu öffnen.

1. Wählen Sie für **Datastore** (Datenspeicher) in der Dropdownliste einen vorhandenen Datenspeicher aus. Sie können auch einen neuen Datenspeicher erstellen. Informationen, wie Sie dazu vorgehen, finden Sie unter [Zugreifen auf Daten in Azure Storage-Diensten](../service/how-to-access-data.md).

1. Geben Sie den Pfad im Datenspeicher an, in den die Daten geschrieben werden sollen. 


1. Wählen Sie für **File format** (Dateiformat) das Format aus, in dem die Daten gespeichert werden sollen.
 
1. Ausführen der Pipeline.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Gruppe der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 
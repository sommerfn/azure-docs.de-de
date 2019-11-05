---
title: Beispiele für Jupyter Notebooks mit einem öffentlichen NOAA-Dataset
titleSuffix: Azure Open Datasets
description: Verwenden Sie Jupyter-Beispielnotebooks für öffentliche Azure-Datasets, um zu erfahren, wie Sie öffentliche Datasets laden und zum Anreichern von Demodaten verwenden. Zu den Techniken gehören die Verwendung von Spark und Pandas für die Datenverarbeitung.
ms.service: open-datasets
ms.topic: sample
author: cjgronlund
ms.author: cgronlun
ms.date: 11/04/2019
ms.openlocfilehash: 2ccdb7adcae40d22a36fbfb47fba6e4c87b3349a
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73496579"
---
# <a name="example-jupyter-notebooks-show-how-to-enrich-data-with-open-datasets"></a>Jupyter-Beispielnotebooks zeigen das Anreichern von Daten mit öffentlichen Datasets. 
Die Jupyter-Beispielnotebooks für öffentliche Azure-Datasets zeigen Ihnen, wie Sie öffentliche Datasets laden und zum Anreichern von Demodaten verwenden. Zu den Techniken gehören die Verwendung von Apache Spark und Pandas zur Verarbeitung von Daten.

>[!IMPORTANT]
>Bei der Arbeit in einer Spark-fremden Umgebung ermöglichen öffentliche Datasets nur jeweils das Herunterladen der Daten eines Monats mit bestimmten Klassen, um Speicherfehler bei großen Datasets zu vermeiden.

## <a name="load-noaa-integrated-surface-database-isd-data"></a>Laden von NOAA-ISD-Daten (Integrated Surface Database) 
|Notizbuch        | BESCHREIBUNG                                    |
|----------------|------------------------------------------------|
|[Laden der Wetterdaten eines aktuellen Monats in einen Pandas-Datenrahmen](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-access/02-weather-to-pandas-dataframe.ipynb) | Erfahren Sie, wie Sie historische Wetterdaten in Ihren bevorzugten Pandas-Datenrahmen laden. |
|[Laden der Wetterdaten eines aktuellen Monats in einen Spark-Datenrahmen](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-access/01-weather-to-spark-dataframe.ipynb) | Erfahren Sie, wie Sie historische Wetterdaten in Ihren bevorzugten Spark-Datenrahmen laden.  |

## <a name="join-demo-data-with-noaa-isd-data"></a>Verknüpfen von Demodaten mit NOAA-ISD-Daten 
|Notizbuch        | BESCHREIBUNG                                    |
|----------------|------------------------------------------------|
|[Verknüpfen von Demodaten mit Wetterdaten: Pandas](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/02-weather-join-in-pandas.ipynb) | Verknüpfen Sie ein Demodataset für einen Monat von Sensorstandorten mit Wettermesswerten in einem Pandas-Datenrahmen.  |
|[Verknüpfen von Demodaten mit Wetterdaten – Spark](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/01-weather-join-in-spark.ipynb) | Verknüpfen Sie ein Demodataset von Sensorstandorten mit Wettermesswerten in einem Spark-Datenrahmen. |

## <a name="join-nyc-taxi-data-with-noaa-isd-data"></a>Verknüpfen von Taxidaten aus New York mit NOAA-ISD-Daten 
|Notizbuch        | BESCHREIBUNG                                    |
|----------------|------------------------------------------------|
|[Taxifahrtendaten mit Wetterdaten angereichert – Pandas](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/04-nyc-taxi-join-weather-in-pandas.ipynb) | Laden Sie Daten der grünen Taxis in New York (über einen Monat), und reichern Sie sie mit Wetterdaten in einem Pandas-Datenrahmen an. Dieses Beispiel überschreibt die `get_pandas_limit`-Methode und gleicht die Leistung beim Laden der Daten mit der Datenmenge ab.|
|[Taxifahrtendaten mit Wetterdaten angereichert – Spark](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/03-nyc-taxi-join-weather-in-spark.ipynb) | Laden Sie Daten der grünen Taxis in New York, und reichern Sie sie mit Wetterdaten in einem Spark-Datenrahmen an.  |

## <a name="next-steps"></a>Nächste Schritte

* [Tutorial: Regressionsmodellierung mit automatisiertem maschinellem Lernen und einem öffentlichen Dataset](tutorial-opendatasets-automl.md)
* [Python-SDK für öffentliche Datasets](/python/api/azureml-opendatasets/azureml.opendatasets?view=azure-ml-py)
* [Katalog öffentlicher Azure-Datasets](https://azure.microsoft.com/services/open-datasets/catalog/)

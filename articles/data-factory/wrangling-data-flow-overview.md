---
title: Wranglingdatenflüsse in Azure Data Factory
description: Ein Übersicht über Wranglingdatenflüsse in Azure Data Factory
author: djpmsft
ms.author: daperlov
ms.reviewer: gamal
ms.service: data-factory
ms.topic: conceptual
ms.date: 11/01/2019
ms.openlocfilehash: 7b46b1108246f0b83fcfce69844d19d01b1994c4
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73665646"
---
# <a name="what-are-wrangling-data-flows"></a>Was sind Wranglingdatenflüsse?

Organisationen müssen Datenaufbereitung und Data Wrangling durchführen, um komplexe Daten, deren Menge täglich wächst, exakt analysieren zu können. Die Datenaufbereitung ist erforderlich, damit Organisationen die Daten in verschiedenen Geschäftsprozessen verwenden und die Amortisationszeit verkürzen können.

Wranglingdatenflüsse in Azure Data Factory ermöglichen Ihnen das iterative Vorbereiten von Daten ohne Code für die Cloud. Wranglingdatenflüsse können in [Power Query Online](https://docs.microsoft.com/power-query/) integriert werden und stellen Power Query M-Funktionen für Data Factory-Benutzer bereit.

Wranglingdatenflüsse übersetzen M-Code, der vom Power Query Online Mashup Editor generiert wird, in Spark-Code für die Cloudskalierungsausführung.

Wranglingdatenflüsse sind besonders nützlich für Datentechniker oder für Datenintegratoren ohne Programmiererfahrung.

## <a name="use-cases"></a>Anwendungsfälle

### <a name="fast-interactive-data-exploration-and-preparation"></a>Schnelle interaktive Untersuchung und Aufbereitung von Daten

Mehrere Datentechniker und Datenintegratoren ohne Programmiererfahrung können Datasets in der Cloud interaktiv untersuchen und aufbereiten. Mit zunehmender Menge, Vielfalt und Geschwindigkeit der Daten in Data Lakes benötigen Benutzer eine effektive Möglichkeit zum Untersuchen und Aufbereiten von Datasets. Beispielsweise müssen Sie möglicherweise ein Dataset erstellen, das über alle demografischen Informationen zu neuen Kunden seit 2017 verfügt. Sie führen keine Zuordnung zu einem bekannten Ziel durch. Vor dem Veröffentlichen im Lake unterziehen Sie Datasets der Untersuchung, dem Wrangling und der Aufbereitung, damit sie eine Anforderung erfüllen. Wranglingdatenflüsse werden oft für weniger formale Analyseszenarios verwendet. Die aufbereiteten Datasets können für Downstreamtransformationen und -Machine Learning-Vorgänge verwendet werden.

### <a name="code-free-agile-data-preparation"></a>Codefreie agile Datenaufbereitung

Die Datenintegratoren ohne Programmiererfahrung verbringen mehr als 60 % ihrer Zeit mit dem Suchen nach und Aufbereiten von Daten. Sie versuchen, dies ohne Code durchzuführen, um die operative Produktivität zu verbessern. Indem Datenintegratoren ohne Programmiererfahrung die Möglichkeit erhalten, Daten mit bekannten Tools wie Power Query Online skalierbar zu erweitern, zu strukturieren und zu veröffentlichen, wird ihre Produktivität erheblich verbessert. Ein Wranglingdatenfluss aktiviert in Azure Data Factory den vertrauten Power Query Online Mashup Editor, damit Datenintegratoren ohne Programmiererfahrung schnell Fehler beheben, Daten standardisieren und qualitativ hochwertige Daten zur Unterstützung von Geschäftsentscheidungen produzieren können.

### <a name="data-validation"></a>Datenüberprüfung

Überprüfen Sie Ihre Daten visuell ohne Code, um alle Ausreißer sowie Anomalien zu entfernen und sie in eine Form zu bringen, die eine schnelle Analyse ermöglicht.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [einen Wranglingdatenfluss erstellen](wrangling-data-flow-tutorial.md).
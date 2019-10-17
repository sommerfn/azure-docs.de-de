---
title: include file
description: include file
services: data-factory
author: nabhishek
ms.service: data-factory
ms.topic: include
ms.date: 10/09/2019
ms.author: abnarain
ms.openlocfilehash: f634ebb4d8c3f443b5d843b0093c955b384a9f5f
ms.sourcegitcommit: e0a1a9e4a5c92d57deb168580e8aa1306bd94723
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72285591"
---
| Domänennamen                  | Ausgehende Ports | BESCHREIBUNG                              |
| ----------------------------- | -------------- | ---------------------------------------- |
| `*.servicebus.windows.net`    | 443            | Erforderlich für die selbstgehostete Integration Runtime, um Verbindungen mit Datenverschiebungsdiensten in Data Factory herzustellen. |
| `*.frontend.clouddatahub.net` | 443            | Erforderlich für die selbstgehostete Integration Runtime, um Verbindungen mit dem Azure Data Factory-Dienst herzustellen. |
| `download.microsoft.com`    | 443            | Erforderlich für die selbstgehostete Integration Runtime zum Herunterladen der Aktualisierungen. Wenn Sie die automatische Aktualisierung deaktiviert haben, können Sie dies überspringen. |
| `*.core.windows.net`          | 443            | Wird von der selbstgehosteten Integration Runtime verwendet, um Verbindungen mit dem Azure Storage-Konto herzustellen, wenn Sie das Feature [gestaffeltes Kopieren](https://docs.microsoft.com/azure/data-factory/copy-activity-performance#staged-copy) verwenden. |
| `*.database.windows.net`      | 1433           | (Optional:) Erforderlich, wenn Sie von bzw. nach Azure SQL-Datenbank oder Azure SQL Data Warehouse kopieren. Verwenden Sie das Feature für gestaffeltes Kopieren, um Daten nach Azure SQL-Datenbank oder Azure SQL Data Warehouse zu kopieren, ohne den Port 1433 zu öffnen. |
| `*.azuredatalakestore.net`<br>`login.microsoftonline.com/<tenant>/oauth2/token`    | 443            | (Optional:) Erforderlich, wenn Sie von bzw. nach Azure Data Lake Store kopieren. |

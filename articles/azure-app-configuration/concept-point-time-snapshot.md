---
title: 'Azure App Configuration: Point-in-Time-Momentaufnahme | Microsoft-Dokumentation'
description: Ein Überblick über die Funktionsweise von Point-in-Time-Momentaufnahmen in Azure App Configuration
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.service: azure-app-configuration
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: 83770e8c5f415670855b5cf2502d02c4d6919440
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "59998072"
---
# <a name="point-in-time-snapshot"></a>Point-in-Time-Momentaufnahme

Azure App Configuration dokumentiert die genauen Zeiten, zu denen ein neues Schlüssel-Wert-Paar erstellt und dann geändert wird. So entsteht eine lückenlose Chronik aller Schlüssel-Wert-Änderungen. Ein App-Konfigurationsspeicher kann den Verlauf jedes beliebigen Schlüssel-Wert-Paars rekonstruieren und dessen Wert zu einem beliebigen Zeitpunkt (bis hin zum aktuellen Zeitpunkt) wiedergeben. Mithilfe dieses Features können Sie ein altes Schlüssel-Wert-Paar aus der „Vergangenheit“ abrufen. Beispielsweise können Sie die gestrigen (unmittelbar von vor der neuesten Bereitstellung stammenden) Konfigurationseinstellungen abrufen, um eine vorherige Konfiguration wiederherzustellen und ein Rollback der Anwendung durchzuführen.

## <a name="key-value-retrieval"></a>Abrufen von Schlüssel-Wert-Paaren

Wenn Sie Schlüssel-Wert-Paare aus der Vergangenheit abrufen möchten, geben Sie im HTTP-Header eines REST-API-Aufrufs einen Zeitpunkt für die Erstellung einer Momentaufnahme der Schlüssel-Wert-Paare an. Beispiel: 

        GET /kv HTTP/1.1
        Accept-Datetime: Sat, 1 Jan 2019 02:10:00 GMT

Der von App Configuration gespeicherte Änderungsverlauf umfasst derzeit sieben Tage.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer ASP.NET Core-Web-App](./quickstart-aspnet-core-app.md)  

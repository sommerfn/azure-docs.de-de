---
title: Erstellen einer Funktionen-App im Azure-Portal | Microsoft Docs
description: Erstellen Sie eine neue Funktionen-App in Azure App Service über das Portal.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: quickstart
ms.date: 04/11/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: ad9c50953447c1effee48eec5b0cb9f64386e6cc
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2019
ms.locfileid: "67155576"
---
# <a name="create-a-function-app-from-the-azure-portal"></a>Erstellen einer Funktionen-App im Azure-Portal

Für Azure-Funktionen-Apps wird die Infrastruktur von Azure App Service verwendet. In diesem Thema wird erläutert, wie Sie eine Funktionen-App im Azure-Portal erstellen. Bei einer Funktionen-App handelt es sich um den Container, der die Ausführung einzelner Funktionen hostet. Wenn Sie eine Funktionen-App im App Service-Hostingplan erstellen, kann die Funktionen-App sämtliche Funktionen von App Service verwenden.

## <a name="create-a-function-app"></a>Erstellen einer Funktionen-App

[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal.md)]

Geben Sie beim Erstellen einer Funktionen-App einen gültigen **App-Namen** an. Dieser darf nur Buchstaben, Zahlen und Bindestriche enthalten. Unterstriche ( **_** ) sind nicht zulässig.

Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten. Der Name Ihres Speicherkontos muss innerhalb von Azure eindeutig sein. 

Nach der Erstellung der Funktionen-App können Sie einzelne Funktionen in einer oder mehreren Sprachen erstellen. Die Funktionen können Sie [über das Portal](functions-create-first-azure-function.md#create-function), über [Continuous Deployment](functions-continuous-deployment.md) oder durch [Hochladen mit FTP](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp) erstellen.

## <a name="service-plans"></a>Servicepläne

Azure Functions bietet zwei verschiedene Servicepläne: Verbrauchsplan und App Service-Plan. Der Verbrauchsplan weist automatisch Computeleistung zu, wenn Ihr Code ausgeführt wird, und skaliert diese bei Bedarf horizontal hoch, um die Last zu verarbeiten. Wenn der Code nicht mehr ausgeführt wird, wird die Leistung wieder horizontal herunterskaliert. Mit dem App Service-Plan hat die Funktionen-App Zugriff auf alle Funktionen von App Service. Sie müssen Ihren Serviceplan bei der Erstellung der Funktionen-App auswählen. Derzeit kann er dann nicht mehr geändert werden. Weitere Informationen finden Sie unter [Auswählen eines Azure Functions-Hostingplans](functions-scale.md).

Wenn Sie beabsichtigen, JavaScript-Funktionen für einen App Service-Plan zu verwenden, sollten Sie einen Plan mit einer kleineren Anzahl von Kernen auswählen. Weitere Informationen finden Sie in der [JavaScript-Referenz für Funktionen](functions-reference-node.md#choose-single-vcpu-app-service-plans).

<a name="storage-account-requirements"></a>

## <a name="storage-account-requirements"></a>Anforderungen an das Speicherkonto

Beim Erstellen einer Funktionen-App in App Service müssen Sie ein allgemeines Azure Storage-Konto erstellen oder verknüpfen, das Blob Storage, Queue Storage und Table Storage unterstützt. Intern wird Storage in Functions für Vorgänge wie das Verwalten von Triggern und Ausführungen von Protokollierfunktionen verwendet. Manche Speicherkonten unterstützen keine Warteschlangen und Tabellen, wie z.B. reine Blobspeicherkonten, Azure Storage Premium und allgemeine Speicherkonten mit Replikation von ZRS. Diese Konten werden aus dem Blatt „Speicherkonto“ herausgefiltert, wenn eine Funktionen-App erstellt wird.

>[!NOTE]
>Wenn der verbrauchsbasierte Hostingplan verwendet wird, werden die Funktionscode- und Bindungskonfigurationsdateien in Azure File Storage im Hauptspeicherkonto gespeichert. Wenn Sie das Hauptspeicherkonto löschen, wird dieser Inhalt ebenfalls gelöscht und kann nicht wiederhergestellt werden.

Weitere Informationen zu Speicherkontotypen finden Sie unter [Einführung in die Azure Storage-Dienste](../storage/common/storage-introduction.md#azure-storage-services). 

## <a name="next-steps"></a>Nächste Schritte

Da das Azure-Portal das Erstellen und Ausprobieren von Funktionen erleichtert, empfehlen wir [lokale Entwicklung](functions-develop-local.md). Nach dem Erstellen einer Funktions-App im Portal müssen Sie immer noch eine Funktion hinzufügen. 

> [!div class="nextstepaction"]
> [Hinzufügen einer durch HTTP ausgelösten Funktion](functions-create-first-azure-function.md#create-function)

---
title: Erstellen einer Funktionen-App im Azure-Portal | Microsoft Docs
description: Es wird beschrieben, wie Sie in Azure über das Portal eine neue Funktions-App erstellen.
author: ggailey777
manager: gwallace
ms.service: azure-functions
ms.topic: conceptual
ms.date: 08/29/2019
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 93bce0404c9b3bf630416557726dca0c856528c3
ms.sourcegitcommit: ee61ec9b09c8c87e7dfc72ef47175d934e6019cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2019
ms.locfileid: "70170802"
---
# <a name="create-a-function-app-from-the-azure-portal"></a>Erstellen einer Funktionen-App im Azure-Portal

In diesem Thema wird veranschaulicht, wie Sie Azure Functions zum Erstellen einer Funktions-App im Azure-Portal verwenden. Bei einer Funktionen-App handelt es sich um den Container, der die Ausführung einzelner Funktionen hostet. 

## <a name="create-a-function-app"></a>Erstellen einer Funktionen-App

[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal.md)]

Geben Sie beim Erstellen einer Funktionen-App einen gültigen **App-Namen** an. Dieser darf nur Buchstaben, Zahlen und Bindestriche enthalten. Unterstriche ( **_** ) sind nicht zulässig.

Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten. Der Name Ihres Speicherkontos muss innerhalb von Azure eindeutig sein. 

Nach der Erstellung der Funktionen-App können Sie einzelne Funktionen in einer oder mehreren Sprachen erstellen. Die Funktionen können Sie [über das Portal](functions-create-first-azure-function.md#create-function), über [Continuous Deployment](functions-continuous-deployment.md) oder durch [Hochladen mit FTP](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp) erstellen.

## <a name="service-plans"></a>Diensttarife

Azure Functions verfügt über drei verschiedene Diensttarife: Verbrauchstarif, Premium-Tarif und Dedicated-Tarif (App Service). Sie müssen Ihren Diensttarif bei der Erstellung der Funktions-App auswählen. Anschließend kann er dann nicht mehr geändert werden. Weitere Informationen finden Sie unter [Auswählen eines Azure Functions-Hostingplans](functions-scale.md).

Wenn Sie beabsichtigen, JavaScript-Funktionen für einen Dedicated-Tarif (App Service) zu verwenden, sollten Sie einen Plan mit einer geringeren Zahl von Kernen auswählen. Weitere Informationen finden Sie in der [JavaScript-Referenz für Funktionen](functions-reference-node.md#choose-single-vcpu-app-service-plans).

<a name="storage-account-requirements"></a>

## <a name="storage-account-requirements"></a>Anforderungen an das Speicherkonto

Beim Erstellen einer Funktions-App müssen Sie ein allgemeines Azure Storage-Konto erstellen oder verknüpfen, das Blob-, Warteschlangen- und Tabellenspeicher unterstützt. Intern wird Storage in Functions für Vorgänge wie das Verwalten von Triggern und Ausführungen von Protokollierfunktionen verwendet. Manche Speicherkonten unterstützen keine Warteschlangen und Tabellen, wie z.B. reine Blobspeicherkonten, Azure Storage Premium und allgemeine Speicherkonten mit Replikation von ZRS. Diese Konten werden aus dem Blatt „Speicherkonto“ herausgefiltert, wenn eine Funktionen-App erstellt wird.

>[!NOTE]
>Wenn der verbrauchsbasierte Hostingplan verwendet wird, werden die Funktionscode- und Bindungskonfigurationsdateien in Azure File Storage im Hauptspeicherkonto gespeichert. Wenn Sie das Hauptspeicherkonto löschen, wird dieser Inhalt ebenfalls gelöscht und kann nicht wiederhergestellt werden.

Weitere Informationen zu Speicherkontotypen finden Sie unter [Einführung in die Azure Storage-Dienste](../storage/common/storage-introduction.md#azure-storage-services). 

## <a name="next-steps"></a>Nächste Schritte

Da das Azure-Portal das Erstellen und Ausprobieren von Funktionen erleichtert, empfehlen wir [lokale Entwicklung](functions-develop-local.md). Nach dem Erstellen einer Funktions-App im Portal müssen Sie immer noch eine Funktion hinzufügen. 

> [!div class="nextstepaction"]
> [Hinzufügen einer durch HTTP ausgelösten Funktion](functions-create-first-azure-function.md#create-function)

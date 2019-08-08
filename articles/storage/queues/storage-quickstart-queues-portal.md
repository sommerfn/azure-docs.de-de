---
title: 'Azure-Schnellstart: Erstellen einer Warteschlange in Azure Storage mithilfe des Azure-Portals | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung verwenden Sie das Azure-Portal, um eine Warteschlange zu erstellen. Anschließend verwenden Sie das Azure-Portal, um eine Nachricht hinzuzufügen, die Eigenschaften der Nachricht anzuzeigen und die Nachricht aus der Warteschlange zu entfernen.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 03/06/2019
ms.service: storage
ms.subservice: queues
ms.topic: quickstart
ms.reviewer: cbrooks
ms.openlocfilehash: d2f7b01c03b877a3dede496fc02f0d0cc9525cef
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68721317"
---
# <a name="quickstart-create-a-queue-and-add-a-message-with-the-azure-portal"></a>Schnellstart: Erstellen einer Warteschlange und Hinzufügen einer Nachricht mit dem Azure-Portal

In dieser Schnellstartanleitung erfahren Sie, wie Sie mit dem [Azure-Portal](https://portal.azure.com/) eine Warteschlange in Azure Storage erstellen und ihr Nachrichten hinzufügen bzw. Nachrichten aus der Warteschlange entfernen.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

## <a name="create-a-queue"></a>Erstellen einer Warteschlange

Führen Sie die folgenden Schritte aus, um eine Warteschlange im Azure-Portal zu erstellen:

1. Navigieren Sie im Azure-Portal zu Ihrem neuen Speicherkonto.
2. Scrollen Sie im linken Menü für das Speicherkonto zum Abschnitt **Warteschlangendienst**, und wählen Sie **Warteschlangen** aus.
3. Wählen Sie die Schaltfläche **+ Warteschlange**.
4. Geben Sie einen Namen für die neue Warteschlange ein. Der Warteschlangenname muss klein geschrieben werden, mit einem Buchstaben oder einer Zahl beginnen und darf nur Buchstaben, Zahlen und Bindestriche (-) enthalten.
6. Wählen Sie **OK**, um die Warteschlange zu erstellen.

    ![Screenshot: Erstellen einer Warteschlange über das Azure-Portal](media/storage-quickstart-queues-portal/create-queue.png)

## <a name="add-a-message"></a>Hinzufügen einer Nachricht

Fügen Sie als Nächstes der neuen Warteschlange eine Nachricht hinzu. Eine Nachricht kann bis zu 64 KB groß sein.

1. Wählen Sie die neue Warteschlange aus der Liste der Warteschlangen im Speicherkonto aus.
1. Wählen Sie die Schaltfläche **+ Nachricht hinzufügen**, um der Warteschlange eine Nachricht hinzuzufügen. Geben Sie eine Nachricht ins Feld **Nachrichtentext** ein. 
1. Geben Sie an, wann die Nachricht abläuft. Eine Nachricht kann maximal sieben Tage in der Warteschlange verbleiben.
1. Geben Sie an, ob die Nachricht als Base64 codiert werden soll. Die Codierung von binären Daten wird empfohlen.
1. Wählen Sie die Schaltfläche **OK**, um die Nachricht hinzuzufügen.

    ![Screenshot: Hinzufügen einer Nachricht zu einer Warteschlange](media/storage-quickstart-queues-portal/add-message.png)

## <a name="view-message-properties"></a>Anzeigen von Nachrichteneigenschaften

Nachdem Sie eine Nachricht hinzugefügt haben, wird im Azure-Portal eine Liste aller Nachrichten in der Warteschlange angezeigt. Sie können die Nachrichten-ID, den Inhalt der Nachricht, den Zeitpunkt der Einfügung der Nachricht und die Ablaufzeit der Nachricht anzeigen. Darüber hinaus können Sie anzeigen, wie häufig eine bestimmte Nachricht aus der Warteschlange entfernt wurde.

![Screenshot der Nachrichteneigenschaften](media/storage-quickstart-queues-portal/view-message-properties.png)

## <a name="dequeue-a-message"></a>Entfernen einer Nachricht aus der Warteschlange

Sie können über das Azure-Portal eine Nachricht vom Anfang der Warteschlange entfernen. Wenn Sie eine Nachricht aus der Warteschlange entfernen, wird die Nachricht gelöscht. 

Beim Entfernen aus der Warteschlange wird immer die älteste Nachricht in der Warteschlange gelöscht. 

![Screenshot: Entfernen einer Nachricht aus der Warteschlange über das Portal](media/storage-quickstart-queues-portal/dequeue-message.png)

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie gelernt, wie Sie eine Warteschlange erstellen, eine Nachricht hinzufügen, Nachrichteneigenschaften anzeigen und eine Nachricht über das Azure-Portal aus der Warteschlange entfernen.

> [!div class="nextstepaction"]
> [Einführung in Warteschlangen](storage-queues-introduction.md)

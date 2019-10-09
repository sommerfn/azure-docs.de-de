---
title: Aktivieren von Notebooks im Azure Cosmos DB-Konto
description: Die integrierten Notebooks von Azure Cosmos DB ermöglichen es Ihnen, Ihre Daten innerhalb des Portals zu analysieren und zu visualisieren. In diesem Artikel wird beschrieben, wie Sie dieses Feature für Cosmos-Konten aktivieren.
author: deborahc
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/22/2019
ms.author: dech
ms.openlocfilehash: 22bad3b31b8cbe54900b96bd4b327f18916e81a3
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/30/2019
ms.locfileid: "71677148"
---
# <a name="enable-notebooks-for-azure-cosmos-db-accounts"></a>Aktivieren von Notebooks für Azure Cosmos DB-Konten

> [!IMPORTANT]
> Integrierte Notebooks für Azure Cosmos DB sind zurzeit in den folgenden Azure-Regionen verfügbar: „Australien, Osten“, „USA, Osten“, „USA, Osten 2“, „Europa, Norden“, „USA, Süden-Mitte“, „Asien, Südosten“, „Vereinigtes Königreich, Süden“, „Europa, Westen“ und „USA, Westen 2“. Um Notebooks zu verwenden, [erstellen Sie ein neues Konto mit Notebooks](#enable-notebooks-in-a-new-cosmos-account), oder [aktivieren Sie Notebooks für ein vorhandenes Konto](#enable-notebooks-in-an-existing-cosmos-account) in einer dieser Regionen.

Integrierte Jupyter-Notebooks in Azure Cosmos DB ermöglichen es Ihnen, Ihre Daten über das Azure-Portal zu analysieren und zu visualisieren. In diesem Artikel wird beschrieben, wie Sie dieses Feature für Ihr Azure Cosmos DB-Konto aktivieren.

## <a name="enable-notebooks-in-a-new-cosmos-account"></a>Aktivieren von Notebooks in einem neuen Cosmos-Konto
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
1. Wählen Sie **Ressourcen erstellen** > **Datenbanken** > **Azure Cosmos DB** aus.
1. Wählen Sie auf der Seite **Azure Cosmos DB-Konto erstellen** die Option **Notebooks** aus. 
 
    ![Auswählen der Option „Notebooks“ auf dem Blatt „Azure Cosmos DB-Konto erstellen“](media/enable-notebooks/create-new-account-with-notebooks.png)
1. Klicken Sie auf **Überprüfen + erstellen**. Sie können die Optionen **Netzwerk** und **Tags** überspringen. 
1. Überprüfen Sie die Kontoeinstellungen, und wählen Sie anschließend **Erstellen** aus. Die Erstellung des Kontos dauert einige Minuten. Warten Sie, bis auf der Portalseite **Ihre Bereitstellung wurde abgeschlossen.** angezeigt wird. 

    ![Der Bereich „Benachrichtigungen“ im Azure-Portal](media/enable-notebooks/create-new-account-with-notebooks-complete.png)
1. Wählen Sie **Zu Ressource wechseln** aus, um zur Seite des Azure Cosmos DB-Kontos zu wechseln. 

    ![Seite des Azure Cosmos DB-Kontos](../../includes/media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created-3.png)

1. Navigieren Sie zum Bereich **Daten-Explorer**. Der Notebooks-Arbeitsbereich sollte jetzt angezeigt werden.

    ![Neuer Notebooks-Arbeitsbereich von Azure Cosmos DB](media/enable-notebooks/new-notebooks-workspace.png)

## <a name="enable-notebooks-in-an-existing-cosmos-account"></a>Aktivieren von Notebooks in einem vorhandenen Cosmos-Konto
Sie können auch Notebooks für vorhandene Konten aktivieren. Dieser Schritt muss nur einmal pro Konto durchgeführt werden.

1. Navigieren Sie in Ihrem Cosmos-Konto zum Bereich **Daten-Explorer**.
1. Wählen Sie **Notebooks aktivieren** aus.

    ![Erstellen eines neuen Notebooks-Arbeitsbereichs im Daten-Explorer](media/enable-notebooks/enable-notebooks-workspace.png)
1. Daraufhin werden Sie aufgefordert, einen neuen Notebooks-Arbeitsbereich zu erstellen. Wählen Sie **Setup abschließen** aus.
1. Ihr Konto ist jetzt für die Verwendung von Notebooks aktiviert!

## <a name="create-and-run-your-first-notebook"></a>Erstellen und Ausführen des ersten Notebooks

Um zu überprüfen, ob Sie Notebooks verwenden können, wählen Sie eines der Notebooks unter den Beispielnotebooks aus. Dadurch wird eine Kopie des Notebooks in Ihrem Arbeitsbereich gespeichert und geöffnet.

In diesem Beispiel wird **GettingStarted.ipynb**. verwendet. 

![Anzeigen des Notebooks „GettingStarted.ipynb“](media/enable-notebooks/select-getting-started-notebook.png)

So führen Sie das Notebook aus:
1. Wählen Sie die erste Codezelle mit Python-Code aus. 
1. Klicken Sie auf **Ausführen**, um die Zelle auszuführen. Sie können zum Ausführen der Zelle auch **UMSCHALT+EINGABE** verwenden.
1. Aktualisieren Sie den Ressourcenbereich, um die Datenbank und den Container anzuzeigen, die erstellt wurden.

    ![Ausführen des Notebooks mit ersten Schritten](media/enable-notebooks/run-first-notebook-cell.png)

Sie können auch **Neues Notebook** auswählen, um ein neues Notebook zu erstellen, oder eine vorhandene Notebook-Datei (IPYNB) hochladen, indem Sie im Menü **Meine Notebooks** die Option **Datei hochladen** auswählen. 

![Erstellen oder Hochladen eines neuen Notebooks](media/enable-notebooks/create-or-upload-new-notebook.png)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu den Vorzügen von [Jupyter-Notebooks in Azure Cosmos DB](cosmosdb-jupyter-notebooks.md)

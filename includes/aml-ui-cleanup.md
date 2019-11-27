---
title: include file
description: include file
services: machine-learning
ms.service: machine-learning
ms.custom: include file
ms.topic: include
author: sgilley
ms.author: sgilley
ms.date: 11/06/2019
ms.openlocfilehash: 96ede63b097999247675364217cf458a268e54d9
ms.sourcegitcommit: a10074461cf112a00fec7e14ba700435173cd3ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2019
ms.locfileid: "73929679"
---
>[!IMPORTANT]
>Sie können die von Ihnen bei der Vorbereitung erstellten Ressourcen auch in anderen Tutorials und Anleitungen für Azure Machine Learning verwenden.

### <a name="delete-everything"></a>Alles löschen

Wenn Sie die erstellten Ressourcen nicht mehr benötigen, löschen Sie die gesamte Ressourcengruppe, damit Ihnen keine Kosten entstehen.

1. Wählen Sie im Azure-Portal links im Fenster **Ressourcengruppen** aus.
 
   ![Löschen der Ressourcengruppe im Azure-Portal](./media/aml-ui-cleanup/delete-resources.png)

1. Wählen Sie in der Liste die Ressourcengruppe aus, die Sie erstellt haben.

1. Wählen Sie die Option **Ressourcengruppe löschen**.

Durch das Löschen einer Ressourcengruppe werden auch alle im Designer erstellten Ressourcen gelöscht. 

### <a name="delete-individual-assets"></a>Löschen einzelner Objekte

In dem Designer, in dem Sie Ihr Experiment erstellt haben, können Sie einzelne Ressourcen löschen, indem Sie erst die gewünschten Ressourcen und dann die Schaltfläche **Löschen** auswählen.

Das hier erstellte Computeziel wird *automatisch auf null Knoten skaliert*, wenn es nicht verwendet wird. Diese Aktion wird durchgeführt, um Gebühren zu minimieren. Wenn Sie das Computeziel löschen möchten, führen Sie die folgenden Schritte aus:

![Löschen von Assets](./media/aml-ui-cleanup/delete-asset.png)

Die Registrierung von Datasets im Arbeitsbereich kann aufgehoben werden, indem Sie die einzelnen Datasets und anschließend **Registrierung aufheben** auswählen.

![Aufheben der Registrierung eines Datasets](./media/aml-ui-cleanup/unregister-dataset.png)

Zum Löschen eines Datasets wechseln Sie im Azure-Portal oder Azure Storage-Explorer zum Speicherkonto, und löschen Sie diese Ressourcen manuell.



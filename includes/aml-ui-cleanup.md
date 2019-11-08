---
title: include file
description: include file
services: machine-learning
ms.service: machine-learning
ms.custom: include file
ms.topic: include
author: sgilley
ms.author: sgilley
ms.date: 10/22/2019
ms.openlocfilehash: 5a66212122745d0f4426e48e9487e9d674cec53f
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73489927"
---
>[!IMPORTANT]
>Sie können die von Ihnen bei der Vorbereitung erstellten Ressourcen auch in anderen Tutorials und Anleitungen für Azure Machine Learning verwenden.

### <a name="delete-everything"></a>Alles löschen

Wenn Sie die erstellten Ressourcen nicht mehr benötigen, löschen Sie die gesamte Ressourcengruppe, damit Ihnen keine Kosten entstehen.

1. Wählen Sie im Azure-Portal links im Fenster **Ressourcengruppen** aus.
 
   ![Löschen der Ressourcengruppe im Azure-Portal](./media/aml-ui-cleanup/delete-resources.png)

1. Wählen Sie in der Liste die Ressourcengruppe aus, die Sie erstellt haben.

1. Wählen Sie auf der rechten Seite des Fensters die Schaltfläche mit den Auslassungspunkten ( **...** ) aus.

1. Wählen Sie die Option **Ressourcengruppe löschen**.

Durch das Löschen einer Ressourcengruppe werden auch alle im Designer erstellten Ressourcen gelöscht.  

### <a name="delete-only-the-compute-target"></a>Ausschließliches Löschen des Computeziels

Das hier erstellte Computeziel wird *automatisch auf null Knoten skaliert*, wenn es nicht verwendet wird. Dadurch werden Kosten minimiert. Wenn Sie das Computeziel löschen möchten, führen Sie die folgenden Schritte aus:

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) Ihren Arbeitsbereich.

    ![Löschen des Computeziels](./media/aml-ui-cleanup/delete-compute-target.png)

1. Wählen Sie im Abschnitt **Compute** Ihres Arbeitsbereichs die Ressource aus.

1. Klicken Sie auf **Löschen**.

### <a name="delete-individual-assets"></a>Löschen einzelner Objekte

In dem Designer, in dem Sie Ihr Experiment erstellt haben, können Sie einzelne Ressourcen löschen, indem Sie erst die gewünschten Ressourcen und dann die Schaltfläche **Löschen** auswählen.

![Löschen von Assets](./media/aml-ui-cleanup/delete-asset.png)

Die Registrierung von Datasets im Arbeitsbereich kann aufgehoben werden, indem Sie die einzelnen Datasets und anschließend **Registrierung aufheben** auswählen.

![Aufheben der Registrierung eines Datasets](./media/aml-ui-cleanup/unregister-dataset.png)



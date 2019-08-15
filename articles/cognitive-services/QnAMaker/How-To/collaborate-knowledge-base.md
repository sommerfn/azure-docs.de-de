---
title: 'Zusammenarbeit an der Wissensdatenbank: QnA Maker'
titleSuffix: Azure Cognitive Services
description: QnA Maker ermöglicht mehreren Personen, an einer Wissensdatenbank zusammenzuarbeiten. Dieses Feature wird im Rahmen der rollenbasierten Zugriffssteuerung in Azure bereitgestellt.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 01/14/2019
ms.author: diberry
ms.openlocfilehash: 9c5398ff7cb31698db3d4a798b6a082f9e74b99b
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/12/2019
ms.locfileid: "68955135"
---
# <a name="collaborate-on-your-knowledge-base"></a>Zusammenarbeiten an einer Wissensdatenbank

QnA Maker ermöglicht mehreren Personen, an einer Wissensdatenbank zusammenzuarbeiten. Diese Funktion wird im Rahmen der [rollenbasierten Zugriffssteuerung](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) in Azure bereitgestellt. 

Führen Sie die folgenden Schritte aus, um Ihren QnA Maker-Dienst für andere freizugeben:

1. Melden Sie sich am Azure-Portal an, und navigieren Sie zu Ihrer QnA Maker-Ressource.

    ![QnA Maker-Ressourcenliste](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-resource-list.PNG)

2. Öffnen Sie die Registerkarte **Zugriffssteuerung (IAM)** .

    ![IAM in QnA Maker](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam.PNG)

3. Wählen Sie **Hinzufügen**.

    ![Hinzufügen von IAM in QnA Maker](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add.PNG)

4. Wählen Sie die Rolle **Besitzer** oder **Mitwirkender** aus. Durch die rollenbasierte Zugriffssteuerung können Sie keinen schreibgeschützten Zugriff gewähren. Die Rollen „Besitzer“ und „Mitwirkender“ haben Lese-/Schreibzugriffsrechte auf den QnA Maker-Dienst.

    ![Hinzufügen einer IAM-Rolle in QnA Maker](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-role.PNG)

5. Geben Sie die E-Mail-Adresse der Person ein, für die Sie den Dienst freigeben möchten, und klicken Sie auf „Speichern“.

    ![Hinzufügen einer E-Mail-Adresse für IAM in QnA Maker](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-email.PNG)

Wenn sich eine Person, für die Sie Ihren QnA Maker-Dienst freigegeben haben, im [QnA Maker-Portal](https://qnamaker.ai) anmeldet, sieht sie nun alle Wissensdatenbanken in diesem Dienst.

Es ist nicht möglich, eine bestimmte Wissensdatenbank in einem QnA Maker-Dienst freizugeben. Wenn Sie eine differenzierte Zugriffssteuerung wünschen, verteilen Sie Ihre Wissensdatenbanken auf verschiedene QnA Maker-Dienste.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Testen einer Wissensdatenbank](./test-knowledge-base.md)

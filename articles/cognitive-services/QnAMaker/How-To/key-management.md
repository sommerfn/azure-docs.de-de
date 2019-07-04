---
title: 'Ressourcen- und Schlüsselverwaltung: QnA Maker'
titleSuffix: Azure Cognitive Services
description: 'Ihr QnA Maker-Dienst verwendet zwei Arten von Schlüsseln: Abonnementschlüssel und Endpunktschlüssel.'
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 03/04/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: b9be1db9be1d4dd57994e101c07ed430425a5912
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67447438"
---
# <a name="how-to-manage-keys-in-qna-maker"></a>Verwalten von Schlüsseln in QnA Maker

Ihr QnA Maker-Dienst verwendet zwei Arten von Schlüsseln, **Abonnementschlüssel** und **Endpunktschlüssel**.

![Schlüsselverwaltung](../media/qnamaker-how-to-key-management/key-management.png)

1. **Abonnementschlüssel:** Diese Schlüssel werden verwendet, um auf die [QnA Maker-Verwaltungsdienst-APIs](https://go.microsoft.com/fwlink/?linkid=2092179) zuzugreifen. Mit diesen APIs können Sie Ihre Wissensdatenbank bearbeiten.  

2. **Endpunktschlüssel:** Diese Schlüssel werden für den Zugriff auf den Endpunkt der Wissensdatenbank verwendet, um eine Antwort für eine Benutzerfrage abzurufen. In der Regel verwenden Sie diesen Endpunkt in Ihrem Chatbot oder Clientanwendungscode, der den QnA Maker-Dienst nutzt.
 
## <a name="subscription-keys"></a>Abonnementschlüssel
Sie können Ihre Abonnementschlüssel im Azure-Portal anzeigen und zurücksetzen, in dem Sie die QnA Maker-Ressource erstellt haben. 
1. Wechseln Sie im Azure-Portal zur QnA Maker-Ressource.

    ![QnA Maker-Ressourcenliste](../media/qnamaker-how-to-key-management/qnamaker-resource-list.png)

2. Wechseln Sie zu **Schlüssel**.

    ![Abonnementschlüssel](../media/qnamaker-how-to-key-management/subscription-key.PNG)

## <a name="endpoint-keys"></a>Endpunktschlüssel

Endpunktschlüssel können im [QnA Maker-Portal](https://qnamaker.ai) verwaltet werden.

1. Melden Sie sich beim [QnA Maker-Portal](https://qnamaker.ai) an, rufen Sie Ihr Profil auf, und klicken Sie dann auf **Diensteinstellung**.

    ![Endpunktschlüssel](../media/qnamaker-how-to-key-management/Endpoint-keys.png)

2. Zeigen Sie Ihre Schlüssel an, oder setzen Sie die Schlüssel zurück.

    ![Endpunktschlüssel-Manager](../media/qnamaker-how-to-key-management/Endpoint-keys1.png)

    >[!NOTE]
    >Aktualisieren Sie Ihre Schlüssel, wenn Sie das Gefühl haben, dass sie gefährdet sind. Dazu müssen möglicherweise entsprechende Änderungen an Ihrem Clientanwendungs- oder Botcode vorgenommen werden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Wissensdatenbank in einer anderen Sprache](./language-knowledge-base.md)

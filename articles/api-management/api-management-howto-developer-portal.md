---
title: Zugriff auf und Anpassen des neuen Entwicklerportals – Azure API Management | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie das neue Entwicklerportal in API Management verwenden.
services: api-management
documentationcenter: API Management
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2019
ms.author: apimpm
ms.openlocfilehash: deb3fdf3069aaad4982d71806c209fe516e3c3d6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66743361"
---
# <a name="access-and-customize-the-new-developer-portal-in-azure-api-management"></a>Zugriff auf und Anpassen des neuen Entwicklerportals – Azure API Management

Dieser Artikel veranschaulicht, wie Sie auf das neue Azure API Management-Entwicklerportal zugreifen. Er führt Sie durch die Visual Editor-Erfahrung – Hinzufügen und Bearbeiten von Inhalten – sowie durch das Anpassen des Aussehens der Website.

![Neues Entwicklerportal für API Management](media/api-management-howto-developer-portal/cover.png)

## <a name="prerequisites"></a>Voraussetzungen

- Bearbeiten Sie den folgenden Schnellstart: [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md)
- Importieren und veröffentlichen Sie eine Azure API Management-Instanz. Weitere Informationen finden Sie unter [Importieren und Veröffentlichen](import-and-publish.md).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

> [!NOTE]
> Das neue Entwicklerportal ist zurzeit als Vorschauversion verfügbar.

## <a name="managed-and-self-hosted-versions"></a>Verwaltete und selbst gehostete Versionen

Sie können Ihr Entwicklerportal auf zwei Arten erstellen:

- **Verwaltete Version**: Indem Sie in Ihre API Management-Instanz integrierte Portal bearbeiten und anpassen, auf das Sie über die URL `<your-api-management-instance-name>.developer.azure-api.net` zugreifen können.
- **Selbst gehostete Version**: Indem Sie Ihr Portal außerhalb einer API Management-Instanz bereitstellen und selbst hosten. Dieser Ansatz ermöglicht es Ihnen, die Codebasis des Portals zu bearbeiten und die bereitgestellten Kernfunktionen zu erweitern. Details und Anweisungen finden Sie im [GitHub-Repository mit dem Quellcode des Portals][1].

## <a name="access-the-managed-version-of-the-portal"></a>Zugreifen auf die verwaltete Version des Portals

Führen Sie die folgenden Schritte aus, um auf die verwaltete Version des Portals zuzugreifen.

1. Wechseln Sie im Azure-Portal zu Ihrer API Management-Dienstinstanz.
1. Klicken Sie oben in der Navigationsleiste auf die Schaltfläche **Neues Entwicklerportal (Vorschau)** . Eine neue Browserregisterkarte mit einer administrativen Version des Portals wird geöffnet. Wenn Sie zum ersten Mal auf das Portal zugreifen, wird der Standardinhalt automatisch bereitgestellt.

## <a name="edit-and-customize-the-managed-version-of-the-portal"></a>Bearbeiten und Anpassen der verwalteten Version des Portals

In dem folgenden Video demonstrieren wir, wie Sie den Inhalt des Portals bearbeiten, das Aussehen der Website anpassen und die Änderungen veröffentlichen.

> [!VIDEO https://www.youtube.com/embed/5mMtUSmfUlw]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum neuen Entwicklerportal:

- [GitHub-Repository mit dem Quellcode][1]
- [Anweisungen zum Selbsthosting des Portals][2]
- [Öffentliche Roadmap des Projekts][3]

[1]: https://aka.ms/apimdevportal
[2]: https://github.com/Azure/api-management-developer-portal/wiki
[3]: https://github.com/Azure/api-management-developer-portal/projects
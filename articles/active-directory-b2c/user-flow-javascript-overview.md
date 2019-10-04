---
title: JavaScript und Seitenlayoutversionen – Azure Active Directory B2C | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie in Azure Active Directory B2C JavaScript aktivieren und Seitenlayoutversionen verwenden.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 0eb5c89387d8bdcf0e0b72c669c42f716ff5fbb3
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2019
ms.locfileid: "68227104"
---
# <a name="javascript-and-page-layout-versions-in-azure-active-directory-b2c"></a>JavaScript und Seitenlayoutversionen in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Azure AD B2C stellt eine Reihe von gepackten Inhalten bereit, die HTML, CSS und JavaScript für die Benutzeroberflächenelemente in Ihren Benutzerflows und benutzerdefinierten Richtlinien enthalten. Um JavaScript für Ihre Anwendungen zu aktivieren, müssen Sie ein Element zu Ihrer [benutzerdefinierten Richtlinie](active-directory-b2c-overview-custom.md) hinzufügen oder im Portal für Benutzerflows aktivieren, ein Seitenlayout auswählen und [b2clogin.com](b2clogin.md) in Ihren Anforderungen verwenden.

Wenn Sie beabsichtigen, clientseitigen [JavaScript](javascript-samples.md)-Code zu aktivieren, sollten Sie sicherstellen, dass die Elemente, auf denen der JavaScript-Code basiert, unveränderlich sind. Andernfalls könnten Änderungen zu einem unerwarteten Verhalten auf Ihren Benutzerseiten führen. Um diese Probleme zu vermeiden, können Sie die Verwendung eines Seitenlayouts erzwingen und eine Seitenlayoutversion angeben. Dadurch wird sichergestellt, dass alle Inhaltsdefinitionen, auf denen Ihr JavaScript-Code basiert, unveränderlich sind. Auch wenn Sie nicht beabsichtigen, JavaScript zu aktivieren, können Sie eine Seitenlayoutversion für Ihre Seiten angeben.

## <a name="user-flows"></a>Benutzerabläufe

In den **Eigenschaften** des Benutzerflows können Sie JavaScript aktivieren, wodurch auch die Verwendung eines Seitenlayouts erzwungen wird. Anschließend können Sie die Seitenlayoutversion für den Benutzerflow festlegen, wie im nächsten Abschnitt beschrieben.

![Benutzerablaufeigenschaften-Seite mit hervorgehobener Einstellung „JavaScript aktivieren“](media/user-flow-javascript-overview/javascript-settings.png)

### <a name="select-a-page-layout-version"></a>Auswählen einer Seitenlayoutversion

Sie können eine Seitenlayoutversion für Ihre Benutzerflowseiten unabhängig davon angeben, ob Sie JavaScript in den Benutzerfloweigenschaften aktivieren oder nicht. Öffnen Sie den Benutzerflow, und wählen Sie **Seitenlayouts** aus. Wählen Sie unter **LAYOUTNAME** eine Benutzerflowseite und dann die Option **Seitenlayoutversion** aus.

Informationen zu den verschiedenen Seitenlayoutversionen finden Sie im [Versionsänderungsprotokoll](page-layout.md#version-change-log).

![Seitenlayouteinstellungen im Portal mit der Dropdownliste für Seitenlayouts](media/user-flow-javascript-overview/page-layout-version.png)

## <a name="custom-policies"></a>Benutzerdefinierte Richtlinien

Um JavaScript in benutzerdefinierten Richtlinien zu aktivieren, fügen Sie in der Datei für die benutzerdefinierte Richtlinie dem Element **RelyingParty** das Element **ScriptExecution** hinzu. Weitere Informationen finden Sie unter [JavaScript-Beispiele für die Verwendung in Azure Active Directory B2C](javascript-samples.md).

Unabhängig davon, ob Sie JavaScript in den benutzerdefinierten Richtlinien aktivieren oder nicht, können Sie eine Seitenlayoutversion für Ihre Seiten angeben. Weitere Informationen zum Angeben eines Seitenlayouts finden Sie unter [Auswählen eines Seitenlayouts in Azure Active Directory B2C mit benutzerdefinierten Richtlinien](page-layout.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den verschiedenen Seitenlayoutversionen finden Sie im Abschnitt **Versionsänderungsprotokoll** des Artikels [Auswählen eines Seitenlayouts in Azure Active Directory B2C mit benutzerdefinierten Richtlinien](page-layout.md#version-change-log).

Beispiele für die JavaScript-Verwendung finden Sie in [JavaScript-Beispiele für die Verwendung in Azure Active Directory B2C](javascript-samples.md).

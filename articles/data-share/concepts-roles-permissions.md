---
title: Rollen und Anforderungen für Azure Data Share (Vorschau)
description: Rollen und Anforderungen für Azure Data Share (Vorschau)
author: joannapea
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: a5d70b9aa611b4f939cb46b5d25655edd818cb35
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807535"
---
# <a name="roles-and-requirements-for-azure-data-share-preview"></a>Rollen und Anforderungen für Azure Data Share (Vorschau)

Dieser Artikel beschreibt die Rollen, die erforderlich sind, um Daten mithilfe von Azure Data Share (Vorschau) freizugeben sowie zu akzeptieren und zu empfangen. 

## <a name="roles-and-requirements"></a>Rollen und Anforderungen

Um Daten über Azure Data Share freizugeben oder zu empfangen, muss das Benutzerkonto, mit dem Sie sich bei Azure anmelden, in der Lage sein, dem Speicherkonto, aus dem Sie Daten freigeben oder in dem Sie Daten empfangen, Zugriff auf Data Share zu gewähren. Typischerweise ist dies eine Berechtigung, die in der Rolle **Besitzer** vorhanden ist, oder eine benutzerdefinierte Rolle mit zugewiesener Microsoft.Authorization/Rollenzuordnungen/Schreibberechtigung. 

Damit Sie Daten für ein Azure Storage-Konto freigeben oder von einem solchen Konto empfangen können, müssen Sie der Besitzer des Kontos sein. Auch wenn Sie das Speicherkonto selbst erstellt haben, sind Sie nicht automatisch der Besitzer des Speicherkontos. Um sich selbst der Rolle „Besitzer“ des Azure Storage-Kontos hinzuzufügen, gehen Sie wie folgt vor.

1. Navigieren Sie im Azure-Portal zum Speicherkonto.
1. Wählen Sie **Zugriffssteuerung (IAM)** aus.
1. Klicken Sie auf **Hinzufügen**.
1. Fügen Sie sich als Besitzer hinzu.

Klicken Sie im Azure-Portal in der oberen rechten Ecke auf Ihren Benutzernamen und dann auf **Berechtigungen**, um Ihre Berechtigungen im Abonnement anzuzeigen. Wenn Sie Zugriff auf mehrere Abonnements besitzen, wählen Sie das entsprechende Abonnement aus. 

## <a name="resource-provider-registration"></a>Ressourcenanbieterregistrierung 

Wenn Sie eine Azure Data Share-Einladung annehmen, müssen Sie den Microsoft.DataShare-Ressourcenanbieter manuell in Ihrem Abonnement registrieren. Führen Sie diese Schritte aus, um den Microsoft.DataShare-Ressourcenanbieter in Ihrem Azure-Abonnement zu registrieren. 

1. Navigieren Sie im Azure-Portal zu **Abonnements**.
1. Wählen Sie das Abonnement aus, das Sie für Azure Data Share verwenden.
1. Klicken Sie auf **Ressourcenanbieter**.
1. Suchen Sie nach „Microsoft.DataShare“.
1. Klicken Sie auf **Registrieren**.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Rollen in Azure: [Grundlegendes zu Rollendefinitionen](../role-based-access-control/role-definitions.md)


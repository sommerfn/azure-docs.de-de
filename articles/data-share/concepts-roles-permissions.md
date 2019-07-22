---
title: Rollen und Anforderungen für Azure Data Share (Vorschau)
description: Rollen und Anforderungen für Azure Data Share (Vorschau)
author: joannapea
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: 76889904dd89da0b44dad6b47250351c73f75653
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2019
ms.locfileid: "67788525"
---
# <a name="roles-and-requirements-for-azure-data-share-preview"></a>Rollen und Anforderungen für Azure Data Share (Vorschau)

Dieser Artikel beschreibt die Rollen, die erforderlich sind, um Daten mithilfe von Azure Data Share (Vorschau) freizugeben sowie zu akzeptieren und zu empfangen. 

## <a name="roles-and-requirements"></a>Rollen und Anforderungen

Um Daten über Azure Data Share freizugeben oder zu empfangen, muss das Benutzerkonto, mit dem Sie sich bei Azure anmelden, in der Lage sein, dem Speicherkonto, aus dem Sie Daten freigeben oder in dem Sie Daten empfangen, Zugriff auf Data Share zu gewähren. Typischerweise ist dies eine Berechtigung, die in der Rolle **Besitzer** vorhanden ist, oder eine benutzerdefinierte Rolle mit zugewiesener Microsoft.Authorization/Rollenzuordnungen/Schreibberechtigung. 

Um Daten freizugeben oder aus einem Azure Storage-Konto zu empfangen, müssen Sie Besitzer des Speicherkontos sein, in dem Sie Daten empfangen. Auch wenn Sie das Speicherkonto selbst erstellt haben, sind Sie nicht automatisch der Besitzer des Speicherkontos. Um sich selbst der Rolle „Besitzer“ des Azure Storage-Kontos hinzuzufügen, gehen Sie wie folgt vor.

1. Navigieren Sie im Azure-Portal zum Speicherkonto.
1. Wählen Sie **Zugriffssteuerung (IAM)** aus.
1. Klicken Sie auf **Hinzufügen**.
1. Fügen Sie sich selbst als Besitzer von Speicherblobdaten hinzu.

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


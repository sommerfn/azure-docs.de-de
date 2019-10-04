---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/12/2019
ms.author: pafarley
ms.openlocfilehash: dce9b4ffa05b48fc1789859328b221ac1a0f3a27
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68594446"
---
Wenn Ihnen Zugriff auf die Formularerkennung gewährt wird, erhalten Sie eine Willkommens-E-Mail mit mehreren Links und Ressourcen. Verwenden Sie den Link „Azure-Portal“ in dieser Nachricht, um das Azure-Portal zu öffnen und eine Formularerkennungsressource zu erstellen. Geben Sie im Bereich **Erstellen** die folgenden Informationen an:

|    |    |
|--|--|
| **Name** | Einen aussagekräftigen Namen für Ihre Ressource. Es wird empfohlen, einen aussagekräftigen Namen auszuwählen, z. B. *MyNameFormRecognizer*. |
| **Abonnement** | Wählen Sie das Azure-Abonnement aus, dem Zugriff gewährt wurde. |
| **Location** | Der Speicherort Ihrer Cognitive Services-Instanz. Verschiedene Speicherorte können Wartezeiten verursachen, haben aber keinen Einfluss auf die Laufzeitverfügbarkeit Ihrer Ressource. |
| **Preisstufe** | Die Kosten für Ihre Ressource hängen vom ausgewählten Tarif und Ihrer Nutzung ab. Weitere Informationen finden Sie unter API-[Preise](https://azure.microsoft.com/pricing/details/cognitive-services/).
| **Ressourcengruppe** | Die [Azure-Ressourcengruppe](https://docs.microsoft.com/azure/architecture/cloud-adoption/governance/resource-consistency/azure-resource-access#what-is-an-azure-resource-group), die Ihre Ressource enthält. Sie können eine neue Gruppe erstellen oder sie einer bereits bestehenden Gruppe hinzufügen. |

> [!IMPORTANT]
> Wenn Sie im Azure-Portal eine Cognitive Service-Ressource erstellen, können Sie normalerweise einen Abonnementschlüssel für mehrere Dienste (der für mehrere Cognitive Services verwendet wird) oder einen Abonnementschlüssel für einen einzelnen Dienst (der nur für den angegebenen Cognitive Service verwendet wird) erstellen. Da die Formularerkennung derzeit als Vorschauversion verfügbar ist, ist sie jedoch nicht im Abonnement für mehrere Dienste enthalten. Sie können daher nur dann ein Abonnement für einen einzelnen Dienst erstellen, wenn Sie den Link in der Willkommens-E-Mail verwenden.

Nach Abschluss der Bereitstellung Ihrer Formularerkennungsressource suchen Sie sie im Portal in der Liste **Alle Ressourcen** und wählen sie aus. Wählen Sie dann die Registerkarte **Schlüssel** aus, um Ihre Abonnementschlüssel anzuzeigen. Beide Schlüssel gewähren Ihrer App Zugriff auf die Ressource. Kopieren Sie den Wert von **KEY 1**.
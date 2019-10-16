---
author: aahill
ms.author: aahi
ms.service: cognitive-services
ms.topic: include
ms.date: 06/20/2019
ms.openlocfilehash: 942bcc6b150f990f9a9acab0d4ef68bfb6125c1b
ms.sourcegitcommit: 9f330c3393a283faedaf9aa75b9fcfc06118b124
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2019
ms.locfileid: "71996860"
---
>[!NOTE]
> Nach dem 1. Juli 2019 erstellte Endpunkte nutzen das unten gezeigte benutzerdefinierte Format für Subdomänen. Weitere Informationen und eine vollständige Liste mit regionalen Endpunkten finden Sie unter [Benutzerdefinierte Unterdomänennamen für Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains). 

Azure Cognitive Services werden von Azure-Ressourcen dargestellt, die Sie abonnieren. Erstellen Sie im [Azure-Portal](../../cognitive-services-apis-create-account.md) eine Ressource für die Freihanderkennung. 

* Sie können auch einen kostenlosen [Testschlüssel](https://azure.microsoft.com/try/cognitive-services/#decision) abrufen, der sieben Tage lang gültig ist. Nach der Registrierung stehen dieser Schlüssel und ein Endpunkt auf der [Azure-Website](https://azure.microsoft.com/try/cognitive-services/my-apis/) zur Verfügung.

Rufen Sie nach dem Erstellen einer Ressource den Endpunkt und den Schlüssel ab, indem Sie die Ressource im [Azure-Portal](https://ms.portal.azure.com#blade/HubsExtension/BrowseResourceGroupBlade) öffnen und auf **Schnellstart** klicken.

Erstellen Sie zwei [Umgebungsvariablen](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource):

* `INK_RECOGNITION_SUBSCRIPTION_KEY`: Der Endpunkt für die Ressource. Er sieht wie folgt aus: <br> `https://<your-custom-subdomain>.api.cognitive.microsoft.com` 

* `INK_RECOGNITION_ENDPOINT`: Der Abonnementschlüssel zum Authentifizieren Ihrer Anforderungen   

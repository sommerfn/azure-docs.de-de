---
title: Entwicklerressourcen – Language Understanding
titleSuffix: Azure Cognitive Services
description: Entwickler verfügen sowohl über REST-APIs als auch über SDKs für Language Understanding.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 0707ed3dfee9af27b7cab66c7952d1e7c1d57cb8
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703199"
---
# <a name="developer-resources-for-language-understanding"></a>Entwicklerressourcen für Language Understanding

Entwickler können sowohl REST-APIs als auch SDKs für Language Understanding verwenden. 

## <a name="azure-resource-management"></a>Azure-Ressourcenverwaltung

Verwenden Sie die Azure Cognitive Services Management-Ebene, um die Ressource Language Understanding oder Cognitive Service zu erstellen, zu bearbeiten, aufzulisten und zu löschen.

Suchen Sie nach der Referenzdokumentation auf Grundlage des Tools:

* [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/cognitiveservices#az-cognitiveservices-list)

* [Azure RM PowerShell](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/?view=azurermps-4.4.1#cognitive_services)

## <a name="language-understanding-authoring-and-prediction-requests"></a>Erstellungs- und Vorhersageanforderungen für Language Understanding

Der Zugriff auf den Language Understanding-Dienst erfolgt über eine Azure-Ressource, die Sie erstellen müssen. Es gibt zwei Ressourcen: Erstellungs- und Vorhersageendpunktressourcen. Beide Ressourcen ermöglichen es Ihnen, Ihre LUIS-Ressourcen zu steuern. 

Erfahren Sie mehr über den [V3-Vorhersageendpunkt](luis-migration-api-v3.md).

### <a name="rest-apis"></a>REST-APIs

Sowohl Erstellungs- als auch Vorhersage-Endpunkt-APIs sind über Rest-APIs verfügbar:

* [Referenzdokumentation](https://go.microsoft.com/fwlink/?linkid=2092087) zur Erstellung
* [Referenzdokumentation](https://go.microsoft.com/fwlink/?linkid=2092356) zur Vorhersagelaufzeit

### <a name="language-based-sdks"></a>Sprachbasierte SDKs

|Sprache |Referenzdokumentation|Paket|Beispiele|Schnellstarts|
|--|--|--|--|--|
|C#|[Erstellung](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring?view=azure-dotnet)</br>[Vorhersage](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime?view=azure-dotnet)|[NuGet-Erstellung](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.LUIS.Authoring/)<br>[NuGet-Vorhersage](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime/)|[.NET SDK-Beispiele](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/LUIS)|[Erstellen und Verwalten einer App](sdk-csharp-quickstart-authoring-app.md)<br>[Abfragen eines Vorhersageendpunkts](sdk-csharp-quickstart-query-prediction-endpoint.md)|
|Go|[Erstellung und Vorhersage](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.0/luis)|[SDK](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.0/luis)|[Erstellung](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/quickstarts/change-model/go)<br>[Vorhersage](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/quickstarts/analyze-text/go)|[Erstellung mithilfe von REST](luis-get-started-go-add-utterance.md)<br>[Vorhersage mithilfe von REST](luis-get-started-go-get-intent.md)|
|Java|[Erstellung und Vorhersage](https://docs.microsoft.com/java/api/overview/azure/cognitiveservices/client/languageunderstanding?view=azure-java-stable)|[Maven-Erstellung](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-luis-authoring)<br>[Maven-Vorhersage](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-luis-runtime)|[Erstellung](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/quickstarts/change-model/java)<br>[Vorhersage](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/quickstarts/analyze-text/java)|[Erstellung](luis-get-started-java-add-utterance.md)<br>[Vorhersage](luis-get-started-java-get-intent.md)
|Node.js|[Erstellung](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/?view=azure-node-latest)<br>[Vorhersage](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/?view=azure-node-latest)|[NPM-Erstellung](https://www.npmjs.com/package/azure-cognitiveservices-luis-authoring)<br>[NPM-Vorhersage](https://www.npmjs.com/package/azure-cognitiveservices-luis-runtime)|[Erstellung](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/quickstarts/change-model/node)<br>[Vorhersage](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/quickstarts/analyze-text/node)|[Erstellung mithilfe von REST](https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-node-get-intent)<br>[Vorhersage mithilfe von REST](https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-node-add-utterance)|
|Python|[Erstellung und Vorhersage](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/?view=azure-python)|[Pip](https://pypi.org/project/azure-cognitiveservices-language-luis/)|[Erstellung](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/LUIS/application_quickstart.py)|[Erstellung](sdk-python-quickstart-authoring-app.md)<br>[Vorhersage mithilfe von REST](luis-get-started-python-get-intent.md)

## <a name="other-tools-and-sdks"></a>Weitere Tools und SDKs

Das Bot Framework ist als [SDK](https://github.com/Microsoft/botframework) in einer Vielzahl von Sprachen und als Dienst mit [Azure Bot Service](https://dev.botframework.com/) verfügbar. 

Bot Framework bietet [verschiedene Tools](https://github.com/microsoft/botbuilder-tools), um bei Language Understanding zu helfen, einschließlich:

* [LUDown](https://github.com/microsoft/botbuilder-tools/blob/master/packages/Ludown): Erstellen von LUIS-Language Understanding-Modellen mithilfe von Markdowndateien
* [LUIS CLI](https://github.com/microsoft/botbuilder-tools/blob/master/packages/LUIS): Erstellen und Verwalten Ihrer LUIS.ai-Anwendungen
* [Dispatch](https://github.com/microsoft/botbuilder-tools/blob/master/packages/Dispatch): Verwalten von übergeordneten und untergeordneten Apps
* [LUISGen](https://github.com/microsoft/botbuilder-tools/blob/master/packages/LUISGen): Automatisches Generieren von C#/Typescript-Klassen für Ihre LUIS-Absichten und -Entitäten
* [Bot Emulator](https://github.com/Microsoft/BotFramework-Emulator/releases): Eine Desktopanwendung, mit der Entwickler von Bots mit dem Bot Framework SDK erstellte Bots testen und debuggen können


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu allgemeinen [HTTP-Fehlercodes](luis-reference-response-codes.md)
---
title: 'Übersicht: Azure – serverlos'
description: Erstellen Sie leistungsstarke Lösungen in der Cloud, ohne über die Infrastruktur nachdenken zu müssen.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: jeffhollan
ms.author: jehollan
ms.reviewer: klam, estfan, LADocs
ms.topic: article
ms.date: 03/30/2017
ms.openlocfilehash: 363002712bdd06e74360de9af186f5a458a4999d
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2019
ms.locfileid: "68385361"
---
# <a name="overview-azure-serverless-with-azure-logic-apps-and-azure-functions"></a>Übersicht: Serverloses Azure mit Azure Logic Apps und Azure Functions

[Serverlose](https://azure.microsoft.com/solutions/serverless/) Apps bieten Vorteile, wie z.B. eine bessere Entwicklungsgeschwindigkeit, reduzierten Code, Einfachheit und Skalierung. In diesem Artikel werden verschiedene Attribute serverloser Lösungen und serverlose Angebote von Azure behandelt.

## <a name="what-is-serverless"></a>Was bedeutet serverlos?

Serverlos bedeutet nicht, dass keine Server vorhanden sind – es bedeutet lediglich, dass sich Entwickler nicht um Server kümmern müssen. Ein großer Teil der herkömmlichen Anwendungsentwicklung liegt darin, Fragen zum Skalieren, Hosten und Überwachen von Anwendungen zu beantworten, um die Anforderungen der Anwendung zu erfüllen. Bei Serverlosigkeit werden diese Fragen als Teil der Lösung aufgegriffen. Darüber hinaus werden serverlose Apps auf Basis eines verbrauchsbasierten Plans abgerechnet. Wenn die App nie verwendet wird, fallen keine Gebühren an. Diese Features helfen Entwicklern, sich ausschließlich auf die Geschäftslogik einer Lösung zu konzentrieren.

Die Hauptdienste in Azure für die Serverlosigkeit sind [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/) und [Azure Functions](https://azure.microsoft.com/services/functions/). Beide Lösungen folgen den oben beschriebenen Prinzipien und ermöglichen Entwicklern, robuste Cloud-Apps mit minimalem Code zu erstellen.

## <a name="what-is-azure-logic-apps"></a>Was ist Azure Logic Apps?

[Azure Logic Apps](logic-apps-overview.md) ermöglicht die Vereinfachung und die Implementierung skalierbarer Integrationen und Workflows in die Cloud. Dieser Dienst stellt einen visuellen Designer bereit, mit dem Sie Ihre Prozesse in Form von Schritten (als Workflow bezeichnet) modellieren und automatisieren können. Es gibt [viele Connectors](../connectors/apis-list.md) für Clouddienste und lokale Systeme, mit denen Sie schnell eine Verbindung zwischen einer serverlosen App und anderen APIs herstellen können. Jede Logik-App beginnt mit einem Trigger, z.B. „Wenn Dynamics CRM ein Konto hinzugefügt wird“. Nach Auslösen des Triggers kann der Workflow Kombinationen von Aktionen, Konvertierungen und Bedingungslogik ausführen. Logic Apps ist eine gute Wahl zum sorgfältigen Organisieren verschiedener Azure Functions in einem Prozess – besonders, wenn der Prozess die Interaktion mit einem externen System oder einer API benötigt.

Starten Sie zum Einstieg in Logic Apps mit dem [Erstellen Ihrer ersten Logik-App](quickstart-create-first-logic-app-workflow.md). Technische Informationen zu Logic Apps finden Sie in der [Entwicklerreferenz](logic-apps-workflow-definition-language.md).

## <a name="what-is-azure-functions"></a>Übersicht zu Azure Functions

Azure Functions ist ein Dienst, mit dem Sie ganz einfach Codeelemente (Funktionen) in der Cloud ausführen können. Sie müssen nur den Code schreiben, der für das aktuelle Problem erforderlich ist, ohne sich über eine gesamte App oder die erforderliche Infrastruktur Gedanken machen zu müssen. Mit Functions können Entwickler noch produktiver arbeiten und ihre bevorzugte Programmiersprache verwenden (z.B. C#, F#, Node.js, Python oder PHP). Sie bezahlen nur für die Zeit, in der Ihr Code ausgeführt wird. Azure skaliert flexibel nach Bedarf.

Das Tutorial [Erstellen Ihrer ersten Funktion im Azure-Portal](../azure-functions/functions-create-first-azure-function.md) ist der ideale Einstieg in Azure Functions. Technische Informationen zu Functions finden Sie in der [Entwicklerreferenz](../azure-functions/functions-reference.md).

## <a name="how-can-i-build-and-deploy-serverless-apps-in-azure"></a>Wie kann ich serverlose Apps in Azure erstellen und bereitstellen?

Azure bietet umfangreiche Tools zum Entwickeln, Bereitstellen und Verwalten von serverlosen Apps. Sie können Apps mit [Tools in Visual Studio](logic-apps-serverless-get-started-vs.md) oder [Visual Studio Code](quickstart-create-logic-apps-visual-studio-code.md) direkt im Azure-Portal erstellen. Nachdem Sie Ihre App erstellt haben, können Sie [diese App mit Azure Resource Manager-Vorlagen schnell bereitstellen](logic-apps-deploy-azure-resource-manager-templates.md). Azure bietet auch Überwachung, auf die Sie direkt über das Azure-Portal, über die API oder über SDKs bzw. alternativ über integrierte Tools für Azure Monitor-Protokolle und Application Insights zugreifen können.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen einer serverlosen App in Visual Studio](logic-apps-serverless-get-started-vs.md)
* [Serverloses Erstellen eines Customer Insights-Dashboards](logic-apps-scenario-social-serverless.md)
* [Automatisieren der Logik-App-Bereitstellung](logic-apps-azure-resource-manager-templates-overview.md)

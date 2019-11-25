---
title: Beispiele und allgemeine Szenarien – Azure Logic Apps
description: Hier finden Sie Beispiele, allgemeine Szenarien, Tutorials und exemplarische Vorgehensweisen für Azure Logic Apps.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 07/31/2019
ms.openlocfilehash: 9e245d1a0054d9de5b8b524416c8c92eb02353a7
ms.sourcegitcommit: d47a30e54c5c9e65255f7ef3f7194a07931c27df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73025517"
---
# <a name="common-scenarios-examples-tutorials-and-walkthroughs-for-azure-logic-apps"></a>Gängige Szenarien, Beispiele, Tutorials und exemplarische Vorgehensweisen für Azure Logic Apps

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) unterstützt Sie bei der Orchestrierung und Integration verschiedener Dienste durch die Bereitstellung von [Hunderten einsatzbereiten Connectors](../connectors/apis-list.md) – von der lokalen SQL Server-Instanz über SAP bis hin zu Azure Cognitive Services. Der Logic Apps-Dienst arbeitet „serverlos“. Daher müssen Sie sich über Skalierung oder Instanzen keine Gedanken machen. Sie brauchen lediglich den Workflow mit einem Trigger und Aktionen definieren, die im Workflow ausgeführt werden. Die zugrundeliegende Plattform kümmert sich um Skalierung, Verfügbarkeit und Leistung. Logic Apps ist besonders nützlich für Anwendungsfälle und Szenarien, in denen Sie mehrere Aktionen auf verschiedenen Systemen koordinieren müssen.

Um mehr über die vielen Muster und Funktionen zu erfahren, die von Azure Logic Apps unterstützt werden, finden Sie hier einige gängige Beispiele und Szenarien.

## <a name="popular-starting-points-for-logic-app-workflows"></a>Beliebte Ausgangspunkte für Logik-App-Workflows

Jede Logik-App beginnt mit nur einem [*Trigger*](../logic-apps/logic-apps-overview.md#logic-app-concepts), der Ihren Logik-App-Workflow startet und alle Daten als Teil dieses Triggers übergibt. Einige Connectors bieten Trigger, die einen der folgenden Typen haben:

* *Abfragetrigger*: Fragt einen Dienstendpunkt regelmäßig auf neue Daten ab. Wenn neue Daten vorhanden sind, wird der Trigger erstellt, der eine neue Workflowinstanz mit den Daten als Eingabe ausführt.

* *Pushtrigger*: Lauscht auf Daten am Dienstendpunkt und wartet, bis ein bestimmtes Ereignis erfolgt. Wenn das Ereignis eintritt, wird der Trigger sofort ausgelöst. Eine neue Workflowinstanz wird angelegt und ausgeführt, die alle verfügbaren Daten als Eingabe verwendet.

Hier nur einige Beispiele beliebter Trigger:

* Abruf:

  * Der [Trigger **Wiederholung**](../connectors/connectors-native-recurrence.md) ermöglicht Ihnen das Festlegen von Startdatum und -uhrzeit sowie der Wiederholung zum Auslösen Ihrer Logik-App. Beispielsweise können Sie die Wochentage und Tageszeiten auswählen, um Ihre Logik-App auszulösen. Weitere Informationen finden Sie in den folgenden Themen:

    * [Planen und Ausführen von wiederkehrenden automatisierten Aufgaben, Prozessen und Workflows mit Azure Logic Apps](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md)
    * [Tutorial: Überprüfen der Verkehrslage gemäß eines Zeitplans mit Azure Logic Apps](../logic-apps/tutorial-build-schedule-recurring-logic-app-workflow.md)

  * Mit dem Trigger "Wenn eine E-Mail empfangen wird" können Sie Ihre Logik-App von jedem E-Mail-Anbieter, der von Logic Apps unterstützt wird, z.B. [Office 365 Outlook](../connectors/connectors-create-api-office365-outlook.md), [Gmail](https://docs.microsoft.com/connectors/gmail/), [Outlook.com](https://docs.microsoft.com/connectors/outlook/) usw., auf neue E-Mails überprüfen. Weitere Informationen finden Sie in den folgenden Themen: 

    * [Tutorial: Verwalten von Adressenlistenanforderungen mit Azure Logic Apps](../logic-apps/tutorial-process-mailing-list-subscriptions-workflow.md)
    * [Tutorial: Automatisieren der Verarbeitung von E-Mails und Anlagen mit Azure Logic Apps](../logic-apps/tutorial-process-email-attachments-workflow.md)

  * Der [**HTTP**-Trigger](../connectors/connectors-native-http.md) lässt Ihre Logik-App einen angegebenen Dienstendpunkt überprüfen, indem über HTTP kommuniziert wird.
  
* Push:

  * Der [Trigger **Anforderung**](../connectors/connectors-native-reqres.md) ermöglicht Ihrer Logik-App, HTTP-Anforderungen zu empfangen und in Echtzeit auf Ereignisse zu reagieren.

  * Der Trigger [**HTTP-Webhook**](../connectors/connectors-native-webhook.md) abonniert einen Dienstendpunkt durch Registrieren einer *Rückruf-URL* beim jeweiligen Dienst. Auf diese Weise kann der Dienst den Trigger einfach benachrichtigen, wenn das angegebene Ereignis eintritt, sodass der Trigger den Dienst nicht mehr abfragen muss.

Nach Empfangen einer Benachrichtigung über neue Daten oder ein Ereignis wird der Trigger ausgelöst. Er erzeugt eine neue Logik-App-Workflowinstanz und führt die Aktionen im Workflow aus. Sie können im gesamten Workflow auf beliebige Daten im Trigger zugreifen. Der Trigger "Bei einem neuen Tweet" übergibt z.B. den Tweetinhalt an die Ausführung der Logik-App. Probieren Sie für den Einstieg in Azure Logic Apps die folgenden Schnellstartthemen aus:

* [Schnellstart: Erstellen Ihres ersten automatisierten Workflows mit Azure Logic Apps im Azure-Portal](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Schnellstart: Erstellen von automatisierten Aufgaben, Prozessen und Workflows mit Azure Logic Apps und Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md)
* [Schnellstart: Erstellen und Verwalten automatisierter Logik-App-Workflows mit Visual Studio Code](../logic-apps/quickstart-create-logic-apps-visual-studio-code.md)

## <a name="respond-to-triggers-and-extend-actions"></a>Reagieren auf Trigger und Erweitern von Aktionen

Für Systeme und Dienste, für die möglicherweise keine veröffentlichten Connectors verfügbar sind, können Sie Logic Apps darüber hinaus erweitern.

* [Erstellen von benutzerdefinierten Triggern oder Aktionen](../logic-apps/logic-apps-create-api-app.md)
* [Einrichten von Aktionen mit langer Ausführungszeit für Workflowausführungen](../logic-apps/logic-apps-create-api-app.md)
* [Reagieren auf externe Ereignisse und Aktionen mit Webhooks](../logic-apps/logic-apps-create-api-app.md)
* [Aufrufen, Auslösen oder Schachteln von Workflows mithilfe von synchronen Antworten auf HTTP-Anforderungen](../logic-apps/logic-apps-http-endpoint.md)
* [Tutorial: Erstellen eines KI-gestützten sozialen Dashboards innerhalb von Minuten mithilfe von Logic Apps und Power BI](https://aka.ms/logicappsdemo)
* [Video: Reagieren auf Twilio SMS-Webhooks und Senden einer Textantwort](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-Logic-Apps-Walkthrough-Webhook-Functions-and-an-SMS-Bot)

## <a name="control-flow-error-handling-and-logging-capabilities"></a>Ablaufsteuerungs-, Fehlerbehandlungs- und Protokollierungsfunktionen

Logic Apps enthalten umfangreiche Funktionen für die erweiterte Ablaufsteuerung, wie Bedingungen, Schalter, Schleifen und Bereiche. Um robuste Lösungen sicherzustellen, können Sie darüber hinaus in Ihren Workflows Fehler- und Ausnahmebehandlung implementieren. Für Benachrichtigungs- und Diagnoseprotokolle für den Status von Workflowausführungen stellt Azure Logic Apps ferner Überwachung und Warnungen zur Verfügung.

* Durchführen verschiedener Aktionen basierend auf [Bedingungsanweisungen](../logic-apps/logic-apps-control-flow-conditional-statement.md) und [Wechseln von Anweisungen](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Wiederholen von Schritten oder Verarbeiten von Elementen in Arrays und Sammlungen mit Schleifen](../logic-apps/logic-apps-control-flow-loops.md)
* [Gruppieren von Aktionen im Zusammenhang mit Bereichen](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
* [Erstellen von Fehler- und Ausnahmebehandlung in einem Workflow](../logic-apps/logic-apps-exception-handling.md)
* [Anwendungsfall: Wie ein Unternehmen im Gesundheitswesen Logic Apps-Ausnahmebehandlung für HL7 FHIR-Workflows einsetzt](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [Überwachen des Status, Einrichten der Diagnoseprotokollierung und Aktivieren von Warnungen für Azure Logic Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [Monitor and get insights about logic app runs with Operations Management Suite (OMS) and Log Analytics](../logic-apps/logic-apps-monitor-your-logic-apps-oms.md) (Überwachen und Gewinnen von Erkenntnissen zu Logik-App-Ausführungen mit Operations Management Suite (OMS) und Log Analytics)

## <a name="deploy-and-manage-logic-apps"></a>Bereitstellen und Verwalten von Logic Apps

Sie können Logik-Apps mithilfe von Visual Studio, Azure DevOps oder beliebigen anderen Tools für die Quellcodeverwaltung und automatische Builderstellung vollständig entwickeln und bereitstellen. Um die Entwicklung für Workflows und abhängige Verbindungen in einer Ressourcenvorlage zu unterstützen, verwendet Logic Apps Azure-Vorlagen für die Ressourcenbereitstellung. Visual Studio-Tools generieren diese Vorlagen automatisch, die Sie zur Versionsverwaltung in die Quellcodeverwaltung einchecken können.

* [Erstellen und Bereitstellen von Logik-Apps mit Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md)
* [Überwachen des Status, Einrichten der Diagnoseprotokollierung und Aktivieren von Warnungen für Azure Logic Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [Automatisieren der Logik-App-Bereitstellung](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)
* [Beispiel: Herstellen einer Verbindung mit Azure Service Bus-Warteschlangen aus Azure Logic Apps und Bereitstellen mit Azure Pipelines in Azure DevOps](https://docs.microsoft.com/samples/azure-samples/azure-logic-apps-deployment-samples/connect-to-azure-service-bus-queues-from-azure-logic-apps-and-deploy-with-azure-devops-pipelines/)
* [Beispiel: Herstellen einer Verbindung mit Azure-Speicherkonten aus Azure Logic Apps und Bereitstellen mit Azure Pipelines in Azure DevOps](https://docs.microsoft.com/samples/azure-samples/azure-logic-apps-deployment-samples/connect-to-azure-storage-accounts-from-azure-logic-apps-and-deploy-with-azure-devops-pipelines/)
* [Beispiel: Einrichten einer Funktions-App-Aktion für Azure Logic Apps und Bereitstellen mit Azure Pipelines in Azure DevOps](https://docs.microsoft.com/samples/azure-samples/azure-logic-apps-deployment-samples/set-up-an-azure-function-app-action-for-azure-logic-apps-and-deploy-with-azure-devops-pipelines/)
* [Beispiel: Herstellen einer Verbindung mit einem Integrationskonto aus Azure Logic Apps und Bereitstellen mit Azure Pipelines in Azure DevOps](https://docs.microsoft.com/samples/azure-samples/azure-logic-apps-deployment-samples/connect-to-an-integration-account-from-azure-logic-apps-and-deploy-by-using-azure-devops-pipelines/)

## <a name="content-types-conversions-and-transformations-within-a-run"></a>Inhaltstypen, Konvertierungen und Transformationen während der Ausführung

Mithilfe der vielen Funktionen in der [Workflowdefinitionssprache](https://aka.ms/logicappsdocs) von Azure Logic Apps können Sie auf mehrere Inhaltstypen zugreifen, sie konvertieren und transformieren. Beispielsweise können Sie mit den Workflowausdrücken `@json()` und `@xml()` zwischen einer Zeichenfolge, JSON und XML konvertieren. Die Logic Apps-Engine behält Inhaltstypen bei, um die verlustfreie Übertragung von Inhalten zwischen Diensten zu unterstützen.

* [Funktionsweise von Workflowausdrücken in Logic Apps](../logic-apps/logic-apps-author-definitions.md)
* [Verarbeitung von nicht als JSON vorliegenden Inhaltstypen](../logic-apps/logic-apps-content-type.md), wie `application/xml`, `application/octet-stream` und `multipart/formdata`
* [Schema der Definitionssprache für Workflows für Azure Logic Apps](https://aka.ms/logicappsdocs)

## <a name="other-integrations-and-capabilities"></a>Weitere Integrationen und Funktionen

Logic Apps bieten außerdem Integration mit vielen Diensten, wie etwa Azure Functions, Azure API Management, Azure App Services und benutzerdefinierten HTTP-Endpunkten, z.B. REST und SOAP.

* [Erstellen eines sozialen Dashboards in Echtzeit mit Azure Serverless](../logic-apps/logic-apps-scenario-social-serverless.md)
* [Aufrufen von Azure Functions aus Logic-Apps](../logic-apps/logic-apps-azure-functions.md)
* [Tutorial: Auslösen von Logik-Apps mit Azure Functions](../logic-apps/logic-apps-scenario-function-sb-trigger.md)
* [Tutorial: Überwachen von Änderungen an virtuellen Computern mit Azure Event Grid und Logic Apps](../event-grid/monitor-virtual-machine-changes-event-grid-logic-app.md)
* [Tutorial: Erstellen einer Funktion, die in Azure Logic Apps integriert ist](../azure-functions/functions-twitter-email.md)
* [Tutorial: IoT-Remoteüberwachung und -Benachrichtigungen mit Azure Logic Apps zum Herstellen einer Verbindung zwischen Ihrem IoT Hub und Postfach](../iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps.md)
* [Blog: Call SOAP endpoints from logic apps (Aufrufen von SOAP-Endpunkten aus Logic Apps)](https://blogs.msdn.microsoft.com/logicapps/2016/04/07/using-soap-services-with-logic-apps/)

## <a name="end-to-end-scenarios"></a>Komplettszenarien

* [Whitepaper: End-to-end case management integration in the utilities industry with Azure services (Integration der nahtlosen Fallverwaltung in der Versorgungswirtschaft mit Azure-Diensten)](https://aka.ms/enterprise-integration-e2e-case-management-utilities-logic-apps)

## <a name="customer-stories"></a>Kundenstimmen

Erfahren Sie, wie Azure Logic Apps sowie andere Azure-Dienste und Microsoft-Produkte [diesen Unternehmen](https://aka.ms/logic-apps-customer-stories) geholfen haben, durch Vereinfachen, Organisieren, Automatisieren und Orchestrieren komplexer Prozesse ihre Flexibilität zu verbessern und sich auf ihr Kerngeschäft zu konzentrieren.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [Connectors für Logic Apps](../connectors/apis-list.md).
* Erfahren Sie mehr über [Szenarien für die B2B-Unternehmensintegration mit Azure Logic Apps](../logic-apps/logic-apps-enterprise-integration-overview.md).

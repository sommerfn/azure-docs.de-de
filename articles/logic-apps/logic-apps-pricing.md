---
title: Preis- und Abrechnungsmodell – Azure Logic Apps
description: Überblick darüber, wie das Preis- und Abrechnungsmodell für Azure Logic Apps funktioniert.
services: logic-apps
ms.service: logic-apps
ms.suite: logic-apps
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
ms.topic: conceptual
ms.date: 07/19/2019
ms.openlocfilehash: f4651e40a0f4269dc5c8a2fcd4c0e25c1e9bbb3c
ms.sourcegitcommit: 38251963cf3b8c9373929e071b50fd9049942b37
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73045105"
---
# <a name="pricing-model-for-azure-logic-apps"></a>Preismodell für Azure Logic Apps

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) unterstützt Sie dabei, automatisierte Integrationsworkflows zu erstellen und auszuführen, die in der Cloud horizontal herunterskaliert werden können. In diesem Artikel ist beschrieben, wie Abrechnung und Preise für Azure Logic Apps funktionieren. Eine Preisübersicht finden Sie unter [Logic Apps – Preise](https://azure.microsoft.com/pricing/details/logic-apps).

<a name="consumption-pricing"></a>

## <a name="consumption-pricing-model"></a>Modell für nutzungsbasierte Preise

Bei neuen Logik-Apps, die unter dem öffentlichen oder „globalen“ Azure Logic Apps-Dienst ausgeführt werden, bezahlen Sie nur für die tatsächliche Nutzung. Für diese Logik-Apps werden ein nutzungsbasierter Tarif und ein entsprechendes Preismodell verwendet. In Ihrer Logik-App ist jeder Schritt eine Aktion, und mit Azure Logic Apps werden alle Aktionen gemessen, die in Ihrer Logik-App ausgeführt werden.

Zu Aktionen gehören beispielsweise:

* Trigger, die spezielle Aktionen sind. Jede Logik-App erfordert einen Trigger als ersten Schritt.
* [Integrierte oder native Aktionen](../connectors/apis-list.md#built-in) wie HTTP, Aufrufe von Azure Functions und API Management usw.
* Aufrufe von [verwalteten Connectors](../connectors/apis-list.md##managed-connectors), z. B. Outlook 365, Dropbox usw.
* Ablaufsteuerungsschritte, z. B. Schleifen, Bedingungsanweisungen usw.

Für [Standardconnectors](../connectors/apis-list.md#managed-connectors) wird der entsprechende [Preis für Standardconnectors](https://azure.microsoft.com/pricing/details/logic-apps) berechnet. Für allgemein verfügbare [Unternehmensconnectors](../connectors/apis-list.md#managed-connectors) wird der [Preis für Unternehmensconnectors](https://azure.microsoft.com/pricing/details/logic-apps) berechnet, während für die öffentliche Vorschauversion der Unternehmensconnectors der [Preis für Standardconnectors](https://azure.microsoft.com/pricing/details/logic-apps) berechnet wird.

Erfahren Sie mehr darüber, wie die Abrechnung für [Trigger](#triggers) und [Aktionen](#actions) funktioniert.

<a name="fixed-pricing"></a>

## <a name="fixed-pricing-model"></a>Feststehendes Preismodell

Eine [*Integrationsdienstumgebung* (Integration Service Environment, ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) ermöglicht Ihnen privates, isoliertes und dediziertes Erstellen und Ausführen von Logik-Apps, die auf Ressourcen in einem virtuellen Azure-Netzwerk zugreifen können. Für neue Logik-Apps, die in einer ISE ausgeführt werden, zahlen Sie für die folgenden Funktionen einen [monatlichen Fixpreis](https://azure.microsoft.com/pricing/details/logic-apps):

* [Integrierte Trigger und Aktionen](../connectors/apis-list.md#built-in)

* [Standardconnectors](../connectors/apis-list.md#managed-connectors)

* [Unternehmensconnectors](../connectors/apis-list.md#enterprise-connectors) mit beliebig vielen Verbindungen

* Nutzung des [Integrationskontos](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) ohne zusätzliche Kosten, basierend auf Ihrer [ISE-SKU](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level):

  * **Premium-SKU**: Einzelnes Integrationskonto im [Tarif „Standard“](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits)

  * **Entwickler-SKU**: Ein einzelnes Integrationskonto im [Tarif „Free“](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits)

  Jede ISE-SKU ist auf maximal fünf Integrationskonten beschränkt. Es fallen zusätzliche Kosten an, wenn Sie basierend auf Ihrer ISE-SKU weitere Integrationskonten nutzen möchten:

  * **Premium-SKU**: Bis zu vier weitere Standard Konten. Keine Free- oder Basic-Konten.

  * **Entwickler-SKU**: Entweder bis zu vier weitere Standard-Konten oder maximal fünf Standard-Konten insgesamt. Keine Basic-Konten.

Weitere Informationen zu den Grenzwerten für Integrationskonten finden Sie unter [Logic Apps-Grenzwerte und -Konfiguration](../logic-apps/logic-apps-limits-and-config.md#integration-account-limits). Weitere Informationen zu [Tarifen für Integrationskonten und den zugehörigen Preismodellen](#integration-accounts) erhalten Sie weiter unten in diesem Thema.

Für die Premium-ISE-SKU hat die Basiseinheit eine feste Kapazität – wenn Sie mehr Durchsatz benötigen, können Sie entweder während der Erstellung oder danach [weitere Skalierungseinheiten hinzufügen](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#add-capacity). Die Developer-ISE-SKU verfügt nicht über die Funktion zum Hinzufügen weiterer Skalierungseinheiten. Für Logik-Apps, die in einer ISE ausgeführt werden, fallen keine Kosten für die Datenaufbewahrung an.

> [!NOTE]
> In einer ISE wird für integrierte Trigger und Aktionen die Bezeichnung **Core** angezeigt, und diese Elemente werden in derselben ISE wie Ihre Logik-Apps ausgeführt. Standard- und Unternehmensconnectors, für die die Bezeichnung **ISE** angezeigt wird, werden in derselben ISE wie Ihre Logik-Apps ausgeführt. Connectors, für die die Bezeichnung „ISE“ nicht angezeigt wird, werden im globalen Logic Apps-Dienst ausgeführt.

Eine Preisübersicht finden Sie unter [Logic Apps – Preise](https://azure.microsoft.com/pricing/details/logic-apps).

<a name="connectors"></a>

## <a name="connectors"></a>Connectors

Azure Logic Apps-Connectors ermöglichen Ihren Logik-Apps Zugriff auf Apps, Dienste und Systeme in der Cloud oder auf einer lokalen Ressource, indem sie [Trigger](#triggers), [Aktionen](#actions) oder beides zur Verfügung stellen. Connectors werden entweder als „Standard“ oder als „Enterprise“ (Unternehmen) klassifiziert. Eine Übersicht über diese Connectors finden Sie unter [Connectors für Azure Logic Apps](../connectors/apis-list.md). Wenn keine vordefinierten Connectors für die REST-APIs verfügbar sind, die Sie in Ihren Logik-Apps verwenden möchten, können Sie [benutzerdefinierte Connectors](https://docs.microsoft.com/connectors/custom-connectors) erstellen, bei denen es sich lediglich um Wrapper für diese REST-APIs handelt. Benutzerdefinierte Connectors werden als Standardconnectors abgerechnet. Die folgenden Abschnitte enthalten weitere Informationen darüber, wie die Abrechnung für Trigger und Aktionen funktioniert.

<a name="triggers"></a>

## <a name="triggers"></a>Trigger

Trigger sind spezielle Aktionen, mit denen eine Logik-App-Instanz erstellt wird, wenn ein bestimmtes Ereignis eintritt. Trigger agieren auf verschiedene Weise, wovon abhängt, wie die Logik-App nutzungsbezogen abgerechnet wird. Nachfolgend sind die verschiedenen Triggerarten aufgelistet, die es in Azure Logic Apps gibt:

* **Abfragetrigger**: Mit diesem Trigger wird ein Endpunkt fortlaufend auf Nachrichten überprüft, die die Kriterien für das Erstellen einer Logik-App-Instanz und Starten des Workflows erfüllen. Auch wenn keine Logik-App-Instanz erstellt wird, rechnet Logic Apps jede Abrufanforderung als Ausführung ab. Um das Abrufintervall festzulegen, richten Sie den Trigger über den Logik-App-Designer ein.

  [!INCLUDE [logic-apps-polling-trigger-non-standard-metering](../../includes/logic-apps-polling-trigger-non-standard-metering.md)]

* **Webhooktrigger**: Dieser Trigger wartet darauf, dass von einem Client eine Anforderung an einen bestimmten Endpunkt gesendet wird. Jede Anforderung, die an den Webhookendpunkt gesendet wird, wird als eine Aktionsausführung gezählt. Sowohl beim Anforderungstrigger als auch beim HTTP-Webhooktrigger handelt es sich beispielsweise um Webhooktrigger.

* **Wiederholungstrigger**: Dieser Trigger erstellt eine Logik-App-Instanz basierend auf dem Wiederholungsintervall, das Sie im Trigger einrichten. Sie können beispielsweise einen Wiederholungstrigger einrichten, der alle drei Tage oder nach einem komplexeren Zeitplan ausgeführt wird.

<a name="actions"></a>

## <a name="actions"></a>Aktionen

Azure Logic Apps rechnet „integrierte“ Aktionen, z. B. HTTP, als native Aktionen ab. Integrierte Aktionen umfassen beispielsweise HTTP-Aufrufe, Aufrufe von Azure Functions oder API Management und Schritte der Ablaufsteuerung, z. B. Bedingungen, Schleifen und switch-Anweisungen. Jede Aktion hat ihren eigenen Aktionstyp. Zum Beispiel haben Aktionen, mit denen [Connectors](https://docs.microsoft.com/connectors) aufgerufen werden, den Typ „ApiConnection“. Diese Connectors sind als Standard- oder Unternehmensconnectors klassifiziert, deren Verbrauch zu den jeweiligen [Preisen](https://azure.microsoft.com/pricing/details/logic-apps) abgerechnet wird. Unternehmensconnectors in einer *Vorschauversion* werden als Standardconnectors in Rechnung gestellt.

Azure Logic Apps rechnet alle erfolgreichen und nicht erfolgreichen Aktionen als Ausführungen ab. Die folgenden Aktionen werden von Logic Apps jedoch nicht abgerechnet:

* Aktionen, die aufgrund von nicht erfüllten Bedingungen übersprungen werden
* Aktionen, die nicht ausgeführt werden, weil die Logik-App vor dem Abschluss beendet wurde

Für Aktionen, die innerhalb von Schleifen ausgeführt werden, zählt Azure Logic Apps jede Aktion für jeden Zyklus in der Schleife. Angenommen, Sie verfügen über eine „for each“-Schleife, mit der eine Liste verarbeitet wird. Logic Apps rechnet eine Aktion in dieser Schleife ab, indem die Anzahl von Listenelementen mit der Anzahl von Aktionen der Schleife multipliziert wird. Anschließend wird die Aktion hinzugefügt, mit der die Schleife gestartet wird. Daher lautet die Berechnung für eine Liste mit zehn Elementen (10 * 1) + 1, sodass sich 11 Aktionsausführungen ergeben.

## <a name="disabled-logic-apps"></a>Deaktivierte Logik-Apps

Deaktivierte Logik-Apps werden nicht berechnet, da für sie keine neuen Instanzen erstellt werden können. Nachdem Sie eine Logik-App deaktiviert haben, kann es einige Zeit dauern, bis die derzeit ausgeführten Instanzen vollständig beendet werden.

<a name="integration-accounts"></a>

## <a name="integration-accounts"></a>Integrationskonten

Ein [festes Preismodell](https://azure.microsoft.com/pricing/details/logic-apps) gilt für [Integrationskonten](logic-apps-enterprise-integration-create-integration-account.md), mit denen Sie die Features [B2B und EDI](logic-apps-enterprise-integration-b2b.md) und [XML-Verarbeitung](logic-apps-enterprise-integration-xml.md) in Azure Logic Apps ohne zusätzliche Kosten erkunden, entwickeln und testen können. Jedes Azure-Abonnement kann über eine [bestimmte Höchstzahl an Integrationskonten](../logic-apps/logic-apps-limits-and-config.md#integration-account-limits) verfügen. Für jedes Integrationskonto kann eine bestimmte [maximale Anzahl von Artefakten](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits) gespeichert werden, z. B. Handelspartner, Vereinbarungen, Karten, Schemas, Assemblys, Zertifikate, Batchkonfigurationen usw.

Für Azure Logic Apps werden Integrationskonten vom Typ „Free“, „Basic“ und „Standard“ angeboten. Die Tarife „Basic“ und „Standard“ werden über die Vereinbarung zum Servicelevel (SLA) für Logic Apps unterstützt. Der Tarif „Free“ verfügt nicht über eine SLA-Unterstützung, und es gelten Grenzwerte für Durchsatz und Nutzung. Mit Ausnahme von Integrationskonten im Free-Tarif kann jede Azure-Region über mehr als ein Integrationskonto verfügen. Eine Preisübersicht finden Sie unter [Logic Apps – Preise](https://azure.microsoft.com/pricing/details/logic-apps/).

Wenn Sie eine [*Integrationsdienstumgebung* (Integration Service Environment, ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) verwenden ([Premium oder Developer](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level)), kann diese insgesamt fünf Integrationskonten aufweisen. Weitere Informationen zum festen Preismodell für eine ISE finden Sie im obigen Abschnitt [Feststehendes Preismodell](#fixed-pricing) dieses Themas. Eine Preisübersicht finden Sie unter [Logic Apps – Preise](https://azure.microsoft.com/pricing/details/logic-apps).

Sehen Sie sich diese Beschreibungen von Anwendungsfällen an, um die Entscheidung zwischen Integrationskonten vom Typ „Free“, „Basic“ oder „Standard“ treffen zu können:

* **Free:** Ein Konto zum Ausprobieren von Testszenarien. Nicht für Produktionsszenarien geeignet.

* **Basic**: Verwenden Sie ein Konto dieses Typs, wenn Sie nur die Behandlung von Nachrichten nutzen oder als Partner für ein kleines Unternehmen fungieren möchten, das eine Handelspartnerbeziehung mit einem größeren Unternehmen eingegangen ist.

* **Standard**: Kontotyp für komplexere B2B-Beziehungen und eine höhere Anzahl von Entitäten, die Sie verwalten müssen.

<a name="data-retention"></a>

## <a name="data-retention"></a>Beibehaltung von Daten

Mit Ausnahme von Logik-Apps, die in einer ISE (Integration Service Environment) ausgeführt werden, werden alle Ein- und Ausgaben, die während der Ausführung Ihrer Logik-App gespeichert wurden, entsprechend dem [Ausführungsaufbewahrungszeitraum](logic-apps-limits-and-config.md#run-duration-retention-limits) einer Logik-App in Rechnung gestellt. Für Logik-Apps, die in einer ISE ausgeführt werden, fallen keine Kosten für die Datenaufbewahrung an. Eine Preisübersicht finden Sie unter [Logic Apps – Preise](https://azure.microsoft.com/pricing/details/logic-apps).

Für die Überwachung der Speichernutzung Ihrer Logik-App haben Sie folgende Möglichkeiten:

* Anzeigen der Speichereinheiten in GB, die Ihre Logik-App monatlich verwendet
* Anzeigen der Größen der Eingaben und Ausgaben für eine bestimmte Aktion im Ausführungsverlauf Ihrer Logik-App

<a name="storage-consumption"></a>

### <a name="view-logic-app-storage-consumption"></a>Anzeigen der Speichernutzung einer Logik-App

1. Suchen Sie im Azure-Portal nach Ihrer Logik-App, und öffnen Sie sie.

1. Wählen Sie im Menü Ihrer Logik-App unter **Überwachung** die Option **Metrik** aus.

1. Wählen Sie im rechten Bereich unter **Diagrammtitel** in der Liste **Metrik** die Option **Nutzungsabrechnung für Ausführungen mit Speicherverbrauch** aus.

   Mit dieser Metrik erhalten Sie die Anzahl der Speichernutzungseinheiten in GB pro Monat, die in Rechnung gestellt werden.

<a name="input-output-sizes"></a>

### <a name="view-action-input-and-output-sizes"></a>Anzeigen der Größen von Aktionseingaben und -ausgaben

1. Suchen Sie im Azure-Portal nach Ihrer Logik-App, und öffnen Sie sie.

1. Wählen Sie im Menü Ihrer Logik-App die Option **Übersicht** aus.

1. Wählen Sie im rechten Bereich unter **Ausführungsverlauf** die Ausführung aus, zu der die Eingaben und Ausgaben gehören, Sie überprüfen möchten.

1. Wählen Sie unter **Logik-App-Ausführung** die Option **Ausführungsdetails** aus.

1. Wählen Sie im Bereich **Details zur Logik-App-Ausführung** in der Aktionentabelle, in der der Status und die Dauer jeder Aktion aufgeführt sind, die Aktion aus, die Sie anzeigen möchten.

1. Suchen Sie im Bereich **Logik-App-Aktion** nach den Größen für die Eingaben und Ausgaben dieser Aktion. Diese Werte werden unter **Eingabelink** bzw. **Ausgabelink** angezeigt.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu [Azure Logic Apps](logic-apps-overview.md)
* [Erstellen Ihrer ersten Logik-App](quickstart-create-first-logic-app-workflow.md)

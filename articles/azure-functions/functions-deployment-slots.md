---
title: Azure Functions-Bereitstellungsslots
description: Erfahren Sie, wie Sie Bereitstellungsslots mit Azure Functions erstellen und verwenden.
services: functions
documentationcenter: na
author: craigshoemaker
manager: gwallace
keywords: Azure Functions, Funktionen
ms.service: azure-functions
ms.topic: reference
ms.date: 08/12/2019
ms.author: cshoe
ms.openlocfilehash: 50337745b008cdd38dd860a0329e44ee712e7acd
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "70085665"
---
# <a name="azure-functions-deployment-slots"></a>Azure Functions-Bereitstellungsslots

Azure Functions-Bereitstellungsslots ermöglichen Ihrer Funktions-App das Ausführen verschiedener Instanzen, die als „Slots“ bezeichnet werden. Slots sind verschiedene Umgebungen, die über einen öffentlich verfügbaren Endpunkt verfügbar gemacht werden. Eine App-Instanz wird immer dem Produktionsslot zugeordnet, und Sie können Instanzen, die einem Slot zugewiesen sind, bei Bedarf austauschen. Funktions-Apps, die unter dem App Service-Plan ausgeführt werden, haben möglicherweise mehrere Slots, während unter dem Verbrauch-Plan nur ein Slot zulässig ist.

Im Folgenden wird dargestellt, wie sich der Austausch von Slots auf Funktionen auswirkt:

- Die Umleitung des Datenverkehrs erfolgt nahtlos, und es gehen keine Anforderungen aufgrund des Tauschs verloren.
- Wenn eine Funktion während eines Austauschs ausgeführt wird, wird die Ausführung fortgesetzt, und nachfolgende Trigger werden an die getauscht App-Instanz weitergeleitet.

> [!NOTE]
> Slots sind für den Linux-Verbrauch-Plan nicht verfügbar.

## <a name="why-use-slots"></a>Gründe für die Verwendung von Slots

Es gibt eine Reihe von Vorteilen durch die Verwendung von Bereitstellungsslots. In den folgenden Szenarien werden gängige Verwendungsmöglichkeiten für Slots beschrieben:

- **Verschiedene Umgebungen für unterschiedliche Zwecke**: Die Verwendung verschiedener Slots bietet Ihnen die Möglichkeit, App-Instanzen vor dem Austauschen in den Produktions- oder einen Stagingslot zu unterscheiden.
- **Vorwärmen**: Wenn Sie in einem Slot anstatt direkt in der Produktionsumgebung bereitstellen, können Sie die App vor dem Livebetrieb vorwärmen. Außerdem reduziert die Verwendung von Slots die Latenzzeit für HTTP-ausgelöste Workloads. Instanzen werden vor der Bereitstellung vorgewärmt, wodurch sich Kaltstart bei neu bereitgestellten Funktionen verkürzt.
- **Einfache Fallbacks**: Nach einem Austausch mit der Produktion enthält der Slot mit der vorherigen Staging-App die vorherige Produktions-App. Wenn die in den Produktionsslot überführten Änderungen nicht Ihren Erwartungen entsprechen, können Sie den Austausch sofort wieder umkehren, um Ihre „letzte als funktionierend bekannte Instanz“ wiederherzustellen.

## <a name="swap-operations"></a>Austauschvorgänge

Während eines Austauschs wird ein Slot als Quelle und der andere als Ziel betrachtet. Der Quellslot enthält die Instanz der Anwendung, die auf den Zielslot angewendet wird. Die folgenden Schritte stellen sicher, dass es beim Zielslot während eines Austauschs zu keinen Ausfallzeiten kommt:

1. **Einstellungen anwenden:** Einstellungen aus dem Zielslot werden auf alle Instanzen des Quellslot angewendet. Beispielsweise werden die Produktionseinstellungen auf die Staginginstanz angewendet. Die angewendeten Einstellungen umfassen die folgenden Kategorien:
    - [Slotspezifische](#manage-settings) App-Einstellungen und Verbindungszeichenfolgen (sofern zutreffend)
    - Einstellungen für [Continuous Deployment](../app-service/deploy-continuous-deployment.md) (sofern aktiviert)
    - Einstellungen für die [App Service-Authentifizierung](../app-service/overview-authentication-authorization.md) (sofern aktiviert)

1. **Warten auf Neustarts und Verfügbarkeit:** Der Austausch wartet, bis jede Instanz im Quellslot ihren Neustart abgeschlossen hat und für Anforderungen verfügbar ist. Sollte eine Instanz nicht erfolgreich neu gestartet werden, werden sämtliche Änderungen am Quellslot zurückgesetzt, und der Austauschvorgang wird beendet.

1. **Routing aktualisieren:** Nach erfolgreicher Vorwärmung aller Instanzen im Quellslot schließen die beiden Slots den Austausch ab, indem die Routingregeln ausgetauscht werden. Nach diesem Schritt befindet sich die App, die zuvor im Quellslot vorbereitet wurde, im Zielslot (also beispielsweise im Produktionsslot).

1. **Vorgang wiederholen:** Nachdem der Quellslot nun die App vor dem Austausch enthält, die sich zuvor im Zielslot befand, führt App Service den gleichen Vorgang erneut aus, indem alle Einstellungen angewendet und die Instanzen für den Quellslot neu gestartet werden.

Berücksichtigen Sie dabei Folgendes:

- In jeder Phase des Austauschvorgangs erfolgt die Initialisierung der ausgetauschten Apps im Quellslot. Der Zielslot bleibt während der gesamten Vorbereitung des Quellslots online – unabhängig davon, ob der Austausch erfolgreich ist oder nicht.

- Wenn Sie einen Stagingslot gegen den Produktionsslot austauschen möchten, muss der Produktionsslot *immer* der Zielslot sein. So ist sichergestellt, dass Ihre Produktions-App durch den Austauschvorgang nicht beeinträchtigt wird.

- Einstellungen im Zusammenhang mit Ereignisquellen und -bindungen müssen als [Bereitstellungssloteinstellungen](#manage-settings) konfiguriert werden, *bevor Sie einen Austausch initiieren*. Wenn Sie vorzeitig als „sticky“ (persistent) markiert werden, wird sichergestellt, dass Ereignisse und Ausgaben an die richtige Instanz weitergeleitet werden.

## <a name="manage-settings"></a>Verwalten von Einstellungen

[!INCLUDE [app-service-deployment-slots-settings](../../includes/app-service-deployment-slots-settings.md)]

### <a name="create-a-deployment-setting"></a>Erstellen einer Bereitstellungseinstellung

Sie können Einstellungen als Bereitstellungseinstellung markieren, wodurch Sie „sticky“ werden. Eine persistente (sticky) Einstellung wird nicht zusammen mit der App-Instanz ausgetauscht.

Wenn Sie eine Bereitstellungseinstellung in einem Slot erstellen, achten Sie darauf, dass Sie dieselbe Einstellung mit einem eindeutigen Wert in allen anderen Slots erstellen, die an einem Austausch beteiligt sind. Auf diese Weise bleiben die Einstellungsnamen zwischen Slots konsistent, während sich der Wert einer Einstellung nicht ändert. Diese Namenskonsistenz stellt sicher, dass Ihr Code nicht versucht, auf eine Einstellung zuzugreifen, die in einem Slot definiert ist, in einem anderen aber nicht.

Verwenden Sie die folgenden Schritte, um eine Bereitstellungseinstellung zu erstellen:

- Navigieren Sie in der Funktions-App zu *Slots*.
- Klicken Sie auf den Slotnamen.
- Klicken Sie unter *Plattformfeatures > Allgemeine Einstellungen* auf **Konfiguration**.
- Klicken Sie auf den Namen der Einstellung, die für den aktuellen Slot persistent sein soll.
- Klicken Sie auf das Kontrollkästchens **Bereitstellungssloteinstellung**.
- Klicken Sie auf **OK**
- Sobald das Blatt mit den Einstellungen angezeigt wird, klicken Sie auf **Speichern**, um die Änderungen beizubehalten.

![Bereitstellungssloteinstellung](./media/functions-deployment-slots/azure-functions-deployment-slots-deployment-setting.png)

## <a name="deployment"></a>Bereitstellung

Slots sind leer, wenn Sie einen Slot erstellen. Sie können jede der [unterstützten Bereitstellungstechnologien](./functions-deployment-technologies.md) verwenden, um Ihre Anwendung in einem Slot bereitzustellen.

## <a name="scaling"></a>Skalieren

Alle Slots werden auf dieselbe Anzahl von Workern wie der Produktionsslot skaliert.

- Bei Verbrauch-Plänen wird der Slot analog der App skaliert.
- Bei App Service-Plänen wird die App auf eine feste Anzahl von Workern skaliert. Slots werden auf derselben Anzahl von Workern wie der App-Plan ausgeführt.

## <a name="add-a-slot"></a>Hinzufügen eines Slots

Sie können einen Slot über die [CLI](https://docs.microsoft.com/cli/azure/functionapp/deployment/slot?view=azure-cli-latest#az-functionapp-deployment-slot-create) oder über das Portal hinzufügen. In den folgenden Schritten wird veranschaulicht, wie Sie einen neuen Slot im Portal erstellen:

1. Navigieren Sie zu ihrer Funktions-App, und klicken Sie auf das **Pluszeichen** neben *Slots*.

    ![Hinzufügen eines Azure Functions-Bereitstellungsslots](./media/functions-deployment-slots/azure-functions-deployment-slots-add.png)

1. Geben Sie einen Namen in das Textfeld ein, und drücken Sie auf die Schaltfläche **Erstellen**.

    ![Benennen eines Azure Functions-Bereitstellungsslots](./media/functions-deployment-slots/azure-functions-deployment-slots-add-name.png)

## <a name="swap-slots"></a>Austauschen von Slots

Sie können Slot über die [CLI](https://docs.microsoft.com/cli/azure/functionapp/deployment/slot?view=azure-cli-latest#az-functionapp-deployment-slot-swap) oder über das Portal austauschen. In den folgenden Schritten wird veranschaulicht, wie Sie Slots im Portal austauschen:

1. Navigieren zur Funktions-App
1. Klicken Sie auf den Namen des Quellslots, den Sie austauschen möchten.
1. Klicken Sie auf der Registerkarte *Übersicht* auf die Schaltfläche **Austauschen** ![Azure Functions-Bereitstellungsslot austauschen](./media/functions-deployment-slots/azure-functions-deployment-slots-swap.png).
1. Überprüfen Sie die Konfigurationseinstellungen für Ihren Austausch, und klicken Sie auf **Austauschen** ![Azure Functions-Bereitstellungsslot austauschen](./media/functions-deployment-slots/azure-functions-deployment-slots-swap-config.png).

Der Vorgang kann einen Augenblick dauern, während der Austauschvorgang ausgeführt wird.

## <a name="roll-back-a-swap"></a>Ausführen eines Rollbacks für einen Austausch

Wenn ein Austausch zu einem Fehler führt oder Sie einen Austausch einfach „rückgängig“ machen möchten, können Sie ein Rollback auf den ursprünglichen Zustand ausführen. Um zum Zustand vor dem Austausch zurückzukehren, führen Sie einen weiteren Austausch aus, um den Austausch umzukehren.

## <a name="remove-a-slot"></a>Entfernen eines Slots

Sie können einen Slot über die [CLI](https://docs.microsoft.com/cli/azure/functionapp/deployment/slot?view=azure-cli-latest#az-functionapp-deployment-slot-delete) oder über das Portal entfernen. In den folgenden Schritten wird veranschaulicht, wie Sie einen Slot im Portal entfernen:

1. Navigieren zur Übersicht der Funktions-App

1. Klicken Sie auf die Schaltfläche **Löschen**.

    ![Hinzufügen eines Azure Functions-Bereitstellungsslots](./media/functions-deployment-slots/azure-functions-deployment-slots-delete.png)

## <a name="automate-slot-management"></a>Automatisieren der Slotverwaltung

Mithilfe der [Azure CLI](https://docs.microsoft.com/cli/azure/functionapp/deployment/slot?view=azure-cli-latest) können Sie die folgenden Aktionen für einen Slot automatisieren:

- [erstellen](https://docs.microsoft.com/cli/azure/functionapp/deployment/slot?view=azure-cli-latest#az-functionapp-deployment-slot-create)
- [delete](https://docs.microsoft.com/cli/azure/functionapp/deployment/slot?view=azure-cli-latest#az-functionapp-deployment-slot-delete)
- [list](https://docs.microsoft.com/cli/azure/functionapp/deployment/slot?view=azure-cli-latest#az-functionapp-deployment-slot-list)
- [swap](https://docs.microsoft.com/cli/azure/functionapp/deployment/slot?view=azure-cli-latest#az-functionapp-deployment-slot-swap)
- [auto-swap](https://docs.microsoft.com/cli/azure/functionapp/deployment/slot?view=azure-cli-latest#az-functionapp-deployment-slot-auto-swap)

## <a name="change-app-service-plan"></a>Ändern des App Service-Plans

Mit einer Funktions-APP, die unter einem App Service-Plan ausgeführt wird, haben Sie die Möglichkeit, den zugrunde liegenden App Service-Plan für einen Slot zu ändern.

> [!NOTE]
> Unter einem Verbrauch-Plan können Sie den App Service-Plan eines Slots nicht ändern.

Verwenden Sie die folgenden Schritte, um den App Service-Plan eines Slots zu ändern:

1. Navigieren zu einem Slot

1. Klicken Sie unter *Plattformfeatures* auf **Alle Einstellungen**.

    ![Ändern des App Service-Plans](./media/functions-deployment-slots/azure-functions-deployment-slots-change-app-service-settings.png)

1. Klicken Sie auf **App Service-Plan**.

1. Auswählen eines neuen App Service-Plans oder Erstellen einen neuen Plans

1. Klicken Sie auf **OK**

    ![Ändern des App Service-Plans](./media/functions-deployment-slots/azure-functions-deployment-slots-change-app-service-select.png)


## <a name="limitations"></a>Einschränkungen

Azure Functions-Bereitstellungsslots unterliegen folgenden Einschränkungen:

- Die Anzahl der für einen App verfügbaren Slots hängt vom Plan ab. Im Verbrauch-Plan ist nur ein Bereitstellungsslot zulässig. Zusätzliche Slots sind für Apps verfügbar, die unter dem App Service-Plan ausgeführt werden.
- Durch den Austausch eines Slots werden Schlüssel für Apps zurückgesetzt, die über eine App-Einstellung für `AzureWebJobsSecretStorageType` von `files` verfügen.
- Slots sind für den Linux-Verbrauch-Plan nicht verfügbar.

## <a name="support-levels"></a>Supportstufen

Es gibt zwei Ebenen der Unterstützung für Bereitstellungsslots:

- **Allgemeine Verfügbarkeit (General Availability, GA)** : Vollständige Unterstützung und Freigabe für die Verwendung in Produktionsumgebungen.
- **Vorschau**: Noch nicht unterstützt, die Erreichung der allgemeinen Verfügbarkeit wird aber für die Zukunft erwartet.

| Betriebssystem/Hostingplan           | Unterstützungsebene     |
| ------------------------- | -------------------- |
| Windows: Verbrauch       | Allgemeine Verfügbarkeit |
| Windows Premium (Vorschau) | Vorschau              |
| Windows: Dediziert         | Allgemeine Verfügbarkeit |
| Linux: Verbrauch         | Nicht unterstützt          |
| Linux: Premium (Vorschau)   | Vorschau              |
| Linux: Dediziert           | Allgemeine Verfügbarkeit |

## <a name="next-steps"></a>Nächste Schritte

- [Bereitstellungstechnologien in Azure Functions](./functions-deployment-technologies.md)

---
title: Konfigurieren der Personalisierung
titleSuffix: Azure Cognitive Services
description: Die Dienstkonfiguration umfasst die Art, wie der Dienst Belohnungen behandelt, wie oft der Dienst Untersuchungen durchführt, wie oft das Modell neu trainiert wird und wie viele Daten gespeichert werden.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: diberry
ms.openlocfilehash: 4ab1dcf4f3554c941107ec653f717b3680543da2
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73490724"
---
# <a name="configure-personalizer"></a>Konfigurieren der Personalisierung

Die Dienstkonfiguration umfasst die Art, wie der Dienst Belohnungen behandelt, wie oft der Dienst Untersuchungen durchführt, wie oft das Modell neu trainiert wird und wie viele Daten gespeichert werden.

## <a name="create-personalizer-resource"></a>Erstellen einer Personalisierungsressource

Erstellen Sie eine Personalisierungsressource für jede Feedbackschleife. 

1. Melden Sie sich beim [Azure-Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer)an. Über den vorherigen Link gelangen Sie zur Seite **Erstellen** für den Personalisierungsdienst. 
1. Geben Sie Ihren Dienstnamen ein, und wählen Sie ein Abonnement, einen Standort, einen Tarif und eine Ressourcengruppe aus.
1. Wählen Sie die Bestätigung und anschließend **Erstellen** aus.

<a name="configure-service-settings-in-the-azure-portal"></a>

## <a name="configure-service-in-the-azure-portal"></a>Konfigurieren von Diensten im Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer) an.
1. Suchen Sie Ihre Personalisierungsressource. 
1. Wählen Sie im Abschnitt **Ressourcenverwaltung** die Option **Konfiguration** aus.

    Kopieren Sie auf der Seite **Schlüssel** einen Ressourcenschlüssel, bevor Sie das Azure-Portal verlassen. Sie benötigen ihn für die Verwendung des [SDK für die Personalisierung](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer).

<a name="configure-reward-settings-for-the-feedback-loop-based-on-use-case"></a>

### <a name="configure-reward-for-the-feedback-loop-based-on-use-case"></a>Konfigurieren der Belohnung für die Feedbackschleife auf der Grundlage von Anwendungsfällen

Konfigurieren Sie den Dienst für die Verwendung von Belohnungen durch die Feedbackschleife. Änderungen an den folgenden Werten setzen das aktuelle Personalisierungsmodell zurück und trainieren es mit den Daten der letzten 2 Tage neu:

![Konfigurieren der Belohnungswerte für die Feedbackschleife](media/settings/configure-model-reward-settings.png)

|Wert|Zweck|
|--|--|
|Belohnungswartezeit|Legt die Zeitspanne, während der die Personalisierung Werte für einen Rangfolgeaufruf sammelt werden, ab dem Zeitpunkt fest, zu dem der Rangfolgeaufruf erfolgt. Dieser Wert wird festgelegt durch die Frage: „How long should Personalizer wait for rewards calls?“ (Wie lange sollte die Personalisierung auf Belohnungsaufrufe warten?) Jede Belohnung, die nach diesem Zeitfenster eingeht, wird protokolliert, aber nicht für das Lernen verwendet.|
|Standardbelohnung|Wenn während des Zeitfensters für das Warten auf die Belohnung kein Belohnungsaufruf bei der Personalisierung eingeht, weist die Personalisierung die Standardbelohnung zu. Standardmäßig und in den meisten Szenarien ist die Standardbelohnung 0 (null).|
|Belohnungsaggregation|Wenn mehrere Belohnungen für den gleichen Rangfolge-API-Aufruf empfangen werden, wird diese Aggregationsmethode verwendet: **sum** oder **earliest**. „Earliest“ wählt das am frühesten empfangene Ergebnis aus, und der Rest wird verworfen. Dies ist nützlich, wenn Sie eine eindeutige Belohnung zwischen möglicherweise doppelten Aufrufen wünschen. |

Wählen Sie nach dem Ändern dieser Werte unbedingt **Speichern** aus.

### <a name="configure-exploration"></a>Konfigurieren der Erkundung 

Die Personalisierung kann neue Muster ermitteln und im Laufe der Zeit den Verhaltensänderungen der Benutzer durch Durchsuchen von Alternativen anpassen. Der Wert **Erkundung** bestimmt, wie viel Prozent der Rangfolgeaufrufe mit einem Durchsuchen beantwortet werden. 

Änderungen an diesem Wert setzen das aktuelle Personalisierungsmodell zurück und trainieren es mit den Daten der letzten 2 Tage neu.

![Der Wert „Erkundung“ bestimmt, wie viel Prozent der Rangfolgeaufrufe mit einem Durchsuchen beantwortet werden.](media/settings/configure-exploration-setting.png)

Wählen Sie nach dem Ändern dieses Werts unbedingt **Speichern** aus.

### <a name="model-update-frequency"></a>Häufigkeit der Modellaktualisierung

Vom Rangfolgeaufruf der Personalisierung wird nicht automatisch das neueste Modell verwendet, das auf der Grundlage der Relevanz-API-Aufrufe aller aktiven Ereignisse trainiert wurde. Die **Häufigkeit der Modellaktualisierung** legt fest, wie oft das vom Rangfolgeaufruf verwendete Modell aktualisiert wird. 

Eine hohe Modellaktualisierungshäufigkeit ist hilfreich, wenn Veränderungen beim Benutzerverhalten genau nachverfolgt werden sollen. Beispiele wären etwa Websites mit Livenachrichten, viralen Inhalten oder Livegeboten für Produkte. In solchen Szenarien kann beispielsweise eine Frequenz von 15 Minuten verwendet werden. In den meisten Anwendungsfällen ist eine niedrigere Aktualisierungshäufigkeit sinnvoll. Eine Modellaktualisierungshäufigkeit von einer Minute empfiehlt sich beim Debuggen von Anwendungscode mit Personalisierung, bei Demos sowie beim interaktiven Testen von Machine Learning-Aspekten.

![„Model update frequency“ legt fest, wie oft ein neues Personalisierungsmodell neu trainiert wird.](media/settings/configure-model-update-frequency-settings-15-minutes.png)

Wählen Sie nach dem Ändern dieses Werts unbedingt **Speichern** aus.

### <a name="data-retention"></a>Beibehaltung von Daten

**Data retention period** (Zeitraum für die Datenaufbewahrung) legt fest, wie viele Tage die Personalisierung Datenprotokolle aufbewahrt. Frühere Datenprotokolle sind zur Durchführung von [Offlineauswertungen](concepts-offline-evaluation.md) erforderlich, die verwendet werden, um die Effektivität der Personalisierung zu messen und die Lernrichtlinie zu optimieren.

Wählen Sie nach dem Ändern dieses Werts unbedingt **Speichern** aus.

## <a name="export-the-personalizer-model"></a>Exportieren des Personalisierungsmodells

Überprüfen Sie im Abschnitt **Modell- und Lerneinstellungen** der Ressourcenverwaltung die Modellerstellung und das Datum der letzten Aktualisierung, und exportieren Sie das aktuelle Modell. Sie können das Azure-Portal oder die Personalisierungs-APIs verwenden, um eine Modelldatei zu Archivierungszwecken zu exportieren. 

![Exportieren des aktuellen Personalisierungsmodells](media/settings/export-current-personalizer-model.png)

## <a name="import-and-export-learning-policy"></a>Importieren und Exportieren der Lernrichtlinie

Importieren Sie im Abschnitt für **Modell- und Lerneinstellungen** der Ressourcenverwaltung eine neue Lernrichtlinie, oder exportieren Sie die aktuelle Lernrichtlinie.
Sie können Lernrichtliniendateien aus vorherigen Exporten abrufen oder die optimierten Richtlinien herunterladen, die während Offlineauswertungen erkannt wurden. Manuelle Änderungen an diesen Dateien wirken sich auf die Machine Learning-Leistung und die Genauigkeit von Offlineauswertungen aus, und Microsoft kann sich nicht für die Genauigkeit von Machine Learning und Auswertungen oder Dienstausnahmen, die sich aus manuell bearbeiteten Richtlinien ergeben, verbürgen.

## <a name="clear-data-for-your-learning-loop"></a>Löschen von Daten für Ihre Lernschleife

1. Wählen Sie im Azure-Portal für Ihre Personalisierungsressource auf der Seite **Modell- und Lerneinstellungen** die Option **Daten löschen** aus.
1. Aktivieren Sie alle drei Kontrollkästchen, um alle Daten zu löschen und die Lernschleife in den ursprünglichen Zustand zurückzusetzen.

    ![Löschen Sie im Azure-Portal Daten aus der Personalisierungsressource.](./media/settings/clear-data-from-personalizer-resource.png)

    |Wert|Zweck|
    |--|--|
    |Protokollierte Personalisierungs- und Relevanzdaten.|Diese Protokolldaten werden in Offline-Auswertungen verwendet. Löschen Sie die Daten, wenn Sie Ihre Ressource zurücksetzen.|
    |Setzen Sie das Personalisierungsmodell zurück.|Dieses Modell ändert sich bei jedem erneuten Training. Diese Trainingshäufigkeit wird in der **Uploadfrequenz für das Modell** auf der Seite **Konfiguration** angegeben. |
    |Legen Sie die Lernrichtlinie auf die Standardeinstellung fest.|Wenn Sie die Lernrichtlinie im Rahmen einer Offline-Auswertung geändert haben, wird diese auf die ursprüngliche Lernrichtlinie zurückgesetzt.|

1. Wählen Sie **Ausgewählte Daten löschen** aus, um den Löschvorgang zu starten. Der Status wird in den Azure-Benachrichtigungen in der oberen rechten Navigationsleiste angezeigt. 

## <a name="next-steps"></a>Nächste Schritte


[Erfahren Sie mehr zur regionalen Verfügbarkeit.](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)

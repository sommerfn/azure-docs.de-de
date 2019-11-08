---
title: 'Durchführen einer Offlineauswertung: Personalisierung'
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie Ihre Lernschleife mit einer Offlineauswertung analysieren können.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: diberry
ms.openlocfilehash: 9d7336fb933ee5e9781d15214fa3e59446aa5b6a
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73490712"
---
# <a name="analyze-your-learning-loop-with-an-offline-evaluation"></a>Analysieren Ihrer Lernschleife mit einer Offlineauswertung

Erfahren Sie, wie Sie eine Offlineauswertung ausführen und die Ergebnisse verstehen können.

Mit Offlineauswertungen können Sie messen, wie effektiv die Personalisierung verglichen mit dem Standardverhalten Ihrer Anwendung ist, erfahren, welche Features am meisten zur Personalisierung beitragen, und automatisch neue Werte für das Machine Learning ermitteln.

Weitere Informationen finden Sie unter [Offlineauswertung](concepts-offline-evaluation.md).


## <a name="prerequisites"></a>Voraussetzungen

* Eine konfigurierte Personalisierungsschleife
* Die Personalisierungsschleife muss eine repräsentative Menge an Daten aufweisen. Für sinnvolle Auswertungsergebnisse werden mindestens 50.000 Ereignisse in den Protokollen empfohlen. Optional haben Sie möglicherweise auch zuvor _Lernrichtliniendateien_ exportiert, die Sie in der gleichen Auswertung vergleichen und testen können.

## <a name="steps-to-start-a-new-offline-evaluation"></a>Schritte zum Starten einer neuen Offlineauswertung

1. Suchen Sie Ihre Personalisierungsressource im [Azure-Portal](https://azure.microsoft.com/free/).
1. Navigieren Sie im Azure-Portal zum Abschnitt **Bewertungen**, und wählen Sie **Bewertung erstellen** aus.
    ![Navigieren Sie im Azure-Portal zum Abschnitt **Bewertungen**, und wählen Sie **Bewertung erstellen** aus.](./media/offline-evaluation/create-new-offline-evaluation.png)
1. Konfigurieren Sie die folgenden Werte:

    * Einen Auswertungsnamen
    * Start- und Enddatum: Hierbei handelt es sich um Datumsangaben in der Vergangenheit, die den Bereich der zur Auswertung zu verwendenden Daten angeben. Diese Daten müssen gemäß dem Wert [Datenaufbewahrung](how-to-settings.md) in den Protokollen vorliegen.
    * „Ermittlung zur Optimierung“ ist auf **Ja** festgelegt.

    ![Auswählen von Offlineauswertungseinstellungen](./media/offline-evaluation/create-an-evaluation-form.png)

1. Wählen Sie **OK** aus, um die Auswertung zu starten. 

## <a name="results"></a>Ergebnisse

Auswertungen können je nach der Menge zu verarbeitender Daten, der Anzahl der zu vergleichenden Lernrichtlinien und abhängig davon, ob eine Optimierung angefordert wurde, sehr lange dauern.

Nachdem Sie den Vorgang abgeschlossen haben, können Sie die Auswertung aus der Liste der Auswertungen auswählen. 

Vergleiche von Lernrichtlinien einschließlich:

* **Online Policy** (Onlinerichtlinie): Die derzeit in der Personalisierung verwendete Lernrichtlinie
* **Baseline**: Der Standard der Anwendung (wie durch die erste in Rangfolgeaufrufen gesendete Aktion bestimmt),
* **Random Policy** (Zufallsrichtlinie): Ein imaginäres Rangfolgeverhalten, das immer ein zufällige Auswahl von Aktionen aus den angegebenen zurückgibt.
* **Custom Policies** (Benutzerdefinierte Richtlinien): Zusätzliche, beim Starten der Auswertung hochgeladene Lernrichtlinien.
* **Optimized Policy** (Optimierte Richtlinie): Wenn die Auswertung mit der Option zum Ermitteln einer optimierten Richtlinie gestartet wurde, wird sie auch verglichen, und Sie können sie herunterladen oder zur Online-Lernrichtlinie machen, wobei die aktuelle ersetzt wird.

![Ergebnisdiagramme von Offlineauswertungseinstellungen](./media/offline-evaluation/evaluation-results.png)

Effektivität der [Features](concepts-features.md) für Aktionen und Kontext.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, [wie Offlineauswertungen funktionieren](concepts-offline-evaluation.md).

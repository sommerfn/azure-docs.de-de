---
title: Manuelles Failover einer Azure IoT Hub-Instanz | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie ein manuelles Failover für Ihren IoT-Hub in eine andere Region ausführen und die Funktionsweise überprüfen und anschließend die Verlagerung zurück in die ursprüngliche Region und eine erneute Überprüfung durchführen.
author: robinsh
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 07/24/2019
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: 42785e3ee636f24ca185f57a11d4ee1091db3e98
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73890408"
---
# <a name="tutorial-perform-manual-failover-for-an-iot-hub"></a>Tutorial: Ausführen eines manuellen Failovers für eine IoT Hub-Instanz

Das manuelle Failover ist ein Feature des IoT Hub-Diensts, der Kunden die Ausführung eines [Failovers](https://en.wikipedia.org/wiki/Failover) für die Hubvorgänge einer primären Region in der entsprechenden geografisch gekoppelten Azure-Region ermöglicht. Das manuelle Failover kann im Falle eines regionalen Notfalls oder eines längeren Dienstausfalls ausgeführt werden. Sie können auch ein geplantes Failover zum Testen der Notfallwiederherstellungsfunktionen ausführen. Es wird jedoch empfohlen, einen IoT-Testhub anstelle einer in der Produktion ausgeführten Instanz zu verwenden. Das Feature für das manuelle Failover steht Kunden ohne zusätzliche Kosten zur Verfügung.

In diesem Tutorial führen Sie die folgenden Aufgaben aus:

> [!div class="checklist"]
> * Erstellen eines IoT-Hubs über das Azure-Portal 
> * Ausführen eines Failovers. 
> * Anzeigen der Hubausführung am sekundären Standort
> * Ausführen eines Failbacks, um die Vorgänge des IoT-Hubs am primären Standort wiederherzustellen 
> * Überprüfen, ob der Hub ordnungsgemäß am richtigen Standort ausgeführt wird

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 

2. Klicken Sie auf **+ Ressource erstellen**, und wählen Sie dann **Internet der Dinge** und anschließend **IoT Hub**.

   ![Screenshot, auf dem das Erstellen eines IoT-Hubs dargestellt ist](./media/tutorial-manual-failover/create-hub-01.png)

3. Klicken Sie auf die Registerkarte **Grundlagen**. Füllen Sie die folgenden Feldern aus:

    **Abonnement**: Wählen Sie das zu verwendende Azure-Abonnement aus.

    **Ressourcengruppe**: Klicken Sie auf **Neu erstellen**, und geben Sie als Ressourcengruppenname **ManlFailRG** an.

    **Region**: Wählen sie eine Region in Ihrer Nähe aus. In diesem Tutorial wird `West US 2` verwendet. Ein Failover kann nur zwischen geografisch gekoppelten Azure-Regionen ausgeführt werden. Die mit „USA, Westen 2“ geografisch gekoppelte Region ist „WestCentralUS“.
    
   **IoT Hub-Name**: Geben Sie einen Namen für Ihren IoT-Hub an. Der Hubname muss global eindeutig sein. 

   ![Screenshot, auf dem das Blatt „Grundlagen“ zum Erstellen eines IoT-Hubs dargestellt ist](./media/tutorial-manual-failover/create-hub-02-basics.png)

   Klicken Sie auf **Überprüfen + erstellen**. (Für Größe und Skalierung werden die Standardwerte verwendet.) 

4. Überprüfen Sie die Informationen, und klicken Sie dann auf **Erstellen**, um den IoT-Hub zu erstellen. 

   ![Screenshot, auf dem der letzte Schritt beim Erstellen eines IoT-Hubs dargestellt ist](./media/tutorial-manual-failover/create-hub-03-create.png)

## <a name="perform-a-manual-failover"></a>Ausführen eines manuellen Failovers

Beachten Sie, dass für einen IoT-Hub pro Tag maximal zwei Failover und zwei Failbacks ausgeführt werden können.

1. Klicken Sie auf **Ressourcengruppen**, und wählen Sie dann die Ressourcengruppe **ManlFailRG** aus. Klicken Sie in der Liste der Ressourcen auf Ihren Hub. 

1. Klicken Sie im Bereich „IoT Hub“ unter **Einstellungen** auf **Failover**.

   ![Screenshot mit dem Bereich „Eigenschaften“ der IoT Hub-Instanz](./media/tutorial-manual-failover/trigger-failover-01.png)

1. Im Bereich „Manuelles Failover“ werden die Optionen **Aktueller Standort** und **Failover location** (Failoverstandort) angezeigt. Der aktuelle Standort gibt immer den Standort an, an dem der Hub derzeit aktiv ist. Der Failoverstandort ist die standardmäßige [geografisch gekoppelte Azure-Region](../best-practices-availability-paired-regions.md), die mit dem aktuellen Standort gekoppelt ist. Die Standortwerte können nicht geändert werden. In diesem Tutorial ist der aktuelle Standort `West US 2` und der Failoverstandort `West Central US`.

   ![Screenshot mit dem Bereich „Manuelles Failover“](./media/tutorial-manual-failover/trigger-failover-02.png)

1. Klicken Sie oben im Bereich „Manuelles Failover“ auf **Failover starten**. 

1. Geben Sie im Bereich für die Bestätigung den Namen Ihres IoT-Hubs ein, um zu bestätigen, dass für ihn das Failover ausgeführt werden soll. Klicken Sie anschließend zum Initiieren des Failovers auf **Failover**.

   Die Dauer für die Ausführung des manuellen Failovers ist proportional zur Anzahl von Geräten, die für Ihren Hub registriert sind. Wenn Sie beispielsweise 100.000 Geräte besitzen, dauert die Ausführung vielleicht 15 Minuten. Bei fünf Millionen Geräten kann sie jedoch eine Stunde oder länger dauern.

   ![Screenshot mit dem Bereich „Manuelles Failover“](./media/tutorial-manual-failover/trigger-failover-03-confirm.png)

   Während des manuellen Failovers wird ein Banner mit dem Hinweis angezeigt, dass ein manuelles Failover ausgeführt wird. 

   ![Screenshot mit dem Hinweis, dass gerade ein manuelles Failover ausgeführt wird](./media/tutorial-manual-failover/trigger-failover-04-in-progress.png)

   Wenn Sie den IoT Hub-Bereich schließen und wieder öffnen, indem Sie im Bereich „Ressourcengruppe“ auf den Hub klicken, wird ein Banner mit dem Hinweis angezeigt, dass für den Hub gerade ein manuelles Failover ausgeführt wird. 

   ![Screenshot mit dem Hinweis, dass gerade ein IoT Hub-Failover ausgeführt wird](./media/tutorial-manual-failover/trigger-failover-05-hub-inactive.png)

   Nach Abschluss der Ausführung werden die aktuelle Region und die Failoverregion auf der Seite „Manuelles Failover“ getauscht, und der Hub ist wieder aktiv. In diesem Beispiel ist jetzt der aktuelle Standort `WestCentralUS` und der Failoverstandort `West US 2`. 

   ![Screenshot mit abgeschlossenem Failover](./media/tutorial-manual-failover/trigger-failover-06-finished.png)

   Auf der Übersichtsseite wird ebenfalls ein Banner mit dem Hinweis angezeigt, dass das Failover abgeschlossen ist und die IoT Hub-Instanz in `West Central US` ausgeführt wird.

   ![Screenshot mit abgeschlossenem Failover auf der Übersichtsseite](./media/tutorial-manual-failover/trigger-failover-06-finished-overview.png)


## <a name="perform-a-failback"></a>Ein Failback durchführen 

Nach der Ausführung eines manuellen Failovers können Sie die Vorgänge des Hubs wieder in der ursprünglichen primären Region auszuführen: Dieser Schritt wird Failback genannt. Falls Sie soeben ein Failover ausgeführt haben, müssen Sie bis zur Anforderung eines Failbacks eine Stunde warten. Wenn Sie vorher versuchen, das Failback auszuführen, wird eine Fehlermeldung angezeigt.

Ein Failback wird genau wie ein manuelles Failover ausgeführt. Hier sind die Schritte aufgeführt: 

1. Kehren Sie zum Ausführen eines Failbacks zum Iot Hub-Bereich für Ihren IoT-Hub zurück.

2. Klicken Sie im Bereich „IoT Hub“ unter **Einstellungen** auf **Failover**. 

3. Klicken Sie oben im Bereich „Manuelles Failover“ auf **Failover starten**. 

4. Geben Sie im Bereich für die Bestätigung den Namen Ihres IoT-Hubs ein, um zu bestätigen, dass für ihn das Failback ausgeführt werden soll. Klicken Sie dann auf „OK“, um das Failback zu initiieren. 

   ![Screenshot der Anforderung für ein manuelles Failback](./media/tutorial-manual-failover/trigger-failover-03-confirm.png)

   Die Banner werden wie im Abschnitt „Ausführen eines Failovers“ angezeigt. Nach Abschluss des Failbacks wird wie ursprünglich festgelegt erneut `West US 2` als aktueller Standort und `West Central US` als Failoverstandort angezeigt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen 

Wenn Sie die für dieses Tutorial erstellten Ressourcen entfernen möchten, löschen Sie die Ressourcengruppe. Diese Aktion löscht alle in der Gruppe enthaltenen Ressourcen. In diesem Fall werden der IoT-Hub und die Ressourcengruppe selbst entfernt. 

1. Klicken Sie auf **Ressourcengruppen**. 

2. Suchen Sie nach der Ressourcengruppe **ManlFailRG**, und wählen Sie sie aus. Klicken Sie darauf, um es zu öffnen. 

3. Klicken Sie auf **Ressourcengruppe löschen**. Geben Sie bei entsprechender Aufforderung den Namen der Ressourcengruppe ein, und klicken Sie zum Bestätigen auf **Löschen**. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie ein manuelles Failover konfigurieren und ausführen und wie Sie durch Ausführen der folgenden Aufgaben ein Failback anfordern:

> [!div class="checklist"]
> * Erstellen eines IoT-Hubs über das Azure-Portal 
> * Ausführen eines Failovers. 
> * Anzeigen der Hubausführung am sekundären Standort
> * Ausführen eines Failbacks, um die Vorgänge des IoT-Hubs am primären Standort wiederherzustellen 
> * Überprüfen, ob der Hub ordnungsgemäß am richtigen Standort ausgeführt wird

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie den Status eines IoT-Geräts verwalten. 

> [!div class="nextstepaction"]
> [Tutorial: Konfigurieren Ihrer Geräte über einen Back-End-Dienst](tutorial-device-twins.md)

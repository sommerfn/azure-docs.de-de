---
title: Azure-Schnellstart – Ausführen eines Batch-Auftrags – Portal
description: In dieser Schnellstartanleitung wird beschrieben, wie Sie einen Batch-Auftrag mit dem Azure-Portal ausführen.
services: batch
author: laurenhughes
manager: gwallace
ms.service: batch
ms.devlang: na
ms.topic: quickstart
ms.date: 07/03/2018
ms.author: lahugh
ms.custom: mvc
ms.openlocfilehash: 33455a95dff946676e3dfffbd2737ed36be59c64
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2019
ms.locfileid: "68322363"
---
# <a name="quickstart-run-your-first-batch-job-in-the-azure-portal"></a>Schnellstart: Ausführen Ihres ersten Batch-Auftrags im Azure-Portal

In diesem Schnellstart wird veranschaulicht, wie Sie das Azure-Portal zum Erstellen eines Batch-Kontos, eines *Pools* mit Computeknoten (virtuelle Computer) und einen *Auftrag* erstellen, mit dem im Pool einfache *Aufgaben* ausgeführt werden. Nach Abschluss dieser Schnellstartanleitung sind Sie mit den wichtigsten Konzepten des Batch-Diensts vertraut und können Batch mit realistischeren Workloads und in größerem Umfang ausprobieren.

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

## <a name="sign-in-to-azure"></a>Anmelden bei Azure 

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="create-a-batch-account"></a>Erstellen eines Batch-Kontos

Führen Sie diese Schritte aus, um ein Batch-Beispielkonto für Testzwecke zu erstellen. Zum Erstellen von Pools und Aufträgen benötigen Sie ein Batch-Konto. Wie hier gezeigt, können Sie ein Azure-Speicherkonto mit dem Batch-Konto verknüpfen. Auch wenn es für diese Schnellstartanleitung nicht erforderlich ist, ist das Speicherkonto nützlich zum Bereitstellen von Anwendungen und Speichern von Eingabe- und Ausgabedaten für die meisten Workloads in der Praxis.


1. Klicken Sie auf **Ressource erstellen** > **Compute** > **Batch-Dienst**. 

   ![Batch im Marketplace][marketplace_portal]

2. Geben Sie Werte für **Kontoname** und **Ressourcengruppe** ein. Der Kontoname muss für den ausgewählten Azure-**Standort** eindeutig sein, darf nur Kleinbuchstaben oder Zahlen enthalten und muss zwischen 3 und 24 Zeichen lang sein. 

3. Wählen Sie für **Speicherkonto** ein vorhandenes Speicherkonto aus, oder erstellen Sie ein neues.

4. Behalten Sie die Standardwerte für die übrigen Einstellungen bei, und klicken Sie auf **Erstellen**, um das Konto zu erstellen.

   ![Erstellen eines Batch-Kontos][account_portal]  

Greifen Sie im Portal auf das Batch-Konto zu, wenn die Meldung **Bereitstellung erfolgreich** angezeigt wird.

## <a name="create-a-pool-of-compute-nodes"></a>Erstellen eines Pools mit Computeknoten

Nachdem Sie nun über ein Batch-Konto verfügen, können Sie einen Beispielpool mit Windows-Computeknoten für Testzwecke erstellen. Der Pool für dieses kurze Beispiel umfasst zwei Knoten, auf denen ein Windows Server 2012 R2-Image vom Azure Marketplace ausgeführt wird.


1. Klicken Sie im Batch-Konto auf **Pools** > **Hinzufügen**.

2. Geben Sie eine **Pool-ID** mit dem Namen *mypool* ein. 

3. Wählen Sie unter **Betriebssystem** die folgenden Einstellungen. (Sie können auch weitere Optionen ausprobieren.)
  
   |Einstellung  |Wert  |
   |---------|---------|
   |**Imagetyp**|Marketplace (Linux/Windows)|
   |**Herausgeber**     |MicrosoftWindowsServer|
   |**Angebot**     |Windows Server|
   |**sku**     |2012-R2-Datacenter-smalldisk|

   ![Auswählen eines Poolbetriebssystems][pool_os] 

4. Scrollen Sie nach unten, um Werte für die Einstellungen **Knotengröße** und **Skalieren** einzugeben. Die vorgeschlagene Knotengröße bietet für dieses kurze Beispiel eine gute Balance zwischen Leistung und Kosten.
  
   |Einstellung  |Wert  |
   |---------|---------|
   |**Knotentarif**     |Standard_A1|
   |**Ziel für dedizierte Knoten**     |2|

   ![Auswählen einer Poolgröße][pool_size] 

5. Behalten Sie die Standardwerte für die übrigen Einstellungen bei, und klicken Sie auf **OK**, um den Pool zu erstellen.

Batch erstellt den Pool sofort, aber es dauert einige Minuten, bis die Computeknoten zugeordnet und gestartet wurden. Während dieses Zeitraums lautet der **Zuweisungsstatus** des Pools **Größe ändern**. Während für den Pool die Größenänderung durchgeführt wird, können Sie einen Auftrag und Aufgaben erstellen. 

![Pool mit Status „Größe ändern“][pool_resizing]

Nach einigen Minuten ändert sich der Status des Pools **Bereit**, und die Knoten werden gestartet. Klicken Sie auf **Knoten**, um den Status der Knoten zu überprüfen. Wenn der Status eines Knotens **Leerlauf** lautet, ist er zum Ausführen von Aufgaben bereit. 

## <a name="create-a-job"></a>Erstellen eines Auftrags

Da Sie jetzt über einen Pool verfügen, können Sie einen Auftrag erstellen, um ihn darin auszuführen. Ein Batch-Auftrag ist eine logische Gruppe für eine oder mehrere Aufgaben. Ein Auftrag enthält gemeinsame Einstellungen für Aufgaben, z.B. die Priorität und den Pool zum Ausführen von Aufgaben. Der Auftrag enthält ursprünglich keine Aufgaben. 

1. Klicken Sie in der Batch-Kontoansicht auf **Aufträge** > **Hinzufügen**. 

2. Geben Sie eine **Auftrags-ID** mit dem Namen *myjob* ein. Wählen Sie unter **Pool** die Option *mypool*. Behalten Sie die Standardwerte für die übrigen Einstellungen bei, und klicken Sie auf **OK**.

   ![Erstellen eines Auftrags][job_create]

Nachdem der Auftrag erstellt wurde, wird die Seite **Aufgaben** geöffnet.

## <a name="create-tasks"></a>Erstellen von Aufgaben

Erstellen Sie nun Beispielaufgaben für die Ausführung im Auftrag. Normalerweise erstellen Sie mehrere Aufgaben, die von Batch zur Ausführung auf Computeknoten in die Warteschlange eingereiht und verteilt werden. In diesem Beispiel erstellen Sie zwei identische Aufgaben. Für jede Aufgabe wird eine Befehlszeile zum Anzeigen der Batch-Umgebungsvariablen auf einem Computeknoten ausgeführt, und anschließend wird 90 Sekunden gewartet. 

Bei Verwendung von Batch befindet sich die Befehlszeile dort, wo Sie Ihre App bzw. Ihr Skript angeben. In Batch gibt es mehrere Möglichkeiten, Apps und Skripts auf Computeknoten bereitzustellen. 

Gehen Sie wie folgt vor, um die erste Aufgabe zu erstellen:

1. Wählen Sie **Hinzufügen**.

2. Geben Sie *mytask* als **Task-ID** ein. 

3. Geben Sie in der **Befehlszeile** die Zeichenfolge `cmd /c "set AZ_BATCH & timeout /t 90 > NUL"` ein. Behalten Sie die Standardwerte für die übrigen Einstellungen bei, und klicken Sie auf **OK**.

   ![Erstellt eine Aufgabe.][task_create]

Nachdem Sie eine Aufgabe erstellt haben, wird sie von Batch zur Ausführung im Pool in die Warteschlange eingereiht. Wenn ein Knoten für die Ausführung verfügbar ist, wird die Aufgabe ausgeführt.

Kehren Sie zu Schritt 1 zurück, um eine zweite Aufgabe zu erstellen. Geben Sie eine andere **Task-ID** ein, aber verwenden Sie eine identische Befehlszeile. Falls die erste Aufgabe noch ausgeführt wird, startet Batch die zweite Aufgabe auf dem anderen Knoten im Pool.

## <a name="view-task-output"></a>Anzeigen der Aufgabenausgabe

Die obigen Beispiele zu den Aufgaben sind innerhalb weniger Minuten abgeschlossen. Klicken Sie zum Anzeigen der Ausgabe einer abgeschlossenen Aufgabe auf **Dateien auf Knoten**, und wählen Sie anschließend die Datei `stdout.txt` aus. In dieser Datei wird die Standardausgabe der Aufgabe angezeigt. Der Inhalt lautet in etwa wie folgt:

![Anzeigen der Aufgabenausgabe][task_output]

Darin sind die Azure Batch-Umgebungsvariablen zu sehen, die auf dem Knoten festgelegt sind. Beim Erstellen Ihrer eigenen Batch-Aufträge und -Aufgaben können Sie auf diese Umgebungsvariablen in Aufgabenbefehlszeilen sowie in den Apps und Skripts verweisen, die über die Befehlszeilen ausgeführt werden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit den Batch-Tutorials und -Beispielen fortfahren möchten, können Sie das erstellte Batch-Konto und das verknüpfte Speicherkonto aus dieser Schnellstartanleitung verwenden. Für das Batch-Konto selbst fallen keine Gebühren an.

Ihnen werden während der Ausführung der Knoten auch dann Gebühren für den Pool berechnet, wenn keine Aufträge geplant sind. Es ist ratsam, den Pool zu löschen, wenn Sie ihn nicht mehr benötigen. Klicken Sie in der Kontoansicht auf **Pools** und auf den Namen des Pools. Wählen Sie anschließend die Option **Löschen**.  Beim Löschen des Pools werden alle Aufgabenausgaben auf den Knoten gelöscht. 

Löschen Sie die Ressourcengruppe, das Batch-Konto und alle dazugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Wählen Sie hierzu die Ressourcengruppe für das Batch-Konto aus, und klicken Sie auf **Ressourcengruppe löschen**.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie ein Batch-Konto, einen Batch-Pool und einen Batch-Auftrag erstellt. Mit dem Auftrag wurden Beispielaufgaben ausgeführt, und Sie haben die Ausgabe angezeigt, die auf einem der Knoten erstellt wurde. Da Sie sich jetzt mit den wichtigsten Konzepten des Batch-Diensts vertraut gemacht haben, können Sie Batch mit realistischeren Workloads und in größerem Umfang ausprobieren. Weitere Informationen zu Azure Batch finden Sie in den Azure Batch-Tutorials. 

> [!div class="nextstepaction"]
> [Azure Batch-Tutorials](./tutorial-parallel-dotnet.md)

[marketplace_portal]: ./media/quick-create-portal/marketplace-batch.png

[account_portal]: ./media/quick-create-portal/batch-account-portal.png

[account_keys]: ./media/quick-create-portal/batch-account-keys.png

[pool_os]: ./media/quick-create-portal/pool-operating-system.png

[pool_size]: ./media/quick-create-portal/pool-size.png

[pool_resizing]: ./media/quick-create-portal/pool-resizing.png

[job_create]: ./media/quick-create-portal/job-create.png

[task_create]: ./media/quick-create-portal/task-create.png

[task_output]: ./media/quick-create-portal/task-output.png
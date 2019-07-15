---
title: Planen Sie die Aufgaben der Azure Container Registry
description: Setzen Sie Timer, um eine Aufgabe der Azure Container Registry nach einem definierten Zeitplan auszuführen.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 06/27/2019
ms.author: danlep
ms.openlocfilehash: a1123a30025f9be6e994e69703f5ee1aa05d1b49
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509666"
---
# <a name="run-an-acr-task-on-a-defined-schedule"></a>Ausführen einer ACR-Aufgabe nach einem definierten Zeitplan

Dieser Artikel zeigt Ihnen, wie Sie eine [ACR-Aufgabe](container-registry-tasks-overview.md) nach einem Zeitplan ausführen. Planen Sie eine Aufgabe, indem Sie einen oder mehrere *Zeitgebertrigger* einrichten. 

Das Einplanen einer Aufgabe ist für Szenarien wie die folgenden nützlich:

* Führen Sie einen Container-Workload für geplante Wartungsarbeiten aus. Führen Sie beispielsweise eine containerisierte Anwendung aus, um nicht benötigte Bilder aus Ihrer Registrierung zu entfernen.
* Führen Sie während des Arbeitstages im Rahmen Ihrer Live-Überwachung eine Reihe von Tests an einem Produktionsbild durch.

Sie können die Azure Cloud Shell oder eine lokale Installation der Azure CLI verwenden, um die Beispiele in diesem Artikel auszuführen. Wenn Sie es lokal verwenden möchten, ist die Version 2.0.68 oder höher erforderlich. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI][azure-cli-install].


## <a name="about-scheduling-a-task"></a>Über das Einplanen einer Aufgabe

* **Trigger mit Cron-Ausdruck** – Der Zeitgebertrigger für eine Aufgabe verwendet einen *Cron-Ausdruck*. Der Ausdruck ist eine Zeichenkette mit fünf Feldern, die die Minute, Stunde, Tag, Tag, Monat und Wochentag angibt, um die Aufgabe auszulösen. Es werden Frequenzen von bis zu einmal pro Minute unterstützt. 

  Beispielsweise löst der Ausdruck `"0 12 * * Mon-Fri"` an jedem Wochentag um 12 Uhr UTC eine Aufgabe aus. Informationen finden Sie unter den [Details](#cron-expressions) weiter unten in diesem Artikel.
* **Mehrere Zeitgebertrigger** – Das Hinzufügen mehrerer Timer zu einer Aufgabe ist erlaubt, solange die Zeitpläne unterschiedlich sind. 
    * Geben Sie beim Erstellen der Aufgabe mehrere Zeitgebertrigger an oder fügen Sie sie später hinzu.
    * Benennen Sie optional die Trigger zur einfacheren Verwaltung, oder ACR Tasks liefert standardmäßig Triggernamen.
    * Wenn Timer Zeitpläne zu einem Zeitpunkt überschneiden, löst ACR-Tasks den Task zum geplanten Zeitpunkt für jede Zeitgeber an. 
* **Andere Task-Trigger** – In einer Timer-gesteuerten Task können Sie auch Trigger aktivieren, die auf [Quellcode-Commit](container-registry-tutorial-build-task.md) oder [Basisbild-Updates](container-registry-tutorial-base-image-update.md) basieren. Wie andere ACR-Aufgaben können Sie auch eine geplante Aufgabe [manuell auslösen][az-acr-task-run].

## <a name="create-a-task-with-a-timer-trigger"></a>Erstellen einer Aufgabe mit einem Zeitgebertrigger

Wenn Sie eine Aufgabe mit dem Befehl [az acr task create][az-acr-task-create] erstellen, können Sie optional einen Zeitgebertrigger hinzufügen. Fügen Sie den `--schedule`-Parameter hinzu und übergeben Sie einen Cron-Ausdruck für den Timer. 

Als einfaches Beispiel löst der folgende Befehl die tägliche Ausführung des `hello-world`-Bildes vom Docker Hub um 21:00 UTC aus. Die Aufgabe läuft ohne Quellcode-Kontext.

```azurecli
az acr task create \
  --name mytask \
  --registry myregistry \
  --context /dev/null \
  --cmd hello-world \
  --schedule "0 21 * * *"
```

Führen Sie den Befehl [az acr task show][az-acr-task-show] aus, um zu sehen, ob der Timertrigger konfiguriert ist. Standardmäßig ist auch der Trigger für die Aktualisierung des Basisbildes aktiviert.

```console
$ az acr task show --name mytask --registry registry --output table
NAME      PLATFORM    STATUS    SOURCE REPOSITORY       TRIGGERS
--------  ----------  --------  -------------------     -----------------
mytask    linux       Enabled                           BASE_IMAGE, TIMER
```

Triggern Sie die Aufgabe manuell mit [az acr task run][az-acr-task-run], um sicherzustellen, dass sie richtig eingerichtet ist:

```azurecli
az acr task run --name mytask --registry myregistry
```

Wenn der Container erfolgreich läuft, ist die Ausgabe ähnlich wie folgt:

```console
Queued a run with ID: cf2a
Waiting for an agent...
2019/06/28 21:03:36 Using acb_vol_2ca23c46-a9ac-4224-b0c6-9fde44eb42d2 as the home volume
2019/06/28 21:03:36 Creating Docker network: acb_default_network, driver: 'bridge'
[...]
2019/06/28 21:03:38 Launching container with name: acb_step_0

Hello from Docker!
This message shows that your installation appears to be working correctly.
[...]
```

Führen Sie nach der geplanten Zeit den Befehl [az acr task list-runs][az-acr-task-list-runs] aus, um sicherzustellen, dass der Timer die Aufgabe wie erwartet ausgelöst hat:

```azurecli
az acr task list runs --name mytask --registry myregistry --output table
``` 

Wenn der Timer erfolgreich ist, lautet die Ausgabe wie folgt:

```console
RUN ID    TASK     PLATFORM    STATUS     TRIGGER    STARTED               DURATION
--------  -------- ----------  ---------  ---------  --------------------  ----------
[...]
cf2b      mytask   linux       Succeeded  Timer      2019-06-28T21:00:23Z  00:00:06
cf2a      mytask   linux       Succeeded  Manual     2019-06-28T20:53:23Z  00:00:06
```
            
## <a name="manage-timer-triggers"></a>Verwalten der Trigger mit Timer

Verwenden Sie die [az acr Task-Timer][az-acr-task-timer]-Befehle, um die Timer-Trigger für eine ACR-Aufgabe zu verwalten.

### <a name="add-or-update-a-timer-trigger"></a>Hinzufügen oder Aktualisieren eines Timer-Triggers

Nachdem eine Aufgabe erstellt wurde, fügen Sie optional einen Timertrigger hinzu, indem Sie den Befehl [az acr task timer add][az-acr-task-timer-add] verwenden. Das folgende Beispiel fügt einen Timer-Triggernamen *timer2* zu der zuvor erstellten *Mytask* hinzu. Dieser Timer löst die Aufgabe jeden Tag um 10:30 UTC aus.

```azurecli
az acr task timer add \
  --name mytask \
  --registry myregistry \
  --timer-name timer2 \
  --schedule "30 10 * * *"
```

Aktualisieren Sie den Zeitplan eines vorhandenen Triggers oder ändern Sie seinen Status, indem Sie den Befehl [az acr task timer update][az-acr-task-timer-update] verwenden. Aktualisieren Sie beispielsweise den Trigger mit dem Namen *timer2*, um die Aufgabe um 11:30 UTC auszulösen:

```azurecli
az acr task timer update \
  --name mytask \
  --registry myregistry \
  --timer-name timer2 \
  --schedule "30 11 * * *"
```

### <a name="list-timer-triggers"></a>Triggern der Listen-Timer

Der Befehl [az acr task timer list][az-acr-task-timer-list] zeigt die für eine Aufgabe eingerichteten Timertrigger an:

```azurecli
az acr task timer list --name mytask --registry myregistry
```

Beispielausgabe:

```JSON
[
  {
    "name": "timer2",
    "schedule": "30 11 * * *",
    "status": "Enabled"
  },
  {
    "name": "t1",
    "schedule": "0 21 * * *",
    "status": "Enabled"
  }
]
```

### <a name="remove-a-timer-trigger"></a>Entfernen eines Zeitgebertriggers 

Verwenden Sie den Befehl [az acr task timer remove][az-acr-task-timer-remove], um einen Zeitgebertrigger aus einer Aufgabe zu entfernen. Das folgende Beispiel entfernt den *Timer2* Trigger aus *Mytask* :

```azurecli
az acr task timer remove \
  --name mytask \
  --registry myregistry \
  --timer-name timer2
```

## <a name="cron-expressions"></a>CRON-Ausdrücke

ACR Tasks verwendet die [NCronTab](https://github.com/atifaziz/NCrontab)-Bibliothek, um Cron-Ausdrücke zu interpretieren. Unterstützte Ausdrücke in ACR-Aufgaben haben fünf Pflichtfelder, die durch Leerzeichen getrennt sind:

`{minute} {hour} {day} {month} {day-of-week}`

Die mit den Cron-Ausdrücken verwendete Zeitzone ist Coordinated Universal Time (UTC). Stunden werden im 24-Stunden-Format angegeben.

> [!NOTE]
> ACR Tasks unterstützt das `{second}` oder `{year}` Feld in Cron-Ausdrücken nicht. Wenn Sie einen Cron-Ausdruck kopieren, der in einem anderen System verwendet wird, müssen Sie diese Felder entfernen, falls sie verwendet werden.

Jedes Feld kann einen der folgenden Werttypen aufweisen:

|type  |Beispiel  |Auslösung  |
|---------|---------|---------|
|Ein bestimmter Wert |<nobr>"5 * * * *"</nobr>|Stündliche Ausführung jeweils 5 Minuten nach der vollen Stunde|
|Alle Werte (`*`)|<nobr>"* 5 * * *"</nobr>|Jede Minute der Stunde ab 5:00 Uhr UTC (60-Mal pro Tag)|
|Ein Bereich (`-`-Operator)|<nobr>"0 1-3 * * *"</nobr>|3 mal täglich, um 1:00, 2:00 und 3:00 Uhr UTC|  
|Eine Gruppe von Werten (`,`-Operator)|<nobr>"20,30,40 * * * *"</nobr>|3 mal pro Stunde, bei 20 Minuten, 30 Minuten und 40 Minuten nach der Stunde|
|Ein Intervallwert (`/`-Operator)|<nobr>"*/10 * * * *"</nobr>|6 mal pro Stunde, bei 10 Minuten, 20 Minuten usw., nach der Stunde

[!INCLUDE [functions-cron-expressions-months-days](../../includes/functions-cron-expressions-months-days.md)]

### <a name="cron-examples"></a>CRON-Beispiele

|Beispiel|Auslösung  |
|---------|---------|
|`"*/5 * * * *"`|einmal alle fünf Minuten|
|`"0 * * * *"`|einmal zu jeder vollen Stunde|
|`"0 */2 * * *"`|einmal alle zwei Stunden|
|`"0 9-17 * * *"`|zwischen 9:00 und 17:00 Uhr jeweils einmal pro Stunde|
|`"30 9 * * *"`|täglich um 9:30 Uhr|
|`"30 9 * * 1-5"`|werktags um 9:30 Uhr|
|`"30 9 * Jan Mon"`|jeden Montag im Januar um 9:30|


## <a name="next-steps"></a>Nächste Schritte

Beispiele für Aufgaben, die durch Quellcode-Commits oder Basisbild-Updates ausgelöst werden, finden Sie in der [Tutorialserie ACR Tasks](container-registry-tutorial-quick-task.md).



<!-- LINKS - External -->
[task-examples]: https://github.com/Azure-Samples/acr-tasks


<!-- LINKS - Internal -->
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-show]: /cli/azure/acr/task#az-acr-task-show
[az-acr-task-list-runs]: /cli/azure/acr/task#az-acr-task-list-runs
[az-acr-task-timer]: /cli/azure/acr/task/timer
[az-acr-task-timer-add]: /cli/azure/acr/task/timer#az-acr-task-timer-add
[az-acr-task-timer-remove]: /cli/azure/acr/task/timer#az-acr-task-timer-remove
[az-acr-task-timer-list]: /cli/azure/acr/task/timer#az-acr-task-timer-list
[az-acr-task-timer-update]: /cli/azure/acr/task/timer#az-acr-task-timer-update
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-task]: /cli/azure/acr/task
[azure-cli-install]: /cli/azure/install-azure-cli
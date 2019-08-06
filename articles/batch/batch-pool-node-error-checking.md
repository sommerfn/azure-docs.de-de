---
title: Suchen nach Pool- und Knotenfehlern – Azure Batch
description: Erfahren Sie mehr über das Suchen nach Fehlern und Vermeiden dieser Fehler bei der Erstellung von Pools und Knoten.
services: batch
ms.service: batch
author: mscurrell
ms.author: markscu
ms.date: 07/16/2019
ms.topic: conceptual
ms.openlocfilehash: 9481263773cc919fecacce80191cf209ec2a1282
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2019
ms.locfileid: "68359246"
---
# <a name="check-for-pool-and-node-errors"></a>Suchen nach Pool- und Knotenfehlern

Wenn Sie Azure Batch-Pools erstellen und verwalten, erfolgen einige Vorgänge sofort. Einige Vorgänge werden aber asynchron und im Hintergrund ausgeführt. Es kann einige Minuten dauern, bis der Vorgang abgeschlossen ist.

Das Erkennen von Fehlern bei unmittelbar erfolgenden Vorgängen, ist einfach, da sämtliche Fehler direkt über die API, CLI oder Benutzeroberfläche zurückgegeben werden.

In diesem Artikel werden die Hintergrundvorgänge behandelt, die für Pools und Poolknoten erfolgen können. Es wird erörtert, wie Sie Fehler erkennen und vermeiden.

## <a name="pool-errors"></a>Poolfehler

### <a name="resize-timeout-or-failure"></a>Timeout oder Fehler bei Größenänderung

Bei Erstellung eines neuen Pools oder Änderung der Größe eines vorhandenen Pools geben Sie die geplante Anzahl der Knoten an.  Der Vorgang für die Erstellung oder Änderung der Größe wird unmittelbar abgeschlossen, aber die tatsächliche Zuordnung der neuen bzw. die Entfernung vorhandener Knoten kann einige Minuten dauern.  Sie geben das Timeout bei Größenänderung in der API [create](https://docs.microsoft.com/rest/api/batchservice/pool/add) oder [resize](https://docs.microsoft.com/rest/api/batchservice/pool/resize) an. Falls Batch die Zielanzahl von Knoten während des Timeouts für die Größenänderung nicht abrufen kann, wird der Pool in einen stabilen Zustand versetzt und meldet Größenänderungsfehler.

Mit der [ResizeError](https://docs.microsoft.com/rest/api/batchservice/pool/get#resizeerror)-Eigenschaft für die aktuellen Evaluierungslisten werden die aufgetretenen Fehler aufgelistet.

Häufige Gründe für Größenänderungsfehler sind:

- Der Timeout für die Größenänderung ist zu kurz.
  - In den meisten Fällen ist das standardmäßige Timeout von 15 Minuten lang genug, um Poolknoten zuzuordnen oder zu entfernen.
  - Wenn Sie eine große Anzahl von Knoten zuordnen, empfehlen wir, das Timeout für die Größenänderung auf 30 Minuten festzulegen. Das gilt beispielsweise, wenn Sie die Größe über ein Azure Marketplace-Image auf mehr als 1.000 Knoten oder über ein benutzerdefinierte VM-Image auf mehr als 300 Knoten ändern.
- Kernkontingent nicht ausreichend
  - Ein Azure Batch-Konto weist eine Begrenzung für die Anzahl von Kernen auf, die poolübergreifend zugeordnet werden können. Sobald dieses Kontingent erschöpft ist, werden von Azure Batch keine weiteren Knoten zugeordnet. Sie können das Kernkontingent [erhöhen](https://docs.microsoft.com/azure/batch/batch-quota-limit), sodass Azure Batch mehr Knoten zuordnen kann.
- Nicht genügend Subnetz-IP-Adressen, wenn sich ein [Pool in einem virtuellen Netzwerk](https://docs.microsoft.com/azure/batch/batch-virtual-network) befindet
  - Das Subnetz eines virtuellen Netzwerks muss über ausreichend nicht zugewiesene IP-Adressen verfügen, die allen angeforderten Poolknoten zugeordnet werden können. Andernfalls können die Knoten nicht erstellt werden.
- Nicht genügend Ressourcen, wenn sich ein [Pool in einem virtuellen Netzwerk](https://docs.microsoft.com/azure/batch/batch-virtual-network) befindet
  - Sie können Ressourcen, wie z. B. Lastenausgleichsmodule, öffentliche IP-Adressen und Netzwerksicherheitsgruppen, im selben Abonnement wie das Azure Batch-Konto erstellen. Vergewissern Sie sich, dass die Abonnementkontingente für diese Ressourcen ausreichend sind.
- Große Pools mit benutzerdefinierten VM-Images
  - Die Zuordnung großer Pools, die benutzerdefinierte Images verwenden, kann länger dauern und zu Timeouts bei Größenänderungen führen.  Empfehlungen zu Grenzwerten und zur Konfiguration finden Sie unter [Verwenden eines benutzerdefinierten Images zum Erstellen eines VM-Pools](https://docs.microsoft.com/azure/batch/batch-custom-images).

### <a name="automatic-scaling-failures"></a>Fehler bei der automatischen Skalierung

Sie können Azure Batch auch so einrichten, dass die Anzahl der Knoten in einem Pool automatisch skaliert wird. Sie definieren die Parameter für die [automatische Skalierungsformel für einen Pool](https://docs.microsoft.com/azure/batch/batch-automatic-scaling). Der Azure Batch-Dienst verwendet die Formel, um die Anzahl der Knoten im Pool regelmäßig auszuwerten und eine neue Zielanzahl festzulegen. Die folgenden Arten von Problemen können auftreten:

- Die Auswertung für die automatische Skalierung schlägt fehl.
- Die resultierende Größenänderung schlägt fehl und führt zu einem Timeout.
- Ein Problem mit der automatischen Skalierungsformel führt zu falschen Zielwerten für die Knoten. Die Größenänderung funktioniert, oder ein Timeout tritt auf.

Informationen zur letzten Auswertung zur automatischen Skalierung erhalten Sie mithilfe der [autoScaleRun](https://docs.microsoft.com/rest/api/batchservice/pool/get#autoscalerun)-Eigenschaft. Diese Eigenschaft meldet die Auswertungszeit, die Werte und das Ergebnis sowie eventuelle Leistungsfehler.

Das [Ereignis zum Abschluss der Größenänderung von Pools](https://docs.microsoft.com/azure/batch/batch-pool-resize-complete-event) erfasst Informationen zu sämtlichen Auswertungen.

### <a name="delete"></a>Löschen

Wenn Sie einen Pool löschen, der Knoten enthält, löscht Azure Batch zuerst die Knoten. Anschließend wird das Poolobjekt selbst gelöscht. Es kann einige Minuten dauern, bis die Poolknoten gelöscht wurden.

Der [Poolstatus](https://docs.microsoft.com/rest/api/batchservice/pool/get#poolstate) wird von Azure Batch während des Löschvorgangs auf **Wird gelöscht** festgelegt. Die aufrufende Anwendung kann mithilfe der Eigenschaften **state** und **stateTransitionTime** erkennen, ob der Löschvorgang im Pool zu lange dauert.

## <a name="pool-compute-node-errors"></a>Fehler in Pool-Computeknoten

Selbst wenn Azure Batch Knoten in einem Pool erfolgreich zuweist, können verschiedene Probleme dazu führen, dass einige Knoten fehlerhaft sind und keine Tasks ausführen können. Da für diese Knoten weiterhin Gebühren anfallen, ist es wichtig, Probleme zu erkennen. So wird verhindert, dass für Knoten bezahlt werden muss, die nicht genutzt werden können.

### <a name="start-task-failures"></a>Fehler bei Starttask

Für einen Pool kann ein optionaler [Starttask](https://docs.microsoft.com/rest/api/batchservice/pool/add#starttask) angegeben werden. Wie bei jedem Task können eine Befehlszeile und aus dem Speicher herunterzuladende Ressourcendateien angegeben werden. Der Starttask wird für jeden Knoten ausgeführt, nachdem er gestartet wurde. Die **waitForSuccess**-Eigenschaft gibt an, ob Azure Batch wartet, bis der Starttask erfolgreich abgeschlossen ist, ehe weitere Tasks für einen Knoten geplant werden.

Was passiert, wenn Sie den Knoten so konfiguriert haben, dass er auf die erfolgreiche Beendigung des Starttasks wartet, aber der Starttask fehlschlägt? In diesem Fall kann der Knoten nicht verwendet werden, aber es fallen weiterhin Gebühren dafür an.

Starttaskfehler können Sie mithilfe der Eigenschaften [result](https://docs.microsoft.com/rest/api/batchservice/computenode/get#taskexecutionresult) und [failureInfo](https://docs.microsoft.com/rest/api/batchservice/computenode/get#taskfailureinformation) der Knoteneigenschaft [startTaskInfo](https://docs.microsoft.com/rest/api/batchservice/computenode/get#starttaskinformation) der obersten Ebene erkennen.

Ein fehlgeschlagener Starttask führt auch dazu, dass Azure Batch den Knoten [state](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodestate) auf **starttaskfailed** festlegt, wenn **waitForSuccess** auf **TRUE** festgelegt wurde.

Wie bei jedem anderen Task kann es für das Fehlschlagen eines Starttasks viele Ursachen geben.  Zur Fehlerbehebung sollten Sie „stdout“, „stderr“ und weitere taskspezifische Protokolldateien überprüfen.

Starttasks müssen eintrittsinvariant sein, da es möglich ist, dass der Starttask mehrere Male auf demselben Knoten ausgeführt wird. Der Starttask wird ausgeführt, wenn für einen Knoten das Reimaging oder ein Neustart durchgeführt wird. In seltenen Fällen wird ein Starttask ausgeführt, nachdem ein Ereignis den Neustart eines Knotens verursacht hat, bei dem für einen der Betriebssystem- oder temporären Datenträger ein Reimaging durchgeführt wird, während dies für den anderen nicht der Fall war. Da Batch-Starttasks (wie alle Batch-Tasks) vom temporären Datenträger ausgeführt werden, ist dies normalerweise kein Problem. Aber in einigen Fällen, in denen der Starttask eine Anwendung auf dem Betriebssystemdatenträger installiert und andere Daten weiterhin auf dem temporären Datenträger aufbewahrt, kann dies zu Problemen führen, weil die Synchronisierung nicht mehr gewahrt ist. Schützen Sie Ihre Anwendung entsprechend, falls Sie beide Datenträger verwenden.

### <a name="application-package-download-failure"></a>Fehler beim Herunterladen des Anwendungspakets

Sie können ein oder mehrere Anwendungspakete für einen Pool angeben. Die angegebenen Paketdateien werden von Azure Batch auf die einzelnen Knoten heruntergeladen und nach dem Starten der Knoten, aber vor der Planung der Tasks, dekomprimiert. Normalerweise wird eine Befehlszeile für Starttasks in Verbindung mit Anwendungspaketen verwendet. Beispielsweise, um Dateien an einen anderen Speicherort zu kopieren oder ein Setup auszuführen.

Die [errors](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror)-Eigenschaft des Knotens meldet einen Fehler beim Herunterladen und Entkomprimieren eines Anwendungspakets. Der Knotenstatus wird auf **Nicht verwendbar** festgelegt.

### <a name="container-download-failure"></a>Fehler beim Herunterladen von Containern

Sie können in einem Pool eine oder mehrere Containerreferenzen angeben. Batch lädt die angegebenen Container auf die einzelnen Knoten herunter. Die [Fehler](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror)-Eigenschaft des Knotens meldet einen Fehler beim Herunterladen eines Containers und legt den Knotenstatus auf **Nicht verwendbar** fest.

### <a name="node-in-unusable-state"></a>Knoten mit dem Status „Nicht verwendbar“

Es kann verschiedene Ursachen dafür geben, dass Azure Batch den [Knotenstatus](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodestate) auf **Nicht verwendbar** festlegt. Wenn der Knotenstatus auf **Nicht verwendbar** festgelegt ist, können für den Knoten zwar keine Tasks geplant werden, es fallen jedoch weiterhin Gebühren für ihn an.

Knoten mit dem Zustand **Nicht verwendbar** – aber ohne [Fehler](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror) – bedeuten, dass Batch nicht mit dem virtuellen Computer kommunizieren kann. In diesem Fall versucht Batch immer, den virtuellen Computer wiederherzustellen. Batch versucht nicht automatisch, virtuelle Computer wiederherzustellen, die keine Anwendungspakete oder Container installiert haben, obwohl ihr Zustand **Nicht verwendbar** ist.

Wenn Azure Batch die Ursache bestimmen kann, wird sie von der Knoteneigenschaft [errors](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror) gemeldet.

Einige weitere Beispiele für Ursachen für **nicht verwendbare** Knoten sind u. a.:

- Ein benutzerdefiniertes VM-Image ist ungültig. Beispielsweise, wenn ein Image nicht ordnungsgemäß vorbereitet wurde.

- Eine VM wird aufgrund eines Infrastrukturausfalls oder eines niedrigstufigen Upgrades verschoben. Azure Batch stellt den Knoten wieder her.

- Ein VM-Image wurde auf Hardware bereitgestellt, von der es nicht unterstützt wird. Beispiel: Der Versuch, ein CentOS-HPC-Image auf einem virtuellen [Standard_D1_v2](../virtual-machines/linux/sizes-general.md#dv2-series)-Computer auszuführen.

- Die VMs befinden sich in einem [virtuellen Azure-Netzwerk](batch-virtual-network.md), und der Datenverkehr zu den Schlüsselports wurde blockiert.

- Die VMs befinden sich in einem virtuellen Netzwerk, aber der ausgehende Datenverkehr an Azure Storage ist blockiert.

- Die VMs befinden sich in einem virtuellen Netzwerk mit einer DNS-Kundenkonfiguration, und der DNS-Server kann Azure Storage nicht auflösen.

### <a name="node-agent-log-files"></a>Protokolldateien des Knoten-Agents

Der Batch-Agent-Prozess, der auf jedem Poolknoten ausgeführt wird, kann Protokolldateien bereitstellen, die hilfreich sein können, wenn Sie den Support bei einem Problem mit einem Poolknoten kontaktieren müssen. Protokolldateien für einen Knoten können über das Azure-Portal, Batch Explorer oder eine [API](https://docs.microsoft.com/rest/api/batchservice/computenode/uploadbatchservicelogs) hochgeladen werden. Es ist sinnvoll, die Protokolldateien hochzuladen und zu speichern. Anschließend können Sie den Knoten oder Pool löschen, um die Kosten für die ausgeführten Knoten zu sparen.

### <a name="node-disk-full"></a>Knotendatenträger voll

Das temporäre Laufwerk für eine Poolknoten-VM wird von Batch für Auftragsdateien, Taskdateien und gemeinsam genutzte Dateien verwendet. 

- Anwendungspaketdateien
- Taskressourcendateien
- Anwendungsspezifische Dateien, die in einen Batch-Ordner heruntergeladen wurden
- stdout- und stderr-Dateien für jede Ausführung von Taskanwendungen
- Anwendungsspezifische Ausgabedateien

Einige dieser Dateien werden nur einmal geschrieben, wenn die Poolknoten erstellt werden, z. B. Poolanwendungspakete oder Poolstarttask-Ressourcendateien. Auch wenn diese Dateien nur einmal bei der Knotenerstellung geschrieben werden, können sie den gesamten Speicherplatz des temporären Laufwerks belegen, falls sie zu groß sind.

Andere Dateien werden für jeden Task geschrieben, der auf einem Knoten ausgeführt wird, z. B. stdout and stderr. Wenn eine große Zahl von Tasks auf demselben Knoten ausgeführt wird bzw. die Taskdateien zu groß sind, ist das temporäre Laufwerk damit unter Umständen bereits gefüllt.

Die Größe des temporären Laufwerks hängt von der Größe des virtuellen Computers ab. Ein wichtiger Aspekt bei der Wahl der VM-Größe besteht darin, sich zu vergewissern, dass auf dem temporären Laufwerk genügend Platz ist.

- Im Azure-Portal kann beim Hinzufügen eines Pools die vollständige Liste mit VM-Größen angezeigt werden, und die Spalte „Ressourcen-Datenträgergröße“ ist vorhanden.
- Die Artikel, in denen die gesamten VM-Größen beschrieben werden, verfügen über Tabellen mit der Spalte „Temporärer Speicher“, z. B. [Compute-optimierte VM-Größen](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-compute).

Für Dateien, die von den einzelnen Tasks geschrieben werden, kann für jeden Task eine Aufbewahrungsdauer angegeben werden. Hiermit wird festgelegt, wie lange die Taskdateien aufbewahrt werden, bevor sie automatisch bereinigt werden. Die Aufbewahrungszeit kann reduziert werden, um den Speicherbedarf zu verringern.

Wenn der Speicherplatz auf dem temporären Datenträger ausgeschöpft ist, ist es derzeit so, dass die Ausführung der Tasks beendet wird. In Zukunft wird ein [Knotenfehler](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror) gemeldet.


## <a name="next-steps"></a>Nächste Schritte

Überprüfen Sie, ob Sie Ihre Anwendung so konfiguriert haben, dass eine umfassende Fehlerprüfung implementiert ist, insbesondere für asynchrone Vorgänge. Es kann entscheidend sein, Probleme frühzeitig erkennen und diagnostizieren zu können.

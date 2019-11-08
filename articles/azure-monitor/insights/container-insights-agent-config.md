---
title: Konfigurieren der Datensammlung des Azure Monitor für Container-Agent | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie den Azure Monitor für den Container-Agent so konfigurieren, dass dieser die Protokollsammlung von stdout-/stderr- sowie Umgebungsvariablen steuert.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 10/15/2019
ms.openlocfilehash: deab16f3b80ada12a7167e90922dc38f3012be91
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73478698"
---
# <a name="configure-agent-data-collection-for-azure-monitor-for-containers"></a>Konfigurieren der Datensammlung des Azure Monitor für Container-Agent

Azure Monitor für Container sammelt stdout-, stderr- sowie Umgebungsvariablen aus Containerworkloads vom Container-Agent, die für auf Azure Kubernetes Service (AKS) gehostete Managed Kubernetes-Cluster bereitgestellt wurden. Sie können Einstellungen für die Agent-Datensammlung konfigurieren, indem Sie eine benutzerdefinierte Kubernetes-ConfigMaps zum Steuern erstellen. 

In diesem Artikel erfahren Sie, wie Sie Ihren Anforderungen entsprechend eine ConfigMap erstellen und eine Datensammlung konfigurieren.

## <a name="configmap-file-settings-overview"></a>Übersicht über die Einstellungen für die ConfigMap-Datei

Es wird eine Vorlagendatei für die ConfigMap bereitgestellt, die Sie einfach anpassen können, ohne sie von Grund auf neu erstellen zu müssen. Bevor Sie beginnen, sollten Sie die Kubernetes-Dokumentation zu [ConfigMaps](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/) lesen und sich informieren, wie Sie ConfigMaps erstellen, konfigurieren und bereitstellen. So können Sie in den stderr- und stdout-Variablen oder im gesamten Cluster nach Namespace filtern. Zudem können Sie in den Umgebungsvariablen nach Containern filtern, die im Cluster auf allen Pods/Knoten laufen.

>[!IMPORTANT]
>Die Agent-Mindestversion, die zum Erfassen von stdout-, stderr- und Umgebungsvariablen aus Containerworkloads unterstützt wird, ist „ciprod06142019“. Um die Agent-Version zu überprüfen, wählen Sie auf der Registerkarte **Knoten** einen Knoten aus, und notieren Sie im Eigenschaftenbereich den Wert der Eigenschaft **Agent-Imagetag**. Weitere Informationen zu den Agent-Versionen und den Inhalten der jeweiligen Releases finden Sie unter [Versionshinweise zum Agent](https://github.com/microsoft/Docker-Provider/tree/ci_feature_prod).

### <a name="data-collection-settings"></a>Einstellungen für Datensammlung

Die folgenden Einstellungen können zur Steuerung der Datensammlung konfiguriert werden.

|Schlüssel |Datentyp |Wert |BESCHREIBUNG |
|----|----------|------|------------|
|`schema-version` |Zeichenfolge (Groß-/Kleinschreibung wird berücksichtigt) |v1 |Diese Schemaversion wird vom Agent beim Analysieren dieser ConfigMap verwendet. Die derzeit unterstützte Schemaversion ist v1. Die Bearbeitung dieses Werts wird nicht unterstützt und bei der Auswertung der ConfigMap abgelehnt.|
|`config-version` |Zeichenfolge | | Unterstützt die Funktion, die Version dieser Konfigurationsdatei in Ihrem Quellcodeverwaltungssystem/-repository nachzuverfolgen. Es sind maximal 10 Zeichen zulässig, alle zusätzlichen Zeichen werden abgeschnitten. |
|`[log_collection_settings.stdout] enabled =` |Boolean | "true" oder "false" | Hierdurch wird gesteuert, ob die stdout-Containerprotokollsammlung aktiviert wird. Wenn dieser Wert auf `true` festgelegt ist und keine Namespaces für die stdout-Protokollsammlung ausgeschlossen sind (Einstellung `log_collection_settings.stdout.exclude_namespaces` unten), werden stdout-Protokolle von allen Containern auf allen Pods/Knoten im Cluster gesammelt. Wenn Sie in der ConfigMaps nichts angeben, lautet der Standardwert `enabled = true`. |
|`[log_collection_settings.stdout] exclude_namespaces =`|Zeichenfolge | Durch Trennzeichen getrenntes Array |Array aus Kubernetes-Namespaces, für die keine stdout-Protokolle gesammelt werden. Diese Einstellung gilt nur, wenn `log_collection_settings.stdout.enabled` auf `true` festgelegt ist. Wenn Sie in der ConfigMap nichts angeben, lautet der Standardwert `exclude_namespaces = ["kube-system"]`.|
|`[log_collection_settings.stderr] enabled =` |Boolean | "true" oder "false" |Hierdurch wird gesteuert, ob die stderr-Containerprotokollsammlung aktiviert wird. Wenn dieser Wert auf `true` festgelegt ist und keine Namespaces für die stdout-Protokollsammlung ausgeschlossen sind (Einstellung `log_collection_settings.stderr.exclude_namespaces`), werden stderr-Protokolle von allen Containern auf allen Pods/Knoten im Cluster gesammelt. Wenn Sie in der ConfigMaps nichts angeben, lautet der Standardwert `enabled = true`. |
|`[log_collection_settings.stderr] exclude_namespaces =` |Zeichenfolge |Durch Trennzeichen getrenntes Array |Array aus Kubernetes-Namespaces, für die keine stderr-Protokolle gesammelt werden. Diese Einstellung gilt nur, wenn `log_collection_settings.stdout.enabled` auf `true` festgelegt ist. Wenn Sie in der ConfigMap nichts angeben, lautet der Standardwert `exclude_namespaces = ["kube-system"]`. |
| `[log_collection_settings.env_var] enabled =` |Boolean | "true" oder "false" | Mit dieser Einstellung wird die Erfassung von Umgebungsvariablen von allen Pods/Knoten im Cluster gesteuert; falls sie nicht in ConfigMaps angegeben ist, ist sie standardmäßig auf `enabled = true` festgelegt. Wenn die Erfassung von Umgebungsvariablen global aktiviert ist, können Sie sie für einen bestimmten Container deaktivieren. Legen Sie dazu die Umgebungsvariable `AZMON_COLLECT_ENV` auf **false** fest, entweder mit einer Dockerfile-Einstellung oder in der [Konfigurationsdatei für den Pod](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/) unter dem Abschnitt **env:** . Wenn die Erfassung von Umgebungsvariablen global deaktiviert ist, können Sie die Erfassung für einen bestimmten Container nicht aktivieren (d. h., die einzige Außerkraftsetzung, die auf der Containerebene angewendet werden kann, besteht im Deaktivieren der Erfassung, wenn diese bereits global aktiviert ist). |

ConfigMaps ist eine globale Liste, und es kann nur eine ConfigMap auf den Agent angewendet werden. Es kann keine andere ConfigMaps vorhanden sein, die die Sammlungen außer Kraft setzt.

## <a name="configure-and-deploy-configmaps"></a>Konfigurieren und Bereitstellen von ConfigMaps

Führen Sie die folgenden Schritte durch, um Ihre ConfigMap-Konfigurationsdatei zu konfigurieren und für Ihren Cluster bereitzustellen.

1. [Laden Sie die Vorlagendatei (YAML) für die ConfigMap herunter](https://github.com/microsoft/OMS-docker/blob/ci_feature_prod/Kubernetes/container-azm-ms-agentconfig.yaml), und speichern Sie diese unter container-azm-ms-agentconfig.yaml.  

2. Bearbeiten Sie die YAML-Datei für die ConfigMap mit Ihren Anpassungen für das Sammeln von stdout, stderr und/oder Umgebungsvariablen.

    - Konfigurieren Sie den Schlüssel/Wert nach folgendem Beispiel, um bestimmte Namespaces aus der stdout-Protokollsammlung auszuschließen: `[log_collection_settings.stdout] enabled = true exclude_namespaces = ["my-namespace-1", "my-namespace-2"]`.
    
    - Die Sammlung von Umgebungsvariablen deaktivieren Sie für einen bestimmten Container, indem Sie folgendermaßen vorgehen: Legen Sie den Schlüssel/Wert `[log_collection_settings.env_var] enabled = true` fest, um die Variablensammlung global zu aktivieren, und führen Sie dann die [hier](container-insights-manage-agent.md#how-to-disable-environment-variable-collection-on-a-container) aufgeführten Schritte aus, um die Konfiguration für den bestimmten Container abzuschließen.
    
    - Konfigurieren Sie den Schlüssel/Wert nach dem folgenden Beispiel, um die stderr-Protokollsammlung im gesamten Cluster zu deaktivieren: `[log_collection_settings.stderr] enabled = false`.

3. Erstellen Sie die ConfigMap, indem Sie den folgenden kubectl-Befehl ausführen: `kubectl apply -f <configmap_yaml_file.yaml>`.
    
    Beispiel: `kubectl apply -f container-azm-ms-agentconfig.yaml`. 
    
    Es kann einige Minuten dauern, bis die Konfigurationsänderungen übernommen werden, und alle omsagent-Pods im Cluster werden neu gestartet. Der Neustart aller omsagent-Pods erfolgt gleitend, nicht alle werden gleichzeitig neu gestartet. Wenn die Neustarts abgeschlossen sind, wird eine Meldung angezeigt, die der folgenden ähnelt und das Ergebnis anzeigt: `configmap "container-azm-ms-agentconfig" created`.

4. Erstellen Sie die ConfigMap, indem Sie den folgenden kubectl-Befehl ausführen: `kubectl apply -f <configmap_yaml_file.yaml>`.
    
    Beispiel: `kubectl apply -f container-azm-ms-agentconfig.yaml`. 
    
    Es kann einige Minuten dauern, bis die Konfigurationsänderungen übernommen werden, und alle omsagent-Pods im Cluster werden neu gestartet. Der Neustart aller omsagent-Pods erfolgt gleitend, nicht alle werden gleichzeitig neu gestartet. Wenn die Neustarts abgeschlossen sind, wird eine Meldung angezeigt, die der folgenden ähnelt und das Ergebnis anzeigt: `configmap "container-azm-ms-agentconfig" created`.

## <a name="verify-configuration"></a>Überprüfen der Konfiguration 

Verwenden Sie den folgenden Befehl zum Überprüfen der Protokolle aus einem Agent-Pod, um sich zu vergewissern, dass die Konfiguration erfolgreich war: `kubectl logs omsagent-fdf58 -n=kube-system`. Bei Konfigurationsfehlern aus den omsagent-Pods werden von der Ausgabe Fehler wie die folgenden angezeigt:

``` 
***************Start Config Processing******************** 
config::unsupported/missing config schema version - 'v21' , using defaults
```

Fehler im Zusammenhang mit der Anwendung von Konfigurationsänderungen können ebenfalls überprüft werden. Die folgenden Optionen stehen zur zusätzlichen Problembehandlung bei Konfigurationsänderungen zur Verfügung:

- In einem Agent-Pod-Protokoll mithilfe desselben `kubectl logs`-Befehls. 

- In Liveprotokollen. Liveprotokolle zeigen ähnliche Fehler wie den folgenden an:

    ```
    config::error::Exception while parsing config map for log collection/env variable settings: \nparse error on value \"$\" ($end), using defaults, please check config map for errors
    ```

- In der Tabelle **KubeMonAgentEvents** in Ihrem Log Analytics Arbeitsbereich. Für Konfigurationsfehler werden stündlich Daten mit dem Schweregrad *Fehler* gesendet. Wenn keine Fehler vorliegen, enthält der Eintrag in der Tabelle Daten mit dem Schweregrad *Info*, der Fehlerfreiheit angibt. Die Eigenschaft **Tags** enthält weitere Informationen zum Pod und der Container-ID, für die der Fehler aufgetreten ist, sowie zum ersten Vorkommen, letzten Vorkommen und der Anzahl in der letzten Stunde.

Fehler verhindern die Analyse der Datei durch omsagent, weshalb ein Neustart erfolgt und die Standardkonfiguration verwendet wird. Nachdem Sie den oder die Fehler in der ConfigMap korrigiert haben, speichern Sie die YAML-Datei, und wenden Sie die aktualisierte ConfigMap an, indem Sie folgenden Befehl ausführen: `kubectl apply -f <configmap_yaml_file.yaml`.

## <a name="applying-updated-configmap"></a>Anwenden der aktualisierten ConfigMap

Wenn Sie bereits eine ConfigMap in Ihrem Cluster bereitgestellt haben und sie mit einer neueren Konfiguration aktualisieren möchten, können Sie die zuvor verwendete ConfigMap-Datei bearbeiten und dann mit dem gleichen Befehl wie zuvor (`kubectl apply -f <configmap_yaml_file.yaml`) anwenden.

Es kann einige Minuten dauern, bis die Konfigurationsänderungen übernommen werden, und alle omsagent-Pods im Cluster werden neu gestartet. Der Neustart aller omsagent-Pods erfolgt gleitend, nicht alle werden gleichzeitig neu gestartet. Wenn die Neustarts abgeschlossen sind, wird eine Meldung angezeigt, die der folgenden ähnelt und das Ergebnis anzeigt: `configmap "container-azm-ms-agentconfig" updated`.

## <a name="verifying-schema-version"></a>Überprüfen der Schemaversion

Unterstützte Konfigurationsschemaversionen stehen als Podanmerkung (schema-versions) auf dem omsagent-Pod zur Verfügung. Sie können diese mit dem folgenden kubectl-Befehl anzeigen: `kubectl describe pod omsagent-fdf58 -n=kube-system`

Die Ausgabe sieht ähnlich der folgenden mit der Anmerkung „schema-versions“ aus:

```
    Name:           omsagent-fdf58
    Namespace:      kube-system
    Node:           aks-agentpool-95673144-0/10.240.0.4
    Start Time:     Mon, 10 Jun 2019 15:01:03 -0700
    Labels:         controller-revision-hash=589cc7785d
                    dsName=omsagent-ds
                    pod-template-generation=1
    Annotations:    agentVersion=1.10.0.1
                  dockerProviderVersion=5.0.0-0
                    schema-versions=v1 
```

## <a name="next-steps"></a>Nächste Schritte

- Azure Monitor für Container umfasst keinen vordefinierten Satz von Warnungen. Informationen zum Erstellen von empfohlenen Warnungen für hohe CPU- und Arbeitsspeicherauslastung zur Unterstützung Ihrer DevOps oder Betriebsprozesse und -verfahren finden Sie unter [Erstellen von Leistungswarnungen mit Azure Monitor für Container](container-insights-alerts.md).

- Wenn die Überwachung aktiviert ist, um Integrität und Ressourcennutzung Ihres AKS- oder Hybridclusters und der darauf ausgeführten Workloads zu erfassen, informieren Sie sich über die [Verwendung](container-insights-analyze.md) von Azure Monitor für Container.

- Sehen Sie sich die [Beispiele zu Protokollabfragen](container-insights-log-search.md#search-logs-to-analyze-data) an, die auch vordefinierte Abfragen enthalten. Mit diesen Materialien können Sie Auswertungen von bzw. Anpassungen für Warnungen, Visualisierungen und Analysen von Clustern vornehmen.

---
title: Konfigurieren der Datensammlung des Azure Monitor für Container-Agent | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie den Azure Monitor für den Container-Agent so konfigurieren, dass dieser die Protokollsammlung von stdout-/stderr- sowie Umgebungsvariablen steuert.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/12/2019
ms.author: magoedte
ms.openlocfilehash: 12010aaa7bc90bd200264549ad3efb79f46576c6
ms.sourcegitcommit: 10251d2a134c37c00f0ec10e0da4a3dffa436fb3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2019
ms.locfileid: "67867645"
---
# <a name="configure-agent-data-collection-for-azure-monitor-for-containers"></a>Konfigurieren der Datensammlung des Azure Monitor für Container-Agent

Azure Monitor für Container sammelt stdout-, stderr- sowie Umgebungsvariablen aus Containerworkloads vom Container-Agent, die für auf Azure Kubernetes Service (AKS) gehostete Managed Kubernetes-Cluster bereitgestellt wurden. Dieser Agent kann mithilfe des Container-Agents auch Zeitreihendaten (auch als Metriken bezeichnet) von Prometheus sammeln, ohne einen Prometheus-Server und eine Datenbank einrichten und verwalten zu müssen. Sie können Einstellungen für die Agent-Datensammlung konfigurieren, indem Sie eine benutzerdefinierte Kubernetes-ConfigMaps zum Steuern erstellen. 

In diesem Artikel erfahren Sie, wie Sie Ihren Anforderungen entsprechend eine ConfigMap erstellen und eine Datensammlung konfigurieren.

>[!NOTE]
>Die Unterstützung für Prometheus ist zurzeit als Feature in der öffentlichen Vorschau verfügbar.
>

## <a name="configure-your-cluster-with-custom-data-collection-settings"></a>Konfigurieren Ihres Clusters mit benutzerdefinierten Datensammlungseinstellungen

Es wird eine Vorlagendatei für die ConfigMap bereitgestellt, die Sie einfach anpassen können, ohne sie von Grund auf neu erstellen zu müssen. Bevor Sie beginnen, sollten Sie die Kubernetes-Dokumentation zu [ConfigMaps](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/) lesen und sich informieren, wie Sie ConfigMaps erstellen, konfigurieren und bereitstellen. So können Sie in den stderr- und stdout-Variablen oder im gesamten Cluster nach Namespace filtern. Zudem können Sie in den Umgebungsvariablen nach Containern filtern, die im Cluster auf allen Pods/Knoten laufen.

>[!IMPORTANT]
>Die Agent-Mindestversion, die zum Erfassen von stdout-, stderr- und Umgebungsvariablen aus Containerworkloads unterstützt wird, ist „ciprod06142019“. Die Agent-Mindestversion zum Abrufen von Prometheus-Metriken ist „ciprod07092019“. Um die Agent-Version zu überprüfen, wählen Sie auf der Registerkarte **Knoten** einen Knoten aus, und notieren Sie im Eigenschaftenbereich den Wert der Eigenschaft **Agent-Imagetag**.  

### <a name="overview-of-configurable-data-collection-settings"></a>Übersicht über die konfigurierbaren Datensammlungseinstellungen

Die folgenden Einstellungen können zur Steuerung der Datensammlung konfiguriert werden.

|Schlüssel |Datentyp |Wert |BESCHREIBUNG |
|----|----------|------|------------|
|`schema-version` |Zeichenfolge (Groß-/Kleinschreibung wird berücksichtigt) |v1 |Diese Schemaversion wird vom Agent beim Analysieren dieser ConfigMap verwendet. Die derzeit unterstützte Schemaversion ist v1. Die Bearbeitung dieses Werts wird nicht unterstützt und bei der Auswertung der ConfigMap abgelehnt.|
|`config-version` |string | | Unterstützt die Funktion, die Version dieser Konfigurationsdatei in Ihrem Quellcodeverwaltungssystem/-repository nachzuverfolgen. Es sind maximal 10 Zeichen zulässig, alle zusätzlichen Zeichen werden abgeschnitten. |
|`[log_collection_settings.stdout] enabled =` |Boolean | "true" oder "false" | Hierdurch wird gesteuert, ob die stdout-Containerprotokollsammlung aktiviert wird. Wenn dieser Wert auf `true` festgelegt ist und keine Namespaces für die stdout-Protokollsammlung ausgeschlossen sind (Einstellung `log_collection_settings.stdout.exclude_namespaces` unten), werden stdout-Protokolle von allen Containern auf allen Pods/Knoten im Cluster gesammelt. Wenn Sie in der ConfigMaps nichts angeben, lautet der Standardwert `enabled = true`. |
|`[log_collection_settings.stdout] exclude_namespaces =`|string | Durch Trennzeichen getrenntes Array |Array aus Kubernetes-Namespaces, für die keine stdout-Protokolle gesammelt werden. Diese Einstellung gilt nur, wenn `log_collection_settings.stdout.enabled` auf `true` festgelegt ist. Wenn Sie in der ConfigMap nichts angeben, lautet der Standardwert `exclude_namespaces = ["kube-system"]`.|
|`[log_collection_settings.stderr] enabled =` |Boolean | "true" oder "false" |Hierdurch wird gesteuert, ob die stderr-Containerprotokollsammlung aktiviert wird. Wenn dieser Wert auf `true` festgelegt ist und keine Namespaces für die stdout-Protokollsammlung ausgeschlossen sind (Einstellung `log_collection_settings.stderr.exclude_namespaces`), werden stderr-Protokolle von allen Containern auf allen Pods/Knoten im Cluster gesammelt. Wenn Sie in der ConfigMaps nichts angeben, lautet der Standardwert `enabled = true`. |
|`[log_collection_settings.stderr] exclude_namespaces =` |string |Durch Trennzeichen getrenntes Array |Array aus Kubernetes-Namespaces, für die keine stderr-Protokolle gesammelt werden. Diese Einstellung gilt nur, wenn `log_collection_settings.stdout.enabled` auf `true` festgelegt ist. Wenn Sie in der ConfigMap nichts angeben, lautet der Standardwert `exclude_namespaces = ["kube-system"]`. |
| `[log_collection_settings.env_var] enabled =` |Boolean | "true" oder "false" | Hierdurch wird gesteuert, ob die Sammlung von Umgebungsvariablen aktiviert wird. Wenn der Wert auf `false` festgelegt ist, werden für keinen Container auf allen Pods/Knoten im Cluster Umgebungsvariablen gesammelt. Wenn der Wert in der ConfigMap nicht angegeben wird, ist der Standardwert `enabled = true`. |

## <a name="overview-of-configurable-prometheus-scraping-settings"></a>Übersicht über konfigurierbare Prometheus-Abrufeinstellungen

Das aktive Abrufen von Metriken von Prometheus erfolgt aus einer von zwei Perspektiven:

* Clusterübergreifend: HTTP-URL und ermittelte Ziele aus aufgelisteten Endpunkten eines Diensts, k8s-Dienste wie kube-dns und kube-state-metrics sowie Podanmerkungen, die für eine Anwendung spezifisch sind. Die in diesem Kontext gesammelten Metriken werden im ConfigMap-Abschnitt *[Prometheus data_collection_settings.cluster]* definiert.
* Knotenweit: HTTP-URL und ermittelte Ziele aus aufgelisteten Endpunkten eines Diensts. Die in diesem Kontext gesammelten Metriken werden im ConfigMap-Abschnitt *[Prometheus_data_collection_settings.node]* definiert.

|`Scope` | Schlüssel | Datentyp | Wert | BESCHREIBUNG |
|------|-----|-----------|-------|-------------|
| Clusterweit | | | | Geben Sie eine der folgenden drei Methoden zum Abrufen von Endpunkten für Metriken an. |
| | `urls` | string | Durch Trennzeichen getrenntes Array | HTTP-Endpunkt (entweder IP-Adresse oder gültiger URL-Pfad angegeben). Beispiel: `urls=[$NODE_IP/metrics]`. („$NODE_IP“ ist ein spezifischer Azure Monitor für Containerparameter und kann anstelle der Knoten-IP-Adresse verwendet werden. Muss alles in Großbuchstaben sein.) |
| | `kubernetes_services` | string | Durch Trennzeichen getrenntes Array | Ein Array von Kubernetes Services zum Abrufen von Metriken aus „kube-state-metrics“. Beispiel: `kubernetes_services = ["https://metrics-server.kube-system.svc.cluster.local/metrics", http://my-service-dns.my-namespace:9100/metrics]`.|
| | `monitor_kubernetes_pods` | Boolean | "true" oder "false" | Wenn die Option in den clusterweiten Einstellungen auf `true` festgelegt ist, ruft der Azure Monitor für Container-Agent Kubernetes Pods aus dem gesamten Cluster für die folgenden Prometheus-Anmerkungen ab:<br> `prometheus.io/scrape:`<br> `prometheus.io/scheme:`<br> `prometheus.io/path:`<br> `prometheus.io/port:` |
| | `prometheus.io/scrape` | Boolean | "true" oder "false" | Ermöglicht das Abrufen des Pods. |
| | `prometheus.io/scheme` | string | HTTP oder HTTPS | Nimmt den Standardwert Abrufen über HTTP an. Ggf. auf `https` festlegen. | 
| | `prometheus.io/path` | string | Durch Trennzeichen getrenntes Array | Der HTTP-Ressourcenpfad, aus dem Metriken abgerufen werden sollen. Wenn der Metrikpfad nicht `/metrics` lautet, definieren Sie ihn mit dieser Anmerkung. |
| | `prometheus.io/port` | string | 9102 | Einen Port angeben, an dem gelauscht werden soll. Wenn der Port nicht festgelegt ist, wird standardmäßig 9102 verwendet. |
| Knotenweit | `urls` | string | Durch Trennzeichen getrenntes Array | HTTP-Endpunkt (entweder IP-Adresse oder gültiger URL-Pfad angegeben). Beispiel: `urls=[$NODE_IP/metrics]`. („$NODE_IP“ ist ein spezifischer Azure Monitor für Containerparameter und kann anstelle der Knoten-IP-Adresse verwendet werden. Muss alles in Großbuchstaben sein.) |
| Knotenweit oder clusterweit | `interval` | string | 60s | Der Standardwert für das Sammlungsintervall ist eine Minute (60 Sekunden). Sie können die Sammlung entweder für *[prometheus_data_collection_settings.node]* und/oder für *[prometheus_data_collection_settings.cluster]* in Zeiteinheiten wie ns, us (oder Âµs), ms, s, m, h ändern. |
| Knotenweit oder clusterweit | `fieldpass`<br> `fielddrop`| string | Durch Trennzeichen getrenntes Array | Sie können bestimmte Metriken angeben, die vom Endpunkt erfasst werden sollen oder nicht, indem Sie die Zulassungs- (`fieldpass`) und Sperrauflistung (`fielddrop`) festlegen. Sie müssen die Zulassungsliste zuerst festlegen. |

ConfigMap ist eine globale Liste, und es kann nur eine ConfigMap auf den Agent angewendet werden. Es kann keine andere ConfigMap vorhanden sein, die die Sammlungen außer Kraft setzt.

### <a name="configure-and-deploy-configmaps"></a>Konfigurieren und Bereitstellen von ConfigMaps

Führen Sie die folgenden Schritte durch, um Ihre ConfigMap-Konfigurationsdatei zu konfigurieren und für Ihren Cluster bereitzustellen.

1. [Laden Sie die Vorlagendatei (YAML) für die ConfigMap herunter](https://github.com/microsoft/OMS-docker/blob/ci_feature_prod/Kubernetes/container-azm-ms-agentconfig.yaml), und speichern Sie diese unter container-azm-ms-agentconfig.yaml.  
1. Passen Sie die ConfigMap-Datei (YAML-Datei) wie gewünscht an.

    - Konfigurieren Sie den Schlüssel/Wert nach folgendem Beispiel, um bestimmte Namespaces aus der stdout-Protokollsammlung auszuschließen: `[log_collection_settings.stdout] enabled = true exclude_namespaces = ["my-namespace-1", "my-namespace-2"]`.
    - Die Sammlung von Umgebungsvariablen deaktivieren Sie für einen bestimmten Container, indem Sie folgendermaßen vorgehen: Legen Sie den Schlüssel/Wert `[log_collection_settings.env_var] enabled = true` fest, um die Variablensammlung global zu aktivieren, und führen Sie dann die [hier](container-insights-manage-agent.md#how-to-disable-environment-variable-collection-on-a-container) aufgeführten Schritte aus, um die Konfiguration für den bestimmten Container abzuschließen.
    - Konfigurieren Sie den Schlüssel/Wert nach dem folgenden Beispiel, um die stderr-Protokollsammlung im gesamten Cluster zu deaktivieren: `[log_collection_settings.stderr] enabled = false`.

1. Erstellen Sie die ConfigMap, indem Sie den folgenden kubectl-Befehl ausführen: `kubectl apply -f <configmap_yaml_file.yaml>`.
    
    Beispiel: `kubectl apply -f container-azm-ms-agentconfig.yaml`. 
    
    Es kann einige Minuten dauern, bis die Konfigurationsänderungen übernommen werden, und alle omsagent-Pods im Cluster werden neu gestartet. Der Neustart aller omsagent-Pods erfolgt gleitend, nicht alle werden gleichzeitig neu gestartet. Wenn die Neustarts abgeschlossen sind, wird eine Meldung angezeigt, die der folgenden ähnelt und das Ergebnis anzeigt: `configmap "container-azm-ms-agentconfig" created`.

Verwenden Sie den folgenden Befehl zum Überprüfen der Protokolle aus einem Agent-Pod, um sich zu vergewissern, dass die Konfiguration erfolgreich war: `kubectl logs omsagent-fdf58 -n=kube-system`. Bei Konfigurationsfehlern aus den omsagent-Pods werden von der Ausgabe Fehler wie die folgenden angezeigt:

``` 
***************Start Config Processing******************** 
config::unsupported/missing config schema version - 'v21' , using defaults
```

Fehler im Zusammenhang mit der Anwendung von Konfigurationsänderungen für Prometheus können auch überprüft werden.  Entweder in den Protokollen eines Agent-Pods mithilfe desselben `kubectl logs`-Befehls oder in Liveprotokollen. Liveprotokolle zeigen ähnliche Fehler wie den folgenden an:

```
2019-07-08T18:55:00Z E! [inputs.prometheus]: Error in plugin: error making HTTP request to http://invalidurl:1010/metrics: Get http://invalidurl:1010/metrics: dial tcp: lookup invalidurl on 10.0.0.10:53: no such host
```

Fehler verhindern die Analyse der Datei durch omsagent, weshalb ein Neustart erfolgt und die Standardkonfiguration verwendet wird. Nachdem Sie den oder die Fehler in der ConfigMap korrigiert haben, speichern Sie die YAML-Datei, und wenden Sie die aktualisierte ConfigMap an, indem Sie folgenden Befehl ausführen: `kubectl apply -f <configmap_yaml_file.yaml`.

## <a name="applying-updated-configmap"></a>Anwenden der aktualisierten ConfigMap

Wenn Sie bereits eine ConfigMap in Ihrem Cluster bereitgestellt haben und diese mit einer neueren Konfiguration aktualisieren möchten, können Sie einfach die zuvor verwendete ConfigMap-Datei bearbeiten und mit dem gleichen Befehl wie zuvor (`kubectl apply -f <configmap_yaml_file.yaml`) anwenden.

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

## <a name="review-prometheus-data-usage"></a>Überprüfen der Prometheus-Datenverwendung

Um das Erfassungsvolumen jeder Metrikgröße in GB pro Tag zu ermitteln, um zu verstehen, ob es hoch ist, wird die folgende Abfrage bereitgestellt.

```
InsightsMetrics 
| where Namespace contains "prometheus"
| where TimeGenerated > ago(24h)
| summarize VolumeInGB = (sum(_BilledSize) / (1024 * 1024 * 1024)) by Name
| order by VolumeInGB desc
| render barchart
```
Die Ausgabe zeigt ähnliche Ergebnisse wie die folgenden an:

![Protokollabfrageergebnisse von Datenerfassungsvolumen](./media/container-insights-agent-config/log-query-example-usage-03.png)

Um abzuschätzen, was jede Metrikgröße in GB für einen Monat bedeutet, um zu verstehen, ob das Volumen der im Arbeitsbereich empfangenen Erfassungsdaten hoch ist, wird die folgende Abfrage bereitgestellt.

```
InsightsMetrics 
| where Namespace contains "prometheus"
| where TimeGenerated > ago(24h)
| summarize EstimatedGBPer30dayMonth = (sum(_BilledSize) / (1024 * 1024 * 1024)) * 30 by Name
| order by EstimatedGBPer30dayMonth desc
| render barchart
```

Die Ausgabe zeigt ähnliche Ergebnisse wie die folgenden an:

![Protokollabfrageergebnisse von Datenerfassungsvolumen](./media/container-insights-agent-config/log-query-example-usage-02.png)

Weitere Informationen zum Überwachen der Datenverwendung und zum Analysieren von Kosten finden Sie unter [Verwalten von Nutzung und Kosten mit Azure Monitor-Protokollen](../platform/manage-cost-storage.md).

## <a name="next-steps"></a>Nächste Schritte

Azure Monitor für Container umfasst keinen vordefinierten Satz von Warnungen. Informationen zum Erstellen von empfohlenen Warnungen für hohe CPU- und Arbeitsspeicherauslastung zur Unterstützung Ihrer DevOps oder Betriebsprozesse und -verfahren finden Sie unter [Erstellen von Leistungswarnungen mit Azure Monitor für Container](container-insights-alerts.md).

- Weitere Informationen zur Verwendung von Azure Monitor und zum Überwachen anderer Aspekte Ihres AKS-Clusters finden Sie unter [Anzeigen der Azure Kubernetes Service-Integrität](container-insights-analyze.md).

- Sehen Sie sich die [Beispiele zu Protokollabfragen](container-insights-log-search.md#search-logs-to-analyze-data) an, die auch vordefinierte Abfragen enthalten. Mit diesen Materialien können Sie Auswertungen von bzw. Anpassungen für Warnungen, Visualisierungen und Analysen von Clustern vornehmen.
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
ms.date: 06/22/2019
ms.author: magoedte
ms.openlocfilehash: 1e7506e311c38d87371dd1b65440b6fb41a7ce78
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341659"
---
# <a name="configure-agent-data-collection-for-azure-monitor-for-containers"></a>Konfigurieren der Datensammlung des Azure Monitor für Container-Agent

Azure Monitor für Container sammelt stdout-, stderr- sowie Umgebungsvariablen aus Containerworkloads vom Container-Agent, die für auf Azure Kubernetes Service (AKS) gehostete Managed Kubernetes-Cluster bereitgestellt wurden. Sie können Einstellungen für die Agent-Datensammlung konfigurieren, indem Sie eine benutzerdefinierte Kubernetes-ConfigMaps zum Steuern erstellen. In diesem Artikel erfahren Sie, wie Sie Ihren Anforderungen entsprechend eine ConfigMap erstellen und eine Datensammlung konfigurieren.

## <a name="configure-your-cluster-with-custom-data-collection-settings"></a>Konfigurieren Ihres Clusters mit benutzerdefinierten Datensammlungseinstellungen

Es wird eine Vorlagendatei für die ConfigMap bereitgestellt, die Sie einfach anpassen können, ohne sie von Grund auf neu erstellen zu müssen. Bevor Sie beginnen, sollten Sie die Kubernetes-Dokumentation zu [ConfigMaps](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/) lesen und sich informieren, wie Sie ConfigMaps erstellen, konfigurieren und bereitstellen. So können Sie in den stderr- und stdout-Variablen oder im gesamten Cluster nach Namespace filtern. Zudem können Sie in den Umgebungsvariablen nach Containern filtern, die im Cluster auf allen Pods/Knoten laufen.

>[!IMPORTANT]
>Die Mindestversion des Agents, die dieses Feature unterstützt, ist microsoft/oms:ciprod06142019 oder höher. 

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

Verwenden Sie den folgenden Befehl zum Überprüfen der Protokolle aus einem Agent-Pod, um sich zu vergewissern, dass die Konfiguration erfolgreich war: `kubectl logs omsagent-fdf58 -n=kube-system`. Bei Konfigurationsfehlern aus den osmagent-Pods werden von der Ausgabe Fehler wie die folgenden angezeigt:

``` 
***************Start Config Processing******************** 
config::unsupported/missing config schema version - 'v21' , using defaults
```

Fehler verhindern die Analyse der Datei durch omsagent, weshalb ein Neustart erfolgt und die Standardkonfiguration verwendet wird. Nachdem Sie den oder die Fehler in der ConfigMap korrigiert haben, speichern Sie die YAML-Datei, und wenden Sie die aktualisierte ConfigMap an, indem Sie folgenden Befehl ausführen: `kubectl apply -f <configmap_yaml_file.yaml`.

## <a name="applying-updated-configmap"></a>Anwenden der aktualisierten ConfigMap

Wenn Sie bereits eine ConfigMap für Ihren Cluster bereitgestellt haben und diese mit einer neueren Konfiguration aktualisieren möchten, können Sie einfach die zuvor verwendete ConfigMap-Datei bearbeiten und mit dem gleichen Befehl wie zuvor (`kubectl apply -f <configmap_yaml_file.yaml`) anwenden.

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

- Weitere Informationen zur Verwendung von Azure Monitor und zum Überwachen anderer Aspekte Ihres AKS-Clusters finden Sie unter [Anzeigen der Azure Kubernetes Service-Integrität](container-insights-analyze.md).
- Sehen Sie sich die [Beispiele zu Protokollabfragen](container-insights-log-search.md#search-logs-to-analyze-data) an, die auch vordefinierte Abfragen enthalten. Mit diesen Materialien können Sie Auswertungen von bzw. Anpassungen für Warnungen, Visualisierungen und Analysen von Clustern vornehmen.
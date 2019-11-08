---
title: Informationen zu Azure Policy für die AKS-Engine
description: Erfahren Sie, wie Azure Policy CustomResourceDefinitions und Open Policy Agent von Gatekeeper v3 verwendet, um Cluster mit der AKS-Engine zu verwalten.
author: DCtheGeek
ms.author: dacoulte
ms.date: 11/04/2019
ms.topic: conceptual
ms.service: azure-policy
ms.openlocfilehash: 71f3f26b9ea7f24d674c911e18c785b0798a072c
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510067"
---
# <a name="understand-azure-policy-for-aks-engine"></a>Grundlegendes zu Azure Policy für die AKS-Engine

Azure Policy wird in die [AKS-Engine](https://github.com/Azure/aks-engine/blob/master/docs/README.md) integriert, ein System, das praktische Tools bereitstellt, um schnell einen Bootstrapvorgang für einen selbstverwalteten Kubernetes-Cluster in Azure auszuführen. Diese Integration ermöglicht das zentrale und einheitliche Anwenden von Schutz- und Sicherheitsmaßnahmen in großem Umfang für Ihre selbstverwalteten Cluster der AKS-Engine. Durch die erweiterte Verwendung von [Open Policy Agent](https://www.openpolicyagent.org/) (OPA) von [Gatekeeper](https://github.com/open-policy-agent/gatekeeper) v3 (Betaversion), einem _Webhook für die Zugangssteuerung_ für Kubernetes, bietet Azure Policy die Möglichkeit, den Konformitätsstatus Ihrer Azure-Ressourcen und selbstverwalteten Cluster der AKS-Engine von einer zentralen Stelle aus zu verwalten und zu melden.

> [!NOTE]
> Azure Policy für die AKS-Engine befindet sich in der öffentlichen Vorschauphase und weist keine SLA auf. Gatekeeper v3 ist in der Betaversion erhältlich und wird von der Open Source-Community unterstützt. Der Dienst unterstützt nur integrierte Richtliniendefinitionen und einen einzelnen AKS-Engine-Cluster für jede Ressourcengruppe, die mit einem Dienstprinzipal konfiguriert ist.

> [!IMPORTANT]
> Um Unterstützung für Azure Policy für die AKS-Engine, AKS-Engine oder Gatekeeper v3 zu erhalten, erstellen Sie ein [neues Problem](https://github.com/Azure/aks-engine/issues/new/choose) im GitHub-Repository der AKS-Engine.

## <a name="overview"></a>Übersicht

Führen Sie die folgenden Aktionen aus, um Azure Policy für die AKS-Engine für Ihren selbstverwalteten Kubernetes-Cluster in Azure zu aktivieren und zu verwenden:

- [Voraussetzungen](#prerequisites)
- [Installieren des Azure Policy-Add-On](#installing-the-add-on)
- [Zuweisen einer Richtliniendefinition für die AKS-Engine](#built-in-policies)
- [Warten auf die Validierung](#validation-and-reporting-frequency)

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie das Azure Policy-Add-On installieren oder eines der Dienstfeatures aktivieren, muss in Ihrem Abonnement der **Microsoft.PolicyInsights**-Ressourcenanbieter aktiviert und eine Rollenzuweisung für den Clusterdienstprinzipal erstellt werden. 

1. Zum Aktivieren des Ressourcenanbieters führen Sie die Schritte unter [Ressourcenanbieter und -typen](../../../azure-resource-manager/resource-manager-supported-services.md#azure-portal) aus, oder führen Sie entweder den Azure CLI- oder Azure PowerShell-Befehl aus:

   - Azure-Befehlszeilenschnittstelle

     ```azurecli-interactive
     # Log in first with az login if you're not using Cloud Shell
     
     # Provider register: Register the Azure Policy provider
     az provider register --namespace 'Microsoft.PolicyInsights'
     ```

   - Azure PowerShell
   
     ```azurepowershell-interactive
     # Log in first with Connect-AzAccount if you're not using Cloud Shell
   
     # Provider register: Register the Azure Policy provider
     Register-AzResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
     ```

1. Erstellen Sie eine Rollenzuweisung für den Clusterdienstprinzipal.

   - Wenn Ihnen die App-ID des Clusterdienstprinzipals nicht bekannt ist, suchen Sie mit dem folgenden Befehl danach.

     ```bash
     # Get the kube-apiserver pod name
     kubectl get pods -n kube-system
   
     # Find the aadClientID value
     kubectl exec <kube-apiserver pod name> -n kube-system cat /etc/kubernetes/azure.json
     ```

   - Weisen Sie der App-ID des Clusterdienstprinzipals (Wert _aadClientID_ aus dem vorherigen Schritt) mit der Azure CLI die Rolle „Policy Insights Data Writer (Preview)“ zu. Ersetzen Sie `<subscriptionId>` durch Ihre Abonnement-ID und `<aks engine cluster resource group>` durch die Ressourcengruppe, in der sich der selbstverwaltete Kubernetes-Cluster der AKS-Engine befindet.

     ```azurecli-interactive
     az role assignment create --assignee <cluster service principal app ID> --scope "/subscriptions/<subscriptionId>/resourceGroups/<aks engine cluster resource group>" --role "Policy Insights Data Writer (Preview)"
     ```

## <a name="azure-policy-add-on"></a>Azure Policy-Add-On

Das _Azure Policy-Add-On_ für Kubernetes verbindet den Azure Policy-Dienst mit der Gatekeeper-Zugangssteuerung. Das Add-On, das in den Namespace _kube-system_ installiert wird, führt die folgenden Funktionen aus:

- Überprüfen auf Zuweisungen an den AKS-Engine-Cluster mit Azure Policy
- Herunterladen und Installieren von Richtliniendetails, Einschränkungsvorlagen und Einschränkungen
- Ausführen einer vollständigen Konformitätsprüfung für den AKS-Engine-Cluster
- Senden von Details zur Überwachung und Konformität an Azure Policy

### <a name="installing-the-add-on"></a>Installieren des Add-Ons

Sobald die Voraussetzungen erfüllt sind, kann das Azure Policy-Add-On installiert werden. Die Installation kann während des Erstellungs- oder Aktualisierungszyklus einer AKS-Engine oder als unabhängige Aktion auf einem vorhandenen Cluster erfolgen.

- Installation während des Erstellungs- oder Aktualisierungszyklus

  Um das Azure Policy Add-On während der Erstellung eines neuen selbstverwalteten Clusters oder als Update für einen vorhandenen Cluster zu aktivieren, schließen Sie die Eigenschaft **addons** in die Clusterdefinition für die AKS-Engine ein.

  ```json
  "addons": [{
      "name": "azure-policy",
      "enabled": true,
      "config": {
          "auditInterval": "30",
          "constraintViolationsLimit": "20"
      }
  }]
  ```

  Weitere Informationen dazu finden Sie im externen Leitfaden zur [Clusterdefinition für die AKS-Engine](https://github.com/Azure/aks-engine/blob/master/docs/topics/clusterdefinitions.md).

- Installation in vorhandenem Cluster mit Helm-Diagrammen

  Führen Sie die folgenden Schritte aus, um den Cluster vorzubereiten und das Add-On zu installieren:

  1. Installieren Sie Gatekeeper im Namespace _gatekeeper-system_.

     ```bash
     kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/gatekeeper/master/deploy/gatekeeper.yaml
     ```
  1. Fügen Sie das Label _control-plane_ zu _kube-system_ hinzu. Dieses Label schließt die Überwachung von _kube-system_-Pods und -Diensten durch Gatekeeper und das Azure Policy-Add-On aus.

     ```bash
     kubectl label namespaces kube-system control-plane=controller-manager
     ```

  1. Synchronisieren Sie Kubernetes-Daten (Namespace, Pod, Eingang, Dienst) mit OPA.

     ```bash
     kubectl apply -f https://raw.githubusercontent.com/Azure/azure-policy/master/built-in-references/Kubernetes/gatekeeper-opa-sync.yaml
     ```

     Weitere Informationen finden Sie unter [OPA – Replizieren von Daten](https://github.com/open-policy-agent/gatekeeper#replicating-data).

  1. Fügen Sie Helm das Azure Policy-Repository hinzu.

     ```bash
     helm repo add azure-policy https://raw.githubusercontent.com/Azure/azure-policy/master/extensions/policy-addon-kubernetes/helm-charts
     ```

     Weitere Informationen finden Sie in der [Schnellstartanleitung zum Helm-Diagramm](https://helm.sh/docs/using_helm/#quickstart-guide).

  1. Installieren Sie das Add-On mit einem Helm-Diagramm. Ersetzen Sie `<subscriptionId>` durch Ihre Abonnement-ID und `<aks engine cluster resource group>` durch die Ressourcengruppe, in der sich der selbstverwaltete Kubernetes-Cluster der AKS-Engine befindet.

     ```bash
     helm install azure-policy/azure-policy-addon-aks-engine --name azure-policy-addon --set azurepolicy.env.resourceid="/subscriptions/<subscriptionId>/resourceGroups/<aks engine cluster resource group>"
     ```

     Weitere Informationen dazu, was mit dem Helm-Diagramm des Add-Ons installiert wird, finden Sie in der [Definition des Helm-Diagramms für das Azure Policy Add-On](https://github.com/Azure/azure-policy/tree/master/extensions/policy-addon-kubernetes/helm-charts) auf GitHub.

     > [!NOTE]
     > Aufgrund der Beziehung zwischen Azure Policy Add-On und Ressourcengruppen-ID unterstützt Azure Policy nur einen AKS-Engine-Cluster für jede Ressourcengruppe.

Führen Sie den folgenden Befehl aus, um zu überprüfen, ob die Installation des Add-Ons erfolgreich war und der _azure-policy_-Pod ausgeführt wird:

```bash
kubectl get pods -n kube-system
```

### <a name="validation-and-reporting-frequency"></a>Häufigkeit der Prüfung und Berichterstellung

Das Add-On prüft alle fünf Minuten mit Azure Policy, ob sich die Richtlinienzuweisungen geändert haben. Während dieses Aktualisierungszyklus nimmt das Add-On eine Prüfung auf Änderungen vor. Diese Änderungen lösen das Erstellen, Aktualisieren oder Löschen der Einschränkungsvorlagen und Einschränkungen aus.

> [!NOTE]
> Obwohl ein _Clusteradministrator_ berechtigt sein kann, Änderungen an Einschränkungsvorlagen und Einschränkungen vorzunehmen, wird es nicht empfohlen oder unterstützt, Einschränkungsvorlagen oder Einschränkungen zu ändern, die von Azure Policy erstellt wurden. Alle manuellen Änderungen gehen während des Aktualisierungszyklus verloren.

Das Add-On fordert alle fünf Minuten einen vollständigen Scan des Clusters an. Nachdem Details des vollständigen Scans und alle Echtzeitauswertungen von versuchten Änderungen am Cluster durch Gatekeeper erfasst wurden, meldet das Add-On die Ergebnisse an Azure Policy zurück, um sie in [Konformitätsdetails](../how-to/get-compliance-data.md) wie bei allen Azure Policy-Zuweisungen aufzunehmen. Während des Prüfzyklus werden nur Ergebnisse für aktive Richtlinienzuweisungen zurückgegeben. Prüfungsergebnisse können auch als Verstöße angezeigt werden, die im Statusfeld der fehlerhaften Einschränkung aufgeführt sind.

## <a name="policy-language"></a>Richtliniensprache

Die Azure Policy-Sprachstruktur zum Verwalten der AKS-Engine folgt derjenigen der bestehenden Richtlinien. Die Auswirkung _EnforceOPAConstraint_ wird verwendet, um Ihre AKS-Engine-Cluster zu verwalten, und er übernimmt _details_-Eigenschaften, die für die Arbeit mit [OPA Constraint Framework](https://github.com/open-policy-agent/frameworks/tree/master/constraint) und Gatekeeper v3 spezifisch sind. Details und Beispiele finden Sie unter der Auswirkung [EnforceOPAConstraint](effects.md#enforceopaconstraint).

Als Teil der Eigenschaften _details.constraintTemplate_ und _details.constraint_ in der Richtliniendefinition übergibt Azure Policy die URIs dieser [CustomResourceDefinitions](https://github.com/open-policy-agent/gatekeeper#constraint-templates) (CRD) an das Add-On. Rego ist die Sprache, die von OPA und Gatekeeper unterstützt wird, um eine Anforderung an den Kubernetes-Cluster zu validieren. Durch die Unterstützung eines bestehenden Standards für das Kubernetes-Management ermöglicht Azure Policy die Wiederverwendung bestehender Regeln und deren Kombination mit Azure Policy für eine einheitliche Berichterstellungsumgebung zur Cloud-Compliance. Weitere Informationen finden Sie unter [Was ist Rego?](https://www.openpolicyagent.org/docs/how-do-i-write-policies.html#what-is-rego)

## <a name="built-in-policies"></a>Integrierte Richtlinien

Um die integrierten Richtlinien für die Verwaltung Ihres AKS-Engine-Clusters über das Azure-Portal zu finden, führen Sie die folgenden Schritte aus:

1. Starten Sie den Azure Policy-Dienst im Azure-Portal. Wählen Sie **Alle Dienste** im linken Bereich aus, suchen Sie dann nach der Option **Richtlinie** und wählen Sie sie aus.

1. Wählen Sie im linken Bereich der Seite „Azure Policy“ die Option **Definitionen** aus.

1. Verwenden Sie im Dropdown-Listenfeld „Kategorie“ die Option **Alle auswählen**, um den Filter zu löschen, und wählen Sie dann **Kubernetes** aus.

1. Wählen Sie die Richtliniendefinition und dann die Schaltfläche **Zuweisen** aus.

> [!NOTE]
> Bei der Zuweisung der Azure-Richtlinie für die AKS-Engine-Definition muss der **Bereich** die Ressourcengruppe des AKS-Engine-Clusters sein.

Verwenden Sie alternativ die Schnellstartanleitung [Zuweisen einer Richtlinie – Portal](../assign-policy-portal.md), um eine AKS-Engine-Richtlinie zu finden und zuzuweisen. Suchen Sie nach einer AKS-Engine-Richtliniendefinition anstelle des Musters „audit vms“.

> [!IMPORTANT]
> Integrierte Richtlinien in der Kategorie **Kubernetes** können nur mit der AKS-Engine verwendet werden.

## <a name="logging"></a>Protokollierung

### <a name="azure-policy-add-on-logs"></a>Protokolle des Azure Policy-Add-Ons

Als Kubernetes-Controller/-Container erstellt das Azure Policy-Add-On Protokolle im AKS-Engine-Cluster und sucht nach einer Kubernetes-Richtliniendefinition anstelle des Musters „audit vms“.

Zum Anzeigen der Protokolle des Azure Policy-Add-Ons verwenden Sie `kubectl`:

```bash
# Get the Azure Policy Add-on pod name
kubectl -n kube-system get pods -l app=azure-policy --output=name

# Get the logs for the add-on
kubectl logs <Azure Policy Add-on pod name> -n kube-system
```

### <a name="gatekeeper-logs"></a>Gatekeeper-Protokolle

Der Gatekeeper-Pod, _gatekeeper-controller-manager-0_, befindet sich normalerweise im Namespace `gatekeeper-system` oder `kube-system`, kann jedoch je nach Art der Bereitstellung auch in einem anderer Namespace enthalten sein.

Zum Anzeigen der Gatekeeper-Protokolle verwenden Sie `kubectl`:

```bash
NAMESPACE=<namespace of gatekeeper>
kubectl logs gatekeeper-controller-manager-0 -n $NAMESPACE
```

Weitere Informationen finden Sie unter [Debuggen von Gatekeeper](https://github.com/open-policy-agent/gatekeeper#debugging) in der OPA-Dokumentation.

## <a name="remove-the-add-on"></a>Entfernen des Add-Ons

Wenn Sie das Azure Policy Add-On und Gatekeeper aus dem AKS-Engine-Cluster entfernen möchten, verwenden Sie die Methode, die der Installationsweise des Add-Ons entspricht:

- Bei Installation durch Festlegen der **addons**-Eigenschaft in der Clusterdefinition für die AKS-Engine:

  Stellen Sie die Clusterdefinition erneut für die AKS-Engine bereit, nachdem Sie die **addons**-Eigenschaft für _azure-policy_ in „false“ geändert haben:


  ```json
  "addons": [{
      "name": "azure-policy",
      "enabled": false
  }]
  ```

- Bei Installation mit Helm-Diagrammen:

  1. Entfernen Sie alte Einschränkungen.

     Derzeit werden durch den Deinstallationsmechanismus nur das Gatekeeper-System entfernt. Weder Ressourcen vom Typ _ConstraintTemplate_, _Constraint_ oder _Config_, die vom Benutzer erstellt wurden, noch die zugehörigen _CRDs_ werden entfernt.

     Beim Ausführen von Gatekeeper können unerwünschte Einschränkungen wie folgt entfernt werden:

     - Löschen aller Instanzen der Einschränkungsressource
     - Löschen der _ConstraintTemplate_-Ressource, wodurch automatisch die _CRD_ bereinigt werden sollte
     - Löschen der _Config_-Ressource, wodurch Finalizer für synchronisierte Ressourcen entfernt werden

  1. Deinstallieren Sie das Azure Policy-Add-On.
  
     ```bash
     helm del --purge azure-policy-addon
     ```

  1. Deinstallieren Sie Gatekeeper.
  
     ```bash
     kubectl delete -f https://raw.githubusercontent.com/open-policy-agent/gatekeeper/master/deploy/gatekeeper.yaml
     ```

## <a name="diagnostic-data-collected-by-azure-policy-add-on"></a>Vom Azure Policy-Add-On gesammelte Diagnosedaten

Das Azure Policy-Add-On für Kubernetes sammelt begrenzte Clusterdiagnosedaten. Diese Diagnosedaten sind wichtige technische Daten in Bezug auf Software und Leistung. Sie werden für folgende Zwecke verwendet:

- Azure Policy Add-On auf dem neuesten Stand halten
- Azure Policy Add-On sicher, zuverlässig und leistungsfähig halten
- Azure Policy Add-On verbessern – durch die aggregierte Analyse der Verwendung des Add-Ons

Die vom Add-On gesammelten Informationen sind keine persönlichen Daten. Die folgenden Details werden derzeit gesammelt:

- Version des Azure Policy-Add-On-Agents
- Clustertyp
- Clusterregion
- Clusterressourcengruppe
- Clusterressourcen-ID
- Clusterabonnement-ID
- Clusterbetriebssystem (Beispiel: Linux)
- Ort für den Cluster (Beispiel: Seattle)
- Bundesland oder Kanton für den Cluster (Beispiel: Washington)
- Land oder Region für den Cluster (Beispiel: USA)
- Ausnahmen/Fehler, die während der Installation des Agents bei der Richtlinienauswertung durch das Azure Policy Add-On festgestellt werden
- Anzahl von Gatekeeper-Richtlinien, die nicht vom Azure Policy Add-On installiert werden

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die Beispiele unter [Azure Policy-Beispiele](../samples/index.md) an.
- Befassen Sie sich mit der [Struktur von Azure Policy-Definitionen](definition-structure.md).
- Lesen Sie [Grundlegendes zu Richtlinienauswirkungen](effects.md).
- Informieren Sie sich über das [programmgesteuerte Erstellen von Richtlinien](../how-to/programmatically-create.md).
- Informieren Sie sich über das [Abrufen von Konformitätsdaten](../how-to/getting-compliance-data.md).
- Erfahren Sie, wie Sie [nicht konforme Ressourcen korrigieren](../how-to/remediate-resources.md) können.
- Weitere Informationen zu Verwaltungsgruppen finden Sie unter [Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen](../../management-groups/overview.md).
---
title: Informationen zu Azure Policy für Azure Kubernetes Service
description: Erfahren Sie, wie Azure Policy Rego und Open Policy Agent verwendet, um Cluster im Azure Kubernetes Service zu verwalten.
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/24/2019
ms.topic: conceptual
ms.service: azure-policy
ms.openlocfilehash: 6a3d1fb347819015887ffc4fd8089bbc1f3a70de
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73176318"
---
# <a name="understand-azure-policy-for-azure-kubernetes-service"></a>Grundlegendes zu Azure Policy für Azure Kubernetes Service

Mit der Integration von Azure Policy mit [Azure Kubernetes Service](../../../aks/intro-kubernetes.md) (AKS) können Schutz- und Sicherheitsmaßnahmen in großem Umfang zentral und einheitlich auf Ihre Cluster angewendet werden.
Durch die Erweiterung der Verwendung von [GateKeeper](https://github.com/open-policy-agent/gatekeeper), einem _Webhook für die Zugangssteuerung_ für [Open Policy Agent](https://www.openpolicyagent.org/) (OPA), ermöglicht es Azure Policy, den Konformitätsstatus Ihrer Azure-Ressourcen und AKS-Cluster von einer zentralen Stelle aus zu verwalten und zu melden.

> [!NOTE]
> Azure Policy für AKS befindet sich in der eingeschränkten Vorschauversion und unterstützt nur integrierte Richtliniendefinitionen.

## <a name="overview"></a>Übersicht

Führen Sie die folgenden Schritte aus, um Azure Policy für AKS mit Ihrem AKS-Cluster zu aktivieren und zu verwenden:

- [Aktivieren der Previewfunktionen](#opt-in-for-preview)
- [Installieren des Azure Policy-Add-On](#installation-steps)
- [Zuweisen einer Richtliniendefinition für AKS](#built-in-policies)
- [Warten auf die Validierung](#validation-and-reporting-frequency)

## <a name="opt-in-for-preview"></a>Aktivieren der Vorschauversion

Bevor Sie das Azure Policy-Add-On installieren oder eines der Dienstfeatures aktivieren, muss Ihr Abonnement den **Microsoft.ContainerService**- und den **Microsoft.PolicyInsights**-Ressourcenanbieter aktivieren und dann für die Teilnahme an der Vorschau genehmigt werden. Um an der Vorschauversion teilzunehmen, führen Sie diese Schritte entweder im Azure-Portal oder mit der Azure CLI durch:

- Azure-Portal:

  1. Registrieren Sie die Ressourcenanbieter **Microsoft.ContainerService** und **Microsoft.PolicyInsights**. Weitere Informationen finden Sie unter [Ressourcenanbieter und -typen](../../../azure-resource-manager/resource-manager-supported-services.md#azure-portal).

  1. Starten Sie den Azure Policy-Dienst über das Azure-Portal, indem Sie auf **Alle Dienste** klicken und dann nach **Richtlinie** suchen und die entsprechende Option auswählen.

     ![Suchen nach „Policy“ unter „Alle Dienste“](../media/rego-for-aks/search-policy.png)

  1. Wählen Sie links auf der Seite „Azure Policy“ die Option zum **Beitreten zur Vorschauversion** aus.

     ![Beitreten zur Richtlinie für die AKS-Vorschauversion](../media/rego-for-aks/join-aks-preview.png)

  1. Wählen Sie die Zeile des Abonnements aus, das Sie der Vorschauversion hinzufügen möchten.

  1. Wählen Sie die Schaltfläche **Opt-in** (Abonnieren) oben in der Abonnementliste aus.

- Azure-Befehlszeilenschnittstelle:

  ```azurecli-interactive
  # Log in first with az login if you're not using Cloud Shell

  # Provider register: Register the Azure Kubernetes Services provider
  az provider register --namespace Microsoft.ContainerService

  # Provider register: Register the Azure Policy provider
  az provider register --namespace Microsoft.PolicyInsights

  # Feature register: enables installing the add-on
  az feature register --namespace Microsoft.ContainerService --name AKS-AzurePolicyAutoApprove
  
  # Use the following to confirm the feature has registered
  az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKS-AzurePolicyAutoApprove')].{Name:name,State:properties.state}"
  
  # Once the above shows 'Registered' run the following to propagate the update
  az provider register -n Microsoft.ContainerService
  
  # Feature register: enables the add-on to call the Azure Policy resource provider
  az feature register --namespace Microsoft.PolicyInsights --name AKS-DataplaneAutoApprove
  
  # Use the following to confirm the feature has registered
  az feature list -o table --query "[?contains(name, 'Microsoft.PolicyInsights/AKS-DataPlaneAutoApprove')].{Name:name,State:properties.state}"
  
  # Once the above shows 'Registered' run the following to propagate the update
  az provider register -n Microsoft.PolicyInsights
  
  ```

## <a name="azure-policy-add-on"></a>Azure Policy-Add-On

Das _Azure Policy-Add-On_ für Kubernetes verbindet den Azure Policy-Dienst mit der GateKeeper-Zugangssteuerung. Das Add-On, das in den Namespace _azure-policy_ installiert wird, führt die folgenden Funktionen aus:

- Überprüfen auf Zuweisungen an den AKS-Cluster mit Azure Policy
- Herunterladen und Zwischenspeichern von Richtliniendetails, einschließlich der _rego_-Richtliniendefinition, als **configmaps**
- Ausführen einer vollständigen Konformitätsprüfung für den AKS-Cluster
- Senden von Details zur Überwachung und Konformität an Azure Policy

### <a name="installing-the-add-on"></a>Installieren des Add-Ons

#### <a name="prerequisites"></a>Voraussetzungen

Bevor Sie das Add-On in Ihrem AKS-Cluster installieren, muss die Vorschauerweiterung installiert sein. Dieser Schritt erfolgt mit der Azure CLI:

1. Azure CLI-Version 2.0.62 oder höher muss installiert und konfiguriert sein. Führen Sie `az --version` aus, um die Version zu finden. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli).

1. Der AKS-Cluster muss die Version _1.10_ oder höher aufweisen. Verwenden Sie das folgende Skript, um Ihre AKS-Clusterversion zu überprüfen:

   ```azurecli-interactive
   # Log in first with az login if you're not using Cloud Shell

   # Look for the value in kubernetesVersion
   az aks list
   ```

1. Installieren Sie die Version _0.4.0_ der Azure CLI-Vorschauerweiterung für AKS, `aks-preview`:

   ```azurecli-interactive
   # Log in first with az login if you're not using Cloud Shell

   # Install/update the preview extension
   az extension add --name aks-preview

   # Validate the version of the preview extension
   az extension show --name aks-preview --query [version]
   ```

   > [!NOTE]
   > Wenn Sie zuvor die Erweiterung _aks-preview_ installiert haben, installieren Sie alle Updates mit dem Befehl `az extension update --name aks-preview`.

#### <a name="installation-steps"></a>Installationsschritte

Nachdem die Voraussetzungen erfüllt sind, installieren Sie das Azure Policy-Add-On in dem zu verwaltenden AKS-Cluster.

- Azure-Portal

  1. Starten Sie den AKS-Dienst im Azure-Portal, indem Sie auf **Alle Dienste** klicken und dann nach **Kubernetes-Dienste** suchen und die entsprechende Option auswählen.

  1. Wählen Sie einen Ihrer AKS-Cluster aus.

  1. Wählen Sie links **Richtlinien (Vorschauversion)** auf der Seite des Kubernetes-Diensts aus.

     ![Richtlinien des AKS-Clusters](../media/rego-for-aks/policies-preview-from-aks-cluster.png)

  1. Wählen Sie auf der Hauptseite die Schaltfläche **Add-On aktivieren** aus.

     ![Aktivieren des Azure Policy für AKS-Add-Ons](../media/rego-for-aks/enable-policy-add-on.png)

     > [!NOTE]
     > Wenn die Schaltfläche **Add-On aktivieren** abgeblendet ist, wurde das Abonnement noch nicht zur Vorschauversion hinzugefügt. Die erforderlichen Schritte finden Sie unter [Aktivieren der Vorschauversion](#opt-in-for-preview).

- Azure-Befehlszeilenschnittstelle

  ```azurecli-interactive
  # Log in first with az login if you're not using Cloud Shell

  az aks enable-addons --addons azure-policy --name MyAKSCluster --resource-group MyResourceGroup
  ```

### <a name="validation-and-reporting-frequency"></a>Häufigkeit der Prüfung und Berichterstellung

Das Add-On prüft alle fünf Minuten mit Azure Policy, ob sich die Richtlinienzuweisungen geändert haben. Während dieses Aktualisierungszyklus entfernt das Add-On alle _configmaps_ im Namespace _azure-policy_ und erstellt die _configmaps_ für die Verwendung mit GateKeeper neu.

> [!NOTE]
> Obwohl ein _Clusteradministrator_ möglicherweise über die Berechtigung für den Namespace _azure-policy_ verfügt, wird es nicht empfohlen oder unterstützt, Änderungen am Namespace vorzunehmen. Alle manuellen Änderungen gehen während des Aktualisierungszyklus verloren.

Das Add-On fordert alle fünf Minuten einen vollständigen Scan des Clusters an. Nachdem Details des vollständigen Scans und alle Echtzeitauswertungen von versuchten Änderungen am Cluster durch GateKeeper erfasst wurden, meldet das Add-On die Ergebnisse an Azure Policy zurück, um sie in [Konformitätsdetails](../how-to/get-compliance-data.md) wie bei allen Azure Policy-Zuweisungen aufzunehmen. Während des Prüfzyklus werden nur Ergebnisse für aktive Richtlinienzuweisungen zurückgegeben.

## <a name="policy-language"></a>Richtliniensprache

Die Azure Policy-Sprachstruktur zum Verwalten von AKS folgt derjenigen der bestehenden Richtlinien. Der Effekt _EnforceRegoPolicy_ wird verwendet, um Ihre AKS-Cluster zu verwalten und er übernimmt _details_-Eigenschaften, die für die Arbeit mit OPA und GateKeeper spezifisch sind. Details und Beispiele finden Sie unter dem Effekt [EnforceRegoPolicy](effects.md#enforceregopolicy).

Als Teil der Eigenschaft _details.policy_ in der Richtliniendefinition übergibt Azure Policy den URI einer Rego-Richtlinie an das Add-On. Rego ist die Sprache, die von OPA und GateKeeper unterstützt wird, um eine Anforderung an den Kubernetes-Cluster zu validieren oder zu ändern. Durch die Unterstützung eines bestehenden Standards für das Kubernetes-Management ermöglicht Azure Policy die Wiederverwendung bestehender Regeln und deren Kombination mit Azure Policy für eine einheitliche Berichterstellungsumgebung zur Cloud-Compliance. Weitere Informationen finden Sie unter [Was ist Rego?](https://www.openpolicyagent.org/docs/latest/policy-language/#what-is-rego)

## <a name="built-in-policies"></a>Integrierte Richtlinien

Um die integrierten Richtlinien für die Verwaltung von AKS über das Azure-Portal zu finden, führen Sie die folgenden Schritte aus:

1. Starten Sie den Azure Policy-Dienst im Azure-Portal. Wählen Sie **Alle Dienste** im linken Bereich aus, suchen Sie dann nach der Option **Richtlinie** und wählen Sie sie aus.

1. Wählen Sie im linken Bereich der Seite „Azure Policy“ die Option **Definitionen** aus.

1. Verwenden Sie im Dropdown-Listenfeld „Kategorie“ die Option **Alle auswählen**, um den Filter zu löschen, und wählen Sie dann **Kubernetes-Dienst** aus.

1. Wählen Sie die Richtliniendefinition und dann die Schaltfläche **Zuweisen** aus.

> [!NOTE]
> Bei der Zuweisung der Azure-Richtlinie für die AKS-Definition muss der **Bereich** die AKS-Clusterressource umfassen.

Verwenden Sie alternativ die Schnellstartanleitung [Zuweisen einer Richtlinie – Portal](../assign-policy-portal.md), um eine AKS-Richtlinie zu finden und zuzuweisen. Suchen Sie nach einer Kubernetes-Richtliniendefinition anstelle des Musters „audit vms“.

## <a name="logging"></a>Protokollierung

### <a name="azure-policy-add-on-logs"></a>Protokolle des Azure Policy-Add-Ons

Als Kubernetes-Controller/-Container erstellt das Azure Policy-Add-On Protokolle im AKS-Cluster und sucht nach einer Kubernetes-Richtliniendefinition anstelle des Musters „audit vms“. Die Protokolle werden auf der Seite **Insights** des AKS-Clusters angezeigt. Weitere Informationen finden Sie unter [Verstehen der Leistung von AKS-Clustern mit Azure Monitor für Container](../../../azure-monitor/insights/container-insights-analyze.md).

### <a name="gatekeeper-logs"></a>Gatekeeper-Protokolle

Führen Sie die Schritte unter [Aktivieren und Prüfen von Kubernetes-Masterknotenprotokollen in AKS](../../../aks/view-master-logs.md) aus, um GateKeeper-Protokolle für neue Ressourcenanforderungen zu aktivieren.
Hier folgt eine Beispielabfrage, um abgelehnte Ereignisse bei neuen Ressourcenanforderungen anzuzeigen:

```kusto
| where Category == "kube-audit"
| where log_s contains "admission webhook"
| limit 100
```

Um Protokolle aus GateKeeper-Containern anzuzeigen, führen Sie die Schritte im Abschnitt [Aktivieren und Prüfen von Kubernetes-Masterknotenprotokollen in AKS](../../../aks/view-master-logs.md) aus, und aktivieren Sie die Option _kube-apiserver_ im Abschnitt **Diagnoseeinstellungen**.

## <a name="remove-the-add-on"></a>Entfernen des Add-Ons

Verwenden Sie zum Entfernen des Azure Policy-Add-Ons aus Ihrem AKS-Cluster entweder das Azure-Portal oder die Azure CLI:

- Azure-Portal

  1. Starten Sie den AKS-Dienst im Azure-Portal, indem Sie auf **Alle Dienste** klicken und dann nach **Kubernetes-Dienste** suchen und die entsprechende Option auswählen.

  1. Wählen Sie Ihren AKS-Cluster aus, in dem Sie das Azure Policy-Add-On deaktivieren möchten.

  1. Wählen Sie links **Richtlinien (Vorschauversion)** auf der Seite des Kubernetes-Diensts aus.

     ![Richtlinien des AKS-Clusters](../media/rego-for-aks/policies-preview-from-aks-cluster.png)

  1. Wählen Sie auf der Hauptseite die Schaltfläche **Add-On deaktivieren** aus.

     ![Deaktivieren des Azure Policy für AKS-Add-Ons](../media/rego-for-aks/disable-policy-add-on.png)

- Azure-Befehlszeilenschnittstelle

  ```azurecli-interactive
  # Log in first with az login if you're not using Cloud Shell

  az aks disable-addons --addons azure-policy --name MyAKSCluster --resource-group MyResourceGroup
  ```

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die Beispiele unter [Azure Policy-Beispiele](../samples/index.md) an.
- Befassen Sie sich mit der [Struktur von Azure Policy-Definitionen](definition-structure.md).
- Lesen Sie [Grundlegendes zu Richtlinienauswirkungen](effects.md).
- Informieren Sie sich über das [programmgesteuerte Erstellen von Richtlinien](../how-to/programmatically-create.md).
- Informieren Sie sich über das [Abrufen von Konformitätsdaten](../how-to/getting-compliance-data.md).
- Erfahren Sie, wie Sie [nicht konforme Ressourcen korrigieren](../how-to/remediate-resources.md) können.
- Weitere Informationen zu Verwaltungsgruppen finden Sie unter [Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen](../../management-groups/overview.md).

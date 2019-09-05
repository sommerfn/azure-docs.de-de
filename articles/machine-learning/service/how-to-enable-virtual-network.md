---
title: Sichern von Experimenten und Rückschlüssen in einem virtuellen Netzwerk
titleSuffix: Azure Machine Learning service
description: Hier erfahren Sie, wie Sie Experiment-/Trainingsaufträge und Rückschluss-/Bewertungsaufträge in Azure Machine Learning innerhalb eines virtuellen Azure-Netzwerks sichern können.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 08/05/2019
ms.openlocfilehash: 6e5ae4966a62c24594ec6efa9454d5e03f75c25b
ms.sourcegitcommit: 47b00a15ef112c8b513046c668a33e20fd3b3119
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2019
ms.locfileid: "69971540"
---
# <a name="secure-azure-ml-experimentation-and-inference-jobs-within-an-azure-virtual-network"></a>Sichern von Azure ML-Experiment- und Rückschlussaufträgen in einem virtuellen Azure-Netzwerk

In diesem Artikel erfahren Sie, wie Sie Experiment-/Trainingsaufträge und Rückschluss-/Bewertungsaufträge in Azure Machine Learning innerhalb eines virtuellen Azure-Netzwerks (VNET) sichern können.

Ein **virtuelles Netzwerk** fungiert als Sicherheitsgrenze und schirmt Ihre Azure-Ressourcen vom öffentlichen Internet ab. Sie können ein virtuelles Azure-Netzwerk auch in Ihr lokales Netzwerk einbinden. Durch die Einbindung in Netzwerke können Sie auf sichere Weise Ihre Modelle trainieren und auf Ihre bereitgestellten Modelle zugreifen, um Rückschlüsse zu ziehen.

Azure Machine Learning Service ist in Sachen Computeressourcen auf andere Azure-Dienste angewiesen. Computeressourcen bzw. [Computeziele](concept-compute-target.md) dienen zum Trainieren und Bereitstellen von Modellen. Die Ziele können innerhalb eines virtuellen Netzwerks erstellt werden. So können Sie beispielsweise Microsoft Data Science Virtual Machine verwenden, um ein Modell zu trainieren, und das Modell anschließend in Azure Kubernetes Service (AKS) bereitstellen. Weitere Informationen über virtuelle Netzwerke erhalten Sie unter [Überblick über virtuelle Azure-Netzwerke](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).

Dieser Artikel enthält außerdem ausführliche Informationen zu *erweiterten Sicherheitseinstellungen*, also Informationen, die für einfache oder experimentelle Anwendungsfälle nicht erforderlich sind. Bestimmte Abschnitte dieses Artikels bieten Konfigurationsinformationen für eine Vielzahl von Szenarien. Sie müssen die Anweisungen nicht in der vorgegebenen Reihenfolge oder in ihrer Gesamtheit ausführen.

## <a name="prerequisites"></a>Voraussetzungen

+ Ein Azure Machine Learning Service-[Arbeitsbereich](how-to-manage-workspace.md)

+ Allgemeine Kenntnisse zum [Azure Virtual Network-Dienst](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) und [IP-Netzwerken](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm)

+ Ein bereits vorhandenes virtuelles Netzwerk und Subnetz, das mit Ihren Computeressourcen verwendet werden kann

## <a name="use-a-storage-account-for-your-workspace"></a>Verwenden eines Speicherkontos für Ihren Arbeitsbereich

Führen Sie die folgenden Schritte aus, um ein Azure-Speicherkonto für den Arbeitsbereich in einem virtuellen Netzwerk zu verwenden:

1. Erstellen Sie eine Experimentier-Computeinstanz (z. B. eine Machine Learning Compute-Instanz) hinter einem virtuellen Netzwerk, oder fügen Sie eine Experimentier-Computinstanz an den Arbeitsbereich an (z.Bb. einen HDInsight-Cluster oder einen virtuellen Computer).

   Weitere Informationen finden Sie in den Abschnitten „Verwenden einer Machine Learning Compute-Instanz“ und „Verwenden eines virtuellen Computers oder eines HDInsight-Clusters“ in diesem Artikel.

1. Wechseln Sie im Azure-Portal zu dem Speicher, der an Ihren Arbeitsbereich angefügt ist.

   [![Der Speicher, der an den Azure Machine Learning Service-Arbeitsbereich angefügt ist](./media/how-to-enable-virtual-network/workspace-storage.png)](./media/how-to-enable-virtual-network/workspace-storage.png#lightbox)

1. Wählen Sie auf der Seite **Azure Storage** die Option __Firewalls und virtuelle Netzwerke__ aus.

   ![Der Bereich „Firewalls und virtuelle Netzwerke“ auf der Seite „Azure Storage“ im Azure-Portal.](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks.png)

1. Führen Sie auf der Seite __Firewalls und virtuelle Netzwerke__ Folgendes aus:
    - Klicken Sie auf __Ausgewählte Netzwerke__.
    - Wählen Sie unter __Virtuelle Netzwerke__ den Link __Vorhandenes virtuelles Netzwerk hinzufügen__ aus. Durch diese Aktion wird das virtuelle Netzwerk an dem Ort hinzugefügt, wo sich Ihre Experimentier-Computeinstanz befindet (siehe Schritt 1).
    - Aktivieren Sie das Kontrollkästchen __Vertrauenswürdigen Microsoft-Diensten den Zugriff auf dieses Speicherkonto erlauben__.

    > [!IMPORTANT]
    > Wenn Sie mit dem Azure Machine Learning SDK arbeiten, muss Ihre Entwicklungsumgebung eine Verbindung mit dem Azure Storage-Konto herstellen können. Wenn sich das Speicherkonto innerhalb eines virtuellen Netzwerks befindet, muss die Firewall den Zugriff über die IP-Adresse der Entwicklungsumgebung zulassen.
    >
    > Um den Zugriff auf das Speicherkonto zu ermöglichen, öffnen Sie die __Firewalls und virtuellen Netzwerke__ für das Speicherkonto *über einen Webbrowser auf dem Entwicklungsclient*. Fügen Sie dann mithilfe des Kontrollkästchens __Client-IP-Adresse hinzufügen__ die IP-Adresse Ihres Clients dem __ADRESSBEREICH__ hinzu. Sie können die IP-Adresse der Entwicklungsumgebung im Feld __ADRESSBEREICH__ auch manuell eingeben. Nachdem die IP-Adresse für den Client hinzugefügt wurde, kann er mit dem SDK auf das Speicherkonto zugreifen.

   [![Der Bereich „Firewalls und virtuelle Netzwerke“ im Azure-Portal](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks-page.png)](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks-page.png#lightbox)

1. Ändern Sie während der Ausführung des Experiments in Ihrem Experimentiercode die Ausführungskonfiguration für die Verwendung von Azure Blob Storage:

    ```python
    run_config.source_directory_data_store = "workspaceblobstore"
    ```

> [!IMPORTANT]
> Sie können das _Standardspeicherkonto_ für den Azure Machine Learning Service kann _nur zum Experimentieren_ in einem virtuellen Netzwerk platzieren. Das Standardspeicherkonto wird automatisch bereitgestellt, wenn Sie einen Arbeitsbereich erstellen.
>
> Sie können _Nicht-Standardspeicherkonten_ _nur zum Experimentieren_ in einem virtuellen Netzwerk platzieren. Mit dem `storage_account`-Parameter in der [`Workspace.create()`-Funktion](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#create-name--auth-none--subscription-id-none--resource-group-none--location-none--create-resource-group-true--friendly-name-none--storage-account-none--key-vault-none--app-insights-none--container-registry-none--default-cpu-compute-target-none--default-gpu-compute-target-none--exist-ok-false--show-output-true-) können Sie ein benutzerdefiniertes Speicherkonto über die Azure-Ressourcen-ID angeben.
>
> Sowohl die standardmäßigen als auch die nicht standardmäßigen Speicherkonten, die für _Rückschlüsse_ verwendet werden, benötigen _uneingeschränkten Zugriff auf das Speicherkonto_.
>
> Sollten Sie nicht sicher sein, ob Sie diese Einstellungen geändert haben, sehen Sie sich den Abschnitt „Ändern der Standard-Netzwerkzugriffsregel“ in [Konfigurieren von Azure Storage-Firewalls und virtuellen Netzwerken](https://docs.microsoft.com/azure/storage/common/storage-network-security) an. Befolgen Sie die Anleitungen, um bei Rückschlüssen oder der Modellbewertung den Zugriff aus allen Netzwerken zuzulassen.

## <a name="use-a-key-vault-instance-with-your-workspace"></a>Verwenden einer Key Vault-Instanz mit Ihrem Arbeitsbereich

Die mit dem Arbeitsbereich verknüpfte Key Vault-Instanz wird von Azure Machine Learning Service zum Speichern der folgenden Anmeldeinformationen verwendet:
* Der verknüpften Speicherkonto-Verbindungszeichenfolge
* Kennwörter in Azure Container Repository-Instanzen
* Verbindungszeichenfolgen zur Verbindung mit Datenspeichern.

Um die Azure Machine Learning-Experimentierfunktionen mit Azure Key Vault hinter einem virtuellen Netzwerk zu verwenden, gehen Sie wie folgt vor:
1. Wechseln Sie zu dem Schlüsseltresor, der mit dem Arbeitsbereich verknüpft ist.

   [![Der Schlüsseltresor, der mit dem Azure Machine Learning Service-Arbeitsbereich verknüpft ist](./media/how-to-enable-virtual-network/workspace-key-vault.png)](./media/how-to-enable-virtual-network/workspace-key-vault.png#lightbox)

1. Wählen Sie auf der Seite **Key Vault** im linken Bereich __Firewalls und virtuelle Netzwerke__ aus.

   ![Der Abschnitt „Firewalls und virtuelle Netzwerke“ im Bereich „Key Vault“.](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks.png)

1. Führen Sie auf der Seite __Firewalls und virtuelle Netzwerke__ Folgendes aus:
    - Wählen Sie unter __Zugriff erlauben von__ den Eintrag __Ausgewählte Netzwerke__ aus.
    - Wählen Sie unter __Virtuelle Netzwerke__ die Option __Vorhandenes virtuelles Netzwerk hinzufügen__ aus, um das virtuelle Netzwerk hinzuzufügen, in dem sich Ihre Experimentier-Computerinstanz befindet.
    - Wählen Sie unter __Vertrauenswürdigen Microsoft-Diensten die Umgehung dieser Firewall erlauben__ die Option __Ja__ aus.

   [![Der Abschnitt „Firewalls und virtuelle Netzwerke“ im Bereich „Key Vault“](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks-page.png)](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks-page.png#lightbox)

## <a name="use-a-machine-learning-compute-instance"></a>Verwenden einer Machine Learning Compute-Instanz

Wenn Sie eine Azure Machine Learning Compute-Instanz in einem virtuellen Netzwerk verwenden möchten, müssen die folgenden Netzwerkanforderungen erfüllt sein:

> [!div class="checklist"]
> * Das virtuelle Netzwerk muss sich im gleichen Abonnement und in der gleichen Region befinden wie der Azure Machine Learning Service-Arbeitsbereich.
> * Das für den Computecluster angegebene Subnetz muss über genügend nicht zugewiesene IP-Adressen für die Anzahl virtueller Computer verfügen, die für den Cluster vorgesehen sind. Falls das Subnetz zu wenige nicht zugewiesene IP-Adressen hat, wird der Cluster teilweise zugeordnet.
> * Überprüfen Sie, ob Berechtigungen für die Verwaltung des virtuellen Netzwerks durch Ihre Sicherheitsrichtlinien oder -sperren für das Abonnement oder die Ressourcengruppe Ihres virtuellen Netzwerks eingeschränkt werden. Wenn Sie zum Schutz des virtuellen Netzwerks den Datenverkehr einschränken möchten, lassen Sie einige Ports für den Compute-Dienst geöffnet. Weitere Informationen finden Sie im Abschnitt [Erforderliche Ports](#mlcports).
> * Wenn Sie mehrere Computecluster in einem einzelnen virtuellen Netzwerk platzieren möchten, müssen Sie möglicherweise eine Kontingenterhöhung für eine oder mehrere Ihrer Ressourcen anfordern.

Die Machine Learning Compute-Instanz ordnet in der Ressourcengruppe, die das virtuelle Netzwerk enthält, automatisch zusätzliche Netzwerkressourcen zu. Für jeden Computecluster ordnet der Dienst folgende Ressourcen zu:

* Eine Netzwerksicherheitsgruppe
* Eine öffentliche IP-Adresse
* Ein Lastenausgleichsmodul

Diese Ressourcen werden durch die [Ressourcenkontingente](https://docs.microsoft.com/azure/azure-subscription-service-limits) des Abonnements beschränkt.

### <a id="mlcports"></a>Erforderliche Ports

Machine Learning Compute verwendet derzeit den Azure Batch-Dienst, um virtuelle Computer im angegebenen virtuellen Netzwerk bereitzustellen. Das Subnetz muss die eingehende Kommunikation des Batch-Diensts zulassen. Sie verwenden diese Kommunikation zur Planung der Ausführungen auf den Machine Learning Compute-Knoten sowie zur Kommunikation mit Azure Storage und anderen Ressourcen. Der Batch-Dienst fügt Netzwerksicherheitsgruppen (NSG) auf der Ebene der Netzwerkschnittstellen (NIC) hinzu, die an virtuelle Computer angefügt sind. Diese NSGs konfigurieren automatisch Eingangs- und Ausgangsregeln, um folgenden Datenverkehr zuzulassen:

- Eingehender TCP-Datenverkehr an den Ports 29876 und 29877 aus einem __Diensttag__ von __BatchNodeManagement__.

    ![Eine Regel für eingehenden Datenverkehr, die das BatchNodeManagement-Diensttag verwendet.](./media/how-to-enable-virtual-network/batchnodemanagement-service-tag.png)

- (Optional) Eingehender TCP-Datenverkehr an Port 22, um Remotezugriff zu ermöglichen. Verwenden Sie diesen Port nur, wenn Sie mithilfe von SSH über die öffentliche IP-Adresse eine Verbindung herstellen möchten.

- Ausgehender Datenverkehr an allen Ports zum virtuellen Netzwerk

- Ausgehender Datenverkehr an allen Ports zum Internet

Seien Sie vorsichtig, wenn Sie Eingangs- und Ausgangsregeln in von Batch konfigurierten NSGs ändern. Wenn eine NSG die Kommunikation mit den Computeknoten blockiert, legt der Compute-Dienst den Zustand der Computeknoten auf „Nicht verwendbar“ fest.

Sie müssen keine NSGs auf der Subnetzebene angeben, da der Azure Batch-Dienst eigene NSGs konfiguriert. Wenn das angegebene Subnetz allerdings über zugewiesene NSGs oder eine Firewall verfügt, konfigurieren Sie die Eingangs- und Ausgangssicherheitsregeln wie weiter oben erwähnt.

Die NSG-Regelkonfiguration im Azure-Portal wird in den folgenden Abbildungen dargestellt:

[![Die NSG-Eingangsregeln für Machine Learning Compute](./media/how-to-enable-virtual-network/amlcompute-virtual-network-inbound.png)](./media/how-to-enable-virtual-network/amlcompute-virtual-network-inbound.png#lightbox)

![Die NSG-Ausgangsregeln für Machine Learning Compute.](./media/how-to-enable-virtual-network/experimentation-virtual-network-outbound.png)

### <a id="limiting-outbound-from-vnet"></a> Beschränken ausgehender Verbindungen aus dem virtuellen Netzwerk

Wenn Sie die Standardausgangsregeln nicht verwenden möchten und den ausgehenden Zugriff auf Ihr virtuelles Netzwerk beschränken möchten, gehen Sie wie folgt vor:

- Verweigern Sie ausgehende Internetverbindungen mit NSG-Regeln.

- Begrenzen Sie ausgehenden Datenverkehr auf Folgendes:
   - Azure Storage, mit dem __Diensttag__ von __Storage.Region_Name__ (z. B. „Storage.EastUS“)
   - Azure Container Registry, mit dem __Diensttag__ von __AzureContainerRegistry.Region_Name__ (z. B. „AzureContainerRegistry.EastUS“)
   - Azure Machine Learning Service, mit dem __Diensttag__ von __AzureMachineLearning__

Die NSG-Regelkonfiguration im Azure-Portal wird in der folgenden Abbildung dargestellt:

[![Die NSG-Ausgangsregeln für Machine Learning Compute](./media/how-to-enable-virtual-network/limited-outbound-nsg-exp.png)](./media/how-to-enable-virtual-network/limited-outbound-nsg-exp.png#lightbox)

### <a name="user-defined-routes-for-forced-tunneling"></a>Benutzerdefinierte Routen für erzwungenes Tunneln

Wenn Sie das erzwungene Tunneling mit Azure Machine Learning Compute verwenden, fügen Sie dem Subnetz mit den Computeressourcen [benutzerdefinierte Routen (UDR)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) hinzu.

* Richten Sie für jede IP-Adresse eine benutzerdefinierte Route ein, die vom Azure Batch-Dienst in der Region Ihrer Ressourcen verwendet wird. Diese benutzerdefinierten Routen ermöglichen dem Batch-Dienst, für die zeitliche Planung von Tasks mit den Serverknoten zu kommunizieren. Mit den folgenden Verfahren können Sie eine Liste der IP-Adressen des Batch-Diensts abrufen:

    * Laden Sie die [Azure-IP-Adressbereiche und Diensttags](https://www.microsoft.com/download/details.aspx?id=56519) herunter und suchen Sie in der Datei nach `BatchNodeManagement.<region>`, wobei `<region>` Ihre Azure-Region ist.

    * Laden Sie die Informationen mithilfe der [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) herunter. Im folgenden Beispiel werden die IP-Adressinformationen heruntergeladen und die Informationen für die Region „USA, Osten 2“ herausgefiltert:

        ```azurecli-interactive
        az network list-service-tags -l "East US 2" --query "values[?starts_with(id, 'Batch')] | [?properties.region=='eastus2']"
        ```

* Ausgehender Datenverkehr an Azure Storage darf nicht durch Ihre lokale Netzwerkappliance blockiert werden. Insbesondere sind die URLs im Format `<account>.table.core.windows.net`, `<account>.queue.core.windows.net` und `<account>.blob.core.windows.net`.

Wenn Sie die benutzerdefinierten Routen hinzufügen, definieren Sie die Route für jedes zugehörige Batch-IP-Adresspräfix, und legen Sie den __Typ des nächsten Hops__ auf __Internet__ fest. Die folgende Abbildung zeigt ein Beispiel dieser benutzerdefinierten Route im Azure-Portal:

![Beispiel für eine benutzerdefinierte Route für ein Adresspräfix](./media/how-to-enable-virtual-network/user-defined-route.png)

Weitere Informationen finden Sie unter [Erstellen eines Azure Batch-Pools in einem virtuellen Netzwerk](../../batch/batch-virtual-network.md#user-defined-routes-for-forced-tunneling).

### <a name="create-a-machine-learning-compute-cluster-in-a-virtual-network"></a>Erstellen eines Machine Learning Compute-Clusters in einem virtuellen Netzwerk

Gehen Sie zum Erstellen eines Machine Learning Compute-Clusters wie folgt vor:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) Ihren Azure Machine Learning Service-Arbeitsbereich aus.

1. Wählen Sie im Abschnitt __Anwendung__ die Option __Compute__ und dann __Compute hinzufügen__ aus.

1. Gehen Sie zum Konfigurieren dieser Computeressource für die Verwendung eines virtuellen Netzwerks wie folgt vor:

    a. Wählen Sie für __Netzwerkkonfiguration__ den Wert __Erweitert__ aus.

    b. Wählen Sie in der Dropdownliste __Ressourcengruppe__ die Ressourcengruppe aus, die das virtuelle Netzwerk enthält.

    c. Wählen Sie in der Dropdownliste __Virtuelles Netzwerk__ das virtuelle Netzwerk aus, das das Subnetzwerk enthält.

    d. Wählen Sie in der Dropdownliste __Subnetz__ das zu verwendende Subnetz aus.

   ![Die virtuellen Netzerkeinstellungen für Machine Learning Compute](./media/how-to-enable-virtual-network/amlcompute-virtual-network-screen.png)

Ein Machine Learning Compute-Cluster kann auch mithilfe des Azure Machine Learning SDK erstellt werden. Der folgende Code erstellt einen neuen Machine Learning Compute-Cluster im Subnetz `default` eines virtuellen Netzwerks namens `mynetwork`:

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# The Azure virtual network name, subnet, and resource group
vnet_name = 'mynetwork'
subnet_name = 'default'
vnet_resourcegroup_name = 'mygroup'

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that cluster does not exist already
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print("Found existing cpucluster")
except ComputeTargetException:
    print("Creating new cpucluster")

    # Specify the configuration for the new cluster
    compute_config = AmlCompute.provisioning_configuration(vm_size="STANDARD_D2_V2",
                                                           min_nodes=0,
                                                           max_nodes=4,
                                                           vnet_resourcegroup_name=vnet_resourcegroup_name,
                                                           vnet_name=vnet_name,
                                                           subnet_name=subnet_name)

    # Create the cluster with the specified name and configuration
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

    # Wait for the cluster to be completed, show the output log
    cpu_cluster.wait_for_completion(show_output=True)
```

Nach Abschluss des Erstellungsprozesses trainieren Sie Ihr Modell, indem Sie den Cluster in einem Experiment verwenden. Weitere Informationen finden Sie unter [Auswählen und Verwenden eines Computeziels für das Training](how-to-set-up-training-targets.md).

## <a name="use-a-virtual-machine-or-hdinsight-cluster"></a>Verwenden eines virtuellen Computers oder HDInsight-Clusters

> [!IMPORTANT]
> Azure Machine Learning Service unterstützt nur virtuelle Computer, auf denen Ubuntu ausgeführt wird.

Gehen Sie wie folgt vor, um einen virtuellen Computer oder einen Azure HDInsight-Cluster in einem virtuellen Netzwerk mit Ihrem Arbeitsbereich zu verwenden:

1. Erstellen Sie einen virtuellen Computer oder einen HDInsight-Cluster mithilfe des Azure-Portals oder der Azure-Befehlszeilenschnittstelle, und platzieren Sie den Cluster in einem virtuellen Azure-Netzwerk. Weitere Informationen finden Sie in den folgenden Artikeln:
    * [Erstellen und Verwalten virtueller Azure-Netzwerke für virtuelle Linux-Computer](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)

    * [Erweitern von Azure HDInsight per Azure Virtual Network](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network)

1. Damit Azure Machine Learning Service mit dem SSH-Port des virtuellen Computers oder des Clusters kommunizieren kann, konfigurieren Sie einen Quelleintrag für die Netzwerksicherheitsgruppe. Der SSH-Port ist in der Regel der Port 22. Gehen Sie wie folgt vor, um Datenverkehr aus dieser Quelle zuzulassen:

    * Wählen Sie in der Dropdownliste __Quelle__ den Eintrag __Diensttag__ aus.

    * Wählen Sie in der Dropdownliste __Quelldiensttag__ den Eintrag __AzureMachineLearning__ aus.

    * Wählen Sie in der Dropdownliste __Quellportbereiche__ den Eintrag __*__ aus.

    * Wählen in der Dropdownliste __Ziel__ die Option __Alle__ aus.

    * Wählen Sie in der Dropdownliste __Quellportbereiche__ den Eintrag __22__ aus.

    * Wählen Sie unter __Protokoll__ die Option __Alle__ aus.

    * Wählen Sie für __Aktion__ die Option __Zulassen__ aus.

   ![Eingangsregeln für Experimente auf einem virtuellen Computer oder in einem HDInsight-Cluster innerhalb eines virtuellen Netzwerks](./media/how-to-enable-virtual-network/experimentation-virtual-network-inbound.png)

    Behalten Sie die Standardausgangsregeln für die Netzwerksicherheitsgruppe bei. Weitere Informationen finden Sie unter [Sicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules) bei den Standardsicherheitsregeln.

    Wenn Sie die Standardausgangsregeln nicht verwenden möchten und den ausgehenden Zugriff auf Ihr virtuelles Netzwerk beschränken möchten, finden Sie weitere Informationen im Abschnitt [Beschränken der ausgehenden Konnektivität aus dem virtuellen Netzwerk](#limiting-outbound-from-vnet).

1. Fügen Sie den virtuellen Computer oder den HDInsight-Cluster an Ihren Azure Machine Learning Service-Arbeitsbereich an. Weitere Informationen finden Sie unter [Einrichten von Computezielen für das Modelltraining](how-to-set-up-training-targets.md).

<a id="aksvnet"></a>

## <a name="use-azure-kubernetes-service-aks"></a>Verwenden von Azure Kubernetes Service (AKS)

Gehen Sie folgendermaßen vor, um AKS in einem virtuellen Netzwerk zu Ihrem Arbeitsbereich hinzuzufügen:

> [!IMPORTANT]
> Bevor Sie mit dem folgenden Verfahren beginnen, befolgen Sie die Anweisungen im Abschnitt „Voraussetzungen“ von [Konfigurieren von Azure CNI-Netzwerken in Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/configure-advanced-networking#prerequisites),und planen Sie die IP-Adressierung für Ihren Cluster.
>
> Die AKS-Instanz und das virtuelle Azure-Netzwerk müssen sich in derselben Region befinden.

1. Stellen Sie im [Azure-Portal](https://portal.azure.com) sicher, dass für die Netzwerksicherheitsgruppe (NSG), die das virtuelle Netzwerk steuert, eine Regel für eingehenden Datenverkehr für Azure Machine Learning Service unter Verwendung von __AzureMachineLearning__ als **QUELLE** (SOURCE) aktiviert ist.

    [![Der Bereich „Compute hinzufügen“ in Azure Machine Learning Service](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-aml.png)](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-aml.png#lightbox)

1. Wählen Sie Ihren Azure Machine Learning Service-Arbeitsbereich aus.

1. Wählen Sie im Abschnitt __Anwendung__ die Option __Compute__ und dann __Compute hinzufügen__ aus.

1. Gehen Sie zum Konfigurieren dieser Computeressource für die Verwendung eines virtuellen Netzwerks wie folgt vor:

    - Wählen Sie für __Netzwerkkonfiguration__ den Wert __Erweitert__ aus.

    - Wählen Sie in der Dropdownliste __Ressourcengruppe__ die Ressourcengruppe aus, die das virtuelle Netzwerk enthält.

    - Wählen Sie in der Dropdownliste __Virtuelles Netzwerk__ das virtuelle Netzwerk aus, das das Subnetzwerk enthält.

    - Wählen Sie in der Dropdownliste __Subnetz__ das Subnetz aus.

    - Geben Sie in das Feld __Kubernetes Service Address Range__ (Kubernetes-Dienstadressbereich) den Kubernetes-Dienstadressbereich ein. Dieser Adressbereich verwendet einen IP-Adressbereich in CIDR-Notation (Classless Inter-Domain Routing), um die für den Cluster verfügbaren IP-Adressen zu definieren. Er darf sich mit keinem IP-Adressbereich eines Subnetzes überschneiden (z. B. 10.0.0.0/16).

    - Geben Sie in das Feld __Kubernetes DNS Service IP Address__ (Kubernetes-DNS-Dienst-IP-Adresse) die Kubernetes-DNS-Dienst-IP-Adresse ein. Diese IP-Adresse wird dem Kubernetes-DNS-Dienst zugewiesen. Sie muss innerhalb des Kubernetes-Dienstadressbereichs liegen (z. B. 10.0.0.10).

    - Geben Sie in das Feld __Adresse der Docker-Brücke__ die Adresse der Docker-Brücke ein. Diese IP-Adresse wird der Docker-Brücke zugewiesen. Sie darf weder dem IP-Adressbereich eines Subnetzes noch dem Adressbereich des Kubernetes-Diensts angehören (z. B. 172.17.0.1/16).

   ![Azure Machine Learning Service: VNET-Einstellungen für Machine Learning Compute](./media/how-to-enable-virtual-network/aks-virtual-network-screen.png)

1. Stellen Sie sicher, dass für die Netzwerksicherheitsgruppe, die das virtuelle Netzwerk steuert, eine Sicherheitsregel für eingehenden Datenverkehr für den Bewertungsendpunkt aktiviert ist, damit sie von außerhalb des virtuellen Netzwerks aus aufgerufen werden kann.
   > [!IMPORTANT]
   > Behalten Sie die Standardausgangsregeln für die NSG bei. Weitere Informationen finden Sie unter [Sicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules) bei den Standardsicherheitsregeln.

   [![Eine Eingangssicherheitsregel](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-scoring.png)](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-scoring.png#lightbox)

Sie können auch das Azure Machine Learning SDK verwenden, um Azure Kubernetes Service einem virtuellen Netzwerk hinzuzufügen. Wenn Sie bereits über einen AKS-Cluster in einem virtuellen Netzwerk verfügen, können Sie ihn dem Arbeitsbereich anfügen (siehe [Bereitstellen von Modellen mit dem Azure Machine Learning-Dienst](how-to-deploy-to-aks.md)). Der folgende Code erstellt eine neue AKS-Instanz im Subnetz `default` eines virtuellen Netzwerks namens `mynetwork`:

```python
from azureml.core.compute import ComputeTarget, AksCompute

# Create the compute configuration and set virtual network information
config = AksCompute.provisioning_configuration(location="eastus2")
config.vnet_resourcegroup_name = "mygroup"
config.vnet_name = "mynetwork"
config.subnet_name = "default"
config.service_cidr = "10.0.0.0/16"
config.dns_service_ip = "10.0.0.10"
config.docker_bridge_cidr = "172.17.0.1/16"

# Create the compute target
aks_target = ComputeTarget.create(workspace=ws,
                                  name="myaks",
                                  provisioning_configuration=config)
```

Nach Abschluss des Erstellungsprozesses können Sie Rückschlüsse für einen AKS-Cluster hinter einem virtuellen Netzwerk ziehen oder das Modell bewerten. Weitere Informationen finden Sie unter [Bereitstellen im AKS](how-to-deploy-to-aks.md).

## <a name="next-steps"></a>Nächste Schritte

* [Einrichten von Computezielen für das Modelltraining](how-to-set-up-training-targets.md)
* [Deploy models with the Azure Machine Learning service](how-to-deploy-and-where.md) (Bereitstellen von Modellen mit dem Azure Machine Learning-Dienst)
* [Sicheres Bereitstellen von Modellen mit SSL](how-to-secure-web-service.md)


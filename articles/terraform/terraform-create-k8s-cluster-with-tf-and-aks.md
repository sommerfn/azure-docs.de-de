---
title: Erstellen eines Kubernetes-Clusters mit Azure Kubernetes Service (AKS) und Terraform
description: Tutorial, in dem das Erstellen eines Kubernetes-Clusters mit Azure Kubernetes Service und Terraform veranschaulicht wird
services: terraform
ms.service: azure
keywords: Terraform, DevOps, virtueller Computer, Azure, Kubernetes
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 12/04/2018
ms.openlocfilehash: 257a2d78a54e292faecda836811f0a58fabd584d
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2019
ms.locfileid: "68854499"
---
# <a name="create-a-kubernetes-cluster-with-azure-kubernetes-service-and-terraform"></a>Erstellen eines Kubernetes-Clusters mit Azure Kubernetes Service und Terraform
[Azure Kubernetes Service (AKS)](/azure/aks/) verwaltet Ihre gehostete Kubernetes-Umgebung und ermöglicht so die schnelle und einfache Bereitstellung und Verwaltung von Containeranwendungen – ganz ohne Kenntnisse im Zusammenhang mit Containerorchestrierung. Darüber hinaus übernimmt die Lösung die Betreuung laufender Vorgänge und Wartungsaufgaben für Sie, indem sie Ressourcen ganz nach Bedarf bereitstellt, aktualisiert und skaliert, ohne Ihre Anwendungen offline zu schalten.

In diesem Tutorial erfahren Sie, wie die folgenden Aufgaben bei der Erstellung eines [Kubernetes](https://www.redhat.com/en/topics/containers/what-is-kubernetes)-Clusters mit [Terraform](https://terraform.io) und AKS ausgeführt werden:

> [!div class="checklist"]
> * Verwenden von HCL (HashiCorp-Sprache) zum Definieren eines Kubernetes-Clusters
> * Verwenden von Terraform und AKS zum Erstellen eines Kubernetes-Clusters
> * Verwenden des kubectl-Tools zum Testen der Verfügbarkeit eines Kubernetes-Clusters

## <a name="prerequisites"></a>Voraussetzungen

- **Azure-Abonnement**: Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.

- **Konfigurieren von Terraform:** Befolgen Sie die Anweisungen im Artikel [Installieren und Konfigurieren von Terraform zum Bereitstellen von VMs und sonstiger Infrastruktur in Azure](/azure/virtual-machines/linux/terraform-install-configure).

- **Azure-Dienstprinzipal:** Befolgen Sie die Anweisungen im Abschnitt **Erstellen des Dienstprinzipals** des Artikels [Erstellen eines Azure-Dienstprinzipals mit der Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest). Notieren Sie sich die Werte für „appId“, „displayName“, „password“ und „tenant“.

## <a name="create-the-directory-structure"></a>Erstellen der Verzeichnisstruktur
Der erste Schritt ist das Erstellen des Verzeichnisses, das Ihre Terraform-Konfigurationsdateien für die Übung enthält.

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).

1. Öffnen Sie [Azure Cloud Shell](/azure/cloud-shell/overview). Falls Sie zuvor noch keine Umgebung ausgewählt haben, wählen Sie **Bash** als Umgebung aus.

    ![Cloud Shell-Eingabeaufforderung](./media/terraform-create-k8s-cluster-with-tf-and-aks/azure-portal-cloud-shell-button-min.png)

1. Wechseln Sie in das Verzeichnis `clouddrive`.

    ```bash
    cd clouddrive
    ```

1. Erstellen Sie ein Verzeichnis namens `terraform-aks-k8s`.

    ```bash
    mkdir terraform-aks-k8s
    ```

1. Wechseln Sie zum neuen Verzeichnis:

    ```bash
    cd terraform-aks-k8s
    ```

## <a name="declare-the-azure-provider"></a>Deklarieren des Azure-Anbieters
Erstellen Sie die Terraform-Konfigurationsdatei, die den Azure-Anbieter deklariert.

1. Erstellen Sie in Cloud Shell eine Datei namens `main.tf`.

    ```bash
    vi main.tf
    ```

1. Drücken Sie die I-TASTE, um den Einfügemodus zu starten.

1. Fügen Sie den folgenden Code in den Editor ein:

    ```JSON
    provider "azurerm" {
        version = "~>1.5"
    }

    terraform {
        backend "azurerm" {}
    }
    ```

1. Drücken Sie die **ESC**-TASTE, um den Einfügemodus zu beenden.

1. Speichern Sie die Datei, und geben Sie den folgenden Befehl ein, um den vi-Editor zu schließen:

    ```bash
    :wq
    ```

## <a name="define-a-kubernetes-cluster"></a>Definieren eines Kubernetes-Clusters
Erstellen Sie die Terraform-Konfigurationsdatei, die die Ressourcen für den Kubernetes-Cluster deklariert.

1. Erstellen Sie in Cloud Shell eine Datei namens `k8s.tf`.

    ```bash
    vi k8s.tf
    ```

1. Drücken Sie die I-TASTE, um den Einfügemodus zu starten.

1. Fügen Sie den folgenden Code in den Editor ein:

    ```JSON
    resource "azurerm_resource_group" "k8s" {
        name     = "${var.resource_group_name}"
        location = "${var.location}"
    }
    
    resource "random_id" "log_analytics_workspace_name_suffix" {
        byte_length = 8
    }

    resource "azurerm_log_analytics_workspace" "test" {
        # The WorkSpace name has to be unique across the whole of azure, not just the current subscription/tenant.
        name                = "${var.log_analytics_workspace_name}-${random_id.log_analytics_workspace_name_suffix.dec}"
        location            = "${var.log_analytics_workspace_location}"
        resource_group_name = "${azurerm_resource_group.k8s.name}"
        sku                 = "${var.log_analytics_workspace_sku}"
    }

    resource "azurerm_log_analytics_solution" "test" {
        solution_name         = "ContainerInsights"
        location              = "${azurerm_log_analytics_workspace.test.location}"
        resource_group_name   = "${azurerm_resource_group.k8s.name}"
        workspace_resource_id = "${azurerm_log_analytics_workspace.test.id}"
        workspace_name        = "${azurerm_log_analytics_workspace.test.name}"

        plan {
            publisher = "Microsoft"
            product   = "OMSGallery/ContainerInsights"
        }
    }

    resource "azurerm_kubernetes_cluster" "k8s" {
        name                = "${var.cluster_name}"
        location            = "${azurerm_resource_group.k8s.location}"
        resource_group_name = "${azurerm_resource_group.k8s.name}"
        dns_prefix          = "${var.dns_prefix}"

        linux_profile {
            admin_username = "ubuntu"

            ssh_key {
                key_data = "${file("${var.ssh_public_key}")}"
            }
        }

        agent_pool_profile {
            name            = "agentpool"
            count           = "${var.agent_count}"
            vm_size         = "Standard_DS1_v2"
            os_type         = "Linux"
            os_disk_size_gb = 30
        }

        service_principal {
            client_id     = "${var.client_id}"
            client_secret = "${var.client_secret}"
        }

        addon_profile {
            oms_agent {
            enabled                    = true
            log_analytics_workspace_id = "${azurerm_log_analytics_workspace.test.id}"
            }
        }

        tags = {
            Environment = "Development"
        }
    }
    ```

    Der obige Code legt den Namen des Clusters, den Speicherort und „resource_group_name“ fest. Darüber hinaus wird der dns_prefix-Wert festgelegt, der Teil des vollqualifizierten Domänennamens (FQDN) ist, der für Zugriff auf den Cluster verwendet wird.

    Mit dem Datensatz **linux_profile** können Sie die Einstellungen konfigurieren, die eine Anmeldung bei Workerknoten mithilfe von SSH ermöglichen.

    Mit AKS zahlen Sie nur für die Workerknoten. Der Datensatz **agent_pool_profile** konfiguriert die Details für diese Workerknoten. Der Datensatz **agent_pool_profile** enthält die Anzahl der zu erstellenden Workerknoten und den Typ der Workerknoten. Wenn Sie den Cluster in der Zukunft zentral hochskalieren oder herunterskalieren müssen, ändern Sie den Wert **count** in diesem Datensatz.

1. Drücken Sie die **ESC**-TASTE, um den Einfügemodus zu beenden.

1. Speichern Sie die Datei, und geben Sie den folgenden Befehl ein, um den vi-Editor zu schließen:

    ```bash
    :wq
    ```

## <a name="declare-the-variables"></a>Deklarieren der Variablen

1. Erstellen Sie in Cloud Shell eine Datei namens `variables.tf`.

    ```bash
    vi variables.tf
    ```

1. Drücken Sie die I-TASTE, um den Einfügemodus zu starten.

1. Fügen Sie den folgenden Code in den Editor ein:

    ```JSON
    variable "client_id" {}
    variable "client_secret" {}

    variable "agent_count" {
        default = 3
    }

    variable "ssh_public_key" {
        default = "~/.ssh/id_rsa.pub"
    }

    variable "dns_prefix" {
        default = "k8stest"
    }

    variable cluster_name {
        default = "k8stest"
    }

    variable resource_group_name {
        default = "azure-k8stest"
    }

    variable location {
        default = "Central US"
    }

    variable log_analytics_workspace_name {
        default = "testLogAnalyticsWorkspaceName"
    }

    # refer https://azure.microsoft.com/global-infrastructure/services/?products=monitor for log analytics available regions
    variable log_analytics_workspace_location {
        default = "eastus"
    }

   # refer https://azure.microsoft.com/pricing/details/monitor/ for log analytics pricing 
   variable log_analytics_workspace_sku {
        default = "PerGB2018"
   }
    ```

1. Drücken Sie die **ESC**-TASTE, um den Einfügemodus zu beenden.

1. Speichern Sie die Datei, und geben Sie den folgenden Befehl ein, um den vi-Editor zu schließen:

    ```bash
    :wq
    ```

## <a name="create-a-terraform-output-file"></a>Erstellen einer Terraform-Ausgabedatei
Mit [Terraform-Ausgaben](https://www.terraform.io/docs/configuration/outputs.html) können Sie Werte definieren, die für den Benutzer hervorgehoben werden, wenn Terraform einen Plan anwendet. Sie können mit dem Befehl `terraform output` abgefragt werden. In diesem Abschnitt erstellen Sie eine Ausgabedatei, die Zugriff auf den Cluster mit [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/) ermöglicht.

1. Erstellen Sie in Cloud Shell eine Datei namens `output.tf`.

    ```bash
    vi output.tf
    ```

1. Drücken Sie die I-TASTE, um den Einfügemodus zu starten.

1. Fügen Sie den folgenden Code in den Editor ein:

    ```JSON
    output "client_key" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.client_key}"
    }

    output "client_certificate" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.client_certificate}"
    }

    output "cluster_ca_certificate" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.cluster_ca_certificate}"
    }

    output "cluster_username" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.username}"
    }

    output "cluster_password" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.password}"
    }

    output "kube_config" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config_raw}"
    }

    output "host" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.host}"
    }
    ```

1. Drücken Sie die **ESC**-TASTE, um den Einfügemodus zu beenden.

1. Speichern Sie die Datei, und geben Sie den folgenden Befehl ein, um den vi-Editor zu schließen:

    ```bash
    :wq
    ```

## <a name="set-up-azure-storage-to-store-terraform-state"></a>Einrichten von Azure-Speicher zum Speichern des Terraform-Status
Terraform verfolgt den Status lokal über die Datei `terraform.tfstate`. Dieses Muster funktioniert gut in einer Umgebung mit nur einer Person. In einer zweckmäßigeren Umgebung mit mehreren Personen müssen Sie den Status allerdings auf dem Server mit [Azure-Speicher](/azure/storage/) nachverfolgen. In diesem Abschnitt rufen Sie die erforderlichen Informationen zum Speicherkonto (Kontoname und Kontoschlüssel) ab und erstellen einen Speichercontainer, in dem die Terraform-Statusinformationen gespeichert werden.

1. Wählen Sie im Azure-Portal im linken Menü **Alle Dienste** aus.

1. Wählen Sie **Speicherkonten** aus.

1. Wählen Sie auf der Registerkarte **Speicherkonten** den Namen des Speicherkontos aus, in dem Terraform den Status speichern soll. Beispielsweise können Sie das Speicherkonto verwenden, das erstellt wurde, als Sie Cloud Shell zum ersten Mal geöffnet haben.  Der Name des von Cloud Shell erstellten Speicherkontos beginnt in der Regel mit `cs` gefolgt von einer zufälligen Zeichenfolge aus Zahlen und Buchstaben. **Merken Sie sich den Namen des Speicherkontos, das Sie auswählen, da er später benötigt wird.**

1. Wählen Sie auf der Registerkarte des Speicherkontos **Zugriffsschlüssel** aus.

    ![Menü des Speicherkontos](./media/terraform-create-k8s-cluster-with-tf-and-aks/storage-account.png)

1. Notieren Sie sich den Wert des **Schlüssels** **key1**. (Wenn Sie das Symbol auf der rechten Seite des Schlüssels auswählen, wird der Wert in die Zwischenablage kopiert.)

    ![Speicherkonto-Zugriffsschlüssel](./media/terraform-create-k8s-cluster-with-tf-and-aks/storage-account-access-key.png)

1. Erstellen Sie in Cloud Shell einen Container in Ihrem Azure-Speicherkonto (ersetzen Sie die Platzhalter &lt;YourAzureStorageAccountName> und &lt;YourAzureStorageAccountAccessKey> durch die entsprechenden Werte für Ihr Azure-Speicherkonto).

    ```bash
    az storage container create -n tfstate --account-name <YourAzureStorageAccountName> --account-key <YourAzureStorageAccountKey>
    ```

## <a name="create-the-kubernetes-cluster"></a>Erstellen des Kubernetes-Clusters
In diesem Abschnitt erfahren Sie, wie der Befehl `terraform init` verwendet wird, um die Ressourcen zu erstellen, die in den Konfigurationsdateien definiert sind, die Sie in den vorherigen Abschnitten erstellt haben.

1. Initialisieren Sie in Cloud Shell Terraform (ersetzen Sie die Platzhalter &lt;YourAzureStorageAccountName> und &lt;YourAzureStorageAccountAccessKey> durch die entsprechenden Werte für Ihr Azure-Speicherkonto).

    ```bash
    terraform init -backend-config="storage_account_name=<YourAzureStorageAccountName>" -backend-config="container_name=tfstate" -backend-config="access_key=<YourStorageAccountAccessKey>" -backend-config="key=codelab.microsoft.tfstate" 
    ```
    
    Der Befehl `terraform init` zeigt den Erfolg der Initialisierung von Back-End- und -Anbieter-Plug-Ins an:

    ![Beispiel für Ergebnisse von „terraform init“](./media/terraform-create-k8s-cluster-with-tf-and-aks/terraform-init-complete.png)

1. Exportieren Sie die Anmeldeinformationen für den Dienstprinzipal. Ersetzen Sie die Platzhalter &lt;your-client-id> und &lt;your-client-secret> jeweils mit den **appId**- und **password**-Werten Ihres Dienstprinzipals.

    ```bash
    export TF_VAR_client_id=<your-client-id>
    export TF_VAR_client_secret=<your-client-secret>
    ```

1. Führen Sie den Befehl `terraform plan` aus, um den Terraform-Plan zu erstellen, der die Infrastrukturelemente definiert. 

    ```bash
    terraform plan -out out.plan
    ```

    Der Befehl `terraform plan` zeigt die Ressourcen an, die erstellt werden, wenn Sie den Befehl `terraform apply` ausführen:

    ![Beispiel für Ergebnisse von „terraform plan“](./media/terraform-create-k8s-cluster-with-tf-and-aks/terraform-plan-complete.png)

1. Führen Sie den Befehl `terraform apply` aus, um den Plan zum Erstellen des Kubernetes-Clusters anzuwenden. Der Prozess zum Erstellen eines Kubernetes-Clusters dauert einige Minuten. Dies kann zu einem Timeout der Cloud Shell-Sitzung führen. Wenn ein Timeout der Cloud Shell-Sitzung eintritt, können Sie die Schritte im Abschnitt „Wiederherstellen nach einem Cloud Shell-Timeout“ ausführen, um das Tutorial abzuschließen.

    ```bash
    terraform apply out.plan
    ```

    Der Befehl `terraform apply` zeigt die Ergebnisse der Erstellung der Ressourcen an, die in den Konfigurationsdateien definiert sind:

    ![Beispiel für Ergebnisse von „terraform apply“](./media/terraform-create-k8s-cluster-with-tf-and-aks/terraform-apply-complete.png)

1. Wählen Sie im Azure-Portal im linken Menü **Alle Dienste** aus, um die Ressourcen zu sehen, die für Ihren neuen Kubernetes-Cluster erstellt wurden.

    ![Cloud Shell-Eingabeaufforderung](./media/terraform-create-k8s-cluster-with-tf-and-aks/k8s-resources-created.png)

## <a name="recover-from-a-cloud-shell-timeout"></a>Wiederherstellen nach einem Cloud Shell-Timeout
Bei einem Timeout der Cloud Shell-Sitzung können Sie mit den folgenden Schritte eine Wiederherstellung erreichen:

1. Starten Sie eine Cloud Shell-Sitzung.

1. Wechseln Sie zu dem Verzeichnis, das Ihre Terraform Konfigurationsdateien enthält.

    ```bash
    cd /clouddrive/terraform-aks-k8s
    ```

1. Führen Sie den folgenden Befehl aus:

    ```bash
    export KUBECONFIG=./azurek8s
    ```
    
## <a name="test-the-kubernetes-cluster"></a>Testen des Kubernetes-Clusters
Die Kubernetes-Tools können verwendet werden, um den neu erstellten Cluster zu überprüfen.

1. Rufen Sie die Kubernetes-Konfiguration aus dem Terraform-Status ab, und speichern Sie sie in einer Datei, die kubectl lesen kann.

    ```bash
    echo "$(terraform output kube_config)" > ./azurek8s
    ```

1. Legen Sie eine Umgebungsvariable so fest, dass kubectl die richtige Konfiguration auswählt.

    ```bash
    export KUBECONFIG=./azurek8s
    ```

1. Überprüfen Sie die Integrität des Clusters.

    ```bash
    kubectl get nodes
    ```

    Sie sollten die Details der Workerknoten sehen, und sie sollten alle den Status **Ready** aufweisen, wie in der folgenden Abbildung dargestellt:

    ![Das kubectl-Tool ermöglicht die Überprüfung der Integrität des Kubernetes-Clusters](./media/terraform-create-k8s-cluster-with-tf-and-aks/kubectl-get-nodes.png)

## <a name="monitor-health-and-logs"></a>Überwachung von Integrität und Protokollen
Beim Erstellen des AKS-Clusters wurde die Überwachung aktiviert, um Integritätsmetriken für die Clusterknoten und die Pods zu erfassen. Diese Integritätsmetriken sind im Azure-Portal verfügbar. Weitere Informationen zur Überwachung der Integrität von Containern finden Sie unter [Überwachen der Integrität von Azure Kubernetes Service](https://docs.microsoft.com/azure/azure-monitor/insights/container-insights-overview).

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie Terraform und AKS zum Erstellen eines Kubernetes-Clusters verwenden. In folgenden zusätzlichen Ressourcen können Sie mehr über Terraform in Azure erfahren: 

 [Dokumentation zu Terraform in Azure](https://docs.microsoft.com/azure/terraform/)  
 [Terraform Azure Provider Documentation](https://aka.ms/terraform) (Dokumentation zum Azure-Anbieter für Terraform)  
 [Terraform Azure Provider Source](https://aka.ms/tfgit) (Referenz zum Azure-Anbieter für Terraform)  
 [Terraform Azure Modules](https://aka.ms/tfmodules) (Azure-Module für Terraform)

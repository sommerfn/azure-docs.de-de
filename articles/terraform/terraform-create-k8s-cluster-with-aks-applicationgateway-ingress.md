---
title: Erstellen eines Kubernetes-Clusters mit Application Gateway als Eingangscontroller mit Azure Kubernetes Service (AKS)
description: Tutorial, in dem das Erstellen eines Kubernetes-Clusters mit Azure Kubernetes Service und Application Gateway als Eingangscontroller veranschaulicht wird
services: terraform
ms.service: azure
keywords: Terraform, DevOps, virtueller Computer, Azure, Kubernetes, eingehend, Application Gateway
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/20/2019
ms.openlocfilehash: 0373b254a900fd34232bb6863c93802fa7b51aab
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71169951"
---
# <a name="create-a-kubernetes-cluster-with-application-gateway-ingress-controller-using-azure-kubernetes-service-and-terraform"></a>Erstellen eines Kubernetes-Clusters mit Application Gateway-Eingangscontroller unter Verwendung von Azure Kubernetes Service und Terraform
[Azure Kubernetes Service (AKS)](/azure/aks/) verwaltet Ihre gehostete Kubernetes-Umgebung. Mit AKS können Sie ganz ohne Kenntnisse im Bereich Containerorchestrierung schnell und einfach containerbasierte Anwendungen bereitstellen und verwalten. Darüber hinaus übernimmt die Lösung die Betreuung laufender Vorgänge und Wartungsaufgaben für Sie, indem sie Ressourcen ganz nach Bedarf bereitstellt, aktualisiert und skaliert, ohne Ihre Anwendungen offline zu schalten.

Ein Eingangscontroller ist eine Softwarekomponente, die einen Reverseproxy, konfigurierbare Datenverkehrsweiterleitung und TLS-Terminierung für Kubernetes-Dienste bereitstellt. Mithilfe von Ressourcen für eingehende Kubernetes-Daten werden Eingangsregeln und Routen für einzelne Kubernetes-Dienste konfiguriert. Durch die Verwendung von einem Eingangscontroller und Eingangsregeln kann eine einzelne IP-Adresse zum Weiterleiten von Datenverkehr an mehrere Dienste in einem Kubernetes-Cluster verwendet werden. Alle oben genannten Funktionen werden von Azure [Application Gateway](/azure/Application-Gateway/) bereitgestellt, was Application Gateway zu einem idealen Eingangscontroller für Kubernetes in Azure macht. 

In diesem Tutorial erfahren Sie, wie Sie die folgenden Aufgaben ausführen, um einen [Kubernetes](https://www.redhat.com/en/topics/containers/what-is-kubernetes)-Cluster mit AKS und Application Gateway als Eingangscontroller zu erstellen:

> [!div class="checklist"]
> * Verwenden von HCL (HashiCorp-Sprache) zum Definieren eines Kubernetes-Clusters
> * Erstellen einer Application Gateway-Ressource mithilfe von Terraform
> * Verwenden von Terraform und AKS zum Erstellen eines Kubernetes-Clusters
> * Verwenden des kubectl-Tools zum Testen der Verfügbarkeit eines Kubernetes-Clusters

## <a name="prerequisites"></a>Voraussetzungen

- **Azure-Abonnement**: Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.

- **Konfigurieren von Terraform:** Befolgen Sie die Anweisungen im Artikel [Installieren und Konfigurieren von Terraform zum Bereitstellen von VMs und sonstiger Infrastruktur in Azure](/azure/virtual-machines/linux/terraform-install-configure).

- **Azure-Dienstprinzipal:** Befolgen Sie die Anweisungen im Abschnitt **Erstellen des Dienstprinzipals** des Artikels [Erstellen eines Azure-Dienstprinzipals mit der Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest). Notieren Sie sich die Werte für „appId“, „displayName“ und „password“.
  - Führen Sie den folgenden Befehl aus, und notieren Sie sich die Objekt-ID des Dienstprinzipals:

    ```azurecli
    az ad sp list --display-name <displayName>
    ```

## <a name="create-the-directory-structure"></a>Erstellen der Verzeichnisstruktur
Der erste Schritt ist das Erstellen des Verzeichnisses, das Ihre Terraform-Konfigurationsdateien für die Übung enthält.

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).

1. Öffnen Sie [Azure Cloud Shell](/azure/cloud-shell/overview). Falls Sie zuvor noch keine Umgebung ausgewählt haben, wählen Sie **Bash** als Umgebung aus.

    ![Cloud Shell-Eingabeaufforderung](./media/terraform-k8s-cluster-appgw-with-tf-aks/azure-portal-cloud-shell-button-min.png)

1. Wechseln Sie in das Verzeichnis `clouddrive`.

    ```bash
    cd clouddrive
    ```

1. Erstellen Sie ein Verzeichnis namens `terraform-aks-k8s`.

    ```bash
    mkdir terraform-aks-appgw-ingress
    ```

1. Wechseln Sie zum neuen Verzeichnis:

    ```bash
    cd terraform-aks-appgw-ingress
    ```

## <a name="declare-the-azure-provider"></a>Deklarieren des Azure-Anbieters
Erstellen Sie die Terraform-Konfigurationsdatei, die den Azure-Anbieter deklariert.

1. Erstellen Sie in Cloud Shell eine Datei namens `main.tf`.

    ```bash
    vi main.tf
    ```

1. Drücken Sie die I-TASTE, um den Einfügemodus zu starten.

1. Fügen Sie den folgenden Code in den Editor ein:

    ```hcl
    provider "azurerm" {
        version = "~>1.18"
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

## <a name="define-input-variables"></a>Definieren von Eingabevariablen
Erstellen Sie die Terraform-Konfigurationsdatei, in der alle für diese Bereitstellung erforderlichen Variablen aufgeführt werden.

1. Erstellen Sie in Cloud Shell eine Datei namens `variables.tf`.

    ```bash
    vi variables.tf
    ```

1. Drücken Sie die I-TASTE, um den Einfügemodus zu starten.

1. Fügen Sie den folgenden Code in den Editor ein:
    
    ```hcl
    variable "resource_group_name" {
      description = "Name of the resource group already created."
    }
    
    variable "location" {
      description = "Location of the cluster."
    }
    
    variable "aks_service_principal_app_id" {
      description = "Application ID/Client ID  of the service principal. Used by AKS to manage AKS related resources on Azure like vms, subnets."
    }
    
    variable "aks_service_principal_client_secret" {
      description = "Secret of the service principal. Used by AKS to manage Azure."
    }
    
    variable "aks_service_principal_object_id" {
      description = "Object ID of the service principal."
    }
    
    variable "virtual_network_name" {
      description = "Virtual network name"
      default     = "aksVirtualNetwork"
    }
    
    variable "virtual_network_address_prefix" {
      description = "Containers DNS server IP address."
      default     = "15.0.0.0/8"
    }
    
    variable "aks_subnet_name" {
      description = "AKS Subnet Name."
      default     = "kubesubnet"
    }
    
    variable "aks_subnet_address_prefix" {
      description = "Containers DNS server IP address."
      default     = "15.0.0.0/16"
    }
    
    variable "app_gateway_subnet_address_prefix" {
      description = "Containers DNS server IP address."
      default     = "15.1.0.0/16"
    }
    
    variable "app_gateway_name" {
      description = "Name of the Application Gateway."
      default = "ApplicationGateway1"
    }
    
    variable "app_gateway_sku" {
      description = "Name of the Application Gateway SKU."
      default = "Standard_v2"
    }
    
    
    variable "app_gateway_tier" {
      description = "Tier of the Application Gateway SKU."
      default = "Standard_v2"
    }
    
    
    variable "aks_name" {
      description = "Name of the AKS cluster."
      default     = "aks-cluster1"
    }
    variable "aks_dns_prefix" {
      description = "Optional DNS prefix to use with hosted Kubernetes API server FQDN."
      default     = "aks"
    }
    
    
    variable "aks_agent_os_disk_size" {
      description = "Disk size (in GB) to provision for each of the agent pool nodes. This value ranges from 0 to 1023. Specifying 0 will apply the default disk size for that agentVMSize."
      default     = 40
    }
    
    variable "aks_agent_count" {
      description = "The number of agent nodes for the cluster."
      default     = 3
    }
    
    variable "aks_agent_vm_size" {
      description = "The size of the Virtual Machine."
      default     = "Standard_D3_v2"
    }
    
    variable "kubernetes_version" {
      description = "The version of Kubernetes."
      default     = "1.11.5"
    }
    
    variable "aks_service_cidr" {
      description = "A CIDR notation IP range from which to assign service cluster IPs."
      default     = "10.0.0.0/16"
    }
    
    variable "aks_dns_service_ip" {
      description = "Containers DNS server IP address."
      default     = "10.0.0.10"
    }
    
    variable "aks_docker_bridge_cidr" {
      description = "A CIDR notation IP for Docker bridge."
      default     = "172.17.0.1/16"
    }
    
    variable "aks_enable_rbac" {
      description = "Enable RBAC on the AKS cluster. Defaults to false."
      default     = "false"
    }
    
    variable "vm_user_name" {
      description = "User name for the VM"
      default     = "vmuser1"
    }
    
    variable "public_ssh_key_path" {
      description = "Public key path for SSH."
      default     = "~/.ssh/id_rsa.pub"
    }
    
    variable "tags" {
      type = "map"
    
      default = {
        source = "terraform"
      }
    }
    ```

## <a name="define-the-resources"></a>Definieren der Ressourcen 
Erstellen Sie die Terraform-Konfigurationsdatei für die Erstellung der Ressourcen. 

1. Erstellen Sie in Cloud Shell eine Datei namens `resources.tf`.

    ```bash
    vi resources.tf
    ```

1. Drücken Sie die I-TASTE, um den Einfügemodus zu starten.

1. Fügen Sie die folgenden Codeblöcke in den Editor ein:

    a. Erstellen Sie einen Block mit lokalen Variablen für die Wiederverwendung berechneter Variablen:

    ```hcl
    # # Locals block for hardcoded names. 
    locals {
        backend_address_pool_name      = "${azurerm_virtual_network.test.name}-beap"
        frontend_port_name             = "${azurerm_virtual_network.test.name}-feport"
        frontend_ip_configuration_name = "${azurerm_virtual_network.test.name}-feip"
        http_setting_name              = "${azurerm_virtual_network.test.name}-be-htst"
        listener_name                  = "${azurerm_virtual_network.test.name}-httplstn"
        request_routing_rule_name      = "${azurerm_virtual_network.test.name}-rqrt"
        app_gateway_subnet_name = "appgwsubnet"
    }
    ```

    b. Erstellen Sie eine Datenquelle für die Ressourcengruppe und eine neue Benutzeridentität:

    ```hcl
    data "azurerm_resource_group" "rg" {
      name = "${var.resource_group_name}"
    }
    
    # User Assigned Idntities 
    resource "azurerm_user_assigned_identity" "testIdentity" {
      resource_group_name = "${data.azurerm_resource_group.rg.name}"
      location            = "${data.azurerm_resource_group.rg.location}"
    
      name = "identity1"
    
      tags = "${var.tags}"
    }
    ```

    c. Erstellen Sie die grundlegenden Netzwerkressourcen:

    ```hcl
    resource "azurerm_virtual_network" "test" {
      name                = "${var.virtual_network_name}"
      location            = "${data.azurerm_resource_group.rg.location}"
      resource_group_name = "${data.azurerm_resource_group.rg.name}"
      address_space       = ["${var.virtual_network_address_prefix}"]
    
      subnet {
        name           = "${var.aks_subnet_name}"
        address_prefix = "${var.aks_subnet_address_prefix}" 
      }
    
      subnet {
        name           = "appgwsubnet"
        address_prefix = "${var.app_gateway_subnet_address_prefix}"
      }
    
      tags = "${var.tags}"
    }
    
    data "azurerm_subnet" "kubesubnet" {
      name                 = "${var.aks_subnet_name}"
      virtual_network_name = "${azurerm_virtual_network.test.name}"
      resource_group_name  = "${data.azurerm_resource_group.rg.name}"
    }
    
    data "azurerm_subnet" "appgwsubnet" {
      name                 = "appgwsubnet"
      virtual_network_name = "${azurerm_virtual_network.test.name}"
      resource_group_name  = "${data.azurerm_resource_group.rg.name}"
    }
    
    # Public Ip 
    resource "azurerm_public_ip" "test" {
      name                         = "publicIp1"
      location                     = "${data.azurerm_resource_group.rg.location}"
      resource_group_name          = "${data.azurerm_resource_group.rg.name}"
      public_ip_address_allocation = "static"
      sku                          = "Standard"
    
      tags = "${var.tags}"
    }
    ```

    d. Erstellen Sie die Application Gateway-Ressource:

    ```hcl
    resource "azurerm_application_gateway" "network" {
      name                = "${var.app_gateway_name}"
      resource_group_name = "${data.azurerm_resource_group.rg.name}"
      location            = "${data.azurerm_resource_group.rg.location}"
    
      sku {
        name     = "${var.app_gateway_sku}"
        tier     = "Standard_v2"
        capacity = 2
      }
    
      gateway_ip_configuration {
        name      = "appGatewayIpConfig"
        subnet_id = "${data.azurerm_subnet.appgwsubnet.id}"
      }
    
      frontend_port {
        name = "${local.frontend_port_name}"
        port = 80
      }
    
      frontend_port {
        name = "httpsPort"
        port = 443
      }
    
      frontend_ip_configuration {
        name                 = "${local.frontend_ip_configuration_name}"
        public_ip_address_id = "${azurerm_public_ip.test.id}"
      }
    
      backend_address_pool {
        name = "${local.backend_address_pool_name}"
      }
    
      backend_http_settings {
        name                  = "${local.http_setting_name}"
        cookie_based_affinity = "Disabled"
        port                  = 80
        protocol              = "Http"
        request_timeout       = 1
      }
    
      http_listener {
        name                           = "${local.listener_name}"
        frontend_ip_configuration_name = "${local.frontend_ip_configuration_name}"
        frontend_port_name             = "${local.frontend_port_name}"
        protocol                       = "Http"
      }
    
      request_routing_rule {
        name                       = "${local.request_routing_rule_name}"
        rule_type                  = "Basic"
        http_listener_name         = "${local.listener_name}"
        backend_address_pool_name  = "${local.backend_address_pool_name}"
        backend_http_settings_name = "${local.http_setting_name}"
      }
    
      tags = "${var.tags}"
    
      depends_on = ["azurerm_virtual_network.test", "azurerm_public_ip.test"]
    }
    ```

    e. Erstellen Sie Rollenzuweisungen:

    ```hcl
    resource "azurerm_role_assignment" "ra1" {
      scope                = "${data.azurerm_subnet.kubesubnet.id}"
      role_definition_name = "Network Contributor"
      principal_id         = "${var.aks_service_principal_object_id }"
    
      depends_on = ["azurerm_virtual_network.test"]
    }
    
    resource "azurerm_role_assignment" "ra2" {
      scope                = "${azurerm_user_assigned_identity.testIdentity.id}"
      role_definition_name = "Managed Identity Operator"
      principal_id         = "${var.aks_service_principal_object_id}"
      depends_on           = ["azurerm_user_assigned_identity.testIdentity"]
    }
    
    resource "azurerm_role_assignment" "ra3" {
      scope                = "${azurerm_application_gateway.network.id}"
      role_definition_name = "Contributor"
      principal_id         = "${azurerm_user_assigned_identity.testIdentity.principal_id}"
      depends_on           = ["azurerm_user_assigned_identity.testIdentity", "azurerm_application_gateway.network"]
    }
    
    resource "azurerm_role_assignment" "ra4" {
      scope                = "${data.azurerm_resource_group.rg.id}"
      role_definition_name = "Reader"
      principal_id         = "${azurerm_user_assigned_identity.testIdentity.principal_id}"
      depends_on           = ["azurerm_user_assigned_identity.testIdentity", "azurerm_application_gateway.network"]
    }
    ```

    f. Erstellen Sie den Kubernetes-Cluster.

    ```hcl
    resource "azurerm_kubernetes_cluster" "k8s" {
      name       = "${var.aks_name}"
      location   = "${data.azurerm_resource_group.rg.location}"
      dns_prefix = "${var.aks_dns_prefix}"
    
      resource_group_name = "${data.azurerm_resource_group.rg.name}"
    
      linux_profile {
        admin_username = "${var.vm_user_name}"
    
        ssh_key {
          key_data = "${file(var.public_ssh_key_path)}"
        }
      }
    
      addon_profile {
        http_application_routing {
          enabled = false
        }
      }
    
      agent_pool_profile {
        name            = "agentpool"
        count           = "${var.aks_agent_count}"
        vm_size         = "${var.aks_agent_vm_size}"
        os_type         = "Linux"
        os_disk_size_gb = "${var.aks_agent_os_disk_size}"
        vnet_subnet_id  = "${data.azurerm_subnet.kubesubnet.id}"
      }
    
      service_principal {
        client_id     = "${var.aks_service_principal_app_id}"
        client_secret = "${var.aks_service_principal_client_secret}"
      }
    
      network_profile {
        network_plugin     = "azure"
        dns_service_ip     = "${var.aks_dns_service_ip}"
        docker_bridge_cidr = "${var.aks_docker_bridge_cidr}"
        service_cidr       = "${var.aks_service_cidr}"
      }
    
      depends_on = ["azurerm_virtual_network.test", "azurerm_application_gateway.network"]
      tags       = "${var.tags}"
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


## <a name="create-a-terraform-output-file"></a>Erstellen einer Terraform-Ausgabedatei
Mit [Terraform-Ausgaben](https://www.terraform.io/docs/configuration/outputs.html) können Sie Werte definieren, die für den Benutzer hervorgehoben werden, wenn Terraform einen Plan anwendet. Sie können mit dem Befehl `terraform output` abgefragt werden. In diesem Abschnitt erstellen Sie eine Ausgabedatei, die Zugriff auf den Cluster mit [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/) ermöglicht.

1. Erstellen Sie in Cloud Shell eine Datei namens `output.tf`.

    ```bash
    vi output.tf
    ```

1. Drücken Sie die I-TASTE, um den Einfügemodus zu starten.

1. Fügen Sie den folgenden Code in den Editor ein:

    ```hcl
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
Terraform verfolgt den Status lokal über die Datei `terraform.tfstate`. Dieses Muster funktioniert gut in einer Umgebung mit nur einer Person. In einer praxisorientierteren Umgebung mit mehreren Personen müssen Sie den Status allerdings auf dem Server mit [Azure Storage](/azure/storage/) nachverfolgen. In diesem Abschnitt rufen Sie die erforderlichen Informationen zum Speicherkonto (Kontoname und Kontoschlüssel) ab und erstellen einen Speichercontainer, in dem die Terraform-Statusinformationen gespeichert werden.

1. Wählen Sie im Azure-Portal im linken Menü **Alle Dienste** aus.

1. Wählen Sie **Speicherkonten** aus.

1. Wählen Sie auf der Registerkarte **Speicherkonten** den Namen des Speicherkontos aus, in dem Terraform den Status speichern soll. Beispielsweise können Sie das Speicherkonto verwenden, das erstellt wurde, als Sie Cloud Shell zum ersten Mal geöffnet haben.  Der Name des von Cloud Shell erstellten Speicherkontos beginnt in der Regel mit `cs` gefolgt von einer zufälligen Zeichenfolge aus Zahlen und Buchstaben. **Notieren Sie sich den Namen des ausgewählten Speicherkontos, da Sie ihn später benötigen.**

1. Wählen Sie auf der Registerkarte des Speicherkontos **Zugriffsschlüssel** aus.

    ![Menü des Speicherkontos](./media/terraform-k8s-cluster-appgw-with-tf-aks/storage-account.png)

1. Notieren Sie sich den Wert des **Schlüssels** **key1**. (Wenn Sie das Symbol auf der rechten Seite des Schlüssels auswählen, wird der Wert in die Zwischenablage kopiert.)

    ![Speicherkonto-Zugriffsschlüssel](./media/terraform-k8s-cluster-appgw-with-tf-aks/storage-account-access-key.png)

1. Erstellen Sie in Cloud Shell einen Container in Ihrem Azure-Speicherkonto (ersetzen Sie die Platzhalter &lt;YourAzureStorageAccountName> und &lt;YourAzureStorageAccountAccessKey> durch die entsprechenden Werte für Ihr Azure-Speicherkonto).

    ```azurecli
    az storage container create -n tfstate --account-name <YourAzureStorageAccountName> --account-key <YourAzureStorageAccountKey>
    ```

## <a name="create-the-kubernetes-cluster"></a>Erstellen des Kubernetes-Clusters
In diesem Abschnitt erfahren Sie, wie der Befehl `terraform init` verwendet wird, um die Ressourcen zu erstellen, die in den Konfigurationsdateien definiert sind, die Sie in den vorherigen Abschnitten erstellt haben.

1. Initialisieren Sie in Cloud Shell Terraform (ersetzen Sie die Platzhalter &lt;YourAzureStorageAccountName> und &lt;YourAzureStorageAccountAccessKey> durch die entsprechenden Werte für Ihr Azure-Speicherkonto).

    ```bash
    terraform init -backend-config="storage_account_name=<YourAzureStorageAccountName>" -backend-config="container_name=tfstate" -backend-config="access_key=<YourStorageAccountAccessKey>" -backend-config="key=codelab.microsoft.tfstate" 
    ```
    
    Der Befehl `terraform init` zeigt den Erfolg der Initialisierung von Back-End- und -Anbieter-Plug-Ins an:

    ![Beispiel für Ergebnisse von „terraform init“](./media/terraform-k8s-cluster-appgw-with-tf-aks/terraform-init-complete.png)

1. Erstellen Sie eine Variablendatei, um Eingabewerte anzugeben. Erstellen Sie in Cloud Shell eine Datei namens `main.tf`.

    ```bash
    vi terraform.tfvars
    ```

1. Drücken Sie die I-TASTE, um den Einfügemodus zu starten.

1. Fügen Sie die folgenden Variablen, die Sie weiter oben erstellt haben, in den Editor ein:

    ```hcl
      resource_group_name = <Name of the Resource Group already created>

      location = <Location of the Resource Group>
        
      aks_service_principal_app_id = <Service Principal AppId>
        
      aks_service_principal_client_secret = <Service Principal Client Secret>
        
      aks_service_principal_object_id = <Service Principal Object Id>
        
    ```

1. Drücken Sie die **ESC**-TASTE, um den Einfügemodus zu beenden.

1. Speichern Sie die Datei, und geben Sie den folgenden Befehl ein, um den vi-Editor zu schließen:

    ```bash
    :wq
    ```

1. Führen Sie den Befehl `terraform plan` aus, um den Terraform-Plan zu erstellen, der die Infrastrukturelemente definiert. 

    ```bash
    terraform plan -out out.plan
    ```

    Der Befehl `terraform plan` zeigt die Ressourcen an, die erstellt werden, wenn Sie den Befehl `terraform apply` ausführen:

    ![Beispiel für Ergebnisse von „terraform plan“](./media/terraform-k8s-cluster-appgw-with-tf-aks/terraform-plan-complete.png)

1. Führen Sie den Befehl `terraform apply` aus, um den Plan zum Erstellen des Kubernetes-Clusters anzuwenden. Der Prozess zum Erstellen eines Kubernetes-Clusters dauert einige Minuten. Dies kann zu einem Timeout der Cloud Shell-Sitzung führen. Wenn ein Timeout der Cloud Shell-Sitzung eintritt, können Sie die Schritte im Abschnitt „Wiederherstellen nach einem Cloud Shell-Timeout“ ausführen, um das Tutorial abzuschließen.

    ```bash
    terraform apply out.plan
    ```

    Der Befehl `terraform apply` zeigt die Ergebnisse der Erstellung der Ressourcen an, die in den Konfigurationsdateien definiert sind:

    ![Beispiel für Ergebnisse von „terraform apply“](./media/terraform-k8s-cluster-appgw-with-tf-aks/terraform-apply-complete.png)

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppen** aus, um die Ressourcen anzuzeigen, die für Ihren neuen Kubernetes-Cluster in der ausgewählten Ressourcengruppe erstellt wurden.

    ![Cloud Shell-Eingabeaufforderung](./media/terraform-k8s-cluster-appgw-with-tf-aks/k8s-resources-created.png)

## <a name="recover-from-a-cloud-shell-timeout"></a>Wiederherstellen nach einem Cloud Shell-Timeout
Bei einem Timeout der Cloud Shell-Sitzung können Sie zur Wiederherstellung die folgenden Schritte ausführen:

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

    ![Das kubectl-Tool ermöglicht die Überprüfung der Integrität des Kubernetes-Clusters](./media/terraform-k8s-cluster-appgw-with-tf-aks/kubectl-get-nodes.png)


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie Terraform und AKS zum Erstellen eines Kubernetes-Clusters verwenden. Weitere Informationen zu Terraform in Azure finden Sie hier:
 
 > [!div class="nextstepaction"] 
 > [Dokumentation zu Terraform in Azure](https://docs.microsoft.com/azure/terraform/)
 

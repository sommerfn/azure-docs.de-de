---
title: Erstellen eines VM-Clusters mit Terraform und HCL
description: Erstellen Sie mit Terraform und HCL (HashiCorp Configuration Language) einen Cluster mit virtuellen Linux-Computern mit Lastenausgleich in Azure.
services: terraform
ms.service: azure
keywords: Terraform, Devops, virtueller Computer, Netzwerk, Module
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 11/13/2017
ms.openlocfilehash: 284dcd99dc77d7ec0fb5cb214d49b6fcf93a6aef
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2019
ms.locfileid: "68854483"
---
# <a name="create-a-vm-cluster-with-terraform-and-hcl"></a>Erstellen eines VM-Clusters mit Terraform und HCL

Dieses Tutorial veranschaulicht das Erstellen eines kleinen Computeclusters mithilfe der [HashiCorp Configuration Language](https://www.terraform.io/docs/configuration/syntax.html) (HCL). Die Konfiguration erstellt einen Lastenausgleich, zwei virtuelle Linux-Computer in einer [Verfügbarkeitsgruppe](/azure/virtual-machines/windows/manage-availability#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) und alle erforderlichen Netzwerkressourcen.

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Einrichten der Azure-Authentifizierung
> * Erstellen einer Terraform-Konfigurationsdatei
> * Initialisieren von Terraform
> * Erstellen eines Terraform-Ausführungsplans
> * Anwenden des Terraform-Ausführungsplans

## <a name="1-set-up-azure-authentication"></a>1. Einrichten der Azure-Authentifizierung

> [!NOTE]
> Wenn Sie [Terraform-Umgebungsvariablen verwenden](/azure/virtual-machines/linux/terraform-install-configure) oder dieses Tutorial in der [Azure Cloud Shell](terraform-cloud-shell.md) ausführen, überspringen Sie diesen Abschnitt.

In diesem Abschnitt generieren Sie einen Azure-Dienstprinzipal und zwei Terraform-Konfigurationsdateien mit den Anmeldeinformationen aus dem Sicherheitsprinzipal.

1. [Richten Sie einen Azure AD-Dienstprinzipal ein](/azure/virtual-machines/linux/terraform-install-configure#set-up-terraform-access-to-azure), damit Terraform Ressourcen in Azure bereitstellen kann. Notieren Sie sich beim Erstellen des Prinzipals die Werte für die Abonnement-ID, den Mandanten, die appID und das Kennwort.

2. Öffnen Sie eine Eingabeaufforderung.

3. Erstellen Sie ein leeres Verzeichnis zum Speichern der Terraform-Dateien.

4. Erstellen Sie eine neue Datei für die Variablendeklarationen. Sie können diese Datei mit einem beliebigen Namen und der Erweiterung `.tf` versehen.

5. Kopieren Sie den folgenden Code in die Variablendeklarationsdatei:

   ```tf
   variable subscription_id {}
   variable tenant_id {}
   variable client_id {}
   variable client_secret {}
  
   provider "azurerm" {
      subscription_id = "${var.subscription_id}"
      tenant_id = "${var.tenant_id}"
      client_id = "${var.client_id}"
      client_secret = "${var.client_secret}"
   }
   ```

6. Erstellen Sie eine neue Datei, die die Werte für die Terraform-Variablen enthält. Es ist üblich, die Terraform-Variablendatei mit `terraform.tfvars` zu benennen, da Terraform automatisch jede Datei mit dem Namen `terraform.tfvars` (oder einem Namen nach dem Muster `*.auto.tfvars`) lädt, wenn sie im aktuellen Verzeichnis vorhanden ist. 

7. Kopieren Sie den folgenden Code in die Variablendatei. Achten Sie darauf, die Platzhalter wie folgt zu ersetzen: Geben Sie für `subscription_id` die Azure-Abonnement-ID ein, die Sie beim Ausführen von `az account set` angegeben haben. Verwenden Sie für `tenant_id` den `tenant`-Wert, der von `az ad sp create-for-rbac` zurückgegeben wurde. Verwenden Sie für `client_id` den `appId`-Wert, der von `az ad sp create-for-rbac` zurückgegeben wurde. Verwenden Sie für `client_secret` den `password`-Wert, der von `az ad sp create-for-rbac` zurückgegeben wurde.

   ```tf
   subscription_id = "<azure-subscription-id>"
   tenant_id = "<tenant-returned-from-creating-a-service-principal>"
   client_id = "<appId-returned-from-creating-a-service-principal>"
   client_secret = "<password-returned-from-creating-a-service-principal>"
   ```

## <a name="2-create-a-terraform-configuration-file"></a>2. Erstellen einer Terraform-Konfigurationsdatei

In diesem Abschnitt erstellen Sie eine Datei, die Ressourcendefinitionen für die Infrastruktur enthält.

1. Erstellen Sie eine neue Datei mit dem Namen `main.tf`. 

2. Kopieren Sie die folgenden Beispielressourcendefinitionen in die neu erstellte Datei `main.tf`: 

   ```tf
   resource "azurerm_resource_group" "test" {
    name     = "acctestrg"
    location = "West US 2"
   }

   resource "azurerm_virtual_network" "test" {
    name                = "acctvn"
    address_space       = ["10.0.0.0/16"]
    location            = "${azurerm_resource_group.test.location}"
    resource_group_name = "${azurerm_resource_group.test.name}"
   }

   resource "azurerm_subnet" "test" {
    name                 = "acctsub"
    resource_group_name  = "${azurerm_resource_group.test.name}"
    virtual_network_name = "${azurerm_virtual_network.test.name}"
    address_prefix       = "10.0.2.0/24"
   }

   resource "azurerm_public_ip" "test" {
    name                         = "publicIPForLB"
    location                     = "${azurerm_resource_group.test.location}"
    resource_group_name          = "${azurerm_resource_group.test.name}"
    allocation_method            = "Static"
   }

   resource "azurerm_lb" "test" {
    name                = "loadBalancer"
    location            = "${azurerm_resource_group.test.location}"
    resource_group_name = "${azurerm_resource_group.test.name}"

    frontend_ip_configuration {
      name                 = "publicIPAddress"
      public_ip_address_id = "${azurerm_public_ip.test.id}"
    }
   }

   resource "azurerm_lb_backend_address_pool" "test" {
    resource_group_name = "${azurerm_resource_group.test.name}"
    loadbalancer_id     = "${azurerm_lb.test.id}"
    name                = "BackEndAddressPool"
   }

   resource "azurerm_network_interface" "test" {
    count               = 2
    name                = "acctni${count.index}"
    location            = "${azurerm_resource_group.test.location}"
    resource_group_name = "${azurerm_resource_group.test.name}"

    ip_configuration {
      name                          = "testConfiguration"
      subnet_id                     = "${azurerm_subnet.test.id}"
      private_ip_address_allocation = "dynamic"
      load_balancer_backend_address_pools_ids = ["${azurerm_lb_backend_address_pool.test.id}"]
    }
   }

   resource "azurerm_managed_disk" "test" {
    count                = 2
    name                 = "datadisk_existing_${count.index}"
    location             = "${azurerm_resource_group.test.location}"
    resource_group_name  = "${azurerm_resource_group.test.name}"
    storage_account_type = "Standard_LRS"
    create_option        = "Empty"
    disk_size_gb         = "1023"
   }

   resource "azurerm_availability_set" "avset" {
    name                         = "avset"
    location                     = "${azurerm_resource_group.test.location}"
    resource_group_name          = "${azurerm_resource_group.test.name}"
    platform_fault_domain_count  = 2
    platform_update_domain_count = 2
    managed                      = true
   }

   resource "azurerm_virtual_machine" "test" {
    count                 = 2
    name                  = "acctvm${count.index}"
    location              = "${azurerm_resource_group.test.location}"
    availability_set_id   = "${azurerm_availability_set.avset.id}"
    resource_group_name   = "${azurerm_resource_group.test.name}"
    network_interface_ids = ["${element(azurerm_network_interface.test.*.id, count.index)}"]
    vm_size               = "Standard_DS1_v2"

    # Uncomment this line to delete the OS disk automatically when deleting the VM
    # delete_os_disk_on_termination = true

    # Uncomment this line to delete the data disks automatically when deleting the VM
    # delete_data_disks_on_termination = true

    storage_image_reference {
      publisher = "Canonical"
      offer     = "UbuntuServer"
      sku       = "16.04-LTS"
      version   = "latest"
    }

    storage_os_disk {
      name              = "myosdisk${count.index}"
      caching           = "ReadWrite"
      create_option     = "FromImage"
      managed_disk_type = "Standard_LRS"
    }

    # Optional data disks
    storage_data_disk {
      name              = "datadisk_new_${count.index}"
      managed_disk_type = "Standard_LRS"
      create_option     = "Empty"
      lun               = 0
      disk_size_gb      = "1023"
    }

    storage_data_disk {
      name            = "${element(azurerm_managed_disk.test.*.name, count.index)}"
      managed_disk_id = "${element(azurerm_managed_disk.test.*.id, count.index)}"
      create_option   = "Attach"
      lun             = 1
      disk_size_gb    = "${element(azurerm_managed_disk.test.*.disk_size_gb, count.index)}"
    }

    os_profile {
      computer_name  = "hostname"
      admin_username = "testadmin"
      admin_password = "Password1234!"
    }

    os_profile_linux_config {
      disable_password_authentication = false
    }

    tags {
      environment = "staging"
    }
   }
   ```

## <a name="3-initialize-terraform"></a>3. Initialisieren von Terraform 

Mit dem Befehl [terraform init](https://www.terraform.io/docs/commands/init.html) wird ein Verzeichnis initialisiert, das die Terraform-Konfigurationsdateien enthält – die Dateien, die Sie in den vorherigen Abschnitten erstellt haben. Es empfiehlt sich, nach dem Schreiben einer neuen Terraform-Konfiguration immer den Befehl `terraform init` auszuführen. 

> [!TIP]
> Der Befehl `terraform init` ist idempotent, d. h., er kann wiederholt aufgerufen werden und erzeugt immer das gleiche Ergebnis. Wenn Sie in einer Zusammenarbeitsumgebung arbeiten und vermuten, dass die Konfigurationsdateien möglicherweise geändert wurden, ist es deshalb immer ratsam, vor dem Ausführen oder Anwenden eines Plans den Befehl `terraform init` aufzurufen.

Führen Sie zum Initialisieren von Terraform den folgenden Befehl aus:

  ```cmd
  terraform init
  ```

  ![Initialisieren von Terraform](media/terraform-create-vm-cluster-with-infrastructure/terraform-init.png)

## <a name="4-create-a-terraform-execution-plan"></a>4. Erstellen eines Terraform-Ausführungsplans

Der Befehl [terraform plan](https://www.terraform.io/docs/commands/plan.html) dient zum Erstellen eines Ausführungsplans. Zum Generieren eines Ausführungsplans werden alle `.tf`-Dateien im aktuellen Verzeichnis aggregiert. 

Wenn Sie in einer Zusammenarbeitsumgebung arbeiten, in der die Konfiguration möglicherweise zwischen dem Erstellen und dem Anwenden des Ausführungsplans geändert wurde, sollten Sie mithilfe des [-out-Parameters des Befehls „terraform plan“](https://www.terraform.io/docs/commands/plan.html#out-path) den Ausführungsplan in einer Datei speichern. Wenn Sie hingegen in einer Umgebung ohne weitere Personen arbeiten, können Sie den `-out`-Parameter weglassen.

Falls der Name der Terraform-Variablendatei nicht `terraform.tfvars` lautet und nicht dem Muster `*.auto.tfvars` entspricht, müssen Sie beim Ausführen des Befehls `terraform plan` dessen [-var-file-Parameter](https://www.terraform.io/docs/commands/plan.html#var-file-foo) verwenden, um den Dateinamen anzugeben.

Beim Ausführen des Befehls `terraform plan` erfolgt eine Aktualisierung, und es wird bestimmt, welche Aktionen erforderlich sind, um den in den Konfigurationsdateien angegebenen gewünschten Zustand zu erreichen.

Wenn Sie den Ausführungsplan nicht speichern müssen, führen Sie den folgenden Befehl aus:

  ```cmd
  terraform plan
  ```

Wenn Sie den Ausführungsplan speichern müssen, führen Sie den folgenden Befehl aus (ersetzen Sie den Platzhalter &lt;path> durch den gewünschten Ausgabepfad):

  ```cmd
  terraform plan -out=<path>
  ```

![Erstellen eines Terraform-Ausführungsplans](media/terraform-create-vm-cluster-with-infrastructure/terraform-plan.png)

## <a name="5-apply-the-terraform-execution-plan"></a>5. Anwenden des Terraform-Ausführungsplans

Im letzten Schritt dieses Tutorials wird mit dem Befehl [terraform apply](https://www.terraform.io/docs/commands/apply.html) der Satz der durch den Befehl `terraform plan` generierten Aktionen angewendet.

Wenn Sie den letzten Ausführungsplan anwenden möchten, führen Sie den folgenden Befehl aus:

  ```cmd
  terraform apply
  ```

Wenn Sie einen zuvor gespeicherten Ausführungsplan anwenden möchten, führen Sie den folgenden Befehl aus (ersetzen Sie den Platzhalter &lt;path> durch den Pfad zum gespeicherten Ausführungsplan):

  ```cmd
  terraform apply <path>
  ```

![Anwenden eines Terraform-Ausführungsplans](media/terraform-create-vm-cluster-with-infrastructure/terraform-apply.png)

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die Liste der [Azure Terraform-Module](https://registry.terraform.io/modules/Azure) an.
- Erstellen Sie eine [VM-Skalierungsgruppe mit Terraform](terraform-create-vm-scaleset-network-disks-hcl.md).

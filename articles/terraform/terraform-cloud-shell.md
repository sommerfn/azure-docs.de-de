---
title: 'Tutorial: Konfigurieren von Azure Cloud Shell für Terraform'
description: Verwenden Sie Terraform mit Azure Cloud Shell, um die Authentifizierung und die Vorlagenkonfiguration zu vereinfachen.
ms.topic: tutorial
ms.date: 10/26/2019
ms.openlocfilehash: db9edfadbe01edc1ee9df09c284e3895ee11f3d3
ms.sourcegitcommit: 28688c6ec606ddb7ae97f4d0ac0ec8e0cd622889
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/18/2019
ms.locfileid: "74159127"
---
# <a name="tutorial-configure-azure-cloud-shell-for-terraform"></a>Tutorial: Konfigurieren von Azure Cloud Shell for Terraform

Terraform kann hervorragend über eine Bash-Befehlszeile unter macOS, Windows oder Linux verwendet werden. Die Ausführung Ihrer Terraform-Konfigurationen in der Bash-Umgebung von [Azure Cloud Shell](/azure/cloud-shell/overview) hat einige besondere Vorteile. In diesem Tutorial erfahren Sie, wie Sie Terraform-Skripts für die Bereitstellung in Azure unter Verwendung von Cloud Shell schreiben.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Automatische Konfiguration von Anmeldeinformationen

Terraform ist nach der Installation sofort in Cloud Shell verfügbar. Terraform-Skripts werden über Azure authentifiziert, wenn Sie bei Cloud Shell angemeldet sind, und ermöglichen die Verwaltung der Infrastruktur ohne zusätzliche Konfiguration. Die automatische Authentifizierung umgeht zwei manuelle Prozesse:
- Erstellen eines Active Directory-Dienstprinzipals
- Konfigurieren der Azure-Terraform-Anbietervariablen


## <a name="use-modules-and-providers"></a>Verwenden von Modulen und Anbietern

Azure-Terraform-Module benötigen Anmeldeinformationen, um auf Azure-Ressourcen zugreifen und diese ändern zu können. Fügen Sie den folgenden Code hinzu, um Terraform-Module in Cloud Shell zu verwenden:


```hcl
# Configure the Microsoft Azure Provider
provider "azurerm" {
}
```

Cloud Shell übergibt die erforderlichen Werte für den Anbieter `azurerm` über Umgebungsvariablen, wenn Sie einen der CLI-Befehle vom Typ `terraform` verwenden.

## <a name="other-cloud-shell-developer-tools"></a>Andere Cloud Shell-Entwicklungstools

Dateien und Shellzustände bleiben in Azure Storage zwischen Cloud Shell-Sitzungen erhalten. Verwenden Sie den [Azure Storage-Explorer](/azure/vs-azure-tools-storage-manage-with-storage-explorer), um Dateien auf Ihrem lokalen Computer zu kopieren und in Cloud Shell hochzuladen.

Die Azure CLI steht in Cloud Shell zur Verfügung und eignet sich gut zum Testen von Konfigurationen sowie zum Überprüfen Ihrer Arbeit nach Abschluss von `terraform apply` oder `terraform destroy`.


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen eines Azure-VM-Clusters mit Terraform mithilfe der Modulregistrierung](terraform-create-vm-cluster-module.md)

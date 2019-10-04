---
title: Verwenden eines Azure Marketplace-Images zum Erstellen eines virtuellen Terraform-Computers für Linux mit verwalteter Identität
description: Verwenden Sie ein Marketplace-Image, um einen virtuellen Terraform-Computer für Linux mit verwalteter Identität und Remotezustandsverwaltung zur einfachen Bereitstellung von Ressourcen in Azure zu erstellen.
services: terraform
ms.service: azure
keywords: Terraform, DevOps, MSI, VM, Remotezustand, Azure
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/20/2019
ms.openlocfilehash: 835170448db8709812ca62ac67f8659b3b6a2807
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71173064"
---
# <a name="use-an-azure-marketplace-image-to-create-a-terraform-linux-virtual-machine-with-managed-identities-for-azure-resources"></a>Verwenden eines Azure Marketplace-Images zum Erstellen eines virtuellen Terraform-Computers für Linux mit verwalteten Identitäten für Azure-Ressourcen

In diesem Artikel erfahren Sie, wie Sie mit einem [Terraform-Marketplace-Image](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.terraform?tab=Overview) eine Ubuntu-Linux-VM (16.04 LTS) erstellen, auf der die neueste [Terraform](https://www.terraform.io/intro/index.html)-Version installiert ist und die mithilfe [verwalteter Identitäten für Azure-Ressourcen](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview) konfiguriert wurde. Dieses Image konfiguriert außerdem ein Remote-Back-End, um die Verwaltung des [Remotezustands](https://www.terraform.io/docs/state/remote.html) mit Terraform zu ermöglichen. 

Das Terraform-Marketplace-Image vereinfacht den Einstieg in die Verwendung von Terraform in Azure, ohne dass Sie Terraform manuell installieren und konfigurieren müssen. 

Für dieses Terraform-VM-Image fallen keine Softwaregebühren an. Sie zahlen nur die Gebühren für die Azure-Hardwarenutzung, die basierend auf der Größe des virtuellen Computers berechnet werden, der bereitgestellt wurde. Weitere Informationen zu den Computegebühren finden Sie auf der Seite [Virtuelle Linux-Computer – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie einen virtuellen Terraform-Computer für Linux erstellen können, benötigen Sie ein Azure-Abonnement. Wenn Sie noch kein Abonnement besitzen, helfen Ihnen die Informationen unter [Erstellen Sie noch heute Ihr kostenloses Azure-Konto](https://azure.microsoft.com/free/) weiter.  

## <a name="create-your-terraform-virtual-machine"></a>Erstellen des virtuellen Terraform-Computers 

Führen Sie zum Erstellen einer Instanz eines virtuellen Terraform-Computers für Linux die folgenden Schritte aus: 

1. Wechseln Sie im Azure-Portal zum Eintrag [Ressource erstellen](https://ms.portal.azure.com/#create/hub).

2. Suchen Sie in der Suchleiste **Marketplace durchsuchen** nach **Terraform**. Wählen Sie die Vorlage **Terraform** aus. 

3. Klicken auf der Registerkarte mit den Terraform-Details unten rechts auf die Schaltfläche **Erstellen**.

    ![Erstellen eines virtuellen Terraform-Computers](media/terraformmsi.png)

4. Die folgenden Abschnitte enthalten die Angaben für jeden Schritt des Assistenten zum Erstellen des virtuellen Terraform-Computers für Linux. Im folgenden Abschnitt sind die Angaben aufgeführt, die zum Konfigurieren jedes dieser Schritte erforderlich sind.

## <a name="details-on-the-create-terraform-tab"></a>Details auf der Registerkarte „Terraform erstellen“

Geben Sie auf der Registerkarte **Terraform erstellen** folgende Informationen ein:

1. **Grundlagen**
    
   * **Name**: Der Name Ihrer Terraform-VM.
   * **Benutzername**: Die ID für die erste Kontoanmeldung.
   * **Kennwort**: Das erste Kontokennwort. (Sie können statt des Kennworts einen öffentlichen SSH-Schlüssel verwenden.)
   * **Abonnement**: Das Abonnement, in dem der Computer erstellt und abgerechnet werden soll. Sie müssen für dieses Abonnement über Berechtigungen zum Erstellen von Ressourcen verfügen.
   * **Ressourcengruppe**: Eine neue oder vorhandene Ressourcengruppe.
   * **Standort**: Das am besten geeignete Rechenzentrum. In der Regel handelt es sich dabei um das Rechenzentrum, in dem der größte Teil Ihrer Daten gespeichert ist, oder das Rechenzentrum, das Ihrem physischen Standort am nächsten ist, um den schnellsten Netzwerkzugriff zu erreichen.

2. **Zusätzliche Einstellungen**

   * **Größe**: Größe des virtuellen Computers. 
   * **VM-Datenträgertyp**: SSD oder HDD.

3. **Zusammenfassung für Terraform**

   * Stellen Sie sicher, dass alle eingegebenen Informationen richtig sind. 

4. **Kaufen**

   * Um den Bereitstellungsprozess zu starten, klicken Sie auf **Kaufen**. Ein Link zu den Bedingungen der Transaktion wird bereitgestellt. Für den virtuellen Computer fallen über die Computekosten hinaus keine weiteren Kosten für die Servergröße an, die Sie unter „Größe“ ausgewählt haben.

Das Terraform-VM-Image führt die folgenden Schritte aus:

* Es erstellt einen virtuellen Computer mit vom System zugewiesener Identität basierend auf dem Ubuntu 16.04 LTS-Image.
* Es installiert die MSI-Erweiterung auf dem virtuellen Computer um das Ausstellen von OAuth-Token für Azure-Ressourcen zu ermöglichen.
* Es weist der verwalteten Identität RBAC-Berechtigungen zu, wodurch Besitzerrechte für die Ressourcengruppe erteilt werden.
* Es erstellt einen Vorlagenordner für Terraform (tfTemplate).
* Es konfiguriert einen Terraform-Remotezustand mit dem Azure-Back-End vor.

## <a name="access-and-configure-a-linux-terraform-virtual-machine"></a>Zugreifen auf eine Terraform-VM für Linux und Konfigurieren der VM

Nachdem der virtuelle Computer erstellt wurde, können Sie sich über SSH bei diesem Computer anmelden. Verwenden Sie dazu die Kontoanmeldeinformationen, die Sie in Schritt 3 im Abschnitt „Grundlagen“ für die Textshellschnittstelle erstellt haben. Unter Windows können Sie ein SSH-Clienttool wie [PuTTY](https://www.putty.org/)herunterladen.

Nachdem Sie über SSH eine Verbindung mit dem virtuellen Computer hergestellt haben, müssen Sie den verwalteten Identitäten für Azure-Ressourcen auf dem virtuellen Computer Berechtigungen vom Typ „Mitwirkender“ für das gesamte Abonnement erteilen. 

Die Berechtigung für Mitwirkende ermöglicht der verwalteten Dienstidentität auf der VM die Verwendung von Terraform zum Erstellen von Ressourcen außerhalb der VM-Ressourcengruppe. Dazu müssen Sie lediglich einmal ein Skript ausführen. Verwenden Sie den folgenden Befehl:

`. ~/tfEnv.sh`

Das vorherige Skript verwendet den Mechanismus zur [interaktiven Anmeldung mit Azure CLI 2.0](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest#sign-in-interactively), um die Authentifizierung bei Azure vorzunehmen und der verwalteten Identität des virtuellen Computers die Berechtigung „Mitwirkender“ für das gesamte Abonnement zuzuweisen. 

 Der virtuelle Computer verfügt über ein Terraform-Back-End mit Remotezustand. Um dieses Back-End in Ihrer Terraform-Bereitstellung zu aktivieren, kopieren Sie die Datei „remoteState.tf“ aus dem Verzeichnis „tfTemplate“ in das Stammverzeichnis der Terraform-Skripts.  

 `cp  ~/tfTemplate/remoteState.tf .`

 Weitere Informationen zur Remotezustandsverwaltung finden Sie auf [dieser Seite zum Terraform-Remotezustand](https://www.terraform.io/docs/state/remote.html). Der Speicherzugriffsschlüssel ist in dieser Datei nicht geschützt und muss daher entfernt werden, bevor Terraform-Konfigurationsdateien in die Quellcodeverwaltung gelangen.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie Sie einen virtuellen Terraform-Computer für Linux in Azure einrichten. In folgenden zusätzlichen Ressourcen können Sie mehr über Terraform in Azure erfahren: 

 [Dokumentation zu Terraform in Azure](https://docs.microsoft.com/azure/terraform/)  
 [Terraform Azure Provider Documentation](https://aka.ms/terraform) (Dokumentation zum Azure-Anbieter für Terraform)  
 [Terraform Azure Provider Source](https://aka.ms/tfgit) (Referenz zum Azure-Anbieter für Terraform)  
 [Terraform Azure Modules](https://aka.ms/tfmodules) (Azure-Module für Terraform)
 


















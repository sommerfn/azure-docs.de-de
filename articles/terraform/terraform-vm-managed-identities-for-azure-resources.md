---
title: 'Tutorial: Erstellen eines virtuellen Linux-Computers mit einer verwalteten Identität auf der Grundlage des Azure Marketplace-Images unter Verwendung von Terraform'
description: Hier erfahren Sie, wie Sie unter Verwendung eines Azure Marketplace-Images einen virtuellen Terraform-Computer unter Linux mit verwalteter Identität und Remotezustandsverwaltung erstellen.
ms.service: terraform
author: tomarchermsft
ms.author: tarcher
ms.topic: tutorial
ms.date: 11/07/2019
ms.openlocfilehash: 233012d6caf1280914a6d2439ae856d69570fff7
ms.sourcegitcommit: 35715a7df8e476286e3fee954818ae1278cef1fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73838049"
---
# <a name="tutorial-create-a-linux-vm-with-a-managed-identity-from-the-azure-marketplace-image-using-terraform"></a>Tutorial: Erstellen eines virtuellen Linux-Computers mit einer verwalteten Identität auf der Grundlage des Azure Marketplace-Images unter Verwendung von Terraform

In diesem Artikel erfahren Sie, wie Sie mit einem [Terraform-Marketplace-Image](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.terraform?tab=Overview) eine Ubuntu-Linux-VM (16.04 LTS) erstellen, auf der die neueste [Terraform](https://www.terraform.io/intro/index.html)-Version installiert ist und die mithilfe [verwalteter Identitäten für Azure-Ressourcen](/azure/active-directory/managed-service-identity/overview) konfiguriert wurde. Dieses Image konfiguriert außerdem ein Remote-Back-End, um die Verwaltung des [Remotezustands](https://www.terraform.io/docs/state/remote.html) mit Terraform zu ermöglichen. 

Das Terraform-Marketplace-Image vereinfacht den Einstieg in die Verwendung von Terraform in Azure, ohne dass Sie Terraform manuell installieren und konfigurieren müssen. 

Für dieses Terraform-VM-Image fallen keine Softwaregebühren an. Sie zahlen nur die Gebühren für die Azure-Hardwarenutzung auf der Grundlage der Größe des bereitgestellten virtuellen Computers. 

Weitere Informationen zu den Computegebühren finden Sie auf der Seite [Virtuelle Linux-Computer – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

## <a name="prerequisites"></a>Voraussetzungen
Für die Erstellung eines virtuellen Terraform-Computers unter Linux benötigen Sie ein Azure-Abonnement. Wenn Sie noch kein Abonnement besitzen, helfen Ihnen die Informationen unter [Erstellen Sie noch heute Ihr kostenloses Azure-Konto](https://azure.microsoft.com/free/) weiter.  

## <a name="create-your-terraform-vm"></a>Erstellen Ihres virtuellen Terraform-Computers 

Führen Sie die folgenden Schritte aus, um eine Instanz eines virtuellen Terraform-Computers unter Linux zu erstellen: 

1. Wechseln Sie im Azure-Portal zum Eintrag [Ressource erstellen](https://ms.portal.azure.com/#create/hub).

1. Suchen Sie in der Suchleiste **Marketplace durchsuchen** nach **Terraform**. 

1. Wählen Sie **Create** aus. 

1. Die folgenden Abschnitte enthalten Eingaben für die einzelnen Schritte des Assistenten zum Erstellen des virtuellen Terraform-Computers unter Linux. Im folgenden Abschnitt sind die Angaben aufgeführt, die zum Konfigurieren jedes dieser Schritte erforderlich sind.

## <a name="details-on-the-create-terraform-tab"></a>Details auf der Registerkarte „Terraform erstellen“

Geben Sie auf der Registerkarte **Terraform erstellen** folgende Informationen ein:

1. **Grundlagen**
    
   * **Name**: Der Name Ihres virtuellen Terraform-Computers.
   * **Benutzername**: Die ID für die erste Kontoanmeldung.
   * **Kennwort**: Das erste Kontokennwort. (Sie können statt des Kennworts einen öffentlichen SSH-Schlüssel verwenden.)
   * **Abonnement**: Das Abonnement, in dem der Computer erstellt und abgerechnet werden soll. Sie müssen für dieses Abonnement über Berechtigungen zum Erstellen von Ressourcen verfügen.
   * **Ressourcengruppe**: Eine neue oder vorhandene Ressourcengruppe.
   * **Standort**: Das am besten geeignete Rechenzentrum. In der Regel handelt es sich dabei um das Rechenzentrum, in dem der größte Teil Ihrer Daten gespeichert ist, oder das Rechenzentrum, das Ihrem physischen Standort am nächsten ist, um den schnellsten Netzwerkzugriff zu erreichen.

2. **Zusätzliche Einstellungen**

   * **Größe**: Die Größe des virtuellen Computers. 
   * **VM-Datenträgertyp**: SSD oder HDD.

3. **Zusammenfassung für Terraform**

   * Stellen Sie sicher, dass alle eingegebenen Informationen richtig sind. 

4. **Kaufen**

   * Um den Bereitstellungsprozess zu starten, klicken Sie auf **Kaufen**. Ein Link zu den Bedingungen der Transaktion wird bereitgestellt. Für den virtuellen Computer fallen über die Computekosten hinaus keine weiteren Kosten für die Servergröße an, die Sie unter „Größe“ ausgewählt haben.

Das Terraform-VM-Image führt die folgenden Schritte aus:

* Es erstellt einen virtuellen Computer mit vom System zugewiesener Identität basierend auf dem Ubuntu 16.04 LTS-Image.
* Es installiert die Erweiterung für verwaltete Identitäten für Azure-Ressourcen auf dem virtuellen Computer, um die Ausstellung von OAuth-Token für Azure-Ressourcen zu ermöglichen.
* Es weist der verwalteten Identität RBAC-Berechtigungen zu, wodurch Besitzerrechte für die Ressourcengruppe erteilt werden.
* Es erstellt einen Vorlagenordner für Terraform (tfTemplate).
* Es konfiguriert einen Terraform-Remotezustand mit dem Azure-Back-End vor.

## <a name="access-and-configure-a-linux-terraform-vm"></a>Zugreifen auf einen virtuellen Terraform-Computer unter Linux und Konfigurieren des virtuellen Computers

Führen Sie nach Erstellung des virtuellen Computers die folgenden Schritte aus:

1. Melden Sie sich per SSH bei dem virtuellen Computer an. Verwenden Sie die Kontoanmeldeinformationen, die Sie im vorherigen Abschnitt erstellt haben. Unter Windows können Sie ein SSH-Clienttool wie [PuTTY](https://www.putty.org/)herunterladen.

1. Gewähren Sie verwalteten Identitäten für Azure-Ressourcen auf dem virtuellen Computer Berechtigungen vom Typ „Mitwirkender“ für das gesamte Abonnement. 

    Die Berechtigung vom Typ „Mitwirkender“ ermöglicht es verwalteten Identitäten für Azure-Ressourcen auf dem virtuellen Computer, mithilfe von Terraform Ressourcen außerhalb der VM-Ressourcengruppe zu erstellen. Führen Sie für diesen Schritt das folgende Skript aus: 
    
    ```bash
    . ~/tfEnv.sh
    ```

    Dieses Skript verwendet die [interaktive Azure CLI-Anmeldung](/cli/azure/authenticate-azure-cli?view=azure-cli-latest#sign-in-interactively) für die Authentifizierung bei Azure. Bei diesem Prozess wird die [Berechtigung „Mitwirkender für verwaltete Identität“](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) für das gesamte Abonnement zugewiesen. 

1. Der virtuelle Computer verfügt über ein Terraform-Back-End mit Remotezustand. Um dieses Back-End in Ihrer Terraform-Bereitstellung zu aktivieren, müssen Sie die Datei `remoteState.tf` in das Stammverzeichnis der Terraform-Skripts kopieren.

    ```bash
    cp  ~/tfTemplate/remoteState.tf .
    ```

    Weitere Informationen zur Remotezustandsverwaltung finden Sie [hier](https://www.terraform.io/docs/state/remote.html). Der Speicherzugriffsschlüssel wird in dieser Datei verfügbar gemacht. Schließen Sie ihn aus, bevor Sie Terraform-Konfigurationsdateien in der Quellcodeverwaltung committen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"] 
> [Weitere Informationen zur Verwendung von Terraform in Azure](/azure/terraform)
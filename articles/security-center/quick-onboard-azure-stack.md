---
title: Integrieren von Azure Stack-VMs in Azure Security Center
description: In diesem Schnellstart erfahren Sie, wie Sie die VM-Erweiterung zur Update- und Konfigurationsverwaltung für Azure Monitor auf einer Azure Stack-VM bereitstellen.
services: security-center
documentationcenter: na
author: pipposera
manager: dsavage
ms.assetid: 8982348a-0624-40c7-8a1e-642a523c7f6b
ms.service: security-center
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/02/2019
ms.author: fiseraci
ms.openlocfilehash: c186dcb2d7d7d423f5c001bdb4d3f3503beebd9c
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73686530"
---
# <a name="quickstart-onboard-your-azure-stack-virtual-machines-to-security-center"></a>Schnellstart: Integrieren Ihrer Azure Stack-VMs in Security Center
Nachdem Sie Ihr Azure-Abonnement eingebunden haben, können Sie Security Center zum Schützen Ihrer in Azure Stack ausgeführten VMs einrichten, indem Sie die VM-Erweiterung zur **Update- und Konfigurationsverwaltung für Azure Monitor** aus dem Azure Stack-Marketplace hinzufügen.

In diesem Schnellstart erfahren Sie, wie Sie die VM-Erweiterung zur **Update- und Konfigurationsverwaltung für Azure Monitor** auf einer in Azure Stack ausgeführten VM (Linux und Windows werden unterstützt) bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen
Für den Einstieg in Security Center benötigen Sie ein Microsoft Azure-Abonnement. Wenn Sie nicht über ein Abonnement verfügen, können Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/pricing/free-trial/) registrieren.

Bevor Sie mit diesem Schnellstart beginnen, benötigen Sie ein Azure-Abonnement für den Standard-Tarif von Security Center. Anweisungen zum Upgrade finden Sie unter [Schnellstart: Einbinden Ihres Azure-Abonnements in Security Center Standard](security-center-get-started.md). Sie können den Standard-Tarif von Security Center 30 Tage lang kostenlos testen. Weitere Informationen finden Sie auf der [Preisseite](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="select-your-workspace-in-azure-security-center"></a>Auswählen Ihres Arbeitsbereichs in Azure Security Center

1. Melden Sie sich beim [Azure-Portal](https://azure.microsoft.com/features/azure-portal/) an.
2. Wählen Sie im Menü **Microsoft Azure** die Option **Security Center**. **Security Center – Übersicht** wird geöffnet. 

   ![Übersicht über das Security Center][2]

3. Wählen Sie im Security Center im Hauptmenü die Option **Erste Schritte** aus.
4. Wählen Sie die Registerkarte **Erste Schritte** aus.

   ![Erste Schritte][3]

5. Klicken Sie auf **Konfigurieren** unter **Neue Nicht-Azure-Computer hinzufügen**. Es wird eine Liste Ihrer Log Analytics-Arbeitsbereiche angezeigt. Die Liste enthält, falls zutreffend, den Standardarbeitsbereich, der von Security Center für Sie erstellt wurde, wenn die automatische Bereitstellung aktiviert wurde. Wählen Sie diesen Arbeitsbereich oder einen anderen Arbeitsbereich aus, an den die Azure Stack-VM Sicherheitsdaten melden soll.

    ![Hinzufügen eines Azure-fremden Computers](./media/quick-onboard-windows-computer/non-azure.png)

   Das Blatt **Direkt-Agent** wird geöffnet. Auf dem Blatt finden Sie einen Link zum Herunterladen des Agents und Schlüssel für Ihre Arbeitsbereichs-ID, die Sie zum Konfigurieren des Agents benötigen.

   >[!NOTE]
   > Sie müssen den Agent NICHT manuell herunterladen. Der Agent wird in den folgenden Schritten als VM-Erweiterung installiert.

6. Klicken Sie rechts von **Arbeitsbereichs-ID** auf das Kopiersymbol, und fügen Sie die ID in Editor ein.

7. Klicken Sie rechts von **Primärschlüssel** auf das Kopiersymbol, und fügen Sie den Schlüssel in Editor ein.

## <a name="add-the-virtual-machine-extension-to-your-existing-azure-stack-virtual-machines"></a>Hinzufügen der VM-Erweiterung zu Ihren vorhandenen Azure Stack-VMs
Jetzt müssen Sie die VM-Erweiterung zur **Update- und Konfigurationsverwaltung für Azure Monitor** den in Azure Stack ausgeführten VMs hinzufügen.

1. Melden Sie sich auf einer neuen Browserregisterkarte beim **Azure Stack**-Portal an.
2. Wechseln Sie zur Seite **Virtuelle Computer**, und wählen Sie die VM aus, die mit Security Center geschützt werden soll. Informationen zum Erstellen einer VM in Azure Stack finden Sie im [Schnellstart für Windows-VMs ](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-quick-windows-portal) oder im [Schnellstart für Linux-VMs](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-quick-linux-portal).
3. Wählen Sie **Erweiterungen**. Die Liste der auf dieser VM installierten VM-Erweiterungen wird angezeigt.
4. Klicken Sie auf die Registerkarte **Hinzufügen**. Das Menüblatt **Neue Ressource** mit einer Liste der verfügbaren VM-Erweiterungen wird geöffnet. 
5. Wählen Sie die Erweiterung **Azure Monitor, Update and Configuration Management** (Azure Monitor, Update- und Konfigurationsverwaltung) aus, und klicken Sie auf **Erstellen**. Das Konfigurationsblatt **Erweiterung installieren** wird geöffnet.

>[!NOTE]
> Falls die Erweiterung **Azure Monitor, Update- und Konfigurationsverwaltung** in Ihrem Marketplace nicht aufgeführt ist, können Sie sich an Ihren Azure Stack-Betreiber wenden, damit sie zur Verfügung gestellt wird.

6. Fügen Sie auf dem Konfigurationsblatt **Erweiterung installieren** die **Arbeitsbereichs-ID** und den **Arbeitsbereichsschlüssel (Primärschlüssel)** ein, die Sie im vorherigen Schritt in Editor kopiert haben.
7. Geben Sie die erforderlichen Konfigurationseinstellungen an, und klicken Sie dann auf **Weiter**.
8. Nach Abschluss der Installation wird die Erweiterung mit dem Status **Bereitstellung erfolgreich** angezeigt. Es kann bis zu einer Stunde dauern, bis die VM im Security Center-Portal angezeigt wird.

Weitere Informationen zur Installation und Konfiguration des Agents für Windows finden Sie unter [Verbinden von Windows-Computern](../azure-monitor/platform/agent-windows.md#install-the-agent-using-setup-wizard).

Informationen zum Beheben von Problemen mit dem Agent für Linux finden Sie unter [Behandeln von Problemen mit dem Log Analytics-Agent für Linux](../azure-monitor/platform/agent-linux-troubleshoot.md).

Jetzt können Sie Ihre Azure-VMs und Nicht-Azure-Computer zentral überwachen. Im Security Center-Portal in Azure wird unter **Compute** eine Übersicht aller VMs und Computer mit zugehörigen Empfehlungen angezeigt. In Security Center werden unter „Sicherheitswarnungen“ auch alle erkannten Bedrohungen für diese Computer angezeigt.

  ![Blatt „Compute“][6]

Auf dem Blatt **Compute** werden zwei Arten von Symbolen dargestellt:

![icon1](./media/quick-onboard-windows-computer/security-center-monitoring-icon1.png) Nicht-Azure-Computer 

![icon2](./media/quick-onboard-windows-computer/security-center-monitoring-icon2.png) Azure-VM (Azure Stack-VMs werden in dieser Gruppe angezeigt)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn die Erweiterung nicht mehr benötigt wird, können Sie sie über das Azure Stack-Portal von der VM entfernen.

So entfernen Sie die Erweiterung:

1. Öffnen Sie das **Azure Stack-Portal**.
2. Wechseln Sie zur Seite **Virtuelle Computer**, und wählen Sie die VM aus, von der Sie die Erweiterung entfernen möchten.
3. Wählen Sie **Erweiterungen** und dann die Erweiterung **Microsoft.EnterpriseCloud.Monitoring** aus.
4. Klicken Sie auf **Deinstallieren**, und bestätigen Sie die Auswahl durch Klicken auf **Ja**.

## <a name="next-steps"></a>Nächste Schritte
In diesem Schnellstart haben Sie die Erweiterung zur Update- und Konfigurationsverwaltung für Azure Monitor auf einer in Azure Stack ausgeführten VM bereitgestellt. Weitere Informationen zur Verwendung von Security Center erhalten Sie, wenn Sie mit dem Tutorial zur Konfiguration einer Sicherheitsrichtlinie und zum Bewerten der Sicherheit Ihrer Ressourcen fortfahren.

> [!div class="nextstepaction"]
> [Tutorial: Definieren und Bewerten von Sicherheitsrichtlinien](tutorial-security-policy.md)

<!--Image references-->
[2]: ./media/quick-onboard-windows-computer/overview.png
[3]: ./media/quick-onboard-windows-computer/get-started.png
[4]: ./media/quick-onboard-windows-computer/add-computer.png
[5]: ./media/quick-onboard-windows-computer/log-analytics-mma-setup-laworkspace.png
[6]: ./media/quick-onboard-windows-computer/compute.png

---
title: Einbinden von Windows-Computern in Azure Security Center
description: Dieser Schnellstart veranschaulicht die Microsoft Monitoring Agent-Bereitstellung auf einem Windows-Computer.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/3/2018
ms.author: memildin
ms.openlocfilehash: 417d8379d019a9ef0da41638cba4a1f9cb7b8bc2
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73686504"
---
# <a name="quickstart-onboard-windows-computers-to-azure-security-center"></a>Schnellstart: Einbinden von Windows-Computern in Azure Security Center
Nachdem Sie Ihre Azure-Abonnements integriert haben, können Sie Security Center für Ressourcen aktivieren, die außerhalb von Azure ausgeführt werden, z.B. lokale Ressourcen oder Ressourcen in anderen Clouds, indem Sie den Microsoft Monitoring Agent bereitstellen.

Dieser Schnellstart veranschaulicht die Microsoft Monitoring Agent-Installation auf einem Windows-Computer.

## <a name="prerequisites"></a>Voraussetzungen
Für den Einstieg in Security Center benötigen Sie ein Microsoft Azure-Abonnement. Wenn Sie nicht über ein Abonnement verfügen, können Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/pricing/free-trial/) registrieren.

Vor dem Start dieses Schnellstarts müssen Sie zum Tarif „Standard“ von Security Center wechseln. Anweisungen zum Upgrade finden Sie unter [Schnellstart: Einbinden Ihres Azure-Abonnements in Security Center Standard](security-center-get-started.md). Sie können Security Center Standard kostenlos testen. Weitere Informationen finden Sie auf der [Preisseite](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="add-new-windows-computer"></a>Hinzufügen eines neuen Windows-Computers

1. Melden Sie sich beim [Azure-Portal](https://azure.microsoft.com/features/azure-portal/) an.
2. Wählen Sie im Menü **Microsoft Azure** die Option **Security Center**. **Security Center – Übersicht** wird geöffnet.

   ![Übersicht über das Security Center][2]

3. Wählen Sie im Security Center im Hauptmenü die Option **Erste Schritte** aus.
4. Wählen Sie die Registerkarte **Erste Schritte** aus.

   ![Erste Schritte][3]

5. Klicken Sie auf **Konfigurieren** unter **Neue Nicht-Azure-Computer hinzufügen**. Es wird eine Liste Ihrer Log Analytics-Arbeitsbereiche angezeigt. Die Liste enthält, falls zutreffend, den Standardarbeitsbereich, der von Security Center für Sie erstellt wurde, wenn die automatische Bereitstellung aktiviert wurde. Wählen Sie diesen Arbeitsbereich oder einen anderen Arbeitsbereich aus, den Sie verwenden möchten.

    ![Hinzufügen eines Azure-fremden Computers](./media/quick-onboard-windows-computer/non-azure.png)

   Das Blatt **Direkt-Agent** wird mit einem Link zum Herunterladen eines Windows- oder Linux-Agents sowie Schlüsseln für Ihre Arbeitsbereichs-ID geöffnet, die Sie zum Konfigurieren des Agents benötigen.

6. Klicken Sie auf den Link **Windows-Agent herunterladen** für den entsprechenden Prozessortyp Ihres Computers, um die Setupdatei herunterzuladen.

7. Klicken Sie rechts von **Arbeitsbereichs-ID** auf das Kopiersymbol, und fügen Sie die ID in Editor ein.

8. Klicken Sie rechts von **Primärschlüssel** auf das Kopiersymbol, und fügen Sie den Schlüssel in Editor ein.

## <a name="install-the-agent"></a>Installieren des Agents
Jetzt müssen Sie die heruntergeladene Datei auf dem Zielcomputer installieren.

1. Kopieren Sie die Datei auf den Zielcomputer, und führen Sie Setup aus.
2. Wählen Sie auf der Seite **Willkommen** die Option **Weiter** aus.
3. Lesen Sie die Seite **Lizenzbedingungen** durch, und klicken Sie anschließend auf **Ich stimme zu**.
4. Auf der Seite **Zielordner** können Sie den Standardinstallationsordner entweder ändern oder beibehalten. Klicken Sie anschließend auf **Weiter**.
5. Stellen Sie über die Seite **Agent-Setupoptionen** eine Verbindung zwischen dem Agent und Azure Log Analytics her, und klicken Sie anschließend auf **Weiter**.
6. Fügen Sie auf der Seite **Azure Log Analytics** die **Arbeitsbereichs-ID** und den **Arbeitsbereichsschlüssel (Primärschlüssel)** ein, die Sie im vorherigen Schritt in Editor kopiert haben.
7. Wenn der Computer in einen Log Analytics-Arbeitsbereich in Azure Government Cloud melden soll, wählen Sie **Azure US-Regierung** in der Dropdownliste **Azure Cloud** aus. Wenn der Computer über einen Proxyserver mit dem Log Analytics-Dienst kommunizieren muss, klicken Sie auf **Erweitert**, und stellen Sie die URL sowie die Portnummer des Proxyservers bereit.
8. Klicken Sie auf **Weiter**, nachdem Sie die Bereitstellung der erforderlichen Konfigurationseinstellungen abgeschlossen haben.

   ![Installieren des Agents][5]

9. Überprüfen Sie Ihre Auswahl auf der Seite **Bereit zum Installieren**, und klicken Sie dann auf **Installieren**.
10. Klicken Sie auf der Seite **Die Konfiguration wurde erfolgreich abgeschlossen** auf **Fertig stellen**.

Nach Abschluss wird der **Microsoft Monitoring Agent** in der **Systemsteuerung** angezeigt. Sie können hier Ihre Konfiguration überprüfen und sicherstellen, dass der Agent verbunden ist.

Weitere Informationen zur Installation und Konfiguration des Agents finden Sie unter [Verbinden von Windows-Computern](../azure-monitor/platform/agent-windows.md#install-the-agent-using-setup-wizard).

Jetzt können Sie Ihre Azure-VMs und Nicht-Azure-Computer zentral überwachen. Unter **Compute** haben Sie einen Überblick über alle VMs und Computer mit Empfehlungen. Jede Spalte steht für eine Gruppe von Empfehlungen. Die Farbe stellt den aktuellen Sicherheitsstatus der VM oder des Computers für diese Empfehlung dar. In Security Center werden in Sicherheitswarnungen außerdem alle erkannten Bedrohungen für diese Computer angezeigt.

  ![Blatt „Compute“][6]

Auf dem Blatt **Compute** werden zwei Arten von Symbolen dargestellt:

![icon1](./media/quick-onboard-windows-computer/security-center-monitoring-icon1.png) Nicht-Azure-Computer

![icon2](./media/quick-onboard-windows-computer/security-center-monitoring-icon2.png) Azure VM

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn er nicht mehr benötigt wird, können Sie den Agent vom Windows-Computer entfernen.

So entfernen Sie den Agent

1. Öffnen Sie die **Systemsteuerung**.
2. Öffnen Sie **Programme und Features**.
3. Wählen Sie in **Programme und Features** die Option **Microsoft Monitoring Agent** aus, und klicken Sie auf **Deinstallieren**.

## <a name="next-steps"></a>Nächste Schritte
In diesem Schnellstart haben Sie den Microsoft Monitoring Agent auf einem Windows-Computer bereitgestellt. Weitere Informationen zur Verwendung von Security Center erhalten Sie, wenn Sie mit dem Tutorial zur Konfiguration einer Sicherheitsrichtlinie und zum Bewerten der Sicherheit Ihrer Ressourcen fortfahren.

> [!div class="nextstepaction"]
> [Tutorial: Definieren und Bewerten von Sicherheitsrichtlinien](tutorial-security-policy.md)

<!--Image references-->
[2]: ./media/quick-onboard-windows-computer/overview.png
[3]: ./media/quick-onboard-windows-computer/get-started.png
[4]: ./media/quick-onboard-windows-computer/add-computer.png
[5]: ./media/quick-onboard-windows-computer/log-analytics-mma-setup-laworkspace.png
[6]: ./media/quick-onboard-windows-computer/compute.png

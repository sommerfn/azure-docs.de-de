---
title: Onboarding von Linux-Computern in Azure Security Center | Microsoft-Dokumentation
description: Dieser Schnellstart zeigt, wie Sie Ihre Linux-Computer in Security Center integrieren.
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
ms.date: 12/02/2018
ms.author: memildin
ms.openlocfilehash: 9f49b858a23d0a1f58505d9f9971a31e8c9167e9
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73664495"
---
# <a name="quickstart-onboard-linux-computers-to-azure-security-center"></a>Schnellstart: Einbinden von Linux-Computern in Azure Security Center
Nachdem Sie Ihre Azure-Abonnements integriert haben, können Sie Security Center für Linux-Ressourcen aktivieren, die außerhalb von Azure (etwa lokal oder in anderen Clouds) ausgeführt werden. Stellen Sie dazu einen Agent bereit. Der Agent heißt Microsoft Monitoring Agent (MMA), wird aber auch als OMS-Agent bezeichnet.

Dieser Schnellstart veranschaulicht die Installation des Agents auf einem Linux-Computer.

## <a name="prerequisites"></a>Voraussetzungen
Für den Einstieg in Security Center benötigen Sie ein Microsoft Azure-Abonnement. Wenn Sie nicht über ein Abonnement verfügen, können Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/pricing/free-trial/) registrieren.

Vor dem Start dieses Schnellstarts müssen Sie zum Tarif „Standard“ von Security Center wechseln. Anweisungen zum Upgrade finden Sie unter [Schnellstart: Einbinden Ihres Azure-Abonnements in Security Center Standard](security-center-get-started.md). Sie können Security Center Standard kostenlos testen. Weitere Informationen finden Sie auf der [Preisseite](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="add-new-linux-computer"></a>Hinzufügen eines neuen Linux-Computers

1. Melden Sie sich beim [Azure-Portal](https://azure.microsoft.com/features/azure-portal/) an.
2. Wählen Sie im Menü **Microsoft Azure** die Option **Security Center**. **Security Center – Übersicht** wird geöffnet.

   ![Übersicht über das Security Center][2]

3. Wählen Sie im Security Center im Hauptmenü die Option **Erste Schritte** aus.
4. Wählen Sie die Registerkarte **Erste Schritte**. ![Erste Schritte][3]

5. Klicken Sie auf **Konfigurieren** unter **Neue Nicht-Azure-Computer hinzufügen**. Anschließend wird eine Liste Ihrer Log Analytics-Arbeitsbereiche angezeigt. Die Liste enthält, falls zutreffend, den Standardarbeitsbereich, der von Security Center für Sie erstellt wurde, wenn die automatische Bereitstellung aktiviert wurde. Wählen Sie diesen Arbeitsbereich oder einen anderen Arbeitsbereich aus, den Sie verwenden möchten.

    ![Hinzufügen eines Azure-fremden Computers](./media/quick-onboard-linux-computer/non-azure.png)

6. Klicken Sie auf der Seite **Direkt-Agent** unter **OMS-Agent für Linux herunterladen und integrieren** auf die Schaltfläche **Kopieren**, um den Befehl *wget* zu kopieren.

7. Öffnen Sie Editor, und fügen Sie diesen Befehl ein. Speichern Sie diese Datei an einem Speicherort, auf den auf Ihrem Linux-Computer zugegriffen werden kann.

## <a name="install-the-agent"></a>Installieren des Agents

1. Öffnen Sie auf dem Linux-Computer die Datei, die zuvor gespeichert wurde. Wählen Sie den gesamten Inhalt aus, und kopieren Sie ihn. Öffnen Sie eine Terminalkonsole, und fügen Sie den Befehl ein.
2. Nachdem die Installation abgeschlossen ist, können Sie überprüfen, ob *omsagent* installiert ist, indem Sie den Befehl *pgrep* ausführen. Der Befehl gibt die PID (Prozess-ID) von *omsagent* zurück (siehe unten):

   ![Installieren des Agents][5]

Die Protokolle für den Agent befinden sich unter */var/opt/microsoft/omsagent/\<Arbeitsbereichs-ID>/log/* :

  ![Protokolle für Agent][6]

Nach einiger Zeit (bis zu 30 Minuten) wird der neue Linux-Computer in Security Center angezeigt.

Jetzt können Sie Ihre Azure-VMs und Nicht-Azure-Computer zentral überwachen. Unter **Compute** haben Sie einen Überblick über alle VMs und Computer mit Empfehlungen. Jede Spalte steht für eine Gruppe von Empfehlungen. Die Farbe stellt den aktuellen Sicherheitsstatus der VM oder des Computers für diese Empfehlung dar. In Security Center werden in Sicherheitswarnungen außerdem alle erkannten Bedrohungen für diese Computer angezeigt.

  ![Blatt „Compute“][7] Auf dem Blatt **Compute** werden zwei Arten von Symbolen dargestellt:

  ![icon1](./media/quick-onboard-linux-computer/security-center-monitoring-icon1.png) Nicht-Azure-Computer

  ![icon2](./media/quick-onboard-linux-computer/security-center-monitoring-icon2.png) Azure VM

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn er nicht mehr benötigt wird, können Sie den Agent vom Linux-Computer entfernen.

So entfernen Sie den Agent

1. Laden Sie das [universelle Skript](https://github.com/Microsoft/OMS-Agent-for-Linux/releases) des Linux-Agents auf den Computer herunter.
2. Führen Sie die SH-Bündeldatei mit dem Argument *--purge* auf dem Computer aus, um den Agent und dessen Konfiguration vollständig zu entfernen.

    `sudo sh ./omsagent-<version>.universal.x64.sh --purge`

## <a name="next-steps"></a>Nächste Schritte
In diesem Schnellstart haben Sie den Agent auf einem Linux-Computer bereitgestellt. Weitere Informationen zur Verwendung von Security Center erhalten Sie, wenn Sie mit dem Tutorial zur Konfiguration einer Sicherheitsrichtlinie und zum Bewerten der Sicherheit Ihrer Ressourcen fortfahren.

> [!div class="nextstepaction"]
> [Tutorial: Definieren und Bewerten von Sicherheitsrichtlinien](tutorial-security-policy.md)

<!--Image references-->
[1]: ./media/quick-onboard-linux-computer/portal.png
[2]: ./media/quick-onboard-linux-computer/overview.png
[3]: ./media/quick-onboard-linux-computer/get-started.png
[4]: ./media/quick-onboard-linux-computer/add-computer.png
[5]: ./media/quick-onboard-linux-computer/pgrep-command.png
[6]: ./media/quick-onboard-linux-computer/logs-for-agent.png
[7]: ./media/quick-onboard-linux-computer/compute.png

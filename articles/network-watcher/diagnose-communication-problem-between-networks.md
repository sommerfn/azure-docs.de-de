---
title: 'Tutorial: Diagnostizieren eines Problems mit der Kommunikation zwischen Netzwerken – Azure-Portal | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie ein Kommunikationsproblem zwischen einem virtuellen Azure-Netzwerk und einem mit diesem über ein Gateway für virtuelle Azure-Netzwerke verbundenen lokalen oder anderen virtuellen Netzwerk mit der VPN-Diagnosefunktion von Network Watcher diagnostizieren.
services: network-watcher
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
Customer intent: I need to determine why resources in a virtual network can't communicate with resources in a different network.
ms.service: network-watcher
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: e4dc66a5ca6e18b12586b5b03742fba3d6c3d6c4
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2019
ms.locfileid: "64704216"
---
# <a name="tutorial-diagnose-a-communication-problem-between-networks-using-the-azure-portal"></a>Tutorial: Diagnostizieren eines Problems mit der Kommunikation zwischen Netzwerken mit dem Azure-Portal

Ein Gateway für virtuelle Netzwerke verbindet ein virtuelles Azure-Netzwerk mit einem lokalen oder sonstigen virtuellen Netzwerk. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Diagnostizieren eines Problems mit einem Gateway für virtuelle Netzwerke mit der VPN-Diagnosefunktion von Network Watcher.
> * Diagnostizieren eines Problems mit einer Gatewayverbindung
> * Lösen eines Problems mit einem Gateway

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung der VPN-Diagnose benötigen Sie ein aktives VPN-Gateway. Wenn Ihnen kein VPN-Gateway zur Diagnose zur Verfügung steht, können Sie mit einem [PowerShell-Skript](../vpn-gateway/scripts/vpn-gateway-sample-site-to-site-powershell.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) eines bereitstellen. Sie können das PowerShell-Skript ausführen von:
- **Lokale PowerShell-Installation:** Dieses Skript erfordert das Azure PowerShell-Modul `Az`. Führen Sie `Get-Module -ListAvailable Az` aus, um die installierte Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-Az-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzAccount` ausführen, um eine Verbindung mit Azure herzustellen.
- **Azure Cloud Shell:** Der Dienst [Azure Cloud Shell](https://shell.azure.com/powershell), in dem die neueste Version von PowerShell installiert und konfiguriert ist, meldet Sie bei Azure an.

Das Skript benötigt ungefähr eine Stunde, um ein VPN-Gateway zu erstellen. Die restlichen Schritte setzen voraus, dass das Gateway, das Sie diagnostizieren, mit dem identisch ist, das von diesem Skript bereitgestellt wird. Wenn Sie stattdessen Ihr eigenes vorhandenes Gateway diagnostizieren, variieren Ihre Ergebnisse.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="enable-network-watcher"></a>Aktivieren von Network Watcher

Wenn Sie bereits eine Network Watcher-Instanz in der Region „USA, Osten“ aktiviert haben, fahren Sie mit [Diagnostizieren eines Gateways](#diagnose-a-gateway) fort.

1. Wählen Sie im Portal die Option **Alle Dienste** aus. Geben Sie im Feld **Filter** die Zeichenfolge *Network Watcher* ein. Wenn **Network Watcher** in den Ergebnissen angezeigt wird, wählen Sie ihn aus.
2. Wählen Sie die **Regionen** aus, um sie zu erweitern, und wählen Sie dann **...** rechts von **USA, Osten** aus (siehe folgende Abbildung):

    ![Aktivieren von Network Watcher](./media/diagnose-communication-problem-between-networks/enable-network-watcher.png)

3. Klicken Sie auf **Network Watcher aktivieren**.

## <a name="diagnose-a-gateway"></a>Diagnostizieren eines Gateways

1. Wählen Sie im Portal auf der linken Seite **Alle Dienste** aus.
2. Beginnen Sie mit der Eingabe von *Network Watcher* im Feld **Filter**. Wählen Sie **Network Watcher** aus, wenn der Begriff in den Suchergebnissen angezeigt wird.
3. Wählen Sie unter **NETZWERKDIAGNOSETOOLS** die Option **VPN-Diagnose** aus.
4. Wählen Sie **Speicherkonto** und dann das Speicherkonto aus, in das Diagnoseinformationen geschrieben werden sollen.
5. Wählen Sie in der Liste **Speicherkonten** das Speicherkonto aus, das Sie nutzen möchten. Wenn Sie kein Speicherkonto besitzen, wählen Sie **+ Speicherkonto** aus, geben Sie die erforderlichen Informationen ein, oder wählen Sie sie aus, und wählen Sie dann **Erstellen**, um eines zu erstellen. Wenn Sie mithilfe des Skripts in [Voraussetzungen](#prerequisites) ein VPN-Gateway erstellt haben, können Sie das Speicherkonto in der gleichen Ressourcengruppe wie das Gateway erstellen – *TestRG1*.
6. Wählen Sie in der Liste der **Container** den Container aus, den Sie verwenden möchten, und wählen Sie dann **Auswählen** aus. Wenn Sie nicht über Container verfügen, wählen Sie **+ Container** aus, geben Sie einen Namen für den Container ein, und wählen Sie dann **OK** aus.
7. Wählen Sie ein Gateway aus, und wählen Sie dann **Mit der Problembehandlung beginnen** aus. Wie in der folgenden Abbildung gezeigt, wird der Test für ein Gateway mit dem Namen **Vnet1GW** ausgeführt:

    ![VPN-Diagnose](./media/diagnose-communication-problem-between-networks/vpn-diagnostics.png)

8. Während der Testausführung wird **Wird ausgeführt** in der Spalte **PROBLEMBEHANDLUNGSSTATUS** angezeigt, wo in der vorherigen Abbildung **Nicht gestartet** angezeigt wird. Der Test nimmt möglicherweise einige Minuten in Anspruch.
9. Zeigen Sie den Status eines abgeschlossenen Tests an. Die folgende Abbildung zeigt die Statusergebnisse eines abgeschlossenen Diagnosetests:

    ![Status](./media/diagnose-communication-problem-between-networks/status.png)

    Für den **PROBLEMBEHANDLUNGSSTATUS** wird **Fehlerhaft** angezeigt, außerdem sehen Sie eine **Zusammenfassung** des Problems und eine Beschreibung im **Detail** auf der Registerkarte **Status**.
10. Bei Auswahl der Registerkarte **Aktion** bietet die VPN-Diagnose zusätzliche Informationen. In dem in der folgenden Abbildung gezeigten Beispiel teilt die VPN-Diagnose Ihnen mit, dass Sie die Integrität jeder Verbindung überprüfen sollten:

    ![Aktion](./media/diagnose-communication-problem-between-networks/action.png)

## <a name="diagnose-a-gateway-connection"></a>Diagnostizieren einer Gatewayverbindung

Ein Gateway ist über eine Gatewayverbindung mit anderen Netzwerken verbunden. Gateway und Gatewayverbindungen müssen für eine erfolgreiche Kommunikation zwischen einem virtuellen Netzwerk und einem verbundenen Netzwerk fehlerfrei sein.

1. Schließen Sie Schritt 7 von [Diagnostizieren eines Gateways](#diagnose-a-gateway) erneut ab, und wählen Sie nun eine Verbindung aus. Im folgenden Beispiel wird eine Verbindung mit dem Namen **VNet1toSite1** getestet:

    ![Verbindung](./media/diagnose-communication-problem-between-networks/connection.png)

    Der Test dauert mehrere Minuten.
2. Nachdem der Test der Verbindung abgeschlossen ist, erhalten Sie Ergebnisse, die den Ergebnissen in den folgenden Abbildungen auf den Registerkarten **Status** und **Aktion** ähneln:

    ![Verbindungsstatus](./media/diagnose-communication-problem-between-networks/connection-status.png)

    ![Verbindungsaktion](./media/diagnose-communication-problem-between-networks/connection-action.png)

    Die VPN-Diagnose informiert Sie auf der Registerkarte **Status**, wo das Problem liegt, und bietet Ihnen auf der Registerkarte **Aktion** einige Vorschläge zur möglichen Ursache des Problems.

    Wenn das Gateway, das Sie getestet haben, identisch ist mit dem, das vom [Skript](../vpn-gateway/scripts/vpn-gateway-sample-site-to-site-powershell.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) in [Voraussetzungen](#prerequisites) bereitgestellt wurde, dann wird das Problem durch das auf der Registerkarte **Status** aufgeführte Problem und die ersten beiden Elemente auf der Registerkarte **Aktion** genau beschrieben. Das Skript konfiguriert eine Platzhalter-IP-Adresse, 23.99.221.164, für das lokale VPN-Gatewaygerät.

    Um das Problem zu beheben, müssen Sie sicherstellen, dass Ihr lokales VPN-Gateway [ordnungsgemäß konfiguriert](../vpn-gateway/vpn-gateway-about-vpn-devices.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) ist, und die durch das Skript für das lokale Netzwerkgateway konfigurierte IP-Adresse in die tatsächliche öffentliche Adresse Ihres lokalen VPN-Gateways ändern.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mithilfe des Skripts ein VPN-Gateway in den [Voraussetzungen](#prerequisites) ausschließlich erstellt haben, um dieses Tutorial abzuschließen, und es nicht mehr benötigen, löschen Sie die Ressourcengruppe und alle darin enthaltenen Ressourcen:

1. Geben Sie oben im Portal *TestRG1* im Feld **Suche** ein. Wenn Sie **TestRG1** in den Suchergebnissen finden, wählen Sie dies aus.
2. Wählen Sie die Option **Ressourcengruppe löschen**.
3. Geben Sie *TestRG1* für **GEBEN SIE DEN RESSOURCENGRUPPENNAMEN EIN:**, und wählen Sie **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie ein Problem mit einem Gateway für virtuelle Netzwerke diagnostizieren. Möglicherweise möchten Sie die Netzwerkkommunikation mit einem virtuellen Computer protokollieren, damit Sie das Protokoll auf Anomalien überprüfen können. Wie Sie dies durchführen, erfahren Sie im nächsten Tutorial.

> [!div class="nextstepaction"]
> [Protokollieren des ein- und ausgehenden Netzwerkdatenverkehrs eines virtuellen Computers](network-watcher-nsg-flow-logging-portal.md)

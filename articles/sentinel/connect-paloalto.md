---
title: Verknüpfen von Palo Alto Networks-Daten mit der Vorschauversion von Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Palo Alto Networks-Daten mit Azure Sentinel verknüpfen.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: a4b21d67-1a72-40ec-bfce-d79a8707b4e1
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2019
ms.author: rkarlin
ms.openlocfilehash: 5860e1b1b817985aafd95f6f63d39553489482b9
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68679232"
---
# <a name="connect-your-palo-alto-networks-appliance"></a>Verbinden Ihrer Palo Alto Networks-Appliance

> [!IMPORTANT]
> Azure Sentinel ist aktuell als Public Preview verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Sie können Azure Sentinel mit jeder Palo Alto Networks-Appliance verbinden, indem Sie die Protokolldateien im Syslog Common Error Format (CEF) speichern. Die Integration mit Azure Sentinel ermöglicht Ihnen die einfache Ausführung von Analysen und Abfragen von Daten der Protokolldateien von Palo Alto Networks. Weitere Informationen dazu, wie Azure Sentinel CEF-Daten erfasst, finden Sie unter [Verbinden der externen Lösung mithilfe von Common Event Format](connect-common-event-format.md).

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="step-1-connect-your-palo-alto-networks-appliance-using-an-agent"></a>Schritt 1: Verbinden Ihrer Palo Alto Networks-Appliance mithilfe eines Agents

Sie müssen einen Agent auf einem dedizierten Computer (VM oder lokal) bereitstellen, um Ihre Palo Alto Networks-Appliance mit Azure Sentinel zu verbinden und Kommunikation zwischen der Appliance und Azure Sentinel zu ermöglichen. 

Alternativ können Sie den Agent manuell auf einem vorhandenen virtuellen Azure-Computer, auf einem virtuellen Computer in einer anderen Cloud oder auf einem lokalen Computer bereitstellen.

> [!NOTE]
> Stellen Sie sicher, dass Sie die Sicherheit des Computers gemäß der Sicherheitsrichtlinie Ihrer Organisation konfigurieren. Beispielsweise können Sie das Netzwerk so konfigurieren, dass es der Sicherheitsrichtlinie für das Unternehmensnetzwerk entspricht, und die Ports und Protokolle im Daemon Ihren Anforderungen entsprechend ändern. 

Netzwerkdiagramme zu beiden Optionen finden Sie unter [Herstellen einer Verbindung mit Datenquellen](connect-data-sources.md#agent-options).


### <a name="deploy-the-agent"></a>Bereitstellen des Agents 

1. Klicken Sie im Azure Sentinel-Portal auf **Data connectors** (Datenconnectors), und wählen Sie **Palo Alto Networks** und dann **Open connector page** (Connectorseite öffnen) aus. 

1. Wählen Sie unter **Download and install the Syslog agent** (Syslog-Agent herunterladen und installieren) Ihren Computertyp aus: entweder Azure oder lokal. 
1. Wählen Sie auf dem daraufhin geöffneten Bildschirm **Virtuelle Computer** den Computer aus, den Sie verwenden möchten, und klicken Sie dann auf **Verbinden**.
1. Wenn Sie **Download and install agent for Azure Linux virtual machines** (Agent für virtuelle Azure Linux-Computer herunterladen und installieren) auswählen, wählen Sie den Computer aus, und klicken Sie auf **Verbinden**. Wenn Sie **Download and install agent for non-Azure Linux virtual machines** (Agent für Azure-fremde virtuelle Linux-Computer herunterladen und installieren) im Fenster **Direkt-Agent** auswählen, führen Sie das Skript unter **Agent für Linux herunterladen und integrieren** aus.      1. Legen Sie auf dem Connector-Bildschirm unter **Configure and forward Syslog** (Syslog konfigurieren und weiterleiten) fest, welchen Syslog-Daemon (**rsyslog.d** oder **syslog-ng**) Sie verwenden. 
1. Kopieren Sie die folgenden Befehle, und führen Sie sie auf der Appliance aus:
     - Wenn Sie „rsyslog.d“ ausgewählt haben, gehen Sie wie folgt vor:
              
       1. Weisen Sie den Syslog-Daemon an, in der Umgebung „local_4“ zu lauschen und die Syslog-Nachrichten über den Port 25226 an den Azure Sentinel-Agent zu senden. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            
       2. Laden Sie die [Konfigurationsdatei „security_events“](https://aka.ms/asi-syslog-config-file-linux) herunter, mit der der Syslog-Agent zum Lauschen am Port 25226 konfiguriert wird, und installieren Sie sie. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Ersetzen Sie hier {0} durch die GUID Ihres Arbeitsbereichs.
            
       1. Starten Sie den Syslog-Daemon neu: `sudo service rsyslog restart`
             
    - Wenn Sie „syslog-ng“ ausgewählt haben, gehen Sie wie folgt vor:

         1. Weisen Sie den Syslog-Daemon an, in der Umgebung „local_4“ zu lauschen und die Syslog-Nachrichten über den Port 25226 an den Azure Sentinel-Agent zu senden. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
         2. Laden Sie die [Konfigurationsdatei „security_events“](https://aka.ms/asi-syslog-config-file-linux) herunter, mit der der Syslog-Agent zum Lauschen am Port 25226 konfiguriert wird, und installieren Sie sie. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Ersetzen Sie hier {0} durch die GUID Ihres Arbeitsbereichs.

         3. Starten Sie den Syslog-Daemon neu: `sudo service syslog-ng restart`
 1. Starten Sie den Syslog-Agent mit dem folgenden Befehl neu: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
 1. Vergewissern Sie sich, dass im Agent-Protokoll keine Fehler enthalten sind, indem Sie den folgenden Befehl ausführen: `tail /var/opt/microsoft/omsagent/log/omsagent.log`


 
## <a name="step-2-forward-palo-alto-networks-logs-to-the-syslog-agent"></a>Schritt 2: Weiterleiten der Palo Alto Networks-Protokolle an den Syslog-Agent

Konfigurieren Sie Palo Alto Networks, um Syslog-Nachrichten im CEF-Format über den Syslog-Agent an Ihren Azure-Arbeitsbereich weiterzuleiten.
1.  Wechseln Sie zu [Common Event Format (CEF) Configuration Guides](https://docs.paloaltonetworks.com/resources/cef), und laden Sie die PDF-Datei für den Typ Ihrer Appliance herunter. Führen Sie alle Anweisungen in der Anleitung aus, um Ihre Palo Alto Networks-Appliance so einzurichten, dass sie CEF-Ereignisse erfasst. 

1.  Wechseln Sie zu [Configure Syslog monitoring](https://aka.ms/asi-syslog-paloalto-forwarding) (Konfigurieren der Syslog-Überwachung), und führen Sie die Schritte 2 und 3 aus, um CEF-Ereignisweiterleitung von Ihrer Palo Alto Networks-Appliance an Azure Sentinel zu konfigurieren.

    1. Legen Sie die Option **Syslog server format** (Format des Syslog-Servers) auf **BSD** fest.
    1. Legen Sie die Option **Facility number** (Einrichtungsanzahl) auf genau den Wert fest, den Sie im Syslog-Agent festgelegt haben.

       > [!NOTE]
       > Bei den Kopieren/Einfügen-Vorgängen aus der PDF-Datei kann es vorkommen, dass Text geändert wird und zufällige Zeichen eingefügt werden. Um dies zu vermeiden, kopieren Sie den Text in einen Editor, und entfernen Sie, bevor Sie den Text einfügen, alle Zeichen, die das Protokollformat beeinträchtigen könnten, wie dies im folgenden Beispiel zu sehen ist.
 
        ![Problem beim Kopieren von CEF-Text](./media/connect-cef/paloalto-text-prob1.png)

2. Um das relevante Schema in Log Analytics für die Palo Alto Networks-Ereignisse zu verwenden, suchen Sie nach **CommonSecurityLog**.

## <a name="step-3-validate-connectivity"></a>Schritt 3: Überprüfen der Konnektivität

Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden. 

1. Stellen Sie sicher, dass Sie die richtige Einrichtung verwenden. Die Einrichtung muss in Ihrem Gerät und in Azure Sentinel identisch sein. Sie können überprüfen, welche Einrichtungsdatei Sie in Azure Sentinel verwenden, und sie in der Datei `security-config-omsagent.conf` ändern. 

2. Stellen Sie sicher, dass für Ihre Protokolle der richtige Port im Syslog-Agent verwendet wird. Führen Sie diesen Befehl auf dem Syslog-Agent-Computer aus: `tcpdump -A -ni any  port 514 -vv` Durch diesen Befehl werden die Protokolle angezeigt, die vom Gerät an den Syslog-Computer übermittelt werden. Stellen Sie sicher, dass Protokolle von der Quellappliance über den richtigen Port und die richtige Einrichtung empfangen werden.

3. Stellen Sie sicher, dass die Protokolle, die Sie senden [RFC 3164](https://tools.ietf.org/html/rfc3164) erfüllen.

4. Auf dem Computer, auf dem der Syslog-Agent ausgeführt wird, stellen Sie sicher, dass die Ports 514 und 25226 geöffnet sind und lauschen, indem Sie den Befehl `netstat -a -n:` verwenden. Weitere Informationen zur Verwendung dieses Befehls finden Sie auf der [Linux-Manpage zu „netstat(8)“](https://linux.die.net/man/8/netstat). Wenn sie ordnungsgemäß lauschen, wird Folgendes angezeigt:

   ![Azure Sentinel-Ports](./media/connect-cef/ports.png) 

5. Stellen Sie sicher, dass der Daemon darauf eingestellt ist, an Port 514 zu lauschen, an dem Sie die Protokolle senden.
    - Für rsyslog:<br>Stellen Sie sicher, dass die Datei `/etc/rsyslog.conf` diese Konfiguration enthält:

           # provides UDP syslog reception
           module(load="imudp")
           input(type="imudp" port="514")
        
           # provides TCP syslog reception
           module(load="imtcp")
           input(type="imtcp" port="514")

      Weitere Informationen finden Sie unter [imudp: UDP-Syslog-Eingabemodul](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imudp.html#imudp-udp-syslog-input-module) und [imtcp: TCP-Syslog-Eingabemodul](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imtcp.html#imtcp-tcp-syslog-input-module).

   - Für syslog-ng:<br>Stellen Sie sicher, dass die Datei `/etc/syslog-ng/syslog-ng.conf` diese Konfiguration enthält:

           # source s_network {
            network( transport(UDP) port(514));
             };
     Weitere Informationen finden Sie im [Administratorhandbuch zu syslog-ng Open Source Edition 3.16](https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.16/administration-guide/19#TOPIC-956455).

1. Stellen Sie sicher, dass der Syslog-Daemon und der Agent kommunizieren. Führen Sie diesen Befehl auf dem Syslog-Agent-Computer aus: `tcpdump -A -ni any  port 25226 -vv` Durch diesen Befehl werden die Protokolle angezeigt, die vom Gerät an den Syslog-Computer übermittelt werden. Vergewissern Sie sich, dass die Protokolle auch vom Agent empfangen werden.

6. Wenn diese beiden Befehle erfolgreiche Ergebnisse zurückgegeben haben, überprüfen Sie in Log Analytics, ob Ihre Protokolle eingehen. Alle Ereignisse, die von diesen Appliances übermittelt werden, werden in Log Analytics unformatiert unter dem Typ `CommonSecurityLog` angezeigt.

7. Hier können Sie überprüfen, ob Fehler aufgetreten sind oder die Protokolle nicht eingehen: `tail /var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log`. Wenn dort Fehler wegen fehlender Protokollformatübereinstimmung aufgeführt werden, wechseln Sie zu `/etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"`, und sehen Sie sich die Datei `security_events.conf` an, und stellen Sie sicher, dass Ihre Protokolle das in dieser Datei angezeigte reguläre Ausdrucksformat einhalten.

8. Vergewissern Sie sich, dass die Standardgröße der Syslog-Nachrichten auf 2.048 Bytes (2 KB) beschränkt ist. Wenn die Protokolle zu lang sind, aktualisieren Sie die Konfigurationsdatei „security_events.conf“ mithilfe des folgenden Befehls: `message_length_limit 4096`








## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie Palo Alto Networks-Appliances mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats.md).


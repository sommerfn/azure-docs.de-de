---
title: Verknüpfen von Fortinet-Daten mit der Vorschauversion von Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Fortinet-Daten mit Azure Sentinel verknüpfen.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: add92907-0d7c-42b8-a773-f570f2d705ff
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2019
ms.author: rkarlin
ms.openlocfilehash: 339b8c1b59720989016f68fdb94fae30c26b42f0
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68679283"
---
# <a name="connect-your-fortinet-appliance"></a>Herstellen einer Verbindung mit Ihrer Fortinet-Appliance

> [!IMPORTANT]
> Azure Sentinel ist derzeit als öffentliche Vorschauversion (Public Preview) verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt. Sie sollte nicht für Produktionsworkloads verwendet werden. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Sie können Azure Sentinel mit jeder Fortinet-Appliance verbinden, indem Sie die Protokolldateien als „Syslog CEF“ (Common Event Format) speichern. Die Azure Sentinel-Integration ermöglicht die einfache Ausführung von Analysen und Abfragen für Protokolldateidaten von Fortinet. Weitere Informationen dazu, wie Azure Sentinel CEF-Daten erfasst, finden Sie unter [Verbinden der externen Lösung mithilfe von Common Event Format](connect-common-event-format.md).

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="step-1-connect-your-fortinet-appliance-by-using-an-agent"></a>Schritt 1: Verbinden Ihrer Fortinet-Appliance mithilfe eines Agents

Um Ihre Fortinet-Appliance mit Azure Sentinel zu verbinden und die Kommunikation zwischen der Appliance und Azure Sentinel zu ermöglichen, muss ein Agent auf einem dedizierten Computer (virtuell oder lokal) bereitgestellt werden. 

Sie können den Agent auch manuell auf einem vorhandenen virtuellen Azure-Computer, auf einem virtuellen Computer in einer anderen Cloud oder auf einem lokalen Computer bereitstellen.

> [!NOTE]
> Stellen Sie sicher, dass Sie die Sicherheit des Computers gemäß der Sicherheitsrichtlinie Ihrer Organisation konfigurieren. Beispielsweise können Sie das Netzwerk so konfigurieren, dass es der Sicherheitsrichtlinie für das Unternehmensnetzwerk entspricht, und die Ports und Protokolle im Daemon Ihren Anforderungen entsprechend ändern. 

Netzwerkdiagramme zu beiden Optionen finden Sie unter [Herstellen einer Verbindung mit Datenquellen](connect-data-sources.md#agent-options).

### <a name="deploy-the-agent"></a>Bereitstellen des Agents

1. Klicken Sie im Azure Sentinel-Portal auf **Data connectors** (Datenconnectors), und wählen Sie **Fortinet** und dann **Open connector page** (Connectorseite öffnen) aus. 

1. Wählen Sie unter **Download and install the Syslog agent** (Syslog-Agent herunterladen und installieren) Ihren Computertyp aus: entweder Azure oder lokal. 
1. Wählen Sie auf dem daraufhin geöffneten Bildschirm **Virtuelle Computer** den Computer aus, den Sie verwenden möchten, und klicken Sie dann auf **Verbinden**.
1. Wenn Sie **Download and install agent for Azure Linux virtual machines** (Agent für virtuelle Azure Linux-Computer herunterladen und installieren) auswählen, wählen Sie den Computer aus, und klicken Sie auf **Verbinden**. Wenn Sie **Download and install agent for non-Azure Linux virtual machines** (Agent für Azure-fremde virtuelle Linux-Computer herunterladen und installieren) im Fenster **Direkt-Agent** auswählen, führen Sie das Skript unter **Agent für Linux herunterladen und integrieren** aus.
1. Legen Sie auf dem Connectorbildschirm unter **Configure and forward Syslog** (Syslog konfigurieren und weiterleiten) den verwendeten Syslog-Daemon (**rsyslog.d** oder **syslog-ng**) fest. 
1. Kopieren Sie die folgenden Befehle, und führen Sie sie auf der Appliance aus:
   - Wenn Sie „rsyslog.d“ ausgewählt haben, gehen Sie wie folgt vor:
            
     1. Weisen Sie den Syslog-Daemon an, in der Einrichtung „local_4“ zu lauschen und die Syslog-Nachrichten über den Port 25226 an den Azure Sentinel-Agent zu senden. Verwenden Sie den folgenden Befehl: `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226\n\n:msg, contains, \"Fortinet\"  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
     1. Laden Sie die [Konfigurationsdatei „security_events“](https://aka.ms/asi-syslog-config-file-linux) herunter, mit der der Syslog-Agent zum Lauschen am Port 25226 konfiguriert wird, und installieren Sie sie. Verwenden Sie den folgenden Befehl: `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` ({0} muss durch die GUID Ihres Arbeitsbereichs ersetzt werden.)
     1. Starten Sie den Syslog-Daemon mithilfe des folgenden Befehls neu: `sudo service rsyslog restart`
            
   - Wenn Sie „syslog-ng“ ausgewählt haben, gehen Sie wie folgt vor:

      1. Weisen Sie den Syslog-Daemon an, in der Einrichtung „local_4“ zu lauschen und die Syslog-Nachrichten über den Port 25226 an den Azure Sentinel-Agent zu senden. Verwenden Sie den folgenden Befehl: `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };\n\nfilter f_msg_oms { match(\"Fortinet\" value(\"MESSAGE\")); };\n  destination security_msg_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_msg_oms); destination(security_msg_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
      1. Laden Sie die [Konfigurationsdatei „security_events“](https://aka.ms/asi-syslog-config-file-linux) herunter, mit der der Syslog-Agent zum Lauschen am Port 25226 konfiguriert wird, und installieren Sie sie. Verwenden Sie den folgenden Befehl: `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` ({0} muss durch die GUID Ihres Arbeitsbereichs ersetzt werden.)
      1. Starten Sie den Syslog-Daemon mithilfe des folgenden Befehls neu: `sudo service syslog-ng restart`
1. Starten Sie den Syslog-Agent mithilfe des folgenden Befehls neu: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
1. Vergewissern Sie sich, dass im Agent-Protokoll keine Fehler enthalten sind, indem Sie den folgenden Befehl ausführen: `tail /var/opt/microsoft/omsagent/log/omsagent.log`

 
## <a name="step-2-forward-fortinet-logs-to-the-syslog-agent"></a>Schritt 2: Weiterleiten von Fortinet-Protokollen an den Syslog-Agent

Konfigurieren Sie Fortinet, um Syslog-Nachrichten im CEF-Format über den Syslog-Agent an Ihren Azure-Arbeitsbereich weiterzuleiten.

1. Öffnen Sie die Befehlszeilenschnittstelle für Ihre Fortinet-Appliance, und führen Sie die folgenden Befehle aus:

        config log syslogd setting
        set format cef
        set facility <facility_name>
        set port 514
        set reliable disable
        set server <ip_address_of_Receiver>
        set status enable
        end

    - Ersetzen Sie die **IP-Adresse** des Servers durch die IP-Adresse des Agents.
    - Legen Sie **facility_name** auf die Einrichtung fest, die Sie im Agent konfiguriert haben. Der Agent legt diesen Wert standardmäßig auf „local4“ fest.
    - Legen Sie den Syslog-Port **** auf **514** fest (oder auf den Port, den Sie für den Agent festgelegt haben).
    - In frühen Versionen von FortiOS muss zum Aktivieren des CEF-Formats ggf. der Befehl **set csv disable** ausgeführt werden.
 
   > [!NOTE] 
   > Weitere Informationen finden Sie in der [Fortinet-Dokumentbibliothek](https://aka.ms/asi-syslog-fortinet-fortinetdocumentlibrary). Wählen Sie Ihre Version aus, und verwenden Sie das **Handbuch** und die **Referenz für Protokollmeldungen**.

 Suchen Sie nach `CommonSecurityLog`, um das relevante Schema in Azure Monitor Log Analytics für die Fortinet-Ereignisse zu verwenden.


## <a name="step-3-validate-connectivity"></a>Schritt 3: Überprüfen der Konnektivität

Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden. 

1. Stellen Sie sicher, dass Sie die richtige Einrichtung verwenden. Die Einrichtung muss in Ihrem Gerät und in Azure Sentinel identisch sein. Sie können überprüfen, welche Einrichtungsdatei Sie in Azure Sentinel verwenden, und sie in der Datei `security-config-omsagent.conf` ändern. 

2. Stellen Sie sicher, dass für Ihre Protokolle der richtige Port im Syslog-Agent verwendet wird. Führen Sie auf dem Syslog-Agent-Computer den folgenden Befehl aus: `tcpdump -A -ni any  port 514 -vv`. Durch diesen Befehl werden die Protokolle angezeigt, die vom Gerät an den Syslog-Computer gestreamt werden. Stellen Sie sicher, dass Protokolle von der Quellappliance über den richtigen Port und die richtige Einrichtung empfangen werden.

3. Stellen Sie sicher, dass die Protokolle, die Sie senden [RFC 3164](https://tools.ietf.org/html/rfc3164) erfüllen.

4. Vergewissern Sie sich auf dem Computer, auf dem der Syslog-Agent ausgeführt wird, mithilfe des Befehls `netstat -a -n:`, dass die Ports 514 und 25226 geöffnet sind und lauschen. Weitere Informationen zur Verwendung dieses Befehls finden Sie auf der [Linux-Manpage zu „netstat(8)“](https://linux.die.net/man/8/netstat). Wenn ordnungsgemäß gelauscht wird, wird Folgendes angezeigt:

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
     Weitere Informationen finden Sie unter [imudp: UDP-Syslog-Eingabe-Modul](https://rsyslog.readthedocs.io/en/latest/configuration/modules/imudp.html) und [syslog-ng Open Source-Edition 3.16 – Administratorhandbuch](https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.16/administration-guide/19#TOPIC-956455).

1. Vergewissern Sie sich, dass der Syslog-Daemon und der Agent miteinander kommunizieren. Führen Sie auf dem Syslog-Agent-Computer den folgenden Befehl aus: `tcpdump -A -ni any  port 25226 -vv`. Durch diesen Befehl werden die Protokolle angezeigt, die vom Gerät an den Syslog-Computer gestreamt werden. Vergewissern Sie sich, dass die Protokolle auch vom Agent empfangen werden.

6. Wenn diese beiden Befehle erfolgreiche Ergebnisse zurückgegeben haben, überprüfen Sie in Log Analytics, ob Ihre Protokolle eingehen. Alle Ereignisse, die von diesen Appliances übermittelt werden, werden in Log Analytics unformatiert unter dem Typ `CommonSecurityLog` angezeigt.

7. Hier können Sie überprüfen, ob Fehler aufgetreten sind oder die Protokolle nicht eingehen: `tail /var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log`. Sollten Fehler vorhanden sein, die auf Protokollformatkonflikte zurückzuführen sind, wechseln Sie zu `/etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"`, und sehen Sie sich die Datei `security_events.conf` an. Vergewissern Sie sich, dass Ihre Protokolle dem in dieser Datei angezeigten Format für reguläre Ausdrücke entsprechen.

8. Vergewissern Sie sich, dass die Standardgröße der Syslog-Nachrichten auf 2.048 Bytes (2 KB) beschränkt ist. Sollten Protokolle zu lang sein, aktualisieren Sie die Konfigurationsdatei „security_events.conf“ mithilfe des folgenden Befehls: `message_length_limit 4096`

10. Falls der Agent Ihre Fortinet-Protokolle nicht erhält, führen Sie abhängig von der Art des verwendeten Syslog-Daemons den folgenden Befehl aus, um die Einrichtung sowie die Protokolle festzulegen, die nach dem Wort „Fortinet“ durchsucht werden sollen:
       - rsyslog.d: `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226\n\n:msg, contains, \"Fortinet\"  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`

     Starten Sie den Syslog-Daemon mithilfe des folgenden Befehls neu: `sudo service rsyslog restart`
       - syslog-ng: `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };\n\nfilter f_msg_oms { match(\"Fortinet\" value(\"MESSAGE\")); };\n  destination security_msg_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_msg_oms); destination(security_msg_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
      
     Starten Sie den Syslog-Daemon mithilfe des folgenden Befehls neu: `sudo service syslog-ng restart`

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie Fortinet-Appliances mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats.md).


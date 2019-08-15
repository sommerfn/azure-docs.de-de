---
title: Konfigurieren des Azure Log Analytics-Agents für Linux-Hybridcomputer | Microsoft-Dokumentation
description: Hier erfahren Sie, wie der Log Analytics-Agent für Linux bereitgestellt wird, der auf Computern außerhalb von Azure ausgeführt wird, und wie Sie die Sammlung von Daten mit Log Analytics aktivieren.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: quickstart
ms.date: 06/14/2019
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 4484269194aa3d637101a6a0b83eacb268d4c16e
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/12/2019
ms.locfileid: "67147283"
---
# <a name="configure-log-analytics-agent-for-linux-computers-in-a-hybrid-environment"></a>Konfigurieren des Log Analytics-Agents für Linux-Computer in einer Hybridumgebung
[Azure Log Analytics](../platform/agent-windows.md) kann Daten direkt von Ihren physischen oder virtuellen Linux-Computern in Ihrem Datencenter oder einer anderen Cloudumgebung zur detaillierten Analyse und Korrelation in einem einzelnen Repository sammeln. Dieser Schnellstart zeigt Ihnen, wie Sie in wenigen einfachen Schritten Daten von Ihrem Linux-Computer konfigurieren und sammeln.  Informationen zu virtuellen Azure Linux-Computern finden Sie im folgenden Thema [Sammeln von Daten zu Azure Virtual Machines](quick-collect-azurevm.md).  

Bevor Sie fortfahren, sollten Sie zuerst den Artikel mit der [Übersicht](../platform/log-analytics-agent.md) über den Log Analytics-Agent lesen, um die unterstützten Konfigurationen, die [unterstützten Linux-Betriebssysteme](../platform/log-analytics-agent.md#supported-linux-operating-systems) und die [Konfiguration der Netzwerkfirewall](../platform/log-analytics-agent.md#network-firewall-requirements) zu verstehen. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.
Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an. 

## <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs
1. Klicken Sie im Azure-Portal auf **Alle Dienste**. Geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Log Analytics**.

    ![Azure-Portal](media/quick-collect-linux-computer/azure-portal-01.png) 

2. Klicken Sie auf **Erstellen**, und wählen Sie anschließend Optionen für die folgenden Elemente aus:

   * Geben Sie einen Namen für den neuen **Log Analytics-Arbeitsbereich** ein, z.B. *DefaultLAWorkspace*. OMS-Arbeitsbereiche werden jetzt als Log Analytics-Arbeitsbereiche bezeichnet.   
   * Wählen Sie ein **Abonnement** aus, mit dem eine Verknüpfung erstellt werden soll, indem Sie in der Dropdownliste einen anderen Eintrag auswählen, falls der Standardeintrag nicht geeignet ist.
   * Wählen Sie für **Ressourcengruppe** eine vorhandene Ressourcengruppe aus, die einen oder mehrere virtuelle Azure-Computer enthält.  
   * Wählen Sie den **Speicherort** für die Bereitstellung Ihrer virtuellen Computer aus.  Weitere Informationen finden Sie auf der Seite zur [Verfügbarkeit von Log Analytics in den einzelnen Regionen](https://azure.microsoft.com/regions/services/).  
   * Wenn Sie einen Arbeitsbereich in einem neuen Abonnement erstellen, das nach dem 2. April 2018 erstellt wurde, wird automatisch der Tarif *Pro GB* verwendet. In diesem Fall ist keine Tarifauswahloption verfügbar.  Wenn Sie einen Arbeitsbereich für ein Abonnement erstellen, das vor dem 2. April erstellt oder mit einer vorhandenen EA-Registrierung verknüpft wurde, wählen Sie Ihren bevorzugten Tarif aus.  Weitere Informationen zu den einzelnen Tarifen finden Sie unter [Log Analytics – Preise](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Erstellen des Log Analytics-Ressourcenblatts](media/quick-collect-linux-computer/create-loganalytics-workspace-02.png)<br>  

3. Klicken Sie nach dem Bereitstellen der erforderlichen Informationen unter **Log Analytics-Arbeitsbereich** auf **OK**.  

Die Informationen werden überprüft, und der Arbeitsbereich wird erstellt. Sie können den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen. 

## <a name="obtain-workspace-id-and-key"></a>Abrufen von Arbeitsbereichs-ID und -Schlüssel
Vor der Installation des Log Analytics-Agents für Linux benötigen Sie die Arbeitsbereichs-ID und den Schlüssel für Ihren Log Analytics-Arbeitsbereich.  Diese Informationen sind für das Wrapperskript des Agents erforderlich, um den Agent ordnungsgemäß zu konfigurieren und sicherzustellen, dass er erfolgreich mit Log Analytics kommunizieren kann.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]  

1. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**. Geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Log Analytics**.
2. Wählen Sie in der Liste der Log Analytics-Arbeitsbereiche den zuvor erstellten *DefaultLAWorkspace*.
3. Wählen Sie **Erweiterte Einstellungen**.

    ![Erweiterte Einstellungen für Log Analytics](media/quick-collect-linux-computer/log-analytics-advanced-settings-01.png) 
 
4. Wählen Sie **Verbundene Quellen** und dann **Linux Server** aus.   
5. Der Wert rechts von **Arbeitsbereichs-ID** und **Primärschlüssel**. Kopieren Sie beide Angaben, und fügen Sie sie in den von Ihnen bevorzugten Editor ein.   

## <a name="install-the-agent-for-linux"></a>Installieren des Agents für Linux
Die folgenden Schritte konfigurieren das Setup des Agents für Log Analytics in Azure und Azure Government Cloud.  

>[!NOTE]
>Der Log Analytics-Agent für Linux kann nicht für die Berichterstattung für mehrere Log Analytics-Arbeitsbereiche konfiguriert werden.  

Wenn Ihr Linux-Computer über einen Proxyserver mit Log Analytics kommunizieren muss, kann die Proxykonfiguration in der Befehlszeile durch den Zusatz `-p [protocol://][user:password@]proxyhost[:port]` angegeben werden.  Für die *proxyhost*-Eigenschaft kann ein vollqualifizierter Domänenname oder eine IP-Adresse des Proxyservers verwendet werden. 

Beispiel: `https://user01:password@proxy01.contoso.com:30443`

1. Führen Sie den folgenden Befehl aus, und stellen Sie die zuvor kopierte Arbeitsbereichs-ID sowie den Primärschlüssel bereit, um den Linux-Computer so zu konfigurieren, dass er eine Verbindung mit Log Analytics herstellt. Mit dem folgenden Befehl wird der Agent heruntergeladen, die Prüfsumme des Agents überprüft und anschließend der Agent installiert. 
    
    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY>
    ```

    Der folgende Befehl enthält den Proxyparameter `-p` und Beispielsyntax.

   ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -p [protocol://][user:password@]proxyhost[:port] -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY>
    ```

2. Führen Sie den folgenden Befehl aus, und stellen Sie die zuvor kopierte Arbeitsbereichs-ID sowie den Primärschlüssel bereit, um den Linux-Computer so zu konfigurieren, dass er eine Verbindung mit Log Analytics in Azure Government Cloud herstellt. Mit dem folgenden Befehl wird der Agent heruntergeladen, die Prüfsumme des Agents überprüft und anschließend der Agent installiert. 

    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY> -d opinsights.azure.us
    ``` 

    Der folgende Befehl enthält den Proxyparameter `-p` und Beispielsyntax.

   ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -p [protocol://][user:password@]proxyhost[:port] -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY> -d opinsights.azure.us
    ```
2. Starten Sie den Agent neu, indem Sie den folgenden Befehl ausführen: 

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
    ``` 

## <a name="collect-event-and-performance-data"></a>Sammeln von Ereignis- und Leistungsdaten
Log Analytics kann Ereignisdaten aus dem Linux-Syslog und Leistungsindikatoren sammeln, die Sie für längerfristige Analysen und Berichte angeben, und Maßnahmen einleiten, wenn eine bestimmte Bedingung erkannt wird.  Führen Sie diese Schritte aus, um die Sammlung von Ereignissen aus dem Linux-Syslog sowie mehreren allgemeinen Leistungsindikatoren zu konfigurieren.  

1. Wählen Sie **Syslog**.  
2. Sie können ein Ereignisprotokoll hinzufügen, indem Sie den Namen des Protokolls eingeben. Geben Sie **Syslog** ein, und klicken Sie dann auf das Pluszeichen **+** .  
3. Deaktivieren Sie in der Tabelle die Schweregrade **Info**, **Hinweis** und **Debuggen**. 
4. Klicken Sie ganz oben auf der Seite auf **Speichern**, um die Konfiguration zu speichern.
5. Klicken Sie auf **Linux-Leistungsdaten**, um die Erfassung von Leistungsindikatoren auf einem Linux-Computer zu aktivieren. 
6. Wenn Sie die Linux-Leistungsindikatoren zum ersten Mal für einen neuen Log Analytics-Arbeitsbereich konfigurieren, haben Sie die Möglichkeit, schnell mehrere allgemeine Indikatoren zu erstellen. Diese werden in einer Liste aufgeführt, und neben jedem Indikator finden Sie ein Kontrollkästchen. 

    ![Standardmäßige Windows-Leistungsindikatoren ausgewählt](media/quick-collect-linux-computer/linux-perfcounters-default.png)
    
    Klicken Sie auf **Ausgewählte Leistungsindikatoren hinzufügen**. Sie werden hinzugefügt und mit einem Stichprobenintervall von zehn Sekunden voreingestellt.

7. Klicken Sie ganz oben auf der Seite auf **Speichern**, um die Konfiguration zu speichern.

## <a name="view-data-collected"></a>Anzeigen gesammelter Daten
Jetzt haben Sie die Datensammlung aktiviert und können ein einfaches Protokollsuchebeispiel ausführen, um einige Daten vom Zielcomputer anzuzeigen.  

1. Navigieren Sie im Azure-Portal zu Log Analytics, und wählen Sie den zuvor erstellten Arbeitsbereich aus.
2. Klicken Sie auf die Kachel **Protokollsuche** und im Bereich „Protokollsuche“ auf den Abfragefeldtyp `Perf`, und drücken Sie dann auf EINGABE, oder klicken Sie auf die Suchschaltfläche rechts neben dem Abfragefeld.

    ![Beispiel für Log Analytics-Protokollsuchabfrage](media/quick-collect-linux-computer/log-analytics-portal-queryexample.png)

    Die Abfrage in der folgenden Abbildung gibt z. B. 735 Leistungsdatensätze zurück.

    ![Log Analytics: Ergebnis der Protokollsuche](media/quick-collect-linux-computer/log-analytics-search-perf.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn er nicht mehr benötigt wird, können Sie den Agent vom Linux-Computer entfernen und den Log Analytics-Arbeitsbereich löschen.  

Führen Sie den folgenden Befehl auf dem Linux-Computer aus, um den Agent zu entfernen. Mit dem Argument *--purge* werden der Agent und die dazugehörige Konfiguration vollständig entfernt.

   `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh --purge`

Wählen Sie zum Löschen des Arbeitsbereichs den Log Analytics-Arbeitsbereich aus, den Sie zuvor erstellt haben, und klicken Sie auf der Ressourcenseite auf **Löschen**.

![Löschen von Log Analytics-Ressourcen](media/quick-collect-linux-computer/log-analytics-portal-delete-resource.png)

## <a name="next-steps"></a>Nächste Schritte
Jetzt sammeln Sie Betriebs- und Leistungsdaten von Ihrem lokalen Linux-Computer und können problemlos beginnen, *kostenlos* Daten zu untersuchen, zu analysieren und Aktionen an ihnen vorzunehmen.  

Um zu erfahren, wie Sie die Daten anzeigen und analysieren, fahren Sie mit dem Tutorial fort.   

> [!div class="nextstepaction"]
> [Anzeigen oder Analysieren der Daten in Log Analytics](../../azure-monitor/learn/tutorial-viewdata.md)

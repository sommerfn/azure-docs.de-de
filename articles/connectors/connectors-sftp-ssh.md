---
title: 'Herstellen einer Verbindung mit dem SFTP-Server mit SSH: Azure Logic Apps'
description: Automatisieren von Aufgaben, die Dateien auf einem SFTP-Server mithilfe von SSH und Azure Logic Apps überwachen, erstellen, verwalten, senden und empfangen
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: divswa, klam, LADocs
ms.topic: article
ms.date: 06/18/2019
tags: connectors
ms.openlocfilehash: a48ba0d2d691314a1ca7c91ac7ae27b62fbb379b
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73825240"
---
# <a name="monitor-create-and-manage-sftp-files-by-using-ssh-and-azure-logic-apps"></a>Überwachen, Erstellen und Verwalten von SFTP-Dateien mithilfe von SSH und Azure Logic Apps

Um Aufgaben zu automatisieren, die Dateien auf einem [Secure File Transfer Protocol (SFTP)](https://www.ssh.com/ssh/sftp/)-Server überwachen, erstellen, senden und empfangen, indem Sie das [Secure Shell (SSH)](https://www.ssh.com/ssh/protocol/)-Protokoll verwenden, können Sie Integrationsworkflows mithilfe von Azure Logic Apps und dem SFTP-SSH-Connector erstellen und automatisieren. SFTP ist ein Netzwerkprotokoll, das Dateizugriff, Dateiübertragung und Dateiverwaltung über jeden beliebigen zuverlässigen Datenstrom ermöglicht. Im Folgenden finden Sie einige Beispielaufgaben, die automatisiert werden können:

* Sie können die Vorgänge überwachen, wenn Dateien hinzugefügt oder geändert werden.
* Sie können Dateien abrufen, erstellen, kopieren, umbenennen, aktualisieren, auflisten und löschen.
* Sie können Ordner erstellen.
* Sie können Dateiinhalte und Metadaten abrufen.
* Sie können Archive in Ordner extrahieren.

Sie können Trigger verwenden, die Ereignisse auf Ihrem SFTP-Server überwachen und die Ausgabe für andere Aktionen verfügbar machen. Sie können Aktionen verwenden, die verschiedene Aufgaben auf Ihrem SFTP-Server ausführen. Darüber hinaus können die Ausgaben von SFTP-Aktionen auch von anderen Aktionen in Ihrer Logik-App verwendet werden. Wenn Sie beispielsweise regelmäßig Dateien von Ihrem SFTP-Server abrufen, können Sie mithilfe des Office 365 Outlook-Connectors oder des Outlook.com-Connectors E-Mail-Benachrichtigungen zu diesen Dateien und ihren Inhalten senden. Falls Sie noch nicht mit Logik-Apps vertraut sind, finden Sie weitere Informationen unter [Was ist Azure Logic Apps?](../logic-apps/logic-apps-overview.md).

Weitere Unterschiede zwischen dem SFTP-SSH-Connector und dem SFTP-Connector finden Sie unter [Vergleichen von SFTP-SSH und SFTP](#comparison) weiter unten in diesem Thema.

## <a name="limits"></a>Einschränkungen

* Standardmäßig können SFTP-SSH-Aktionen Dateien lesen oder schreiben, die *1 GB oder kleiner* sind, aber jeweils nur in Blöcken von *15 MB*. Um Dateien mit einer Größe von mehr als 15 MB zu verarbeiten, unterstützen SFTP-SSH-Aktionen [Nachrichtensegmentierung](../logic-apps/logic-apps-handle-large-messages.md), mit Ausnahme der Aktion „Copy File“ (Datei kopieren), die nur 15 MB große Dateien verarbeiten kann. Die Aktion **Get file content** (Dateiinhalt abrufen) verwendet implizit Nachrichtensegmentierung.

* SFTP-SSH-Trigger unterstützen keine Segmentierung. Trigger wählen beim Anfordern von Dateiinhalten nur Dateien aus, die 15 MB oder kleiner sind. Verwenden Sie stattdessen das folgende Muster, um Dateien abzurufen, die größer als 15 MB sind:

  * Verwenden Sie einen SFTP-SSH-Trigger, der Dateieigenschaften zurückgibt, z.B. **When a file is added or modified (properties only)** (Beim Hinzufügen oder Ändern einer Datei (nur Eigenschaften)).

  * Lassen Sie auf den Trigger die SFTP-SSH-Aktion **Get file content** (Dateiinhalt abrufen) folgen, die die vollständige Datei liest und implizit Nachrichtensegmentierung verwendet.

<a name="comparison"></a>

## <a name="compare-sftp-ssh-versus-sftp"></a>Vergleichen von SFTP-SSH und SFTP

Hier sind weitere wesentliche Unterschiede zwischen dem SFTP-SSH-Connector und dem SFTP-Connector, bei denen der SFTP-SSH-Connector diese Funktionen bietet:

* Er verwendet die [SSH.NET](https://github.com/sshnet/SSH.NET)-Bibliothek, die eine Open Source-SSH-Bibliothek (Secure Shell) mit Unterstützung für .NET ist.

* Standardmäßig können SFTP-SSH-Aktionen Dateien lesen oder schreiben, die *1 GB oder kleiner* sind, aber jeweils nur in Blöcken von *15 MB*. Um Dateien zu verarbeiten, die größer als 15 MB sind, können Aktionen auf [Nachrichtensegmentierung](../logic-apps/logic-apps-handle-large-messages.md) zurückgreifen. Um große Dateien hochzuladen, benötigen Sie sowohl Lese- als auch Schreibberechtigungen. Die Aktion „Copy File“ (Datei kopieren) unterstützt jedoch nur 15 MB große Dateien, da diese Aktion keine Nachrichtensegmentierung unterstützt. SFTP-SSH-Trigger unterstützen keine Segmentierung.

* Er stellt die Aktion **Ordner erstellen** bereit, wodurch ein Ordner unter dem angegebenen Pfad auf dem SFTP-Server erstellt wird.

* Er stellt die Aktion **Datei umbenennen** bereit, wodurch eine Datei auf dem SFTP-Server umbenannt wird.

* Er speichert die Verbindung zum SFTP-Server *für bis zu 1 Stunde*, wodurch die Leistung verbessert und die Anzahl der Verbindungsversuche auf dem Server reduziert wird. Um die Dauer für dieses Verhalten beim Zwischenspeichern festzulegen, bearbeiten Sie die Eigenschaft [**ClientAliveInterval**](https://man.openbsd.org/sshd_config#ClientAliveInterval) in der SSH-Konfiguration auf Ihrem SFTP-Server.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Ihre SFTP-Serveradresse und Kontoanmeldeinformationen, über die Ihre Logik-App auf Ihr SFTP-Konto zugreifen kann. Außerdem benötigen Sie Zugriff auf einen privaten SSH-Schlüssel und das Kennwort für den privaten SSH-Schlüssel. Zum Verwenden der Blockerstellung beim Hochladen großer Dateien benötigen Sie sowohl Lese- als auch Schreibberechtigungen.

  > [!IMPORTANT]
  >
  > Der SFTP-SSH-Connector unterstützt *nur* diese privaten Schlüssel, Formate, Algorithmen und Fingerabdrücke:
  >
  > * **Formate für private Schlüssel**: Die Schlüssel „RSA“ (Rivest Shamir-Adleman) und „DSA“ (Digital Signature Algorithm) und OpenSSH- und ssh.com-Formate. Wenn Ihr privater Schlüssel im PuTTY-Dateiformat (PPK) vorliegt, [konvertieren Sie den Schlüssel zuerst in das OpenSSH-Dateiformat (PEM)](#convert-to-openssh).
  >
  > * **Verschlüsselungsalgorithmen**: DES-EDE3-CBC, DES EDE3 CFB, DES-CBC, AES-128-CBC, AES-192-CBC und AES-256-CBC
  >
  > * **Fingerabdruck**: MD5
  >
  > Nach dem Hinzufügen des SFTP-SSH-Triggers oder der gewünschten Aktion zu Ihrer Logik-App müssen Sie Verbindungsinformationen für Ihren SFTP-Server bereitstellen. Wenn Sie Ihren privaten SSH-Schlüssel für diese Verbindung angeben, ***geben Sie den Schlüssel nicht manuell ein, oder bearbeiten Sie ihn manuell***, was zum Fehlschlagen der Verbindung führen könnte. Stellen Sie stattdessen sicher, dass Sie ***den Schlüssel aus Ihrer Datei für private SSH-Schlüssel kopieren*** und diesen Schlüssel in die Verbindungsdetails ***einfügen***. 
  > Weitere Informationen finden Sie im Abschnitt [Herstellen einer Verbindung zu SFTP mit SSH](#connect) weiter unten in diesem Artikel.

* Grundlegende Kenntnisse über die [Erstellung von Logik-Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Die Logik-App, in der Sie auf Ihr SFTP-Konto zugreifen möchten. Um mit einem SFTP-SSH-Trigger zu beginnen, [erstellen Sie eine leere Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md). Um eine SFTP-SSH-Aktion zu verwenden, starten Sie Ihre Logik-App mit einem anderen Trigger, z.B. dem **Wiederholungstrigger**.

## <a name="how-sftp-ssh-triggers-work"></a>Funktionsweise von SFTP-SSH-Triggern

SFTP-SSH-Trigger rufen das SFTP-Dateisystem ab und suchen nach jeder Datei, die seit dem letzten Abruf geändert wurde. Bei einigen Tools können Sie den Zeitstempel beibehalten, wenn sich die Dateien ändern. In diesen Fällen müssen Sie diese Funktion deaktivieren, sodass der Trigger arbeiten kann. Hier sind einige gängige Einstellungen:

| SFTP-Client | Aktion |
|-------------|--------|
| Winscp | Navigieren Sie zu **Optionen** > **Voreinstellungen** > **Übertragen** > **Bearbeiten** > **Zeitstempel beibehalten** > **Deaktivieren**. |
| FileZilla | Wechseln Sie zu **Übertragung** > **Änderungszeitpunkt der übertragenen Dateien beibehalten** > **Deaktivieren**. |
|||

Wenn ein Trigger eine neue Datei findet, überprüft er, ob die neue Datei vollständig ist und nicht nur teilweise geschrieben wurde. Zum Beispiel werden bei einer Datei möglicherweise gerade Änderungen vorgenommen, wenn der Trigger den Dateiserver überprüft. Um zu vermeiden, dass eine nur zum Teil geschriebene Datei zurückgegeben wird, vermerkt der Trigger den Zeitstempel für die Datei mit den kürzlichen Änderungen, gibt diese Datei jedoch nicht sofort zurück. Der Trigger gibt die Datei erst dann zurück, wenn der Server erneut abgerufen wird. Dieses Verhalten kann in manchen Fällen zu Verzögerungen führen, die bis zu zweimal länger als das Abrufintervall des Triggers sein können.

<a name="convert-to-openssh"></a>

## <a name="convert-putty-based-key-to-openssh"></a>PuTTY-basierten Schlüssel in OpenSSH konvertieren

Wenn Ihr privater Schlüssel im PuTTY-Format vorliegt das die Dateinamenerweiterung PPK (PuTTY Private Key) verwendet, konvertieren Sie zunächst den Schlüssel in das OpenSSH-Format, das die Dateinamenerweiterung PEM (Privacy Enhanced Mail) verwendet.

### <a name="unix-based-os"></a>Unix-basiertes Betriebssystem

1. Wenn die PuTTY-Tools nicht bereits auf Ihrem System installiert sind, holen Sie dies jetzt nach, z. B.:

   `sudo apt-get install -y putty`

1. Führen Sie diesen Befehl aus, der eine Datei erstellt, die Sie mit dem SFTP-SSH-Connector verwenden können:

   `puttygen <path-to-private-key-file-in-PuTTY-format> -O private-openssh -o <path-to-private-key-file-in-OpenSSH-format>`

   Beispiel:

   `puttygen /tmp/sftp/my-private-key-putty.ppk -O private-openssh -o /tmp/sftp/my-private-key-openssh.pem`

### <a name="windows-os"></a>Windows-Betriebssystem

1. Wenn Sie dies noch nicht getan haben, [laden Sie das neueste PuTTY-Generator-Tool (PuTTYgen.exe) herunter](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html), und starten Sie dann das Tool.

1. Wählen Sie in diesem Bildschirm **Laden** aus.

   ![„Laden“ auswählen](./media/connectors-sftp-ssh/puttygen-load.png)

1. Navigieren Sie zu Ihrer privaten Schlüsseldatei im PuTTY-Format, und wählen Sie **Öffnen** aus.

1. Wählen Sie im Menü **Konvertierungen** den Eintrag **OpenSSH-Schlüssel exportieren** aus.

   ![„OpenSSH-Schlüssel exportieren“ auswählen](./media/connectors-sftp-ssh/export-openssh-key.png)

1. Speichern Sie die private Schlüsseldatei mit der Dateinamenerweiterung `.pem`.

<a name="connect"></a>

## <a name="connect-to-sftp-with-ssh"></a>Herstellen einer Verbindung zu SFTP mit SSH

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und öffnen Sie Ihre Logik-App im Logik-App-Designer, sofern sie nicht bereits geöffnet ist.

1. Geben Sie im Fall einer leeren Logik-App im Suchfeld die Zeichenfolge „sftp ssh“ als Filter ein. Wählen Sie in der Triggerliste den gewünschten Trigger aus.

   Oder

   Wählen Sie für vorhandene Logik-Apps im letzten Schritt zum Hinzufügen einer Aktion die Option **Neuer Schritt** aus. Geben Sie im Suchfeld den Begriff „sftp ssh“ als Filter ein. Wählen Sie in der Liste mit den Aktionen die gewünschte Aktion aus.

   Wenn Sie zwischen Schritten eine Aktion einfügen möchten, bewegen Sie den Mauszeiger über den Pfeil zwischen den Schritten. Wählen Sie das daraufhin angezeigte Pluszeichen ( **+** ) und dann **Aktion hinzufügen** aus.

1. Geben Sie die erforderlichen Informationen für Ihre Verbindung ein.

   > [!IMPORTANT]
   >
   > Wenn Sie Ihren privaten SSH-Schlüssel in der Eigenschaft **SSH private key** eingeben, führen Sie diese zusätzlichen Schritte aus, um sicherzustellen, dass Sie den vollständigen und korrekten Wert für diese Eigenschaft angeben. Ein ungültiger Schlüssel führt zu einem Verbindungsfehler.

   Sie können jeden Text-Editor verwenden. Hier finden Sie Beispielschritte, die zeigen, wie Sie Ihren Schlüssel am Beispiel von Notepad.exe korrekt kopieren und einfügen.

   1. Öffnen Sie die Datei für den privaten SSH-Schlüssel in einem Text-Editor. Bei diesen Schritten verwenden wir als Beispiel Editor.

   1. Wählen Sie im Editor-Menü **Bearbeiten** die Option **Alles markieren** aus.

   1. Klicken Sie auf **Bearbeiten** > **Kopieren**.

   1. Fügen Sie in den von Ihnen hinzugefügten SFTP-SSH-Trigger oder die von Ihnen hinzugefügte Aktion den kopierter Schlüssel *complete* in die Eigenschaft **SSH private key** ein, die mehrere Zeilen unterstützt.  Stellen Sie sicher, dass Sie den Schlüssel ***einfügen***. ***Sie dürfen ihn nicht manuell eingeben oder bearbeiten***.

1. Wenn Sie die Verbindungsdetails eingegeben haben, wählen Sie **Erstellen** aus.

1. Geben Sie jetzt die erforderlichen Details für Ihren ausgewählten Trigger oder Ihre ausgewählte Aktion an, und fahren Sie mit dem Erstellen Ihres Logik-App-Workflows fort.

## <a name="examples"></a>Beispiele

<a name="file-added-modified"></a>

### <a name="sftp---ssh-trigger-when-a-file-is-added-or-modified"></a>SFTP – SSH-Trigger: When a file is added or modified (Wenn eine Datei hinzugefügt oder geändert wird)

Dieser Trigger startet einen Logik-App-Workflow, wenn auf einem SFTP-Server eine Datei hinzugefügt oder geändert wird. Sie können beispielsweise eine Bedingung hinzufügen, die den Inhalt der Datei überprüft und den Inhalt basierend darauf abruft, ob er eine bestimmte Bedingung erfüllt. Sie können dann eine Aktion hinzufügen, die den Inhalt der Datei abruft und in einem Ordner auf dem SFTP-Server ablegt.

**Beispiel für Unternehmen**: Sie können mit diesem Trigger beispielsweise einen SFTP-Ordner auf neue Dateien überwachen, die Kundenbestellungen darstellen. Anschließend können Sie eine SFTP-Aktion wie etwa **Dateiinhalt abrufen** verwenden, um den Inhalt einer Bestellung zur weiteren Verarbeitung abzurufen und in einer Bestelldatenbank zu speichern.

<a name="get-content"></a>

### <a name="sftp---ssh-action-get-content-using-path"></a>SFTP – SSH-Aktion: Dateiinhalt über Pfad abrufen

Diese Aktion ruft den Inhalt einer Datei auf einem SFTP-Server ab. Sie können z.B. den Trigger aus dem vorherigen Beispiel sowie eine Bedingung hinzufügen, die vom Dateiinhalt erfüllt werden muss. Wenn die Bedingung als TRUE ausgewertet wird, kann die Aktion ausgeführt werden, die den Inhalt abruft.

## <a name="connector-reference"></a>Connector-Referenz

Technische Details zu Triggern, Aktionen und Beschränkungen aus der OpenAPI-Beschreibung (ehemals Swagger) des Connectors finden Sie auf der [Referenzseite](/connectors/sftpconnector/) des Connectors.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)

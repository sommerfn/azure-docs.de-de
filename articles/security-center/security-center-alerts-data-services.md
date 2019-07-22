---
title: Bedrohungserkennung für Datendienste in Azure Security Center | Microsoft-Dokumentation
description: Dieses Thema stellt die Warnungen der Datendienste vor, die in Azure Security Center verfügbar sind.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: da960861-0b6c-4d80-932d-898cdebb4f83
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: monhaber
ms.openlocfilehash: 87cfd2769e473d26c2dcae1b7b418f6fb1739915
ms.sourcegitcommit: c0419208061b2b5579f6e16f78d9d45513bb7bbc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2019
ms.locfileid: "67626286"
---
# <a name="threat-detection-for-data-services-in-azure-security-center"></a>Bedrohungserkennung für Datendienste in Azure Security Center

 Security Center analysiert die Protokolle der Datenspeicherdienste und löst Warnungen aus, wenn eine Bedrohung für Ihre Datenressourcen erkannt wird. Dieses Thema listet die Warnungen auf, die von Security Center für die folgenden Dienste generiert werden:

* [Azure SQL-Datenbank und SQL Data Warehouse](#data-sql)
* [Azure Storage (in englischer Sprache)](#azure-storage)

## Azure SQL-Datenbank und SQL Data Warehouse<a name="data-sql"></a>

Die SQL-Bedrohungserkennung erkennt anormale Aktivitäten, die auf ungewöhnliche und möglicherweise schädliche Versuche hinweisen, auf Datenbanken zuzugreifen oder diese zu missbrauchen. Security Center analysiert die SQL-Überwachungsprotokolle und wird nativ in der SQL-Engine ausgeführt.

|Warnung|BESCHREIBUNG|
|---|---|
|**Anfälligkeit für die Einschleusung von SQL-Befehlen**|Eine Anwendung hat eine fehlerhafte SQL-Anweisung in der Datenbank generiert. Dies kann ein Hinweis auf ein mögliches Sicherheitsrisiko in Bezug auf Angriffe mit Einschleusung von SQL-Befehlen sein. Es gibt zwei mögliche Gründe für die Generierung einer fehlerhaften Anweisung: Ein Fehler im Anwendungscode, der zur fehlerhaften SQL-Anweisung führt. Anwendungscode oder gespeicherte Prozeduren führen bei der Erstellung der fehlerhaften SQL-Anweisung keine Bereinigung der Benutzereingabe durch, und dies kann für eine Einschleusung von SQL-Befehlen ausgenutzt werden.|
|**Potenzielle Einschleusung von SQL-Befehlen**|Es ist ein aktiver Missbrauch für eine identifizierte Anwendung aufgetreten, die für die Einschleusung von SQL-Befehlen anfällig ist. Dies bedeutet, dass ein Angreifer versucht, schädliche SQL-Anweisungen einzuschleusen, indem er den anfälligen Anwendungscode bzw. die gespeicherten Prozeduren verwendet.|
|**Zugriff von einem ungewöhnlichen Ort**|Es ist eine Änderung des Zugriffsmusters für SQL Server erfolgt, weil sich eine Person von einem ungewöhnlichen Ort aus bei SQL Server angemeldet hat. In einigen Fällen erkennt die Warnung eine legitime Aktion (eine neue Anwendung oder Wartungsarbeiten von Entwicklern). In anderen Fällen erkennt die Warnung eine schädliche Aktion (ehemaliger Mitarbeiter, externer Angreifer).|
|**Zugriff über einen unbekannten Prinzipal**|Es ist eine Änderung des Zugriffsmusters für SQL Server erfolgt, weil sich eine Person mit einem ungewöhnlichen Prinzipal (SQL-Benutzer) bei SQL Server angemeldet hat. In einigen Fällen erkennt die Warnung eine legitime Aktion (neue Anwendung, Wartungsarbeiten von Entwicklern). In anderen Fällen erkennt die Warnung eine schädliche Aktion (ehemaliger Mitarbeiter, externer Angreifer).|
|**Zugriff über eine potenziell schädliche Anwendung**|Für den Zugriff auf die Datenbank wurde eine potenziell schädliche Anwendung verwendet. In einigen Fällen erkennt die Warnung aktive Eindringversuche. In anderen Fällen erkennt die Warnung einen Angriff mit allgemeinen Angriffstools.|
|**Brute-Force-Angriff auf SQL-Anmeldeinformationen**|Es ist eine ungewöhnlich hohe Anzahl von fehlerhaften Anmeldungen mit unterschiedlichen Anmeldeinformationen aufgetreten. In einigen Fällen erkennt die Warnung aktive Eindringversuche. In anderen Fällen erkennt die Warnung einen Brute-Force-Angriff.|

Weitere Informationen zu Warnungen zur Erkennung von SQL-Bedrohungen finden Sie unter [Bedrohungserkennung von Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview), und lesen Sie außerdem den Abschnitt zu den Warnungen zur Erkennung von Bedrohungen. Lesen Sie auch [How Azure Security Center helps reveal a Cyberattack](https://azure.microsoft.com/blog/how-azure-security-center-helps-reveal-a-cyberattack/) (Wie ein Cyberangriff mit Azure Security Center erkannt werden kann), um ein Beispiel dafür zu erhalten, wie Security Center die Erkennung bösartiger SQL-Aktivitäten verwendet, um einen Angriff zu erkennen.

## Azure Storage<a name="azure-storage"></a>

>[!NOTE]
> Advanced Threat Protection für Azure Storage ist derzeit nur für Blob Storage verfügbar. 

Advanced Threat Protection für Azure Storage ermöglicht die Nutzung intelligenter Sicherheitsfunktionen zur Erkennung von ungewöhnlichen und möglicherweise schädlichen Versuchen, auf Speicherkonten zuzugreifen oder diese unbefugt zu nutzen. Aufgrund dieser Schutzebene können Sie Bedrohungen begegnen, ohne dass Sie ein Sicherheitsexperte sein oder Systeme für die Überwachung der Sicherheit verwalten müssen.

Security Center analysiert Diagnoseprotokolle von Lese-, Schreib- und Löschanforderungen für den Blobspeicher, um Bedrohungen zu erkennen, und löst Warnungen aus, wenn Anomalien in der Aktivität auftreten. Weitere Informationen finden Sie unter [Konfigurieren der Storage Analytics-Protokollierung](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account#configure-logging).

> [!div class="mx-tableFixed"]

|Warnung|BESCHREIBUNG|
|---|---|
|**Anomalie: Zugriff von einem ungewöhnlichen Ort**|Bei der Netzwerk-Datenverkehrsanalyse wurde anormale ausgehende RDP-Kommunikation (Remote Desktop Protocol) erkannt, die von einer Ressource in Ihrer Bereitstellung stammt. Diese Aktivität gilt als anormal für diese Umgebung und kann darauf hindeuten, dass Ihre Ressource gefährdet ist und nun dazu verwendet wird, um einen Brute-Force-Angriff auf den externen RDP-Endpunkt auszuführen. Diese Art von Aktivität kann dazu führen, dass Ihre IP-Adresse von externen Entitäten als schädlich gekennzeichnet wird.|
|**Anomalie beim Anwendungszugriff**|Gibt an, dass auf dieses Speicherkonto über eine ungewöhnliche Anwendung zugegriffen wurde. Mögliche Ursache: Ein Angreifer hat mit einer neuen Anwendung auf Ihr Speicherkonto zugegriffen.|
|**Anomalie: Anonymer Zugriff**|Gibt an, dass sich am Zugriffsmuster eines Speicherkontos etwas geändert hat. Auf dieses Konto wurde beispielsweise auf ungewöhnliche Weise (ohne Authentifizierung) zugegriffen, was im Vergleich zum letzten Zugriffsmuster für dieses Konto ein unerwartetes Verhalten ist. Mögliche Ursache: Ein Angreifer hat öffentlichen Lesezugriff auf einen Container ausgenutzt, der Blobspeicher enthält.|
|**Anomalie: Datenexfiltration**|Gibt an, dass eine große Datenmenge extrahiert wurde, die im Vergleich zur letzten Aktivität bei diesem Speichercontainer ungewöhnlich ist. Mögliche Ursache: Ein Angreifer hat eine große Menge an Daten aus einem Container extrahiert, der Blobspeicher enthält.|
|**Anomalie: Unerwarteter Löschvorgang**|Gibt an, dass in einem Speicherkonto mindestens ein Löschvorgang aufgetreten ist, der im Vergleich zur letzten Aktivität für dieses Konto unerwartet ist. Mögliche Ursache: Ein Angreifer hat Daten im Speicherkonto gelöscht.|
|**Upload eines Azure Cloud Service-Pakets**|Gibt an, dass ein Azure Cloud Service-Paket (CSPKG-Datei) auf eine Weise in ein Speicherkonto hochgeladen wurde, die im Vergleich zur letzten Aktivität für das Konto ungewöhnlich ist. Mögliche Ursache: Ein Angreifer hat die Bereitstellung von bösartigem Code über Ihr Speicherkonto für einen Azure-Clouddienst vorbereitet.|
|**Anomalie beim Berechtigungszugriff**|Gibt an, dass die Zugriffsberechtigungen für diesen Speichercontainer auf ungewöhnliche Weise geändert wurden. Mögliche Ursache: Ein Angreifer hat die Containerberechtigungen geändert, um seine Sicherheit zu schwächen oder Persistenz zu erlangen.|
|**Anomalie beim Überprüfungszugriff**|Gibt an, dass die Zugriffsberechtigungen eines Speicherkontos auf eine Weise überprüft wurden, die im Vergleich zur letzten Aktivität für dieses Konto ungewöhnlich ist. Mögliche Ursache: Ein Angreifer hat für einen späteren Angriff eine Reconnaissance durchgeführt.|
|**Anomalie bei der Datenuntersuchung**|Gibt an, dass Blobs oder Container in einem Speicherkonto in einer Weise aufgezählt wurden, die im Vergleich zur letzten Aktivität für dieses Konto anormal ist. Mögliche Ursache: Ein Angreifer hat für einen späteren Angriff eine Reconnaissance durchgeführt.|

>[!NOTE]
>Advanced Threat Protection für Azure Storage ist derzeit in Azure Government- und Sovereign Cloud-Regionen nicht verfügbar.

Weitere Informationen zu den Warnungen für den Speicher finden Sie im Artikel [Advanced Threat Protection für Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection) und im Abschnitt zu den Schutzwarnungen.

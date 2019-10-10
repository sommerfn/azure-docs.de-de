---
title: Bedrohungserkennung für Datendienste in Azure Security Center | Microsoft-Dokumentation
description: Dieses Thema stellt die Warnungen der Datendienste vor, die in Azure Security Center verfügbar sind.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: da960861-0b6c-4d80-932d-898cdebb4f83
ms.service: security-center
ms.topic: conceptual
ms.date: 07/24/2019
ms.author: memildin
ms.openlocfilehash: 7e45dffd15cdf2acf15c7d46ed0cf42fc5997a6a
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72244576"
---
# <a name="threat-detection-for-data-services-in-azure-security-center"></a>Bedrohungserkennung für Datendienste in Azure Security Center

 Azure Security Center analysiert die Protokolle der Datenspeicherdienste und löst Warnungen aus, wenn eine Bedrohung für Ihre Datenressourcen erkannt wird. Dieses Thema listet die Warnungen auf, die von Security Center für die folgenden Dienste generiert werden:

* [Azure SQL-Datenbank und Azure SQL Data Warehouse](#data-sql)
* [Azure Storage (in englischer Sprache)](#azure-storage)
* [Azure Cosmos DB](#cosmos-db)

## SQL-Datenbank und SQL Data Warehouse <a name="data-sql"></a>

Die SQL-Bedrohungserkennung erkennt anormale Aktivitäten, die auf ungewöhnliche und möglicherweise schädliche Versuche hinweisen, auf Datenbanken zuzugreifen oder diese zu missbrauchen. 

|Warnung|BESCHREIBUNG|
|---|---|
|**Anfälligkeit für die Einschleusung von SQL-Befehlen**|Eine Anwendung hat eine fehlerhafte SQL-Anweisung in der Datenbank generiert. Dies kann ein Hinweis auf ein mögliches Sicherheitsrisiko in Bezug auf Angriffe mit Einschleusung von SQL-Befehlen sein. Es gibt zwei mögliche Gründe für eine fehlerhafte Anweisung. Ein Fehler im Anwendungscode, der zu der fehlerhaften SQL-Anweisung geführt hat. Oder: Anwendungscode oder gespeicherte Prozeduren, die bei der Erstellung der fehlerhaften SQL-Anweisung keine Bereinigung der Benutzereingabe durchgeführt haben, was zur Einschleusung von SQL-Befehlen ausgenutzt werden kann.|
|**Potenzielle Einschleusung von SQL-Befehlen**|Es ist ein aktiver Missbrauch für eine identifizierte Anwendung aufgetreten, die für die Einschleusung von SQL-Befehlen anfällig ist. Das bedeutet, dass ein Angreifer versucht, schädliche SQL-Anweisungen einzuschleusen, indem er den anfälligen Anwendungscode bzw. die gespeicherten Prozeduren verwendet.|
|**Zugriff von einem ungewöhnlichen Ort**|Das Zugriffsmusters für SQL Server hat sich geändert, da sich eine Person von einem ungewöhnlichen Ort aus beim Server angemeldet hat. In einigen Fällen erkennt die Warnung eine legitime Aktion (eine neue Anwendung oder Wartungsarbeiten von Entwicklern). In anderen Fällen erkennt die Warnung eine schädliche Aktion (einen ehemaligen Mitarbeiter oder einen externen Angreifer.)|
|**Zugriff über einen unbekannten Prinzipal**|Das Zugriffsmuster für SQL Server hat sich geändert. Jemand hat sich beim Server mit einem ungewöhnlichen Prinzipal (Benutzer) angemeldet. In einigen Fällen erkennt die Warnung eine legitime Aktion (eine neue Anwendung oder Wartungsarbeiten von Entwicklern). In anderen Fällen erkennt die Warnung eine schädliche Aktion (einen ehemaligen Mitarbeiter oder einen externen Angreifer.)|
|**Zugriff über eine potenziell schädliche Anwendung**|Für den Zugriff auf die Datenbank wurde eine potenziell schädliche Anwendung verwendet. In einigen Fällen erkennt die Warnung aktive Eindringversuche. In anderen Fällen erkennt die Warnung einen Angriff mit gängigen Tools.|
|**Brute-Force-Angriff auf SQL-Anmeldeinformationen**|Es wurde eine ungewöhnlich hohe Anzahl von fehlerhaften Anmeldungen mit unterschiedlichen Anmeldeinformationen festgestellt. In einigen Fällen erkennt die Warnung aktive Eindringversuche. In anderen Fällen erkennt die Warnung einen Brute-Force-Angriff.|

Weitere Informationen zu Warnungen im Zusammenhang mit der Erkennung von SQL-Bedrohungen finden Sie unter [Bedrohungserkennung von Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview). Lesen Sie insbesondere den Abschnitt zu den Warnungen der Bedrohungserkennung. Unter [How Azure Security Center helps reveal a Cyberattack](https://azure.microsoft.com/blog/how-azure-security-center-helps-reveal-a-cyberattack/) (Erkennen von Cyberangriffen mit Azure Security Center) finden Sie ein Beispiel dafür, wie Security Center die Erkennung schädlicher SQL-Aktivitäten verwendet, um einen Angriff zu erkennen.

## Storage <a name="azure-storage"></a>

>[!NOTE]
> Advanced Threat Protection für Storage ist derzeit nur für Blobspeicher verfügbar.

Advanced Threat Protection für Storage ermöglicht die Nutzung intelligenter Sicherheitsfunktionen zur Erkennung von ungewöhnlichen und möglicherweise schädlichen Versuchen, auf Speicherkonten zuzugreifen oder diese unbefugt zu nutzen. Dank dieser Schutzebene können selbst Benutzer ohne fundierte Sicherheitskenntnisse etwas gegen Bedrohungen unternehmen und Systeme für die Sicherheitsüberwachung verwalten.

Security Center analysiert Diagnoseprotokolle von Lese-, Schreib- und Löschanforderungen für den Blobspeicher, um Bedrohungen zu erkennen, und löst Warnungen aus, wenn Anomalien in der Aktivität auftreten. Weitere Informationen finden Sie unter [Konfigurieren der Protokollierung](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account#configure-logging).

> [!div class="mx-tableFixed"]

|Warnung|BESCHREIBUNG|
|---|---|
|**Anomalie: Zugriff von einem ungewöhnlichen Ort**|Bei der Stichprobenanalyse des Netzwerkdatenverkehrs wurde eine ungewöhnliche ausgehende RDP-Kommunikation (Remote Desktop Protocol) erkannt, die von einer Ressource in Ihrer Bereitstellung stammt. Diese Aktivität ist für die Umgebung unnormal. Dies kann darauf hindeuten, dass Ihre Ressource kompromittiert wurde und jetzt für Brute-Force-Angriffe auf einen externen RDP-Endpunkt verwendet wird. Diese Art von Aktivität kann dazu führen, dass Ihre IP-Adresse von externen Stellen als schädlich gekennzeichnet wird.|
|**Anomalie beim Anwendungszugriff**|Gibt an, dass auf dieses Speicherkonto über eine ungewöhnliche Anwendung zugegriffen wurde. Mögliche Ursache: Ein Angreifer hat mit einer neuen Anwendung auf Ihr Speicherkonto zugegriffen.|
|**Anomalie: Anonymer Zugriff**|Gibt an, dass sich am Zugriffsmuster eines Speicherkontos etwas geändert hat. Auf dieses Konto wurde beispielsweise auf ungewöhnliche Weise (ohne Authentifizierung) zugegriffen, was im Vergleich zum letzten Zugriffsmuster für dieses Konto ein unerwartetes Verhalten ist. Mögliche Ursache: Ein Angreifer hat öffentlichen Lesezugriff auf einen Container ausgenutzt, der Blobspeicher enthält.|
|**Tor Anomaly** (Tor-Anomalie)|Gibt an, dass auf dieses Konto erfolgreich über eine IP-Adresse zugegriffen wurde, bei der es sich um einen bekannten aktiven Exitknoten von Tor (bekannter Anonymisierungsproxy) handelt. Der Schweregrad dieser Warnung hängt vom verwendeten Authentifizierungstyp (sofern zutreffend) sowie davon ab, ob es sich hierbei um den ersten Zugriff dieser Art handelt. Mögliche Ursachen: Ein Angreifer hat mit Tor auf Ihr Speicherkonto zugegriffen, oder ein berechtigter Benutzer hat unter Verwendung von Tor auf Ihr Speicherkonto zugegriffen.|
|**Anomalie: Datenexfiltration**|Gibt an, dass eine große Datenmenge extrahiert wurde, die im Vergleich zur letzten Aktivität bei diesem Speichercontainer ungewöhnlich ist. Mögliche Ursache: Ein Angreifer hat eine große Menge an Daten aus einem Container extrahiert, der Blobspeicher enthält.|
|**Anomalie: Unerwarteter Löschvorgang**|Gibt an, dass in einem Speicherkonto mindestens ein Löschvorgang aufgetreten ist, der im Vergleich zur letzten Aktivität für dieses Konto unerwartet ist. Mögliche Ursache: Ein Angreifer hat Daten im Speicherkonto gelöscht.|
|**Upload eines Azure Cloud Services-Pakets**|Gibt an, dass ein Azure Cloud Services-Paket (CSPKG-Datei) auf eine Weise in ein Speicherkonto hochgeladen wurde, die im Vergleich zur letzten Aktivität für das Konto ungewöhnlich ist. Mögliche Ursache: Ein Angreifer hat die Bereitstellung von bösartigem Code über Ihr Speicherkonto für einen Azure-Clouddienst vorbereitet.|
|**Anomalie beim Berechtigungszugriff**|Gibt an, dass die Zugriffsberechtigungen für diesen Speichercontainer auf ungewöhnliche Weise geändert wurden. Mögliche Ursache: Ein Angreifer hat die Containerberechtigungen geändert, um die Sicherheit zu schwächen oder Persistenz zu erlangen.|
|**Anomalie beim Überprüfungszugriff**|Gibt an, dass die Zugriffsberechtigungen eines Speicherkontos auf eine Weise überprüft wurden, die im Vergleich zur letzten Aktivität für dieses Konto ungewöhnlich ist. Mögliche Ursache: Ein Angreifer hat für einen späteren Angriff eine Reconnaissance durchgeführt.|
|**Anomalie bei der Datenuntersuchung**|Gibt an, dass Blobs oder Container in einem Speicherkonto in einer Weise aufgezählt wurden, die im Vergleich zur letzten Aktivität für dieses Konto anormal ist. Mögliche Ursache: Ein Angreifer hat für einen späteren Angriff eine Reconnaissance durchgeführt.|

>[!NOTE]
>Advanced Threat Protection für Storage ist derzeit in Azure Government- und Sovereign Cloud-Regionen nicht verfügbar.

Weitere Informationen zu den Warnungen für Storage finden Sie im Artikel [Advanced Threat Protection für Azure Storage](../storage/common/storage-advanced-threat-protection.md). Lesen Sie insbesondere den Abschnitt zu den Schutzwarnungen.

## Azure Cosmos DB<a name="cosmos-db"></a>

Die folgenden Warnungen werden bei ungewöhnlichen und potenziell schädlichen Zugriffsversuchen oder Exploit-Vorgängen für Azure Cosmos DB-Konten generiert:

|Warnung|BESCHREIBUNG|
|---|---|
|**Zugriff von einem ungewöhnlichen Ort**|Gibt an, dass sich das Zugriffsmuster für ein Azure Cosmos DB-Konto geändert hat. Im Vergleich zu den letzten Aktivitäten hat jemand von einer unbekannten IP-Adresse aus auf dieses Konto zugegriffen. Entweder hat ein Angreifer auf das Konto zugegriffen, oder ein berechtigter Benutzer hat von einem neuen, ungewöhnlichen geografischen Standort aus darauf zugegriffen. Ein Beispiel für Letzteres wäre etwa eine Remotewartung durch eine neue Anwendung oder einen neuen Entwickler.|
|**Ungewöhnliche Datenexfiltration**|Gibt an, dass sich das Datenextraktionsmuster eines Azure Cosmos DB-Kontos geändert hat. Im Vergleich zu den letzten Aktivitäten hat jemand eine ungewöhnliche Datenmenge extrahiert. Ein Angreifer hat möglicherweise eine große Datenmenge aus einer Azure Cosmos DB-Datenbank extrahiert (beispielsweise Datenexfiltration/-leck oder eine nicht autorisierte Datenübertragung). Oder: Ein berechtigter Benutzer oder eine berechtigte Anwendung hat eine ungewöhnliche Datenmenge aus einem Container extrahiert (beispielsweise für eine Sicherung bei einer Wartungsaktivität).|

Weitere Informationen finden Sie unter [Advanced Threat Protection für Azure Cosmos DB](../cosmos-db/cosmos-db-advanced-threat-protection.md).

---
title: Checkliste für die Sicherheit der Azure-Datenbank | Microsoft-Dokumentation
description: Dieser Artikel bietet einen Satz von Checklisten für die Sicherheit der Azure-Datenbank.
services: security
documentationcenter: na
author: unifycloud
manager: barbkess
editor: tomsh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: tomsh
ms.openlocfilehash: 4a6796c0a9a64bc40255de64b39ce881e1b8855c
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68934723"
---
# <a name="azure-database-security-checklist"></a>Checkliste für die Sicherheit der Azure-Datenbank

Um die Sicherheit zu verbessern, umfasst die Azure-Datenbank eine Reihe von integrierten Sicherheitsfunktionen, mit denen Sie den Zugriff begrenzen und kontrollieren können.

Das umfasst:

-   Eine Firewall, die es Ihnen die ermöglicht, [Firewallregeln](../../sql-database/sql-database-firewall-configure.md) zu erstellen, die die Konnektivität mit den IP-Adressen beschränken
-   Firewallregeln auf Serverebene zugänglich vom Azure-Portal
-   Firewallregeln auf Datenebene zugänglich von SSMS
-   Herstellen einer sicheren Verbindung mit Ihrer Datenbank mithilfe einer sicheren Verbindungszeichenfolge
-   Verwenden der Zugriffsverwaltung
-   Datenverschlüsselung
-   SQL-Datenbanküberwachung
-   Bedrohungserkennung von SQL-Datenbank

## <a name="introduction"></a>Einführung
Cloud Computing erfordert neue Sicherheitsparadigmen, die vielen Anwendungsbenutzern, Datenbankadministratoren und Programmierern nicht geläufig sind. Dies hat zur Folge, dass einige Unternehmen aufgrund von wahrgenommenen Sicherheitsrisiken zögern, eine Cloud-Infrastruktur zu implementieren. Allerdings kann ein Großteil dieser Risiken über ein besseres Bild von Sicherheitsfunktionen, die in Microsoft Azure und Microsoft Azure SQL-Datenbank integriert sind, überwunden werden.

## <a name="checklist"></a>Checkliste
Es wird empfohlen, den Artikel [Azure Database Security Best Practices (Bewährte Methoden für die Sicherheit der Azure-Datenbank)](database-best-practices.md) vor dem Überprüfen dieser Checkliste durchzulesen. Wenn Sie die bewährten Methoden kennen, können Sie die Checkliste optimal nutzen. Sie können diese Checkliste dann auch nutzen, um sicherzustellen, dass Sie wichtige Sicherheitsprobleme der Azure-Datenbank behoben haben.


|Checklistenkategorie| BESCHREIBUNG|
| ------------ | -------- |
|**Schützen von Daten**||
| <br> Verschlüsselung von Daten während der Übertragung| <ul><li>[Transport Layer Security](https://docs.microsoft.com/windows-server/security/tls/transport-layer-security-protocol) für die Datenverschlüsselung beim Verschieben von Daten in die Netzwerke.</li><li>Die Datenbank erfordert sichere Kommunikation von Clients auf der Grundlage des Protokolls [Tabular Data Stream (TDS)](https://msdn.microsoft.com/library/dd357628.aspx) über Transport Layer Security (TLS).</li></ul> |
|<br>Verschlüsselung ruhender Daten| <ul><li>[Transparente Datenverschlüsselung](https://go.microsoft.com/fwlink/?LinkId=526242), wenn inaktive Daten physisch in digitaler Form gespeichert werden.</li></ul>|
|**Steuern des Zugriffs**||  
|<br> Datenbankzugriff | <ul><li>[Authentifizierung](../../sql-database/sql-database-control-access.md) (Azure Active Directory-Authentifizierung, AD-Authentifizierung) verwendet von Azure Active Directory verwaltete Identitäten.</li><li>[Autorisierung](../../sql-database/sql-database-control-access.md) erteilt Benutzern die minimal erforderlichen Berechtigungen.</li></ul> |
|<br>Anwendungszugriff| <ul><li>[Sicherheit auf Zeilenebene](https://msdn.microsoft.com/library/dn765131) (Verwendet Sicherheitsrichtlinien und beschränkt gleichzeitig den Zugriff auf Zeilenebene auf Grundlage einer Benutzeridentität, einer Rolle oder eines Ausführungskontexts).</li><li>[Dynamische Datenmaskierung](../../sql-database/sql-database-dynamic-data-masking-get-started.md) (Verwendet Permission &amp; Policy, schränkt die Offenlegung sensibler Daten ein, indem sie für nicht berechtigte Benutzer maskiert werden).</li></ul>|
|**Proaktive Überwachung**||  
| <br>Nachverfolgen und Erkennen| <ul><li>Die [Überprüfung](../../sql-database/sql-database-auditing.md) verfolgt Datenbankereignisse und schreibt diese in ein Überwachungs-/Aktivitätsprotokoll in Ihrem [Azure Speicherkonto](../../storage/common/storage-create-storage-account.md).</li><li>Nachverfolgen der Integrität der Azure-Datenbank mit [Azure Monitor-Aktivitätsprotokollen](../../azure-monitor/platform/activity-logs-overview.md).</li><li>Die [Bedrohungserkennung](../../sql-database/sql-database-threat-detection.md) erkennt anormale Datenbankaktivitäten, die auf potenzielle Sicherheitsrisiken für die Datenbank hindeuten. </li></ul> |
|<br>Azure Security Center| <ul><li>Die [Überwachung der Daten](../../security-center/security-center-enable-auditing-on-sql-databases.md) verwendet Azure Security Center als eine zentralisierte Sicherheitsüberwachungslösung für SQL und anderen Azure-Dienste.</li></ul>|       

## <a name="conclusion"></a>Zusammenfassung
Die Azure-Datenbank ist eine robuste Datenbankplattform mit einem vollständigen Umfang an Sicherheitsfunktionen, die viele Anforderungen des Unternehmens sowie behördliche Vorschriften erfüllen. Sie können Daten problemlos schützen, indem Sie den physischen Zugriff auf Ihre Daten steuern und eine Vielzahl von Optionen für die Datensicherheit auf der Datei-, Spalten- oder Zeilenebene mit Transparent Data Encryption, der Verschlüsselung auf Zellenebene oder Sicherheit auf Zeilenebene verwenden. Always Encrypted kann auch Vorgänge für verschlüsselte Daten vereinfachen, da der Prozess der Anwendungsaktualisierungen vereinfacht wird. Durch den Zugriff auf Überwachungsprotokolle der SQL-Datenbankaktivität erhalten sie zudem Informationen dazu, wie und wann auf die Daten zugegriffen wird.

## <a name="next-steps"></a>Nächste Schritte
Sie können den Schutz Ihrer Datenbank vor schädlichen Benutzern oder nicht autorisiertem Zugriff mit wenigen einfachen Schritten verbessern. In diesem Tutorial lernen Sie Folgendes:

- Einrichten von [Firewallregeln](../../sql-database/sql-database-firewall-configure.md) für den Server und/oder die Datenbank
- Schützen von Daten durch [Verschlüsselung](https://docs.microsoft.com/sql/relational-databases/security/encryption/sql-server-encryption).
- Aktivieren der [SQL-Datenbanküberwachung](../../sql-database/sql-database-auditing.md).


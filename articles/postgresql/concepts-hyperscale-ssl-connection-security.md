---
title: Konfigurieren von SSL in Azure Database for PostgreSQL – Hyperscale (Citus)
description: Hier finden Sie Anweisungen und Informationen zum Konfigurieren von Azure Database for PostgreSQL – Hyperscale (Citus) und zugehörigen Anwendungen für die ordnungsgemäße Verwendung von SSL-Verbindungen.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/17/2019
ms.openlocfilehash: be02c9114f30a63fe710cfd2fc56347bfc68fab5
ms.sourcegitcommit: a6718e2b0251b50f1228b1e13a42bb65e7bf7ee2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71275296"
---
# <a name="configure-ssl-in-azure-database-for-postgresql---hyperscale-citus"></a>Konfigurieren von SSL in Azure Database for PostgreSQL – Hyperscale (Citus)
Verbindungen von Clientanwendungen mit dem Hyperscale (Citus)-Koordinatorknoten erfordern Secure Sockets Layer (SSL). Das Erzwingen von SSL-Verbindungen zwischen dem Datenbankserver und Clientanwendungen trägt zum Schutz vor Man-in-the-Middle-Angriffen bei, indem der Datenstrom zwischen dem Server und der Anwendung verschlüsselt wird.

## <a name="enforcing-ssl-connections"></a>Erzwingen von SSL-Verbindungen
Bei allen Azure Database for PostgreSQL-Servern, die über das Azure-Portal bereitgestellt werden, ist die Erzwingung von SSL-Verbindungen standardmäßig aktiviert. 

Entsprechend enthalten Verbindungszeichenfolgen, die im Azure-Portal unter dem Server in den Verbindungszeichenfolgen-Einstellungen vorab definiert sind, die erforderlichen Parameter für gängige Sprachen für die Verbindung mit dem Datenbankserver mithilfe von SSL. Der SSL-Parameter variiert je nach Connector, z.B. „ssl=true“ oder „sslmode=require“ oder „sslmode=required“ und weitere Variationen.

## <a name="ensure-your-application-or-framework-supports-ssl-connections"></a>Sicherstellen, dass die Anwendung oder das Framework SSL-Verbindungen unterstützt
Einige Anwendungsframeworks, die PostgreSQL für ihre Datenbankdienste verwenden, aktivieren SSL nicht standardmäßig während der Installation. Wenn Ihr PostgreSQL-Server SSL-Verbindungen erzwingt, die Anwendung aber nicht für SSL konfiguriert wurde, kann die Anwendung unter Umständen keine Verbindung mit dem Datenbankserver herstellen. Lesen Sie in der Dokumentation Ihrer Anwendung nach, wie SSL-Verbindungen aktiviert werden.

## <a name="applications-that-require-certificate-verification-for-ssl-connectivity"></a>Anwendungen, die eine Zertifikatüberprüfung für SSL-Verbindungen erfordern
In einigen Fällen erfordern Anwendungen eine lokale Zertifikatdatei, die aus der Zertifikatdatei (CER) einer vertrauenswürdigen Zertifizierungsstelle (Certificate Authority, CA) generiert wurde, um eine sichere Verbindung herzustellen. Das Zertifikat für Verbindungen mit Azure Database for PostgreSQL – Hyperscale (Citus) befindet sich unter https://dl.cacerts.digicert.com/DigiCertGlobalRootCA.crt.pem. Laden Sie die Zertifikatdatei herunter, und speichern Sie sie am gewünschten Ort.

### <a name="connect-using-psql"></a>Herstellen einer Verbindung mit psql
Das folgende Beispiel zeigt, wie Sie mit dem Befehlszeilenprogramm „psql“ eine Verbindung mit dem Hyperscale (Citus)-Koordinatorknoten herstellen. Verwenden Sie die Einstellung `sslmode=verify-full` für die Verbindungszeichenfolge, um die Überprüfung des SSL-Zertifikats zu erzwingen. Übergeben Sie den Pfad der lokalen Zertifikatdatei an den Parameter `sslrootcert`.

Unten ist ein Beispiel für die psql-Verbindungszeichenfolge angegeben:
```
psql "sslmode=verify-full sslrootcert=DigiCertGlobalRootCA.crt.pem host=mydemoserver.postgres.database.azure.com dbname=citus user=citus password=your_pass"
```
> [!TIP]
> Vergewissern Sie sich, dass der Wert, den Sie an `sslrootcert` übergeben haben, mit dem Dateipfad für das von Ihnen gespeicherte Zertifikat übereinstimmt.

## <a name="next-steps"></a>Nächste Schritte
Optimieren Sie die Sicherheit mit [Firewallregeln in Azure Database for PostgreSQL – Hyperscale (Citus)](concepts-hyperscale-firewall-rules.md).

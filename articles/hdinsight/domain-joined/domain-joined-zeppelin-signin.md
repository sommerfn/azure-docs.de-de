---
title: Anmeldung bei Zeppelin in Azure HDInsight nicht möglich
description: Anmeldung bei Zeppelin in Azure HDInsight nicht möglich
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/12/2019
ms.openlocfilehash: d4bb9e090b238eacec77f4c19bbf9afb3e09a912
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2019
ms.locfileid: "71091060"
---
# <a name="scenario-unable-to-sign-in-to-apache-zeppelin-in-azure-hdinsight"></a>Szenario: Anmeldung bei Apache Zeppelin in Azure HDInsight nicht möglich

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Anmeldung bei Apache Zeppelin nach Änderung des ADDS-Passworts im Active Directory nicht möglich.

## <a name="cause"></a>Ursache

Der im `activeDirectoryRealm.systemUsername` in der `shiro_ini`-Datei aufgeführte Benutzer hat das Active Directory-Kennwort geändert.

## <a name="resolution"></a>Lösung

1. Stellen Sie sicher, dass das geänderte Kennwort die Ursache ist, indem Sie `activeDirectoryRealm.systemPassword = <new password>` in die `shiro_ini`-Zeppelin-Konfiguration in Ambari aufnehmen. Entfernen Sie die `activeDirectoryRealm.hadoopSecurityCredentialPath`-Einstellung. Unten finden Sie den Speicherort von `shiro ini`.

    ![Shiro](./media/domain-joined-zeppelin-signin/shiro.png)

1. Wenn sich Benutzer nach Schritt 1 nun bei Zeppelin anmelden können, erstellen Sie eine neue `jceks`-Datei mit dem neuen Kennwort, und ersetzen Sie `activeDirectoryRealm.hadoopSecurityCredentialPath` durch die neue Datei.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Herstellen einer Verbindung mit [@AzureSupport](https://twitter.com/azuresupport), dem offiziellen Microsoft Azure-Konto zum Verbessern der Kundenfreundlichkeit. Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.

---
title: Konvertieren der Inhalte eines Dienstprinzipalzertifikats in ein Base64-codiertes Zeichenfolgenformat in Azure HDInsight
description: Konvertieren der Inhalte eines Dienstprinzipalzertifikats in ein Base64-codiertes Zeichenfolgenformat in Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 07/31/2019
ms.openlocfilehash: 2045121d16d67d09826eaaad7800f21422776cdc
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68700130"
---
# <a name="scenario-converting-service-principal-certificate-contents-to-base-64-encoded-string-format-in-azure-hdinsight"></a>Szenario: Konvertieren der Inhalte eines Dienstprinzipalzertifikats in ein Base64-codiertes Zeichenfolgenformat in Azure HDInsight

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Sie erhalten eine Fehlermeldung, dass die Eingabe keine gültige Basis64-Zeichenfolge ist, da sie ein Zeichen, das nicht Base64-codiert ist, mehr als zwei Füllzeichen oder ein Nicht-Leerzeichen zwischen den Füllzeichen enthält.

## <a name="cause"></a>Ursache

Wenn Sie PowerShell oder die Azure-Vorlagenbereitstellung verwenden, um Cluster mit Data Lake als primären oder zusätzlichen Speicher zu erstellen, weist der Inhalt des Dienstprinzipalzertifikats für den Zugriff auf das Data Lake Storage-Konto das Base64-Format auf. Eine fehlerhafte Konvertierung des Inhalts des PFX-Zertifikats in eine Base64-codierte Zeichenfolge kann zu diesem Fehler führen.

## <a name="resolution"></a>Lösung

Wenn das Dienstprinzipalzertifikat im PFX-Format vorliegt (Beispielschritte zum Erstellen von Dienstprinzipalen finden Sie [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-hdinsight-datalake-store-azure-storage)), können Sie den folgenden PowerShell-Befehl oder C#-Codeausschnitt verwenden, um den Inhalt des Zertifikats in das Base64-Format zu konvertieren.

```powershell
$servicePrincipalCertificateBase64 = [System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes(path-to-servicePrincipalCertificatePfxFile))
```

```csharp
using System;
using System.IO;

namespace ConsoleApplication
{
    class Program
    {
        static void Main(string[] args)
        {
            var certContents = File.ReadAllBytes(@"<path to pfx file>");
            string certificateData = Convert.ToBase64String(certContents);
            System.Diagnostics.Debug.WriteLine(certificateData);
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie über den [Azure-Communitysupport](https://azure.microsoft.com/support/community/) Antworten von Azure-Experten.

* Setzen Sie sich mit [@AzureSupport](https://twitter.com/azuresupport) in Verbindung – dem offiziellen Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber erhält die Azure-Community Kontakt mit den richtigen Ressourcen: Antworten, Support und Experten.

* Wenn Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführliche Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.

---
title: Der Zugriff auf Data Lake-Speicherdateien in Azure HDInsight ist nicht möglich
description: Der Zugriff auf Data Lake-Speicherdateien in Azure HDInsight ist nicht möglich
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/13/2019
ms.openlocfilehash: 3e4745d1c9e28e907d87298d1ab8ef3c9b104b1d
ms.sourcegitcommit: fe50db9c686d14eec75819f52a8e8d30d8ea725b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2019
ms.locfileid: "69014642"
---
# <a name="unable-to-access-data-lake-storage-files-in-azure-hdinsight"></a>Der Zugriff auf Data Lake-Speicherdateien in Azure HDInsight ist nicht möglich

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue-acl-verification-failed"></a>Problem: Fehler bei der ACL-Überprüfung

Es wird eine Fehlermeldung angezeigt, die der folgenden ähnelt:

```
LISTSTATUS failed with error 0x83090aa2 (Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.).
```

### <a name="cause"></a>Ursache

Der Benutzer hat möglicherweise Berechtigungen des Dienstprinzipals für Dateien/Ordner widerrufen.

### <a name="resolution"></a>Lösung

1. Überprüfen Sie, ob der Dienstprinzipal über „x“-Berechtigungen zum Durchlaufen des Pfads verfügt. Weitere Informationen finden Sie unter [Berechtigungen](https://hdinsight.github.io/ClusterCRUD/ADLS/adls-create-permission-setup.html). dfs-Beispielbefehl zum Überprüfen des Zugriffs auf Dateien/Ordner im Data Lake Speicherkonto:

    ```
    hdfs dfs -ls /<path to check access>
    ```

1. Richten Sie die erforderlichen Berechtigungen für den Zugriff auf den Pfad auf Grundlage des ausgeführten Lese-/Schreibvorgangs ein. Informationen zu den für verschiedene Dateisystemvorgänge erforderlichen Berechtigungen finden Sie hier.

---

## <a name="issue-service-principal-certificate-expiry"></a>Problem: Ablauf des Dienstprinzipalzertifikats

Es wird eine Fehlermeldung angezeigt, die der folgenden ähnelt:

```
Token Refresh failed - Received invalid http response: 500
```

### <a name="cause"></a>Ursache

Das für den Dienstprinzipalzugriff angegebene Zertifikat ist möglicherweise abgelaufen.

1. Greifen Sie über SSH auf den Hauptknoten zu. Überprüfen Sie den Zugriff auf das Speicherkonto mit dem folgenden dfs-Befehl:

    ```
    hdfs dfs -ls /
    ```

1. Vergewissern Sie sich, dass die Fehlermeldung in etwa wie folgt aussieht:

    ```
    {"stderr": "-ls: Token Refresh failed - Received invalid http response: 500, text = Response{protocol=http/1.1, code=500, message=Internal Server Error, url=http://gw0-abccluster.24ajrd4341lebfgq5unsrzq0ue.fx.internal.cloudapp.net:909/api/oauthtoken}}...
    ```

1. Rufen Sie eine der URLs aus `core-site.xml property` - `fs.azure.datalake.token.provider.service.urls` ab.

1. Führen Sie den folgenden curl-Befehl aus, um das OAuth-Token abzurufen.

    ```
    curl gw0-abccluster.24ajrd4341lebfgq5unsrzq0ue.fx.internal.cloudapp.net:909/api/oauthtoken
    ```

1. Die Ausgabe für einen gültigen Dienstprinzipal sollte etwa wie folgt aussehen:

    ```
    {"AccessToken":"MIIGHQYJKoZIhvcNAQcDoIIGDjCCBgoCAQA…….","ExpiresOn":1500447750098}
    ```

1. Wenn das Dienstprinzipalzertifikat abgelaufen ist, sieht die Ausgabe in etwa wie folgt aus:

    ```
    Exception in OAuthTokenController.GetOAuthToken: 'System.InvalidOperationException: Error while getting the OAuth token from AAD for AppPrincipalId 23abe517-2ffd-4124-aa2d-7c224672cae2, ResourceUri https://management.core.windows.net/, AADTenantId https://login.windows.net/80abc8bf-86f1-41af-91ab-2d7cd011db47, ClientCertificateThumbprint C49C25705D60569884EDC91986CEF8A01A495783 ---> Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException: AADSTS70002: Error validating credentials. AADSTS50012: Client assertion contains an invalid signature. **[Reason - The key used is expired.**, Thumbprint of key used by client: 'C49C25705D60569884EDC91986CEF8A01A495783', Found key 'Start=08/03/2016, End=08/03/2017, Thumbprint=C39C25705D60569884EDC91986CEF8A01A4956D1', Configured keys: [Key0:Start=08/03/2016, End=08/03/2017, Thumbprint=C39C25705D60569884EDC91986CEF8A01A4956D1;]]
    Trace ID: e4d34f1c-a584-47f5-884e-1235026d5000
    Correlation ID: a44d870e-6f23-405a-8b23-9b44aebfa4bb
    Timestamp: 2017-10-06 20:44:56Z ---> System.Net.WebException: The remote server returned an error: (401) Unauthorized.
    at System.Net.HttpWebRequest.GetResponse()
    at Microsoft.IdentityModel.Clients.ActiveDirectory.HttpWebRequestWrapper.<GetResponseSyncOrAsync>d__2.MoveNext()
    ```

1. Alle anderen Fehler/Zertifikatfehler, die sich auf Azure Active Directory beziehen, können erkannt werden, indem Sie die Gateway-URL pingen, um das OAuth-Token abzurufen.

1. Wenn Sie den folgenden Fehler erhalten, wenn Sie versuchen, über den HDI-Cluster auf ADLS zuzugreifen: Überprüfen Sie, ob das Zertifikat abgelaufen ist, indem Sie die oben beschriebenen Schritte ausführen.

    ```
    Error: java.lang.IllegalArgumentException: Token Refresh failed - Received invalid http response: 500, text = Response{protocol=http/1.1, code=500, message=Internal Server Error, url=http://clustername.hmssomerandomstringc.cx.internal.cloudapp.net:909/api/oauthtoken}
    ```

### <a name="resolution"></a>Lösung

Erstellen Sie ein neues Zertifikat, oder weisen Sie ein vorhandenes Zertifikat mit dem folgenden PowerShell-Skript zu:

```powershell
$clusterName = 'CLUSTERNAME'
$resourceGroupName = 'RGNAME'
$subscriptionId = 'SUBSCRIPTIONID'
$appId = 'APPLICATIONID'
$generateSelfSignedCert = $false
$addNewCertKeyCredential = $true
$certFilePath = 'NEW_CERT_PFX_LOCAL_PATH'
$certPassword = Read-Host "Enter Certificate Password"

if($generateSelfSignedCert)
{
    Write-Host "Generating new SelfSigned certificate"
    
    $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=hdinsightAdlsCert" -KeySpec KeyExchange
    $certBytes = $cert.Export([System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12, $certPassword);
    $certString = [System.Convert]::ToBase64String($certBytes)
}
else
{

    Write-Host "Reading the cert file from path $certFilePath"

    $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($certFilePath, $certPassword)
    $certString = [System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes($certFilePath))
}


Login-AzureRmAccount

if($addNewCertKeyCredential)
{
    Write-Host "Creating new KeyCredential for the app"

    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    
    New-AzureRmADAppCredential -ApplicationId $appId -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore

    Write-Host "Waiting for 30 seconds for the permissions to get propagated"

    Start-Sleep -s 30
}

Select-AzureRmSubscription -SubscriptionId $subscriptionId

Write-Host "Updating the certificate on HDInsight cluster."

Invoke-AzureRmResourceAction `
    -ResourceGroupName $resourceGroupName `
    -ResourceType 'Microsoft.HDInsight/clusters' `
    -ResourceName $clusterName `
    -ApiVersion '2015-03-01-preview' `
    -Action 'updateclusteridentitycertificate' `
    -Parameters @{ ApplicationId = $appId.ToString(); Certificate = $certString; CertificatePassword = $certPassword.ToString() } `
    -Force

```

Erstellen Sie zum Zuweisen eines vorhandenen Zertifikats ein Zertifikat, und halten Sie die PFX-Datei und das Kennwort bereit. Ordnen Sie das Zertifikat dem Dienstprinzipal zu, mit dem der Cluster erstellt wurde, und halten Sie die AppId bereit.

Führen Sie den PowerShell-Befehl aus, nachdem Sie die Parameter durch die tatsächlichen Werte ersetzt haben.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie über den [Azure-Communitysupport](https://azure.microsoft.com/support/community/) Antworten von Azure-Experten.

* Herstellen einer Verbindung mit [@AzureSupport](https://twitter.com/azuresupport), dem offiziellen Microsoft Azure-Konto zum Verbessern der Kundenfreundlichkeit. Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.

* Wenn Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.

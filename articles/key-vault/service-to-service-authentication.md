---
title: Dienst-zu-Dienst-Authentifizierung in Azure Key Vault mithilfe von .NET
description: Verwenden Sie Bibliothek „Microsoft.Azure.Services.AppAuthentication“ zur Authentifizierung beim Azure Key Vault mithilfe von .NET.
keywords: Lokale Anmeldeinformationen für die Authentifizierung bei Azure Key Vault
author: msmbaldwin
manager: rkarlin
services: key-vault
ms.author: mbaldwin
ms.date: 08/28/2019
ms.topic: conceptual
ms.service: key-vault
ms.openlocfilehash: 6996a5965454dfd5997f0c0404e0c348c68b626f
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72177469"
---
# <a name="service-to-service-authentication-to-azure-key-vault-using-net"></a>Dienst-zu-Dienst-Authentifizierung in Azure Key Vault mithilfe von .NET

Zum Authentifizieren in Azure Key Vault benötigen Sie die Anmeldeinformationen eines Azure Active Directory (Azure AD), und zwar entweder einen gemeinsamen geheimen Schlüssel oder ein Zertifikat.

Das Verwalten solcher Anmeldeinformationen kann schwierig sein. Die Versuchung ist groß, Anmeldeinformationen in Quell- oder Konfigurationsdateien einzuschließen, um sie in einer App zu bündeln. Die `Microsoft.Azure.Services.AppAuthentication` für .NET-Bibliothek vereinfacht dieses Problem. Die Anmeldeinformationen des Entwicklers werden für die Authentifizierung während der lokalen Entwicklung verwendet. Wenn die Lösung später in Azure bereitgestellt wird, wechselt die Bibliothek automatisch zu Anwendungsanmeldeinformationen. Während der lokalen Entwicklung sind Entwickleranmeldeinformationen sicherer, da Sie keine Azure AD-Anmeldeinformationen erstellen oder Anmeldeinformationen zwischen Entwicklern freigeben müssen.

Die `Microsoft.Azure.Services.AppAuthentication`-Bibliothek verwaltet die Authentifizierung automatisch, wodurch Sie sich wiederum auf Ihre Projektmappe anstatt auf Ihre Anmeldeinformationen konzentrieren können. Sie unterstützt die lokale Entwicklung mit Microsoft Visual Studio, Azure CLI oder der integrierten Azure AD-Authentifizierung. Bei Bereitstellung für eine Azure-Ressource, die eine verwaltete Identität unterstützt, verwendet die Bibliothek automatisch [verwaltete Identitäten für Azure-Ressourcen](../active-directory/msi-overview.md). Es sind keine Änderungen von Code bzw. Konfiguration erforderlich. Die Bibliothek unterstützt auch die direkte Verwendung von Azure AD-[Clientanmeldeinformationen](../azure-resource-manager/resource-group-authenticate-service-principal.md), wenn keine verwaltete Identität verfügbar ist oder wenn der Sicherheitskontext des Entwicklers bei der lokalen Entwicklung nicht bestimmt werden kann.

## <a name="prerequisites"></a>Voraussetzungen

- [Visual Studio 2019](https://www.visualstudio.com/downloads/) oder [Visual Studio 2017 v15.5](https://blogs.msdn.microsoft.com/visualstudio/2017/10/11/visual-studio-2017-version-15-5-preview/).

- Die App-Authentifizierungserweiterung für Visual Studio, die als separate Erweiterung für Visual Studio 2017 Update 5 und im Paket mit dem Produkt in Update 6 und höher verfügbar ist. Mit Update 6 oder höher können Sie die Installation der App-Authentifizierungserweiterung überprüfen, indem Sie Azure-Entwicklungstools aus dem Visual Studio-Installationsprogramm auswählen.

## <a name="using-the-library"></a>Verwenden der Bibliothek

Für .NET-Anwendungen ist die einfachste Methode zum Verwenden einer verwalteten Identität ein `Microsoft.Azure.Services.AppAuthentication`-Paket. Erfahren Sie, wie Sie loslegen können:

1. Wählen Sie **Tools** > **NuGet-Paket-Manager** > **NuGet-Pakete für Projektmappe verwalten** aus, um Ihrem Projekt Verweise auf die NuGet-Pakete [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) und [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) hinzuzufügen.

1. Fügen Sie den folgenden Code hinzu:

    ``` csharp
    using Microsoft.Azure.Services.AppAuthentication;
    using Microsoft.Azure.KeyVault;

    // Instantiate a new KeyVaultClient object, with an access token to Key Vault
    var azureServiceTokenProvider1 = new AzureServiceTokenProvider();
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider1.KeyVaultTokenCallback));

    // Optional: Request an access token to other Azure services
    var azureServiceTokenProvider2 = new AzureServiceTokenProvider();
    string accessToken = await azureServiceTokenProvider2.GetAccessTokenAsync("https://management.azure.com/").ConfigureAwait(false);
    ```

Die `AzureServiceTokenProvider`-Klasse speichert das Token im Arbeitsspeicher zwischen und ruft es kurz vor dem Ablaufdatum von Azure AD ab. Sie brauchen also vor dem Aufruf der `GetAccessTokenAsync`-Methode nicht mehr das Ablaufdatum zu überprüfen. Rufen Sie einfach die Methode auf, wenn Sie das Token verwenden möchten.

Die `GetAccessTokenAsync`-Methode erfordert einen Ressourcenbezeichner. Weitere Informationen zu Microsoft Azure-Diensten finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen](../active-directory/msi-overview.md).

## <a name="local-development-authentication"></a>Authentifizierung für die lokale Entwicklung

Für die lokale Entwicklung gibt es zwei primäre Authentifizierungsszenarien: [Authentifizierung bei Azure-Diensten](#authenticating-to-azure-services) und [Authentifizierung bei benutzerdefinierten Diensten](#authenticating-to-custom-services).

### <a name="authenticating-to-azure-services"></a>Authentifizierung bei Azure-Diensten

Lokale Computer unterstützen keine verwalteten Identitäten für Azure-Ressourcen. Daher verwendet die `Microsoft.Azure.Services.AppAuthentication`-Bibliothek Ihre Entwickleranmeldeinformationen für die Ausführung in Ihrer lokalen Entwicklungsumgebung. Wenn die Lösung in Azure bereitgestellt wird, verwendet die Bibliothek eine verwaltete Identität, um zu einem Flow zum Erteilen der OAuth 2.0-Clientanmeldeinformationen zu wechseln. Dieses Vorgehen bedeutet, dass Sie den gleichen Code ohne Sorge lokal und remote testen können.

Für die lokale Entwicklung ruft `AzureServiceTokenProvider` Token mithilfe von **Visual Studio**, der **Azure-Befehlszeilenschnittstelle** (CLI) oder der **integrierten Azure AD-Authentifizierung** ab. Jede Option wird nacheinander ausprobiert, und die Bibliothek verwendet die erste Option, die erfolgreich ausgeführt wird. Wenn keine Option funktioniert, wird eine `AzureServiceTokenProviderException`-Ausnahme mit detaillierten Informationen ausgelöst.

#### <a name="authenticating-with-visual-studio"></a>Authentifizierung mit Visual Studio

Authentifizieren mithilfe von Visual Studio:

1. Melden Sie sich bei Visual Studio an, und verwenden Sie **Extras**&nbsp;>&nbsp;**Optionen**, um **Optionen** zu öffnen.

1. Wählen Sie **Azure-Dienstauthentifizierung**, ein Konto für die lokale Entwicklung und dann **OK** aus.

Wenn Probleme bei der Verwendung von Visual Studio auftreten, z. B. Fehler im Zusammenhang mit der Tokenanbieterdatei, überprüfen Sie sorgfältig die vorherigen Schritte.

Es kann auch erforderlich sein, Ihr Entwicklertoken erneut zu authentifizieren. Wählen Sie zu diesem Zweck **Extras**&nbsp;>&nbsp;**Optionen** und dann **Azure&nbsp;Dienst&nbsp;authentifizierung** aus. Suchen Sie unter dem ausgewählten Konto nach einem Link **Erneut authentifizieren**. Wählen Sie diesen aus, um eine erneute Authentifizierung durchzuführen.

#### <a name="authenticating-with-azure-cli"></a>Authentifizierung mit Azure CLI

Um Azure CLI für die lokale Entwicklung zu verwenden, achten Sie darauf, dass Sie Version [Azure CLI v2.0.12](/cli/azure/install-azure-cli) oder höher verwenden.

So verwenden Sie die Azure-Befehlszeilenschnittstelle:

1. Suchen Sie in der Windows-Taskleiste nach „Azure CLI“, um die **Microsoft Azure-Eingabeaufforderung** zu öffnen.

1. Anmeldung beim Azure-Portal: *az login* für die Anmeldung bei Azure.

1. Überprüfen Sie den Zugriff, indem Sie *az account get-access-token --resource https://vault.azure.net* eingeben. Wenn Sie einen Fehler erhalten, vergewissern Sie sich, dass die richtige Azure-Version ordnungsgemäß installiert ist.

   Wenn Azure CLI nicht im Standardverzeichnis installiert ist, erhalten Sie möglicherweise eine Fehlermeldung, die angibt, dass `AzureServiceTokenProvider` den Pfad zu Azure CLI nicht finden kann. Verwenden Sie die Umgebungsvariable **AzureCLIPath**, um den Azure CLI-Installationsordner zu definieren. `AzureServiceTokenProvider` fügt das in der Umgebungsvariable **AzureCLIPath** angegebene Verzeichnis bei Bedarf zur Umgebungsvariable **Path** hinzu.

1. Wenn Sie mit mehreren Konten bei Azure CLI angemeldet sind, oder wenn Ihr Konto Zugriff auf mehrere Abonnements hat, müssen Sie das zu verwendende Abonnement angeben. Geben Sie den Befehl *az account set --subscription <Abonnement-ID>* ein.

Mit diesem Befehl wird nur bei einem Fehler eine Ausgabe generiert. Geben Sie den Befehl `az account list` ein, um die aktuellen Kontoeinstellungen zu überprüfen.

#### <a name="authenticating-with-azure-ad-authentication"></a>Authentifizierung mit der Azure AD-Authentifizierung

Um die Azure AD-Authentifizierung zu verwenden, überprüfen Sie Folgendes:

- Ihr lokales Active Directory wird mit Azure AD synchronisiert. Weitere Informationen finden Sie unter [Was bedeutet Hybrididentität in Azure Active Directory?](../active-directory/connect/active-directory-aadconnect.md).

- Ihr Code wird auf einem in die Domäne eingebundenen Computer ausgeführt.

### <a name="authenticating-to-custom-services"></a>Authentifizierung bei benutzerdefinierten Diensten

Wenn ein Dienst Azure-Dienste aufruft, wurden die vorherigen Schritte erfolgreich ausgeführt, da Azure-Dienste den Zugriff auf Benutzer und Anwendungen zuzulassen.

Beim Erstellen eines Diensts, der einen benutzerdefinierten Dienst aufruft, verwenden Sie Azure AD-Clientanmeldeinformationen für die lokale Entwicklungsauthentifizierung. Es gibt zwei Optionen:

- Verwenden Sie einen Dienstprinzipal für die Anmeldung bei Azure:

    1. Erstellen eines Dienstprinzipals Weitere Informationen finden Sie unter [Erstellen eines Azure-Dienstprinzipals mit Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli).

    1. Verwenden Sie Azure CLI, um sich mit dem folgenden Befehl anzumelden:

        ```azurecli
        az login --service-principal -u <principal-id> --password <password> --tenant <tenant-id> --allow-no-subscriptions
        ```

        Da der Dienstprinzipal möglicherweise keinen Zugriff auf ein Abonnement hat, verwenden Sie das Argument `--allow-no-subscriptions`.

- Verwenden Sie Umgebungsvariablen, um Details zum Dienstprinzipal anzugeben. Weitere Informationen finden Sie unter [Ausführen der Anwendung mithilfe eines Dienstprinzipals](#running-the-application-using-a-service-principal).

Nachdem Sie sich bei Azure angemeldet haben, verwendet `AzureServiceTokenProvider` den Dienstprinzipal zum Abrufen eines Tokens für die lokale Entwicklung.

Dieses Vorgehen betrifft nur die lokale Entwicklung. Wenn Ihre Lösung in Azure bereitgestellt wird, wechselt die Bibliothek zu einer verwalteten Identität zur Authentifizierung.

## <a name="running-the-application-using-managed-identity-or-user-assigned-identity"></a>Ausführen der Anwendung mithilfe einer verwalteten Identität oder benutzerseitig zugewiesenen Identität

Wenn Sie den Code auf einer Azure App Service-Instanz oder einem virtuellen Azure-Computer ausführen und eine verwaltete Identität aktiviert ist, verwendet die Bibliothek automatisch die verwaltete Identität. Es sind keine Codeänderungen erforderlich, aber die verwaltete Identität muss über *GET*-Berechtigungen für den Schlüsseltresor verfügen. Über die *Zugriffsrichtlinien* des Schlüsseltresors können Sie der verwalteten Identität *GET*-Berechtigungen erteilen.

Alternativ können Sie sich auch mit einer benutzerseitig zugewiesenen Identität authentifizieren. Weitere Informationen zu benutzerseitig zugewiesenen Identitäten finden Sie unter [Informationen zu verwalteten Identitäten für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md#how-does-the-managed-identities-for-azure-resources-work). Um sich mit einer vom Benutzer zugewiesenen Identität zu authentifizieren, müssen Sie die Client-ID der vom Benutzer zugewiesenen Identität in der Verbindungszeichenfolge angeben. Die Verbindungszeichenfolge ist in [Unterstützung der Verbindungszeichenfolge](#connection-string-support) angegeben.

## <a name="running-the-application-using-a-service-principal"></a>Ausführen der Anwendung mithilfe eines Dienstprinzipals

Es ist möglicherweise erforderlich, Azure AD-Clientanmeldeinformationen zum Authentifizieren zu erstellen. Diese Situation kann beispielsweise in den folgenden Szenarien eintreten:

- Der Code wird in einer lokalen Entwicklungsumgebung, aber nicht unter der Identität des Entwicklers ausgeführt. Service Fabric verwendet zum Beispiel das [NetworkService-Konto](../service-fabric/service-fabric-application-secret-management.md) für lokale Entwicklung.

- Der Code wird in einer lokalen Entwicklungsumgebung ausgeführt, und Sie authentifizieren sich bei einem benutzerdefinierten Dienst, sodass Sie Ihre Entwickleridentität nicht nutzen können.

- Ihr Code wird für eine Azure-Computeressource ausgeführt, die verwaltete Identitäten für Azure-Ressourcen noch nicht unterstützt (z.B. Azure Batch).

Es gibt drei primäre Methoden, um einen Dienstprinzipal zur Ausführung Ihrer Anwendung zu verwenden. Um eine dieser Methoden zu verwenden, müssen Sie zunächst einen Dienstprinzipal erstellen. Weitere Informationen finden Sie unter [Erstellen eines Azure-Dienstprinzipals mit Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli).

### <a name="use-a-certificate-in-local-keystore-to-sign-into-azure-ad"></a>Verwenden eines Zertifikats in einem lokalen Keystore für die Anmeldung bei Azure AD

1. Erstellen Sie mit dem Azure CLI-Befehl [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) ein Dienstprinzipalzertifikat.

    ```azurecli
    az ad sp create-for-rbac --create-cert
    ```

    Durch diesen Befehl wird eine PEM-Datei (privater Schlüssel) erstellt, die in Ihrem Basisverzeichnis gespeichert wird. Stellen Sie dieses Zertifikat entweder dem *LocalMachine*- oder *CurrentUser*-Store bereit.

    > [!Important]
    > Der CLI-Befehl generiert eine PEM-Datei, aber Windows bietet systemeigene Unterstützung nur für PFX-Zertifikate. Um stattdessen ein PFX-Zertifikat zu generieren, verwenden Sie die hier gezeigten PowerShell-Befehle: [Erstellen eines Dienstprinzipals mit selbstsigniertem Zertifikat](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-self-signed-certificate). Mit diesen Befehlen wird automatisch auch das Zertifikat bereitgestellt.

1. Legen Sie eine Umgebungsvariable namens **AzureServicesAuthConnectionString** auf den folgenden Wert fest:

    ```azurecli
    RunAs=App;AppId={AppId};TenantId={TenantId};CertificateThumbprint={Thumbprint};
          CertificateStoreLocation={CertificateStore}
    ```

    Ersetzen Sie *{AppId}* , *{TenantId}* und *{Thumbprint}* durch die in Schritt 1 generierten Werte. Ersetzen Sie *{CertificateStore}* entweder durch *LocalMachine* oder durch *CurrentUser*, abhängig von Ihrem Bereitstellungsplan.

1. Führen Sie die Anwendung aus.

### <a name="use-a-shared-secret-credential-to-sign-into-azure-ad"></a>Verwenden eines gemeinsamen geheimen Schlüssel als Azure AD-Anmeldeinformationen

1. Erstellen Sie ein Dienstprinzipalzertifikat mit einem Kennwort mithilfe des Azure CLI-Befehls [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) mit dem Parameter „--sdk-auth“.

    ```azurecli
    az ad sp create-for-rbac --sdk-auth
    ```

1. Legen Sie eine Umgebungsvariable namens **AzureServicesAuthConnectionString** auf den folgenden Wert fest:

    ```azurecli
    RunAs=App;AppId={AppId};TenantId={TenantId};AppKey={ClientSecret}
    ```

    Ersetzen Sie _{AppId}_ , _{TenantId}_ und _{ClientSecret}_ durch die in Schritt 1 generierten Werte.

1. Führen Sie die Anwendung aus.

Nachdem alles ordnungsgemäß eingerichtet wurde, sind keine weiteren Codeänderungen mehr erforderlich. `AzureServiceTokenProvider` verwendet die Umgebungsvariable und das Zertifikat zum Authentifizieren bei Azure AD.

### <a name="use-a-certificate-in-key-vault-to-sign-into-azure-ad"></a>Verwenden eines Zertifikats in Key Vault für die Anmeldung bei Azure AD

Mit dieser Option können Sie das Clientzertifikat eines Dienstprinzipals in Key Vault speichern und für die Authentifizierung des Dienstprinzipals verwenden. Diese Option können Sie für folgende Szenarien verwenden:

- Lokale Authentifizierung, bei der Sie sich mit einem expliziten Dienstprinzipal authentifizieren und die Anmeldeinformationen des Dienstprinzipals sicher in einem Key Vault aufbewahren möchten. Das Entwicklerkonto muss auf den Key Vault zugreifen können.

- Authentifizierung über Azure, bei der Sie explizite Anmeldeinformationen verwenden und die Anmeldeinformationen des Dienstprinzipals sicher in einem Key Vault aufbewahren möchten. Sie können diese Option auch für ein mandantenübergreifendes Szenario verwenden. Die Verwaltete Identität muss auf den Key Vault zugreifen können.

Die verwaltete Identität oder Ihre Entwickleridentität muss die Berechtigung haben, das Clientzertifikat aus dem Key Vault abzurufen. Die AppAuthentication-Bibliothek verwendet das abgerufene Zertifikat als Clientanmeldeinformationen des Dienstprinzipals.

Verwenden eines Clientzertifikats zur Authentifizierung des Dienstprinzipals:

1. Erstellen Sie ein Dienstprinzipalzertifikat, und speichern Sie es automatisch in ihrem Key Vault. Verwenden Sie den Azure CLI-Befehl [az ad sp create-for-rbac --keyvault \<keyvaultname> --cert \<certificatename> --create-cert --skip-assignment](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac):

    ```azurecli
    az ad sp create-for-rbac --keyvault <keyvaultname> --cert <certificatename> --create-cert --skip-assignment
    ```

    Der Bezeichner des Zertifikats wird eine URL im Format `https://<keyvaultname>.vault.azure.net/secrets/<certificatename>` sein.

1. Ersetzen Sie `{KeyVaultCertificateSecretIdentifier}` in dieser Verbindungszeichenfolge mit dem Zertifikatbezeichner:

    ```azurecli
    RunAs=App;AppId={TestAppId};KeyVaultCertificateSecretIdentifier={KeyVaultCertificateSecretIdentifier}
    ```

    Wenn Ihr Key Vault beispielsweise *myKeyVault* hieß und Sie ein Zertifikat mit dem Namen *myCert* erstellt haben, würde der Zertifikatbezeichner wie folgt lauten:

    ```azurecli
    RunAs=App;AppId={TestAppId};KeyVaultCertificateSecretIdentifier=https://myKeyVault.vault.azure.net/secrets/myCert
    ```

## <a name="connection-string-support"></a>Unterstützung der Verbindungszeichenfolge

Standardmäßig verwendet `AzureServiceTokenProvider` mehrere Methoden zum Abrufen eines Tokens.

Um den Prozess zu steuern, verwenden Sie eine Verbindungszeichenfolge, die an den `AzureServiceTokenProvider`-Konstruktor übergeben oder in der Umgebungsvariablen *AzureServicesAuthConnectionString* angegeben wurde.

Die folgenden Optionen werden unterstützt:

| Option für die Verbindungszeichenfolge | Szenario | Kommentare|
|:--------------------------------|:------------------------|:----------------------------|
| `RunAs=Developer; DeveloperTool=AzureCli` | Lokale Entwicklung | `AzureServiceTokenProvider` verwendet AzureCli, um das Token abzurufen. |
| `RunAs=Developer; DeveloperTool=VisualStudio` | Lokale Entwicklung | `AzureServiceTokenProvider` verwendet Visual Studio, um das Token abzurufen. |
| `RunAs=CurrentUser` | Lokale Entwicklung | `AzureServiceTokenProvider` verwendet die integrierte Azure AD-Authentifizierung zum Abrufen des Tokens. |
| `RunAs=App` | [Verwaltete Identitäten für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/index.yml) | `AzureServiceTokenProvider` verwendet eine verwaltete Identität, um das Token abzurufen. |
| `RunAs=App;AppId={ClientId of user-assigned identity}` | [Benutzerseitig zugewiesene Identität für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md#how-does-the-managed-identities-for-azure-resources-work) | `AzureServiceTokenProvider` verwendet eine vom Benutzer zugewiesene Identität, um das Token abzurufen. |
| `RunAs=App;AppId={TestAppId};KeyVaultCertificateSecretIdentifier={KeyVaultCertificateSecretIdentifier}` | Benutzerdefinierte Dienstauthentifizierung | `KeyVaultCertificateSecretIdentifier` ist die geheime ID des Zertifikats. |
| `RunAs=App;AppId={AppId};TenantId={TenantId};CertificateThumbprint={Thumbprint};CertificateStoreLocation={LocalMachine or CurrentUser}`| Dienstprinzipal | `AzureServiceTokenProvider` verwendet ein Zertifikat zum Abrufen des Tokens von Azure AD. |
| `RunAs=App;AppId={AppId};TenantId={TenantId};CertificateSubjectName={Subject};CertificateStoreLocation={LocalMachine or CurrentUser}` | Dienstprinzipal | `AzureServiceTokenProvider` verwendet ein Zertifikat zum Abrufen des Tokens von Azure AD.|
| `RunAs=App;AppId={AppId};TenantId={TenantId};AppKey={ClientSecret}` | Dienstprinzipal |`AzureServiceTokenProvider` verwendet einen geheimen Schlüssel zum Abrufen des Tokens von Azure AD. |

## <a name="samples"></a>Beispiele

Um die `Microsoft.Azure.Services.AppAuthentication`-Bibliothek in der Praxis zu sehen, lesen Sie die folgenden Codebeispiele.

- [Verwenden einer verwalteten Identität zum Abrufen eines Geheimnisses aus Azure Key Vault zur Laufzeit](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet)

- [Programmgesteuerte Bereitstellung einer Azure Resource Manager-Vorlage aus einem virtuellen Azure-Computer mit einer verwalteten Identität](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet).

- [Verwenden eines .NET Core-Beispiels und einer verwalteten Identität zum Aufrufen der Azure-Dienste aus einer Azure Linux-VM](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/).

## <a name="appauthentication-troubleshooting"></a>AppAuthentication-Problembehandlung

### <a name="common-issues-during-local-development"></a>Allgemeine Probleme bei der lokalen Entwicklung

#### <a name="azure-cli-is-not-installed-youre-not-logged-in-or-you-dont-have-the-latest-version"></a>Azure CLI ist nicht installiert, Sie sind nicht angemeldet, oder Sie verfügen nicht über die neueste Version.

Führen Sie *az account get-access-token* aus, um zu überprüfen, ob Azure CLI ein Token für Sie anzeigt. Besagt die Meldung, dass **kein solches Programm gefunden** wurde, installieren Sie die [neueste Version der Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest). Sie werden unter Umständen aufgefordert, sich anzumelden.

#### <a name="azureservicetokenprovider-cant-find-the-path-for-azure-cli"></a>AzureServiceTokenProvider kann den Pfad für Azure CLI nicht finden

AzureServiceTokenProvider sucht an den Standardinstallationsorten nach Azure CLI. Wenn Azure CLI nicht gefunden wird, legen Sie die Umgebungsvariable **AzureCLIPath** auf den Azure CLI-Installationsordner fest. AzureServiceTokenProvider fügt die Umgebungsvariable der PATH-Umgebungsvariablen hinzu.

#### <a name="youre-logged-into-azure-cli-using-multiple-accounts-the-same-account-has-access-to-subscriptions-in-multiple-tenants-or-you-get-an-access-denied-error-when-trying-to-make-calls-during-local-development"></a>Sie sind bei Azure CLI mit mehreren Konten angemeldet, dasselbe Konto hat Zugriff auf Abonnements in mehreren Mandanten, oder Sie erhalten einen Fehler „Zugriff verweigert“, wenn Sie versuchen, während der lokalen Entwicklung Aufrufe auszuführen.

Legen Sie mithilfe der Azure CLI das Standardabonnement auf ein Abonnement fest, das über das gewünschte Konto verfügt. Das Abonnement muss sich im selben Mandanten wie die Ressource befinden, auf die Sie zugreifen möchten: **az account set --subscription [Abonnement-ID]** . Wenn keine Ausgabe angezeigt wird, war die Ausführung erfolgreich. Vergewissern Sie sich, dass das richtige Konto jetzt der Standardwert ist, indem Sie **az account list** verwenden.

### <a name="common-issues-across-environments"></a>Häufige Probleme in allen Umgebungen

#### <a name="unauthorized-access-access-denied-forbidden-or-similar-error"></a>Fehler der Art „Nicht autorisierter Zugriff“, „Zugriff verweigert“, „Unzulässig“ oder ähnlicher Fehler

Der verwendete Prinzipal besitzt keinen Zugriff auf die Ressource, auf die er zugreifen möchte. Erteilen Sie entweder Ihrem Benutzerkonto oder dem MSI-"Contributor" des App Service Zugriff auf eine Ressource. Welche dieser Optionen Sie ausführen müssen, hängt davon ab, ob Sie das Beispiel auf dem lokalen Computer ausführen oder es in Azure in Ihrem App Service bereitstellen. Einige Ressourcen wie Schlüsseltresore haben ebenfalls ihre eigenen [Zugriffsrichtlinien](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault#data-plane-and-access-policies), die Sie verwenden, um Prinzipalen wie Benutzern, Apps und Gruppen Zugriff zu erteilen.

### <a name="common-issues-when-deployed-to-azure-app-service"></a>Häufige Probleme bei Bereitstellung in Azure App Service

#### <a name="managed-identity-isnt-set-up-on-the-app-service"></a>Verwaltete Identität ist nicht im App Service eingerichtet

Überprüfen Sie mithilfe der [Kudu-Debugkonsole](https://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/), ob die Umgebungsvariablen MSI_ENDPOINT und MSI_SECRET vorhanden sind. Wenn diese Umgebungsvariablen nicht vorhanden sind, ist die verwaltete Identität im App Service nicht aktiviert.

### <a name="common-issues-when-deployed-locally-with-iis"></a>Häufige Probleme bei der lokalen Bereitstellung mit IIS

#### <a name="cant-retrieve-tokens-when-debugging-app-in-iis"></a>Beim Debuggen der App in IIS können keine Token abgerufen werden

Standardmäßig wird AppAuth in IIS in einem anderen Benutzerkontext ausgeführt. Daher besitzt es keinen Zugriff auf Ihre Entwickleridentität zum Abrufen von Zugriffstoken. Mit den folgenden Schritten können Sie IIS für die Ausführung mit Ihrem Benutzerkontext konfigurieren:
- Konfigurieren Sie den Anwendungspool für die Web-App so, dass sie als Ihr aktuelles Benutzerkonto ausgeführt wird. Weitere Informationen finden Sie [hier](https://docs.microsoft.com/iis/manage/configuring-security/application-pool-identities#configuring-iis-application-pool-identities).
- Konfigurieren Sie „setProfileEnvironment“ auf „true“. Weitere Informationen finden Sie [hier](https://docs.microsoft.com/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration). 

    - Wechseln Sie zu „%windir%\System32\inetsrv\config\applicationHost.config“.
    - Suchen Sie nach „setProfileEnvironment“. Wenn der Wert auf „False“ festgelegt ist, ändern Sie ihn in „True“. Wenn er nicht vorhanden ist, fügen Sie ihn dem processModel-Element (/configuration/system.applicationHost/applicationPools/applicationPoolDefaults/processModel/@setProfileEnvironment) als Attribut hinzu, und legen Sie ihn auf „True“ fest.

- Erfahren Sie mehr über [verwaltete Identitäten für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/index.yml).
- Erfahren Sie mehr über Azure AD-[Authentifizierungsszenarien](../active-directory/develop/active-directory-authentication-scenarios.md).

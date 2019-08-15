---
title: 'Tutorial: Verwenden von Azure Key Vault mit einem virtuellen Windows-Computer in Python | Microsoft-Dokumentation'
description: In diesem Tutorial konfigurieren Sie eine ASP.NET Core-Anwendung zum Lesen eines Geheimnisses aus Ihrem Schlüsseltresor.
services: key-vault
author: msmbaldwin
manager: rajvijan
ms.service: key-vault
ms.topic: tutorial
ms.date: 09/05/2018
ms.author: mbaldwin
ms.custom: mvc
ms.openlocfilehash: ae9aaea91f62a7e79835e65a5a9f9c76017afdb1
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68934384"
---
# <a name="tutorial-use-azure-key-vault-with-a-windows-virtual-machine-in-python"></a>Tutorial: Verwenden von Azure Key Vault mit einem virtuellen Windows-Computer in Python

Azure Key Vault unterstützt Sie beim Schutz von Geheimnissen wie API-Schlüsseln und Datenbank-Verbindungszeichenfolgen, die für den Zugriff auf Ihre Anwendungen, Dienste und IT-Ressourcen benötigt werden.

In diesem Tutorial wird beschrieben, wie Sie vorgehen müssen, damit eine Konsolenanwendung Informationen aus Azure Key Vault lesen kann. Verwenden Sie hierfür verwaltete Identitäten für Azure-Ressourcen. 

Das Tutorial veranschaulicht folgende Vorgehensweisen:

> [!div class="checklist"]
> * Erstellen eines Schlüsseltresors
> * Hinzufügen eines Geheimnisses zum Schlüsseltresor.
> * Abrufen eines Geheimnisses aus dem Schlüsseltresor
> * Erstellen Sie einen virtuellen Azure-Computer.
> * Aktivieren einer verwalteten Identität
> * Zuweisen von Berechtigungen zur VM-Identität

Machen Sie sich zunächst mit den [grundlegenden Konzepten von Key Vault](key-vault-whatis.md#basic-concepts) vertraut. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen.

## <a name="prerequisites"></a>Voraussetzungen

Für Windows, Mac und Linux:
  * [Git-Client](https://git-scm.com/downloads)
  * Für dieses Tutorial ist es erforderlich, dass Sie die Azure CLI lokal ausführen. Hierfür muss Azure CLI Version 2.0.4 oder höher installiert sein. Führen Sie `az --version` aus, um die Version zu finden. Informationen zur Installation und Aktualisierung der CLI finden Sie bei Bedarf unter [Installieren von Azure CLI 2.0](https://review.docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="about-managed-service-identity"></a>Informationen zur verwalteten Dienstidentität

Azure Key Vault dient zum sicheren Speichern von Anmeldeinformationen, damit diese nicht in Ihrem Code erscheinen. Sie müssen sich jedoch bei Azure Key Vault authentifizieren, um Ihre Schlüssel abzurufen. Und für die Authentifizierung bei Key Vault benötigen Sie wiederum Anmeldeinformationen. Ein klassisches Bootstrap-Dilemma. Die verwaltete Dienstidentität (Managed Service Identity, MSI) löst dieses Problem, indem sie eine _Bootstrap-Identität_ bereitstellt, die den Prozess vereinfacht.

Wenn Sie MSI für einen Azure-Dienst aktivieren (beispielsweise für Azure Virtual Machines, Azure App Service oder Azure Functions), erstellt Azure einen [Dienstprinzipal](key-vault-whatis.md#basic-concepts). MSI führt diesen Schritt für die Instanz des Diensts in Azure Active Directory (Azure AD) aus und fügt die Anmeldeinformationen des Dienstprinzipals in diese Instanz ein. 

![MSI](media/MSI.png)

Als Nächstes ruft Ihr Code einen lokalen, für die Azure-Ressource verfügbaren Metadatendienst auf, um ein Zugriffstoken zu erhalten. Ihr Code verwendet das vom lokalen MSI-Endpunkt erhaltene Zugriffstoken für die Authentifizierung bei einem Azure Key Vault-Dienst. 

## <a name="log-in-to-azure"></a>Anmelden an Azure

Geben Sie Folgendes ein, um sich mithilfe der Azure-Befehlszeilenschnittstelle bei Azure anzumelden:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#az-group-create) eine Ressourcengruppe. 

Wählen Sie einen Namen für die Ressourcengruppe aus, und fügen Sie den Platzhalter ein. Im folgenden Beispiel wird eine Ressourcengruppe am Standort „USA, Westen“ erstellt:

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "West US"
```

Ihre neu erstellte Ressourcengruppe wird im gesamten Tutorial verwendet.

## <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors

Geben Sie zum Erstellen eines Schlüsseltresors in der Ressourcengruppe, die Sie im vorherigen Schritt erstellt haben, die folgenden Informationen ein:

* Name des Schlüsseltresors: eine Zeichenfolge mit 3 bis 24 Zeichen, die nur Zahlen (0–9), Buchstaben (a–z, A–Z) und Bindestriche (-) enthalten darf.
* Ressourcengruppenname
* Standort: **USA, Westen**

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```
An diesem Punkt ist nur Ihr Azure-Konto zum Ausführen von Vorgängen für den neuen Schlüsseltresor autorisiert.

## <a name="add-a-secret-to-the-key-vault"></a>Hinzufügen eines Geheimnisses zum Schlüsseltresor

Wir fügen ein Geheimnis hinzu, um die Vorgehensweise zu veranschaulichen. Bei diesem Geheimnis kann es sich beispielsweise um eine SQL-Verbindungszeichenfolge oder um beliebige andere Informationen handeln, die sowohl sicher aufbewahrt werden als auch für Ihre Anwendung verfügbar sein müssen.

Geben Sie den folgenden Befehl ein, um im Schlüsseltresor ein Geheimnis namens **AppSecret** zu erstellen:

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Dieses Geheimnis speichert den Wert **MySecret**.

## <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers
Mithilfe einer der folgenden Methoden können Sie einen virtuellen Computer erstellen:

* [Die Azure-CLI](../virtual-machines/windows/quick-create-cli.md)
* [PowerShell](../virtual-machines/windows/quick-create-powershell.md)
* [Azure-Portal](../virtual-machines/windows/quick-create-portal.md)

## <a name="assign-an-identity-to-the-vm"></a>Zuweisen einer Identität zum virtuellen Computer
In diesem Schritt erstellen Sie mithilfe des folgenden Befehls in der Azure CLI eine systemseitig zugewiesene Identität für den virtuellen Computer:

```azurecli
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

Beachten Sie die systemseitig zugewiesene Identität, die im folgenden Code angezeigt wird. Die Ausgabe des obigen Befehls lautet etwa wie folgt: 

```azurecli
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

## <a name="assign-permissions-to-the-vm-identity"></a>Zuweisen von Berechtigungen für die VM-Identität
Jetzt können Sie Ihrem Schlüsseltresor die zuvor erstellten Identitätsberechtigungen zuweisen, indem Sie den folgenden Befehl ausführen:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="log-on-to-the-virtual-machine"></a>Anmelden beim virtuellen Computer

Führen Sie die Anweisungen unter [Herstellen einer Verbindung mit einem virtuellen Azure-Computer unter Windows und Anmelden auf diesem Computer](../virtual-machines/windows/connect-logon.md) aus, um sich beim virtuellen Computer anzumelden.

## <a name="create-and-run-a-sample-python-app"></a>Erstellen und Ausführen einer Python-Beispiel-App

Im nächsten Abschnitt finden Sie eine Beispieldatei mit dem Namen *Sample.py*. Die Datei verwendet eine [Anforderungsbibliothek](http://docs.python-requests.org/en/master/), um HTTP-GET-Aufrufe auszuführen.

## <a name="edit-samplepy"></a>Bearbeiten der Datei „Sample.py“

Nachdem Sie *Sample.py* erstellt haben, öffnen Sie die Datei, und kopieren Sie den Code in diesem Abschnitt. 

Der Code ist ein zweistufiger Prozess:
1. Rufen Sie ein Token vom lokalen MSI-Endpunkt auf dem virtuellen Computer ab.  
  Dadurch wird auch ein Token von Azure AD abgerufen.
1. Übergeben Sie das Token an Ihren Schlüsseltresor, und rufen Sie anschließend Ihr Geheimnis ab. 

```
    # importing the requests library 
    import requests 

    # Step 1: Fetch an access token from a Managed Identity enabled azure resource.      
    # Note that the resource here is https://vault.azure.net for public cloud and api-version is 2018-02-01
    MSI_ENDPOINT = "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net"
    r = requests.get(MSI_ENDPOINT, headers = {"Metadata" : "true"}) 
      
    # extracting data in json format 
    # This request gets an access_token from Azure AD by using the local MSI endpoint.
    data = r.json() 
    
    # Step 2: Pass the access_token received from previous HTTP GET call to your key vault.
    KeyVaultURL = "https://prashanthwinvmvault.vault.azure.net/secrets/RandomSecret?api-version=2016-10-01"
    kvSecret = requests.get(url = KeyVaultURL, headers = {"Authorization": "Bearer " + data["access_token"]})
    
    print(kvSecret.json()["value"])
```

Sie können den Geheimniswert anzeigen, indem Sie den folgenden Code ausführen: 

```
python Sample.py
```

Der Code oben veranschaulicht, wie Sie mit Azure Key Vault Vorgänge auf einem virtuellen Windows-Computer ausführen. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie den virtuellen Computer und Ihren Schlüsseltresor, wenn diese nicht mehr benötigt werden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Azure Key Vault-REST-API](https://docs.microsoft.com/rest/api/keyvault/)

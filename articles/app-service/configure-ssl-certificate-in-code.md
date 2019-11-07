---
title: Verwenden eines SSL-Zertifikats in Anwendungscode – Azure App Service | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Clientzertifikate verwenden, um eine Verbindung mit Remoteressourcen herzustellen, die diese erfordern.
services: app-service
documentationcenter: ''
author: cephalin
manager: gwallace
editor: ''
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/16/2019
ms.author: cephalin
ms.reviewer: yutlin
ms.custom: seodec18
ms.openlocfilehash: 1f042f72f82d2198472fe81670c697c0c4b28321
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510547"
---
# <a name="use-an-ssl-certificate-in-your-application-code-in-azure-app-service"></a>Verwenden eines SSL-Zertifikats in Ihrem Anwendungscode in Azure App Service

Der Code Ihrer App Service-App kann als Client fungieren und auf einen externen Dienst zugreifen, für den Zertifikatauthentifizierung erforderlich ist. Diese Schrittanleitung zeigt, wie Sie öffentliche oder private Zertifikate in Ihrem Anwendungscode verwenden.

Bei der Verwendung von Zertifikaten in Ihrem Code wird die SSL-Funktionalität in App Service genutzt. Dazu muss Ihre App im Tarif **Basic** oder höher ausgeführt werden. Alternativ dazu können Sie [die Zertifikatdatei in Ihr App-Repository einschließen](#load-certificate-from-file), dies ist jedoch keine empfohlene Vorgehensweise für private Zertifikate.

Wenn Sie Ihre SSL-Zertifikate über App Service verwalten, können Sie die Zertifikate und Ihren Anwendungscode separat verwalten und Ihre sensiblen Daten schützen.

## <a name="prerequisites"></a>Voraussetzungen

Im Rahmen dieser Schrittanleitung müssen Sie folgende Schritte durchführen:

- [Erstellen einer App Service-App](/azure/app-service/)
- [Hinzufügen eines Zertifikats zur App](configure-ssl-certificate.md)

## <a name="find-the-thumbprint"></a>Ermitteln des Fingerabdrucks

Wählen Sie im linken Menü des <a href="https://portal.azure.com" target="_blank">Azure-Portals</a> die Optionen **App Services** >  **\<App-Name>** aus.

Wählen Sie im linken Navigationsbereich Ihrer App **TLS-/SSL-Einstellungen** und dann **Private Schlüsselzertifikate (PFX)** oder **Öffentliche Schlüsselzertifikate (CER)** aus.

Ermitteln Sie das Zertifikat, das Sie verwenden möchten, und kopieren Sie den Fingerabdruck des Zertifikats.

![Kopieren des Zertifikatfingerabdrucks](./media/configure-ssl-certificate/create-free-cert-finished.png)

## <a name="load-the-certificate"></a>Laden des Zertifikats

Um ein Zertifikat in Ihrem App-Code zu verwenden, fügen Sie der App-Einstellung `WEBSITE_LOAD_CERTIFICATES` seinen Fingerabdruck hinzu, indem Sie den folgenden Befehl in der <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a> ausführen:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_LOAD_CERTIFICATES=<comma-separated-certificate-thumbprints>
```

Damit auf alle Zertifikate zugegriffen werden kann, legen Sie den Wert auf `*` fest.

> [!NOTE]
> Diese Einstellung platziert die angegebenen Zertifikate für die meisten Tarife im Speicher [Current User\My](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores). Im Tarif **Isolated** allerdings (d.h., die App wird in einer [App Service-Umgebung](environment/intro.md) ausgeführt) wird das Zertifikat im Speicher [Local Machine\My](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores) platziert.
>

Die konfigurierten Zertifikate können jetzt von Ihrem Code verwendet werden.

## <a name="load-the-certificate-in-code"></a>Laden des Zertifikats im Code

Nachdem Ihr Zertifikat verfügbar ist, greifen Sie in C#-Code über den Fingerabdruck des Zertifikats auf dieses zu. Der folgende Code lädt ein Zertifikat mit dem Fingerabdruck `E661583E8FABEF4C0BEF694CBC41C28FB81CD870`.

```csharp
using System;
using System.Security.Cryptography.X509Certificates;

...
X509Store certStore = new X509Store(StoreName.My, StoreLocation.CurrentUser);
certStore.Open(OpenFlags.ReadOnly);
X509Certificate2Collection certCollection = certStore.Certificates.Find(
                            X509FindType.FindByThumbprint,
                            // Replace below with your certificate's thumbprint
                            "E661583E8FABEF4C0BEF694CBC41C28FB81CD870",
                            false);
// Get the first cert with the thumbprint
if (certCollection.Count > 0)
{
    X509Certificate2 cert = certCollection[0];
    // Use certificate
    Console.WriteLine(cert.FriendlyName);
}
certStore.Close();
...
```

<a name="file"></a>
## <a name="load-certificate-from-file"></a>Laden des Zertifikats aus einer Datei

Wenn Sie die Zertifikatdatei aus Ihrem Anwendungsverzeichnis laden müssen, sollte der Upload besser über [FTPS](deploy-ftp.md) anstatt z.B. über [Git](deploy-local-git.md) erfolgen. Halten Sie vertrauliche Daten wie beispielsweise ein privates Zertifikat aus der Quellcodeverwaltung heraus.

Auch wenn Sie die Datei direkt in Ihrem .NET-Code laden, überprüft die Bibliothek dennoch, ob das aktuelle Benutzerprofil geladen ist. Um das aktuelle Benutzerprofil zu laden, legen Sie die App-Einstellung `WEBSITE_LOAD_USER_PROFILE` mit dem folgenden Befehl in der <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a> fest.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_LOAD_USER_PROFILE=1
```

Sobald diese Einstellung festgelegt ist, lädt das folgende C#-Beispiel ein Zertifikat namens `mycert.pfx` aus dem `certs`-Verzeichnis des Repositorys Ihrer App.

```csharp
using System;
using System.Security.Cryptography.X509Certificates;

...
// Replace the parameter with "~/<relative-path-to-cert-file>".
string certPath = Server.MapPath("~/certs/mycert.pfx");

X509Certificate2 cert = GetCertificate(certPath, signatureBlob.Thumbprint);
...
```

## <a name="more-resources"></a>Weitere Ressourcen

* [Schützen eines benutzerdefinierten DNS-Namens mit einer SSL-Bindung](configure-ssl-bindings.md)
* [Erzwingen von HTTPS](configure-ssl-bindings.md#enforce-https)
* [Erzwingen von TLS 1.1/1.2](configure-ssl-bindings.md#enforce-tls-versions)
* [Häufig gestellte Fragen: App Service-Zertifikate](https://docs.microsoft.com/azure/app-service/faq-configuration-and-management/)

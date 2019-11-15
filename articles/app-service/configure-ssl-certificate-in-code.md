---
title: 'Verwenden eines SSL-Zertifikats im Code: Azure App Service | Microsoft-Dokumentation'
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
ms.date: 11/04/2019
ms.author: cephalin
ms.reviewer: yutlin
ms.custom: seodec18
ms.openlocfilehash: 93dfe784d45cd9cd93d22c5e8c3275c563f7f88b
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73572082"
---
# <a name="use-an-ssl-certificate-in-your-code-in-azure-app-service"></a>Verwenden eines SSL-Zertifikats in Ihrem Code in Azure App Service

Sie können in Ihrem Anwendungscode auf die [öffentlichen oder privaten Zertifikate zugreifen, die Sie App Service hinzufügen](configure-ssl-certificate.md). Der App-Code kann als Client fungieren und auf einen externen Dienst mit erforderlicher Zertifikatauthentifizierung zugreifen oder ggf. kryptografische Aufgaben ausführen. Diese Schrittanleitung zeigt, wie Sie öffentliche oder private Zertifikate in Ihrem Anwendungscode verwenden.

Bei der Verwendung von Zertifikaten in Ihrem Code wird die SSL-Funktionalität in App Service genutzt. Dazu muss Ihre App im Tarif **Basic** oder höher ausgeführt werden. Befindet sich Ihre App in der Ebene **Free** oder **Shared**, können Sie [die Zertifikatsdatei in Ihr App-Repository aufnehmen](#load-certificate-from-file).

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

## <a name="make-the-certificate-accessible"></a>Sicherstellen, dass auf das Zertifikat zugegriffen werden kann

Fügen Sie zum Zugreifen auf ein Zertifikat in Ihrem App-Code der App-Einstellung `WEBSITE_LOAD_CERTIFICATES` den Fingerabdruck hinzu, indem Sie den folgenden Befehl in <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a> ausführen:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_LOAD_CERTIFICATES=<comma-separated-certificate-thumbprints>
```

Damit auf alle Zertifikate zugegriffen werden kann, legen Sie den Wert auf `*` fest.

## <a name="load-certificate-in-windows-apps"></a>Laden eines Zertifikats in Windows-Apps

Die App-Einstellung `WEBSITE_LOAD_CERTIFICATES` macht die angegebenen Zertifikate für Ihre unter Windows gehostete App im Windows-Zertifikatspeicher zugänglich. Der Speicherort hängt vom [Tarif](overview-hosting-plans.md) ab:

- Tarif **Isolated**: [Local Machine\My](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores) 
- Alle anderen Tarife: [Current User\My](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores)

In C#-Code greifen Sie auf das Zertifikat anhand des Zertifikatfingerabdrucks zu. Der folgende Code lädt ein Zertifikat mit dem Fingerabdruck `E661583E8FABEF4C0BEF694CBC41C28FB81CD870`.

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

In Java-Code greifen Sie über den Speicher „Windows-MY“ unter Verwendung des Felds für den Antragstellernamen auf das Zertifikat zu (siehe [Öffentliches Schlüsselzertifikat](https://en.wikipedia.org/wiki/Public_key_certificate)). Der folgende Code zeigt, wie Sie ein Zertifikat für einen privaten Schlüssel laden:

```java
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RequestMapping;
import java.security.KeyStore;
import java.security.cert.Certificate;
import java.security.PrivateKey;

...
KeyStore ks = KeyStore.getInstance("Windows-MY");
ks.load(null, null); 
Certificate cert = ks.getCertificate("<subject-cn>");
PrivateKey privKey = (PrivateKey) ks.getKey("<subject-cn>", ("<password>").toCharArray());

// Use the certificate and key
...
```

Informationen zu Sprachen, die keine Unterstützung oder nur unzureichende Unterstützung für den Windows-Zertifikatspeicher bieten, finden Sie unter [Laden des Zertifikats aus einer Datei](#load-certificate-from-file).

## <a name="load-certificate-in-linux-apps"></a>Laden eines Zertifikats in Linux-Apps

Die App-Einstellungen `WEBSITE_LOAD_CERTIFICATES` machen die angegebenen Zertifikate für Ihre unter Linux gehosteten Apps (einschließlich benutzerdefinierter Container-Apps) als Dateien zugänglich. Die Dateien befinden sich in den folgenden Verzeichnissen:

- Private Zertifikate: `/var/ssl/private` (Dateien vom Typ `.p12`)
- Öffentliche Zertifikate: `/var/ssl/certs` (Dateien vom Typ `.der`)

Die Zertifikatsdateinamen sind die Zertifikatfingerabdrücke. Der folgende C#-Code zeigt, wie ein öffentliches Zertifikat in eine Linux-App geladen wird.

```csharp
using System;
using System.Security.Cryptography.X509Certificates;

...
var bytes = System.IO.File.ReadAllBytes("/var/ssl/certs/<thumbprint>.der");
var cert = new X509Certificate2(bytes);

// Use the loaded certificate
```

Informationen zum Laden eines SSL-Zertifikats aus einer Datei in Node.js, PHP, Python, Java oder Ruby finden Sie in der Dokumentation für die jeweilige Sprache oder Webplattform.

## <a name="load-certificate-from-file"></a>Laden des Zertifikats aus einer Datei

Wenn Sie eine manuell hochgeladene Zertifikatsdatei laden müssen, sollte der Upload eher über [FTPS](deploy-ftp.md) statt beispielsweise über [Git](deploy-local-git.md) erfolgen. Halten Sie vertrauliche Daten wie beispielsweise ein privates Zertifikat aus der Quellcodeverwaltung heraus.

> [!NOTE]
> ASP.NET und ASP.NET Core unter Windows müssen auf den Zertifikatspeicher auch dann zugreifen, wenn Sie ein Zertifikat aus einer Datei laden. Laden Sie zum Laden einer Zertifikatsdatei in eine Windows .NET-App das aktuelle Benutzerprofil mit dem folgenden Befehl in <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>:
>
> ```azurecli-interactive
> az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_LOAD_USER_PROFILE=1
> ```

Im folgenden C#-Beispiel wird ein öffentliches Zertifikat aus einem relativen Pfad in die App geladen:

```csharp
using System;
using System.Security.Cryptography.X509Certificates;

...
var bytes = System.IO.File.ReadAllBytes("~/<relative-path-to-cert-file>");
var cert = new X509Certificate2(bytes);

// Use the loaded certificate
```

Informationen zum Laden eines SSL-Zertifikats aus einer Datei in Node.js, PHP, Python, Java oder Ruby finden Sie in der Dokumentation für die jeweilige Sprache oder Webplattform.

## <a name="more-resources"></a>Weitere Ressourcen

* [Schützen eines benutzerdefinierten DNS-Namens mit einer SSL-Bindung](configure-ssl-bindings.md)
* [Erzwingen von HTTPS](configure-ssl-bindings.md#enforce-https)
* [Erzwingen von TLS 1.1/1.2](configure-ssl-bindings.md#enforce-tls-versions)
* [Häufig gestellte Fragen: App Service-Zertifikate](https://docs.microsoft.com/azure/app-service/faq-configuration-and-management/)

---
title: 'Schützen eines benutzerdefinierten DNS-Namens mit einer SSL-Bindung: Azure App Service | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie ein App Service-Zertifikat erwerben und es an Ihre App Service-App binden.
services: app-service
author: cephalin
manager: gwallace
tags: buy-ssl-certificates
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 10/25/2019
ms.author: cephalin
ms.reviewer: yutlin
ms.custom: seodec18
ms.openlocfilehash: 259a4d33ba6e8c072f8df906da4784119b299822
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73509054"
---
# <a name="secure-a-custom-dns-name-with-an-ssl-binding-in-azure-app-service"></a>Schützen eines benutzerdefinierten DNS-Namens mit einer SSL-Bindung: Azure App Service

In diesem Artikel erfahren Sie, wie Sie durch die Erstellung einer Zertifikatsbindung die [benutzerdefinierte Domäne](app-service-web-tutorial-custom-domain.md) in Ihrer [App Service-App](https://docs.microsoft.com/azure/app-service/) oder [Funktions-App](https://docs.microsoft.com/azure/azure-functions/) schützen. Wenn Sie fertig sind, können Sie am Endpunkt `https://` für Ihren benutzerdefinierten DNS-Namen (z. B. `https://www.contoso.com`) auf Ihre App Service-App zugreifen. 

![Web-App mit benutzerdefiniertem SSL-Zertifikat](./media/configure-ssl-bindings/app-with-custom-ssl.png)

Das Schützen einer [benutzerdefinierten Domäne](app-service-web-tutorial-custom-domain.md) mit einem Zertifikat umfasst zwei Schritte:

- [Hinzufügen eines privaten Zertifikats zu App Service](configure-ssl-certificate.md), das alle [Anforderungen für SSL-Bindungen](configure-ssl-certificate.md#private-certificate-requirements) erfüllt
-  Erstellen einer SSL-Bindung für die entsprechende benutzerdefinierte Domäne. Dieser zweite Schritt wird in diesem Artikel behandelt.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Aktualisieren des Tarifs für Ihre App
> * Schützen einer benutzerdefinierten Domäne mit einem Zertifikat
> * Erzwingen von HTTPS
> * Erzwingen von TLS 1.1/1.2
> * Automatisieren der TLS-Verwaltung mit Skripts

## <a name="prerequisites"></a>Voraussetzungen

Im Rahmen dieser Schrittanleitung müssen Sie folgende Schritte durchführen:

- [Erstellen einer App Service-App](/azure/app-service/)
- [Zuordnen eines Domänennamens zu Ihrer App](app-service-web-tutorial-custom-domain.md) oder [Kaufen und Konfigurieren eines Domänennamens in Azure](manage-custom-dns-buy-domain.md)
- [Hinzufügen eines privaten Zertifikats zu Ihrer App](configure-ssl-certificate.md)

> [!NOTE]
> Die einfachste Möglichkeit zum Hinzufügen eines privaten Zertifikats besteht darin, [ein von App Service verwaltetes kostenloses Zertifikat zu erstellen](configure-ssl-certificate.md#create-a-free-certificate-preview) (Vorschau).

[!INCLUDE [Prepare your web app](../../includes/app-service-ssl-prepare-app.md)]

<a name="upload"></a>

## <a name="secure-a-custom-domain"></a>Schützen einer benutzerdefinierten Domäne

Führen Sie folgende Schritte aus:

Wählen Sie im linken Menü des <a href="https://portal.azure.com" target="_blank">Azure-Portals</a> die Optionen **App Services** >  **\<App-Name>** aus.

Öffnen Sie im linken Navigationsbereich Ihrer App das Dialogfeld **TLS-/SSL-Bindung**, indem Sie folgende Schritte ausführen:

- Auswählen von **Benutzerdefinierte Domänen** > **Bindung hinzufügen**
- Auswählen von **TLS-/SSL-Einstellungen** > **TLS-/SSL-Bindung hinzufügen**

![Hinzufügen der Bindung zur Domäne](./media/configure-ssl-bindings/secure-domain-launch.png)

Wählen Sie unter **Benutzerdefinierte Domäne** die benutzerdefinierte Domäne aus, für die Sie eine Bindung hinzufügen möchten.

Wenn Ihre App bereits über ein Zertifikat für die ausgewählte benutzerdefinierte Domäne verfügt, navigieren Sie direkt zu [Bindung erstellen](#create-binding). Gehen Sie andernfalls wie im Anschluss beschrieben vor.

### <a name="add-a-certificate-for-custom-domain"></a>Hinzufügen eines Zertifikats für eine benutzerdefinierte Domäne

Verfügt Ihre App über kein Zertifikat für die ausgewählte benutzerdefinierte Domäne, haben Sie zwei Optionen:

- **Hochladen eines PFX-Zertifikats:** Führen Sie den Workflow unter [Hochladen eines privaten Zertifikats](configure-ssl-certificate.md#upload-a-private-certificate) aus, und wählen Sie dann diese Option hier aus.
- **Importieren eines App Service-Zertifikats:** Führen Sie den Workflow unter [Importieren eines App Service-Zertifikats](configure-ssl-certificate.md#import-an-app-service-certificate) aus, und wählen Sie dann diese Option hier aus.

> [!NOTE]
> Sie können auch ein [kostenloses Zertifikat erstellen](configure-ssl-certificate.md#create-a-free-certificate-preview) (Vorschau) oder [ein Key Vault-Zertifikat importieren](configure-ssl-certificate.md#import-a-certificate-from-key-vault). Sie müssen diesen Schritt jedoch separat ausführen und anschließend zum Dialogfeld **TLS-/SSL-Bindung** zurückkehren.

### <a name="create-binding"></a>Erstellen einer Bindung

Konfigurieren Sie mithilfe der folgende Tabelle die SSL-Bindung im Dialogfeld **TLS-/SSL-Bindung**, und klicken Sie dann auf **Bindung hinzufügen**.

| Einstellung | BESCHREIBUNG |
|-|-|
| Benutzerdefinierte Domäne | Der Domänenname, für den die SSL-Bindung hinzugefügt werden soll |
| Fingerabdruck des privaten Zertifikats | Das zu bindende Zertifikat. |
| TLS-/SSL-Typ | <ul><li>**[SNI SSL:](https://en.wikipedia.org/wiki/Server_Name_Indication)** Es können mehrere SNI-SSL-Bindungen hinzugefügt werden. Bei dieser Option können mehrere zur selben IP-Adresse zugehörige Domänen durch mehrere SSL-Zertifikate geschützt werden. Die meisten modernen Browser (einschließlich Internet Explorer, Chrome, Firefox und Opera) unterstützen SNI (ausführlichere Informationen finden Sie unter [Servernamensanzeige](https://wikipedia.org/wiki/Server_Name_Indication)).</li><li>**IP-SSL:** Unter Umständen kann nur eine IP-SSL-Bindung hinzugefügt werden. Bei dieser Option kann eine dedizierte öffentliche IP-Adresse nur durch ein SSL-Zertifikat geschützt werden. Führen Sie nach dem Konfigurieren der Bindung die Schritte unter [Neuzuordnen eines Datensatzes für IP-SSL](#remap-a-record-for-ip-ssl) durch.<br/>IP-SSL wird nur in Produktionstarifen oder im Tarif „Isoliert“ unterstützt. </li></ul> |

Nach Abschluss des Vorgangs wird der SSL-Status der benutzerdefinierten Domäne in **Sicher** geändert.

![SSL-Bindung erfolgreich](./media/configure-ssl-bindings/secure-domain-finished.png)

> [!NOTE]
> Der Zustand **Sicher** unter **Benutzerdefinierte Domänen** bedeutet, dass die Domäne durch ein Zertifikat geschützt ist. App Service überprüft jedoch nicht, ob das Zertifikat beispielsweise selbstsigniert oder abgelaufen ist, was ebenfalls dazu führen kann, dass ein Browser einen Fehler oder eine Warnung anzeigt.

## <a name="remap-a-record-for-ip-ssl"></a>Neuzuordnen eines A-Eintrags für IP-SSL

Wenn Sie nicht IP-SSL in Ihrer App verwenden, fahren Sie mit [Testen von HTTPS](#test-https) fort.

Standardmäßig verwendet Ihre App eine freigegebene öffentliche IP-Adresse. Wenn Sie ein Zertifikat mit IP-SSL binden, erstellt App Service eine neue, dedizierte IP-Adresse für Ihre App.

Wenn Sie Ihrer App einen A-Eintrag zugeordnet haben, aktualisieren Sie Ihre Domänenregistrierung mit dieser neuen, dedizierten IP-Adresse.

Die Seite **Benutzerdefinierte Domäne** Ihrer App wird mit der neuen, dedizierten IP-Adresse aktualisiert. [Kopieren Sie diese IP-Adresse](app-service-web-tutorial-custom-domain.md#info) und [ordnen Sie dieser neuen IP-Adresse dann den A-Eintrag neu zu](app-service-web-tutorial-custom-domain.md#map-an-a-record).

## <a name="test-https"></a>Testen von HTTPS

Rufen Sie in verschiedenen Browsern `https://<your.custom.domain>` auf, um zu überprüfen, ob Ihre App angeboten wird.

![Portalnavigation zur Azure-App](./media/configure-ssl-bindings/app-with-custom-ssl.png)

> [!NOTE]
> Wenn Ihre App Zertifikatüberprüfungsfehler meldet, verwenden Sie wahrscheinlich ein selbstsigniertes Zertifikat.
>
> Wenn dies nicht der Fall ist, haben Sie beim Exportieren des Zertifikats als PFX-Datei eventuell Zwischenzertifikate ausgelassen.

## <a name="prevent-ip-changes"></a>Verhindern von IP-Änderungen

Ihre IP-Adresse für eingehenden Datenverkehr kann sich ändern, wenn Sie eine Bindung löschen, auch wenn es sich um eine IP-SSL-Bindung handelt. Dies ist besonders wichtig, wenn Sie ein Zertifikat verlängern, das sich bereits in einer IP-SSL-Bindung befindet. Um eine Änderung der IP-Adresse in Ihrer App zu vermeiden, führen Sie diese Schritte in angegebener Reihenfolge aus:

1. Laden Sie das neue Zertifikat hoch.
2. Binden Sie das neue Zertifikat an die gewünschte benutzerdefinierte Domäne, ohne die alte zu löschen. Dadurch wird die Bindung ersetzt, anstatt die alte Bindung zu entfernen.
3. Löschen Sie das alte Zertifikat. 

## <a name="enforce-https"></a>Erzwingen von HTTPS

Standardmäßig können Benutzer weiterhin mit HTTP auf Ihre App zugreifen. Sie können alle HTTP-Anforderungen an den HTTPS-Port umleiten.

Wählen Sie im linken Navigationsbereich der App-Seite **SSL-Einstellungen** aus. Wählen Sie anschließend für **Nur HTTPS** die Option **Ein**.

![Erzwingen von HTTPS](./media/configure-ssl-bindings/enforce-https.png)

Wenn der Vorgang abgeschlossen ist, navigieren Sie zu einer beliebigen HTTP-URL, die auf Ihre App verweist. Beispiel:

- `http://<app_name>.azurewebsites.net`
- `http://contoso.com`
- `http://www.contoso.com`

## <a name="enforce-tls-versions"></a>Erzwingen von TLS-Versionen

Ihre App lässt standardmäßig [TLS](https://wikipedia.org/wiki/Transport_Layer_Security) 1.2 zu. Diese TLS-Version wird von Branchenstandards wie [PCI-DSS](https://wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard) empfohlen. Andere TLS-Versionen können wie folgt erzwungen werden:

Wählen Sie im linken Navigationsbereich der App-Seite **SSL-Einstellungen** aus. Wählen Sie dann unter **TLS-Version** die gewünschte TLS-Mindestversion aus. Diese Einstellung steuert nur eingehende Aufrufe. 

![Erzwingen von TLS 1.1 oder 1.2](./media/configure-ssl-bindings/enforce-tls1-2.png)

Nach Abschluss des Vorgangs lehnt Ihre App alle Verbindungen mit niedrigerer TLS-Version ab.

## <a name="automate-with-scripts"></a>Automatisieren mit Skripts

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

[!code-azurecli[main](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")] 

### <a name="powershell"></a>PowerShell

[!code-powershell[main](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate to a web app")]

## <a name="more-resources"></a>Weitere Ressourcen

* [Verwenden eines SSL-Zertifikats in Ihrem Anwendungscode](configure-ssl-certificate-in-code.md)
* [Häufig gestellte Fragen: App Service-Zertifikate](https://docs.microsoft.com/azure/app-service/faq-configuration-and-management/)

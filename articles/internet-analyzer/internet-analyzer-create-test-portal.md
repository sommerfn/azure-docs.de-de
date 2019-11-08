---
title: Erstellen eines Internet Analyzer-Tests über das Portal | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie Ihren ersten Internet Analyzer-Test erstellen.
services: internet-analyzer
author: megan-beatty
ms.service: internet-analyzer
ms.topic: tutorial
ms.date: 10/16/2019
ms.author: mebeatty
ms.openlocfilehash: 64bbed9b558d4c20889b28a5247e2113d20daa77
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73509834"
---
# <a name="create-an-internet-analyzer-test-using-portal-preview"></a>Erstellen eines Internet Analyzer-Tests über das Portal (Vorschau)

Es gibt zwei Möglichkeiten zum Erstellen einer Internet Analyzer-Ressource: über das Azure-Portal oder mithilfe der [CLI](internet-analyzer-cli.md). In diesem Abschnitt erfahren Sie, wie Sie eine neue Azure Internet Analyzer-Ressource mit unserem Portal erstellen.

> [!IMPORTANT]
> Diese öffentliche Vorschauversion wird ohne Servicelevelvereinbarung bereitgestellt und sollte nicht für Produktionsworkloads verwendet werden. Unter Umständen werden bestimmte Features nicht unterstützt, verfügen über eingeschränkte Funktionen und sind nicht an allen Azure-Standorten verfügbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Voraussetzungen

Die öffentliche Vorschau ist für die globale Verwendung verfügbar. Allerdings ist der Datenspeicher in der Vorschauphase auf *USA, Westen 2* beschränkt.

## <a name="basics"></a>Grundlagen

1. Befolgen Sie die Anweisungen im Abschnitt **Wie nehme ich an der Vorschauversion teil?** der [häufig gestellten Fragen zu Azure Internet Analyzer](internet-analyzer-faq.md), um Zugriff auf die Vorschauversion von Internet Analyzer zu erhalten.
2. Klicken Sie auf der Startseite im [Azure-Portal](https://preview.portal.azure.com) auf **+ Ressource erstellen**. Internet Analyzer ist zurzeit nur über die Vorschauversion des Azure-Portals verfügbar.
3. Suchen Sie auf der Seite **Neu** über das Feld *Marketplace durchsuchen* nach „Internet Analyzer“.
4. Klicken Sie auf **Internet Analyzer (preview)** (Internet Analyzer (Vorschau)). Vergewissern Sie sich, dass der Herausgeber *Microsoft* und die Kategorie *Netzwerk* lautet.
5. Klicken Sie auf der Seite **Internet Analyzer (preview)** (Internet Analyzer (Vorschau)) auf **Erstellen**, um die Seite **Create an Internet Analyzer** (Internet Analyzer-Instanz erstellen) zu öffnen.
6. Geben Sie die folgenden Konfigurationseinstellungen für die Internet Analyzer-Ressource an:

    * **Abonnement:** Das Azure-Abonnement zum Hosten der neuen Internet Analyzer-Ressource. ***Verwenden Sie dieselbe Abonnement-ID wie beim Anfordern des Zugriffs auf die Vorschauversion.***
    * **Ressourcengruppe:** Die Azure-Ressourcengruppe, in der die neue Internet Analyzer-Ressource erstellt wird. Sollte noch keine Ressourcengruppe vorhanden sein, können Sie eine neue Ressourcengruppe erstellen.
    * **Name:** Der Name des neuen Internet Analyzer-Ressourcenprofils
    * **Region:** Die öffentliche Azure-Region, in der die Ressource erstellt wird. Während der Vorschauphase ist nur *USA, Westen 2* verfügbar.

7. Klicken Sie nach Angabe der Profileinstellungen auf **Überprüfen + erstellen**.

## <a name="configuration"></a>Konfiguration

Die Ausführung der grundlegenden Schritte ist Voraussetzung für das Konfigurieren eines Tests und das Einbetten des JavaScript-Clients. Navigieren Sie nach dem Erstellen eines Profils zu **Einstellungen > Konfiguration**, um Ihren ersten Test zu konfigurieren.

1. Geben Sie im Feld **Testname** einen Namen für den Test ein.
2. Fügen Sie im Feld **Beschreibung** eine Beschreibung für den Test hinzu.
3. Klicken Sie auf **Endpunkt konfigurieren**. Auf der rechten Seite wird eine Registerkarte angezeigt. Wählen Sie den Typ des Endpunkts aus, den Sie konfigurieren möchten: eine einzelne Azure-Region, mehrere Azure-Regionen oder einen benutzerdefinierten Endpunkt.

    >
    ***Vorkonfigurierte Endpunkte: Kombinationen aus einzelnen und mehreren Azure-Regionen***
    * Wählen Sie eine Region oder mehrere Regionen aus einer [vorkonfigurierten Liste von Azure-Endpunkten](internet-analyzer-faq.md) aus.
    * Wählen Sie als Nächstes den Typ der Anwendungs- oder Inhaltsbereitstellungsarchitektur aus, die Sie auswerten möchten.
        * Einzelne Azure-Region: Websitebeschleunigung ([Azure Front Door](https://azure.microsoft.com/services/frontdoor/)), statisches Content Caching ([Azure CDN für Microsoft](https://azure.microsoft.com/services/cdn/)) oder keine Angabe
        * Mehrere Azure-Regionen: Websitebeschleunigung ([Azure Front Door](https://azure.microsoft.com/services/frontdoor/)), DNS-Steuerung ([Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/))  

    ***Benutzerdefinierte Endpunkte***
    * Befolgen Sie die Anweisungen auf der Seite [Create Custom Endpoint](internet-analyzer-custom-endpoint.md) (Benutzerdefinierten Endpunkt erstellen).
    * Fügen Sie den HTTPS-URL-Speicherort des 1 Pixel großen Bilds im Portal hinzu.
    >

4. Klicken Sie auf **Hinzufügen**, um den Endpunkt zum Test hinzuzufügen.
5. Wiederholen Sie die Schritte 1 bis 4, um den zweiten Endpunkt zu konfigurieren. Endpunkt B wird immer relativ zu Endpunkt A gemessen: Berücksichtigen Sie bei der Konfiguration von Endpunkten, welcher Endpunkt als Testelement verwendet werden soll.
6. Klicken Sie auf die Schaltfläche **Speichern**, um den Test zu speichern. Nachdem Sie einen Test gespeichert haben, können Sie die Endpunkte eines bestimmten Tests nicht mehr bearbeiten.
7. Wählen Sie die Tests aus, die Sie starten möchten, und klicken Sie auf **Test starten**. Dadurch wird der ***Zustand*** der Tests in ***Wird ausgeführt*** geändert. Sie können Tests jederzeit starten. Der JavaScript-Client muss jedoch eingebettet werden, damit der Test mit der Erfassung von Messungen beginnt.
8. Fügen Sie jederzeit weitere Tests hinzu. Beachten Sie, dass der eindeutige JavaScript-Client erst generiert wird, wenn ein Test erstellt wurde.

## <a name="embed-client"></a>Einbetten des Clients

Der JavaScript-Client muss in Ihre Webanwendung eingebettet werden, damit Sie einen Test starten können. Nachdem Sie mindestens einen Test konfiguriert haben, klicken Sie auf **Überprüfen + erstellen**, navigieren Sie zu **Einstellungen > Konfiguration**, und kopieren Sie den JavaScript-Client. Genaue Anweisungen finden Sie auf der Seite [Einbetten des Internet Analyzer-Clients](internet-analyzer-embed-client.md).  

## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie die [häufig gestellten Fragen zu Internet Analyzer](internet-analyzer-faq.md).
* Informieren Sie sich ausführlicher über das Einbetten des [Internet Analyzer-Clients](internet-analyzer-embed-client.md) und das Erstellen eines [benutzerdefinierten Endpunkts](internet-analyzer-custom-endpoint.md).

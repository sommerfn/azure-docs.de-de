---
title: Häufig gestellte Fragen zu Azure Media Services v3 | Microsoft-Dokumentation
description: Dieser Artikel enthält Antworten auf häufig gestellte Fragen zu Azure Media Services v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 06/21/2019
ms.author: juliako
ms.openlocfilehash: 28b9c8f343437c20e277d2f3ba53767afa45a5c2
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2019
ms.locfileid: "68501251"
---
# <a name="media-services-v3-frequently-asked-questions"></a>Häufig gestellte Fragen zu Media Services v3

Dieser Artikel enthält Antworten auf häufig gestellte Fragen zu Azure Media Services (AMS) v3.

## <a name="general"></a>Allgemein

### <a name="what-azure-roles-can-perform-actions-on-azure-media-services-resources"></a>Welche Azure-Rollen können Aktionen mit den Ressourcen von Azure Media Services durchführen? 

Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung (RBAC) für Media Services-Konten](rbac-overview.md).

### <a name="how-do-i-configure-media-reserved-units"></a>Wie lassen sich reservierte Einheiten für Medien konfigurieren?

Für die Audioanalyse- und Videoanalyseaufträge, die von Media Services v3 oder Video Indexer ausgelöst werden, wird dringend empfohlen, Ihr Konto mit zehn S3-MRUs bereitzustellen. Erstellen Sie im [Azure-Portal](https://portal.azure.com/) ein Supportticket, falls Sie mehr als zehn S3-MRUs benötigen.

Details dazu finden Sie unter [Skalieren der Medienverarbeitung an der Befehlszeilenschnittstelle](media-reserved-units-cli-how-to.md).

### <a name="what-is-the-recommended-method-to-process-videos"></a>Was ist die empfohlene Methode zum Verarbeiten von Videos?

Verwenden Sie [Transformationen](https://docs.microsoft.com/rest/api/media/transforms), um allgemeine Aufgaben zur Codierung oder Analyse von Videos zu konfigurieren. Jede **Transformation** beschreibt eine Vorgehensweise oder einen Workflow von Aufgaben zur Verarbeitung Ihrer Video- oder Audiodateien. Ein [Auftrag](https://docs.microsoft.com/rest/api/media/jobs) ist die eigentliche Anforderung an Media Services, die erstellte **Transformation** auf ein bestimmtes Eingabevideo oder auf einen Audioinhalt anzuwenden. Nachdem die Transformation erstellt wurde, können Sie mithilfe von Media Services-APIs oder der veröffentlichten SDKs Aufträge übermitteln. Weitere Informationen finden Sie unter [Transformationen und Aufträge](transforms-jobs-concept.md).

### <a name="how-does-pagination-work"></a>Wie funktioniert die Paginierung?

Bei Verwendung der Paginierung sollten Sie immer den Link „Weiter“ verwenden, um die Sammlung zu enumerieren, und sich nicht auf eine bestimmte Seitengröße verlassen. Weitere Informationen und Beispiele finden Sie unter [Filterung, Sortierung, Paginierung](entities-overview.md).

### <a name="what-features-are-not-yet-available-in-azure-media-services-v3"></a>Welche Features sind noch nicht in Azure Media Services v3 verfügbar?

Details finden Sie unter [Featurelücken in Bezug auf v2-APIs](migrate-from-v2-to-v3.md#feature-gaps-with-respect-to-v2-apis).

### <a name="what-is-the-process-of-moving-a-media-services-account-between-subscriptions"></a>Wie wird ein Media Services-Konto zwischen Abonnements verschoben?  

Informationen dazu finden Sie unter [Verschieben eines Media Services-Kontos zwischen Abonnements](media-services-account-concept.md).

## <a name="live-streaming"></a>Livestreaming 

###  <a name="how-to-insert-breaksvideos-and-image-slates-during-live-stream"></a>Wie werden Unterbrechungen/Videos und Bildtafeln während des Livedatenstroms eingefügt?

Die Livecodierung von Media Services v3 unterstützt aktuell das Einfügen von Video oder Bildtafeln in Livedatenströme noch nicht. 

Sie können einen [lokalen Liveencoder](recommended-on-premises-live-encoders.md) verwenden, um das Quellvideo zu wechseln. Viele Apps bieten die Möglichkeit zum Wechseln der Quelle, darunter Telestream Wirecast, Switcher Studio (unter iOS), OBS Studio (kostenlose App) und viele weitere.

## <a name="content-protection"></a>Content Protection

### <a name="should-i-use-an-aes-128-clear-key-encryption-or-a-drm-system"></a>Sollte ich eine Verschlüsselung mit unverschlüsseltem AES-128-Schlüssel oder ein DRM-System verwenden?

Kunden fragen sich oft, ob sie die AES-Verschlüsselung oder ein DRM-System verwenden sollten. Der Hauptunterschied zwischen den beiden Systemen besteht darin, dass bei der AES-Verschlüsselung der Inhaltsschlüssel während der Übertragung ohne zusätzliche Verschlüsselung (als Klartext) an den Client übertragen wird. So kann der Schlüssel zur Entschlüsselung des Inhalts für den Clientplayer zugänglich gemacht und in einer Netzwerkablaufverfolgung auf dem Client als unverschlüsselter Text angezeigt werden. Eine Verschlüsselung mit einem unverschlüsselten AES-128-Schlüssel eignet sich für Anwendungsfälle, in denen der Betrachter vertrauenswürdig ist (z.B. bei Unternehmensvideos, die innerhalb eines Unternehmens verteilt und von Mitarbeitern angesehen werden).

DRM-Systeme wie PlayReady, Widevine und FairPlay bieten eine zusätzliche Verschlüsselungsebene für den Schlüssel, der zum Entschlüsseln des Inhalts verwendet wird (im Vergleich zu einem unverschlüsselten AES-128-Schlüssel). Der Inhaltsschlüssel wird zusätzlich zur Verschlüsselung auf Übertragungsebene durch TLS in einen durch die DRM-Runtime geschützten Schlüssel verschlüsselt. Zusätzlich erfolgt die Entschlüsselung in einer sicheren Umgebung auf Betriebssystemebene, wo ein Angriff durch einen böswilligen Benutzer schwieriger auszuführen ist. DRM wird für Anwendungsfälle empfohlen, in denen der Betrachter möglicherweise nicht vertrauenswürdig ist und Sie ein Höchstmaß an Sicherheit benötigen.

### <a name="how-and-where-to-get-jwt-token-before-using-it-to-request-license-or-key"></a>Wie und wo kann ich JWT-Token abrufen, um damit dann eine Lizenz oder einen Schlüssel anzufordern?

1. Für die Produktion benötigen Sie einen Sicherheitstokendienst (STS) (Webdienst), der bei einer HTTPS-Anforderung JWT-Token ausgibt. Zum Testen können Sie den in der Methode **GetTokenAsync** angegebenen Code verwenden, der in [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs) definiert ist.
2. Player müssen nach der Authentifizierung eines Benutzers beim STS ein solches Token anfordern und dieses als Wert des Token zuweisen. Sie können die [Azure Media Player-API](https://amp.azure.net/libs/amp/latest/docs/) verwenden.

* Ein Beispiel für die Ausführung des STS mit einem symmetrischen und einem asymmetrischen Schlüssel finden Sie unter [https://aka.ms/jwt](https://aka.ms/jwt). 
* Ein Beispiel für einen Player, der auf dem Azure Media Player basiert und ein solches JWT-Token verwendet, finden Sie unter [https://aka.ms/amtest](https://aka.ms/amtest) (erweitern Sie den Link „player_settings“, um die Tokeneingabe anzusehen).

### <a name="how-do-you-authorize-requests-to-stream-videos-with-aes-encryption"></a>Wie autorisiere ich Anforderungen zum Streamen von Videos mit der AES-Verschlüsselung?

Der richtige Ansatz ist die Nutzung von STS:

Fügen Sie in STS je nach Benutzerprofil unterschiedliche Ansprüche hinzu (z.B. „Premium-Benutzer“, „Basic-Benutzer“, „Benutzer der kostenlosen Testversion“). Bei unterschiedlichen Ansprüchen in einem JWT kann der Benutzer unterschiedliche Inhalte sehen. Selbstverständlich hat ContentKeyPolicyRestriction für unterschiedliche Inhalte/Objekte die entsprechenden RequiredClaims.

Verwenden Sie die Azure Media Services-APIs für die Konfiguration von Lizenzen/Schlüsselbereitstellungen und die Verschlüsselung Ihrer Objekte (siehe [dieses Beispiel](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES/Program.cs)).

Weitere Informationen finden Sie unter

- [Übersicht über den Inhaltsschutz](content-protection-overview.md)
- [Entwurf eines Multi-DRM-Inhaltsschutzsystems mit Zugriffssteuerung](design-multi-drm-system-with-access-control.md)

### <a name="http-or-https"></a>HTTP oder HTTPS?
Die ASP.NET MVC-Playeranwendung muss Folgendes unterstützen:

* Die Benutzerauthentifizierung in Azure AD, die über HTTPS erfolgen muss.
* Den JWT-Austausch zwischen Client und Azure AD, der über HTTPS erfolgen muss.
* Den DRM-Lizenzerwerb durch den Client, der über HTTPS erfolgen muss, wenn Lizenzen von Media Services bereitgestellt werden. Bei der PlayReady-Produktsuite ist HTTPS für die Lizenzbereitstellung nicht zwingend vorgegeben. Wenn Ihr PlayReady-Lizenzserver sich außerhalb von Media Services befindet, können Sie HTTP oder HTTPS verwenden.

Die ASP.NET-Playeranwendung verwendet HTTPS als Best Practice, daher befindet der Media Player sich auf einer HTTPS-Seite. Für das Streaming wird jedoch HTTP bevorzugt, daher müssen Sie das Problem gemischter Inhalte berücksichtigen.

* Der Browser lässt keine gemischten Inhalte zu. Plug-Ins wie Silverlight und OSMF für Smooth Streaming und DASH lassen diese jedoch zu. Gemischte Inhalte sind ein Sicherheitsproblem, da sie die Möglichkeit eröffnen, dass schadhafter JavaScript-Code eingeschleust wird, wodurch Kundendaten gefährdet werden können. Browser blockieren diese Funktion standardmäßig. Die einzige Möglichkeit, dieses Problem zu begehen, besteht darin, auf Server- bzw. Ursprungsseite alle Domänen zuzulassen (egal ob HTTPS oder HTTP). Dies ist aber wahrscheinlich auch nicht sehr sinnvoll.
* Vermeiden Sie gemischte Inhalte. Sowohl die Playeranwendung als auch Media Player sollten HTTP oder HTTPS verwenden. Wenn Sie gemischte Inhalte wiedergeben, erfordert die Smooth Streaming-Technologie von Silverlight das Deaktivieren einer Warnung zu gemischten Inhalten. Die Smooth Streaming-Technologie von Adobe Flash verarbeitet gemischte Inhalte hingegen ohne entsprechende Warnung.
* Wenn Ihr Streamingendpunkt vor August 2014 erstellt wurde, wird HTTPS nicht unterstützt. Erstellen und verwenden Sie in diesem Fall einen neuen Streamingendpunkt für HTTPS.

### <a name="what-about-live-streaming"></a>Was gilt für Livestreaming?

Sie können genau den gleichen Entwurf und die gleiche Implementierung zum Schutz von Livestreams in Media Services verwenden, indem Sie das Medienobjekt, das einem Programm zugeordnet ist, als VOD-Medienobjekt (Video On Demand) behandeln. Um Live-Inhalte mit mehreren DRM-Systemen zu schützen, wenden Sie dieselben Einrichtungs- und Verarbeitungsschritte wie bei einem VOD-Medienobjekt an, bevor Sie das Medienobjekt der Liveausgabe zuordnen.

### <a name="what-about-license-servers-outside-media-services"></a>Was gilt für Lizenzserver außerhalb von Media Services?

In vielen Fällen haben Kunden in eine Lizenzserverfarm investiert, die sich entweder in ihrem eigenen Rechenzentrum befindet oder von DRM-Dienstanbietern gehostet wird. Mit dem Inhaltsschutz von Media Services können Sie im Hybridmodus arbeiten. Inhalte können in Media Services gehostet und dynamisch geschützt werden, während DRM-Lizenzen von Servern außerhalb von Media Services bereitgestellt werden. Berücksichtigen Sie in diesem Fall die folgenden Änderungen:

* Der Sicherheitstokendienst (Security Token Service, STS) muss Token ausstellen, die zulässig sind und von der Lizenzserverfarm überprüft werden können. Beispielsweise erfordern die von Axinom bereitgestellten Widevine-Lizenzserver ein spezifisches JWT, das eine Berechtigungsnachricht enthält. Aus diesem Grund benötigen Sie einen Sicherheitstokendienst, um ein solches JWT auszustellen. 
* Sie müssen den Lizenzbereitstellungsdienst nicht mehr in Media Services konfigurieren. Sie müssen die Lizenzerwerbs-URLs (für PlayReady, Widevine und FairPlay) bei der Konfiguration von „ContentKeyPolicies“ angeben.

## <a name="media-services-v2-vs-v3"></a>Media Services v2 i. Vgl. mit v3 

### <a name="can-i-use-the-azure-portal-to-manage-v3-resources"></a>Kann ich v3-Ressourcen im Azure-Portal verwalten?

Derzeit können Sie das Azure-Portal nicht für die Verwaltung von v3-Ressourcen verwenden. Verwenden Sie die [REST-API](https://aka.ms/ams-v3-rest-ref), die [Befehlszeilenschnittstelle](https://aka.ms/ams-v3-cli-ref) oder eines der unterstützten [SDKs](media-services-apis-overview.md#sdks).

### <a name="is-there-an-assetfile-concept-in-v3"></a>Gibt es in v3 ein AssetFile-Konzept?

„AssetFile“ wurde aus der AMS-API entfernt. So ist Media Services nicht länger vom Storage SDK abhängig. Ab jetzt sind die Informationen, die zu Azure Storage gehören, in Storage enthalten und nicht mehr in Media Services. 

Weitere Informationen finden Sie unter [Migration zu Media Services v3](migrate-from-v2-to-v3.md).

### <a name="where-did-client-side-storage-encryption-go"></a>Wo ist die clientseitige Speicherverschlüsselung geblieben?

Aktuell wird empfohlen, die serverseitige Speicherverschlüsselung (die standardmäßig aktiviert ist) zu verwenden. Weitere Informationen finden Sie unter [Azure Storage Service Encryption für ruhende Daten](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

## <a name="next-steps"></a>Nächste Schritte

[Media Services v3 – Übersicht](media-services-overview.md)

---
title: 'Herstellen einer Verbindung mit Windows Virtual Desktop unter iOS: Azure'
description: Informationen zum Herstellen einer Verbindung mit Windows Virtual Desktop mithilfe des iOS-Clients.
services: virtual-desktop
author: heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: helohr
ms.openlocfilehash: bfc7efa6e8ead3b53704e3c9bd189b18cb787618
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73605835"
---
# <a name="connect-with-the-ios-client"></a>Herstellen einer Verbindung mit dem iOS-Client

> Gilt für: iOS 8.0 oder höher. Kompatibel mit iPhone, iPad und iPod Touch.

>[!NOTE]
> Der iOS-Client befindet sich derzeit noch in der Vorschau.

Mit unserem herunterladbaren Client können Sie über Ihr iOS-Gerät auf virtuelle Windows Virtual Desktop-Ressourcen zugreifen. In dieser Anleitung erfahren Sie, wie Sie den iOS-Client einrichten.

## <a name="install-the-ios-beta-client"></a>Installieren der Betaversion des iOS-Clients
So installieren Sie die Betaversion des iOS-Clients:

1. Installieren Sie die App [Apple TestFlight](https://apps.apple.com/us/app/testflight/id899247664) auf Ihrem iOS-Gerät.
2. Öffnen Sie auf Ihrem iOS-Gerät einen Browser, und navigieren Sie zu [aka.ms/rdiosbeta](https://aka.ms/rdiosbeta).
3. Wählen Sie unter der Bezeichnung **Step 2: Join the Beta** die Option **Start Testing** aus.
4. Wenn Sie zur TestFlight-App weitergeleitet werden, wählen Sie **Accept** und dann **Install** aus.

## <a name="subscribe-to-a-feed"></a>Abonnieren eines Feeds

Abonnieren Sie den Feed, der vom Administrator bereitgestellt wird, um die Liste der verwalteten Ressourcen zu erhalten, auf die Sie über Ihr iOS-Gerät zugreifen können.

Abonnieren Sie einen Feed wie folgt:

1. Tippen Sie im Verbindungscenter auf **+** , und tippen Sie dann auf **Arbeitsbereich hinzufügen**.
2. Geben Sie die Feed-URL in das Feld **Feed-URL** ein. Die Feed-URL kann eine URL oder eine E-Mail-Adresse sein.
   - Wenn Sie eine URL verwenden, verwenden Sie die URL, die Ihr Administrator Ihnen mitgeteilt hat. Normalerweise lautet die URL <https://rdweb.wvd.microsoft.com>.
   - Geben Sie Ihre E-Mail-Adresse ein, um E-Mail zu verwenden. Dies weist den Client an, nach einer URL zu suchen, die Ihrer E-Mail-Adresse zugeordnet ist, wenn der Administrator den Server auf diese Weise konfiguriert hat.
3. Tippen Sie auf **Weiter**.
4. Geben Sie Ihre Anmeldeinformationen ein, wenn Sie dazu aufgefordert werden.
   - Geben Sie als **Benutzername** den Benutzernamen mit der Berechtigung für den Zugriff auf Ressourcen an.
   - Geben Sie als **Kennwort** das diesem Benutzernamen zugeordnete Kennwort an.
   - Sie werden möglicherweise auch aufgefordert, zusätzliche Faktoren anzugeben, wenn Ihr Administrator die Authentifizierung auf diese Weise konfiguriert hat.
5. Tippen Sie auf **Speichern**.

Danach sollte das Verbindungscenter die Remoteressourcen anzeigen.

Sobald Sie einen Feed abonniert haben, wird der Inhalt des Feeds in regelmäßigen Abständen automatisch aktualisiert. Ressourcen können basierend auf Änderungen durch Ihren Administrator hinzugefügt, geändert oder entfernt werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwenden des iOS-Betaclients finden Sie in der Dokumentation [Erste Schritte mit dem iOS-Client](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-ios).

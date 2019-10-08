---
title: 'Herstellen einer Verbindung mit Windows Virtual Desktop aus Windows 10 oder Windows 7: Azure'
description: Informationen zum Herstellen einer Verbindung mit Windows Virtual Desktop mithilfe des Windows Desktop-Clients.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: helohr
ms.openlocfilehash: 561dad566d7f1caaf609c8013fa075062e4471ea
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/30/2019
ms.locfileid: "71676746"
---
# <a name="connect-with-the-windows-desktop-client"></a>Herstellen einer Verbindung mit dem Windows-Desktopclient

> Gilt für: Windows 7 und Windows 10

Mit dem Windows Desktop-Client können Sie auf Geräten mit Windows 7 oder Windows 10 auf Windows Virtual Desktop-Ressourcen zugreifen.

> [!IMPORTANT]
> Windows Virtual Desktop unterstützt nicht den RemoteApp-Client und den Client für Desktopverbindungen (RADC). Der Client für Remotedesktopverbindung (MSTSC) wird ebenfalls nicht unterstützt.

## <a name="install-the-windows-desktop-client"></a>Installieren des Windows Desktop-Clients

Dies folgenden Clients können Sie zurzeit herunterladen:

- [Windows (64-Bit)](https://go.microsoft.com/fwlink/?linkid=2068602)

Wir aktualisieren diese Liste, wenn der Client für weitere Versionen von Windows verfügbar wird.

Sie können den Client für den aktuellen Benutzer installieren. Dafür sind keine Administratorrechte erforderlich. Der Administrator kann den Client aber auch so installieren und konfigurieren, dass alle Benutzer auf dem Gerät darauf zugreifen können.

Nach der Installation kann der Client über das Startmenü gestartet werden, indem Sie nach **Remote Desktop** suchen.

## <a name="subscribe-to-a-feed"></a>Abonnieren eines Feeds

Rufen Sie die Liste mit den verfügbaren verwalteten Ressourcen ab, indem Sie den von Ihrem Administrator bereitgestellten Feed abonnieren. Durch das Abonnieren werden die Ressourcen auf Ihrem lokalen PC verfügbar gemacht.

Abonnieren Sie einen Feed wie folgt:

1. Öffnen Sie den Windows Desktop-Client.
2. Wählen Sie auf der Hauptseite **Abonnieren** aus, um eine Verbindung mit dem Dienst herzustellen und Ihre Ressourcen abzurufen.
3. Melden Sie sich mit Ihrem Benutzerkonto an, wenn Sie dazu aufgefordert werden.

Nachdem Sie sich erfolgreich angemeldet haben, wird eine Liste der Ressourcen angezeigt, auf die Sie zugreifen können.

Sie können zwei Methoden nutzen, um Ressourcen zu starten.

- Doppelklicken Sie auf der Hauptseite des Clients auf eine Ressource, um sie zu starten.
- Starten Sie eine Ressource über das Startmenü, wie Sie dies sonst auch für andere Apps tun.
  - Sie können auch in der Suchleiste nach Apps suchen.

Nachdem Sie einen Feed abonniert haben, wird der Inhalt des Feeds in regelmäßigen Abständen automatisch aktualisiert. Ressourcen können basierend auf Änderungen durch Ihren Administrator hinzugefügt, geändert oder entfernt werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung des Windows Desktop-Clients finden Sie unter [Erste Schritte mit dem Windows Desktop-Client](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/windowsdesktop).

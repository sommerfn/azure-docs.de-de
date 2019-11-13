---
title: Herstellen einer Verbindung zu Windows Virtual Desktop in Android – Azure
description: Informationen zum Herstellen einer Verbindung mit Windows Virtual Desktop mithilfe des Android-Clients.
services: virtual-desktop
author: heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 09/26/2019
ms.author: helohr
ms.openlocfilehash: 41b0c1ced9e66bd58d73683865b2c40afc16c5d3
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73605852"
---
# <a name="connect-with-the-android-client"></a>Herstellen einer Verbindung mit dem Android-Client

> Gilt für: Android 4.1 und höher, Chromebooks mit ChromeOS 53 und höher.

>[!NOTE]
> Die Möglichkeit, auf Windows Virtual Desktop-Ressourcen über den Android-Client zuzugreifen, ist zurzeit als Vorschau verfügbar.

Mit unserem herunterladbaren Client können Sie über Ihr Android-Gerät auf Windows Virtual Desktop-Ressourcen zugreifen. Sie können den Android-Client auch auf Chromebook-Geräten verwenden, die Google Play Store unterstützen. In dieser Anleitung erfahren Sie, wie Sie den Android-Client einrichten.

## <a name="install-the-android-client"></a>Installieren des Android-Clients

Beginnen Sie mit dem [Herunterladen](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android) des Clients, und installieren Sie ihn auf Ihrem Android-Gerät.

## <a name="subscribe-to-a-feed"></a>Abonnieren eines Feeds

Abonnieren Sie den Feed, der vom Administrator bereitgestellt wird, um die Liste der verwalteten Ressourcen zu erhalten, auf die Sie über Ihr Android-Gerät zugreifen können.

Abonnieren Sie einen Feed wie folgt:

1. Tippen Sie im Connection Center auf **+** und dann auf **Remote Resource Feed**.
2. Geben Sie die Feed-URL in das Feld **Feed-URL** ein. Die Feed-URL kann eine URL oder eine E-Mail-Adresse sein.
   - Wenn Sie eine URL verwenden, verwenden Sie die URL, die Ihr Administrator Ihnen mitgeteilt hat, normalerweise <https://rdweb.wvd.microsoft.com>.
   - Geben Sie Ihre E-Mail-Adresse ein, um E-Mail zu verwenden. Wenn Ihr Administrator den Server entsprechend konfiguriert hat, sucht der Client nach einer Ihrer E-Mail-Adresse zugeordneten URL.
3. Tippen Sie auf **WEITER**.
4. Geben Sie Ihre Anmeldeinformationen ein, wenn Sie dazu aufgefordert werden.
   - Geben Sie als **Benutzername** den Benutzernamen mit der Berechtigung für den Zugriff auf Ressourcen an.
   - Geben Sie als **Kennwort** das diesem Benutzernamen zugeordnete Kennwort an.
   - Sie werden möglicherweise auch aufgefordert, zusätzliche Faktoren anzugeben, wenn Ihr Administrator die Authentifizierung entsprechend konfiguriert hat.

Danach sollte das Connection Center die Remoteressourcen anzeigen.

Sobald Sie einen Feed abonniert haben, wird der Inhalt des Feeds in regelmäßigen Abständen automatisch aktualisiert. Ressourcen können basierend auf Änderungen durch Ihren Administrator hinzugefügt, geändert oder entfernt werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung des Android-Clients finden Sie unter [Erste Schritte mit dem Android-Client](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-android).

---
title: Überlegungen zur Universellen Windows-Plattform (Microsoft-Authentifizierungsbibliothek für .NET)
titleSuffix: Microsoft identity platform
description: Erfahren Sie mehr über bestimmte Überlegungen zur Verwendung der Universellen Windows-Plattform mit der Microsoft-Authentifizierungsbibliothek für .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/16/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 27a41ad084d21c7623011b5678fb84ed27d68325
ms.sourcegitcommit: be8e2e0a3eb2ad49ed5b996461d4bff7cba8a837
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72802668"
---
# <a name="universal-windows-platform-specific-considerations-with-msalnet"></a>Spezifische Überlegungen zur Universellen Windows-Plattform mit MSAL.NET
Unter UWP gibt es verschiedene Überlegungen, die Sie bei Verwendung von MSAL.NET berücksichtigen müssen.

## <a name="the-usecorporatenetwork-property"></a>UseCorporateNetwork-Eigenschaft
In der WinRT-Plattform hat `PublicClientApplication` die folgende boolesche Eigenschaft ``UseCorporateNetwork``. Durch diese Eigenschaft profitieren Win8.1- und UWP-Anwendungen von der integrierten Windows-Authentifizierung (und daher vom einmaligen Anmelden des im Betriebssystem angemeldeten Benutzers), falls dieser mit einem Konto in einem verbundenen Azure AD-Mandanten angemeldet ist. Wenn Sie diese Eigenschaft festlegen, nutzt MSAL.NET WAB (Web Authentication Broker).

> [!IMPORTANT]
> Wenn diese Eigenschaft auf TRUE festgelegt ist, wird davon ausgegangen, dass der Anwendungsentwickler die integrierte Windows-Authentifizierung (IWA) in der Anwendung aktiviert hat. Hier:
> - Aktivieren Sie in ``Package.appxmanifest`` für Ihre UWP-Anwendung auf der Registerkarte **Funktionen** die folgenden Funktionen:
>   - Unternehmensauthentifizierung
>   - Private Netzwerke (Client und Server)
>   - Freigegebenes Benutzerzertifikat

Die integrierte Windows-Authentifizierung ist standardmäßig nicht aktiviert, da für Anwendungen, welche die Funktion „Unternehmensauthentifizierung“ oder „Freigegebene Benutzerzertifikate“ anfordern, eine höhere Überprüfungsebene erforderlich ist, um im Windows Store akzeptiert zu werden, und gegebenenfalls nicht alle Entwickler die Überprüfung auf der höheren Ebene durchführen möchten.

Die zugrunde liegende Implementierung auf die Universelle Windows-Plattform (WAB) funktioniert nicht ordnungsgemäß in Enterprise-Szenarios, für die der bedingte Zugriff aktiviert wurde. Das Symptom ist, dass der Benutzer versucht, sich mit Windows Hello anzumelden, und es wird vorgeschlagen, ein Zertifikat auszuwählen, aber:

- das Zertifikat für die PIN wurde nicht gefunden,
- oder der Benutzer wählt sie aus, wird jedoch nie zur Eingabe der PIN aufgefordert.

Eine Möglichkeit zur Umgehung dieses Problems besteht darin, eine alternative Methode zu verwenden (Benutzername/Kennwort + Telefonauthentifizierung), jedoch hat sich diese Vorgehensweise nicht bewährt.

## <a name="troubleshooting"></a>Problembehandlung

Einige Kunden haben berichtet, dass in einigen bestimmten Unternehmensumgebungen der folgende Anmeldefehler aufgetreten ist:

```Text
We can't connect to the service you need right now. Check your network connection or try this again later
```

während sie wissen, dass sie über eine Internetverbindung verfügen, und dies mit einem öffentlichen Netzwerk funktioniert.

Eine Problemumgehung ist, sicherzustellen, dass WAB (die zugrunde liegende Windows-Komponente) privates Netzwerk zulässt. Dies können Sie erreichen, indem Sie einen Registrierungsschlüssel festlegen:

```Text
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\authhost.exe\EnablePrivateNetwork = 00000001
```

Weitere Informationen finden Sie unter [Webauthentifizierungsbroker – Fiddler](https://docs.microsoft.com/windows/uwp/security/web-authentication-broker#fiddler).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den folgenden Beispielen:

Beispiel | Plattform | BESCHREIBUNG 
|------ | -------- | -----------|
|[active-directory-dotnet-native-uwp-v2](https://github.com/azure-samples/active-directory-dotnet-native-uwp-v2) | UWP | Eine msal.net verwendende Clientanwendung der Universellen Windows-Plattform für den Zugriff auf Microsoft Graph für eine Benutzerauthentifizierung mit dem Azure AD v2.0-Endpunkt <br>![Topologie](media/msal-net-uwp-considerations/topology-native-uwp.png)|
|[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/Azure-Samples/active-directory-xamarin-native-v2) | Xamarin iOS, Android, UWP | Eine einfache Xamarin Forms-App, mit der die Verwendung von MSAL zum Authentifizieren von MSA und Azure AD über den AAD v2.0-Endpunkt und der Zugriff auf Microsoft Graph mit dem sich daraus ergebenden Token veranschaulicht wird <br>![Topologie](media/msal-net-uwp-considerations/topology-xamarin-native.png)|

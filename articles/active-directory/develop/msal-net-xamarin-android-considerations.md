---
title: Überlegungen zu Xamarin.Android (Microsoft-Authentifizierungsbibliothek für .NET)
titleSuffix: Microsoft identity platform
description: Erfahren Sie mehr über die besonderen Überlegungen zur Verwendung von Xamarin Android mit der Microsoft-Authentifizierungsbibliothek für .NET (MSAL.NET).
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
ms.date: 04/24/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2d6af9753887ffa593a44fba9faa3376066417a8
ms.sourcegitcommit: be8e2e0a3eb2ad49ed5b996461d4bff7cba8a837
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72802648"
---
# <a name="xamarin-android-specific-considerations-with-msalnet"></a>Besondere Überlegungen zu Xamarin.Android mit MSAL.NET
In diesem Artikel erfahren Sie mehr über die besonderen Überlegungen zur Verwendung von Xamarin.Android mit der Microsoft-Authentifizierungsbibliothek für .NET (MSAL.NET).

## <a name="set-the-parent-activity"></a>Festlegen der übergeordneten Aktivität

In Xamarin.Android müssen Sie die übergeordnete Aktivität so festlegen, dass das Token nach der Interaktion zurückgerufen wird.

```csharp
var authResult = AcquireTokenInteractive(scopes)
 .WithParentActivityOrWindow(parentActivity)
 .ExecuteAsync();
```
Sie können dies auch auf der PublicClientApplication-Ebene (in MSAL 4.2 und höher) über einen Rückruf festlegen.

```CSharp
// Requires MSAL.NET 4.2 or above
var pca = PublicClientApplicationBuilder
  .Create("<your-client-id-here>")
  .WithParentActivityOrWindow(() => parentActivity)
  .Build();
```

Es wird empfohlen, das [Current Activity Plugin](https://github.com/jamesmontemagno/CurrentActivityPlugin) zu verwenden.  Anschließend sieht Ihr PublicClientApplication-Buildercode wie folgt aus:

```CSharp
// Requires MSAL.NET 4.2 or above
var pca = PublicClientApplicationBuilder
  .Create("<your-client-id-here>")
  .WithParentActivityOrWindow(() => CrossCurrentActivity.Current)
  .Build();
```


## <a name="ensuring-control-goes-back-to-msal-once-the-interactive-portion-of-the-authentication-flow-ends"></a>Sicherstellen der Rückkehr des Steuerelements zur MSAL-Bibliothek nach Ende des interaktiven Teils des Authentifizierungsablaufs
Unter Android müssen Sie die `OnActivityResult`-Methode von `Activity` außer Kraft setzen und die Methode „SetAuthenticationContinuationEventArgs“ der MSAL-Klasse „AuthenticationContinuationHelper“ aufrufen.

```csharp
protected override void OnActivityResult(int requestCode, 
                                         Result resultCode, Intent data)
{
 base.OnActivityResult(requestCode, resultCode, data);
 AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(requestCode,
                                                                         resultCode,
                                                                         data);
}

```
Durch diese Zeile wird sichergestellt, dass das Steuerelement nach Ende des interaktiven Teils des Authentifizierungsablaufs zur MSAL-Bibliothek zurückkehrt.

## <a name="update-the-android-manifest"></a>Aktualisieren des Android-Manifests
`AndroidManifest.xml` sollte folgende Werte enthalten:
```csharp
<activity android:name="microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="msal{client_id}" android:host="auth" />
         </intent-filter>
</activity>
```

## <a name="use-the-embedded-web-view-optional"></a>Verwenden der eingebetteten Webansicht (optional)

MSAL.NET verwendet standardmäßig den Systemwebbrowser. Dadurch können Sie SSO (Einmaliges Anmelden) bei Webanwendungen und anderen Apps abrufen. In seltenen Fällen kann es erforderlich sein, anzugeben, dass Sie die eingebettete Webansicht verwenden möchten. Weitere Informationen finden Sie unter [Verwendung eines Webbrowsers in MSAL.NET](msal-net-web-browsers.md) und [Android-Systembrowser](msal-net-system-browser-android-considerations.md).

```csharp
bool useEmbeddedWebView = !app.IsSystemWebViewAvailable;

var authResult = AcquireTokenInteractive(scopes)
 .WithParentActivityOrWindow(parentActivity)
 .WithEmbeddedWebView(useEmbeddedWebView)
 .ExecuteAsync();
```

## <a name="troubleshooting"></a>Problembehandlung
Wenn Sie eine neue Xamarin.Forms-Anwendung erstellen und einen Verweis auf das NuGet-Paket „MSAL.Net“ hinzufügen, funktioniert dies problemlos.
Wenn Sie allerdings eine vorhandene Xamarin.Forms-Anwendung auf MSAL.NET Preview 1.1.2 oder höher upgraden möchten, können Probleme beim Erstellen auftreten.

Führen Sie folgende Schritte durch, um diese Probleme zu beheben:
- Aktualisieren Sie das vorhandene MSAL.NET-NuGet-Paket auf MSAL.NET Preview 1.1.2 oder höher.
- Überprüfen Sie, ob Xamarin.Forms automatisch auf Version 2.5.0.122203 aktualisiert wird (falls nicht, führen Sie ein Update auf diese Version durch).
- Überprüfen Sie, ob Xamarin.Android.Support.v4 automatisch auf Version 25.4.0.2 aktualisiert wird (falls nicht, führen Sie ein Update auf diese Version durch).
- Die Xamarin.Android.Support-Pakete sollten alle Version 25.4.0.2 als Zielversion aufweisen.
- Bereinigen/Neu erstellen
- Legen Sie die maximale Anzahl paralleler Projektbuilds in Visual Studio auf 1 fest (Optionen > Projekte und Projektmappen > Erstellen und Ausführen > Maximale Anzahl paralleler Projektbuilds).
- Alternativ können Sie „/m“ aus Ihrem Befehl entfernen, wenn Sie über die Befehlszeile erstellen.


### <a name="error-the-name-authenticationcontinuationhelper-does-not-exist-in-the-current-context"></a>Fehler Der Name „AuthenticationContinuationHelper“ ist im aktuellen Kontext nicht vorhanden.

Dies liegt eventuell daran, dass Visual Studio die Datei „Android.csproj*“ nicht ordnungsgemäß aktualisiert hat. Manchmal enthält der **\<HintPath>** Dateipfad fälschlicherweise netstandard13 anstelle von **monoandroid90**.

```xml
<Reference Include="Microsoft.Identity.Client, Version=3.0.4.0, Culture=neutral, PublicKeyToken=0a613f4dd989e8ae,
           processorArchitecture=MSIL">
  <HintPath>..\..\packages\Microsoft.Identity.Client.3.0.4-preview\lib\monoandroid90\Microsoft.Identity.Client.dll</HintPath>
</Reference>
```

## <a name="next-steps"></a>Nächste Schritte

Detailliertere Informationen und Beispiele finden Sie im Abschnitt [Android Specific Considerations (Besondere Überlegungen zu Android)](https://github.com/azure-samples/active-directory-xamarin-native-v2#android-specific-considerations) der Datei „readme.md“ des folgenden Beispiels:

| Beispiel | Plattform | BESCHREIBUNG |
| ------ | -------- | ----------- |
|[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) | Xamarin iOS, Android, UWP | Eine einfache Xamarin Forms-App, die die Verwendung der MSAL zum Authentifizieren von MSA und Azure AD über den AAD v2.0-Endpunkt und den Zugriff auf Microsoft Graph mit dem resultierenden Token veranschaulicht. <br>![Topologie](media/msal-net-xamarin-android-considerations/topology.png) |
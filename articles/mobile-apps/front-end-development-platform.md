---
title: Wählen der Front-End-Entwicklungsplattform zum Erstellen von Clientanwendungen mit Visual Studio und Azure-Diensten
description: Erfahren Sie mehr über die unterstützten nativen und plattformübergreifenden Sprachen zum Erstellen von Clientanwendungen.
author: elamalani
manager: elamalani
ms.service: vs-appcenter
ms.assetid: 355f0959-aa7f-472c-a6c7-9eecea3a34b9
ms.topic: conceptual
ms.date: 10/22/2019
ms.author: emalani
ms.openlocfilehash: 891967e74e52c1311d73cbe13fff8c95e479478c
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72795086"
---
# <a name="choose-mobile-development-framework"></a>Auswählen eines Frameworks für die mobile Entwicklung
Entwickler können clientseitige Technologien verwenden, um mobile Anwendungen selbst zu erstellen, wozu sie bestimmte Frameworks und Muster für einen plattformübergreifenden Ansatz verwenden. Entwickler können, je nach ihren Entscheidungsfaktoren, native Anwendungen (für native Einzelplattformen mit Sprachen wie Objective-C und Java), plattformübergreifende Anwendungen (mit Xamarin, .NET und C#) und Hybridanwendungen (mit Cordova) und deren Varianten erstellen.

## <a name="native-platforms"></a>Native Plattformen
Das Entwickeln einer nativen Anwendung erfordert plattformspezifische Programmiersprachen, SDKs und Entwicklungsumgebungen sowie weitere Tools, die von Betriebssystemanbietern bereitgestellt werden.

### <a name="ios"></a>iOS
Ein von Apple entwickeltes Betriebssystem. Apps, die unter iOS erstellt wurden, werden auf Apple-Geräten des Typs iPhone oder iPad ausgeführt.

- **Programmiersprachen**: Objective-C, Swift
- **IDE**: Xcode
- **SDK**: iOS SDK

### <a name="android"></a>Android
Ein von Google entwickeltes und das weltweit beliebteste Betriebssystem. Anwendungen, die unter Android erstellt wurden, können auf verschiedenen Smartphones und Tablets ausgeführt werden.

- **Programmiersprache**: Java, Kotlin 
- **IDE**: Android Studio und Android Developer Tools 
- **SDK**: Android SDK

### <a name="windows"></a>Windows
- **Programmiersprache**: C#
- **IDE**: Visual Studio, Visual Studio Code
- **SDK**: Windows SDK

#### <a name="pros"></a>Vorteile
- Hohe Benutzerfreundlichkeit
- Reaktionsschnelle Anwendungen mit hoher Leistung und der Möglichkeit, Schnittstellen zu nativen Bibliotheken zu nutzen
- Hochgradig sichere Anwendungen

#### <a name="cons"></a>Nachteile
- Anwendung wird nur auf einer Plattform ausgeführt
- Erstellen einer Anwendung erfordert mehr Entwicklerressourcen und ist teurer
- Geringere Wiederverwendung von Code

## <a name="cross-platforms-and-hybrid-applications"></a>Plattformübergreifende und Hybridanwendungen
Plattformübergreifende Anwendungen bieten Ihnen die Möglichkeit, native mobile Anwendungen einmal zu schreiben, den Code für alle zu verwenden und sie unter iOS, Android und Windows auszuführen.

### <a name="xamarin"></a>Xamarin
Mit [Xamarin](https://visualstudio.microsoft.com/xamarin/), das Microsoft gehört, können Sie robuste, plattformübergreifende mobile Anwendungen in C# mit einer Klassenbibliothek und Runtime erstellen. Diese Runtime funktioniert auf vielen Plattformen, einschließlich iOS, Android und Windows, aber trotzdem werden native (nicht interpretierte) Anwendungen kompiliert, die sehr leistungsfähig sind. Xamarin kombiniert alle Möglichkeiten der nativen Plattformen und fügt eine Reihe von eigenen leistungsstarken Features hinzu.

- **Programmiersprache**: C#
- **IDE**: Visual Studio unter Windows oder Mac

### <a name="react-native"></a>React Native
[React Native](https://facebook.github.io/react-native/), das 2015 von Facebook veröffentlicht wurde, ist ein [Open Source](https://github.com/facebook/react-native)-JavaScript-Framework zum Schreiben von echten, mit nativem Rendern arbeitenden mobilen Anwendungen für iOS und Android. Das Framework basiert auf React, Facebooks JavaScript-Bibliothek zum Erstellen von Benutzeroberflächen, wird aber nicht für Browser, sondern für mobile Plattformen verwendet. In React Native werden native Komponenten anstelle von Webkomponenten als Bausteine verwendet.
 
- **Programmiersprache**: JavaScript
- **IDE**: Visual Studio Code

### <a name="unity"></a>Unity
 Unity ist eine Engine, die zum Erstellen von Spielen optimiert ist. Mit Unity können Entwickler hochwertige 2D/3D-Anwendungen mit C# für verschiedene Plattformen, einschließlich Windows, iOS, Android und Xbox, erstellen.

### <a name="cordova"></a>Cordova
Mit Cordova können Sie Hybridanwendungen erstellen, wozu Sie Visual Studio-Tools für Apache Codova oder Visual Studio Code mit Erweiterungen für Cordova verwenden. Mit dem Hybridansatz können Sie Komponenten mit Websites teilen und webserverbasierte Anwendungen mit gehosteten Webanwendungen wiederverwenden, die auf Cordova basieren.

#### <a name="pros"></a>Vorteile
- Verbesserte Codenutzbarkeit durch Erstellen einer Codebasis für mehrere Plattformen
- Für eine größere Zielgruppe über viele Plattformen
- Erheblich kürzere Entwicklungszeit
- Einfach zu starten und zu aktualisieren

#### <a name="cons"></a>Nachteile
- Geringere Leistung
- Fehlende Flexibilität
- Jede Plattform hat einen einzigartigen Satz von Features und Funktionen, um die native Anwendung kreativer zu gestalten.
- Längere Entwicklungszeit für die Benutzeroberfläche
- Tooleinschränkung

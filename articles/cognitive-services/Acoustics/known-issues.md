---
title: Bekannte Probleme mit dem Projekt Akustik-Plug-In
titlesuffix: Azure Cognitive Services
description: In Projekt Akustik treten möglicherweise die folgenden bekannten Probleme auf.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: b71b93f271608d946d964f70dae9eefbef77e87b
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72243044"
---
# <a name="project-acoustics-known-issues"></a>Bekannte Probleme mit Projekt Akustik
In diesem Artikel werden Probleme beschrieben, die beim Verwenden von Projekt Akustik auftreten können.

## <a name="acoustic-parameters-are-lost-when-you-rename-a-scene"></a>Beim Umbenennen einer Szene gehen akustische Parameter verloren

Wenn Sie eine Szene umbenennen, werden nicht alle zu dieser Szene gehörenden akustischen Parameter automatisch an die neue Szene übertragen. Sie bleiben jedoch in der alten Medienobjektdatei erhalten. Suchen Sie nach der Datei *[Szenenname]_AcousticParameters.asset* im Verzeichnis *Editor* neben Ihrer Szenendatei. Benennen Sie die Datei entsprechend dem neuen Szenennamen um.

## <a name="deploy-to-android-bug-from-some-unity-versions"></a>Fehler beim Bereitstellen auf Android von einigen Unity-Versionen aus

Einige Versions von Unity weisen einen [Fehler](https://issuetracker.unity3d.com/issues/android-ios-audiosource-playing-through-google-resonance-audio-sdk-with-spatializer-enabled-does-not-play-on-built-player) beim Bereitstellen von Audio-Plug-Ins auf Android auf. Stellen Sie sicher, dass Sie keine Version verwenden, die von diesem Fehler betroffen ist.

## <a name="could-not-find-metadata-file-systemsecuritydll-error"></a>Fehler „could not find metadata file System.Security.dll“ (Die Metadatendatei „System.Security.dll“ wurde nicht gefunden)

Stellen Sie sicher, dass für die **Scripting Runtime Version** in den **Player**-Einstellungen *.NET 4.x Equivalent* (.NET 4.x-Entsprechung) festgelegt ist, und starten Sie Unity neu.

## <a name="authentication-problems-when-connecting-to-azure"></a>Authentifizierungsprobleme beim Herstellen einer Verbindung mit Azure

Überprüfen Sie Folgendes:
- Sie haben die richtigen Anmeldeinformationen für Ihr Azure-Konto angegeben.
- Ihr Konto unterstützt den Typ von Knoten, den Sie im Baking angefordert haben.
- Ihre Systemuhr ist ordnungsgemäß festgelegt.

## <a name="the-bake-tab-still-shows-deleting-after-you-cancel"></a>Auf der Registerkarte „Bake“ wird nach dem Abbrechen immer noch „deleting“ (wird gelöscht) angezeigt.
Projekt Akustik bereinigt alle Azure-Ressourcen für einen Auftrag nach erfolgreichem Abschluss oder Abbruch. Dieser Vorgang kann bis zu 5 Minuten dauern.

## <a name="next-steps"></a>Nächste Schritte
* Sehen Sie sich den Beispielinhalt für [Unity](unity-quickstart.md) oder [Unreal](unreal-quickstart.md) an.

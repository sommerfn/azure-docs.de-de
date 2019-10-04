---
title: Bekannte Probleme mit dem Project Acoustics-Plug-In
titlesuffix: Azure Cognitive Services
description: Bei Verwendung der Designervorschau für Project Acoustics können die folgenden bekannten Probleme auftreten.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: 37084480423de90f50beced187eda202b39f8bf1
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68933062"
---
# <a name="project-acoustics-known-issues"></a>Bekannte Probleme mit Projekt Akustik
Bei Verwendung der Designervorschau für Project Acoustics können die folgenden bekannten Probleme auftreten.

## <a name="acoustic-parameters-are-lost-when-you-rename-a-scene"></a>Beim Umbenennen einer Szene gehen akustische Parameter verloren

Wenn Sie eine Szene umbenennen, werden nicht alle zu dieser Szene gehörenden akustischen Parameter automatisch an die neue Szene übertragen. Sie bleiben jedoch in der alten ASSET-Datei erhalten. Suchen Sie nach der Datei **Szenenname_Akustikparameter.asset** im Verzeichnis **Editor** neben Ihrer Szenendatei. Benennen Sie Ihre Datei entsprechend dem neuen Szenennamen um.

## <a name="deploying-to-android-from-some-unity-versions"></a>Bereitstellen auf Android von einigen Unity-Versionen aus

Einige Unity-Versionen weisen einen Fehler beim Bereitstellen von Audio-Plug-Ins auf Android auf. Stellen Sie sicher, dass Sie keine Version verwenden, die von [diesem Fehler](https://issuetracker.unity3d.com/issues/android-ios-audiosource-playing-through-google-resonance-audio-sdk-with-spatializer-enabled-does-not-play-on-built-player) betroffen sind.

## <a name="i-get-an-error-that-could-not-find-metadata-file-systemsecuritydll"></a>Die Fehlermeldung „could not find metadata file System.Security.dll“ (Die Metadatendatei „System.Security.dll“ wurde nicht gefunden) wird angezeigt

Stellen Sie sicher, dass für die Scripting Runtime Version in den Player-Einstellungen **.NET 4.x Equivalent** (.NET 4.x-Entsprechung) festgelegt ist, und starten Sie Unity neu.

## <a name="im-having-authentication-problems-when-connecting-to-azure"></a>Ich habe beim Herstellen einer Verbindung mit Azure Authentifizierungsprobleme

Vergewissern Sie sich, dass Sie die richtigen Anmeldeinformationen für Ihr Azure-Konto verwendet haben, dass Ihr Konto den im Bake-Vorgang angeforderten Knotentyp unterstützt und dass Ihre Systemuhr stimmt.

## <a name="canceling-a-bake-leaves-the-bake-tab-in-deleting-state"></a>Das Abbrechen eines Bake-Vorgangs lässt die Registerkarte für den Bake-Vorgang im Zustand „Wird gelöscht“.
Projekt Akustik bereinigt alle Azure-Ressourcen für einen Auftrag nach erfolgreichem Abschluss oder Abbruch. Der Vorgang kann bis zu 5 Minuten dauern.

## <a name="next-steps"></a>Nächste Schritte
* Sehen Sie sich den Beispielinhalt für [Unity](unity-quickstart.md) oder [Unreal](unreal-quickstart.md) an.


---
title: Erstellen eines benutzerdefinierten Schlüsselworts – Speech Service
titleSuffix: Azure Cognitive Services
description: Ihr Gerät lauscht immer auf ein Schlüsselwort (oder einen Ausdruck). Wenn der Benutzer das Schlüsselwort nennt, sendet das Gerät sämtliche nachfolgende Audioaufnahmen an die Cloud, bis der Benutzer aufhört zu sprechen. Durch das Anpassen Ihres Schlüsselworts können Sie Ihr Gerät auf effektive Weise differenzieren und Ihr Branding stärken.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: bf9afb66163532b4095e0d30b1167010320abbf8
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73490898"
---
# <a name="create-a-custom-keyword-by-using-the-speech-service"></a>Erstellen eines benutzerdefinierten Schlüsselworts mit dem Speech Service

Ihr Gerät lauscht immer auf ein Schlüsselwort (oder einen Ausdruck). „Hey Cortana“ ist beispielsweise ein Schlüsselwort für den Cortana-Assistenten. Wenn der Benutzer das Schlüsselwort nennt, sendet das Gerät sämtliche nachfolgende Audioaufnahmen an die Cloud, bis der Benutzer aufhört zu sprechen. Durch das Anpassen Ihres Schlüsselworts können Sie Ihr Gerät auf effektive Weise differenzieren und Ihr Branding stärken.

In diesem Artikel erfahren Sie, wie Sie für Ihr Gerät ein benutzerdefiniertes Schlüsselwort erstellen.

## <a name="choose-an-effective-keyword"></a>Auswählen eines effektiven Schlüsselworts

Berücksichtigen Sie bei der Auswahl eines Schlüsselworts die folgenden Richtlinien:

* Ihr Schlüsselwort sollte ein englisches Wort oder ein englischer Ausdruck sein. Es sollte nicht länger als zwei Sekunden dauern, es auszusprechen.

* Wörter mit 4 bis 7 Silben eignen sich am besten. So ist „Hey, Computer“ beispielsweise ein gutes Schlüsselwort. Nur „Hey“ ist ein schlechtes Aktivierungswort.

* Bei Schlüsselwörtern sollten englische Ausspracheregeln befolgt werden.

* Durch ein einzigartiges Wort oder ein Kunstwort, das die allgemeinen englischen Ausspracheregeln befolgt, könnten Ergebnisse mit falsch positiven Werten verringert werden. „Computerama“ würde sich beispielsweise gut als Schlüsselwort eignen.

* Wählen Sie kein gängiges Wort aus. Die Wörter „eat“ und „go“ werden beispielsweise häufig in gewöhnlichen Konversationen gesagt. Sie könnten falsche Auslöser für Ihr Gerät sein.

* Vermeiden Sie die Verwendung eines Schlüsselworts, das verschiedene Aussprachevarianten haben könnte. Benutzern müsste die „richtige“ Aussprache bekannt sein, um eine Antwort von ihrem Gerät erhalten zu können. „509“ kann beispielsweise als „five zero nine“, „five oh nine“ oder als „five hundred and nine“ ausgesprochen werden. „R.E.I.“ kann als „r-e-i“ oder als „ray“ ausgesprochen werden. „Live“ kann „/līv/“ oder „/liv/“ ausgesprochen werden.

* Verwenden Sie keine Sonderzeichen, Symbole oder Ziffern. „Go#“ und „20 + cats“ wären beispielsweise keine guten Aktivierungswörter. „Go sharp“ oder „twenty plus cats“ kann jedoch funktionieren. Sie können weiterhin die Symbole in Ihrem Branding sowie Marketing und Dokumentation zur Untermauerung der richtigen Aussprache verwenden.

> [!NOTE]
> Wenn Sie als Schlüsselwort ein markenrechtlich geschütztes Wort auswählen, müssen Sie Eigentümer dieser Marke sein oder über eine Berechtigung vom Eigentümer der Marke zur Verwendung des Worts verfügen. Microsoft haftet nicht für rechtliche Probleme, die möglicherweise durch Ihre Auswahl eines Schlüsselworts auftreten.

## <a name="create-your-keyword"></a>Erstellen Ihres Schlüsselworts

Bevor Sie auf Ihrem Gerät ein benutzerdefiniertes Schlüsselwort verwenden können, müssen Sie es mithilfe des Microsoft-Diensts für die Generierung eines benutzerdefinierten Schlüsselworts erstellen. Nachdem Sie ein Schlüsselwort angegeben haben, erstellt der Dienst eine Datei, die Sie anschließend zur Aktivierung des Schlüsselworts auf Ihrem Gerät in Ihrem Development Kit bereitstellen.

1. Wechseln Sie zu [Speech Studio](https://aka.ms/sdsdk-speechportal), und **melden Sie sich an**. Wenn Sie kein Speech-Abonnement haben, wählen Sie [**Abonnement erstellen**](https://go.microsoft.com/fwlink/?linkid=2086754) aus.

1. Geben Sie auf der Seite [Benutzerdefiniertes Schlüsselwort](https://aka.ms/sdsdk-wakewordportal) das Schlüsselwort Ihrer Wahl ein, und klicken Sie auf **Schlüsselwort hinzufügen**. Wir haben einige [Richtlinien](#choose-an-effective-keyword), die bei der Auswahl eines effektiven Stichworts helfen. Der Support ist derzeit auf die Sprache „en-US“ beschränkt.

    ![Eingeben Ihres Schlüsselworts](media/speech-devices-sdk/custom-kws-portal-enter-keyword.png)

1. Im Portal werden nun Kandidaten für die Aussprache Ihres Schlüsselworts erstellt. Hören Sie sich die einzelnen Kandidaten an, indem Sie auf die Wiedergabeschaltflächen klicken. Deaktivieren Sie dann alle nicht ordnungsgemäßen Aussprachevarianten. Wenn nur noch geeignete Aussprachen aktiviert sind, wählen Sie **Übermitteln** aus, um mit dem Generieren des Schlüsselworts zu beginnen. Wenn Sie das Schlüsselwort ändern möchten, entfernen Sie zunächst das vorhandene, indem Sie auf die Schaltfläche „Löschen“ klicken, die rechts in der Zeile angezeigt wird, wenn Sie den Mauszeiger darüber bewegen.

    ![Überprüfen Ihres Schlüsselworts](media/speech-devices-sdk/custom-kws-portal-review-keyword.png)

1. Es kann bis zu einer Minute dauern, bis das Modell generiert ist. Sie werden dann aufgefordert, die Datei herunterzuladen.

    ![Herunterladen Ihres Schlüsselworts](media/speech-devices-sdk/custom-kws-portal-download-keyword.png)

1. Speichern Sie die ZIP-Datei auf Ihrem Computer. Sie benötigen diese Datei, um Ihr benutzerdefiniertes Schlüsselwort im Development Kit bereitzustellen.

## <a name="next-steps"></a>Nächste Schritte

Testen Ihres benutzerdefinierten Schlüsselworts mit dem [Schnellstart zum Speech-Geräte-SDK](https://aka.ms/sdsdk-quickstart).

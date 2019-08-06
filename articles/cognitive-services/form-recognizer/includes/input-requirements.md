---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/27/2019
ms.author: pafarley
ms.openlocfilehash: 17dc32f8948387b90229d3c4c07102cff98e3018
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562675"
---
Die Formularerkennung arbeitet mit Eingabedokumenten, die diese Anforderungen erfüllen:

* Das Format muss JPG, PNG oder PDF (Text oder Scan) sein. In Text eingebettete PDFs eignen sich besser, da beim Extrahieren von Zeichen und Speicherort keine Fehler entstehen können.
* Wenn Ihre PDF-Dateien mit einem Kennwort geschützt sind, müssen Sie die Sperre entfernen, bevor Sie sie senden.
* Die Dateigröße muss weniger als 4 MB betragen.
* Bei Bildern müssen die Abmessungen zwischen 50 × 50 Pixel und 4.200 × 4.200 Pixel liegen.
* Werden Papierdokumente eingescannt, sollten dies Scans mit hoher Qualität sein.
* Für Text muss das lateinische Alphabet (englische Zeichen) verwendet werden.
* Die Daten müssen Schlüssel und Werte enthalten.
* Schlüssel können über oder links neben den Werten, aber nicht unter oder rechts davon angezeigt werden.

Die Formularerkennung unterstützt derzeit nicht die folgenden Arten von Eingabedaten:

* Komplexe Tabellen (geschachtelte Tabellen, zusammengeführte Header oder Zellen usw.)
* Kontrollkästchen oder Optionsfelder
* PDF-Dokumente mit mehr als 50 Seiten
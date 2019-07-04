---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: include
ms.date: 06/27/2019
ms.author: pafarley
ms.openlocfilehash: 690219347782aad2140b0a1b0541590c5426c568
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67459729"
---
Die Formularerkennung arbeitet mit Eingabedokumenten, die diese Anforderungen erfüllen:

* Das Format muss JPG, PNG oder PDF (Text oder Scan) sein. In Text eingebettete PDFs eignen sich besser, da beim Extrahieren von Zeichen und Speicherort keine Fehler entstehen können.
* Die Dateigröße muss weniger als 4 MB betragen.
* Bei Bildern müssen die Abmessungen zwischen 50 × 50 Pixel und 4.200 × 4.200 Pixel liegen.
* Werden Papierdokumente eingescannt, sollten dies Scans mit hoher Qualität sein.
* Für Text muss das lateinische Alphabet (englische Zeichen) verwendet werden.
* Daten müssen gedruckt (nicht handschriftlich) vorliegen.
* Die Daten müssen Schlüssel und Werte enthalten.
* Schlüssel können über oder links neben den Werten, aber nicht unter oder rechts davon angezeigt werden.

Die Formularerkennung unterstützt derzeit nicht die folgenden Arten von Eingabedaten:

* Komplexe Tabellen (geschachtelte Tabellen, zusammengeführte Header oder Zellen usw.)
* Kontrollkästchen oder Optionsfelder
* PDF-Dokumente mit mehr als 50 Seiten
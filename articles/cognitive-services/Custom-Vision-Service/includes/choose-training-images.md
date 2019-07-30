---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: include
ms.date: 07/17/2019
ms.author: pafarley
ms.openlocfilehash: ee2c1fd1fc1cad07b14a2c99318be20be30db9c5
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2019
ms.locfileid: "68423592"
---
Es wird empfohlen, im ersten Trainingssatz mindestens 30 Bilder pro Tag zu verwenden. Sie sollten auch einige zusätzliche Bilder sammeln, um Ihr Modell zu testen, nachdem es trainiert wurde.

Verwenden Sie zum effektiven Trainieren Ihres Modells Bilder mit optischer Vielfalt. Wählen Sie Bilder aus, die sich nach folgenden Aspekten unterscheiden:
* Kamerawinkel
* Belichtung
* background
* Visueller Stil
* Einzelne/gruppierte Motive
* size
* type

Stellen Sie außerdem sicher, dass alle Ihre Trainingsbilder die folgenden Kriterien erfüllen:
* JPG-, PNG- oder BMP-Format
* Höchstens 6 MB groß (4 MB für Vorhersagebilder)
* Mindestens 256 Pixel an der kürzesten Seite; kürzere Bilder werden von Custom Vision Service automatisch hochskaliert.
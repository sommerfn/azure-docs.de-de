---
title: Azure Data Share-Terminologie
description: Informationen zu häufigen Begriffen zum Beschreiben von Ressourcen, die in Azure Data Share verwendet werden (Datenanbieter, Datenconsumer, Datenfreigabe, Freigabeabonnement, Momentaufnahme, Einladung, Empfänger)
ms.service: data-share
author: joannapea
ms.author: joanpo
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 33532380d8f98df44029eeea998130d1da5fdafd
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73468555"
---
# <a name="azure-data-share-concepts"></a>Azure Data Share-Konzepte 

In Azure Data Share gibt es neue Begriffe im Zusammenhang mit der Datenfreigabe. Dieser Artikel beschreibt einige häufig verwendete Begriffe, die überall im Dienst verwendet werden. 

## <a name="data-provider"></a>Datenanbieter

Ein Datenanbieter ist die Organisation, die Daten für Kunden freigibt. Der Datenanbieter kann in der Regel der Besitzer oder ein Kurator der Daten sein. Datenanbieter möchten Daten verschiedener Typen freigeben. Einige Beispiele für Daten, die ein Datenanbieter freigeben möchte, sind Rohdaten, z.B. Point-of-Sale- oder Zeitreihendaten. Ein Datenanbieter kann auch vorverarbeitete, kuratierte Daten freigeben, die bereits Analysen und Erkenntnisse beinhalten. 

## <a name="data-consumer"></a>Datenconsumer 

Ein Datenconsumer ist die Organisation, die Daten von einem Datenanbieter empfängt. Der Datenconsumer möchte möglicherweise die freigegebenen Daten mit seinen eigenen Daten verknüpfen, um Erkenntnisse zu gewinnen. In einigen Fällen kann der Datenconsumer Daten empfangen, die bereits verarbeitet wurden. 

## <a name="data-share"></a>Datenfreigabe

Eine Datenfreigabe ist eine Gruppe von Datasets, die als einzelne Entität freigegeben werden. Die Datasets können aus einer Reihe von Azure-Datenquellen stammen, die von Azure Data Share unterstützt werden. Zurzeit unterstützt Azure Data Share Azure Blob Storage und Azure Data Lake Store. 

## <a name="share-subscription"></a>Freigabeabonnement 

Ein Freigabenabonnement wird erstellt, wenn ein Datenconsumer eine Datenfreigabeeinladung von einem Datenanbieter annimmt. Datenanbieter können aktive Freigabeabonnements anzeigen, indem Sie zu **Gesendete Freigaben** in ihrem Azure Data Share-Konto navigieren und **Freigabeabonnements** auswählen.

Ein Datenconsumer kann überprüfen, ob er über ein aktives Freigabeabonnement verfügt, indem er zu **Empfangene Freigaben** navigiert und den Status seiner empfangenen Freigaben anzeigt. 

## <a name="snapshot"></a>Momentaufnahme

Eine Momentaufnahme kann von einem Datenconsumer erstellt werden, wenn er eine Datenfreigabeeinladung annimmt. Wenn er eine Einladung angenommen hat, kann er eine vollständige Momentaufnahme der Daten auslösen, die für ihn freigegeben wurden. Die Momentaufnahme ist eine Kopie der Daten zu dem Zeitpunkt, zu dem der Datenconsumer die Momentaufnahme generiert hat. 

Es gibt zwei Arten von Momentaufnahmen: vollständige und inkrementelle. Eine vollständige Momentaufnahme enthält alle Daten in der Datenfreigabe. Eine inkrementelle Momentaufnahme enthält alle Daten, die seit der Auslösung der letzten Momentaufnahme aktualisiert/hinzugefügt wurden. 

## <a name="snapshot-settings-in-azure-data-share"></a>Momentaufnahmeeinstellungen in Azure Data Share
 
Ein Datenanbieter kann eine Momentaufnahmeeinstellung für eine Dateifreigabe aktivieren. Diese Einstellung ermöglicht es dem Datenconsumer, inkrementelle Aktualisieren zu empfangen, wenn sie auftreten. Diese Einstellung sollte aktiviert werden, wenn der Datenanbieter möchte, dass seine Datenconsumer Aktualisierungen der Daten empfangen, die freigegeben wurden. 

Wenn ein Datenanbieter diese Einstellung aktiviert, kann ein Wiederholungsintervall ausgewählt werden. Das Wiederholungsintervall kann stündlich oder täglich sein. 

Ein Datenconsumer hat die Möglichkeit, diesen Momentaufnahme-Zeitplan zu abonnieren, um inkrementelle Aktualisierungen zu erhalten, die alle Daten beinhalten, die sich geändert haben, seit er zum ersten Mal eine neue Momentaufnahme generiert hat. 

## <a name="invitation"></a>Einladung

Ein Datenanbieter kann mehrere Empfängern zu seiner Datenfreigabe einladen. Er kann zu diesem Zweck der Datenfreigabe Empfänger hinzufügen. Einladungen können auch hinzugefügt werden, nachdem eine Datenfreigabe erstellt wurde. 

Ein Datenanbieter kann eine Einladung löschen, nachdem sie gesendet wurde, wenn sie nicht angenommen wurde. Wenn der Datenanbieter eine Einladung löscht, die noch nicht angenommen wurde, ist der Datenconsumer nicht mehr in der Lage, die Einladung zu akzeptieren. 

Einladungen können bis zu fünfmal täglich erneut gesendet werden. 

## <a name="recipient"></a>Recipient

Ein Empfänger ist eine Person, die eine Einladung für eine Datenfreigabe empfängt. In der Regel wird ein Datenanbieter einer Datenfreigabe Empfänger hinzufügen, die er erstellt. Sobald der Empfänger einer Einladung die Einladung annimmt, wird er zu einem Datenconsumer.  

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Freigeben von Daten finden Sie im Tutorial zum [Freigeben Ihrer Daten](share-your-data.md).

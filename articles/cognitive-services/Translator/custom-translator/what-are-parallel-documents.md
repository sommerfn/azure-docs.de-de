---
title: Was sind parallele Dokumente? - Custom Translator
titleSuffix: Azure Cognitive Services
description: Parallele Dokumente sind Dokumentpaare, bei denen ein Dokument die Übersetzung des anderen enthält. Das eine Dokument des Paars enthält also die Sätze in der Ausgangssprache und das andere die entsprechenden Sätze in der Zielsprache.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: d7c38a44e3111a319e4146b3c9b71a22b0d31bfd
ms.sourcegitcommit: 9a4296c56beca63430fcc8f92e453b2ab068cc62
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2019
ms.locfileid: "72675467"
---
# <a name="what-are-parallel-documents"></a>Was sind parallele Dokumente?

Parallele Dokumente sind Dokumentpaare, bei denen ein Dokument die Übersetzung des anderen enthält. Das eine Dokument des Paars enthält also die Sätze in der Ausgangssprache und das andere die entsprechenden Sätze in der Zielsprache.
Es spielt keine Rolle, welche Sprache als „Quelle“ markiert ist und welche als „Ziel“. Mit einem parallelen Dokument kann ein Übersetzungssystem in beide Richtungen trainiert werden.

## <a name="requirements"></a>Requirements (Anforderungen)

Sie benötigen mindestens 10.000 eindeutige zugeordnete parallele Sätze, um ein System zu trainieren. Durch diese Einschränkung wird sichergestellt, dass Ihre parallelen Sätze genügend eindeutiges Vokabular zum erfolgreichen Trainieren eines Übersetzungsmodells enthalten. Es empfiehlt sich, kontinuierlich weitere parallele Inhalte hinzuzufügen und das Training erneut auszuführen, um die Qualität Ihres Übersetzungssystems zu verbessern. Weitere Informationen finden Sie unter [Satzpaarbildung und -zuordnung in parallelen Dokumenten](https://docs.microsoft.com/azure/cognitive-services/translator/custom-translator/sentence-alignment).

Gemäß den Bestimmungen von Microsoft dürfen in Custom Translator hochgeladene Dokumente die Copyright-, Schutz- und Urheberrechte von Dritten nicht verletzen. Weitere Informationen finden Sie in den [Nutzungsbestimmungen](https://azure.microsoft.com/support/legal/cognitive-services-terms/).
Durch das Hochladen eines Dokuments über das Portal ändern sich die Rechte am geistigen Eigentum des Dokuments selbst nicht.

## <a name="use-of-parallel-documents"></a>Verwenden von parallelen Dokumenten

Parallele Dokumente werden vom System zu folgenden Zwecken verwendet:

1.  Um zu lernen, wie Wörter, Ausdrücke und Sätze zwischen den beiden Sprachen einander im Allgemeinen entsprechen bzw. zugeordnet werden.

2.  Um zu lernen, wie der jeweilige Kontext abhängig von den umgebenden Ausdrücke verarbeitet werden sollte. Manche Wörter müssen je nach Kontext unterschiedlich übersetzt werden.

Eine bewährte Methode besteht darin, eine 1:1-Entsprechung zwischen den Sätzen in den Quell- und Zielsprachversionen der Dokumente sicherzustellen.

Wenn Ihr Projekt domänen- bzw. kategoriespezifisch ist, sollte die Terminologie dieser Kategorie in den Dokumenten konsistent verwendet werden. Die Qualität des resultierenden Übersetzungssystems hängt von der Anzahl von Sätzen in Ihren Dokumenten und von der Qualität der Sätze ab. Je mehr Beispiele für den unterschiedlichen Gebrauch eines kategoriespezifischen Worts Ihre Dokumente enthalten, desto besser kann es vom System übersetzt werden.

Dokumente, die hochgeladen werden, sind private Dokumente der jeweiligen Arbeitsbereiche und können in beliebig vielen Projekten oder Trainingsvorgängen verwendet werden. Aus Ihren Dokumenten extrahierte Sätze werden separat als Unicode-Nur-Text-Dateien in Ihrem Repository gespeichert und können von Ihnen gelöscht werden. Verwenden Sie Custom Translator nicht als Dokumentrepository. Es ist nicht möglich, die Dokumente in dem Format herunterzuladen, in dem Sie sie hochgeladen haben.



## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie ein [Wörterbuch](what-is-dictionary.md) in Custom Translator verwenden.

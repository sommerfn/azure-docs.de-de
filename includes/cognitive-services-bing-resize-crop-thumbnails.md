---
author: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 11/09/2018
ms.author: nitinme
ms.openlocfilehash: b1cf3dc24482da70372f775d0ba63a203df1b285
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51572193"
---
Einige Bing-Antworten enthalten URLs zu von Bing bereitgestellten Miniaturbildern. Miniaturbilder können vergrößert/verkleinert und zugeschnitten werden. 

> [!NOTE]
> Achten Sie darauf, dass Größe und Zuschnitt des Miniaturbilds ein Suchszenario ermöglichen, und respektieren Sie die Rechte Dritter gemäß den Verwendungs- und Anzeigeanforderungen der Bing-Suche-API.


Wenn Sie die Größe eines Bilds ändern möchten, fügen Sie die Abfrageparameter „w“ (Breite) und „h“ (Höhe) in die URL der Miniaturansicht ein. Geben Sie Breite und Höhe in Pixel an. Beispiel:   
  
`https://<host>/th?id=JN.5l3yzwy%2f%2fHj59U6XhssIQ&pid=Api&w=200&h=200`  
  
Wenn Sie die Größe des Bilds ändern, wird das Seitenverhältnis beibehalten. Hierzu wird am Rand des Bilds ggf. ein weißer Bereich hinzugefügt. Wenn Sie also beispielsweise die Größe eines Bilds mit den Abmessungen 480 x 359 auf 200 x 200 festlegen, ohne das Bild zuzuschneiden, füllt das Bild zwar die gesamte Breite aus, oben und unten wird jedoch ein weißer Bereich mit 25 Pixeln hinzugefügt. Gleiches gilt bei einem Bild mit den Abmessungen 359 x 480; in diesem Fall wird der weiße Bereich allerdings links und rechts hinzugefügt. Wenn Sie das Bild zuschneiden, wird der weiße Bereich nicht hinzugefügt.  

 
Die folgende Abbildung zeigt die Originalgröße eines Miniaturbilds (480 x 300):  
  
![Originalbild einer Landschaft](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape.PNG)  
  
Die folgende Abbildung zeigt das Bild nach der Größenänderung auf 200 x 200: Das Seitenverhältnis wird beibehalten, und am oberen und unteren Rand wird ein weißer Bereich hinzugefügt. (Der schwarze Rand dient lediglich zur Veranschaulichung der Auffüllung.)  
  
![Landschaftsbild nach der Größenänderung](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape-resized.PNG)  



Wenn die angegebenen Abmessungen die Breite und Höhe des Originalbilds übersteigen, wird am linken und am oberen Rand ein weißer Bereich hinzugefügt.  
  
Bilder können durch Angabe des Abfrageparameters „c“ zugeschnitten werden. Folgende Werte sind verfügbar:  
  
- 4&mdash;Blindes Seitenverhältnis  
- 7&mdash;Intelligentes Seitenverhältnis  
  
Bei Verwendung des Zuschnitts mit intelligentem Seitenverhältnis (c = 7) wird das Bild ausgehend von der Mitte des relevanten Bildbereichs unter Beibehaltung des Seitenverhältnisses nach außen hin zugeschnitten. Bei dem relevanten Bereich handelt es sich um den Bereich, der nach Ermessen von Bing die wichtigsten Elemente enthält. Die folgende Abbildung zeigt ein Beispiel für einen relevanten Bereich:  
  
![Relevanter Bereich](./media/cognitive-services-bing-resize-crop/bing-resize-crop-regionofinterest.PNG)

Wenn Sie die Größe eines Bilds ändern und den Zuschnitt mit intelligentem Seitenverhältnis verwenden, wird das Bild unter Beibehaltung des Seitenverhältnisses auf die angeforderte Größe reduziert. Anschließend wird das Bild auf die neuen Abmessungen zugeschnitten. Wenn die Breite nach der Größenänderung also beispielsweise kleiner oder gleich der Höhe ist, wird das Bild links und rechts von der Mitte des relevanten Bereichs zugeschnitten. Andernfalls wird das Bild ausgehend von der Mitte des relevanten Bereichs oben und unten zugeschnitten.  
  
 
Die folgende Abbildung zeigt das Bild nach einer Verkleinerung auf 200 x 200 unter Verwendung des Zuschnitts mit intelligentem Seitenverhältnis:  
  
![Landschaftsbild, zugeschnitten auf 200 x 200](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape200x200c7.PNG)
  
Die folgende Abbildung zeigt das Bild nach einer Verkleinerung auf 200 x 100 unter Verwendung des Zuschnitts mit intelligentem Seitenverhältnis:  
   
![Landschaftsbild, zugeschnitten auf 200 x 100](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape200x100c7.PNG)
  
Die folgende Abbildung zeigt das Bild nach einer Verkleinerung auf 100 x 200 unter Verwendung des Zuschnitts mit intelligentem Seitenverhältnis:  
  
![Landschaftsbild, zugeschnitten auf 100 x 200](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape100x200c7.PNG)



Falls Bing den relevanten Bereich des Bilds nicht bestimmen kann, wird der Zuschnitt mit blindem Seitenverhältnis verwendet.  
  
Bei Verwendung des Zuschnitts mit blindem Seitenverhältnis (c = 4) schneidet Bing das Bild nach folgenden Regeln zu:  
  
- (Ursprüngliche Bildbreite / ursprüngliche Bildhöhe) < (angeforderte Bildbreite / angeforderte Bildhöhe): Das Bild wird von der linken oberen Ecke aus gemessen und unten abgeschnitten.  
- (Ursprüngliche Bildbreite / ursprüngliche Bildhöhe) > (angeforderte Bildbreite / angeforderte Bildhöhe): Das Bild wird von der Mitte aus gemessen und links und rechts abgeschnitten.  



Die folgende Abbildung zeigt ein Bild im Hochformat mit den Abmessungen 225 x 300:  
  
![Originalbild einer Sonnenblume](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower.PNG)
  
Die folgende Abbildung zeigt das Bild nach einer Verkleinerung auf 200 x 200 unter Verwendung des Zuschnitts mit blindem Seitenverhältnis: Das Bild wird von der linken oberen Ecke aus gemessen. Das führt dazu, dass der untere Teil des Bilds abgeschnitten wird.  
  
![Bild einer Sonnenblume, zugeschnitten auf 200 x 200](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower200x200c4.PNG)
  
Die folgende Abbildung zeigt das Bild nach einer Verkleinerung auf 200 x 100 unter Verwendung des Zuschnitts mit blindem Seitenverhältnis: Das Bild wird von der linken oberen Ecke aus gemessen. Das führt dazu, dass der untere Teil des Bilds abgeschnitten wird.  
  
![Bild einer Sonnenblume, zugeschnitten auf 200 x 100](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower200x100c4.PNG)
  
Die folgende Abbildung zeigt das Bild nach einer Verkleinerung auf 100 x 200 unter Verwendung des Zuschnitts mit blindem Seitenverhältnis: Das Bild wird von der Mitte aus gemessen. Das führt dazu, dass das Bild links und rechts zugeschnitten wird.  
  
![Bild einer Sonnenblume, zugeschnitten auf 100 x 200](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower100x200c4.PNG)


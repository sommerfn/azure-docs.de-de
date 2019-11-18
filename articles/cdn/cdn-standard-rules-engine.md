---
title: Erzwingen von HTTPS über die Azure CDN-Standardregel-Engine | Microsoft-Dokumentation
description: Mit der Standardregel-Engine können Sie anpassen, wie HTTP-Anforderungen von Azure CDN von Microsoft behandelt werden. So können Sie z. B. die Übermittlung bestimmter Inhaltstypen blockieren, eine Cacherichtlinie definieren oder HTTP-Header ändern.
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 11/01/2019
ms.author: magattus
ms.openlocfilehash: b24c4a04e0c02258a918ee075066d90c22ea0c75
ms.sourcegitcommit: b2fb32ae73b12cf2d180e6e4ffffa13a31aa4c6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73615829"
---
# <a name="azure-cdn-standard-rules-engine"></a>Azure CDN-Standardregel-Engine

> [!NOTE]
> Die Standardregel-Engine ist nur für Azure CDN von Microsoft verfügbar. 

Über die Azure CDN-Standardregel-Engine können Sie anpassen, wie HTTP-Anforderungen verarbeitet werden. Beispielsweise können Sie die Übermittlung bestimmter Inhaltstypen blockieren, eine Cacherichtlinie definieren oder einen HTTP-Header ändern. In diesem Tutorial wird veranschaulicht, wie Sie eine Regel erstellen, mit der Ihre Benutzer automatisch an HTTPS umgeleitet werden. 


## <a name="tutorial"></a>Tutorial

1. Wählen Sie in Azure CDN auf der Seite **CDN-Profil** aus den Microsoft-Profilen den Endpunkt aus, für den Sie Regeln konfigurieren möchten.
  
2. Klicken Sie auf die links angezeigte Registerkarte **Regel-Engine**.
   
    Das Blatt „Regel-Engine“ mit der Regel „Global“ wird geöffnet. 
   
    [![Seite für neue CDN-Regeln](./media/cdn-standard-rules-engine/cdn-new-rule.png)](./media/cdn-standard-rules-engine/cdn-new-rule.png#lightbox)
   
   > [!IMPORTANT]
   > Die Reihenfolge, in der mehrere Regeln aufgelistet sind, beeinflusst deren Verarbeitung. Eine Regel kann die von einer vorherigen Regel angegebenen Aktionen überschreiben.
   >

3. Klicken Sie auf die Schaltfläche **Regel hinzufügen**, und geben Sie einen Regelnamen an. Regelnamen müssen mit einem Buchstaben beginnen und dürfen nur Ziffern und Buchstaben enthalten.

4. Ermitteln Sie die Anforderungstypen, auf die die Regel angewendet wird. Verwenden Sie die Dropdownliste, um die Übereinstimmungsbedingung **Protokoll anfordern** auszuwählen, und verwenden Sie den Wert **Ist gleich** **HTTP**.
   
   [![Übereinstimmungsbedingung für die CDN-Regel](./media/cdn-standard-rules-engine/cdn-match-condition.png)](./media/cdn-standard-rules-engine/cdn-match-condition.png#lightbox)
   
   > [!NOTE]
   > In der Dropdownliste sind mehrere Übereinstimmungsbedingungen verfügbar. Eine ausführliche Liste von Übereinstimmungsbedingungen finden Sie unter [Übereinstimmungsbedingungen der Regel-Engine](cdn-standard-rules-engine-match-conditions.md).
   
5. Wählen Sie die Aktion aus, die auf die identifizierten Anforderungen angewendet wird. Wählen Sie in der Dropdownliste die Aktion **URL-Umleitung** aus, und geben Sie für den Typ den Wert **Gefunden (302)** und als Protokoll **HTTPS** an. Lassen Sie alle weiteren Felder leer, um die eingehenden Werte zu verwenden.
   
   [![CDN-Regelaktion](./media/cdn-standard-rules-engine/cdn-action.png)](./media/cdn-standard-rules-engine/cdn-action.png#lightbox)
   
   > [!NOTE]
   > In der Dropdownliste sind mehrere Aktionen verfügbar. Eine detaillierte Liste der Aktionen finden Sie unter [Aktionen der Regel-Engine](cdn-standard-rules-engine-actions.md).

6. Klicken Sie auf **Speichern**, um die neue Regel zu speichern.  Die neue Regel wird jetzt bereitgestellt.
   
   > [!IMPORTANT]
   > Es kann bis zu 15 Minuten dauern, bis Regeländerungen über Azure CDN verteilt wurden.
   >
   

## <a name="see-also"></a>Weitere Informationen

- [Übersicht über das Azure Content Delivery Network](cdn-overview.md)
- [Referenz zur Standardregel-Engine](cdn-standard-rules-engine-reference.md)
- [Übereinstimmungsbedingungen der Standardregel-Engine](cdn-standard-rules-engine-match-conditions.md)
- [Aktionen der Standardregel-Engine](cdn-standard-rules-engine-actions.md)

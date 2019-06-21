---
title: Azure Front Door Service – URL-Rewrite| Microsoft-Dokumentation
description: In diesem Artikel erläutert, wie Azure Front Door Service die URL-Rewrite-Funktion durchführt, wenn diese konfiguriert wurde.
services: front-door
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: dc2126276e3e8e0d35ce8ed1f835544386659eff
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "60736179"
---
# <a name="url-rewrite-custom-forwarding-path"></a>URL-Rewrite (benutzerdefinierter Weiterleitungspfad)
Azure Front Door Service unterstützt URL-Rewrite, indem es Ihnen ermöglicht, einen optionalen **benutzerdefinierten Weiterleitungspfad** zu konfigurieren, der beim Erstellen der Anforderung zum Weiterleiten an das Back-End verwendet werden soll. Wenn kein benutzerdefinierter Weiterleitungspfad bereitgestellt wird, kopiert Front Door standardmäßig den URL-Eingangspfad in die URL, die in der weitergeleiteten Anforderung verwendet wurde. Der in der weitergeleiteten Anforderung verwendete Hostheader wird für das ausgewählten Back-End konfiguriert. Informationen zu den Aktionen und zur Konfiguration des Hostheaders finden Sie unter [Back-End-Hostheader](front-door-backend-pool.md#hostheader).

Die Stärke von URL-Rewrite anhand eines benutzerdefinierten Weiterleitungspfads besteht darin, dass ein beliebiger Teil des Eingangspfads, der einem Platzhalterpfad entspricht, in den weitergeleiteten Pfad kopiert wird (diese Pfadsegmente sind die **grün** dargestellten Segmente im folgenden Beispiel):
</br>
![URL-Rewrite in Azure Front Door][1]

## <a name="url-rewrite-example"></a>Beispiel für URL-Rewrite
Sehen wir uns eine Routingregel mit folgenden konfigurierten Front-End-Hosts und Pfaden an:

| Host      | Paths       |
|------------|-------------|
| www\.contoso.com | /\*         |
|            | /foo        |
|            | /foo/\*     |
|            | /foo/bar/\* |

Die erste Spalte der folgenden Tabelle enthält Beispiele für eingehende Anforderungen, und die zweite Spalte zeigt, welcher Pfad der Route am genauesten übereinstimmt.  Die dritten und nachfolgenden Spalten der ersten Zeile der Tabelle sind Beispiele für konfigurierte **benutzerdefinierte Weiterleitungspfade**. Die übrigen Zeilen in diesen Spalten zeigen Beispiele dafür, wie der Pfad der weitergeleiteten Anforderung aussähe, wenn er mit der Anforderung in der jeweiligen Zeile abgeglichen würde.

Die zweite Zeile zeigt beispielsweise, dass der weitergeleitete Pfad für die eingehende Anforderung `www.contoso.com/sub` für den benutzerdefinierten Weiterleitungspfad `/` folgendermaßen lauten würde: `/sub`. Wenn der benutzerdefinierte Weiterleitungspfad `/fwd/` wäre, würde der weitergeleitete Pfad `/fwd/sub` lauten. Für die übrigen Spalten gilt dasselbe Prinzip. Die **hervorgehobenen** Teile der folgenden Pfade zeigen den Teil, der mit dem Platzhalter übereinstimmt.


| Eingehende Anforderung       | Genaueste Pfadübereinstimmung | /          | /fwd/          | /foo/          | /foo/bar/          |
|------------------------|--------------------------|------------|----------------|----------------|--------------------|
| www\.contoso.com/            | /\*                      | /          | /fwd/          | /foo/          | /foo/bar/          |
| www\.contoso.com/**sub**     | /\*                      | /**sub**   | /fwd/**sub**   | /foo/**sub**   | /foo/bar/**sub**   |
| www\.contoso.com/**a/b/c**   | /\*                      | /**a/b/c** | /fwd/**a/b/c** | /foo/**a/b/c** | /foo/bar/**a/b/c** |
| www\.contoso.com/foo         | /foo                     | /          | /fwd/          | /foo/          | /foo/bar/          |
| www\.contoso.com/foo/        | /foo/\*                  | /          | /fwd/          | /foo/          | /foo/bar/          |
| www\.contoso.com/foo/**bar** | /foo/\*                  | /**bar**   | /fwd/**bar**   | /foo/**bar**   | /foo/bar/**bar**   |


## <a name="optional-settings"></a>Optionale Einstellungen
Es gibt zusätzliche, optionale Einstellungen, die Sie für alle Routingregeleinstellungen angeben können:

* **Cachekonfiguration**: Wenn diese Option deaktiviert oder nicht angegeben ist, versuchen die mit dieser Routingregel übereinstimmenden Anforderungen nicht, den zwischengespeicherten Inhalt zu verwenden. Stattdessen rufen sie die Inhalte immer aus dem Back-End ab. Informationen hierzu finden Sie unter [Zwischenspeicherung mit Front Door](front-door-caching.md).



## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Erstellen einer Azure Front Door Service-Konfiguration](quickstart-create-front-door.md).
- Informieren Sie sich über die [Funktionsweise von Azure Front Door Service](front-door-routing-architecture.md).

<!--Image references-->
[1]: ./media/front-door-url-rewrite/front-door-url-rewrite-example.jpg
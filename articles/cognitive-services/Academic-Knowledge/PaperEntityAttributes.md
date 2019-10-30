---
title: Attribute der Artikelentität – Academic Knowledge-API
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die Attribute, die Sie für die Artikelentität in der Academic Knowledge-API verwenden können.
services: cognitive-services
author: DarrinEide
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 10/22/2019
ms.author: darrine
ROBOTS: NOINDEX
ms.openlocfilehash: c300a6477daa5759a68d5d11d40b1a71b46bd808
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72793834"
---
# <a name="paper-entity"></a>Artikelentität

<sub> * Die folgenden Attribute sind spezifisch für die Artikelentität. (Ty = '0') </sub>

NAME | BESCHREIBUNG | type | Vorgänge
--- | --- | --- | ---
AA.AfId | ID der Zugehörigkeit des Autors | Int64 | Equals
AA.AfN | Name der Zugehörigkeit des Autors | Zeichenfolge | Equals, StartsWith
AA.AuId | ID des Autors | Int64 | Equals
AA.AuN | Normalisierter Autorname | Zeichenfolge | Equals, StartsWith
AA.DAuN | Ursprünglicher Autorname | Zeichenfolge | Keine
AA.DAfN | Ursprünglicher Name der Zugehörigkeit | Zeichenfolge | Keine
AA.S | Numerische Position in der Autorenliste | Int32 | Equals
CC | Anzahl der Zitate | Int32 | Keine  
C.CId | ID der Konferenzreihe | Int64 | Equals
C.CN | Name der Konferenzreihe | Zeichenfolge | Equals, StartsWith
D | Veröffentlichungsdatum im Format JJJJ-MM-TT | Date | Equals, IsBetween
E | Erweiterte Metadaten (siehe Tabelle unten) | Zeichenfolge | –  
ECC | Geschätzte Zitatanzahl | Int32 | Keine
F.DFN | Ursprünglicher Name für den Forschungsbereich | Zeichenfolge | Keine
F.FId | ID des Forschungsbereichs | Int64 | Equals
F.FN | Normalisierter Name für den Forschungsbereich | Zeichenfolge | Equals, StartsWith
Id | Artikel-ID | Int64 | Equals
J.JId | ID der Publikation | Int64 | Equals
J.JN | Name der Publikation | Zeichenfolge | Equals, StartsWith
Pt | Veröffentlichungstyp (0: unbekannt, 1: Journalartikel, 2: Patent, 3: Konferenzbeitrag, 4: Buchkapitel, 5: Buch, 6: Verweiseintrag in Buch, 7: Dataset, 8: Repository) | Zeichenfolge | Equals
RId | Liste der referenzierten Artikel-IDs | Int64[] | Equals
Ti | Normalisierter Titel | Zeichenfolge | Equals, StartsWith
W | Eindeutige Wörter im Titel | String[] | Equals
J | Veröffentlichungsjahr | Int32 | Equals, IsBetween

## <a name="extended-metadata-attributes"></a>Erweiterte Metadatenattribute ##

NAME | BESCHREIBUNG               
--- | ---
BT | BibTeX-Dokumenttyp („a“: Journalartikel, „b“: Buch, „c“: Buchkapitel, „p“: Konferenzbeitrag)
BV | Name des BibTeX-Veranstaltungsorts
CC | Zitatkontexte – Liste der IDs der referenzierten Artikel und des entsprechenden Kontexts in dem Artikel (z. B. [{123: [„braune Füchse sind für ihre Sprünge bekannt, wie in Dokument 123 referenziert“, „die faulen Hunde sind historisch irreführend, wie in Artikel 123 gezeigt“]})
DN | Ursprünglicher Titel des Artikels
DOI | Digitaler Objektbezeichner
FP | Erste Seite des Artikels in der Veröffentlichung
I | Veröffentlichungsausgabe
IA | Umgekehrte Zusammenfassung
IA.IndexLength | Anzahl der Elemente im Index (Anzahl der Wörter in der Zusammenfassung)
IA.InvertedIndex | Liste der Wörter in der Zusammenfassung und ihre entsprechende Position in der ursprünglichen Zusammenfassung (z. B. [{„der“:[0, 15, 30]}, {„braune“:[1]}, {„Fuchs“:[2]}])
LP | Letzte Seite des Artikels in der Veröffentlichung
PB | Herausgeber
S | Quellen – Liste der Internetquellen des Artikels, sortiert nach statischem Rang
S.Ty | Quellentyp (1:HTML, 2:Text, 3:PDF, 4:DOC, 5:PPT, 6:XLS, 7:PS)
S.U | Quell-URL
V | Veröffentlichung
VFN | Vollständiger Name des Journals oder Veranstaltungsorts der Konferenz
VSN | Kurzname des Journals oder Veranstaltungsorts der Konferenz

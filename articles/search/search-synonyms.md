---
title: 'Synonyme für die Abfrageerweiterung über einen Suchindex: Azure Search'
description: Erstellen einer Synonymzuordnung zum Erweitern des Bereichs einer Suchabfrage für einen Azure Search-Index. Der Bereich wird erweitert, um gleichwertige Begriffe einzubeziehen, die Sie in einer Liste bereitstellen.
author: brjohnstmsft
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 05/02/2019
manager: nitinme
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: a17e2ae5313f9d0b662d343230a04dd3e726c16d
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72331179"
---
# <a name="synonyms-in-azure-search"></a>Synonyme in Azure Search

Synonyme in Suchmaschinen ordnen entsprechende Begriffe zu, die den Bereich einer Abfrage implizit erweitern, ohne dass der Benutzer den Begriff tatsächlich bereitstellen muss. Bei dem Begriff „Hund“ und Synonymzuordnungen von „hündisch“ und „Welpe“ fallen z. B. alle Dokumente, die „Hund“, „hündisch“ oder „Welpe“ enthalten, in den Gültigkeitsbereich der Abfrage.

In Azure Search erfolgt die Synonymerweiterung zur Abfragezeit. Sie können Synonymzuordnungen zu einem Dienst hinzufügen, ohne vorhandene Vorgänge zu stören. Sie können eine **synonymMaps**-Eigenschaft zu einer Felddefinition hinzufügen, ohne den Index neu erstellen zu müssen.

## <a name="create-synonyms"></a>Erstellen von Synonymen

Die Erstellung von Synonymen wird über das Portal nicht unterstützt, Sie können dafür jedoch die REST-API oder das .NET SDK verwenden. Zum Einstieg in REST werden die [Verwendung von Postman](search-get-started-postman.md) und die Erstellung von Anforderungen mit der folgenden API empfohlen: [Create Synonym Map](https://docs.microsoft.com/rest/api/searchservice/create-synonym-map). C#-Entwickler können mit dem [Hinzufügen von Synonymen in Azure Search mit C#](search-synonyms-tutorial-sdk.md) beginnen.

Bei Verwendung von [von Kunden verwalteten Schlüsseln](search-security-manage-encryption-keys.md) für die dienstseitige Verschlüsselung ruhender Daten können Sie diesen Schutz optional auf die Inhalte der Synonymzuordnung anwenden.

## <a name="use-synonyms"></a>Verwenden von Synonymen

In Azure Search basiert die Synonymunterstützung auf Synonymzuordnungen, die Sie definieren und zu Ihrem Dienst hochladen. Diese Zuordnungen bilden eine unabhängige Ressource (wie Indizes oder Datenquellen) und können von jedem durchsuchbaren Feld in einem beliebigen Index in Ihrem Suchdienst verwendet werden.

Synonymzuordnungen und Indizes werden unabhängig voneinander verwaltet. Nachdem Sie eine Synonymzuordnung definiert und zu Ihrem Dienst hochgeladen haben, können Sie die Synonymfunktion für ein Feld aktivieren, indem Sie eine neue Eigenschaft namens **synonymMaps** in der Felddefinition hinzufügen. Das Erstellen, Aktualisieren und Löschen einer Synonymzuordnung betrifft immer das ganze Dokument. Daher können Sie keine Teile der Synonymzuordnung inkrementell erstellen, aktualisieren oder löschen. Selbst das Aktualisieren eines einzelnen Eintrags erfordert das erneute Laden.

Das Integrieren von Synonymen in Ihre Suchanwendung ist ein zweistufiger Prozess:

1.  Fügen Sie Ihrem Suchdienst über die nachfolgenden APIs eine Synonymzuordnung hinzu.  

2.  Konfigurieren Sie ein durchsuchbares Feld, um die Synonymzuordnung in der Indexdefinition zu verwenden.

Sie können mehrere Synonymzuordnungen für Ihre Suchanwendung erstellen (z. B. nach Sprache, wenn Ihre Anwendung einen mehrsprachigen Kundenstamm unterstützt). Derzeit kann ein Feld nur eine der Zuordnungen verwenden. Sie können die „synonymMaps“-Eigenschaft für ein Feld jederzeit aktualisieren.

### <a name="synonymmaps-resource-apis"></a>Ressourcen-APIs von SynonymMaps

#### <a name="add-or-update-a-synonym-map-under-your-service-using-post-or-put"></a>Fügen Sie eine Synonymzuordnung mithilfe von POST oder PUT unter Ihrem Dienst hinzu, oder aktualisieren Sie sie.

Synonymzuordnungen werden über POST oder PUT zum Dienst hochgeladen. Jede Regel muss durch das Zeilenumbruchzeichen („\n“) getrennt werden. Sie können bis zu 5.000 Regeln pro Synonymzuordnung in einem kostenlosen Dienst und 10.000 Regeln in allen anderen SKUs definieren. Jede Regel kann bis zu 20 Erweiterungen aufweisen.

Synonymzuordnungen müssen das unten beschriebene „Apache Solr“-Format aufweisen. Wenn ein vorhandenes Synonymwörterbuch in einem anderen Format vorliegt und sie es direkt verwenden möchten, können Sie uns über [UserVoice](https://feedback.azure.com/forums/263029-azure-search) informieren.

Sie können eine neue Synonymzuordnung mithilfe von HTTP POST erstellen, wie im folgenden Beispiel:

    POST https://[servicename].search.windows.net/synonymmaps?api-version=2019-05-06
    api-key: [admin key]

    {
       "name":"mysynonymmap",
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

Alternativ können Sie PUT verwenden und den Namen der Synonymzuordnung für den URI angeben. Wenn die Synonymzuordnung nicht vorhanden ist, wird sie erstellt.

    PUT https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2019-05-06
    api-key: [admin key]

    {
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

##### <a name="apache-solr-synonym-format"></a>Apache Solr-Synonymformat

Das Solr-Format unterstützt vergleichbare und explizite Synonymzuordnungen. Zuordnungsregeln entsprechen den Open-Source-Synonymfilterspezifikation von Apache Solr, die im folgenden Dokument beschrieben ist: [SynonymFilter](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions#FilterDescriptions-SynonymFilter). Im folgenden finden Sie eine Beispielregel für vergleichbare Synonyme.
```
USA, United States, United States of America
```

Mit der obigen Regel wird die Suchabfrage „USA“ zu „USA“ ODER „Vereinigte Staaten“ ODER „Vereinigte Staaten von Amerika“ erweitert.

Eine explizite Zuordnung wird durch einen Pfeil gekennzeichnet „=>“. Eine Begriffsfolge (sofern angegeben) einer Suchabfrage, die den linken Teil von „=>“ vergleicht, wird durch die Alternativen auf der rechten Seite ersetzt. Bei der nachfolgenden Regel werden die Suchabfragen „Washington“, „Wash.“ oder „WA“ alle in „WA“ umgeschrieben. Die explizite Zuordnung gilt nur für die angegebene Richtung und schreibt in diesem Fall die Abfrage „WA“ nicht zu „Washington“ um.
```
Washington, Wash., WA => WA
```

#### <a name="list-synonym-maps-under-your-service"></a>Listen Sie Synonymzuordnungen unter Ihrem Dienst auf.

    GET https://[servicename].search.windows.net/synonymmaps?api-version=2019-05-06
    api-key: [admin key]

#### <a name="get-a-synonym-map-under-your-service"></a>Rufen Sie eine Synonymzuordnung unter Ihrem Dienst ab.

    GET https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2019-05-06
    api-key: [admin key]

#### <a name="delete-a-synonyms-map-under-your-service"></a>Löschen Sie eine Synonymzuordnung unter Ihrem Dienst.

    DELETE https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2019-05-06
    api-key: [admin key]

### <a name="configure-a-searchable-field-to-use-the-synonym-map-in-the-index-definition"></a>Konfigurieren Sie ein durchsuchbares Feld, um die Synonymzuordnung in der Indexdefinition zu verwenden.

Es kann die neue **synonymMaps**-Feldeigenschaft verwendet werden, um eine Synonymzuordnung anzugeben, die für ein durchsuchbares Feld verwendet werden soll. Synonymzuordnungen sind Ressourcen der Dienstebene. Jedes Feld eines Index unter dem Dienst kann auf die Zuordnungen verweisen.

    POST https://[servicename].search.windows.net/indexes?api-version=2019-05-06
    api-key: [admin key]

    {
       "name":"myindex",
       "fields":[
          {
             "name":"id",
             "type":"Edm.String",
             "key":true
          },
          {
             "name":"name",
             "type":"Edm.String",
             "searchable":true,
             "analyzer":"en.lucene",
             "synonymMaps":[
                "mysynonymmap"
             ]
          },
          {
             "name":"name_jp",
             "type":"Edm.String",
             "searchable":true,
             "analyzer":"ja.microsoft",
             "synonymMaps":[
                "japanesesynonymmap"
             ]
          }
       ]
    }

**synonymMaps** kann für durchsuchbare Felder vom Typ „Edm.String“ oder „Collection(Edm.String)“ angegeben werden.

> [!NOTE]
> Sie können nur eine Synonymzuordnung pro Feld verwenden. Wenn Sie mehrere Synonymzuordnungen verwenden möchten, können Sie uns über [UserVoice](https://feedback.azure.com/forums/263029-azure-search) informieren.

## <a name="impact-of-synonyms-on-other-search-features"></a>Auswirkungen von Synonymen auf andere Suchfeatures

Das Feature „Synonyme“ ändert die ursprüngliche Abfrage mithilfe von Synonymen mit dem OR-Operator. Aus diesem Grund werden der ursprüngliche Begriff und Synonyme von der Treffermarkierung und von Bewertungsprofilen als gleichwertig behandelt.

Das Feature „Synonyme“ gilt für Suchabfragen und nicht für Filter oder Facets. Ebenso basieren Vorschläge nur auf dem ursprünglichen Begriff. Synonymübereinstimmungen werden in der Antwort nicht angezeigt.

Die Synonymerweiterungen gelten nicht für Platzhaltersuchbegriffe. Präfix-, Fuzzy- und Regex-Begriffe werden nicht erweitert.

Wenn Sie eine einzelne Abfrage, die eine Synonymerweiterung sowie Platzhalter-, Regex- und Fuzzysuche umfasst, durchführen möchten, können Sie die Abfragen mit der OR-Syntax kombinieren. Um beispielsweise Synonyme mit Platzhaltern für eine einfache Abfragesyntax zu kombinieren, lautet der Begriff `<query> | <query>*`.

Wenn Sie über einen vorhandenen Index in einer Entwicklungsumgebung (nicht Produktionsumgebung) verfügen, experimentieren Sie mit einem kleinen Wörterbuch, um zu prüfen, wie sich das Hinzufügen von Synonymen auf die Suche auswirkt, einschließlich der Auswirkungen auf Bewertungsprofile, Treffermarkierungen und Vorschläge.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Synonymzuordnung](https://docs.microsoft.com/rest/api/searchservice/create-synonym-map)
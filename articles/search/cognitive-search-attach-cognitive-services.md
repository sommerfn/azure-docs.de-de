---
title: Anfügen einer Cognitive Services-Ressource an ein Skillset
titleSuffix: Azure Cognitive Search
description: Anweisungen zum Anfügen eines Cognitive Services All-in-One-Abonnements an eine KI-Anreicherungspipeline in der kognitiven Azure-Suche.
manager: nitinme
author: LuisCabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 11ca5f71cb0d08a4bebf72407035a9557c794f9f
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72788026"
---
# <a name="attach-a-cognitive-services-resource-to-a-skillset-in-azure-cognitive-search"></a>Anfügen einer Cognitive Services-Ressource an ein Skillset in der kognitiven Azure-Suche 

KI-Algorithmen steuern die [Anreicherungspipelines](cognitive-search-concept-intro.md), die für die Inhaltstransformation in der kognitiven Azure-Suche verwendet werden. Diese Algorithmen basieren auf Azure Cognitive Services-Ressourcen, einschließlich [Maschinelles Sehen](https://azure.microsoft.com/services/cognitive-services/computer-vision/) für Bildanalyse und optische Zeichenerkennung (OCR) und [Textanalyse](https://azure.microsoft.com/services/cognitive-services/text-analytics/) für Entitätserkennung, Schlüsselbegriffserkennung und andere Anreicherungen. Bei Verwendung in der kognitiven Azure-Suche zu Dokumentanreicherungszwecken werden die Algorithmen in einem *Sill* umschlossen, in einem *Skillset* platziert und bei der Indizierung durch einen *Indexer* referenziert.

Sie können eine begrenzte Anzahl von Dokumenten kostenlos anreichern. Oder Sie können eine abrechenbare Cognitive Services-Ressource an ein *Skillset* für größere und häufigere Workloads anfügen. In diesem Artikel erfahren Sie, wie Sie eine abrechenbare Cognitive Services-Ressource anfügen, um Dokumente während der [Indizierung](search-what-is-an-index.md) der kognitiven Azure-Suche anzureichern.

> [!NOTE]
> Abrechenbare Ereignisse umfassen Aufrufe von Cognitive Services-APIs und die Bildextraktion als Teil der Dokumententschlüsselungsphase in der kognitiven Azure-Suche. Für die Textextraktion aus Dokumenten oder für Qualifikationen, über die keine Cognitive Services aufgerufen werden, fallen keine Gebühren an.
>
> Die Ausführung abrechenbarer Qualifikationen erfolgt nach dem [nutzungsbasierten Preis für Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/). Die Preise für die Bildextraktion finden Sie in der [Preisübersicht für die kognitive Azure-Suche](https://go.microsoft.com/fwlink/?linkid=2042400).

## <a name="same-region-requirement"></a>Anforderung derselben Region

Die kognitive Azure-Suche und Azure Cognitive Services müssen in derselben Region vorhanden sein. Andernfalls wird zur Laufzeit die folgende Meldung angezeigt: `"Provided key is not a valid CognitiveServices type key for the region of your search service."` 

Es gibt keine Möglichkeit, einen Dienst zwischen Regionen zu verschieben. Wenn diese Fehlermeldung angezeigt wird, sollten Sie eine neue Cognitive Services-Ressource in der gleichen Region wie die kognitive Azure-Suche erstellen.

> [!NOTE]
> Einige integrierte Qualifikationen basieren auf nicht regionalen Cognitive Services (etwa die [Qualifikation „Textübersetzung“](cognitive-search-skill-text-translation.md)). Wenn Sie Ihrem Skillset eine dieser Skills hinzufügen, ist nicht garantiert, dass die Daten die Region der Ressource der kognitiven Azure-Suche oder der Cognitive Services-Ressource nicht verlassen. Ausführlichere Informationen finden Sie unter [Verfügbare Produkte nach Region](https://aka.ms/allinoneregioninfo).

## <a name="use-free-resources"></a>Verwenden kostenloser Ressourcen

Sie können eine eingeschränkte, kostenlose Verarbeitungsoption verwenden, um das Tutorial zur KI-Anreicherung und die Schnellstartübungen abzuschließen.

Kostenlose Ressourcen (begrenzte Anreicherung) sind auf 20 Dokumente pro Tag pro Abonnement begrenzt.

1. Öffnen Sie den Assistenten „Daten importieren“:

   ![Öffnen des Assistenten „Daten importieren“](media/search-get-started-portal/import-data-cmd.png "Öffnen des Assistenten „Daten importieren“")

1. Wählen Sie eine Datenquelle aus, und fahren Sie mit **Add AI enrichment (Optional)** (KI-Anreicherung hinzufügen (optional)) fort. Eine ausführliche Anleitung für diesen Assistenten finden Sie unter [Erstellen eines Index im Azure-Portal](search-get-started-portal.md).

1. Erweitern Sie **Cognitive Services-Instanz anfügen**, und wählen Sie dann **Free (begrenzte Anreicherung)** aus:

   ![Erweiterter Abschnitt „Cognitive Services-Instanz anfügen“](./media/cognitive-search-attach-cognitive-services/attach1.png "Erweiterter Abschnitt „Cognitive Services-Instanz anfügen“")

1. Sie können nun mit den nächsten Schritten fortfahren, einschließlich **Hinzufügen von kognitiven Skills**.

## <a name="use-billable-resources"></a>Verwenden abrechenbarer Ressourcen

Für Workloads, bei denen täglich mehr als 20 Anreicherungen erstellt werden, müssen Sie eine abrechenbare Cognitive Services-Ressource anfügen. Es wird empfohlen, immer eine abrechenbare Cognitive Services-Ressource anzufügen, auch wenn Sie keine Cognitive Services-APIs aufrufen möchten. Durch Anfügen einer Ressource wird das tägliche Limit außer Kraft gesetzt.

Ihnen werden nur Qualifikationen in Rechnung gestellt, mit denen die Cognitive Services-APIs aufgerufen werden. [Benutzerdefinierte Qualifikationen](cognitive-search-create-custom-skill-example.md) oder Qualifikationen wie [Textzusammenführung](cognitive-search-skill-textmerger.md), [Textteilung](cognitive-search-skill-textsplit.md) und [Shaper](cognitive-search-skill-shaper.md), die nicht API-basiert sind, werden Ihnen nicht in Rechnung gestellt.

1. Öffnen Sie den Assistenten „Daten importieren“, wählen Sie eine Datenquelle aus, und fahren Sie mit **Add AI enrichment (Optional)** (KI-Anreicherung hinzufügen (optional)) fort.

1. Erweitern Sie **Cognitive Services-Instanz anfügen**, und wählen Sie dann **Neue Cognitive Services-Ressource erstellen** aus. Eine neue Registerkarte wird geöffnet, sodass Sie die Ressource erstellen können:

   ![Erstellen einer Cognitive Services-Ressource](./media/cognitive-search-attach-cognitive-services/cog-services-create.png "Erstellen einer Cognitive Services-Ressource")

1. Wählen Sie in der Liste **Standort** die Region aus, in der sich der Dienst der kognitiven Azure-Suche befindet. Achten Sie darauf, diese Region aus Gründen der Leistungsoptimierung zu verwenden. Durch Verwendung dieser Region entfallen auch die Gebühren für regionsübergreifende ausgehende Bandbreite.

1. Wählen Sie in der Liste **Tarif** den Tarif **S0** aus, um die gesamte Sammlung der Cognitive Services-Funktionen einschließlich der Funktionen für Bildanalyse und Sprache zu erhalten, die die in der kognitiven Azure-Suche enthaltenen integrierten Skills unterstützen.

   Für den Tarif „S0“ finden Sie Sätze für bestimmte Arbeitsauslastungen auf der Seite [Cognitive Services – Preise](https://azure.microsoft.com/pricing/details/cognitive-services/).
  
   + Stellen Sie sicher, dass in der Liste **Wählen Sie ein Angebot aus** die Option **Cognitive Services** ausgewählt ist.
   + Die Preise für **Textanalysen Standard** unter den Funktionen für **Sprache** gelten für die KI-Indizierung.
   + Die Preise für **Maschinelles Sehen S1** sind unter den Funktionen für **Bildanalyse** aufgeführt.

1. Wählen Sie **Erstellen** aus, um die neue Cognitive Services-Ressource bereitzustellen.

1. Kehren Sie zur vorherigen Registerkarte mit dem Assistenten „Daten importieren“ zurück. Wählen Sie **Aktualisieren** aus, um die Cognitive Services-Ressource anzuzeigen, und wählen Sie dann die Ressource aus:

   ![Auswählen der Cognitive Services-Ressource](./media/cognitive-search-attach-cognitive-services/attach2.png "Auswählen der Cognitive Services-Ressource")

1. Erweitern Sie den Abschnitt **Add cognitive skills** (Kognitive Skills hinzufügen), um die spezifischen kognitiven Skills auszuwählen, die für die Daten ausgeführt werden sollen. Führen Sie dann die restlichen Schritte des Assistenten aus.

## <a name="attach-an-existing-skillset-to-a-cognitive-services-resource"></a>Anfügen einer vorhandenen Qualifikationsgruppe an eine Cognitive Services-Ressource

Wenn Sie bereits über eine Qualifikationsgruppe verfügen, können Sie diese an eine neue oder andere Cognitive Services-Ressource anfügen.

1. Wählen Sie auf der Seite **Dienstübersicht** die Registerkarte **Qualifikationsgruppen** aus:

   ![Registerkarte „Skillsets“](./media/cognitive-search-attach-cognitive-services/attach-existing1.png "Registerkarte „Skillsets“")

1. Wählen Sie den Namen der Qualifikationsgruppe und dann eine vorhandene Ressource aus, oder erstellen Sie eine neue Ressource. Wählen Sie zum Bestätigen Ihrer Änderungen **OK** aus.

   ![Liste der Skillsetressourcen](./media/cognitive-search-attach-cognitive-services/attach-existing2.png "Liste der Skillsetressourcen")

   Denken Sie daran, dass die Option **Free (begrenzte Anreicherung)** auf 20 Dokumente täglich beschränkt ist und dass Sie **Neue Cognitive Services-Ressource erstellen** zum Bereitstellen einer neuen abrechenbaren Ressource verwenden können. Wenn Sie eine neue Ressource erstellen, wählen Sie **Aktualisieren** aus, um die Liste der Cognitive Services-Ressourcen zu aktualisieren, und wählen Sie dann die Ressource aus.

## <a name="attach-cognitive-services-programmatically"></a>Programmgesteuertes Anfügen von Cognitive Services

Wenn Sie die Qualifikationsgruppe programmgesteuert definieren, fügen Sie einen `cognitiveServices`-Abschnitt zur Qualifikationsgruppe hinzu. Nehmen Sie in diesen Abschnitt den Schlüssel der Cognitive Services-Ressource auf, die Sie der Qualifikationsgruppe zuordnen möchten. Denken Sie daran, dass sich die Ressource in derselben Region wie die Ressource der kognitiven Azure-Suche befinden muss. Nehmen Sie außerdem `@odata.type` auf, und legen Sie ihn auf `#Microsoft.Azure.Search.CognitiveServicesByKey` fest.

Das folgende Beispiel zeigt dieses Muster. Beachten Sie den Abschnitt `cognitiveServices` am Ende der Definition.

```http
PUT https://[servicename].search.windows.net/skillsets/[skillset name]?api-version=2019-05-06
api-key: [admin key]
Content-Type: application/json
```
```json
{
    "name": "skillset name",
    "skills": 
    [
      {
        "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
        "categories": [ "Organization" ],
        "defaultLanguageCode": "en",
        "inputs": [
          {
            "name": "text", "source": "/document/content"
          }
        ],
        "outputs": [
          {
            "name": "organizations", "targetName": "organizations"
          }
        ]
      }
    ],
    "cognitiveServices": {
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
        "description": "mycogsvcs",
        "key": "<your key goes here>"
    }
}
```

## <a name="example-estimate-costs"></a>Beispiel: Schätzen der Kosten

Zum Schätzen der Kosten für die Indizierung der kognitiven Suche gehen Sie von einem durchschnittlichen Dokument aus, damit einige Zahlen zur Verfügung stehen. Beispielsweise kann Folgendes angenommen werden:

+ 1\.000 PDFs
+ Jeweils sechs Seiten
+ Ein Bild pro Seite (6.000 Bilder)
+ 3\.000 Zeichen pro Seite

Als Beispiel dient eine Pipeline, die Dokumententschlüsselung jeder PDF-Datei, Bild- und Textextraktion, optische Zeichenerkennung (OCR) von Bildern und die Erkennung von Entitäten für Organisationen umfasst.

Die in diesem Artikel angegebenen Preise sind hypothetisch. Sie dienen der Veranschaulichung des Schätzungsprozesses. Ihre Kosten können niedriger ausfallen. Die tatsächlichen Preise von Transaktionen finden Sie unter [Cognitive Services – Preise](https://azure.microsoft.com/pricing/details/cognitive-services).

1. Im Fall der Dokumententschlüsselung mit Text- und Bildinhalten ist die Textextraktion derzeit kostenlos. Bei 6.000 Bildern und einem angenommenen Preis von 1 € für jeweils 1.000 extrahierte Bilder belaufen sich die Kosten auf 6,00 € für diesen Schritt.

2. Für die optische Zeichenerkennung (OCR) von 6.000 Bildern in Englisch verwendet die kognitive Qualifikation „OCR“ den besten Algorithmus (DescribeText). Angenommen, jeweils 1.000 zu analysierende Bilder kosten 2,50 €, dann würden für diesen Schritt 15,00 € anfallen.

3. Für die Extraktion von Entitäten würden insgesamt 3 Textdatensätze pro Seite entstehen. Jeder Datensatz umfasst 1.000 Zeichen. Drei Textdatensätze pro Seite multipliziert mit 6.000 Seiten ergibt 18.000 Textdatensätze. Angenommen, die Kosten betragen 2,00 €/1.000 Textdatensätze, dann würde dieser Schritt 36,00 € kosten.

Insgesamt würden Sie also etwa 57,00 € zahlen, um 1.000 PDF-Dokumente dieser Art mit der beschriebenen Qualifikationsgruppe zu erfassen.

## <a name="next-steps"></a>Nächste Schritte
+ [Preisübersicht für die kognitive Azure-Suche](https://azure.microsoft.com/pricing/details/search/)
+ [Definieren eines Skillsets](cognitive-search-defining-skillset.md)
+ [Erstellen eines Skillsets (REST)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Zuordnen angereicherter Felder](cognitive-search-output-field-mapping.md)

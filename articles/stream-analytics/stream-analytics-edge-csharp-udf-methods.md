---
title: Entwickeln von .NET Standard-Funktionen für Azure Stream Analytics-Aufträge (Vorschau)
description: Erfahren Sie, wie Sie benutzerdefinierte C#-Funktionen für Stream Analytics-Aufträge schreiben.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
manager: kfile
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 10/28/2019
ms.custom: seodec18
ms.openlocfilehash: 0bc59ac3e55466f8ac06a3a8fa9cf08fecbb5ce3
ms.sourcegitcommit: d47a30e54c5c9e65255f7ef3f7194a07931c27df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73024948"
---
# <a name="develop-net-standard-user-defined-functions-for-azure-stream-analytics-jobs-preview"></a>Entwickeln von benutzerdefinierten .NET Standard-Funktionen für Azure Stream Analytics-Aufträge (Vorschau)

Azure Stream Analytics bietet eine SQL-ähnliche Abfragesprache zum Durchführen von Transformationen und Berechnungen über Datenströme von Ereignisdaten. Es gibt viele integrierte Funktionen, für einige komplexere Szenarien ist jedoch zusätzliche Flexibilität erforderlich. Mit benutzerdefinierten Funktionen (UDF) in .NET Standard können Sie Ihre eigenen Funktionen in einer beliebigen .NET Standard-Sprache (C#, F# usw.) schreiben, um die Stream Analytics-Abfragesprache zu erweitern. UDFs ermöglichen das Durchführen komplexer mathematischer Berechnungen, das Importieren benutzerdefinierter ML-Modelle mit ML.NET und das Verwenden von benutzerdefinierter eingefügter Logik für die fehlenden Daten. Das UDF-Feature für Stream Analytics-Aufträge befindet sich derzeit in der Vorschauphase und sollte nicht für Produktionsworkloads verwendet werden.

Das .NET-UDF-Feature für Cloudaufträge ist in den folgenden Regionen verfügbar:
* USA, Westen-Mitte (verfügbar)
* Europa, Norden (verfügbar)
* USA, Osten (verfügbar)
* USA, Westen (Rollout in Kürze)
* USA, Osten 2 (Rollout in Kürze)
* Europa, Westen (Rollout in Kürze)

Wenn Sie dieses Feature in einer anderen Region verwenden möchten, können Sie den [Zugriff anfordern](https://aka.ms/ccodereqregion).

## <a name="overview"></a>Übersicht
Visual Studio-Tools für Azure Stream Analytics vereinfachen das Verfassen benutzerdefinierter Funktionen, das lokale Testen Ihrer Aufträge (sogar offline) und das Veröffentlichen Ihres Stream Analytics-Auftrags in Azure. Nach der Veröffentlichung in Azure können Sie den Auftrag auf IoT-Geräten mit IoT Hub bereitstellen.

Zum Implementieren von UDFs stehen drei Methoden zur Auswahl:

* CodeBehind-Dateien in einem ASA-Projekt
* Benutzerdefinierte Funktion aus einem lokalen Projekt
* Vorhandenes Paket aus einem Azure Storage-Konto

## <a name="package-path"></a>Paketpfad

Der Pfad aller UDF-Pakete hat das Format `/UserCustomCode/CLR/*`. Dynamic Link Libraries (DLLs) und Ressourcen werden in den Ordner `/UserCustomCode/CLR/*` kopiert. Damit werden die Benutzer-DLLs von den System- und Azure Stream Analytics-DLLs isoliert. Dieser Paketpfad wird für alle Funktionen genutzt, unabhängig davon, mit welcher Methode sie verwendet werden.

## <a name="supported-types-and-mapping"></a>Unterstützte Typen und Zuordnungen

|**UDF-Typ (C#)**  |**Azure Stream Analytics-Typ**  |
|---------|---------|
|long  |  bigint   |
|double  |  double   |
|Zeichenfolge  |  nvarchar(max)   |
|dateTime  |  dateTime   |
|struct  |  IRecord   |
|object  |  IRecord   |
|Array\<Objekt>  |  IArray   |
|dictionary<Zeichenfolge, Objekt>  |  IRecord   |

## <a name="codebehind"></a>CodeBehind
Sie können benutzerdefinierte Funktionen im CodeBehind **Script.asql** schreiben. Visual Studio-Tools kompilieren automatisch die CodeBehind-Datei in eine Assemblydatei. Die Assemblys werden als ZIP-Datei verpackt und in Ihr Speicherkonto hochgeladen, wenn Sie Ihren Auftrag an Azure übermitteln. Im Tutorial [C#-UDF für Stream Analytics-Edge-Aufträge](stream-analytics-edge-csharp-udf.md) erfahren Sie, wie Sie eine UDF in C# mit CodeBehind schreiben. 

## <a name="local-project"></a>Lokales Projekt
Benutzerdefinierte Funktionen können in einer Assembly geschrieben werden, auf die später in einer Azure Stream Analytics-Abfrage verwiesen wird. Dies ist die empfohlene Option für komplexe Funktionen, die das ganze Potenzial einer .NET Standard-Sprache über die Ausdruckssprache erfordern, z.B. bei prozeduraler Logik oder Rekursion. Sie können benutzerdefinierte Funktionen aus einem lokalen Projekt auch verwenden, wenn Sie die Logik der Funktion für mehrere Azure Stream Analytics-Abfragen verwenden möchten. Durch das Hinzufügen von benutzerdefinierten Funktionen zu Ihrem lokalen Projekt haben Sie die Möglichkeit, Ihre Funktionen lokal in Visual Studio zu debuggen und zu testen.

So verweisen Sie auf ein lokales Projekt

1. Erstellen Sie eine neue Klassenbibliothek in Ihrer Projektmappe.
2. Schreiben Sie Code in Ihrer Klasse. Beachten Sie, dass die Klassen als *öffentlich* und die Objekte als *statisch öffentlich* definiert werden müssen. 
3. Erstellen Sie Ihr Projekt. Die Tools verpacken alle Artefakte im Papierkorbordner als ZIP-Datei und laden die ZIP-Datei im Speicherkonto hoch. Verwenden Sie für externe Referenzen die Assemblyreferenz anstelle des NuGet-Pakets.
4. Verweisen Sie in Ihrem Azure Stream Analytics-Projekt auf die neue Klasse.
5. Fügen Sie in Ihrem Azure Stream Analytics-Projekt eine neue Funktion hinzu.
6. Konfigurieren Sie den Pfad der Assembly in der Konfigurationsdatei des Auftrags (`JobConfig.json`). Legen Sie den Assemblypfad auf **Verweis auf lokales Projekt oder CodeBehind** fest.
7. Erstellen Sie sowohl das Funktionsprojekt als auch das Azure Stream Analytics-Projekt neu.  

### <a name="example"></a>Beispiel

In diesem Beispiel ist **UDFTest** ein C#-Klassenbibliotheksprojekt, und **ASAUDFDemo** ist das Azure Stream Analytics-Projekt, das auf **UDFTest** verweist.

![Azure Stream Analytics-IoT Edge-Projekt in Visual Studio](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-demo.png)

1. Erstellen Sie Ihr C#-Projekt, mit dem Sie einen Verweis auf Ihre benutzerdefinierte C#-Funktion aus der Azure Stream Analytics-Abfrage hinzufügen können.
    
   ![Erstellen eines Azure Stream Analytics-IoT Edge-Projekts in Visual Studio](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-build-project.png)

2. Fügen Sie den Verweis auf das C#-Projekt im ASA-Projekt hinzu. Klicken Sie mit der rechten Maustaste auf den Knoten „Verweise“, und wählen Sie „Verweis hinzufügen“ aus.

   ![Hinzufügen eines Verweises zu einem C#-Projekt in Visual Studio](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-add-reference.png)

3. Wählen Sie in der Liste den Namen des C#-Projekts aus. 
    
   ![Auswählen des Namens des C#-Projekts in der Verweisliste](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-choose-project-name.png)

4. **UDFTest** sollte unter **Verweise** im **Projektmappen-Explorer** aufgeführt werden.

   ![Anzeigen des Verweises auf die benutzerdefinierte Funktion im Projektmappen-Explorer](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-added-reference.png)

5. Klicken Sie mit der rechten Maustaste auf den Ordner **Funktionen**, und wählen Sie **Neues Element** aus.

   ![Hinzufügen neuer Elemente zu Funktionen in der Projektmappe zum Azure Stream Analytics-Edge](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-add-csharp-function.png)

6. Fügen Sie Ihrem Azure Stream Analytics-Projekt die C#-Funktion **SquareFunction.json** hinzu.

   ![Auswählen der C#-Funktion aus den Stream Analytics-Edgeelementen in Visual Studio](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-add-csharp-function-2.png)

7. Doppelklicken Sie im **Projektmappen-Explorer** auf die Funktion, um das Konfigurationsdialogfeld zu öffnen.

   ![Konfiguration der C#-Funktion in Visual Studio](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-csharp-function-config.png)

8. Wählen Sie in der Konfiguration der C#-Funktion **Aus ASA-Projektverweis laden** und dann in der Dropdownliste die zugehörigen Assembly-, Klassen- und Methodennamen aus. Für Verweise auf die Methoden, Typen und Funktionen in der Stream Analytics-Abfrage müssen die Klassen als *öffentlich* und die Objekte als *statisch öffentlich* definiert sein.

   ![Konfiguration der Stream Analytics-C#-Funktion](./media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-asa-csharp-function-config.png)

## <a name="existing-packages"></a>Vorhandene Pakete

Sie können .NET Standard-UDFs in einer IDE Ihrer Wahl erstellen und sie dann über Ihre Azure Stream Analytics-Abfrage aufrufen. Kompilieren Sie zunächst Ihren Code, und packen Sie alle DLLs. Der Pfad des Pakets hat das Format `/UserCustomCode/CLR/*`. Laden Sie dann `UserCustomCode.zip` in das Stammverzeichnis des Containers in Ihrem Azure Storage-Konto hoch.

Nachdem die ZIP-Pakete der Assembly in Ihr Azure Storage-Konto hochgeladen wurden, können Sie die Funktionen in Azure Stream Analytics-Abfragen verwenden. Sie müssen lediglich die Storage-Informationen in der Konfiguration des Stream Analytics-Auftrags einfügen. Sie können die Funktion mit dieser Option nicht lokal testen, da Ihr Paket von den Visual Studio-Tools nicht herunterladen wird. Der Paketpfad wird vom Dienst analysiert. 

Konfigurieren Sie den Assemblypfad in der Konfigurationsdatei des Auftrags (`JobConfig.json`).

Erweitern Sie den Abschnitt mit der **benutzerdefinierten Codekonfiguration**, und geben Sie die folgenden vorgeschlagenen Werte für die Konfiguration an:

   |**Einstellung**|**Empfohlener Wert**|
   |-------|---------------|
   |Global Storage Settings Resource (Ressource für globale Speichereinstellungen)|Datenquelle aus aktuellem Konto wählen|
   |Global Storage Settings Subscription (Abonnement für globale Speichereinstellungen)| <Ihr Abonnement>|
   |Global Storage Settings Storage Account (Speicherkonto für globale Speichereinstellungen)| <Ihr Speicherkonto>|
   |Custom Code Storage Settings Resource (Ressource für Speichereinstellungen für benutzerdefinierten Code)|Datenquelle aus aktuellem Konto wählen|
   |Custom Code Storage Settings Storage Account (Speicherkonto für Speichereinstellungen für benutzerdefinierten Code)|<Ihr Speicherkonto>|
   |Custom Code Storage Settings Container (Container für Speichereinstellungen für benutzerdefinierten Code)|<Ihr Speichercontainer>|
   |Custom Code Assembly Source (Assemblyquelle für benutzerdefinierten Code)|Vorhandene Assemblypakete aus der Cloud|
   |Custom Code Assembly Source (Assemblyquelle für benutzerdefinierten Code)|UserCustomCode.zip|

## <a name="limitations"></a>Einschränkungen
Für die Vorschauversion für benutzerdefinierte Funktionen gelten derzeit folgende Einschränkungen:

* .NET Standard-UDFs können nur in Visual Studio erstellt und in Azure veröffentlicht werden. Schreibgeschützte Versionen von .NET Standard-UDFs können im Azure-Portal unter **Funktionen** angezeigt werden. Das Erstellen von .NET Standard-Funktionen wird im Azure-Portal nicht unterstützt.

* Der Abfrage-Editor im Azure-Portal zeigt einen Fehler an, wenn Sie eine .NET Standard-UDF im Portal verwenden. 

* Da der benutzerdefinierte Code den Kontext mit der Azure Stream Analytics-Engine gemeinsam nutzt, kann benutzerdefinierter Code nicht auf Elemente verweisen, deren Namespace oder DLL-Name in Konflikt mit Azure Stream Analytics-Code steht. Sie können beispielsweise nicht auf *Newtonsoft Json* verweisen.

## <a name="next-steps"></a>Nächste Schritte

* [Tutorial: Schreiben einer benutzerdefinierten C#-Funktion für einen Azure Stream Analytics-Auftrag (Vorschauversion)](stream-analytics-edge-csharp-udf.md)
* [Tutorial: Azure Stream Analytics – benutzerdefinierte JavaScript-Funktionen](stream-analytics-javascript-user-defined-functions.md)
* [Anzeigen von Azure Stream Analytics-Aufträgen mit Visual Studio](stream-analytics-vs-tools.md)

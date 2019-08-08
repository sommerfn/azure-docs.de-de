---
title: 'Tutorial: Erstellen einer Entitätsverknüpfungs-App – C#'
titlesuffix: Azure Cognitive Services
description: Analysieren Sie benannte Entitäten für Text und Verknüpfungen auf relevante Einträge in einer Wissensdatenbank mithilfe der Entitätsverknüpfungs-API.
services: cognitive-services
author: DavidLiCIG
manager: nitinme
ms.service: cognitive-services
ms.subservice: entity-linking-intelligence
ms.topic: tutorial
ms.date: 07/06/2016
ms.author: davl
ROBOTS: NOINDEX
ms.openlocfilehash: 4b283103920230a0d2aae98c83f75fb03679a675
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68706808"
---
# <a name="tutorial-build-an-entity-linking-app-with-c"></a>Tutorial: Erstellen einer Entitätsverknüpfungs-App mit C#

Der Entity Linking-Analysedienst von Microsoft ist ein Tool zur Verarbeitung natürlicher Sprache, um nach Text und Verknüpfungen benannte Entitäten auf relevante Einträge in einer Wissensdatenbank zu analysieren. 

In diesem Tutorial wird die Entitätsverknüpfung mit der Clientbibliothek des Entity Linking-Analysediensts als NuGet-Paket erläutert. 

### <a name="Prerequisites">Voraussetzungen</a>

- Visual Studio 2015
- Ein API-Schlüssel von Microsoft Cognitive Services
- Abrufen der Clientbibliothek und des Beispiels
- NuGet-Paket des Entity Linking-Analysediensts von Microsoft

Sie können die Clientbibliothek der API für den Entity Linking-Analysedienst über das [SDK](https://www.github.com/microsoft/cognitive-entitylinking-windows) herunterladen. Die heruntergeladene ZIP-Datei muss in einen Ordner Ihrer Wahl extrahiert werden. Viele Benutzer wählen hierfür den Visual Studio 2015-Ordner.

### <a name="step-1-subscribe-entity-linking-intelligence-service-and-get-your-own-key">Schritt 1: Abonnieren des Entity Linking-Analysediensts und Abrufen Ihres Schlüssels</a>
Bevor Sie den Entity Linking-Analysedienst verwenden, müssen Sie sich für einen API-Schlüssel registrieren. Weitere Informationen finden Sie unter [Cognitive Services](https://www.microsoft.com/cognitive-services/en-us/sign-up). In diesem Tutorial können sowohl der Primär- als auch der Sekundärschlüssel verwendet werden.

### <a name="step-2-create-a-new-project-in-visual-studio"> Schritt 2: Erstellen eines neuen Projekts in Visual Studio</a>

Beginnen wir zunächst mit der Erstellung eines neuen Projekts in Visual Studio. Starten Sie zuerst Visual Studio 2015 über das Startmenü. Erstellen Sie dann ein neues Projekt, indem Sie für Ihre Projektvorlage zu **Installiert > Vorlagen > Visual C# > Windows > Universell > Leere App** navigieren:

 ![Erstellen einer neuen universellen App](./Images/CreateUWP.png)

### <a name="step-3-add-the-entity-linking-nuget-package-to-your-project">Schritt 3: Hinzufügen des NuGet-Pakets für den Entity Linking-Analysedienst zu Ihrem Projekt</a>

Der Entity Linking-Analysedienst von Cognitive Services wird als Paket von „NuGet.org“ veröffentlicht und muss installiert werden, bevor Sie ihn verwenden können.
Um ihn zu Ihrem Projekt hinzuzufügen, navigieren Sie zur Registerkarte **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf Ihr Projekt, und wählen Sie **NuGet-Pakete verwalten** aus.

Wählen Sie zunächst im Fenster **NuGet-Paket-Manager** in der oberen rechten Ecke „NuGet.org“ als Ihre **Paketquelle** aus. Klicken Sie in der oberen linken Ecke auf **Durchsuchen**, und geben Sie in das Suchfeld „ProjectOxford.EntityLinking“ ein. Wählen Sie das NuGet-Paket **Microsoft.ProjectOxford.EntityLinking** aus, und klicken Sie auf **Installieren**.

Suchen Sie dann nach „Newtonsoft.Json“, und installieren Sie es. Wenn Sie zur Überprüfung von Änderungen aufgefordert werden, klicken Sie auf **OK**. Wenn die Lizenzbedingungen des Entity Linking-Analysedienst angezeigt werden, klicken Sie auf **Ich stimme zu**.

Der Entity Linking-Analysedienst wird nun als Teil Ihrer Anwendung installiert. Sie können dies überprüfen, indem Sie prüfen, ob der Verweis „Microsoft.ProjectOxford.EntityLinking“ als Bestandteil Ihres Projekts im Projektmappen-Explorer vorhanden ist.

 ![In das Projekt einbezogene NuGet-Bibliothek](./Images/NugetLibraryInProject.png)
 
### <a name="step-4-add-an-input-and-output-text-block-to-your-apps-xaml">Schritt 4: Hinzufügen eines Eingabe- und Ausgabetextblocks zur XAML-Datei Ihrer App</a>
Navigieren Sie im **Projektmappen-Explorer** zu **MainPage.xaml**, und doppelklicken Sie dann auf die Datei, die daraufhin in einem neuen Fenster geöffnet wird. Der Einfachheit halber können Sie auf der Registerkarte **Designer** auf die Schaltfläche **XAML** doppelklicken. Hierdurch wird der **Visual Designer** ausgeblendet, und der gesamte Speicherplatz wird für die Codeansicht reserviert.

 ![In das Projekt einbezogene NuGet-Bibliothek](./Images/UWPMainPage.png)
 
Als Textdienst besteht die beste Möglichkeit zur Visualisierung der Funktionalität darin, einen Eingabe- und einen Ausgabetextblock zu erstellen. Fügen Sie hierzu den folgenden XAML-Code in das **Raster** ein. Dieser Code fügt drei Komponenten hinzu: Ein Eingabetextfeld, einen Ausgabetextblock und eine Startschaltfläche.
 
 ```XAML
 <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="*" />
        <RowDefinition Height="50" />
    </Grid.RowDefinitions>
    <TextBox x:Name="inputBox" Grid.Row="0" TextWrapping="Wrap" Text="Enter a paragraph" Margin="10" AcceptsReturn="True" />
    <TextBlock x:Name="outputBlock" Grid.Row="1" TextWrapping="Wrap" Text="Result will be here" Margin="10" />
    <Button x:Name="button" Grid.Row="2" Content="Get Result" />
</Grid>
 ```
 
### <a name="step-5-proceed-to-add-entity-linking-intelligence-service">Schritt 5: Hinzufügen des Entity Linking-Analysediensts</a>
 
Die Benutzeroberfläche wird nun erstellt. Bevor der Entity Linking-Dienst verwendet werden kann, muss der button_Click-Handler hinzugefügt werden. Öffne Sie über den **Projektmappen-Explorer** die Datei **MainPage.xaml**. Fügen Sie am Ende der Schaltfläche einen button_Click-Handler hinzu.
 
 ```XAML
 <Button x:Name="button" Grid.Row="2" Content="Get Result" Click="button_Click" />
 ```
 
Im Code muss ein button_Click-Handler implementiert werden. Öffnen Sie über den **Projektmappen-Explorer** die Datei **MainPage.xaml.cs**, um den button_Click-Handler zu implementieren. Der EntityLinkingServiceClient ist ein Wrapper zum Abrufen von Entity Linking-Antworten. Das Konstruktorargument für den EntityLinkingServiceClient ist der Cognitive Services-Abonnementschlüssel. Fügen Sie den Abonnementschlüssel ein, den Sie aus **Schritt 1** bezogen haben, um den Entity Linking-Dienst aufzurufen. 

Nachfolgend finden Sie einen Beispielcode, der mithilfe des Entity Linking-Diensts „wikipediaId“ zur Antwort hinzufügt. 
 
 ```csharp
 private async void button_Click(object sender, RoutedEventArgs e)
{
    var text = this.inputBox.Text;
    var client = new EntityLinkingServiceClient("Your subscription key","https://api.labs.cognitive.microsoft.com");
    var linkResponse = await client.LinkAsync(text);
    var result = string.Join(", ", linkResponse.Select(i => i.WikipediaID).ToList());
    this.outputBlock.Text = result;
}
 ```
 
Nun sind Sie startklar, um Ihre erste Entity Linking-App zur Verarbeitung natürlicher Sprache auszuführen. Drücken Sie die **F5-Taste**, um die Anwendung zu kompilieren und zu starten. Fügen Sie Textausschnitte oder -absätze in das Eingabefeld ein. Klicken Sie auf die Schaltfläche „Ergebnis abrufen“, und sehen Sie sich die ermittelten Entitäten im Ausgabeblock an.
 
 ![Beispielergebnis einer UWP-App](./Images/DemoCodeResult.png)
 
### <a name="summary">Zusammenfassung</a>
 
In diesem Tutorial haben Sie erfahren, wie mit nur wenigen Codezeilen in C# und XAML eine Anwendung zur Nutzung der Clientbibliothek des Entity Linking-Analysediensts erstellt wird. 


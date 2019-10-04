---
title: Lokale oder cloudbasierte Hybridanwendung (.NET) mit Azure Windows Communication Foundation (WCF) Relay | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen lokalen Windows Communication Foundation-Anwendungsdienst (WCF-Dienst) für eine Webanwendung in der Cloud mit Azure Relay verfügbar machen.
services: service-bus-relay
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: 9ed02f7c-ebfb-4f39-9c97-b7dc15bcb4c1
ms.service: service-bus-relay
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: spelluru
ms.openlocfilehash: b86d535e4cbc275b3ee777d7c70146f7711c502c
ms.sourcegitcommit: 7df70220062f1f09738f113f860fad7ab5736e88
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2019
ms.locfileid: "71212979"
---
# <a name="expose-an-on-premises-wcf-service-to-a-web-application-in-the-cloud-by-using-azure-relay"></a>Machen Sie einen lokalen Windows Communication Foundation-Anwendungsdienst (WCF-Dienst) für eine Webanwendung in der Cloud mit Azure Relay verfügbar.

In diesem Artikel wird gezeigt, wie Sie mit Microsoft Azure und Visual Studio eine Hybridcloudanwendung erstellen. Sie erstellen eine Anwendung, die mehrere Azure-Ressourcen in der Cloud nutzt. In diesem Tutorial erfahren Sie Folgendes:

* Erstellen oder Anpassen eines vorhandenen Webdiensts zur Verwendung durch eine Weblösung
* Verwenden des Azure Windows Communication Foundation Relay-Diensts (WCF) zum Freigeben von Daten zwischen einer Azure-Anwendung und einem an einem anderen Ort gehosteten Webdienst

In diesem Tutorial führen Sie die folgenden Aufgaben durch:

> [!div class="checklist"]
>
> * Installieren der erforderlichen Komponenten für das Tutorial
> * Überprüfen des Szenarios
> * Erstellen eines Namespaces
> * Erstellen eines lokalen Servers
> * Erstellen einer ASP.NET-Anwendung
> * Führen Sie die App lokal aus.
> * Bereitstellen der Web-App in Azure
> * Ausführen der App in Azure

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

* Ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/), bevor Sie beginnen.
* [Visual Studio 2015 oder eine höhere Version.](https://www.visualstudio.com) Für die Beispiele in diesem Tutorial wird Visual Studio 2019 verwendet.
* Azure SDK für .NET. Installieren Sie das SDK über die [SDK-Downloadseite](https://azure.microsoft.com/downloads/).

## <a name="how-azure-relay-helps-with-hybrid-solutions"></a>So unterstützt Sie Azure Relay bei hybriden Lösungen

Unternehmenslösungen bestehen in der Regel aus einer Kombination von benutzerdefiniertem Code und vorhandenen Funktionen. Der benutzerdefinierte Code berücksichtigt neue und eindeutige geschäftliche Anforderungen. Bereits vorhandene Lösungen und Systeme stellen vorhandene Funktionen zur Verfügung.

Lösungsarchitekten setzen die Cloud inzwischen ein, um Skalierungsanforderungen leichter bewältigen zu können und die Betriebskosten zu senken. Dabei stellen sie fest, dass sich vorhandene Dienstressourcen, die sie als Bausteine für ihre Lösungen nutzen möchten, innerhalb der Unternehmensfirewall befinden und somit nicht problemlos von der Cloudlösung darauf zugegriffen werden kann. Viele interne Dienste werden nicht so erstellt oder gehostet, dass sie einfach am Rand des Unternehmensnetzwerks verfügbar gemacht werden können.

[Azure Relay](https://azure.microsoft.com/services/service-bus/) übernimmt vorhandene WCF-Webdienste und stellt diese Dienste sicher für Lösungen zur Verfügung, die sich außerhalb des Unternehmensumkreises befinden, ohne dass aufwändige Änderungen an der Netzwerkinfrastruktur des Unternehmens vorgenommen werden müssen. Solche Relay-Dienste werden weiterhin in der vorhandenen Umgebung gehostet, das Lauschen an eingehenden Sitzungen und Anforderungen wird jedoch an den in der Cloud gehosteten Relay-Dienst delegiert. Azure Relay schützt diese Dienste per SAS-Authentifizierung ([Shared Access Signature](../service-bus-messaging/service-bus-sas.md)) außerdem vor unbefugtem Zugriff.

## <a name="review-the-scenario"></a>Überprüfen des Szenarios

In diesem Tutorial erstellen Sie eine ASP.NET-Website, mit der Sie eine Produktliste auf der Produktbestandsseite anzeigen können.

![Szenario][0]

In diesem Tutorial wird davon ausgegangen, dass Sie über Produktinformationen in einem vorhandenen lokalen System verfügen, und mithilfe von Azure Relay darauf zugegriffen. Ein Webdienst, der in einer einfachen Konsolenanwendung ausgeführt wird, simuliert diese Situation. Er enthält eine Reihe von In-Memory-Produkten. Sie können diese Konsolenanwendung auf Ihrem eigenen Computer ausführen und die Webrolle in Azure bereitstellen. Auf diese Weise können Sie sehen, wie die im Azure-Rechenzentrum ausgeführte Webrolle Ihren Computer aufruft. Dieser Aufruf erfolgt, obwohl sich Ihr Computer mit ziemlicher Sicherheit hinter mindestens einer Firewall und einer NAT-Ebene (Network Address Translation) befindet.

## <a name="set-up-the-development-environment"></a>Einrichten der Entwicklungsumgebung

Bevor Sie mit der Entwicklung von Azure-Anwendungen beginnen können, müssen Sie die entsprechenden Tools herunterladen und die Entwicklungsumgebung einrichten:

1. Installieren Sie das Azure SDK für .NET über die [Downloadseite](https://azure.microsoft.com/downloads/) des SDK.
1. Wählen Sie in der Spalte **.NET** die von Ihnen verwendete Version von [Visual Studio](https://www.visualstudio.com) aus. In diesem Tutorial wird Visual Studio 2019 verwendet.
1. Wenn Sie aufgefordert werden, das Installationsprogramm auszuführen oder zu speichern, wählen Sie **Ausführen** aus.
1. Wählen Sie im Dialogfeld **Webplattform-Installer** die Option **Installieren** aus, und setzen Sie die Installation fort.

Nach Abschluss der Installation haben Sie alles zur Hand, was Sie benötigen, um mit der Entwicklung der App zu beginnen. Das SDK enthält Tools, mit denen Sie ganz leicht Azure-Anwendungen in Visual Studio entwickeln können.

## <a name="create-a-namespace"></a>Erstellen eines Namespace

Der erste Schritt umfasst die Einrichtung eines Namespaces und das Abrufen eines [Shared Access Signature](../service-bus-messaging/service-bus-sas.md)-Schlüssels (SAS). Ein Namespace stellt eine Anwendungsgrenze für jede Anwendung bereit, die über den Relaydienst verfügbar gemacht wird. Es wird automatisch ein SAS-Schlüssel generiert, wenn ein Dienstnamespace erstellt wird. Dienstnamespace und SAS-Schlüssel bilden gemeinsam die Anmeldeinformationen, mit denen sich Azure für den Zugriff auf die Anwendung authentifiziert.

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="create-an-on-premises-server"></a>Erstellen eines lokalen Servers

Zunächst erstellen Sie ein simuliertes lokales Produktkatalogsystem.  Bei diesem Projekt handelt es sich um eine Visual Studio-Konsolenanwendung, bei dem das [Azure Service Bus-NuGet-Paket](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) verwendet wird, um die Service Bus-Bibliotheken und -Konfigurationseinstellungen einzubinden. <a name="create-the-project"></a>

1. Starten Sie Microsoft Visual Studio als Administrator. Klicken Sie hierzu mit der rechten Maustaste auf das Visual Studio-Programmsymbol, und wählen Sie **Als Administrator ausführen** aus.
1. Wählen Sie in Visual Studio **Neues Projekt erstellen** aus.
1. Wählen Sie unter **Neues Projekt erstellen** die Option **Konsolen-App (.NET Framework)** für C# und dann **Weiter** aus.
1. Geben Sie dem Projekt den Namen *ProductsServer*, und wählen Sie dann **Erstellen** aus.

   ![Neues Projekt konfigurieren][11]

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **ProductsServer**, und wählen Sie anschließend **NuGet-Pakete verwalten** aus.
1. Wählen Sie **Durchsuchen** aus, suchen Sie nach **WindowsAzure.ServiceBus**, und wählen Sie diese Option aus. Wählen Sie **Installieren** aus, und akzeptieren Sie die Nutzungsbedingungen.

   ![Auswählen eines NuGet-Pakets][13]

   Nun wird auf die erforderlichen Clientassemblys verwiesen.

1. Fügen Sie eine neue Klasse für den Produktvertrag hinzu. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **ProductsServer**, und wählen Sie anschließend **Hinzufügen** > **Klasse** aus.
1. Geben Sie in **Name** den Namen *ProductsContract.cs* ein, und wählen Sie dann **Hinzufügen** aus.

Nehmen Sie die folgenden Codeänderungen an Ihrer Projektmappe vor:

1. Ersetzen Sie in *ProductsContract.cs* die Namespacedefinition durch den folgenden Code, mit dem der Vertrag für den Dienst definiert wird.

    ```csharp
    namespace ProductsServer
    {
        using System.Collections.Generic;
        using System.Runtime.Serialization;
        using System.ServiceModel;

        // Define the data contract for the service
        [DataContract]
        // Declare the serializable properties.
        public class ProductData
        {
            [DataMember]
            public string Id { get; set; }
            [DataMember]
            public string Name { get; set; }
            [DataMember]
            public string Quantity { get; set; }
        }

        // Define the service contract.
        [ServiceContract]
        interface IProducts
        {
            [OperationContract]
            IList<ProductData> GetProducts();

        }

        interface IProductsChannel : IProducts, IClientChannel
        {
        }
    }
    ```

1. Ersetzen Sie in *Program.cs* die Namespacedefinition durch den folgenden Code, mit dem der Profildienst und der dafür vorgesehene Host hinzugefügt werden.

    ```csharp
    namespace ProductsServer
    {
        using System;
        using System.Linq;
        using System.Collections.Generic;
        using System.ServiceModel;

        // Implement the IProducts interface.
        class ProductsService : IProducts
        {

            // Populate array of products for display on website
            ProductData[] products =
                new []
                    {
                        new ProductData{ Id = "1", Name = "Rock",
                                         Quantity = "1"},
                        new ProductData{ Id = "2", Name = "Paper",
                                         Quantity = "3"},
                        new ProductData{ Id = "3", Name = "Scissors",
                                         Quantity = "5"},
                        new ProductData{ Id = "4", Name = "Well",
                                         Quantity = "2500"},
                    };

            // Display a message in the service console application
            // when the list of products is retrieved.
            public IList<ProductData> GetProducts()
            {
                Console.WriteLine("GetProducts called.");
                return products;
            }

        }

        class Program
        {
            // Define the Main() function in the service application.
            static void Main(string[] args)
            {
                var sh = new ServiceHost(typeof(ProductsService));
                sh.Open();

                Console.WriteLine("Press ENTER to close");
                Console.ReadLine();

                sh.Close();
            }
        }
    }
    ```

1. Doppelklicken Sie im **Projektmappen-Explorer** auf **App.config**, um die Datei im Visual Studio-Editor zu öffnen. Fügen Sie unten im `<system.ServiceModel>`-Element (aber noch innerhalb von `<system.ServiceModel>`) den folgenden XML-Code hinzu. Ersetzen Sie dabei `yourServiceNamespace` durch den Namen Ihres Namespace und `yourKey` durch den SAS-Schlüssel, den Sie zuvor aus dem Portal abgerufen haben:

    ```xml
    <system.serviceModel>
    ...
      <services>
         <service name="ProductsServer.ProductsService">
           <endpoint address="sb://yourServiceNamespace.servicebus.windows.net/products" binding="netTcpRelayBinding" contract="ProductsServer.IProducts" behaviorConfiguration="products"/>
         </service>
      </services>
      <behaviors>
         <endpointBehaviors>
           <behavior name="products">
             <transportClientEndpointBehavior>
                <tokenProvider>
                   <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
             </transportClientEndpointBehavior>
           </behavior>
         </endpointBehaviors>
      </behaviors>
    </system.serviceModel>
    ```

    > [!NOTE]
    > Der durch `transportClientEndpointBehavior` verursachte Fehler ist nur eine Warnung und kein blockierendes Problem für dieses Beispiel.

1. Ersetzen Sie in *App.config* im `<appSettings>`-Element den Wert der Verbindungszeichenfolge durch die Verbindungszeichenfolge, die Sie zuvor aus dem Portal abgerufen haben.

    ```xml
    <appSettings>
       <!-- Service Bus specific app settings for messaging connections -->
       <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

1. Drücken Sie STRG+UMSCHALT+B, oder wählen Sie **Build** > **Projektmappe erstellen** aus, um die Anwendung zu erstellen und Ihre bisherige Arbeit zu überprüfen.

## <a name="create-an-aspnet-application"></a>Erstellen einer ASP.NET-Anwendung

In diesem Abschnitt erstellen Sie eine einfache ASP.NET-Anwendung, in der aus Ihrem Produktdienst abgerufene Daten angezeigt werden.

### <a name="create-the-project"></a>Erstellen des Projekts

1. Stellen Sie sicher, dass Visual Studio als Administrator ausgeführt wird.
1. Wählen Sie in Visual Studio **Neues Projekt erstellen** aus.
1. Wählen Sie unter **Neues Projekt erstellen** die Option **ASP.NET-Webanwendung (.NET Framework)** für C# und anschließend **Weiter** aus.
1. Nennen Sie das Projekt *ProductsPortal*, und wählen Sie **Erstellen** aus.
1. Wählen Sie unter **Neue ASP.NET-Webanwendung erstellen** die Option **MVC** und dann **Ändern** unter **Authentifizierung** aus.

   ![Auswählen einer ASP.NET-Webanwendung][16]

1. Wählen Sie in **Authentifizierung ändern** die Option **Keine Authentifizierung** und dann **OK** aus. Für dieses Tutorial stellen Sie eine App bereit, für die keine Benutzeranmeldung erforderlich ist.

    ![Angeben des Authentifizierungstyps][18]

1. Wählen Sie im Dialogfeld **Neue ASP.NET-Webanwendung erstellen** die Option **Erstellen** aus, um die MVC-App zu erstellen.
1. Konfigurieren Sie die Azure-Ressourcen für eine neue Web-App. Befolgen Sie die Schritte unter [Veröffentlichen Ihrer Webanwendung](../app-service/app-service-web-get-started-dotnet-framework.md#launch-the-publish-wizard). Kehren Sie anschließend zu diesem Tutorial zurück, und fahren Sie mit dem nächsten Schritt fort.
1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Modelle**, und wählen Sie dann **Hinzufügen** > **Klasse** aus.
1. Benennen Sie die Klasse *Produkt.cs*, und wählen Sie dann **Hinzufügen** aus.

    ![Erstellen des Produktmodells][17]

### <a name="modify-the-web-application"></a>Ändern der Webanwendung

1. Ersetzen Sie in der Datei *Product.cs* in Visual Studio die vorhandene Namespacedefinition durch den folgenden Code:

   ```csharp
    // Declare properties for the products inventory.
    namespace ProductsWeb.Models
    {
       public class Product
       {
           public string Id { get; set; }
           public string Name { get; set; }
           public string Quantity { get; set; }
       }
    }
    ```

1. Erweitern Sie im **Projektmappen-Explorer** die Option **Controller**, und doppelklicken Sie auf **HomeController.cs**, um die Datei in Visual Studio zu öffnen.
1. Ersetzen Sie in der Datei *HomeController.cs* die vorhandene Namespacedefinition durch den folgenden Code:

    ```csharp
    namespace ProductsWeb.Controllers
    {
        using System.Collections.Generic;
        using System.Web.Mvc;
        using Models;

        public class HomeController : Controller
        {
            // Return a view of the products inventory.
            public ActionResult Index(string Identifier, string ProductName)
            {
                var products = new List<Product>
                    {new Product {Id = Identifier, Name = ProductName}};
                return View(products);
            }
         }
    }
    ```

1. Erweitern Sie im **Projektmappen-Explorer** die Option **Views** > **Shared**, und doppelklicken Sie dann auf **_Layout.cshtml**, um die Datei im Visual Studio-Editor zu öffnen.
1. Ändern Sie alle Vorkommnisse von `My ASP.NET Application` in *Northwind Traders Products*.
1. Entfernen Sie die Links `Home`, `About` und `Contact`. Löschen Sie im folgenden Beispiel den hervorgehobenen Code.

    ![Löschen der generierten Listenelemente][41]

1. Erweitern Sie im **Projektmappen-Explorer** die Option **Views** > **Home**, und doppelklicken Sie dann auf **Index.cshtml**, um die Datei im Visual Studio-Editor zu öffnen. Ersetzen Sie den gesamten Inhalt der Datei durch den folgenden Code:

   ```html
   @model IEnumerable<ProductsWeb.Models.Product>

   @{
            ViewBag.Title = "Index";
   }

   <h2>Prod Inventory</h2>

   <table>
             <tr>
                 <th>
                     @Html.DisplayNameFor(model => model.Name)
                 </th>
                 <th></th>
                 <th>
                     @Html.DisplayNameFor(model => model.Quantity)
                 </th>
             </tr>

   @foreach (var item in Model) {
             <tr>
                 <td>
                     @Html.DisplayFor(modelItem => item.Name)
                 </td>
                 <td>
                     @Html.DisplayFor(modelItem => item.Quantity)
                 </td>
             </tr>
   }

   </table>
   ```

1. Drücken Sie STRG+UMSCHALT+B, um das Projekt zu erstellen und Ihre bisherige Arbeit zu überprüfen.

### <a name="run-the-app-locally"></a>Lokales Ausführen der App

Führen Sie die Anwendung aus, um sicherzustellen, dass sie funktioniert.

1. Stellen Sie sicher, dass **ProductsPortal** das aktive Projekt ist. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektnamen, und wählen Sie **Als Startprojekt festlegen**. aus.
1. Drücken Sie in Visual Studio die Taste F5.

Die Anwendung sollte ausgeführt und in einem Browser angezeigt werden.

![Webanwendung][21]

## <a name="put-the-pieces-together"></a>Zusammensetzen der Einzelteile

Im nächsten Schritt wird der lokale Produktserver mit der ASP.NET-Webanwendung zusammengeführt.

1. Falls noch nicht geschehen, öffnen Sie in Visual Studio das Projekt **ProductsPortal**, das Sie im Abschnitt [Erstellen einer ASP.NET-Anwendung](#create-an-aspnet-application) erstellt haben.
1. Fügen Sie das NuGet-Paket den Projektverweisen hinzu, wie im Schritt [Erstellen eines lokalen Servers](#create-an-on-premises-server) beschrieben. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **ProductsPortal**, und klicken Sie anschließend auf **NuGet-Pakete verwalten**.
1. Suchen Sie nach *WindowsAzure.ServiceBus*, und wählen Sie das Element **WindowsAzure.ServiceBus** aus. Schließen Sie die Installation ab, und schließen Sie das Dialogfeld.
1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **ProductsPortal**, und wählen Sie anschließend **Hinzufügen** > **Vorhandenes Element** aus.
1. Navigieren Sie im Konsolenprojekt **ProductsServer** zur Datei *ProductsContract.cs*. Heben Sie *ProductsContract.cs* hervor. Wählen Sie den Pfeil nach unten neben **Hinzufügen** und dann **Als Link hinzufügen** aus.

   ![Als Link hinzufügen][24]

1. Öffnen Sie nun die Datei *HomeController.cs* im Visual Studio-Editor, und ersetzen Sie die Namespacedefinition durch den folgenden Code. Achten Sie darauf, `yourServiceNamespace` durch den Namen Ihres Dienstnamespace und `yourKey` durch Ihren SAS-Schlüssel zu ersetzen. Der Client kann jetzt mit diesem Code den lokalen Dienst aufrufen und das Ergebnis des Aufrufs zurückgeben.

   ```csharp
   namespace ProductsWeb.Controllers
   {
       using System.Linq;
       using System.ServiceModel;
       using System.Web.Mvc;
       using Microsoft.ServiceBus;
       using Models;
       using ProductsServer;

       public class HomeController : Controller
       {
           // Declare the channel factory.
           static ChannelFactory<IProductsChannel> channelFactory;

           static HomeController()
           {
               // Create shared access signature token credentials for authentication.
               channelFactory = new ChannelFactory<IProductsChannel>(new NetTcpRelayBinding(),
                   "sb://yourServiceNamespace.servicebus.windows.net/products");
               channelFactory.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior {
                   TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                       "RootManageSharedAccessKey", "yourKey") });
           }

           public ActionResult Index()
           {
               using (IProductsChannel channel = channelFactory.CreateChannel())
               {
                   // Return a view of the products inventory.
                   return this.View(from prod in channel.GetProducts()
                                    select
                                        new Product { Id = prod.Id, Name = prod.Name,
                                            Quantity = prod.Quantity });
               }
           }
       }
   }
   ```

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Projektmappe **ProductsPortal**. Stellen Sie sicher, dass Sie mit der rechten Maustaste auf die Projektmappe und nicht auf das Projekt klicken. Wählen Sie **Hinzufügen** > **Vorhandenes Projekt** aus.
1. Navigieren Sie zum Projekt **ProductsServer**, und doppelklicken Sie dann auf die Projektmappendatei *ProductsServer.csproj*, um sie hinzuzufügen.
1. **ProductsServer** muss ausgeführt werden, damit die Daten unter **ProductsPortal** angezeigt werden. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Projektmappe **ProductsPortal**, und wählen Sie dann **Eigenschaften** aus, um **Eigenschaftenseiten** anzuzeigen.
1. Wählen Sie **Allgemeine Eigenschaften** > **Startprojekt** und dann **Mehrere Startprojekte** aus. Vergewissern Sie sich, dass **ProductsServer** und **ProductsPortal** in dieser Reihenfolge angezeigt werden und jeweils **Start** als **Aktion** festgelegt ist.

      ![Mehrere Startprojekte][25]

1. Wählen Sie **Allgemeine Eigenschaften** > **Projektabhängigkeiten** auf der linken Seite aus.
1. Wählen Sie für **Projekte** die Option **ProductsPortal** aus. Vergewissern Sie sich, dass **ProductsServer** ausgewählt ist.

    ![Projektabhängigkeiten][26]

1. Wählen Sie für **Projekte** die Option **ProductsServer** aus. Stellen Sie sicher, dass **ProductsPortal** nicht aktiviert ist, und wählen Sie dann **OK** aus, um Ihre Änderungen zu speichern.

## <a name="run-the-project-locally"></a>Lokales Ausführen des Projekts

Drücken Sie F5, um die Anwendung in Visual Studio lokal zu testen. Zuerst muss der lokale Server, **ProductsServer**, gestartet werden, gefolgt von der Anwendung **ProductsPortal** in einem Browserfenster. Dieses Mal werden Sie feststellen, dass in der Produktbestandsliste Daten aus dem lokalen Produktdienstsystem aufgeführt werden.

![Webanwendung][10]

Wählen Sie auf der Seite **ProductsPortal** die Option **Aktualisieren** aus. Bei jeder Aktualisierung der Seite zeigt die Server-App eine Meldung an, wenn `GetProducts()` von **ProductsServer** aufgerufen wird.

Schließen Sie beide Anwendungen, bevor Sie mit dem nächsten Abschnitt fortfahren.

## <a name="deploy-the-productsportal-project-to-an-azure-web-app"></a>Bereitstellen des Projekts ProductsPortal in einer Azure-Web-App

Im nächsten Schritt wird das **ProductsPortal**-Front-End der Azure-Web-App erneut veröffentlicht:

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **ProductsPortal**, und wählen Sie anschließend **Veröffentlichen** aus. Wählen Sie auf der Seite **Veröffentlichen** die Option **Veröffentlichen** aus.

   > [!NOTE]
   > Im Browserfenster wird unter Umständen eine Fehlermeldung angezeigt, wenn das Webprojekt **ProductsPortal** nach der Bereitstellung automatisch gestartet wird. Dies ist zu erwarten und tritt auf, da die Anwendung **ProductsServer** noch nicht ausgeführt wird.
   >

1. Kopieren Sie die URL der bereitgestellten Web-App. Sie benötigen die URL später. Sie können diese URL auch in Visual Studio im Fenster **Aktivität von Azure App Service** abrufen:

   ![URL der bereitgestellten App][9]

1. Schließen Sie das Browserfenster, um die ausgeführte Anwendung zu beenden.

<a name="set-productsportal-as-web-app"></a>Bevor Sie die Anwendung in der Cloud ausführen, müssen Sie dafür sorgen, dass **ProductsPortal** in Visual Studio als Web-App gestartet wird.

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf das Projekt **ProductsPortal**, und wählen Sie dann **Eigenschaften** aus.
1. Wählen Sie **Web** aus. Wählen Sie unter **Startaktion** die Option **Start-URL** aus. Geben Sie die URL für Ihre zuvor bereitgestellte Web-App ein, in diesem Beispiel `https://productsportal20190906122808.azurewebsites.net/`.

    ![Start-URL][27]

1. Wählen Sie **Datei** > **Alle speichern** aus.
1. Wählen Sie **Erstellen** > **Projektmappe erneut erstellen** aus.

## <a name="run-the-application"></a>Ausführen der Anwendung

Drücken Sie F5, um die Anwendung zu erstellen und auszuführen. Zuerst muss der lokale Server (die Konsolenanwendung **ProductsServer**) gestartet werden, gefolgt von der Anwendung **ProductsPortal** in einem Browserfenster, wie hier dargestellt:

   ![Ausführen der Web-App in Azure][1]

Im Produktbestand werden die aus dem lokalen Produktdienstsystem abgerufenen Daten aufgelistet und in der Web-App angezeigt. Vergewissern Sie sich anhand der URL, dass **ProductsPortal** in der Cloud als Azure-Web-App ausgeführt wird.

   > [!IMPORTANT]
   > Die Konsolenanwendung **ProductsServer** muss ausgeführt werden, und sie muss die Daten für die Anwendung **ProductsPortal** bereitstellen können. Sollte im Browser ein Fehler angezeigt werden, warten Sie noch einige Sekunden, bis **ProductsServer** geladen wurde und die folgende Nachricht angezeigt wird. Aktualisieren Sie dann den Browser.
   >

Aktualisieren Sie im Browser die Seite **ProduktePortal**. Bei jeder Aktualisierung der Seite zeigt die Server-App eine Meldung an, wenn `GetProducts()` von **ProductsServer** aufgerufen wird.

![Aktualisierte Ausgabe][38]

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem folgenden Tutorial fort:

> [!div class="nextstepaction"]
>[Verfügbarmachen eines lokalen WCF-Diensts für einen WCF-Client außerhalb Ihres Netzwerks](service-bus-relay-tutorial.md)

[0]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hybrid.png
[1]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/run-web-app.png
[NuGet]: https://nuget.org

[11]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/configure-productsserver.png
[13]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/install-nuget-service-bus-productsserver.png
[15]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-2.png
[16]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/choose-web-application-template.png
[17]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/add-class-productsportal.png
[18]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/change-authentication.png
[9]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/web-publish-activity.png
[10]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/run-web-app-locally.png

[21]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/run-web-app-locally-no-content.png
[24]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/add-existing-item-link.png
[25]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-13.png
[26]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-14.png
[27]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/launch-app-as-web-app.png

[36]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
[37]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service1.png
[38]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service2.png
[41]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-40.png
[43]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-43.png

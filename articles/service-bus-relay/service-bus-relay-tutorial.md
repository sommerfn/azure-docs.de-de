---
title: 'Tutorial: Verfügbarmachen eines lokalen WCF REST-Diensts für einen externen Client mithilfe von Azure WCF Relay'
description: 'Tutorial: Erstellen einer Client- und Dienstanwendung mit WCF-Relay.'
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: 53dfd236-97f1-4778-b376-be91aa14b842
ms.service: service-bus-relay
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/05/2019
ms.author: spelluru
ms.openlocfilehash: e2dd0448dfed55450a6319936f49831e5d6d77f3
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73718846"
---
# <a name="tutorial-expose-an-on-premises-wcf-rest-service-to-external-client-by-using-azure-wcf-relay"></a>Tutorial: Verfügbarmachen eines lokalen WCF REST-Diensts für einen externen Client mithilfe von Azure WCF Relay

In diesem Tutorial wird die Erstellung einer Clientanwendung und eines Diensts für WCF Relay mithilfe von Azure Relay beschrieben. Ein ähnliches Tutorial, in dem die [Messagingfunktionen von Service Bus](../service-bus-messaging/service-bus-messaging-overview.md) beschrieben werden, finden Sie unter [Erste Schritte mit Service Bus-Warteschlangen](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).

Dieses Tutorial vermittelt Ihnen Grundkenntnisse zu den Schritten, mit denen eine Client- und Dienstanwendung für WCF Relay erstellt wird. Wie die ursprünglichen WCF-Entsprechungen ist ein Dienst ein Konstrukt, mit dem ein oder mehrere Endpunkte verfügbar gemacht werden. Jeder Endpunkt macht mindestens einen Dienstvorgang verfügbar. Der Endpunkt eines Diensts gibt eine Adresse an, unter der der Dienst gefunden werden kann, eine Bindung, die die Information enthält, dass ein Client mit dem Dienst kommunizieren muss, und einen Vertrag, der die Funktionen definiert, die der Dienst für seine Clients bereitstellt. Der Hauptunterschied zwischen WCF und WCF-Relay besteht darin, dass der Endpunkt in der Cloud und nicht lokal auf Ihrem Computer bereitgestellt wird.

Nachdem Sie die Abschnitte in diesem Tutorial der Reihe nach durchgearbeitet haben, verfügen Sie über einen ausgeführten Dienst. Außerdem verfügen Sie über einen Client, mit dem die Vorgänge des Diensts aufgerufen werden können. 

In diesem Tutorial führen Sie die folgenden Aufgaben durch:

> [!div class="checklist"]
>
> * Installieren der erforderlichen Komponenten für das Tutorial
> * Erstellen eines Relaynamespace
> * Erstellen eines WCF-Dienstvertrags
> * Implementieren des WCF-Vertrags
> * Hosten und Ausführen des WCF-Diensts für die Registrierung beim Relaydienst
> * Erstellen eines WCF-Clients für den Dienstvertrag
> * Konfigurieren des WCF-Clients
> * Implementieren des WCF-Clients
> * Ausführen der Anwendungen

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

* Ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/), bevor Sie beginnen.
* [Visual Studio 2015 oder eine höhere Version.](https://www.visualstudio.com) Für die Beispiele in diesem Tutorial wird Visual Studio 2019 verwendet.
* Azure SDK für .NET. Installieren Sie das SDK über die [SDK-Downloadseite](https://azure.microsoft.com/downloads/).

## <a name="create-a-relay-namespace"></a>Erstellen eines Relaynamespace

Der erste Schritt umfasst die Einrichtung eines Namespaces und das Abrufen eines [Shared Access Signature](../service-bus-messaging/service-bus-sas.md)-Schlüssels (SAS). Ein Namespace stellt eine Anwendungsgrenze für jede Anwendung bereit, die über den Relaydienst verfügbar gemacht wird. Es wird automatisch ein SAS-Schlüssel generiert, wenn ein Dienstnamespace erstellt wird. Dienstnamespace und SAS-Schlüssel bilden gemeinsam die Anmeldeinformationen, mit denen sich Azure für den Zugriff auf die Anwendung authentifiziert.

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="define-a-wcf-service-contract"></a>Definieren eines WCF-Dienstvertrags

Im Dienstvertrag ist angegeben, welche Vorgänge vom Dienst unterstützt werden. Vorgänge sind Webdienstmethoden oder -funktionen. Verträge werden durch die Definition einer C++-, C#- oder Visual Basic-Schnittstelle erstellt. Jede Methode in der Schnittstelle entspricht einem bestimmten Dienstvorgang. Auf jede Schnittstelle muss das Attribut [ServiceContractAttribute](/dotnet/api/system.servicemodel.servicecontractattribute) und auf jeden Vorgang das Attribut [OperationContractAttribute](/dotnet/api/system.servicemodel.operationcontractattribute) angewendet werden. Wenn eine Methode in einer Schnittstelle das Attribut [ServiceContractAttribute](/dotnet/api/system.servicemodel.servicecontractattribute), aber nicht das Attribut [OperationContractAttribute](/dotnet/api/system.servicemodel.operationcontractattribute) besitzt, wird diese Methode nicht verfügbar gemacht. Der Code für diese Aufgaben wird im Beispiel im Anschluss an das Verfahren bereitgestellt. Eine ausführlichere Darstellung von Verträgen und Diensten finden Sie unter [Entwerfen und Implementieren von Diensten](/dotnet/framework/wcf/designing-and-implementing-services).

### <a name="create-a-relay-contract-with-an-interface"></a>Erstellen eines Relayvertrags mit einer Schnittstelle

1. Starten Sie Microsoft Visual Studio als Administrator. Klicken Sie hierzu mit der rechten Maustaste auf das Visual Studio-Programmsymbol, und wählen Sie **Als Administrator ausführen** aus.
1. Wählen Sie in Visual Studio **Neues Projekt erstellen** aus.
1. Wählen Sie unter **Neues Projekt erstellen** die Option **Konsolen-App (.NET Framework)** für C# und dann **Weiter** aus.
1. Geben Sie dem Projekt den Namen *EchoService*, und wählen Sie **Erstellen** aus.

   ![Erstellen einer Konsolen-App][2]

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **NuGet-Pakete verwalten** aus. Wählen Sie im **NuGet-Paket-Manager** die Option **Durchsuchen** aus, suchen Sie nach **WindowsAzure.ServiceBus**, und wählen Sie diese Option aus. Wählen Sie **Installieren** aus, und akzeptieren Sie die Nutzungsbedingungen.

    ![Service Bus-Paket][3]

   Mit diesem Paket werden automatisch Verweise auf die Service Bus-Bibliotheken und das WCF-Element `System.ServiceModel` hinzugefügt. [System.ServiceModel](/dotnet/api/system.servicemodel) ist der Namespace, der den programmgesteuerten Zugriff auf die grundlegenden Funktionen von WCF ermöglicht. Service Bus verwendet viele Objekte und Attribute von WCF, um Dienstverträge zu definieren.

1. Fügen Sie am Anfang von *Program.cs* die folgenden `using`-Anweisungen hinzu:

    ```csharp
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```

1. Ändern Sie den Namespacenamen vom Standardnamen `EchoService` in `Microsoft.ServiceBus.Samples`.

   > [!IMPORTANT]
   > In diesem Tutorial wird der C#-Namespace `Microsoft.ServiceBus.Samples` verwendet. Hierbei handelt es sich um den Namespace des über den Vertrag verwalteten Typs, der in der Konfigurationsdatei im Abschnitt [Konfigurieren des WCF-Clients](#configure-the-wcf-client) verwendet wird. Sie können bei der Erstellung dieses Beispiels einen beliebigen Namespace angeben. Das Tutorial funktioniert aber nur, wenn Sie dann in der Anwendungskonfigurationsdatei die Namespaces des Vertrags und des Diensts entsprechend ändern. Der in der Datei *App.config* angegebene Namespace muss mit dem in Ihren C#-Dateien angegebenen Namespace identisch sein.
   >

1. Definieren Sie direkt nach der Namespacedeklaration `Microsoft.ServiceBus.Samples` (jedoch innerhalb des Namespaces) eine neue Schnittstelle namens `IEchoContract`, und wenden Sie das Attribut `ServiceContractAttribute` mit dem Namespacewert `https://samples.microsoft.com/ServiceModel/Relay/` auf die Schnittstelle an. Fügen Sie nach der Namespacedeklaration den folgenden Code ein:

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

    Der Namespacewert unterscheidet sich von dem Namespace, den Sie im gesamten Codebereich verwenden. Der Namespacewert wird stattdessen als eindeutiger Bezeichner für diesen Vertrag verwendet. Das explizite Angeben des Namespace verhindert, dass der Standardwert für den Namespace dem Vertragsnamen hinzugefügt wird.

   > [!NOTE]
   > In der Regel enthält der Dienstvertragsnamespace ein Benennungsschema, das Versionsinformationen umfasst. Durch das Einbeziehen von Versionsinformationen im Dienstvertragsnamespace können Dienste größere Änderungen isolieren, indem ein neuer Dienstvertrag mit einem neuen Namespace definiert und an einem neuen Endpunkt bereitgestellt wird. Auf diese Weise können Clients weiterhin den alten Dienstvertrag verwenden, ohne dass sie aktualisiert werden müssen. Versionsinformationen können aus einer Datumsangabe oder einer Buildnummer bestehen. Weitere Informationen finden Sie unter [Dienstversionsverwaltung](/dotnet/framework/wcf/service-versioning). In diesem Tutorial umfasst das Benennungsschema des Dienstvertragsnamespace keine Versionsinformationen.
   >

1. Deklarieren Sie innerhalb der Schnittstelle `IEchoContract` eine Methode für den einzelnen Vorgang, den der Vertrag `IEchoContract` in der Schnittstelle bereitstellt, und wenden Sie das Attribut `OperationContractAttribute` auf die Methode an, die Sie als Teil des öffentlichen WCF-Relayvertrags bereitstellen möchten:

    ```csharp
    [OperationContract]
    string Echo(string text);
    ```

1. Deklarieren Sie direkt im Anschluss an die `IEchoContract`-Schnittstellendefinition einen Kanal mit Vererbung von `IEchoContract` und Vererbung an die `IClientChannel`-Schnittstelle, wie im folgenden Beispiel gezeigt:

    ```csharp
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    Ein Kanal ist das WCF-Objekt, über das der Host und der Client einander Informationen übergeben. Später schreiben Sie Code für den Kanal, um ein Echo der Informationen zwischen den beiden Anwendungen zu erzeugen.

1. Wählen Sie **Erstellen** > **Projektmappe erstellen** aus (oder STRG+UMSCHALT+B), um die Korrektheit Ihrer bisherigen Arbeit zu bestätigen.

### <a name="example-of-a-wcf-contract"></a>Beispiel für einen WCF-Vertrag

Das folgende Codebeispiel zeigt eine Basisschnittstelle, die einen WCF-Relayvertrag definiert.

```csharp
using System;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Nachdem nun die Schnittstelle erstellt ist, können Sie die Schnittstelle implementieren.

## <a name="implement-the-wcf-contract"></a>Implementieren des WCF-Vertrags

Zum Erstellen eines Azure-Relays müssen Sie zuerst den Vertrag erstellen, indem Sie eine Schnittstelle verwenden. Weitere Informationen zum Erstellen der Schnittstelle finden Sie im vorherigen Abschnitt. Mit der nächsten Prozedur wird die Schnittstelle implementiert. Diese Aufgabe umfasst das Erstellen einer Klasse mit dem Namen `EchoService`, mit der die benutzerdefinierte `IEchoContract`-Schnittstelle implementiert wird. Nachdem Sie die Schnittstelle implementiert haben, konfigurieren Sie die Schnittstelle mit einer *App.config*-Konfigurationsdatei. Die Konfigurationsdatei enthält die erforderlichen Informationen für die Anwendung. Dies sind der Name des Diensts, der Name des Vertrags und der Protokolltyp, der für die Kommunikation mit dem Relaydienst verwendet wird. Der für diese Aufgaben verwendete Code ist im Beispiel enthalten, das auf die Prozedur folgt. Eine allgemeinere Darstellung der Implementierung eines Dienstvertrags finden Sie unter [Implementieren von Dienstverträgen](/dotnet/framework/wcf/implementing-service-contracts).

1. Erstellen Sie eine neue Klasse namens `EchoService` direkt unterhalb der Definition der `IEchoContract`-Schnittstelle. Die `EchoService`-Klasse implementiert die `IEchoContract`-Schnittstelle.

    ```csharp
    class EchoService : IEchoContract
    {
    }
    ```

    Ähnlich wie bei anderen Schnittstellenimplementierungen können Sie die Definition in einer anderen Datei implementieren. In diesem Tutorial befindet sich die Implementierung allerdings in derselben Datei wie die Schnittstellendefinition und die `Main()`-Methode.

1. Wenden Sie das [ServiceBehaviorAttribute](/dotnet/api/system.servicemodel.servicebehaviorattribute)-Attribut auf die `IEchoContract`-Schnittstelle an. Das Attribut gibt den Dienstnamen und den Namespace an. Die `EchoService`-Klasse sieht dann wie folgt aus:

    ```csharp
    [ServiceBehavior(Name = "EchoService", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```

1. Implementieren Sie die `Echo`-Methode, die in der `IEchoContract`-Schnittstelle in der `EchoService`-Klasse definiert ist.

    ```csharp
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```

1. Wählen Sie **Erstellen** > **Projektmappe erstellen** aus (oder STRG+UMSCHALT+B).

### <a name="define-the-configuration-for-the-service-host"></a>Definieren der Konfiguration für den Diensthost

Die Konfigurationsdatei ähnelt einer WCF-Konfigurationsdatei. Sie enthält den Dienstnamen, den Endpunkt und die Bindung. Der Endpunkt ist der Ort, der von Azure Relay für Clients und Hosts verfügbar gemacht wird, um die Kommunikation zu ermöglichen. Die Bindung ist der Protokolltyp, der für die Kommunikation verwendet wird. Der Hauptunterschied besteht darin, dass dieser konfigurierte Dienstendpunkt auf eine [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding)-Bindung verweist, die nicht Bestandteil von .NET Framework ist. [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) ist eine der Bindungen, die durch den Dienst definiert werden.

1. Doppelklicken Sie im **Projektmappen-Explorer** auf **App.config**, um die Datei im Visual Studio-Editor zu öffnen.
1. Ersetzen Sie im `<appSettings>`-Element die Platzhalter durch den Namen Ihres Dienstnamespace und den SAS-Schlüssel, den Sie in einem vorherigen Schritt kopiert haben.
1. Fügen Sie innerhalb der `<system.serviceModel>`-Tags ein `<services>`-Element hinzu. Sie können mehrere Relayanwendungen in einer einzigen Konfigurationsdatei definieren. In diesem Tutorial wird jedoch nur eine Anwendung definiert.

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```

1. Fügen Sie im `<services>`-Element ein `<service>`-Element hinzu, um den Namen des Diensts zu definieren.

    ```xml
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```

1. Definieren Sie im `<service>`-Element den Speicherort des Endpunktvertrags sowie den Bindungstyp für den Endpunkt.

    ```xml
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    Der Endpunkt definiert, wo der Client nach der Hostanwendung sucht. Dieser Schritt wird später in diesem Tutorial verwendet, um einen URI zu erstellen, der den Host über Azure Relay uneingeschränkt verfügbar macht. Die Bindung deklariert die Verwendung von TCP als Protokoll für die Kommunikation mit dem Relaydienst.

1. Wählen Sie **Erstellen** > **Projektmappe erstellen** aus (oder STRG+UMSCHALT+B), um die Korrektheit Ihrer bisherigen Arbeit zu bestätigen.

### <a name="example-of-implementation-of-a-service-contract"></a>Beispiel für die Implementierung eines Dienstvertrags

Der folgende Code zeigt die Implementierung des Dienstvertrags.

```csharp
[ServiceBehavior(Name = "EchoService", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]

    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }
```

Der folgende Code veranschaulicht das grundlegende Format der Datei *App.config*, die dem Diensthost zugeordnet ist.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceBus.Samples.EchoService">
        <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding" />
      </service>
    </services>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="host-and-run-the-wcf-service-to-register-with-the-relay-service"></a>Hosten und Ausführen des WCF-Diensts für die Registrierung beim Relaydienst

Dieser Schritt beschreibt, wie ein Azure Relay-Dienst ausgeführt wird.

### <a name="create-the-relay-credentials"></a>Erstellen der Relayanmeldeinformationen

1. Erstellen Sie in `Main()` zwei Variablen zum Speichern des Namespace und des SAS-Schlüssels, die aus dem Konsolenfenster gelesen werden.

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    Der SAS-Schlüssel wird später für den Zugriff auf Ihr Projekt verwendet. Der Namespace wird als Parameter an `CreateServiceUri` übergeben, um einen Dienst-URI zu erstellen.

1. Deklarieren Sie mithilfe eines [TransportClientEndpointBehavior](/dotnet/api/microsoft.servicebus.transportclientendpointbehavior)-Objekts, dass ein SAS-Schlüssel als Anmeldeinformationstyp verwendet wird. Fügen Sie den folgenden Code direkt unter dem Code hinzu, den Sie im letzten Schritt hinzugefügt haben.

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### <a name="create-a-base-address-for-the-service"></a>Erstellen einer Basisadresse für den Dienst

Erstellen Sie nach dem Code, den Sie im vorherigen Abschnitt hinzugefügt haben, eine `Uri`-Instanz für die Basisadresse des Diensts. Dieser URI gibt das Service Bus-Schema, den Namespace und den Pfad der Dienstschnittstelle an.

```csharp
Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
```

Der Wert „sb“ ist eine Abkürzung für das Service Bus-Schema. Gibt an, dass TCP als Protokoll verwendet wird. Dieses Schema wurde zuvor bereits in der Konfigurationsdatei durch das Angeben von [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) als Bindung festgelegt.

Für dieses Tutorial lautet der URI `sb://putServiceNamespaceHere.windows.net/EchoService`.

### <a name="create-and-configure-the-service-host"></a>Erstellen und Konfigurieren des Diensthosts

1. Legen Sie unter `Main()` den Konnektivitätsmodus auf `AutoDetect` fest.

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    Der Verbindungsmodus beschreibt das Protokoll, das der Dienst für die Kommunikation mit dem Relaydienst verwendet: HTTP oder TCP. Mithilfe der Standardeinstellung `AutoDetect` versucht der Dienst, falls verfügbar, eine Verbindung mit Azure Relay über TCP herzustellen. Wenn TCP nicht verfügbar ist, wird HTTP verwendet. Dieses Ergebnis unterscheidet sich von dem Protokoll, das vom Dienst für die Clientkommunikation angegeben wird. Dieses Protokoll wird durch die verwendete Bindung festgelegt. Ein Dienst kann z.B. die Bindung [BasicHttpRelayBinding](/dotnet/api/microsoft.servicebus.basichttprelaybinding) verwenden, die angibt, dass ihr Endpunkt über HTTP mit Clients kommuniziert. Derselbe Dienst kann auch `ConnectivityMode.AutoDetect` angeben, sodass der Dienst per TCP mit Azure Relay kommuniziert.

1. Erstellen Sie den Diensthost unter Verwendung des URIs, den Sie zuvor in diesem Abschnitt erstellt haben.

    ```csharp
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    Der Diensthost ist das WCF-Objekt, das den Dienst instanziiert. Hier wird der Diensttyp übergeben, den Sie erstellen möchten – ein `EchoService`-Typ –, sowie die Adresse, unter der der Dienst bereitgestellt werden soll.

1. Fügen Sie am Anfang der Datei *Program.cs* Verweise auf [System.ServiceModel.Description](/dotnet/api/system.servicemodel.description) und [Microsoft.ServiceBus.Description](/dotnet/api/microsoft.servicebus.description) hinzu.

    ```csharp
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```

1. Konfigurieren Sie in `Main()` den Endpunkt so, dass öffentlicher Zugriff ermöglicht wird.

    ```csharp
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    In diesem Schritt wird der Relaydienst darüber informiert, dass Ihre Anwendung durch das Untersuchen des Atom-Feeds für Ihr Projekt öffentlich ermittelbar ist. Wenn Sie `DiscoveryType` auf `private` festlegen, kann ein Client trotzdem auf den Dienst zugreifen. Der Dienst wird aber nicht angezeigt, wenn der `Relay`-Namespace durchsucht wird. Der Client muss stattdessen bereits vorher den Endpunktpfad kennen.

1. Wenden Sie die Dienstanmeldeinformationen auf die in der Datei *App.config* definierten Dienstendpunkte an:

    ```csharp
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    Wie bereits erwähnt, können Sie in der Konfigurationsdatei mehrere Dienste und Endpunkte deklarieren. Wenn dies der Fall ist, durchläuft der Code die Konfigurationsdatei und sucht nach jedem Endpunkt, auf den die Anmeldeinformationen angewendet werden sollen. Für dieses Tutorial weist die Konfigurationsdatei nur einen Endpunkt auf.

### <a name="open-the-service-host"></a>Öffnen des Diensthosts

1. Fügen Sie in `Main()` die folgende Zeile hinzu, um den Dienst zu öffnen.

    ```csharp
    host.Open();
    ```

1. Informieren Sie den Benutzer, dass der Dienst ausgeführt wird, und erläutern Sie, wie der Dienst heruntergefahren wird.

    ```csharp
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

1. Schließen Sie danach den Diensthost.

    ```csharp
    host.Close();
    ```

1. Wählen Sie STRG+UMSCHALT+B aus, um das Projekt zu erstellen.

### <a name="example-that-hosts-a-service-in-a-console-application"></a>Beispiel: Hosten eines Diensts in einer Konsolenanwendung

Der abgeschlossene Dienstcode sollte folgendermaßen aussehen. Der Code schließt den Dienstvertrag und die Implementierung aus früheren Schritten des Tutorials ein und hostet den Dienst in einer Konsolenanwendung.

```csharp
using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Description;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { };

    [ServiceBehavior(Name = "EchoService", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {

            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;         

            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS key: ");
            string sasKey = Console.ReadLine();

           // Create the credentials object for the endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create the service URI based on the service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create the service host reading the configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create the ServiceRegistrySettings behavior for the endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add the Relay credentials to all endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }

            // Open the service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            // Close the service.
            host.Close();
        }
    }
}
```

## <a name="create-a-wcf-client-for-the-service-contract"></a>Erstellen eines WCF-Clients für den Dienstvertrag

Die nächste Aufgabe besteht darin, eine Clientanwendung zu erstellen und den Dienstvertrag zu definieren, den Sie später implementieren. Diese Schritte ähneln den Schritten, die zum Erstellen eines Diensts verwendet werden: Definieren eines Vertrags, Bearbeiten einer *App.config*-Datei, Verwenden von Anmeldeinformationen zum Herstellen einer Verbindung mit dem Relaydienst usw. Der für diese Aufgaben verwendete Code wird im Beispiel nach dem Verfahren bereitgestellt.

1. Erstellen Sie in der aktuellen Visual Studio-Projektmappe für den Client ein neues Projekt:

   1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die aktuelle Projektmappe (nicht auf das Projekt), und wählen Sie **Hinzufügen** > **Neues Projekt** aus.
   1. Wählen Sie unter **Neues Projekt erstellen** die Option **Konsolen-App (.NET Framework)** für C# und dann **Weiter** aus.
   1. Geben Sie dem Projekt den Namen *EchoClient*, und wählen Sie **Erstellen** aus.

1. Doppelklicken Sie im **Projektmappen-Explorer** im Projekt **EchoClient** auf die Datei **Program.cs**, um die Datei im Editor zu öffnen (sofern sie nicht bereits geöffnet ist).
1. Ändern Sie den Namespacenamen vom Standardnamen `EchoClient` in `Microsoft.ServiceBus.Samples`.
1. Installieren Sie das [NuGet-Paket für Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus):

   1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **EchoClient**, und wählen Sie anschließend **NuGet-Pakete verwalten** aus.
   1. Wählen Sie **Durchsuchen** aus, suchen Sie nach **WindowsAzure.ServiceBus**, und wählen Sie diese Option aus. Wählen Sie **Installieren** aus, und akzeptieren Sie die Nutzungsbedingungen.

      ![Installieren des Service Bus-Pakets][4]

1. Fügen Sie der Datei *Program.cs* eine `using`-Anweisung für den Namespace [System.ServiceModel](/dotnet/api/system.servicemodel) hinzu.

    ```csharp
    using System.ServiceModel;
    ```

1. Fügen Sie dem Namespace die Dienstvertragsdefinition hinzu, wie im folgenden Beispiel gezeigt. Diese Definition ist mit der im Projekt **Service** verwendeten Definition identisch. Fügen Sie diesen Code am Anfang des Namespace `Microsoft.ServiceBus.Samples` hinzu.

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

1. Wählen Sie STRG+UMSCHALT+B aus, um den Client zu erstellen.

### <a name="example-of-the-echoclient-project"></a>Beispiel für das Projekt „EchoClient“

Mit dem folgenden Code wird der aktuelle Status der Datei *Program.cs* im Projekt **EchoClient** angezeigt.

```csharp
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IEchoContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }


    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="configure-the-wcf-client"></a>Konfigurieren des WCF-Clients

In diesem Schritt erstellen Sie eine *App.config*-Datei für eine Clientbasisanwendung, die auf den zuvor in diesem Tutorial erstellten Dienst zugreift. Diese *App.config*-Datei definiert den Vertrag, die Bindung und den Namen des Endpunkts. Der für diese Aufgaben verwendete Code wird im Beispiel nach dem Verfahren bereitgestellt.

1. Doppelklicken Sie im **Projektmappen-Explorer** im Projekt **EchoClient** auf **App.config**, um die Datei im Visual Studio-Editor zu öffnen.
1. Ersetzen Sie im `<appSettings>`-Element die Platzhalter durch den Namen Ihres Dienstnamespace und den SAS-Schlüssel, den Sie in einem vorherigen Schritt kopiert haben.
1. Fügen Sie im `system.serviceModel`-Element ein `<client>`-Element hinzu.

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    Mit diesem Code wird deklariert, dass Sie eine Clientanwendung im WCF-Stil definieren.

1. Definieren Sie im Element `client` den Namen, den Vertrag und den Bindungstyp für den Endpunkt.

    ```xml
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    Mit diesem Code wird der Name des Endpunkts definiert. Außerdem werden der im Dienst festgelegte Vertrag und die Verwendung von TCP durch die Clientanwendung für die Kommunikation mit Azure Relay definiert. Der Endpunktname wird im nächsten Schritt verwendet, um diese Endpunktkonfiguration mit dem Dienst-URI zu verknüpfen.

1. Wählen Sie **Datei** > **Alle speichern** aus.

### <a name="example-of-the-appconfig-file"></a>Beispiel für die Datei „App.config“

Der folgende Code veranschaulicht die *App.config*-Datei für den Echo-Client.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <client>
      <endpoint name="RelayEndpoint"
                      contract="Microsoft.ServiceBus.Samples.IEchoContract"
                      binding="netTcpRelayBinding"/>
    </client>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="implement-the-wcf-client"></a>Implementieren des WCF-Clients

In diesem Abschnitt implementieren Sie eine Clientbasisanwendung, die auf den zuvor in diesem Tutorial erstellten Dienst zugreift. Ähnlich wie der Dienst führt der Client viele derselben Vorgänge durch, um auf Azure Relay zuzugreifen:

* Festlegen des Verbindungsmodus.
* Erstellen des URIs, der den Hostdienst ermittelt.
* Definieren der Sicherheitsanmeldeinformationen.
* Anwenden der Anmeldeinformationen auf die Verbindung.
* Öffnen der Verbindung.
* Ausführen der anwendungsspezifischen Aufgaben.
* Schließen der Verbindung.

Einer der Hauptunterschiede besteht aber darin, dass die Clientanwendung einen Kanal für die Verbindung mit dem Relaydienst verwendet. Der Dienst verwendet einen Aufruf von **ServiceHost**. Der für diese Aufgaben verwendete Code wird im Beispiel nach dem Verfahren bereitgestellt.

### <a name="implement-a-client-application"></a>Implementieren einer Clientanwendung

1. Legen Sie den Verbindungsmodus auf `AutoDetect` fest. Fügen Sie der Anwendung **EchoClient** in der Methode `Main()` den folgenden Code hinzu.

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

1. Definieren Sie Variablen zum Speichern der Werte für den Dienstnamespace und den SAS-Schlüssel, die aus der Konsole gelesen werden.

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```

1. Erstellen Sie den URI, der den Speicherort des Hosts in Ihrem Relayprojekt definiert.

    ```csharp
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

1. Erstellen Sie das Objekt für die Anmeldeinformationen für Ihren Dienstnamespace-Endpunkt.

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

1. Erstellen Sie die Kanalfactory, mit der die in der Datei *App.config* beschriebene Konfiguration geladen wird.

    ```csharp
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    Bei einer Kanalfactory ist handelt es sich um ein WCF-Objekt, das einen Kanal erstellt, über den der Dienst und die Clientanwendungen kommunizieren.

1. Wenden Sie die Anmeldeinformationen an.

    ```csharp
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```

1. Erstellen und öffnen Sie den Kanal zum Dienst.

    ```csharp
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```

1. Schreiben Sie die grundlegende Benutzeroberfläche und die Funktionen für das Echo.

    ```csharp
    Console.WriteLine("Enter text to echo (or [Enter] to exit):");
    string input = Console.ReadLine();
    while (input != String.Empty)
    {
        try
        {
            Console.WriteLine("Server echoed: {0}", channel.Echo(input));
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: " + e.Message);
        }
        input = Console.ReadLine();
    }
    ```

    Im Code wird die Instanz des Kanalobjekts als Proxy für den Dienst verwendet.

1. Schließen Sie den Kanal und die Factory.

    ```csharp
    channel.Close();
    channelFactory.Close();
    ```

### <a name="example-code-for-this-tutorial"></a>Beispielcode für dieses Tutorial

Der fertige Code sollte wie folgt aussehen. Dieser Code veranschaulicht, wie eine Clientanwendung erstellt wird, wie die Dienstvorgänge aufgerufen werden und wie der Client geschlossen wird, nachdem der Vorgangsaufruf abgeschlossen ist.

```csharp
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;


            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS Key: ");
            string sasKey = Console.ReadLine();



            Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));

            channelFactory.Endpoint.Behaviors.Add(sasCredential);

            IEchoChannel channel = channelFactory.CreateChannel();
            channel.Open();

            Console.WriteLine("Enter text to echo (or [Enter] to exit):");
            string input = Console.ReadLine();
            while (input != String.Empty)
            {
                try
                {
                    Console.WriteLine("Server echoed: {0}", channel.Echo(input));
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: " + e.Message);
                }
                input = Console.ReadLine();
            }

            channel.Close();
            channelFactory.Close();

        }
    }
}
```

## <a name="run-the-applications"></a>Ausführen der Anwendungen

1. Wählen Sie STRG+UMSCHALT+B aus, um die Projektmappe zu erstellen. Mit dieser Aktion werden das Client- und das Dienstprojekt erstellt, die Sie in den vorherigen Schritten erstellt haben.
1. Vergewissern Sie sich vor dem Ausführen der Clientanwendung, dass die Dienstanwendung ausgeführt wird. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Projektmappe **EchoService**, und wählen Sie anschließend **Eigenschaften** aus.
1. Wählen Sie unter **Eigenschaftenseiten** die Option **Allgemeine Eigenschaften** > **Startprojekt** und dann **Mehrere Startprojekte** aus. Vergewissern Sie sich, dass **EchoService** als erster Eintrag in der Liste angezeigt wird.
1. Legen Sie das Feld **Aktion** sowohl für das Projekt **EchoService** als auch für das Projekt **EchoClient** auf **Starten** fest.

    ![Seiten mit Projekteigenschaften][5]

1. Wählen Sie **Projektabhängigkeiten** aus. Wählen Sie unter **Projekte** die Option **EchoClient** aus. Stellen Sie sicher, dass unter **Abhängig von** die Option **EchoService** ausgewählt ist.

    ![Projektabhängigkeiten][6]

1. Wählen Sie **OK** aus, um **Eigenschaftenseiten** zu schließen.
1. Wählen Sie F5 aus, um beide Projekte auszuführen.
1. Beide Konsolenfenster werden geöffnet, und Sie werden jeweils zur Angabe des Namespacenamens aufgefordert. Der Dienst muss zuerst ausgeführt werden. Geben Sie daher im Konsolenfenster von **EchoService** den Namespace ein, und wählen Sie anschließend die EINGABETASTE aus.
1. Als Nächstes werden Sie von der Konsole zum Eingeben Ihres SAS-Schlüssels aufgefordert. Geben Sie den SAS-Schlüssel ein, und wählen Sie die EINGABETASTE aus.

    Im Folgenden wird eine Beispielausgabe des Konsolenfensters gezeigt. Bei den hier aufgeführten Werten handelt es sich nur um Beispiele.

    `Your Service Namespace: myNamespace`

    `Your SAS Key: <SAS key value>`

    Die Dienstanwendung gibt die Adresse, an der sie lauscht, wie im folgenden Beispiel gezeigt im Konsolenfenster aus:

    `Service address: sb://mynamespace.servicebus.windows.net/EchoService/`

    `Press [Enter] to exit`

1. Geben Sie im Konsolenfenster **EchoClient** die gleichen Informationen wie für die Dienstanwendung ein. Geben Sie die gleichen Dienstnamespace- und SAS-Schlüsselwerte für die Clientanwendung ein.
1. Nachdem Sie diese Werte eingegeben haben, öffnet der Client einen Kanal zum Dienst und fordert Sie auf, Text einzugeben, wie im folgenden Beispiel für die Konsolenausgabe gezeigt.

    `Enter text to echo (or [Enter] to exit):`

    Geben Sie Text ein, der an die Dienstanwendung gesendet werden soll, und wählen Sie die EINGABETASTE aus. Dieser Text wird über den Echo-Dienstvorgang an den Dienst gesendet und im Dienstkonsolenfenster ähnlich wie in der folgenden Beispielausgabe angezeigt.

    `Echoing: My sample text`

    Die Clientanwendung empfängt den Rückgabewert des `Echo`-Vorgangs (den ursprünglichen Text) und gibt diesen in ihrem Konsolenfenster aus. Der folgende Text ist eine Beispielausgabe des Clientkonsolenfensters.

    `Server echoed: My sample text`

1. Sie können auf diese Weise weitere Textnachrichten vom Client an den Dienst senden. Wählen Sie nach Abschluss des Vorgangs im Client- bzw. Dienstkonsolenfenster die EINGABETASTE aus, um beide Anwendungen zu beenden.

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem folgenden Tutorial fort:

> [!div class="nextstepaction"]
>[Verfügbarmachen eines lokalen WCF-REST-Diensts für einen Client außerhalb Ihres Netzwerks](service-bus-relay-rest-tutorial.md)

[2]: ./media/service-bus-relay-tutorial/configure-echoservice-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget-service-bus.png
[4]: ./media/service-bus-relay-tutorial/install-nuget-service-bus-client.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png

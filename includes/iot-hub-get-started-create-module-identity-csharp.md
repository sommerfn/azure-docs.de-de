---
title: include file
description: include file
services: iot-hub
author: chrissie926
ms.service: iot-hub
ms.topic: include
ms.date: 08/07/2019
ms.author: menchi
ms.custom: include file
ms.openlocfilehash: a5c1ddd085ae65b9920d73f50f993f4646785a69
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68883841"
---
## <a name="create-a-module-identity"></a>Erstellen einer Modulidentität

In diesem Abschnitt erstellen Sie eine .NET-Konsolen-App, mit der eine Geräte- und eine Modulidentität in der Identitätsregistrierung Ihres Hubs erstellt werden. Ein Gerät oder Modul kann nur dann eine Verbindung mit dem Hub herstellen, wenn in der Identitätsregistrierung ein Eintrag für dieses Gerät vorhanden ist. Weitere Informationen finden Sie im [IoT Hub-Entwicklerhandbuch im Abschnitt zur Identitätsregistrierung](../articles/iot-hub/iot-hub-devguide-identity-registry.md).

Wenn Sie diese Konsolen-App ausführen, generiert sie eine eindeutige ID und einen eindeutigen Schlüssel für das Gerät und das Modul. Ihr Gerät und Ihr Modul verwenden diese Werte, um sich beim Senden von D2C-Nachrichten an IoT Hub zu identifizieren. Bei den IDs gilt Groß-/Kleinschreibung.

1. Öffnen Sie Visual Studio, und wählen Sie **Neues Projekt erstellen** aus.

1. Wählen Sie in **Neues Projekt erstellen** die Option **Konsolen-App (.NET Framework)** aus.

1. Wählen Sie **Weiter** aus, um **Neues Projekt konfigurieren** zu öffnen. Nennen Sie das Projekt *CreateIdentities* und die Projektmappe *IoTHubGetStarted*. Stellen Sie sicher, dass .NET Framework-Version 4.6.1 oder höher verwendet wird.

    ![Eingeben von Name und Framework für die Visual Studio-Projektmappe](./media/iot-hub-get-started-create-module-identity-csharp/configure-createidentities-project.png)

1. Navigieren Sie in Visual Studio zu **Tools** > **NuGet-Paket-Manager** > **NuGet-Pakete für Projektmappe verwalten**. Wählen Sie die Registerkarte **Durchsuchen** aus.

1. Suchen Sie **Microsoft.Azure.Devices**. Wählen Sie diese Option aus, und wählen Sie **Installieren** aus.

    ![Installieren des Azure IoT Hub-.NET-Dienst-SDK in der aktuellen Version](./media/iot-hub-get-started-create-module-identity-csharp/install-service-sdk.png)

1. Fügen Sie am Anfang der Datei **Program.cs** die folgenden `using`-Anweisungen hinzu:

   ```csharp
   using Microsoft.Azure.Devices;
   using Microsoft.Azure.Devices.Common.Exceptions;
   ```

1. Fügen Sie der **Program** -Klasse die folgenden Felder hinzu. Ersetzen Sie den Platzhalterwert durch die IoT Hub-Verbindungszeichenfolge für den Hub, den Sie im vorherigen Abschnitt erstellt haben.

   ```csharp
   const string connectionString = "<replace_with_iothub_connection_string>";
   const string deviceID = "myFirstDevice";
   const string moduleID = "myFirstModule";
   ```

1. Fügen Sie den folgenden Code der **Main**-Klasse hinzu.

   ```csharp
   static void Main(string[] args)
   {
       AddDeviceAsync().Wait();
       AddModuleAsync().Wait();
   }
   ```

1. Fügen Sie der **Program**-Klasse die folgenden Methoden hinzu:

    ```csharp
    private static async Task AddDeviceAsync()
    {
       RegistryManager registryManager = 
         RegistryManager.CreateFromConnectionString(connectionString);
       Device device;

       try
       {
           device = await registryManager.AddDeviceAsync(new Device(deviceID));
       }
       catch (DeviceAlreadyExistsException)
        {
            device = await registryManager.GetDeviceAsync(deviceID);
        }

        Console.WriteLine("Generated device key: {0}", 
          device.Authentication.SymmetricKey.PrimaryKey);
    }

    private static async Task AddModuleAsync()
    {
        RegistryManager registryManager = 
          RegistryManager.CreateFromConnectionString(connectionString);
        Module module;

        try
        {
            module = 
              await registryManager.AddModuleAsync(new Module(deviceID, moduleID));
        }
        catch (ModuleAlreadyExistsException)
        {
            module = await registryManager.GetModuleAsync(deviceID, moduleID);
        }

        Console.WriteLine("Generated module key: {0}", module.Authentication.SymmetricKey.PrimaryKey);
    }
    ```

    Mit der `AddDeviceAsync`-Methode wird eine Geräteidentität mit der ID **myFirstDevice** erstellt. Falls diese Geräte-ID in der Identitätsregistrierung bereits vorhanden ist, werden mit dem Code lediglich die vorhandenen Geräteinformationen abgerufen. Anschließend zeigt die App den Primärschlüssel für diese Identität an. Sie verwenden diesen Schlüssel in der App für das simulierte Gerät, um eine Verbindung mit Ihrem Hub herzustellen.

    Die `AddModuleAsync`-Methode erstellt eine Modulidentität mit der ID **myFirstModule** unter dem Gerät **myFirstDevice**. Falls diese Modul-ID in der Identitätsregistrierung bereits vorhanden ist, werden mit dem Code lediglich die vorhandenen Modulinformationen abgerufen. Anschließend zeigt die App den Primärschlüssel für diese Identität an. Sie verwenden diesen Schlüssel in der App für das simulierte Modul, um eine Verbindung mit Ihrem Hub herzustellen.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

1. Führen Sie diese App aus, und notieren Sie den Geräte- und den Modulschlüssel.

> [!NOTE]
> Die Identitätsregistrierung in IoT Hub speichert nur Geräte- und Modulidentitäten, um sicheren Zugriff auf den Hub zu ermöglichen. In der Identitätsregistrierung werden Geräte-IDs und -schlüssel für die Verwendung als Sicherheitsanmeldeinformationen gespeichert. Darüber hinaus wird in der Identitätsregistrierung ein Flag für den Aktivierungszustand des jeweiligen Geräts gespeichert, mit dem Sie den Zugriff für das betreffende Gerät deaktivieren können. Wenn Ihre App das Speichern weiterer gerätespezifischer Metadaten erfordert, sollte dafür ein anwendungsspezifischer Speicher verwendet werden. Es gibt keinen Flag „Aktiviert/deaktiviert“ für Modulidentitäten. Weitere Informationen finden Sie im [IoT Hub-Entwicklerhandbuch](../articles/iot-hub/iot-hub-devguide-identity-registry.md).

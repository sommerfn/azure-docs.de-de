---
title: Azure Service Fabric-Anwendungsressourcenmodell | Microsoft-Dokumentation
description: Dieser Artikel enthält eine Übersicht über die Verwaltung einer Azure Service Fabric-Anwendung mit Azure Resource Manager.
services: service-fabric
author: athinanthny
ms.service: service-fabric
ms.topic: conceptual
ms.date: 08/07/2019
ms.author: atsenthi
ms.openlocfilehash: dcffc1ba783b49343bf3380b62c3d4085f5aa347
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72390098"
---
# <a name="what-is-the-service-fabric-application-resource-model"></a>Was ist das Service Fabric-Anwendungsressourcenmodell?
Es wird empfohlen, dass Service Fabric-Anwendungen über Azure Resource Manager in Ihrem Service Fabric-Cluster bereitgestellt werden. Auf diese Weise können Anwendungen und Dienste in JSON beschrieben und in der gleichen Resource Manager-Vorlage wie der Cluster bereitgestellt werden. Im Unterschied zur Bereitstellung und Verwaltung von Anwendungen über PowerShell oder die Azure-Befehlszeilenschnittstelle muss nicht gewartet werden, bis der Cluster bereit ist. Der Vorgang der Anwendungsregistrierung und -bereitstellung kann in nur einem Schritt erfolgen. Diese Methode hat sich beim Verwalten des Anwendungslebenszyklus im Cluster bewährt. Weitere Informationen finden Sie unter [Bewährte Methoden](https://docs.microsoft.com/azure/service-fabric/service-fabric-best-practices-infrastructure-as-code#azure-service-fabric-resources).

Falls möglich, verwalten Sie Ihre Anwendungen als Ressourcen-Manager-Ressourcen, um Folgendes zu verbessern:
* Überwachungsprotokoll: Der Ressourcen-Manager überprüft jeden einzelnen Vorgang und führt ein detailliertes *Aktivitätsprotokoll*, das Ihnen helfen kann, Änderungen an diesen Anwendungen und Ihrem Cluster nachzuvollziehen.
* Rollenbasierte Zugriffssteuerung: Die Verwaltung des Zugriffs auf Cluster sowie auf Anwendungen, die im Cluster bereitgestellt sind, kann über dieselbe Resource Manager-Vorlage erfolgen.
* Azure Resource Manager (über das Azure-Portal) fungiert als Zentrale für die Verwaltung Ihres Clusters und kritischer Anwendungsbereitstellungen.

## <a name="service-fabric-application-life-cycle-with-azure-resource-manager"></a>Service Fabric-Anwendungslebenszyklus mit Azure Resource Manager 
In diesem Dokument erfahren Sie Folgendes:

> [!div class="checklist"]
> * Bereitstellen von Anwendungsressourcen mithilfe von Azure Resource Manager 
> * Aktualisieren von Anwendungsressourcen mithilfe von Azure Resource Manager
> * Löschen von Anwendungsressourcen

## <a name="deploy-application-resources-using-azure-resource-manager"></a>Bereitstellen von Anwendungsressourcen mithilfe von Azure Resource Manager  
Zum Bereitstellen einer Anwendung und der zugehörigen Dienste mithilfe des Azure Resource Manager-Anwendungsressourcenmodells müssen Sie den Anwendungscode packen, das Paket hochladen und dann auf den Speicherort des Pakets als Anwendungsressource in einer Azure Resource Manager-Vorlage verweisen. Weitere Informationen finden Sie unter [Packen einer Anwendung](https://docs.microsoft.com/azure/service-fabric/service-fabric-package-apps#create-an-sfpkg).
          
Erstellen Sie dann eine Azure Resource Manager-Vorlage, aktualisieren Sie die Parameterdatei mit Anwendungsdetails, und stellen Sie sie im Service Fabric-Cluster bereit. Einige Beispiele finden Sie [hier](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/tree/master/ARM).

### <a name="create-a-storage-account"></a>Erstellen eines Speicherkontos 
Zum Bereitstellen einer Anwendung über eine Resource Manager-Vorlage ist ein Speicherkonto zum Bereitstellen des Anwendungsimages erforderlich. Sie können ein vorhandenes Speicherkonto wiederverwenden oder ein neues Speicherkonto erstellen, um Ihre Anwendungen bereitzustellen. Sie können diesen Schritt überspringen, wenn Sie ein vorhandenes Speicherkonto verwenden möchten. 

![Speicherkonto erstellen][CreateStorageAccount]

### <a name="configure-storage-account"></a>Konfigurieren des Speicherkontos 
Nach dem Erstellen des Speicherkontos müssen Sie einen Blobcontainer erstellen, in dem die Anwendungen bereitgestellt werden können. Navigieren Sie im Azure-Portal zu dem Speicherkonto, in dem Ihre Anwendungen gespeichert werden sollen. Wählen Sie das Blatt **Blobs** aus, und klicken Sie auf die Schaltfläche **Container hinzufügen**. Fügen Sie einen neuen Container mit der öffentlichen Zugriffsebene „Blob“ hinzu.
   
![Erstellen des Blobs][CreateBlob]

### <a name="stage-application-in-a-storage-account"></a>Bereitstellen der Anwendung in einem Speicherkonto
Damit die Anwendung bereitgestellt werden kann, muss sie in Blob Storage bereitgestellt werden. In diesem Tutorial wird das Anwendungspaket manuell erstellt. Dieser Schritt kann jedoch automatisiert werden.  Weitere Informationen finden Sie unter [Packen einer Anwendung](https://docs.microsoft.com/azure/service-fabric/service-fabric-package-apps#create-an-sfpkg). In den folgenden Schritten wird die [Beispielanwendung „Voting“](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart) verwendet.

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf das Projekt „Voting“, und wählen Sie „Packen“ aus.   
![Packen der Anwendung][PackageApplication]  
2. Öffnen Sie das soeben erstellte Verzeichnis **.\service-fabric-dotnet-quickstart\Voting\pkg\Debug**, und zippen Sie den Inhalt so in einer Datei mit dem Namen **Voting.zip**, dass die Datei „ApplicationManifest.xml“ den Stamm der ZIP-Datei bildet.  
![Zippen der Anwendung][ZipApplication]  
3. Benennen Sie die Erweiterung der Datei von „.zip“ in **.sfpkg** um.
4. Klicken Sie im Azure-Portal im Container **apps** des Speicherkontos auf **Hochladen**, und laden Sie **Voting.sfpkg** hoch.  
![Hochladen des App-Pakets][UploadAppPkg]

Die Anwendung ist damit bereitgestellt. Nun kann die Azure Resource Manager-Vorlage zum Bereitstellen der Anwendung erstellt werden.      
   
### <a name="create-the-azure-resource-manager-template"></a>Erstellen der Azure Resource Manager-Vorlage
Die Beispielanwendung enthält [Azure Resource Manager-Vorlagen](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/tree/master/ARM), die zum Bereitstellen der Anwendung verwendet werden können. Die Vorlagendateien heißen **UserApp.json** und **UserApp.Parameters.json**. 

> [!NOTE] 
> Die Datei **UserApp.Parameters.json** muss mit dem Namen Ihres Clusters aktualisiert werden.
>
>

| Parameter              | BESCHREIBUNG                                 | Beispiel                                                      | Kommentare                                                     |
| ---------------------- | ------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| clusterName            | Der Name des Clusters, in dem die Bereitstellung erfolgt | sf-cluster123                                                |                                                              |
| application            | Der Name der Anwendung.                 | Voting                                                       |
| applicationTypeName    | Der Typname der Anwendung           | VotingType                                                   | Muss mit dem Inhalt der Datei „ApplicationManifest.xml“ übereinstimmen.                 |
| applicationTypeVersion | Die Version des Anwendungstyps         | 1.0.0                                                        | Muss mit dem Inhalt der Datei „ApplicationManifest.xml“ übereinstimmen.                 |
| serviceName            | Der Name des Diensts         | Voting~VotingWeb                                             | Muss im Format „ApplicationName~ServiceType“ vorliegen.            |
| serviceTypeName        | Der Typname des Diensts                | VotingWeb                                                    | Muss mit dem Inhalt der Datei „ServiceManifest.xml“ übereinstimmen.                 |
| appPackageUrl          | Die Blob Storage-URL der Anwendung     | https://servicefabricapps.blob.core.windows.net/apps/Voting.sfpkg | Die URL des Anwendungspakets in Blob Storage (die Schritte für diese Festlegung werden weiter unten beschrieben) |
       
```json
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applications",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes/versions",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'), '/', parameters('applicationTypeVersion'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applications/services",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName'))]",
    "location": "[variables('clusterLocation')]"
}
```

### <a name="deploy-the-application"></a>Bereitstellen der Anwendung 
Führen Sie zum Bereitstellen der Anwendung „New-AzResourceGroupDeployment“ aus, um die Ressourcengruppe bereitzustellen, die den Cluster enthält.
```powershell
New-AzResourceGroupDeployment -ResourceGroupName "sf-cluster-rg" -TemplateParameterFile ".\UserApp.Parameters.json" -TemplateFile ".\UserApp.json" -Verbose
```

## <a name="upgrade-service-fabric-application-using-azure-resource-manager"></a>Aktualisieren einer Service Fabric-Anwendung mithilfe von Azure Resource Manager
Anwendungen, die bereits in einem Service Fabric-Cluster bereitgestellt sind, werden aus folgenden Gründen aktualisiert:

1. Der Anwendung wird ein neuer Dienst hinzugefügt. Eine Dienstdefinition muss den Dateien „service-manifest.xml“ und „application-manifest.xml“ hinzugefügt werden. Um die neue Version der Anwendung anzugeben, müssen Sie dann die Anwendungstypversion von [UserApp.parameters.json](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/blob/master/ARM/UserApp.Parameters.json) von 1.0.0 in 1.0.1 ändern.

    ```
    "applicationTypeVersion": {
        "value": "1.0.1"
    },
    "serviceName2": {
        "value": "Voting~VotingData"
    },
    "serviceTypeName2": {
        "value": "VotingDataType"
    }
    ```
2. Eine neue Version eines vorhandenen Diensts wird der Anwendung hinzugefügt. Dies umfasst Änderungen am Anwendungscode sowie Aktualisierungen der Typversion und des Typnamens der Anwendung.

    ```
     "applicationTypeVersion": {
        "value": "1.0.1"
    },
    ```

## <a name="delete-application-resources"></a>Löschen von Anwendungsressourcen
Anwendungen, die mithilfe des Anwendungsressourcenmodells in Azure Resource Manager bereitgestellt werden, können mit den folgenden Schritten aus dem Cluster gelöscht werden.

1) Rufen Sie mit dem Befehl [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource?view=azps-2.5.0) die Ressourcen-ID für die Anwendung ab.  

    #### <a name="get-resource-id-for-application"></a>Abrufen der Ressourcen-ID für die Anwendung
    ```
    Get-AzResource  -Name <String> | f1
    ```
2) Löschen Sie die Anwendungsressourcen mit dem Befehl [Remove-AzResource](https://docs.microsoft.com/powershell/module/az.resources/remove-azresource?view=azps-2.5.0).  

    #### <a name="delete-application-resource-using-its-resource-id"></a>Löschen der Anwendungsressource anhand der zugehörigen Ressourcen-ID
    ```
    Remove-AzResource  -ResourceId <String> [-Force] [-ApiVersion <String>]
    ```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Anwendungsressourcenmodell:

* [Modellieren einer Anwendung in Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-model)
* [Service Fabric-Anwendungs- und -Dienstmanifeste](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-and-service-manifests)

## <a name="see-also"></a>Siehe auch
* [bewährten Methoden](https://docs.microsoft.com/azure/service-fabric/service-fabric-best-practices-infrastructure-as-code)
* [Verwalten von Anwendungen und Diensten als Azure-Ressourcen](https://docs.microsoft.com/azure/service-fabric/service-fabric-best-practices-infrastructure-as-code)

<!--Image references-->
[CreateStorageAccount]: ./media/service-fabric-application-model/create-storage-account.png
[CreateBlob]: ./media/service-fabric-application-model/create-blob.png
[PackageApplication]: ./media/service-fabric-application-model/package-application.png
[ZipApplication]: ./media/service-fabric-application-model/zip-application.png
[UploadAppPkg]: ./media/service-fabric-application-model/upload-app-pkg.png

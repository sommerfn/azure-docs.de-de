---
title: 'Tutorial: Erstellen einer Java-App in Azure Service Fabric'
description: In diesem Tutorial erfahren Sie, wie Sie eine Reliable Services-Java-Anwendung mit einem Front-End und ein zustandsbehaftetes Reliable Services-Back-End erstellen und die Anwendung in einem Cluster bereitstellen.
services: service-fabric
documentationcenter: java
author: suhuruli
manager: mfussell
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/01/2018
ms.author: suhuruli
ms.custom: mvc, seo-java-july2019, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 5e9b94f0c67b4b4630d554f4dde22502c90e777c
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72376375"
---
# <a name="tutorial-create-an-application-with-a-java-api-front-end-service-and-a-stateful-back-end-service-on-azure-service-fabric"></a>Tutorial: Erstellen einer Anwendung mit einem Java-API-Front-End-Dienst und einem zustandsbehafteten Back-End-Dienst in Azure Service Fabric

Dieses Tutorial ist der erste Teil einer Serie. Das Ergebnis ist eine Abstimmungsanwendung mit einem Java-Web-Front-End, mit der Abstimmungsergebnisse im Cluster in einem zustandsbehafteten Back-End-Dienst in Azure Service Fabric gespeichert werden. Für diese Tutorialreihe benötigen Sie einen funktionierenden Entwicklercomputer mit Mac OSX oder Linux. Wenn Sie die Abstimmungsanwendung nicht manuell erstellen möchten, können Sie den [Quellcode für die fertige Anwendung herunterladen](https://github.com/Azure-Samples/service-fabric-java-quickstart) und mit [Durchlaufen der Beispielanwendung für die Abstimmung](service-fabric-tutorial-create-java-app.md#walk-through-the-voting-sample-application) fortfahren. Gegebenenfalls ist auch die [Schnellstartanleitung für Java Reliable Services](service-fabric-quickstart-java-reliable-services.md) hilfreich.

![Service Fabric-Beispiel „Voting“](./media/service-fabric-tutorial-create-java-app/service-fabric-java-voting-app-sample.png)

In dieser Tutorialserie lernen Sie Folgendes:
> [!div class="checklist"]
> * Erstellen einer Service Fabric Reliable Services-Java-Anwendung
> * [Bereitstellen und Debuggen der Anwendung in einem lokalen Cluster](service-fabric-tutorial-debug-log-local-cluster.md)
> * [Bereitstellen der Anwendung in einem Azure-Cluster](service-fabric-tutorial-java-deploy-azure.md)
> * [Einrichten der Überwachung und Diagnose für die Anwendung](service-fabric-tutorial-java-elk.md)
> * [Richten Sie CI/CD ein.](service-fabric-tutorial-java-jenkins.md)


Im ersten Teil der Serie lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines zustandsbehafteten zuverlässigen Java-Diensts
> * Erstellen eines zustandslosen Java-Webanwendungsdiensts
> * Verwenden von Dienstremoting zum Kommunizieren mit dem zustandsbehafteten Dienst
> * Bereitstellen der Anwendung in einem lokalen Service Fabric-Cluster

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen können, müssen Sie Folgendes tun:

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen.
* Richten Sie Ihre Entwicklungsumgebung für [Mac](service-fabric-get-started-mac.md) oder [Linux](service-fabric-get-started-linux.md) ein. Führen Sie die erforderlichen Schritte aus, um das Eclipse-Plug-In, Gradle, das Service Fabric SDK und die Service Fabric-Befehlszeilenschnittstelle (sfctl) zu installieren.

## <a name="create-the-front-end-java-stateless-service"></a>Erstellen des zustandslosen Front-End-Java-Diensts

Erstellen Sie zunächst das Web-Front-End der Abstimmungsanwendung. Eine AngularJS-basierte Webbenutzeroberfläche sendet Anforderungen an den zustandslosen Java-Dienst, der einen einfachen HTTP-Server ausführt. Dieser Dienst verarbeitet die jeweiligen Anforderungen und sendet einen Remoteprozeduraufruf an den zustandsbehafteten Dienst, um die Stimmen zu speichern. 

1. Öffnen Sie Eclipse.

2. Erstellen Sie ein Projekt über **Datei** > **Neu** > **Andere** > **Service Fabric** > **Service Fabric-Projekt**.

    ![Neues Service Fabric-Projekt in Eclipse](./media/service-fabric-tutorial-create-java-app/service-fabric-project-wizard.png)

3. Geben Sie dem Projekt im Dialogfeld des **ServiceFabric-Projekt-Assistenten** den Namen **Voting**, und wählen Sie **Weiter** aus.

    ![Auswählen des zustandslosen Java-Diensts im Dialogfeld für einen neuen Dienst](./media/service-fabric-tutorial-create-java-app/name-service-fabric-project-wizard.png) 

4. Wählen Sie auf der Seite **Dienst hinzufügen** die Option **Zustandsloser Dienst** aus, und geben Sie dem Dienst den Namen **VotingWeb**. Wählen Sie **Fertig stellen** aus, um das Projekt zu erstellen.

    ![Erstellen eines zustandslosen Diensts für Ihr Service Fabric-Projekt]( ./media/service-fabric-tutorial-create-java-app/add-service-fabric-votingweb-service.png)

    Eclipse erstellt eine Anwendung und ein Dienstprojekt und zeigt diese Elemente im Paket-Explorer an.

    ![Paket-Explorer von Eclipse nach der Anwendungserstellung]( ./media/service-fabric-tutorial-create-java-app/eclipse-package-explorer.png)

Die Tabelle enthält eine kurze Beschreibung der einzelnen Elemente im Paket-Explorer aus dem obigen Screenshot. 

| **Element des Paket-Explorers** | **Beschreibung** |
| --- | --- |
| PublishProfiles | Enthält JSON-Dateien mit Beschreibungen der Profildetails von lokalen Clustern und Azure Service Fabric-Clustern. Der Inhalt dieser Dateien wird vom Plug-In beim Bereitstellen der Anwendung verwendet. |
| Skripts | Enthält Hilfsskripts, die an der Befehlszeile zum schnellen Verwalten Ihrer Anwendung mit einem Cluster verwendet werden können. |
| VotingApplication | Enthält die Service Fabric-Anwendung, die per Pushvorgang in den Service Fabric-Cluster übertragen wird. |
| VotingWeb | Enthält die Quelldateien des zustandslosen Front-End-Diensts und die dazugehörige Gradle-Builddatei. |
| build.gradle | Die Gradle-Datei, die zum Verwalten des Projekts verwendet wird. |
| settings.gradle | Enthält die Namen des Gradle-Projekts in diesem Ordner. |

### <a name="add-html-and-javascript-to-the-votingweb-service"></a>Hinzufügen von HTML und JavaScript zum VotingWeb-Dienst

Fügen Sie eine HTML-Datei hinzu, um eine Benutzeroberfläche hinzuzufügen, die vom zustandslosen Dienst gerendert werden kann. Diese HTML-Datei wird dann von dem einfachen HTTP-Server gerendert, der in den zustandslosen Java-Dienst eingebettet ist.

1. Erweitern Sie das Verzeichnis *VotingApplication*, um zum Verzeichnis *VotingApplication/VotingWebPkg/Code* zu gelangen.

2. Klicken Sie mit der rechten Maustaste auf das Verzeichnis *Code*, und wählen Sie **Neu** > **Ordner** aus.

3. Nennen Sie den Ordner *wwwroot*, und wählen Sie **Fertig stellen** aus.

    ![Eclipse – Erstellen des Ordners „wwwroot“](./media/service-fabric-tutorial-create-java-app/create-wwwroot-folder.png)

4. Fügen Sie **wwwroot** die Datei **index.html** hinzu, und fügen Sie den folgenden Inhalt in die Datei ein.

```html
<!DOCTYPE html>
<html>
<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/angular-ui-bootstrap/0.13.4/ui-bootstrap-tpls.min.js"></script>
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
<body>

<script>
var app = angular.module('VotingApp', ['ui.bootstrap']);
app.controller("VotingAppController", ['$rootScope', '$scope', '$http', '$timeout', function ($rootScope, $scope, $http, $timeout) {
    $scope.votes = [];
    
    $scope.refresh = function () {
        $http.get('getStatelessList')
            .then(function successCallback(response) {
        $scope.votes = Object.assign(
            {},
            ...Object.keys(response.data) .
            map(key => ({[decodeURI(key)]: response.data[key]}))
        )
        },
        function errorCallback(response) {
            alert(response);
        });
    };

    $scope.remove = function (item) {
       $http.get("removeItem", {params: { item: encodeURI(item) }})
            .then(function successCallback(response) {
                $scope.refresh();
            },
            function errorCallback(response) {
                alert(response);
            });
    };

    $scope.add = function (item) {
        if (!item) {return;}
        $http.get("addItem", {params: { item: encodeURI(item) }})
            .then(function successCallback(response) {
                $scope.refresh();
            },
            function errorCallback(response) {
                alert(response);
            });
    };
}]);
</script>

<div ng-app="VotingApp" ng-controller="VotingAppController" ng-init="refresh()">
    <div class="container-fluid">
        <div class="row">
            <div class="col-xs-8 col-xs-offset-2 text-center">
                <h2>Service Fabric Voting Sample</h2>
            </div>
        </div>

        <div class="row">
            <div class="col-xs-offset-2">
                <form style="width:50% ! important;" class="center-block">
                    <div class="col-xs-6 form-group">
                        <input id="txtAdd" type="text" class="form-control" placeholder="Add voting option" ng-model="item" />
                    </div>
                    <button id="btnAdd" class="btn btn-default" ng-click="add(item)">
                        <span class="glyphicon glyphicon-plus" aria-hidden="true"></span>
                        Add
                    </button>
                </form>
            </div>
        </div>

        <hr />

        <div class="row">
            <div class="col-xs-8 col-xs-offset-2">
                <div class="row">
                    <div class="col-xs-4">
                        Click to vote
                    </div>
                </div>
                <div class="row top-buffer" ng-repeat="(key, value)  in votes">
                    <div class="col-xs-8">
                        <button class="btn btn-success text-left btn-block" ng-click="add(key)">
                            <span class="pull-left">
                                {{key}}
                            </span>
                            <span class="badge pull-right">
                                {{value}} Votes
                            </span>
                        </button>
                    </div>
                    <div class="col-xs-4">
                        <button class="btn btn-danger pull-right btn-block" ng-click="remove(key)">
                            <span class="glyphicon glyphicon-remove" aria-hidden="true"></span>
                            Remove
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

</body>
</html>
```

### <a name="update-the-votingwebjava-file"></a>Aktualisieren der Datei „VotingWeb.java“

Öffnen Sie im Unterprojekt **VotingWeb** die Datei *VotingWeb/src/statelessservice/VotingWeb.java*. Der **VotingWeb**-Dienst ist das Gateway für den zustandslosen Dienst und ist für das Einrichten des Kommunikationslisteners für die Front-End-API zuständig.

Ersetzen Sie in der Datei den Inhalt der vorhandenen Methode **createServiceInstanceListeners** durch Folgendes, und speichern Sie Ihre Änderungen:

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {

    EndpointResourceDescription endpoint = this.getServiceContext().getCodePackageActivationContext().getEndpoint(webEndpointName);
    int port = endpoint.getPort();

    List<ServiceInstanceListener> listeners = new ArrayList<ServiceInstanceListener>();
    listeners.add(new ServiceInstanceListener((context) -> new HttpCommunicationListener(context, port)));
    return listeners;
}
```

### <a name="add-the-httpcommunicationlistenerjava-file"></a>Hinzufügen der Datei „HTTPCommunicationListener.java“

Der HTTP-Kommunikationslistener fungiert als Controller, mit dem der HTTP-Server eingerichtet und die APIs zum Definieren der Abstimmungsaktionen verfügbar gemacht werden. Klicken Sie mit der rechten Maustaste auf das Paket *statelessservice* im Ordner *VotingWeb/src/statelessservice*, und wählen Sie **Neu** > **Datei** aus.  Geben Sie der Datei den Namen *HttpCommunicationListener.java*, und wählen Sie **Fertig stellen** aus.

Ersetzen Sie den Inhalt der Datei durch Folgendes, und speichern Sie anschließend Ihre Änderungen:  Im Abschnitt „Aktualisieren der Datei ‚HttpCommunicationListener.java‘“ wird diese Datei dann geändert, um Abstimmungsdaten für den Back-End-Dienst zu rendern, zu lesen und zu schreiben.  Bisher gibt der Listener einfach nur die statischen HTML-Daten für die Abstimmungs-App zurück.

```java
// ------------------------------------------------------------
//  Copyright (c) Microsoft Corporation.  All rights reserved.
//  Licensed under the MIT License (MIT). See License.txt in the repo root for license information.
// ------------------------------------------------------------

package statelessservice;

import com.google.gson.Gson;
import com.sun.net.httpserver.HttpExchange;
import com.sun.net.httpserver.HttpHandler;
import com.sun.net.httpserver.Headers;

import java.io.File;
import java.io.OutputStream;
import java.io.FileInputStream;

import java.net.InetSocketAddress;
import java.net.URI;

import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.CompletableFuture;
import java.util.logging.Level;
import java.util.logging.Logger;

import microsoft.servicefabric.services.communication.runtime.CommunicationListener;
import microsoft.servicefabric.services.runtime.StatelessServiceContext;
import microsoft.servicefabric.services.client.ServicePartitionKey;
import microsoft.servicefabric.services.remoting.client.ServiceProxyBase;
import microsoft.servicefabric.services.communication.client.TargetReplicaSelector;
import system.fabric.CancellationToken;

public class HttpCommunicationListener implements CommunicationListener {

    private static final Logger logger = Logger.getLogger(HttpCommunicationListener.class.getName());

    private static final String HEADER_CONTENT_TYPE = "Content-Type";
    private static final int STATUS_OK = 200;
    private static final int STATUS_NOT_FOUND = 404;
    private static final int STATUS_ERROR = 500;
    private static final String RESPONSE_NOT_FOUND = "404 (Not Found) \n";
    private static final String MIME = "text/html";
    private static final String ENCODING = "UTF-8";

    private static final String ROOT = "wwwroot/";
    private static final String FILE_NAME = "index.html";
    private StatelessServiceContext context;
    private com.sun.net.httpserver.HttpServer server;
    private ServicePartitionKey partitionKey;
    private final int port;

    public HttpCommunicationListener(StatelessServiceContext context, int port) {
        this.partitionKey = new ServicePartitionKey(0);
        this.context = context;
        this.port = port;
    }

    // Called by openAsync when the class is instantiated
    public void start() {
        try {
            logger.log(Level.INFO, "Starting Server");
            server = com.sun.net.httpserver.HttpServer.create(new InetSocketAddress(this.port), 0);
        } catch (Exception ex) {
            logger.log(Level.SEVERE, null, ex);
            throw new RuntimeException(ex);
        }

        // Responsible for rendering the HTML layout described in the previous step
        server.createContext("/", new HttpHandler() {
            @Override
            public void handle(HttpExchange t) {
                try {
                    File file = new File(ROOT + FILE_NAME).getCanonicalFile();

                    if (!file.isFile()) {
                      // Object does not exist or is not a file: reject with 404 error.
                      t.sendResponseHeaders(STATUS_NOT_FOUND, RESPONSE_NOT_FOUND.length());
                      OutputStream os = t.getResponseBody();
                      os.write(RESPONSE_NOT_FOUND.getBytes());
                      os.close();
                    } else {
                      Headers h = t.getResponseHeaders();
                      h.set(HEADER_CONTENT_TYPE, MIME);
                      t.sendResponseHeaders(STATUS_OK, 0);
    
                      OutputStream os = t.getResponseBody();
                      FileInputStream fs = new FileInputStream(file);
                      final byte[] buffer = new byte[0x10000];
                      int count = 0;
                      while ((count = fs.read(buffer)) >= 0) {
                        os.write(buffer,0,count);
                      }

                      fs.close();
                      os.close();
                    }
                } catch (Exception e) {
                    logger.log(Level.WARNING, null, e);
                }
            }
        });

        /*
        [Replace this entire comment block in the 'Connect the services' section]
        */

        server.setExecutor(null);
        server.start();
    }

    //Helper method to parse raw HTTP requests
    private Map<String, String> queryToMap(String query){
        Map<String, String> result = new HashMap<String, String>();
        for (String param : query.split("&")) {
            String pair[] = param.split("=");
            if (pair.length>1) {
                result.put(pair[0], pair[1]);
            }else{
                result.put(pair[0], "");
            }
        }
        return result;
    }

    //Called closeAsync when the service is shut down
    private void stop() {
        if (null != server)
            server.stop(0);
    }

    //Called by the Service Fabric runtime when this service is created on a node
    @Override
    public CompletableFuture<String> openAsync(CancellationToken cancellationToken) {
        this.start();
                    logger.log(Level.INFO, "Opened Server");
        String publishUri = String.format("http://%s:%d/", this.context.getNodeContext().getIpAddressOrFQDN(), port);
        return CompletableFuture.completedFuture(publishUri);
    }

    //Called by the Service Fabric runtime when the service is shut down
    @Override
    public CompletableFuture<?> closeAsync(CancellationToken cancellationToken) {
        this.stop();
        return CompletableFuture.completedFuture(true);
    }

    //Called by the Service Fabric runtime to forcibly shut this listener down
    @Override
    public void abort() {
        this.stop();
    }
}
```

### <a name="configure-the-listening-port"></a>Konfigurieren des Lauschports

Beim Erstellen des Front-End-Diensts für den VotingWeb-Dienst wählt Service Fabric einen Port aus, an dem der Dienst lauscht.  Der VotingWeb-Dienst fungiert als Front-End für diese Anwendung und akzeptiert externen Datenverkehr. Daher binden wir diesen Dienst an einen festen und bekannten Port. Öffnen Sie im Paket-Explorer die Datei *VotingApplication/VotingWebPkg/ServiceManifest.xml*.  Suchen Sie im Abschnitt **Resources** die Ressource **Endpoint**, und ändern Sie den Wert **Port** in „8080“. (Dieser Port wird im weiteren Verlauf des Tutorials verwendet.) Wenn Sie die Anwendung lokal bereitstellen und ausführen möchten, muss der Anwendungslauschport auf dem Computer geöffnet und verfügbar sein. Fügen Sie den folgenden Codeausschnitt ins Element **ServiceManifest** (d.h. direkt unter dem ```<DataPackage>```-Element) ein.

```xml
<Resources>
    <Endpoints>
        <!-- This endpoint is used by the communication listener to obtain the port on which to
            listen. Please note that if your service is partitioned, this port is shared with
            replicas of different partitions that are placed in your code. -->
        <Endpoint Name="WebEndpoint" Protocol="http" Port="8080" />
    </Endpoints>
  </Resources>
```

## <a name="add-a-stateful-back-end-service-to-your-application"></a>Hinzufügen eines zustandsbehafteten Back-End-Diensts zur Anwendung

Nachdem das Gerüst des Java-Web-API-Diensts nun fertig ist, können wir uns dem zustandsbehafteten Back-End-Dienst zuwenden.

Service Fabric ermöglicht eine konsistente und zuverlässige Speicherung von Daten direkt innerhalb des Diensts anhand von Reliable Collections. Reliable Collections umfassen hoch verfügbare und zuverlässige Sammlungsklassen. Mit der Nutzung dieser Klassen sind alle Benutzer vertraut, die bereits mit Java-Sammlungen gearbeitet haben.

1. Klicken Sie im Paket-Explorer mit der rechten Maustaste im Anwendungsprojekt auf **Voting**, und wählen Sie **Service Fabric** > **Add Service Fabric Service** (Service Fabric-Dienst hinzufügen) aus.

2. Wählen Sie im Dialogfeld **Dienst hinzufügen** die Option **Zustandsbehafteter Dienst** aus, geben Sie dem Dienst den Namen **VotingDataService**, und wählen Sie **Dienst hinzufügen** aus.

    Nach Erstellung Ihres Dienstprojekts stehen Ihnen zwei Dienste in Ihrer Anwendung zur Verfügung. Bei der weiteren Erstellung Ihrer Anwendung können Sie weitere Dienste auf die gleiche Weise hinzufügen. Diese können unabhängig versioniert und aktualisiert werden.

3. Eclipse erstellt ein Dienstprojekt und zeigt es im Paket-Explorer an.

    ![Projekt-Explorer von Eclipse](./media/service-fabric-tutorial-create-java-app/service-fabric-package-explorer-java.png)

### <a name="add-the-votingdataservicejava-file"></a>Hinzufügen der Datei „VotingDataService.java“

Die Datei *VotingDataService.java* enthält die Methoden mit der Logik zum Abrufen, Hinzufügen und Entfernen von Stimmen aus den Reliable Collections. Fügen Sie die folgenden Methoden der **VotingDataService**-Klasse in der Datei *VotingDataService/src/statefulservice/VotingDataService.java* hinzu.

```java
package statefulservice;

import java.util.HashMap;
import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.CompletableFuture;
import java.util.concurrent.atomic.AtomicInteger;

import microsoft.servicefabric.services.communication.runtime.ServiceReplicaListener;

import microsoft.servicefabric.services.runtime.StatefulService;

import microsoft.servicefabric.services.remoting.fabrictransport.runtime.FabricTransportServiceRemotingListener;

import microsoft.servicefabric.data.ReliableStateManager;
import microsoft.servicefabric.data.Transaction;
import microsoft.servicefabric.data.collections.ReliableHashMap;
import microsoft.servicefabric.data.utilities.AsyncEnumeration;
import microsoft.servicefabric.data.utilities.KeyValuePair;

import system.fabric.StatefulServiceContext;

import rpcmethods.VotingRPC;

class VotingDataService extends StatefulService implements VotingRPC {
    private static final String MAP_NAME = "votesMap";
    private ReliableStateManager stateManager;

    protected VotingDataService (StatefulServiceContext statefulServiceContext) {
        super (statefulServiceContext);
    }

    @Override
    protected List<ServiceReplicaListener> createServiceReplicaListeners() {
        this.stateManager = this.getReliableStateManager();
        ArrayList<ServiceReplicaListener> listeners = new ArrayList<>();

        listeners.add(new ServiceReplicaListener((context) -> {
            return new FabricTransportServiceRemotingListener(context,this);
        }));

        return listeners;
    }

    // Method that will be invoked via RPC from the front end to retrieve the complete set of votes in the map
    public CompletableFuture<HashMap<String,String>> getList() {
        HashMap<String, String> tempMap = new HashMap<String, String>();

        try {

            ReliableHashMap<String, String> votesMap = stateManager
                    .<String, String> getOrAddReliableHashMapAsync(MAP_NAME).get();

            Transaction tx = stateManager.createTransaction();
            AsyncEnumeration<KeyValuePair<String, String>> kv = votesMap.keyValuesAsync(tx).get();
            while (kv.hasMoreElementsAsync().get()) {
                KeyValuePair<String, String> k = kv.nextElementAsync().get();
                tempMap.put(k.getKey(), k.getValue());
            }

            tx.close();


        } catch (Exception e) {
            e.printStackTrace();
        }

        return CompletableFuture.completedFuture(tempMap);
    }

    // Method that will be invoked via RPC from the front end to add an item to the votes list or to increase the
    // vote count for a particular item
    public CompletableFuture<Integer> addItem(String itemToAdd) {
        AtomicInteger status = new AtomicInteger(-1);

        try {

            ReliableHashMap<String, String> votesMap = stateManager
                    .<String, String> getOrAddReliableHashMapAsync(MAP_NAME).get();

            Transaction tx = stateManager.createTransaction();
            votesMap.computeAsync(tx, itemToAdd, (k, v) -> {
                if (v == null) {
                    return "1";
                }
                else {
                    int numVotes = Integer.parseInt(v);
                    numVotes = numVotes + 1;
                    return Integer.toString(numVotes);
                }
            }).get();

            tx.commitAsync().get();
            tx.close();

            status.set(1);
        } catch (Exception e) {
            e.printStackTrace();
        }

        return CompletableFuture.completedFuture(new Integer(status.get()));
    }

    // Method that will be invoked via RPC from the front end to remove an item
    public CompletableFuture<Integer> removeItem(String itemToRemove) {
        AtomicInteger status = new AtomicInteger(-1);
        try {
            ReliableHashMap<String, String> votesMap = stateManager
                    .<String, String> getOrAddReliableHashMapAsync(MAP_NAME).get();

            Transaction tx = stateManager.createTransaction();
            votesMap.removeAsync(tx, itemToRemove).get();
            tx.commitAsync().get();
            tx.close();

            status.set(1);
        } catch (Exception e) {
            e.printStackTrace();
        }

        return CompletableFuture.completedFuture(new Integer(status.get()));
    }

}
```

Das Gerüst für den zustandslosen Front-End-Dienst und den Back-End-Dienst wird jetzt erstellt.

## <a name="create-the-communication-interface-to-your-application"></a>Erstellen der Kommunikationsschnittstelle für Ihre Anwendung

 Als Nächstes wird eine Verbindung zwischen dem zustandslosen Front-End-Dienst und dem Back-End-Dienst hergestellt. Beide Dienste nutzen die Schnittstelle „VotingRPC“, die die Vorgänge der Abstimmungsanwendung definiert. Diese Schnittstelle wird vom Front-End- und Back-End-Dienst implementiert, um Remoteprozeduraufrufe (RPCs) zwischen den beiden Diensten zu ermöglichen. Da Eclipse das Hinzufügen von Gradle-Unterprojekten leider nicht unterstützt, muss das Paket, das diese Schnittstelle enthält, manuell hinzugefügt werden.

1. Klicken Sie im Paket-Explorer mit der rechten Maustaste auf das Projekt **Voting**, und wählen Sie dann **Neu** > **Ordner** aus. Nennen Sie den Ordner **VotingRPC/src/rpcmethods**.

    ![Erstellen eines VotingRPC-Pakets im Projekt-Explorer von Eclipse](./media/service-fabric-tutorial-create-java-app/create-voting-rpc-package-java.png)

3. Erstellen Sie unter *Voting/VotingRPC/src/rpcmethods* eine Datei mit dem Namen *VotingRPC.java*, und fügen Sie Folgendes in **diese Datei** ein. 

    ```java
    package rpcmethods;
    
    import java.util.ArrayList;
    import java.util.concurrent.CompletableFuture;
    import java.util.List;
    import java.util.HashMap;
    
    import microsoft.servicefabric.services.remoting.Service;
    
    public interface VotingRPC extends Service {
        CompletableFuture<HashMap<String, String>> getList();
    
        CompletableFuture<Integer> addItem(String itemToAdd);
    
        CompletableFuture<Integer> removeItem(String itemToRemove);
    }
    ```

4. Erstellen Sie im Verzeichnis *Voting/VotingRPC* eine leere Datei mit dem Namen *build.gradle*, und fügen Sie Folgendes in diese Datei ein. Diese Gradle-Datei wird verwendet, um für die JAR-Datei, die von den anderen Diensten importiert wird, den Buildvorgang und die Erstellung durchzuführen. 

    ```gradle
    apply plugin: 'java'
    apply plugin: 'eclipse'
    
    sourceSets {
      main {
         java.srcDirs = ['src']
         output.classesDir = 'out/classes'
          resources {
           srcDirs = ['src']
         }
       }
    }
    
    clean.doFirst {
        delete "${projectDir}/out"
        delete "${projectDir}/VotingRPC.jar"
    }
    
    repositories {
        mavenCentral()
    }
    
    dependencies {
        compile ('com.microsoft.servicefabric:sf-actors:1.0.0')
    }
    
    jar {
        from configurations.compile.collect {
            (it.isDirectory() && !it.getName().contains("native")) ? it : zipTree(it)}

        manifest {
                attributes(
                'Main-Class': 'rpcmethods.VotingRPC')
            baseName "VotingRPC"
            destinationDir = file('./')
        }
    
        exclude 'META-INF/*.RSA', 'META-INF/*.SF','META-INF/*.DSA'
    }

    defaultTasks 'clean', 'jar'
    ```

5. Fügen Sie in der Datei *Voting/settings.gradle* eine Zeile hinzu, um das neu erstellte Projekt in den Build einzufügen. 

    ```gradle
    include ':VotingRPC'
    ```

6. Ersetzen Sie in der Datei *Voting/VotingWeb/src/statelessservice/HttpCommunicationListener.java* den Kommentarblock durch Folgendes:  

    ```java
    server.createContext("/getStatelessList", new HttpHandler() {
        @Override
        public void handle(HttpExchange t) {
            try {
                t.sendResponseHeaders(STATUS_OK,0);
                OutputStream os = t.getResponseBody();
    
                HashMap<String,String> list = ServiceProxyBase.create(VotingRPC.class, new URI("fabric:/VotingApplication/VotingDataService"), partitionKey, TargetReplicaSelector.DEFAULT, "").getList().get();
                String json = new Gson().toJson(list);
                os.write(json.getBytes(ENCODING));
                os.close();
            } catch (Exception e) {
                logger.log(Level.WARNING, null, e);
            }
        }
    });
    
    server.createContext("/removeItem", new HttpHandler() {
        @Override
        public void handle(HttpExchange t) {
            try {
                OutputStream os = t.getResponseBody();
                URI r = t.getRequestURI();
    
                Map<String, String> params = queryToMap(r.getQuery());
                String itemToRemove = params.get("item");

                Integer num = ServiceProxyBase.create(VotingRPC.class, new URI("fabric:/VotingApplication/VotingDataService"), partitionKey, TargetReplicaSelector.DEFAULT, "").removeItem(itemToRemove).get();
    
                if (num != 1)
                {
                    t.sendResponseHeaders(STATUS_ERROR, 0);
                } else {
                    t.sendResponseHeaders(STATUS_OK,0);
                }
    
                String json = new Gson().toJson(num);
                os.write(json.getBytes(ENCODING));
                os.close();
            } catch (Exception e) {
                logger.log(Level.WARNING, null, e);
            }
    
        }
    });
    
    server.createContext("/addItem", new HttpHandler() {
        @Override
        public void handle(HttpExchange t) {
            try {
                URI r = t.getRequestURI();
                Map<String, String> params = queryToMap(r.getQuery());
                String itemToAdd = params.get("item");

                OutputStream os = t.getResponseBody();
                Integer num = ServiceProxyBase.create(VotingRPC.class, new URI("fabric:/VotingApplication/VotingDataService"), partitionKey, TargetReplicaSelector.DEFAULT, "").addItem(itemToAdd).get();
                if (num != 1)
                {
                    t.sendResponseHeaders(STATUS_ERROR, 0);
                } else {
                    t.sendResponseHeaders(STATUS_OK,0);
                }
    
                String json = new Gson().toJson(num);
                os.write(json.getBytes(ENCODING));
                os.close();
            } catch (Exception e) {
                logger.log(Level.WARNING, null, e);
            }
        }
    });
    ```
7. Fügen Sie oben in der Datei *Voting/VotingWeb/src/statelessservice/HttpCommunicationListener.java* die entsprechende Importanweisung hinzu. 

    ```java
    import rpcmethods.VotingRPC; 
    ```

Die Funktionalität für das Front-End, das Back-End und die RPC-Schnittstellen ist jetzt vorhanden. Die nächste Phase umfasst das richtige Konfigurieren der Gradle-Skripts, bevor die Bereitstellung in einem Service Fabric-Cluster erfolgt. 

## <a name="walk-through-the-voting-sample-application"></a>Durchlaufen der Beispielanwendung für die Abstimmung
Die Abstimmungsanwendung besteht aus zwei Diensten:
- Web-Front-End-Dienst (VotingWeb): Ein Java-Web-Front-End-Dienst, der die Webseite bereitstellt und APIs für die Kommunikation mit dem Back-End-Dienst verfügbar macht.
- Back-End-Dienst (VotingDataService): Ein Java-Webdienst, der über Remoteprozeduraufrufe (RPCs) aufgerufene Methoden definiert, um Stimmen dauerhaft zu speichern.

![Abstimmungsbeispieldiagramm](./media/service-fabric-tutorial-create-java-app/walkthrough-java-voting.png)

Wenn Sie in der Anwendung eine Aktion durchführen (Element hinzufügen, Abstimmen, Element entfernen), treten die folgenden Ereignisse ein:
1. Ein JavaScript sendet die entsprechende Anforderung als HTTP-Anforderung an die Web-API im Web-Front-End-Dienst.

2. Der Web-Front-End-Dienst verwendet die integrierte Dienstremotingfunktionalität von Service Fabric, um die Anforderung zu ermitteln und an den Back-End-Dienst weiterzuleiten. 

3. Mit dem Back-End-Dienst werden Methoden definiert, mit denen das Ergebnis in einem Reliable Dictionary aktualisiert wird. Der Inhalt dieses Reliable Dictionary wird auf mehreren Knoten im Cluster repliziert und dauerhaft auf Datenträger gespeichert. Alle Daten der Anwendung werden im Cluster gespeichert. 

## <a name="configure-gradle-scripts"></a>Konfigurieren von Gradle-Skripts 

In diesem Abschnitt werden die Gradle-Skripts für das Projekt konfiguriert. 

1. Ersetzen Sie den Inhalt der Datei *Voting/build.gradle* durch Folgendes.

    ```gradle
    apply plugin: 'java'
    apply plugin: 'eclipse'
    
    subprojects {
        apply plugin: 'java'
    }
    
    defaultTasks 'clean', 'jar', 'copyDeps'
    ```

2. Ersetzen Sie den Inhalt der Datei *Voting/VotingWeb/build.gradle* durch Folgendes:

    ```gradle
    apply plugin: 'java'
    apply plugin: 'eclipse'
    
    sourceSets {
      main {
         java.srcDirs = ['src']
         output.classesDir = 'out/classes'
          resources {
           srcDirs = ['src']
         }
       }
    }
    
    clean.doFirst {
        delete "${projectDir}/../lib"
        delete "${projectDir}/out"
        delete "${projectDir}/../VotingApplication/VotingWebPkg/Code/lib"
        delete "${projectDir}/../VotingApplication/VotingWebPkg/Code/VotingWeb.jar"
    }
    
    repositories {
        mavenCentral()
    }
    
    dependencies {
        compile ('com.microsoft.servicefabric:sf-actors:1.0.0-preview1')
        compile project(':VotingRPC')
    }
    
    task explodeDeps(type: Copy, dependsOn:configurations.compile) { task ->
        configurations.compile.filter {!it.toString().contains("native")}.each{
            from it
        }
    
        configurations.compile.filter {it.toString().contains("native")}.each{
            from zipTree(it)
        }
        into "../lib/"
        include "lib*.so", "*.jar"
    }
    
    task copyDeps<< {
        copy {
            from("../lib/")
            into("../VotingApplication/VotingWebPkg/Code/lib")
            include('lib*.so')
        }
    }
    
    compileJava.dependsOn(explodeDeps)
    
    jar {
        from configurations.compile.collect {(it.isDirectory() && !it.getName().contains("native")) ? it : zipTree(it)}
    
        manifest {
            attributes(
                'Main-Class': 'statelessservice.VotingWebServiceHost')
            baseName "VotingWeb"
            destinationDir = file('../VotingApplication/VotingWebPkg/Code/')
        }
    
        exclude 'META-INF/*.RSA', 'META-INF/*.SF','META-INF/*.DSA'
    }
    
    defaultTasks 'clean', 'jar', 'copyDeps'
    ```

3. Ersetzen Sie den Inhalt der Datei *Voting/VotingData/build.gradle*: 

    ```gradle
    apply plugin: 'java'
    apply plugin: 'eclipse'
    
    sourceSets {
      main {
         java.srcDirs = ['src']
         output.classesDir = 'out/classes'
          resources {
           srcDirs = ['src']
         }
       }
    }
    
    clean.doFirst {
        delete "${projectDir}/../lib"
        delete "${projectDir}/out"
        delete "${projectDir}/../VotingApplication/VotingDataServicePkg/Code/lib"
        delete "${projectDir}/../VotingApplication/VotingDataServicePkg/Code/VotingDataService.jar"
    }
    
    repositories {
        mavenCentral()
    }
    
    dependencies {
        compile ('com.microsoft.servicefabric:sf-actors:1.0.0-preview1')
        compile project(':VotingRPC')
    }
    
    task explodeDeps(type: Copy, dependsOn:configurations.compile) { task ->
        configurations.compile.filter {!it.toString().contains("native")}.each{
            from it
        }
    
        configurations.compile.filter {it.toString().contains("native")}.each{
            from zipTree(it)
        }
        into "../lib/"
        include "lib*.so", "*.jar"
    }
    
    compileJava.dependsOn(explodeDeps)
    
    task copyDeps<< {
        copy {
            from("../lib/")
            into("../VotingApplication/VotingDataServicePkg/Code/lib")
            include('lib*.so')
        }
    }
    
    jar {
        from configurations.compile.collect {
            (it.isDirectory() && !it.getName().contains("native")) ? it : zipTree(it)}
    
        manifest {
            attributes('Main-Class': 'statefulservice.VotingDataServiceHost')
    
            baseName "VotingDataService"
            destinationDir = file('../VotingApplication/VotingDataServicePkg/Code/')
        }
    
        exclude 'META-INF/*.RSA', 'META-INF/*.SF','META-INF/*.DSA'
    }
    
    defaultTasks 'clean', 'jar', 'copyDeps'
    ```

## <a name="deploy-application-to-local-cluster"></a>Bereitstellen der Anwendung in einem lokalen Cluster

An diesem Punkt ist die Anwendung bereit für die Bereitstellung in einem lokalen Service Fabric-Cluster.

1. Klicken Sie im Paket-Explorer mit der rechten Maustaste auf das Projekt **Voting**, und wählen Sie **Service Fabric** > **Build Application** (Anwendung erstellen) aus, um Ihre Anwendung zu erstellen.

2. Führen Sie Ihren lokalen Service Fabric-Cluster aus. Dieser Schritt hängt von Ihrer Entwicklungsumgebung (Mac oder Linux) ab.

    Wenn Sie einen Mac verwenden, führen Sie den lokalen Cluster mit dem folgenden Befehl aus: Ersetzen Sie den in den Parameter **-v** übergebenen Parameter durch den Pfad zu Ihrem eigenen Arbeitsbereich.

    ```bash
    docker run -itd -p 19080:19080 -p 8080:8080 -p --name sfonebox servicefabricoss/service-fabric-onebox
    ```
    Ausführlichere Anweisungen finden Sie im [Einrichtungsleitfaden für OS X](service-fabric-get-started-mac.md).

    Bei der Ausführung auf einem Linux-Computer starten Sie den lokalen Cluster mit dem folgenden Befehl: 

    ```bash 
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```
    Ausführlichere Anweisungen finden Sie im [Einrichtungsleitfaden für Linux](service-fabric-get-started-linux.md).

4. Klicken Sie im Paket-Explorer für Eclipse mit der rechten Maustaste auf das Projekt **Voting**, und wählen Sie **Service Fabric** > **Publish Application** (Anwendung veröffentlichen) aus. 
5. Wählen Sie im Fenster **Publish Application** (Anwendung veröffentlichen) in der Dropdownliste die Option **Local.json** und anschließend **Publish** (Veröffentlichen) aus.
6. Navigieren Sie in Ihrem Webbrowser zu „http:\//localhost:8080“, um Ihre ausgeführte Anwendung im lokalen Service Fabric-Cluster anzuzeigen. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Teil des Tutorials haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines Java-Diensts als zustandsbehafteten zuverlässigen Dienst
> * Erstellen eines Java-Diensts als zustandslosen Webdienst
> * Hinzufügen einer Java-Schnittstelle zur Verarbeitung von Remoteprozeduraufrufen (RPCs) zwischen Ihren Diensten
> * Konfigurieren Ihrer Gradle-Skripts
> * Erstellen und Bereitstellen Ihrer Anwendung in einem lokalen Service Fabric-Cluster

Fahren Sie mit dem nächsten Tutorial fort:
> [!div class="nextstepaction"]
> [Debuggen und Protokollieren von Anwendungen in einem lokalen Cluster](service-fabric-tutorial-debug-log-local-cluster.md)

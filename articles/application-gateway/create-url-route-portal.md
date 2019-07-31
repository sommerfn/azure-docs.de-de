---
title: 'Tutorial: Erstellen eines Anwendungsgateways mit Routingregeln auf URL-Pfadbasis – Azure-Portal'
description: In diesem Tutorial erfahren Sie, wie Sie mit dem Azure-Portal Routingregeln auf URL-Pfadbasis für ein Anwendungsgateway und eine VM-Skalierungsgruppe erstellen.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: tutorial
ms.date: 07/27/2019
ms.author: victorh
ms.openlocfilehash: 63a1faa79374e72eabfbee4ece454728c3b4cc05
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68597612"
---
# <a name="tutorial-create-an-application-gateway-with-path-based-routing-rules-using-the-azure-portal"></a>Tutorial: Erstellen eines Anwendungsgateways mit pfadbasierten Routingregeln mithilfe des Azure-Portals

Sie können mit dem Azure-Portal [Routingregeln auf URL-Pfadbasis](url-route-overview.md) konfigurieren, wenn Sie ein [Anwendungsgateway](overview.md) erstellen. In diesem Tutorial erstellen Sie Back-End-Pools mithilfe von virtuellen Computern. Anschließend erstellen Sie Routingregeln, die sicherstellen, dass Webdatenverkehr an die richtigen Server in den Pools gesendet wird.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Anwendungsgateways
> * Erstellen von virtuellen Computern für Back-End-Server
> * Erstellen von Back-End-Pools mit den Back-End-Servern
> * Erstellen eines Back-End-Listeners
> * Erstellen einer pfadbasierten Routingregel

![URL-Routingbeispiel](./media/create-url-route-portal/scenario.png)

Dieser Artikel kann auch mit der [Azure-Befehlszeilenschnittstelle](tutorial-url-route-cli.md) oder mit [Azure PowerShell](tutorial-url-route-powershell.md) durchgearbeitet werden.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="create-an-application-gateway"></a>Erstellen eines Anwendungsgateways

1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressource erstellen**. Das Fenster **Neu** wird angezeigt.

2. Klicken Sie auf **Netzwerk**, und wählen Sie dann in der Liste **Ausgewählte** die Option **Application Gateway** aus.

### <a name="basics-tab"></a>Registerkarte „Grundlagen“

1. Geben Sie auf der Registerkarte **Grundlagen** die folgenden Werte für die Einstellungen des Anwendungsgateways ein:

   - **Ressourcengruppe**: Wählen Sie **myResourceGroupAG** als Ressourcengruppe aus. Falls diese Gruppe nicht vorhanden ist, wählen Sie **Neue erstellen** aus, um sie zu erstellen.
   - **Name des Anwendungsgateways**: Geben Sie *myAppGateway* als Namen des Anwendungsgateways ein.

     ![Erstellen eines neuen Anwendungsgateways: Grundlagen](./media/application-gateway-create-gateway-portal/application-gateway-create-basics.png)

2.  Für die Kommunikation in Azure zwischen den von Ihnen erstellten Ressourcen ist ein virtuelles Netzwerk erforderlich. Sie können ein neues virtuelles Netzwerk erstellen oder ein bereits vorhandenes virtuelles Netzwerk auswählen. In diesem Beispiel erstellen Sie gleichzeitig mit dem Anwendungsgateway ein virtuelles Netzwerk. Application Gateway-Instanzen werden in separaten Subnetzen erstellt. In diesem Beispiel erstellen Sie zwei Subnetze: eins für das Anwendungsgateway und eins für die Back-End-Server.

    Wählen Sie unter **Virtuelles Netzwerk konfigurieren** die Option **Neu erstellen** aus, um ein neues virtuelles Netzwerk zu erstellen. Geben Sie im Fenster **Virtuelles Netzwerk erstellen**, das geöffnet wird, die folgenden Werte ein,um das virtuelle Netzwerk und zwei Subnetze zu erstellen:

    - **Name**: Geben Sie *myVNet* als Namen des virtuellen Netzwerks ein.

    - **Subnetzname** (Application Gateway-Subnetz): Im Raster **Subnetze** wird ein Subnetz namens *Default* angezeigt. Ändern Sie den Namen dieses Subnetzes in *myAGSubnet*.

      Das Subnetz für das Anwendungsgateway kann nur Anwendungsgateways enthalten. Andere Ressourcen sind nicht zulässig.

    - **Subnetzname** (Subnetz für Back-End-Server): Geben Sie in der zweiten Zeile des Rasters **Subnetze** in der Spalte *Subnetzname* den Namen **myBackendSubnet** ein.

    - **Adressbereich** (Subnetz für Back-End-Server): Geben Sie in der zweiten Zeile des Rasters **Subnetze** einen Adressbereich ein, der sich nicht mit dem Adressbereich des Subnetzes *myAGSubnet* überschneidet. Wenn der Adressbereich von *myAGSubnet* beispielsweise 10.0.0.0/24 lautet, geben Sie *10.0.1.0/24* als Adressbereich für *myBackendSubnet* ein.

    Wählen Sie **OK** aus, um das Fenster **Virtuelles Netzwerk erstellen** zu schließen und die Einstellungen für das virtuelle Netzwerk zu speichern.

     ![Erstellen eines neuen Anwendungsgateways: virtuelles Netzwerk](./media/application-gateway-create-gateway-portal/application-gateway-create-vnet.png)
    
3. Übernehmen Sie auf der Registerkarte **Grundlagen** die Standardwerte für die übrigen Einstellungen, und wählen Sie dann **Weiter: Front-Ends** aus.

### <a name="frontends-tab"></a>Registerkarte „Front-Ends“

1. Vergewissern Sie sich auf der Registerkarte **Front-Ends**, dass der **Typ der Front-End-IP-Adresse** auf **Öffentlich** festgelegt ist. <br>Je nach Anwendungsfall können Sie konfigurieren, dass die Front-End-IP-Adresse öffentlich oder privat ist. In diesem Beispiel verwenden Sie eine öffentliche Front-End-IP-Adresse.
   > [!NOTE]
   > Für die Application Gateway v2-SKU können Sie nur die **öffentliche** Front-End-IP-Konfiguration wählen. Die private Front-End-IP-Konfiguration ist derzeit für diese v2-SKU nicht aktiviert.

2. Wählen Sie **Neu erstellen** für die **Öffentliche IP-Adresse** aus, und geben Sie *myAGPublicIPAddress* als Namen der öffentlichen IP-Adresse ein. Wählen Sie dann **OK** aus. 

     ![Erstellen eines neuen Anwendungsgateways: Front-Ends](./media/application-gateway-create-gateway-portal/application-gateway-create-frontends.png)

3. Klicken Sie auf **Weiter: Back-Ends**.

### <a name="backends-tab"></a>Registerkarte „Back-Ends“

Der Back-End-Pool wird zum Weiterleiten von Anforderungen an die Back-End-Server verwendet, die die Anforderung verarbeiten. Back-End-Pools können Netzwerkadapter, VM-Skalierungsgruppen, öffentliche IP-Adressen, interne IP-Adressen, vollqualifizierte Domänennamen (Fully Qualified Domain Names, FQDN) und Back-Ends mit mehreren Mandanten wie Azure App Service sein. In diesem Beispiel erstellen Sie mit Ihrem Anwendungsgateway einen leeren Back-End-Pool und fügen dann Back-End-Ziele zum Back-End-Pool hinzu.

1. Wählen Sie auf der Registerkarte **Back-Ends** die Option **+Back-End-Pool hinzufügen** aus.

2. Geben Sie im Fenster **Back-End-Pool hinzufügen**, das geöffnet wird, die folgenden Werte ein, um einen leeren Back-End-Pool zu erstellen:

    - **Name**: Geben Sie *appGatewayBackendPool* als Name des Back-End-Pools ein.
    - **Back-End-Pool ohne Ziele hinzufügen**: Wählen Sie **Ja** aus, um einen Back-End-Pool ohne Ziele zu erstellen. Back-End-Ziele werden Sie nach dem Erstellen des Anwendungsgateways hinzufügen.

3. Wählen Sie im Fenster **Back-End-Pool hinzufügen** die Option **Hinzufügen** aus, um die Konfiguration des Back-End-Pools zu speichern und zur Registerkarte **Back-Ends** zurückzukehren.
4. Fügen Sie nun zwei weitere Back-End-Pools namens *imagesBackendPool* und *videoBackendPool* hinzu.

     ![Erstellen eines neuen Anwendungsgateways: Back-Ends](./media/create-url-route-portal/backends.png)

4. Wählen Sie auf der Registerkarte **Back-Ends** die Option **Weiter: Konfiguration** aus.

### <a name="configuration-tab"></a>Registerkarte „Konfiguration“

Auf der Registerkarte **Konfiguration** verbinden Sie das Front-End und die von Ihnen erstellten Back-End-Pools mithilfe von Routingregeln.

1. Wählen Sie **Regel hinzufügen** in der Spalte **Routingregeln** aus.
2. Das Fenster **Routingregel hinzufügen** wird geöffnet. Geben Sie dort *Rule1* als **Regelname** ein.
3. Eine Routingregel erfordert einen Listener. Geben Sie im Fenster **Routingregel hinzufügen** auf der Registerkarte **Listener** die folgenden Werte für den Listener ein:

    - **Name des Listeners**: Geben Sie *DefaultListener* für den Namen des Listeners ein.
    - **Front-End-IP**: Wählen Sie **Öffentlich** aus, um die für das Front-End erstellte öffentliche IP-Adresse auszuwählen.

   Übernehmen Sie auf der Registerkarte **Listener** die Standardwerte für die übrigen Einstellungen. Wählen Sie dann die Registerkarte **Back-End-Ziele** aus, um den Rest der Routingregel zu konfigurieren.
4. Wählen Sie auf der Registerkarte **Back-End-Ziele** den Pool **appGatewayBackendPool** als **Back-End-Ziel** aus.

5. Wählen Sie für die **HTTP-Einstellung** die Option **Neu erstellen** aus, um eine neue HTTP-Einstellung zu erstellen. Die HTTP-Einstellung bestimmt das Verhalten der Routingregel. Geben Sie im Fenster **HTTP-Einstellung hinzufügen**, das geöffnet wird, *myHTTPSetting* als **Name der HTTP-Einstellung** ein. Übernehmen Sie im Fenster **HTTP-Einstellung hinzufügen** die Standardwerte für die übrigen Einstellungen, und wählen Sie dann **Hinzufügen** aus, um zum Fenster **Routingregel hinzufügen** zurückzukehren. 

6. Wählen Sie im Fenster **Routingregel hinzufügen** die Option **Hinzufügen** aus, um die Routingregel zu speichern und zur Registerkarte **Konfiguration** zurückzukehren.



1. Wählen Sie **Regel hinzufügen** in der Spalte **Routingregeln** aus.

2. Das Fenster **Routingregel hinzufügen** wird geöffnet. Geben Sie dort *Rule2* als **Regelname** ein.

3. Eine Routingregel erfordert einen Listener. Geben Sie im Fenster **Routingregel hinzufügen** auf der Registerkarte **Listener** die folgenden Werte für den Listener ein:

    - **Name des Listeners**: Geben Sie *myBackendListener* als Name für den Listener ein.
    - **Front-End-IP**: Wählen Sie **Öffentlich** aus, um die für das Front-End erstellte öffentliche IP-Adresse auszuwählen.
    - **Port:** 8080

   Unter **Zusätzliche Einstellungen**:
   - **Listenertyp**: Basic

   Übernehmen Sie auf der Registerkarte **Listener** die Standardwerte für die übrigen Einstellungen. Wählen Sie dann die Registerkarte **Back-End-Ziele** aus, um den Rest der Routingregel zu konfigurieren.

4. Wählen Sie auf der Registerkarte **Back-End-Ziele** den Pool **appGatewayBackendPool** als **Back-End-Ziel** aus.

5. Wählen Sie unter **HTTP-Einstellung** die Option *myHTTPSetting* aus. Übernehmen Sie im Fenster **HTTP-Einstellung hinzufügen** die Standardwerte für die übrigen Einstellungen, und wählen Sie dann **Hinzufügen** aus, um zum Fenster **Routingregel hinzufügen** zurückzukehren. 

1. Wählen Sie unter **Pfadbasiertes Routing** die Option **Zum Erstellen einer pfadbasierten Regel mehrere Ziele hinzufügen**  aus.
2. Geben Sie im Fenster **Pfadregel hinzufügen** die folgenden Werte für die Pfadregel ein:

   - **Pfad**: */images/\**
   - **Name der Pfadregel**: *Images*
   - **HTTP-Einstellung**: Wählen Sie *myHTTPSetting* aus.
   - **Back-End-Ziel**: *imagesBackendPool*
9. Wählen Sie **Hinzufügen**.
10. Fügen Sie eine weitere Pfadregel mit dem Namen *Video*, dem Pfad */video/\** und dem Pool *videoBackendPool* hinzu.
11. Wählen Sie **Änderungen speichern und zu den Routingregeln zurückwechseln**  aus.

    ![Hinzufügen einer Routingregel](media/create-url-route-portal/add-routing-rule.png)

12. Wählen Sie **Hinzufügen**.

7. Klicken Sie auf **Weiter: Tags** und dann auf **Weiter: Überprüfen + erstellen**.

### <a name="review--create-tab"></a>Registerkarte „Überprüfen und erstellen“

Überprüfen Sie die Einstellungen auf der Registerkarte **Überprüfen und erstellen**, und wählen Sie dann **Erstellen** aus, um das virtuelle Netzwerk, die öffentliche IP-Adresse und das Anwendungsgateway zu erstellen. Die Erstellung des Anwendungsgateways in Azure kann einige Minuten in Anspruch nehmen.

Warten Sie, bis die Bereitstellung erfolgreich abgeschlossen ist, bevor Sie mit dem nächsten Abschnitt fortfahren.


## <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

In diesem Beispiel erstellen Sie drei virtuelle Computer, die als Back-End-Server für das Anwendungsgateway verwendet werden. Sie installieren außerdem IIS auf den virtuellen Computern, um zu überprüfen, ob das Anwendungsgateway erfolgreich erstellt wurde.

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**. Das Fenster **Neu** wird angezeigt.
2. Klicken Sie auf **Compute**, und wählen Sie dann in der Liste **Beliebt** die Option **Windows Server 2016 Datacenter** aus. Die Seite **Virtuellen Computer erstellen** wird angezeigt.

   Application Gateway kann Datenverkehr an jeden beliebigen virtuellen Computer weiterleiten, der im Back-End-Pool verwendet wird. In diesem Beispiel verwenden Sie Windows Server 2016 Datacenter.
1. Geben Sie auf der Registerkarte **Grundlagen** die folgenden Werte für die VM-Einstellungen ein:

    - **Ressourcengruppe**: Wählen Sie **myResourceGroupAG** als Namen der Ressourcengruppe aus.
    - **Name des virtuellen Computers**: Geben Sie *myVM1* als Name des virtuellen Computers ein.
    - **Benutzername**: Geben *azureuser* als Namen des Administratorbenutzers ein.
    - **Kennwort**: Geben Sie *Azure123456!* als Administratorkennwort ein.
4. Übernehmen Sie für die anderen Einstellungen die Standardwerte, und klicken Sie auf **Weiter: Datenträger**.  
5. Übernehmen Sie auf der Registerkarte **Datenträger** die Standardwerte, und klicken Sie auf **Weiter: Netzwerk**.
6. Vergewissern Sie sich auf der Registerkarte **Netzwerk**, dass **myVNet** für **Virtuelles Netzwerk** ausgewählt und **Subnetz** auf **myBackendSubnet** festgelegt ist. Übernehmen Sie für die anderen Einstellungen die Standardwerte, und klicken Sie auf **Weiter: Verwaltung** aus.

   Application Gateway kann mit Instanzen außerhalb des eigenen virtuellen Netzwerks kommunizieren, es muss jedoch sichergestellt werden, dass eine IP-Verbindung besteht.
1. Legen Sie auf der Registerkarte **Verwaltung** die Option **Startdiagnose** auf **Aus** fest. Übernehmen Sie für die anderen Einstellungen die Standardwerte, und klicken Sie auf **Bewerten + erstellen**.
2. Überprüfen Sie auf der Registerkarte **Bewerten + erstellen** die Einstellungen, beheben Sie alle Validierungsfehler, und wählen Sie dann **Erstellen** aus.
3. Warten Sie, bis die Erstellung des virtuellen Computers abgeschlossen ist, bevor Sie fortfahren.

### <a name="install-iis"></a>Installieren von IIS

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Öffnen Sie die interaktive Shell, und vergewissern Sie sich, dass **PowerShell** festgelegt ist.

    ![Installieren der benutzerdefinierten Erweiterung](./media/create-url-route-portal/application-gateway-extension.png)

2. Führen Sie den folgenden Befehl aus, um IIS auf dem virtuellen Computer zu installieren: 

    ```azurepowershell-interactive
    $publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/appgatewayurl.ps1");  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }
    Set-AzVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -Location eastus `
      -ExtensionName IIS `
      -VMName myVM1 `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -Settings $publicSettings
    ```

3. Erstellen Sie zwei weitere virtuelle Computer, und installieren Sie IIS mithilfe der soeben ausgeführten Schritte. Verwenden Sie *myVM2* und *myVM3* als Namen für die virtuellen Computer und als Werte für **VMName** in „Set-AzVMExtension“.

## <a name="add-backend-servers-to-backend-pools"></a>Hinzufügen von Back-End-Servern zu Back-End-Pools

1. Wählen Sie **Alle Ressourcen** und dann **myAppGateway** aus.

2. Klicken Sie im Menü auf der linken Seite auf **Back-End-Pools**.

3. Wählen Sie **appGatewayBackendPool** aus.

4. Wählen Sie unter **Ziele** in der Dropdownliste die Option **Virtueller Computer** aus.

5. Wählen Sie unter **VIRTUELLER COMPUTER** und **NETZWERKSCHNITTSTELLEN** in den Dropdownlisten den virtuellen Computer **myVM1** sowie die zugehörige Netzwerkschnittstelle aus.

    ![Hinzufügen von Back-End-Servern](./media/create-url-route-portal/backend-pool.png)

6. Wählen Sie **Speichern** aus.
7. Wiederholen Sie die Schritte, um *myVM2* und die Schnittstelle zu *imagesBackendPool* und *myVM3* und die Schnittstelle zu *videoBackendPool* hinzuzufügen.

Warten Sie, bis die Bereitstellung abgeschlossen ist, bevor Sie mit dem nächsten Schritt fortfahren.

## <a name="test-the-application-gateway"></a>Testen des Anwendungsgateways

1. Wählen Sie die Option **Alle Ressourcen** und dann **myAGPublicIPAddress** aus.

    ![Notieren der öffentlichen IP-Adresse des Anwendungsgateways](./media/create-url-route-portal/application-gateway-record-ag-address.png)

2. Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in die Adressleiste des Browsers ein. Z.B. http://40.121.222.19.

    ![Testen der Basis-URL im Anwendungsgateway](./media/create-url-route-portal/application-gateway-iistest.png)

3. Ändern Sie die URL in http://&lt;ip-address&gt;:8080/images/test.htm, und ersetzen Sie &lt;ip-address&gt; durch Ihre IP-Adresse. Die Ausgabe sollte in etwa wie folgt aussehen:

    ![Testen der Images-URL im Anwendungsgateway](./media/create-url-route-portal/application-gateway-iistest-images.png)

4. Ändern Sie die URL in http://&lt;ip-address&gt;:8080/video/test.htm, und ersetzen Sie &lt;ip-address&gt; durch Ihre IP-Adresse. Die Ausgabe sollte in etwa wie folgt aussehen:

    ![Testen der Video-URL im Anwendungsgateway](./media/create-url-route-portal/application-gateway-iistest-video.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die mit dem Anwendungsgateway erstellten Ressourcen nicht mehr benötigen, entfernen Sie die Ressourcengruppe. Beim Entfernen der Ressourcengruppe werden auch das Anwendungsgateway und alle zugehörigen Ressourcen entfernt. 

So entfernen Sie die Ressourcengruppe:

1. Wählen Sie im Azure-Portal im Menü auf der linken Seite die Option **Ressourcengruppen** aus.
2. Suchen Sie auf der Seite **Ressourcengruppen** in der Liste nach **myResourceGroupAG**, und wählen Sie den Eintrag aus.
3. Wählen Sie auf der Seite für die **Ressourcengruppe** die Option **Ressourcengruppe löschen** aus.
4. Geben Sie *myResourceGroupAG* für **RESSOURCENGRUPPENNAMEN EINGEBEN** ein, und wählen Sie **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erfahren Sie mehr darüber, welche Möglichkeiten Azure Application Gateway bietet.](application-gateway-introduction.md)

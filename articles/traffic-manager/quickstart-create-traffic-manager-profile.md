---
title: 'Schnellstart: Erstellen eines Profils für hoch verfügbare Anwendungen: Azure-Portal – Azure Traffic Manager'
description: In diesem Schnellstartartikel wird beschrieben, wie Sie ein Traffic Manager-Profil erstellen, um hoch verfügbare Webanwendungen zu entwickeln.
services: traffic-manager
author: asudbring
manager: twooley
Customer intent: As an IT admin, I want to direct user traffic to ensure high availability of web applications.
ms.service: traffic-manager
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2018
ms.author: allensu
ms.openlocfilehash: b6a015c7462f082d5114a51d83cfacb0c550b026
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74032692"
---
# <a name="quickstart-create-a-traffic-manager-profile-using-the-azure-portal"></a>Schnellstart: Erstellen eines Traffic Manager-Profils im Azure-Portal

In dieser Schnellstartanleitung wird beschrieben, wie Sie ein Traffic Manager-Profil erstellen, mit dem die Hochverfügbarkeit für Ihre Webanwendung sichergestellt wird.

In dieser Schnellstartanleitung werden zwei Instanzen einer Webanwendung beschrieben. Jede Instanz wird in einer anderen Azure-Region ausgeführt. Sie erstellen ein Traffic Manager-Profil basierend auf der [Endpunktpriorität](traffic-manager-routing-methods.md#priority). Das Profil leitet den Benutzerdatenverkehr an den primären Standort, an dem die Webanwendung ausgeführt wird. Die Webanwendung wird von Traffic Manager ständig überwacht. Wenn der primäre Standort nicht verfügbar ist, erfolgt automatisch ein Failover zum Sicherungsstandort.

Wenn Sie kein Azure-Abonnement besitzen, können Sie jetzt ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung benötigen Sie zwei Instanzen einer Webanwendung, die in zwei unterschiedlichen Azure-Regionen bereitgestellt werden (*USA, Osten* und *Europa, Westen*). Jede dient jeweils als primärer bzw. Failoverendpunkt für Traffic Manager.

1. Wählen Sie oben links auf dem Bildschirm die Option **Ressource erstellen** > **Web** > **Web-App**.

1. Geben Sie unter **Web-App erstellen** auf der Registerkarte **Grundlagen** die folgenden Werte ein (bzw. wählen Sie sie aus):

   - **Abonnement** > **Ressourcengruppe**: Wählen Sie **Neu erstellen** aus, und geben Sie **myResourceGroupTM1** ein.
   - **Instanzendetails** > **Name**: Geben Sie *myWebAppEastUS* ein.
   - **Instanzdetails** > **Veröffentlichen**: Wählen Sie **Code** aus.
   - **Instanzdetails** > **Laufzeitstapel**: Wählen Sie **ASP.NET V4.7** aus.
   - **Instanzdetails** > **Betriebssystem**: Wählen Sie **Windows** aus.
   - **Instanzendetails** > **Region**:  Wählen Sie **USA, Osten** aus.
   - **App Service-Plan** > **Windows-Plan (USA, Osten)** : Wählen Sie **Neu erstellen** aus, und geben Sie dann **myAppServicePlanEastUS** ein.
   - **App Service-Plan** > **SKU und Größe**: Wählen Sie **Standard (S1)** aus.
   
3. Wählen Sie die Registerkarte **Überwachung** aus, oder wählen Sie **Next:Monitoring** aus.  Legen Sie unter **Überwachung** **Application Insights** > **Application Insights aktivieren** auf **Nein** fest.

4. Wählen Sie **Überprüfen und erstellen** aus.

5. Überprüfen Sie die Einstellungen, und klicken Sie dann auf **Erstellen**.  Wenn die Bereitstellung der Web-App erfolgreich war, wird eine Standardwebsite erstellt.

6. Führen Sie die Schritte zum Erstellen einer zweiten Web-App mit dem Namen *myWebAppWestEurope* aus. Geben Sie der **Ressourcengruppe** den Namen *myResourceGroupTM2*, und verwenden Sie als **Region** die Option *Europa, Westen* und als Name des **App Service-Plans** **myAppServicePlanWestEurope**. Legen Sie bei allen anderen Einstellungen die gleichen Optionen fest wie für *myWebAppEastUS*.

## <a name="create-a-traffic-manager-profile"></a>Erstellen eines Traffic Manager-Profils

Erstellen Sie ein Traffic Manager-Profil, das Benutzerdatenverkehr basierend auf der Endpunktpriorität weiterleitet.

1. Wählen Sie oben links auf dem Bildschirm **Ressource erstellen** > **Netzwerk** > **Traffic Manager-Profil**.
2. Geben Sie unter **Traffic Manager-Profil erstellen** die folgenden Einstellungen ein (bzw. wählen Sie sie aus):

    | Einstellung | Wert |
    | --------| ----- |
    | NAME | Geben Sie einen eindeutigen Namen für Ihr Traffic Manager-Profil ein.|
    | Routingmethode | Wählen Sie **Priorität**.|
    | Subscription | Wählen Sie das Abonnement aus, auf das das Traffic Manager-Profil angewendet werden soll. |
    | Resource group | Wählen Sie *myResourceGroupTM1* aus.|
    | Location |Diese Einstellung bezieht sich auf den Standort der Ressourcengruppe. Sie wirkt sich nicht auf das Traffic Manager-Profil aus, das global bereitgestellt wird.|

3. Klicken Sie auf **Erstellen**.

## <a name="add-traffic-manager-endpoints"></a>Hinzufügen von Traffic Manager-Endpunkten

Fügen Sie die Website in der Region *USA, Osten* als primären Endpunkt für das Routing des gesamten Benutzerdatenverkehrs hinzu. Fügen Sie die Website in *Europa, Westen* als Failoverendpunkt hinzu. Wenn der primäre Endpunkt nicht verfügbar ist, wird der Datenverkehr automatisch an den Failoverendpunkt weitergeleitet.

1. Geben Sie in der Suchleiste des Portals den Namen des Traffic Manager-Profils ein, das Sie im vorherigen Abschnitt erstellt haben.
2. Wählen Sie das Profil in den Suchergebnissen aus.
3. Wählen Sie im **Traffic Manager-Profil** im Abschnitt **Einstellungen** die Option **Endpunkte** und dann **Hinzufügen**.
4. Geben Sie die folgenden Einstellungen ein (bzw. wählen Sie sie aus):

    | Einstellung | Wert |
    | ------- | ------|
    | type | Wählen Sie **Azure-Endpunkt**. |
    | NAME | Geben Sie *myPrimaryEndpoint* ein. |
    | Zielressourcentyp | Wählen Sie **App Service**. |
    | Zielressource | Wählen Sie **App Service auswählen** > **USA, Osten**. |
    | Priority | Wählen Sie **1**. Der gesamte Datenverkehr wird an diesen Endpunkt gesendet, wenn er fehlerfrei ist. |

    ![Screenshot: Ort zum Hinzufügen eines Endpunkts zu Ihrem Traffic Manager-Profil.](./media/quickstart-create-traffic-manager-profile/add-traffic-manager-endpoint.png)

5. Klicken Sie auf **OK**.
6. Wiederholen Sie die Schritte 3 und 4 mit den folgenden Einstellungen, um einen Failoverendpunkt für Ihre zweite Azure-Region zu erstellen:

    | Einstellung | Wert |
    | ------- | ------|
    | type | Wählen Sie **Azure-Endpunkt**. |
    | NAME | Geben Sie *myFailoverEndpoint* ein. |
    | Zielressourcentyp | Wählen Sie **App Service**. |
    | Zielressource | Wählen Sie **App Service auswählen** > **Europa, Westen**. |
    | Priority | Wählen Sie **2**. Der gesamte Datenverkehr wird an diesen Failoverendpunkt geleitet, wenn der primäre Endpunkt fehlerhaft ist. |

7. Klicken Sie auf **OK**.

Wenn Sie das Hinzufügen der beiden Endpunkte abgeschlossen haben, werden sie im **Traffic Manager-Profil** angezeigt. Beachten Sie, dass der Überwachungsstatus jetzt **Online** lautet.

## <a name="test-traffic-manager-profile"></a>Testen des Traffic Manager-Profils

In diesem Abschnitt überprüfen Sie den Domänennamen Ihres Traffic Manager-Profils. Außerdem konfigurieren Sie den primären Endpunkt so, dass er nicht verfügbar ist. Abschließend können Sie sehen, dass die Web-App weiterhin verfügbar ist. Dies liegt daran, dass Traffic Manager den Datenverkehr an den Failoverendpunkt sendet.

### <a name="check-the-dns-name"></a>Überprüfen des DNS-Namens

1. Suchen Sie in der Suchleiste des Portals nach dem Namen des **Traffic Manager-Profils**, das Sie im vorhergehenden Abschnitt erstellt haben.
2. Wählen Sie das Traffic Manager-Profil aus. Die **Übersicht** wird angezeigt.
3. Unter **Traffic Manager-Profil** wird der DNS-Name Ihres neu erstellten Traffic Manager-Profils angezeigt.
  
   ![Screenshot: Standort Ihres Traffic Manager-DNS-Namens](./media/quickstart-create-traffic-manager-profile/traffic-manager-dns-name.png)

### <a name="view-traffic-manager-in-action"></a>Anzeigen von Traffic Manager in Aktion

1. Geben Sie in einem Webbrowser den DNS-Namen Ihres Traffic Manager-Profils an, um die Standardwebsite Ihrer Web-App anzuzeigen.

    > [!NOTE]
    > In diesem Schnellstartszenario werden alle Anforderungen an den primären Endpunkt weitergeleitet. Es ist **Priorität 1** festgelegt.

    ![Screenshot: Webseite zum Bestätigen der Verfügbarkeit des Traffic Manager-Profils](./media/quickstart-create-traffic-manager-profile/traffic-manager-test.png)

2. Deaktivieren Sie Ihren primären Standort, wenn Sie das Traffic Manager-Failover in Aktion sehen möchten:
    1. Wählen Sie auf der Seite mit dem Traffic Manager-Profil im Abschnitt **Übersicht** die Option **myPrimaryEndpoint**.
    2. Wählen Sie unter *myPrimaryEndpoint* die Option **Deaktiviert** > **Speichern**.
    3. Schließen Sie **myPrimaryEndpoint**. Sie sehen, dass der Status jetzt **Deaktiviert** lautet.
3. Kopieren Sie den DNS-Namen des Traffic Manager-Profils aus dem vorherigen Schritt, um die Website in einer neuen Browsersitzung anzuzeigen.
4. Vergewissern Sie sich, dass die Web-App weiterhin verfügbar ist.

Da der primäre Endpunkt nicht verfügbar ist, wurden Sie an den Failoverendpunkt weitergeleitet.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppen, Webanwendungen und alle dazugehörigen Ressourcen, wenn Sie fertig sind. Wählen Sie hierzu jedes einzelne Element in Ihrem Dashboard aus, und klicken Sie dann oben auf der Seite auf die Option **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie ein Traffic Manager-Profil erstellt. Damit können Sie Benutzerdatenverkehr für Webanwendungen mit Hochverfügbarkeit weiterleiten. Weitere Informationen zum Weiterleiten des Datenverkehrs finden Sie in den Tutorials zu Traffic Manager.

> [!div class="nextstepaction"]
> [Traffic Manager-Tutorials](tutorial-traffic-manager-improve-website-response.md)

---
title: Erstellen einer Front Door-Ressource mit Umleitung von HTTP zu HTTPS über das Azure-Portal
description: Erfahren Sie, wie Sie mithilfe des Azure-Portals eine Front Door-Ressource mit Datenverkehr erstellen, der von HTTP zu HTTPS umgeleitet wird.
services: front-door
author: sharad4u
ms.service: frontdoor
ms.topic: article
ms.date: 5/21/2019
ms.author: sharadag
ms.openlocfilehash: 7fabc1e3445d3dbd357700ffde3caeb985cc60c4
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67601956"
---
# <a name="create-a-front-door-with-http-to-https-redirection-using-the-azure-portal"></a>Erstellen einer Front Door-Ressource mit Umleitung von HTTP zu HTTPS über das Azure-Portal

Sie können das Azure-Portal verwenden, um eine [Front Door-Ressource ](front-door-overview.md) mit einem Zertifikat für die SSL-Beendigung zu erstellen. Eine Routingregel wird verwendet, um HTTP-Datenverkehr an HTTPS umzuleiten.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Erstellen einer Front Door-Ressource mit einer vorhandenen Web-App-Ressource
> * Hinzufügen einer benutzerdefinierten Domäne mit einem SSL-Zertifikat 
> * Einrichten von HTTPS-Umleitung für die benutzerdefinierte Domäne

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-a-front-door-with-an-existing-web-app-resource"></a>Erstellen einer Front Door-Ressource mit einer vorhandenen Web-App-Ressource

1. Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.
2. Klicken Sie in der linken oberen Ecke des Azure-Portals auf **Ressource erstellen**.
3. Suchen Sie mithilfe der Suchleiste nach **Front Door.**  Klicken Sie auf **Erstellen**, wenn Sie den Ressourcentyp gefunden haben.
4. Wählen Sie ein Abonnement aus, und verwenden Sie dann eine vorhandene Ressourcengruppe, oder erstellen Sie eine neue Ressourcengruppe. Beachten Sie, dass sich der Speicherort, der in der Benutzeroberfläche abgefragt wird, nur auf die Ressourcengruppe bezieht. Die Front Door-Konfiguration wird an allen [POP-Standorten von Azure Front Door](https://docs.microsoft.com/azure/frontdoor/front-door-faq#what-are-the-pop-locations-for-azure-front-door-service) bereitgestellt.

    ![Konfigurieren der Grundlagen für eine neue Front Door-Ressource](./media/front-door-url-redirect/front-door-create-basics.png)

5. Klicken Sie auf **Weiter**, um auf die Registerkarte „Konfiguration“ zu gelangen. Die Konfiguration für Front Door erfolgt in drei Schritten: Hinzufügen eines Front-End-Standardhosts, Hinzufügen von Back-Ends in einem Back-End-Pool und anschließendes Erstellen von Routingregeln, um das Routingverhalten für den Front-End-Host zuzuordnen. 

     ![Front Door-Konfigurations-Designer](./media/front-door-url-redirect/front-door-designer.png)

6. Klicken Sie auf das Symbol „ **+** “ für die _Front-End-Hosts_, um einen Front-End-Host zu erstellen, und geben Sie einen global eindeutigen Namen für Ihren Front-End-Standardhost für Front Door (`\<**name**\>.azurefd.net`) ein. Klicken Sie auf **Hinzufügen**, um mit dem nächsten Schritt fortzufahren.

     ![Hinzufügen eines Front-End-Hosts](./media/front-door-url-redirect/front-door-create-fehost.png)

7. Klicken Sie auf das Symbol „ **+** “ für die _Back-End-Pools_, um einen Back-End-Pool zu erstellen. Geben Sie einen Namen für den Back-End-Pool an, und klicken Sie dann auf **Back-End hinzufügen**.
8. Wählen Sie den Back-End-Hosttyp als _App Service_ aus. Wählen Sie das Abonnement aus, in dem Ihre Web-App gehostet wird, und wählen Sie dann die entsprechende Web-App aus der Dropdownliste für **Name des Back-End-Hosts** aus.
9. Klicken Sie auf **Hinzufügen**, um das Back-End zu speichern, und klicken Sie dann nochmals auf **Hinzufügen**, um die Konfiguration des Back-End-Pools zu speichern.   ![Hinzufügen eines Back-Ends in einem Back-End-Pool](./media/front-door-url-redirect/front-door-create-backendpool.png)

10. Klicken Sie auf das Symbol „ **+** “ für die _Routingregeln_, um eine Route zu erstellen. Geben Sie einen Namen für die Route an (z.B. „HttpToHttpsRedirect“), und legen Sie dann das Feld _Akzeptierten Protokolle_ auf **Nur HTTP** fest. Stellen Sie sicher, dass der entsprechende _Front-End-Host_ ausgewählt ist.  
11. Legen Sie im Abschnitt _Routendetails_ den _Routentyp_ auf **Umleiten** fest, und stellen Sie sicher, dass der _Umleitungstyp_ auf **Gefunden (302)** und das _Umleitungsprotokoll_ auf **Nur HTTPS** festgelegt ist. 
12. Klicken Sie auf „Hinzufügen“, um die Routingregel für die Umleitung von HTTP zu HTTPS zu speichern.
     ![Hinzufügen einer Umleitungsroute von HTTP zu HTTPS](./media/front-door-url-redirect/front-door-redirect-config-example.png)
13. Fügen Sie eine weitere Routingregel für die Verarbeitung des HTTPS-Datenverkehrs hinzu. Klicken Sie auf das Symbol „ **+** “ für die _Routingregeln_, und geben Sie einen Namen für die Route an (z.B. „DefaultForwardingRoute“), und legen Sie dann das Feld _Akzeptierte Protokolle_ auf **Nur HTTPS** fest. Stellen Sie sicher, dass der entsprechende _Front-End-Host_ ausgewählt ist.
14. Legen Sie im Abschnitt „Routendetails“ den _Routentyp_ auf **Weiterleiten** fest, und stellen Sie sicher, dass der richtige Back-End-Pool ausgewählt ist und das _Weiterleitungsprotokoll_ auf **Nur HTTPS** festgelegt ist. 
15. Klicken Sie auf „Hinzufügen“, um die Routingregel für die Anforderungsweiterleitung zu speichern.
     ![Hinzufügen einer Weiterleitungsroute für HTTPS-Datenverkehr](./media/front-door-url-redirect/front-door-forward-route-example.png)
16. Klicken Sie auf **Überprüfen und erstellen** und dann auf **Erstellen**, um Ihr Front Door-Profil zu erstellen. Navigieren Sie zu der Ressource, nachdem diese erstellt wurde.

## <a name="add-a-custom-domain-to-your-front-door-and-enable-https-on-it"></a>Hinzufügen einer benutzerdefinierten Domäne für die Front Door-Ressource und Aktivieren von HTTPS für diese
Die folgenden Schritte zeigen, wie Sie einer vorhandenen Front Door-Ressource eine benutzerdefinierte Domäne hinzufügen und dann eine Umleitung von HTTP zu HTTPS für diese aktivieren können. 

### <a name="add-a-custom-domain"></a>Hinzufügen einer benutzerdefinierten Domäne

In diesem Beispiel fügen Sie einen CNAME-Eintrag für die Unterdomäne `www` (z.B. `www.contosonews.com`) hinzu.

#### <a name="create-the-cname-record"></a>Erstellen des CNAME-Eintrags

Fügen Sie dem Front-End-Standardhost Ihrer Front Door-Ressource einen CNAME-Eintrag zum Zuordnen einer Unterdomäne hinzu (`<name>.azurefd.net`, wobei `<name>` der Name Ihres Front Door-Profils ist).

Fügen Sie für die Domäne `www.contoso.com` (als Beispiel) einen CNAME-Eintrag hinzu, mit dem der Name `www` dem Element `<name>.azurefd.net` zugeordnet wird.

Nach dem Hinzufügen des CNAME-Eintrags sieht die Seite mit den DNS-Einträgen wie im folgenden Beispiel aus:

![CNAME-Eintrag für benutzerdefinierte Domäne für Front Door](./media/front-door-url-redirect/front-door-dns-cname.png)

#### <a name="onboard-the-custom-domain-on-your-front-door"></a>Durchführen des Onboardings der benutzerdefinierten Domäne für Ihre Front Door-Ressource

1. Klicken Sie auf der Designer-Registerkarte „Front Door“ im Abschnitt „Front-End-Hosts“ auf das Symbol „+“, um eine neue benutzerdefinierte Domäne hinzuzufügen. 
2. Geben Sie den vollqualifizierten benutzerdefinierten DNS-Namen in das Feld für den benutzerdefinierten Hostnamen ein, z.B. `www.contosonews.com`. 
3. Nachdem die CNAME-Zuordnung von der Domäne zu Ihrer Front Door-Ressource überprüft wurde, klicken Sie auf **Hinzufügen**, um die benutzerdefinierte Domäne hinzuzufügen.
4. Klicken Sie auf **Speichern**, um die Änderungen zu übermitteln.

![Menü „Benutzerdefinierte Domänen“](./media/front-door-url-redirect/front-door-add-custom-domain.png)

### <a name="enable-https-on-your-custom-domain"></a>Aktivieren von HTTPS für eine benutzerdefinierte Domäne

1. Klicken Sie auf die benutzerdefinierte Domäne, die hinzugefügt wurde, und ändern Sie den Status unter dem Abschnitt **HTTPS für benutzerdefinierte Domänen** in **Aktiviert**.
2. Sie können den **Zertifikatverwaltungstyp** für das kostenlose Zertifikat, das von Front Door gewartet, verwaltet und automatisch bewertet wird, auf _Durch Front Door verwaltet_ festgelegt lassen. Sie können auch Ihr eigenes benutzerdefiniertes SSL-Zertifikat verwenden, das in Azure Key Vault gespeichert ist. In diesem Tutorial wird davon ausgegangen, dass ein durch Front Door verwaltetes Zertifikat verwendet wird.
![Aktivieren von HTTPS für die benutzerdefinierte Domäne](./media/front-door-url-redirect/front-door-custom-domain-https.png)

3. Klicken Sie auf **Aktualisieren**, um die Auswahl zu speichern, und klicken Sie dann auf **Speichern**.
4. Klicken Sie nach einigen Minuten auf **Aktualisieren**, und klicken Sie dann erneut auf die benutzerdefinierte Domäne, um den Status der Zertifikatbereitstellung anzuzeigen. 

> [!WARNING]
> Die Aktivierung von HTTPS für eine benutzerdefinierte Domäne kann einige Minuten dauern und hängt auch von der Überprüfung des Besitzes ab, wenn der CNAME-Eintrag nicht direkt Ihrem Front Door-Host `<name>.azurefd.net` zugeordnet ist. Weitere Informationen zum [Aktivieren von HTTPS für eine benutzerdefinierte Domäne](./front-door-custom-domain-https.md).

## <a name="configure-the-routing-rules-for-the-custom-domain"></a>Konfigurieren der Routingregeln für die benutzerdefinierte Domäne

1. Klicken Sie auf die Umleitungsroutingregel, die Sie zuvor erstellt haben.
2. Klicken Sie auf die Dropdownliste für Front-End-Hosts, und wählen Sie Ihre benutzerdefinierte Domäne aus, um diese Route auch auf Ihre Domäne anzuwenden.
3. Klicken Sie auf **Aktualisieren**.
4. Führen Sie den gleichen Vorgang auch für die andere Routingregel aus, also für die Weiterleitungsroute, um die benutzerdefinierte Domäne hinzuzufügen.
5. Klicken Sie auf **Speichern**, um die Änderungen zu übermitteln.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Erstellen einer Azure Front Door Service-Konfiguration](quickstart-create-front-door.md).
- Informieren Sie sich über die [Funktionsweise von Azure Front Door Service](front-door-routing-architecture.md).
- Weitere Informationen zur [URL-Umleitung für Front Door](front-door-url-redirect.md).

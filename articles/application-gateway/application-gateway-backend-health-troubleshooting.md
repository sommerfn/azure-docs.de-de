---
title: Behandeln von Problemen mit der Back-End-Integrität in Azure Application Gateway
description: Dieser Artikel führt Sie durch die Problembehandlung der Back-End-Integrität für Azure Application Gateway.
services: application-gateway
author: surajmb
ms.service: application-gateway
ms.topic: article
ms.date: 08/30/2019
ms.author: surmb
ms.openlocfilehash: ce1d0542530c4f190ace52d45e2369d6ecc18772
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2019
ms.locfileid: "70384005"
---
<a name="troubleshoot-backend-health-issues-in-application-gateway"></a>Behandeln von Problemen mit der Back-End-Integrität in Application Gateway
==================================================

<a name="overview"></a>Übersicht
--------

Application Gateway testet standardmäßig die Back-End-Server, um deren Integritätsstatus zu überprüfen und festzustellen, ob sie bereit sind, Anforderungen zu verarbeiten. Benutzer können auch benutzerdefinierte Tests erstellen, um den Hostnamen, den zu überprüfenden Pfad und die Statuscodes, die als fehlerfrei akzeptiert werden sollen, anzugeben. Wenn der Back-End-Server nicht erfolgreich antwortet, wird der Server von Application Gateway in beiden Fällen als fehlerhaft markiert, und die Anforderung wird nicht mehr an den Server weitergeleitet. Nachdem der Server erfolgreich reagiert hat, wird die Verarbeitung der Anforderungen fortgesetzt.

### <a name="how-to-check-backend-health"></a>Überprüfen der Back-End-Integrität

Zum Überprüfen der Integrität des Back-End-Pools können Sie die Seite „Backend Health“ (Back-End-Integrität) im Azure-Portal verwenden. Sie können auch [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.network/get-azapplicationgatewaybackendhealth?view=azps-2.6.0), die [CLI](https://docs.microsoft.com/cli/azure/network/application-gateway?view=azure-cli-latest#az-network-application-gateway-show-backend-health) oder die [REST-API](https://docs.microsoft.com/rest/api/application-gateway/applicationgateways/backendhealth) verwenden, um den Integritätsstatus abzurufen. Lesen Sie den im Link genannten Artikel zu jeder Methode, um weitere Informationen zu erhalten.

Der Status, der von einer der oben erwähnten Methoden abgerufen wird, kann einer von drei Typen sein:

1.  Healthy (Fehlerfrei)

2.  Unhealthy (Fehlerhaft)

3.  Unknown (Unbekannt)

Wenn der Back-End-Integritätsstatus für einen Server fehlerfrei ist, bedeutet dies, dass Application Gateway die Anforderungen an diesen Server weiterleitet. Wenn jedoch die Back-End-Integrität für alle Server in einem Back-End-Pool fehlerhaft oder unbekannt ist, kann es zu Problemen beim Anwendungszugriff kommen. Dieses Dokument beschreibt das Symptom, die Ursache und die Lösung jedes der angezeigten Fehler, wenn die Integrität der Back-End-Server fehlerhaft oder unbekannt sein könnte.

<a name="backend-health-status-unhealthy"></a>Back-End-Integritätsstatus „Unhealthy“ (Fehlerhaft)
-------------------------------

Wenn der Back-End-Integritätsstatus als fehlerhaft gemeldet wird, wird dies wie im folgenden Screenshot im Portal angezeigt:

![Back-End-Integrität von Application Gateway: Fehlerhaft](./media/application-gateway-backend-health-troubleshooting/appgwunhealthy.png)

Wenn Sie Azure PowerShell, die CLI oder eine Azure-REST-API-Abfrage verwenden, wird eine Antwort ähnlich der folgenden angezeigt:
```azurepowershell
PS C:\Users\testuser\> Get-AzApplicationGatewayBackendHealth -Name "appgw1" -ResourceGroupName "rgOne"
BackendAddressPools :
{Microsoft.Azure.Commands.Network.Models.PSApplicationGatewayBackendHealthPool}
BackendAddressPoolsText : [
{
                              "BackendAddressPool": {
                                "Id": "/subscriptions/536d30b8-665b-40fc-bd7e-68c65f816365/resourceGroups/rgOne/providers/Microsoft.Network/applicationGateways/appgw1/b
                          ackendAddressPools/appGatewayBackendPool"
                              },
                              "BackendHttpSettingsCollection": [
                                {
                                  "BackendHttpSettings": {
                                    "TrustedRootCertificates": [],
                                    "Id": "/subscriptions/536d30b8-665b-40fc-bd7e-68c65f816365/resourceGroups/rgOne/providers/Microsoft.Network/applicationGateways/appg
                          w1/backendHttpSettingsCollection/appGatewayBackendHttpSettings"
                                  },
                                  "Servers": [
                                    {
                                      "Address": "10.0.0.5",
                                      "Health": "Healthy"
                                    },
                                    {
                                      "Address": "10.0.0.6",
                                      "Health": "Unhealthy"
                                    }
                                  ]
                                }
                              ]
                            }
                        ]
```
Nachdem der Back-End-Serverstatus für alle Server in einem Back-End-Pool als fehlerhaft angezeigt wird, werden die Anforderungen nicht an sie weitergeleitet, und Application Gateway gibt den Fehler „502 Bad Gateway“ an den anfordernden Client zurück. Um das Problem zu beheben, überprüfen Sie die Spalte „Details“ auf der Registerkarte „Backend health“ (Back-End-Integrität).

Die in der Spalte „Details“ der Registerkarte „Backend health“ (Back-End-Integrität) angezeigte Meldung bietet ausführlichere Einblicke in das Problem. Basierend auf diesen Informationen kann mit der Problembehandlung begonnen werden.

> [!NOTE]
> Die standardmäßige Testanforderung wird im Format <protocol>://127.0.0.0.1:<port>/ gesendet, z.B. <http://127.0.0.1/> für einen HTTP-Test an Port 80. Nur eine Antwort mit den HTTP-Statuscodes 200 bis 399 wird als fehlerfreie Antwort betrachtet. Das Protokoll und der Zielport werden von den HTTP-Einstellungen geerbt. Wenn Application Gateway auf ein anderes Protokoll, einen anderen Hostnamen oder einen anderen Pfad testen und einen anderen Statuscode als fehlerfrei akzeptieren soll, konfigurieren Sie einen benutzerdefinierten Test, und ordnen Sie ihn den HTTP-Einstellungen zu.

### <a name="error-messages"></a>Fehlermeldungen

#### <a name="backend-server-timeout"></a>Timeout des Back-End-Servers

**Nachricht:** Time taken by the backend to respond to application gateway\'s health probe is more than the time-out threshold in the probe setting. (Die Zeit, die das Back-End für die Reaktion auf den Application Gateway-Integritätstest benötigt, liegt über dem Timeoutschwellenwert in der Testeinstellung.)

**Ursache:** Nachdem Application Gateway eine HTTP(S)-Testanforderung an den Back-End-Server gesendet hat, wird für einen bestimmten Zeitraum, der konfiguriert wurde, auf die Antwort vom Back-End-Server gewartet. Wenn der Back-End-Server nicht innerhalb des Timeoutwerts antwortet, wird er als fehlerhaft gekennzeichnet, bis er erneut innerhalb des Timeoutwerts antwortet.

**Lösung:** Überprüfen Sie für den Back-End-Server oder die Anwendung, warum er bzw. sie nicht innerhalb des konfigurierten Timeouts antwortet, und überprüfen Sie auch die Anwendungsabhängigkeiten, z.B., ob die Datenbank Probleme aufweist, die zu einer verzögerten Reaktion führen können. Wenn Sie das Verhalten der Anwendung kennen und diese nur nach dem Timeoutwert antworten sollte, erhöhen Sie den Timeoutwert in den benutzerdefinierten Testeinstellungen.
Sie müssen über einen benutzerdefinierten Test verfügen, um den Wert des Timeouts zu ändern. Wenn Sie wissen möchten, wie Sie einen benutzerdefinierten Test konfigurieren, [lesen Sie die entsprechende Seite in der Dokumentation](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-probe-portal).

Um das Timeout zu erhöhen, führen Sie die folgenden Schritte aus:

1.  Greifen Sie direkt auf den Back-End-Server zu, und überprüfen Sie die Zeit, die der Server benötigt, um auf diese Seite zu antworten. Sie können ein beliebiges Tool für den Zugriff verwenden, einschließlich eines Browsers mit den Entwicklertools.

2.  Nachdem Sie die für die Anwendungsantwort benötigte Zeit ermittelt haben, klicken Sie auf die Registerkarte „Health probes“ (Integritätstests), und wählen Sie den Test aus, der den HTTP-Einstellungen zugeordnet ist.

3.  Geben Sie einen höheren Timeoutwert als die Antwortzeit der Anwendung in Sekunden ein.

4.  Speichern Sie die benutzerdefinierten Testeinstellungen, und überprüfen Sie die Back-End-Integrität darauf, ob nun „Healthy“ (Fehlerfrei) angezeigt wird.

#### <a name="dns-resolution-error"></a>Fehler bei der DNS-Auflösung

**Nachricht:** Application gateway could not create a probe for this backend. (Application Gateway konnte keinen Test für dieses Back-End erstellen.) This usually happens when the FQDN of the backend has not been entered correctly. (Dies ist normalerweise der Fall, wenn der FQDN des Back-Ends nicht ordnungsgemäß eingegeben wurde.) 

**Ursache:** Wenn der Back-End-Pool den Typ „IP-Adresse/FQDN“ oder „App Service“ aufweist, löst Application Gateway in die IP-Adresse des mit DNS-Server eingegebenen FQDN (benutzerdefiniert oder Azure-Standard) auf und versucht, eine Verbindung mit dem Server an dem TCP-Port herzustellen, der in den HTTP-Einstellungen angegeben ist. Wenn aber diese Meldung angezeigt wird, deutet dies darauf hin, dass Application Gateway die IP-Adresse des eingegebenen FQDN nicht erfolgreich auflösen konnte.

**Lösung:**

1.  Überprüfen Sie, ob der in den Back-End-Pool eingegebene FQDN richtig ist. Wenn es sich um eine öffentliche Domäne handelt, versuchen Sie, den FQDN von Ihrem lokalen Computer aufzulösen.

2.  Wenn Sie die IP-Adresse auflösen können, liegt möglicherweise ein Fehler bei der DNS-Konfiguration im VNET vor.

3.  Überprüfen Sie, ob das VNET mit einem benutzerdefinierten DNS-Server konfiguriert ist. Wenn dies der Fall ist, überprüfen Sie auf dem DNS-Server, warum er nicht in die IP-Adresse des angegebenen FQDN auflösen kann.

4.  Wenn es sich um eine Azure-DNS-Standardadresse handelt, überprüfen Sie bei Ihrer Domänennamen-Registrierungsstelle, ob eine ordnungsgemäße Zuordnung eines A-Eintrags oder CNAME-Eintrags vorgenommen wurde.

5.  Wenn die Domäne privat/intern ist, versuchen Sie sie von einem virtuellen Computer im gleichen VNET aufzulösen. Wenn Sie in der Lage sind, sie aufzulösen, starten Sie Application Gateway neu, und überprüfen Sie den Status erneut. Um Application Gateway neu zu starten, müssen Sie die in den entsprechenden Dokumentlinks angegebenen PowerShell-Befehle zum [Beenden](https://docs.microsoft.com/powershell/module/azurerm.network/stop-azurermapplicationgateway?view=azurermps-6.13.0) und [Starten](https://docs.microsoft.com/powershell/module/azurerm.network/start-azurermapplicationgateway?view=azurermps-6.13.0) verwenden.

#### <a name="tcp-connect-error"></a>TCP-Verbindungsfehler

**Nachricht:** Application gateway could not connect to the backend. (Application Gateway konnte keine Verbindung mit dem Back-End herstellen.)
Please check that the backend responds on the port used for the probe. (Überprüfen Sie, ob das Back-End am für den Test verwendeten Port antwortet.)
Also check whether any NSG/UDR/Firewall is blocking access to the Ip and port of this backend. (Überprüfen Sie außerdem, ob eine NSG/UDR/Firewall den Zugriff auf die IP-Adresse und den Port dieses Back-Ends blockiert.)

**Ursache:** Nach der DNS-Auflösungsphase versucht Application Gateway, eine Verbindung mit dem Back-End-Server an dem TCP-Port herzustellen, der in den HTTP-Einstellungen konfiguriert ist. Wenn Application Gateway keine TCP-Sitzung am angegebenen Port einrichten kann, wird der Test mit dieser Meldung als fehlerhaft gekennzeichnet.

**Lösung:** Wenn dieser Fehler angezeigt wird, überprüfen Sie Folgendes:

1.  Überprüfen Sie, ob Sie mithilfe eines Browsers oder mithilfe von PowerShell eine Verbindung mit dem Back-End-Server am in den HTTP-Einstellungen genannten Port herstellen können. Sie können z.B. den folgenden Befehl verwenden: Test-NetConnection -ComputerName www.bing.com -Port 443

2.  Wenn der angegebene Port nicht der gewünschte Port ist, geben Sie die richtige Portnummer ein, damit Application Gateway eine Verbindung mit dem Back-End-Server herstellen kann.

3.  Wenn Sie auch von Ihrem lokalen Computer aus keine Verbindung über den Port herstellen können:

    a.  Überprüfen Sie die NSG-Einstellungen der NIC und des Subnetzes des Back-End-Servers, und überprüfen Sie, ob die eingehenden Verbindungen an den konfigurierten Port zulässig sind Wenn Sie nicht zulässig sind, erstellen Sie eine neue Regel, um die Verbindungen zuzulassen. Informationen zum Erstellen von NSG-Regeln finden Sie auf der [entsprechenden Seite der Dokumentation](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic#create-security-rules).

    b.  Überprüfen Sie die NSG-Einstellungen des Subnetzes von Application Gateway, wenn ausgehender öffentlicher und privater Datenverkehr zulässig ist, damit die Verbindung hergestellt werden kann. Überprüfen Sie den Dokumentlink in (a), um mehr über das Erstellen von NSG-Regeln zu erfahren.
    ```azurepowershell
            $vnet = Get-AzVirtualNetwork -Name "vnetName" -ResourceGroupName "rgName"
            Get-AzVirtualNetworkSubnetConfig -Name appGwSubnet -VirtualNetwork $vnet
    ```

    c.  Überprüfen Sie die UDR-Einstellungen des Subnetzes von Application Gateway und des Subnetzes des Back-End-Servers auf etwaige Routinganomalien. Stellen Sie sicher, dass die UDR keinen Datenverkehr vom Back-End-Subnetz wegleitet. Überprüfen Sie z.B. Routen zu virtuellen Netzwerkgeräten oder Standardrouten, die dem Application Gateway-Subnetz über ExpressRoute/VPN angekündigt werden.

    d.  Zum Überprüfen der effektiven Routen und Regeln für eine NIC können Sie die folgenden PowerShell-Befehle verwenden.
    ```azurepowershell
            Get-AzEffectiveNetworkSecurityGroup -NetworkInterfaceName "nic1" -ResourceGroupName "testrg"
            Get-AzEffectiveRouteTable -NetworkInterfaceName "nic1" -ResourceGroupName "testrg"
    ```
4.  Wenn Sie keine Probleme mit der NSG oder UDR finden, überprüfen Sie Ihren Back-End-Server auf anwendungsbezogene Probleme, durch die Clients keine TCP-Sitzung an den konfigurierten Ports einrichten können. Folgendes sollte überprüft werden:

    e.  Öffnen Sie eine Eingabeaufforderung (WIN+R -\> cmd), geben Sie „netstart“ ein, und drücken Sie die EINGABETASTE.

    f.  Überprüfen Sie, ob der Server am konfigurierten Port lauscht.
        Beispiel:
    ```
            Proto Local Address Foreign Address State PID
            TCP 0.0.0.0:80 0.0.0.0:0 LISTENING 4
    ```
    g.  Wenn dies nicht der Fall ist, überprüfen Sie die Webservereinstellungen (z.B. Sitebindungen in IIS, Server Block in NGINX und virtuellen Host in Apache).

    h.  Überprüfen Sie die Firewalleinstellungen Ihres Betriebssystems, um sicherzustellen, dass eingehender Datenverkehr am Port zulässig ist

#### <a name="http-status-code-mismatch"></a>HTTP-Statuscodekonflikt

**Nachricht:** Status code of the backend\'s HTTP response did not match the probe setting. (Der Statuscode der HTTP-Antwort des Back-Ends stimmte nicht mit der Testeinstellung überein.) Expected:{HTTPStatusCode0} Received:{HTTPStatusCode1}. (Erwartet: {HTTPStatusCode0}, Empfangen: {HTTPStatusCode1}.)

**Ursache:** Nachdem die TCP-Verbindung hergestellt und der SSL-Handshake ausgeführt wurde (wenn SSL aktiviert ist), sendet Application Gateway den Test als HTTP GET-Anforderung an den Back-End-Server. Wie bereits erwähnt, wird als Standardtest \<Protokoll\>://127.0.0.1:\<port\>/ verwendet, und die Antwortstatuscodes im Bereich 200 bis 399 werden als fehlerfrei angesehen. Wenn der Server einen anderen Statuscode zurückgibt, wird er mit dieser Meldung als fehlerhaft gekennzeichnet.

**Lösung:** Abhängig vom Antwortcode des Back-End-Servers können Sie die folgenden Schritte ausführen. Einige der allgemeinen Statuscodes sind hier aufgeführt:

| **Fehler** | **Aktionen** |
| --- | --- |
| Probe status code mismatch: (Teststatuscode-Konflikt:) Received 401 (401 empfangen) | Überprüfen Sie, ob der Back-End-Server Authentifizierung erfordert. Application Gateway-Tests können zu diesem Zeitpunkt keine Anmeldeinformationen für die Authentifizierung übergeben. Lassen Sie \"HTTP 401\" in einer Teststatuscode-Übereinstimmung zu, oder testen Sie einen Pfad, in dem der Server keine Authentifizierung erfordert. | |
| Probe status code mismatch: (Teststatuscode-Konflikt:) Received 403 (403 empfangen) | Access forbidden. (Zugriff verweigert.) Überprüfen Sie, ob der Zugriff auf den Pfad auf dem Back-End-Server zulässig ist. | |
| Probe status code mismatch: (Teststatuscode-Konflikt:) Received 404 (404 empfangen) | Page not found. (Seite nicht gefunden.) Überprüfen Sie, ob auf den Hostnamenpfad auf dem Back-End-Server zugegriffen werden kann. Ändern Sie den hostname- oder den path-Parameter in einen Wert, auf den zugegriffen werden kann. | |
| Probe status code mismatch: (Teststatuscode-Konflikt:) Received 405 (405 empfangen) | Die Testanforderungen von Application Gateway verwenden die HTTP GET-Methode. Überprüfen Sie, ob der Server diese zulässt. | |
| Probe status code mismatch: (Teststatuscode-Konflikt:) Received 500 (500 empfangen) | Interner Serverfehler. Überprüfen Sie die Integrität des Back-End-Servers, und stellen Sie fest, ob die Dienste ausgeführt werden. | |
| Probe status code mismatch: (Teststatuscode-Konflikt:) Received 503 (503 empfangen) | Service unavailable. (Dienst nicht verfügbar.) Überprüfen Sie die Integrität des Back-End-Servers, und stellen Sie fest, ob die Dienste ausgeführt werden. | |

Wenn Sie der Ansicht sind, dass die Antwort legitim ist und Sie möchten, dass Application Gateway andere Statuscodes als fehlerfrei akzeptiert, können Sie einen benutzerdefinierten Test erstellen. Diese Änderung ist in Situationen hilfreich, in denen die Back-End-Website Authentifizierung benötigt. Da die Testanforderungen keine Benutzeranmeldeinformationen enthalten, schlagen sie fehl, und vom Back-End-Server wird ein HTTP 401-Statuscode zurückgegeben.

Um einen benutzerdefinierten Test zu erstellen, führen Sie die [hier](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-probe-portal) aufgeführten Schritte aus.

#### <a name="http-response-body-mismatch"></a>Konflikt im HTTP-Antworttext

**Nachricht:** Body of the backend\'s HTTP response did not match the probe setting. (Der Text der HTTP-Antwort des Back-Ends stimmte nicht mit der Testeinstellung überein.) Received response body does not contain {string}. (Der empfangene Antworttext enthält nicht {Zeichenfolge}.)

**Ursache:** Beim Erstellen eines benutzerdefinierten Tests haben Sie die Möglichkeit, einen Back-End-Server als fehlerfrei zu markieren, indem Sie eine Zeichenfolge aus dem Antworttext abgleichen. Beispielsweise können Sie Application Gateway so konfigurieren, dass „unauthorized“ (nicht autorisiert) als abzugleichende Zeichenfolge angenommen wird. Wenn die Antwort des Back-End-Servers für die Testanforderung die Zeichenfolge „unauthorized“ enthält, wird sie als „healthy“ (fehlerfrei) gekennzeichnet.
Andernfalls wird sie mit dieser Meldung als „unhealthy“ (fehlerhaft) gekennzeichnet.

**Lösung:** Sie können dieses Problem beheben, indem Sie die folgenden Schritte ausführen:

1.  Greifen Sie auf den Back-End-Server lokal oder über einen Clientcomputer im Testpfad zu, und überprüfen Sie den Antworttext.

2.  Überprüfen Sie die benutzerdefinierte Testkonfiguration von Application Gateway, um zu überprüfen, ob der Antworttext mit den konfigurierten Einstellungen übereinstimmt

3.  Wenn dies nicht der Fall ist, ändern Sie die Testkonfiguration in den richtigen Zeichenfolgenwert, der akzeptiert werden soll.

Weitere Informationen zum Testabgleich von Application Gateway finden Sie [hier](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#probe-matching).

#### <a name="backend-server-certificate-invalid-ca"></a>Ungültige Zertifizierungsstelle für das Zertifikat des Back-End-Servers

**Nachricht:** The server certificate used by the backend is not signed by a well-known Certificate Authority (CA). (Das vom Back-End verwendete Serverzertifikat ist nicht von einer bekannten Zertifizierungsstelle (CA) signiert.) Whitelist the backend on the application gateway by uploading the root certificate of the server certificate used by the backend. (Fügen Sie das Back-End für Application Gateway der Whitelist hinzu, indem Sie das Stammzertifikat des Serverzertifikats hochladen, das vom Back-End verwendet wird.)

**Ursache:** End-to-End-SSL mit Application Gateway v2 erfordert, dass das Zertifikat des Back-End-Servers überprüft wird, damit der Server als fehlerfrei angesehen wird.
Damit ein SSL-Zertifikat als vertrauenswürdig eingestuft wird, muss dieses Zertifikat des Back-End-Servers von einer Zertifizierungsstelle ausgestellt worden sein, die im vertrauenswürdigen Speicher von Application Gateway enthalten ist. Wenn das Zertifikat nicht von einer vertrauenswürdigen Zertifizierungsstelle ausgestellt wurde (z.B. selbstsignierte Zertifikate), sollten Benutzer das Zertifikat des Ausstellers in Application Gateway hochladen.

**Lösung:** Führen Sie die folgenden Schritte aus, um das vertrauenswürdige Stammzertifikat zu exportieren und in Application Gateway hochzuladen (die folgenden Schritte sind für Windows-Clients vorgesehen).

1.  Melden Sie sich bei dem Computer an, auf dem die Anwendung gehostet wird.

2.  Öffnen Sie „Run“ (Ausführen), indem Sie WIN+R drücken oder mit der rechten Maustaste auf die Schaltfläche „Start“ und dann auf „Run“ (Ausführen) klicken.

3.  Geben Sie „certmgr.msc“ ein, und drücken Sie die EINGABETASTE. Sie können auch im Startmenü nach dem Zertifikat-Manager suchen.

4.  Suchen Sie nach dem Zertifikat (in der Regel befindet es sich unter \'Certificates - Current User\\Personal\\Certificates\'), und öffnen Sie das Zertifikat.

5.  Wählen Sie in den Zertifikateigenschaften die Registerkarte „Certification Path“ (Zertifizierungspfad) aus.

6.  Wählen Sie das Stammzertifikat und dann „View Certificate“ (Zertifikat anzeigen) aus.

7.  Navigieren Sie in den Zertifikateigenschaften zur Registerkarte „Details“.

8.  Wählen Sie auf der Registerkarte „Details“ die Option „Copy to File“ (In Datei kopieren) aus, und speichern Sie die Datei im Base-64-codierten X.509-Format (CER-Format).

9.  Öffnen Sie nach dem Speichern die Seite mit den HTTP-Einstellungen von Application Gateway im Azure-Portal.

10. Öffnen Sie die HTTP-Einstellungen, und klicken Sie auf „Add Certificate“ (Zertifikat hinzufügen). Suchen Sie nach der Zertifikatdatei, die Sie zuvor gespeichert haben.

11. Klicken Sie auf „Save“ (Speichern), um die HTTP-Einstellungen zu speichern.

Alternativ können Sie das Stammzertifikat von einem Clientcomputer exportieren, indem Sie mit einem Browser (unter Umgehung von Application Gateway) direkt auf den Server zugreifen, um das Stammzertifikat aus dem Browser zu exportieren.

Ausführliche Schritte zum Extrahieren und Hochladen vertrauenswürdiger Stammzertifikate in Application Gateway finden Sie [hier](https://docs.microsoft.com/azure/application-gateway/certificates-for-backend-authentication#export-trusted-root-certificate-for-v2-sku).

#### <a name="trusted-root-certificate-mismatch"></a>Konflikt eines vertrauenswürdigen Stammzertifikats

**Nachricht:** The root certificate of the server certificate used by the backend does not match the trusted root certificate added to the application gateway. (Das Stammzertifikat des Serverzertifikats, das vom Back-End verwendet wird, stimmt nicht mit dem vertrauenswürdigen Stammzertifikat überein, das Application Gateway hinzugefügt wurde.) Ensure that you add the correct root certificate to whitelist the backend. (Stellen Sie sicher, dass Sie das richtige Stammzertifikat hinzufügen, um das Back-End in die Whitelist aufzunehmen.)

**Ursache:** End-to-End-SSL mit Application Gateway v2 erfordert, dass das Zertifikat des Back-End-Servers überprüft wird, damit der Server als fehlerfrei angesehen wird.
Damit ein SSL-Zertifikat als vertrauenswürdig eingestuft wird, muss dieses Zertifikat des Back-End-Servers von einer Zertifizierungsstelle ausgestellt worden sein, die im vertrauenswürdigen Speicher von Application Gateway enthalten ist. Wenn das Zertifikat nicht von einer vertrauenswürdigen Zertifizierungsstelle ausgestellt wurde (z.B. selbstsignierte Zertifikate), sollten Benutzer das Zertifikat des Ausstellers in Application Gateway hochladen.

Das Zertifikat, das in die HTTP-Einstellungen von Application Gateway hochgeladen wurde, muss mit dem Stammzertifikat des Back-End-Serverzertifikats übereinstimmen.

**Lösung:** Wenn die oben genannte Fehlermeldung angezeigt wird, bedeutet dies, dass das Zertifikat, das in Application Gateway hochgeladen wurde, und das auf den Back-End-Server hochgeladene Zertifikat nicht übereinstimmen.

Führen Sie die oben genannten Schritte 1 bis 11 aus, um das richtige vertrauenswürdige Stammzertifikat in Application Gateway hochzuladen.

Ausführliche Schritte zum Extrahieren und Hochladen vertrauenswürdiger Stammzertifikate in Application Gateway finden Sie [hier](https://docs.microsoft.com/azure/application-gateway/certificates-for-backend-authentication#export-trusted-root-certificate-for-v2-sku).
> [!NOTE]
> Der genannte Fehler kann auch auftreten, wenn der Back-End-Server nicht die gesamte Kette des Zertifikats (einschließlich Root -> Intermediate (sofern zutreffend) -> Leaf) im Rahmen des TLS-Handshakes austauscht. Um dies zu überprüfen, können Sie OpenSSL-Befehle von jedem beliebigen Client aus verwenden und mithilfe der konfigurierten Einstellungen im Application Gateway-Test eine Verbindung mit dem Back-End-Server herstellen.

Beispiel:
```
OpenSSL> s_client -connect 10.0.0.4:443 -servername www.example.com -showcerts
```
Wenn die Ausgabe nicht die Rückgabe der gesamten Kette des Zertifikats anzeigt, exportieren Sie das Zertifikat erneut mit der vollständigen Kette (einschließlich des Stammzertifikats), und konfigurieren Sie dies auf Ihrem Back-End-Server. 

CONNECTED(00000188)\
depth=0 OU = Domain Control Validated, CN = \*.example.com\
verify error:num=20:unable to get local issuer certificate\
verify return:1\
depth=0 OU = Domain Control Validated, CN = \*.example.com\
verify error:num=21:unable to verify the first certificate\
verify return:1\
\-\-\-\
Certificate chain\
 0 s:/OU=Domain Control Validated/CN=*.example.com\
   i:/C=US/ST=Arizona/L=Scottsdale/O=GoDaddy.com, Inc./OU=http://certs.godaddy.com/repository//CN=Go Daddy Secure Certificate Authority - G2\
\-----BEGIN CERTIFICATE-----\
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx\
\-----END CERTIFICATE-----

#### <a name="backend-certificate-invalid-common-name-cn"></a>Ungültiger allgemeiner Name (CN) des Back-End-Zertifikats

**Nachricht:** The Common Name (CN) of the backend certificate does not match the host header of the probe. (Der allgemeine Name (CN) des Back-End-Zertifikats stimmt nicht mit dem Hostheader des Tests überein.)

**Ursache:** Application Gateway überprüft, ob der in der HTTP-Einstellung des Back-Ends angegebene Hostname mit dem allgemeinen Namen (Common Name, CN) übereinstimmt, der vom SSL-Zertifikat des Back-End-Servers angegeben wird. Dies ist nur ein Standard_v2- und WAF_v2-SKU-Verhalten. Die SNI der Standard- und WAF-SKU wird als FQDN in der Adresse des Back-End-Pools festgelegt.

Wenn in der v2-SKU ein Standardtest (kein benutzerdefinierter Test konfiguriert/zugeordnet) vorhanden ist, wird die SNI über den Hostnamen festgelegt, der in den HTTP-Einstellungen angegeben ist, oder wenn „Pick hostname from backend address“ (Hostnamen aus Back-End-Adresse entnehmen) in den HTTP-Einstellungen angegeben wird und der Back-End-Adresspool einen gültigen FQDN enthält.

Wenn den HTTP-Einstellungen ein benutzerdefinierter Test zugeordnet ist, wird die SNI über den Hostnamen festgelegt, der in der benutzerdefinierten Testkonfiguration angegeben ist. Wenn „Pick hostname from backend HTTP settings“ (Hostnamen aus HTTP-Einstellungen des Back-Ends entnehmen) im benutzerdefinierten Test überprüft wird, wird er über den Hostnamen festgelegt, der in den HTTP- Einstellungen angegeben ist.

Für den Fall, dass „Pick hostname from backend address“ (Hostnamen aus Back-End-Adresse entnehmen) in den HTTP-Einstellungen festgelegt ist, muss der Back-End-Adresspool einen gültigen FQDN enthalten.

Wenn die oben genannte Fehlermeldung angezeigt wird, bedeutet dies, dass der allgemeine Name (Common Name, CN) des Back-End-Zertifikats nicht mit dem im benutzerdefinierten Test konfigurierten Hostnamen oder den HTTP-Einstellungen übereinstimmt (für den Fall, dass „Pick Hostname from Backend HTTP Settings“ ausgewählt ist). Wenn Sie einen Standardtest verwenden, wird der Hostname als „127.0.0.1“ festgelegt. Wenn dies kein gewünschter Wert ist, sollten Sie einen benutzerdefinierten Test erstellen und den HTTP-Einstellungen zuordnen.

**Lösung:**

Führen Sie die folgenden Schritte aus, um das Problem zu beheben.

Windows:

1.  Melden Sie sich bei dem Computer an, auf dem die Anwendung gehostet wird.

2.  Öffnen Sie „Run“ (Ausführen), indem Sie WIN+R drücken oder mit der rechten Maustaste auf die Schaltfläche „Start“ und dann auf „Run“ (Ausführen) klicken.

3.  Geben Sie „certmgr.msc“ ein, und drücken Sie die EINGABETASTE. Sie können auch im Startmenü nach dem Zertifikat-Manager suchen.

4.  Suchen Sie nach dem Zertifikat (in der Regel befindet es sich unter \'Certificates - Current User\\Personal\\Certificates\'), und öffnen Sie das Zertifikat.

5.  Überprüfen Sie auf der Registerkarte „Details“ den Antragstellernamen des Zertifikats.

6.  Überprüfen Sie den CN des Zertifikats aus den Details, und geben Sie im Feld „Hostname“ des benutzerdefinierten Tests oder in den HTTP-Einstellungen (falls „Pick Hostname from Backend HTTP Settings“ ausgewählt ist) den gleichen Namen ein. Wenn dies nicht der gewünschte Hostname für Ihre Website ist, benötigen Sie ein Zertifikat für diese Domäne, oder geben Sie den richtigen Hostnamen in der Konfiguration des benutzerdefinierten Tests bzw. der HTTP-Einstellung ein.

Für Linux mit OpenSSL

1.  Führen Sie diesen Befehl in OpenSSL aus. 
```
openssl x509 -in certificate.crt -text -noout
```

2.  Suchen Sie in den angezeigten Eigenschaften den CN des Zertifikats, und geben Sie im Feld „Hostname“ der HTTP-Einstellungen denselben Wert ein. Wenn dies nicht der gewünschte Hostname für Ihre Website ist, benötigen Sie ein Zertifikat für diese Domäne, oder geben Sie den richtigen Hostnamen in der Konfiguration des benutzerdefinierten Tests bzw. der HTTP-Einstellung ein.

#### <a name="backend-certificate-is-invalid"></a>Back-End-Zertifikat ungültig

**Nachricht:** Backend certificate is invalid. (Das Back-End-Zertifikat ist ungültig.) Current date is not within the \"Valid from\" and \"Valid to\" date range on the certificate. (Das aktuelle Datum entspricht nicht den Datumsbereichen „Gültig ab“ und „Gültig bis“ für das Zertifikat.)

**Ursache:** Jedes Zertifikat verfügt über eine Gültigkeitsdauer, und die HTTPS-Verbindung ist nur dann sicher, wenn das SSL-Zertifikat des Servers gültig ist.
Die aktuellen Daten müssen sich innerhalb des Bereichs „Gültig ab“ und „Gültig bis“ befinden. Wenn dies nicht der Fall ist, wird das Zertifikat als ungültig betrachtet und zu einem Sicherheitsproblem. An diesem Punkt kennzeichnet Application Gateway den Back-End-Server als fehlerhaft.

**Lösung:** Wenn das SSL-Zertifikat abgelaufen ist, erneuern Sie das Zertifikat bei Ihrem Hersteller, und aktualisieren Sie die Servereinstellungen mit dem neuen Zertifikat. Wenn es sich um ein selbstsigniertes Zertifikat handelt, müssen Sie ein gültiges Zertifikat generieren und das Stammzertifikat in die HTTP-Einstellungen von Application Gateway hochladen. Führen Sie dazu die folgenden Schritte aus:

1.  Öffnen Sie Ihre HTTP-Einstellungen von Application Gateway im Portal.

2.  Wählen Sie die HTTP-Einstellungen mit dem abgelaufenen Zertifikat aus, wählen Sie „Add Certificate“ (Zertifikat hinzufügen) aus, und öffnen Sie die neue Zertifikatdatei.

3.  Entfernen Sie das alte Zertifikat mithilfe des Löschsymbols neben dem Zertifikat, und klicken Sie auf dann auf „Save“ (Speichern).

#### <a name="certificate-verification-failed"></a>Fehler bei der Zertifikatüberprüfung

**Nachricht:** The validity of the backend certificate could not be verified. (Die Gültigkeit des Back-End-Zertifikats konnte nicht bestätigt werden.) To find out the reason, check Open SSL diagnostics for the message associated with error code {errorCode}. (Um den Grund zu ermitteln, überprüfen Sie die OpenSSL-Diagnose für die Meldung, die dem Fehlercode {fehlerCode} zugeordnet ist.)

**Ursache:** Dieser Fehler tritt auf, wenn Application Gateway nicht in der Lage ist, die Gültigkeit des Zertifikats zu bestätigen.

**Lösung:** Um dieses Problem zu beheben, überprüfen Sie auf dem Server, ob das Zertifikat ordnungsgemäß erstellt wurde. Beispielsweise können Sie mit [OpenSSL](https://www.openssl.org/docs/man1.0.2/man1/verify.html) das Zertifikat und seine Eigenschaften überprüfen und versuchen, das Zertifikat erneut in die HTTP-Einstellungen von Application Gateway hochzuladen.

<a name="backend-health-status-unknown"></a>Back-End-Integritätsstatus „Unknown“ (Unbekannt)
-------------------------------
Wenn der Back-End-Integritätsstatus als unbekannt gemeldet wird, wird dies wie im folgenden Screenshot im Portal angezeigt:

![Back-End-Integrität von Application Gateway: Unbekannt](./media/application-gateway-backend-health-troubleshooting/appgwunknown.png)

Für den Fall, dass die Back-End-Integrität als „Unbekannt“ angezeigt wird, kann dies an mindestens einem der folgenden Gründe liegen:

1.  NSG im Application Gateway-Subnetz blockiert den eingehenden Zugriff auf die Ports 65503 bis 65534 (v1-SKU) oder 65200 bis 65535 (v2-SKU) aus der Quelle „Internet“
2.  UDR im Application Gateway-Subnetz mit der Standardroute (0.0.0.0/0) mit dem nächsten Hop nicht als „Internet“
3.  Von ExpressRoute-/VPN-Verbindung mit dem VNET über BGP angekündigte Standardroute
4.  Benutzerdefinierter DNS-Server ist im VNET konfiguriert, das keine öffentlichen Domänennamen auflösen kann
5.  Application Gateway befindet sich in einem fehlerhaften Zustand

**Lösung:**

1.  Überprüfen Sie, ob Ihre NSG den Zugriff auf die Ports 65503 bis 65534 (v1-SKU) oder 65200 bis 65535 (v2-SKU) aus dem Internet blockiert.

    a.  Wählen Sie auf der Registerkarte „Overview“ (Übersicht) von Application Gateway den Link „Virtual Network/Subnet“ (Virtuelles Netzwerk/Subnetz) aus.

    b.  Wählen Sie auf der Registerkarte „Subnets“ (Subnetze) ihres virtuellen Netzwerks das Subnetz aus, in dem Application Gateway bereitgestellt wurde.

    c.  Überprüfen Sie, ob eine NSG konfiguriert ist.

    d.  Wenn dies der Fall ist, suchen Sie nach dieser NSG-Ressource auf der Registerkarte „Search“ (Suchen) oder unter „All resources“ (Alle Ressourcen).

    e.  Fügen Sie im Abschnitt „Inbound rules“ (Regeln für eingehenden Datenverkehr) eine Regel für eingehenden Datenverkehr hinzu, um den Zielportbereich 65503 bis 65534 für die v1-SKU oder 65200 bis 65535 für die v2-SKU mit der Quelle „Any“ (Beliebig) oder „Internet“ zuzulassen.

    f.  Klicken Sie auf „Save“ (Speichern), und überprüfen Sie, ob die Integrität des Back-Ends erfolgreich angezeigt werden kann.

    g.  Alternativ dazu können Sie auch [PowerShell bzw. die CLI](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group) verwenden.

2.  Überprüfen Sie, ob Ihre UDR über eine Standardroute (0.0.0.0/0) mit dem nächsten Hop nicht als „Internet“ verfügt.
    
    a.  Führen Sie die Schritte 1.a und 1.b aus, um Ihr Subnetz zu ermitteln.

    b.  Überprüfen Sie, ob UDR-Angaben konfiguriert sind. Wenn dies der Fall ist, suchen Sie nach dieser Ressource über die Suchleiste oder unter „All resources“ (Alle Ressourcen).

    c.  Überprüfen Sie, ob Standardrouten (0.0.0.0/0) mit dem nächsten Hop nicht als Internet vorhanden sind. Wenn es sich entweder um ein virtuelles Gerät oder um Virtual Network Gateway handelt, müssen Sie sicherstellen, dass das virtuelle Gerät oder das lokale Gerät das Paket ordnungsgemäß zurück an das Internetziel weiterleiten kann, ohne das Paket zu ändern.

    d.  Andernfalls ändern Sie den nächsten Hop in „Internet“, klicken Sie auf „Save“ (Speichern), und überprüfen Sie die Back-End-Integrität

3.  Von ExpressRoute-/VPN-Verbindung mit dem VNET über BGP angekündigte Standardroute

    a.  Wenn Sie über eine ExpressRoute-/VPN-Verbindung mit dem VNET über BGP verfügen und eine Standardroute ankündigen, müssen Sie sicherstellen, dass das Paket zurück an das Internetziel weitergeleitet wird, ohne es zu ändern.

    b.  Sie können dies überprüfen, indem Sie die Option „Connection Troubleshoot“ (Problembehandlung der Verbindung) im Application Gateway-Portal verwenden.

    c.  Wählen Sie das Ziel manuell als beliebige internetroutingfähige IP-Adresse wie „1.1.1.1“ und den Zielport beliebig aus, und überprüfen Sie die Konnektivität.

    d.  Wenn der nächste Hop Virtual Network Gateway ist, wird möglicherweise eine Standardroute über ExpressRoute oder VPN angekündigt.

4.  Wenn ein benutzerdefinierter DNS-Server im VNET konfiguriert ist, überprüfen Sie, ob die Server öffentliche Domänen auflösen können. Die Auflösung öffentlicher Domänennamen ist möglicherweise in Szenarien erforderlich, in denen Application Gateway beispielsweise externe Domänen wie OCSP-Server erreichen oder den Sperrstatus des Zertifikats überprüfen muss.

5.  Um zu überprüfen, ob Application Gateway fehlerfrei ist und ausgeführt wird, navigieren Sie im Portal zur Option „Resource Health“ (Ressourcenintegrität), und überprüfen Sie, ob der Status „Healthy“ (Fehlerfrei) lautet. Wenn der Status „Unhealthy“ (Fehlerhaft) oder „Degraded“ (Beeinträchtigt) angezeigt wird, [wenden Sie sich an den Support](https://azure.microsoft.com/support/options/).

<a name="next-steps"></a>Nächste Schritte
----------

Weitere Informationen zur Application Gateway-Diagnose und zu den -Protokollen finden Sie [hier](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics).

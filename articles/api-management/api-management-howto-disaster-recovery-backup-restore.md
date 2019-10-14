---
title: Implementieren der Notfallwiederherstellung mit Sichern und Wiederherstellen in Azure API Management | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Sichern und Wiederherstellen zur Notfallwiederherstellung in Azure API Management verwenden.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/26/2019
ms.author: apimpm
ms.openlocfilehash: 9c97723687484e8af82d63b6fb4999401a69fb2c
ms.sourcegitcommit: 7868d1c40f6feb1abcafbffcddca952438a3472d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2019
ms.locfileid: "71958532"
---
# <a name="how-to-implement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a>So implementieren Sie die Notfallwiederherstellung mit Sichern und Wiederherstellen von Diensten in Azure API Management

Indem Sie Ihre APIs über Azure API Management veröffentlichen und verwalten, profitieren Sie von Fehlertoleranz- und Infrastrukturfunktionen, die Sie sonst erst manuell entwickeln, implementieren und verwalten müssten. Die Azure-Plattform minimiert einen Großteil der potenziellen Ausfälle zu einem Bruchteil der Kosten.

Bei Verfügbarkeitsproblemen in der Region, in der Ihr API Management-Dienst gehostet wird, sollten Sie jederzeit dazu in der Lage sein, Ihren Dienst in einer anderen Region wiederherzustellen. Abhängig von Ihrer RTO (Recovery Time Objective) empfiehlt es sich gegebenenfalls, einen Standbydienst in mindestens einer Region bereitzuhalten. Sie haben auch die Möglichkeit, die Konfiguration und Inhalte der Regionen gemäß Ihrer RPO (Recovery Point Objective) mit dem aktiven Dienst zu synchronisieren. Die Features zur Sicherung und Wiederherstellung von Diensten bieten die notwendigen Bausteine für die Implementierung einer Notfallwiederherstellungsstrategie.

Sicherungs- und Wiederherstellungsvorgänge können auch zur Replikation der API Management-Dienstkonfiguration zwischen Betriebsumgebungen, z. B. Entwicklung und Staging, verwendet werden. Beachten Sie, dass auch Laufzeitdaten wie Benutzer und Abonnements kopiert werden, was unter Umständen nicht immer wünschenswert ist.

Diese Anleitung zeigt, wie Sie Sicherungs- und Wiederherstellungsvorgänge automatisieren und wie Sie eine erfolgreiche Authentifizierung von Sicherungs- und Wiederherstellungsanforderungen durch Azure Resource Manager sicherstellen.

> [!IMPORTANT]
> Die benutzerdefinierte Hostnamenkonfiguration des Zieldiensts wird durch den Wiederherstellungsvorgang nicht geändert. Wir empfehlen, den gleichen benutzerdefinierten Hostnamen und das gleiche TLS-Zertifikat sowohl für aktive Dienste als auch für Standbydienste zu verwenden, sodass der Datenverkehr nach Abschluss der Wiederherstellung durch eine einfache DNS-CNAME-Änderung an die Standbyinstanz umgeleitet werden kann.
>
> Der Sicherungsvorgang erfasst keine voraggregierten Protokolldaten, die in Berichten auf dem Analytics-Blatt im Azure-Portal verwendet werden.

> [!WARNING]
> Jede Sicherung läuft nach 30 Tagen ab. Wenn Sie versuchen, eine Sicherung nach dem Ablaufzeitraum von 30 Tagen wiederherzustellen, erhalten Sie die Fehlermeldung `Cannot restore: backup expired`.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="authenticating-azure-resource-manager-requests"></a>Authentifizieren von Anforderungen des Azure-Ressourcen-Managers

> [!IMPORTANT]
> Die REST-API für die Sicherung und Wiederherstellung verwendet den Azure-Ressourcen-Manager und weist eine andere Authentifizierungsmethode als die REST-APIs für die Verwaltung von API Management-Entitäten auf. In den Schritten in diesem Abschnitt wird beschrieben, wie Anforderungen des Azure-Ressourcen-Managers authentifiziert werden. Weitere Informationen finden Sie unter [Authentifizieren von Anforderungen des Azure-Ressourcen-Managers](/rest/api/index).

Alle Aufgaben, die Sie mithilfe von Azure Resource Manager für Ressourcen ausführen, müssen entsprechend den folgenden Schritten mit Azure Active Directory authentifiziert werden:

-   Fügen Sie dem Azure Active Directory-Mandanten eine Anwendung hinzu.
-   Legen Sie Berechtigungen für die hinzugefügte Anwendung fest.
-   Rufen Sie das Token zum Authentifizieren von Anforderungen an den Azure-Ressourcen-Manager ab.

### <a name="create-an-azure-active-directory-application"></a>Erstellen einer Azure Active Directory-Anwendung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Verwenden Sie das Abonnement, das Ihre API Management-Dienstinstanz enthält, und navigieren Sie zur Registerkarte **App-Registrierungen** in **Azure Active Directory** (Azure Active Directory > Verwalten > App-Registrierungen).

    > [!NOTE]
    > Wenn das Azure Active Directory-Standardverzeichnis in Ihrem Konto nicht angezeigt wird, bitten Sie den Administrator des Azure-Abonnements, die erforderlichen Berechtigungen für das Konto zu erteilen.

3. Klicken Sie auf **Registrierung einer neuen Anwendung**.

    Das Fenster **Erstellen** wird auf der rechten Seite angezeigt. Dort geben Sie die relevanten Informationen der AAD-App ein.

4. Geben Sie einen Namen für die Anwendung ein.
5. Wählen Sie als Anwendungstyp **Nativ** aus.
6. Geben Sie eine Platzhalter-URL wie z.B. `http://resources` für den **Umleitungs-URI** ein, da es sich um ein Pflichtfeld handelt. Der Wert wird jedoch später nicht verwendet. Klicken Sie auf das Kontrollkästchen, um die Anwendung zu speichern.
7. Klicken Sie auf **Create**.

### <a name="add-an-application"></a>Hinzufügen einer Anwendung

1. Nachdem die Anwendung erstellt wurde, klicken Sie auf **Einstellungen**.
2. Klicken Sie auf **Erforderliche Berechtigungen**.
3. Klicken Sie auf **+Hinzufügen**.
4. Drücken Sie **API auswählen**.
5. Wählen Sie **Windows** **Azure-Dienstverwaltungs-API** aus.
6. Drücken Sie **Auswählen**.

    ![Hinzufügen von Berechtigungen](./media/api-management-howto-disaster-recovery-backup-restore/add-app.png)

7. Klicken Sie neben der neu hinzugefügten Anwendung auf **Delegierte Berechtigungen**, und aktivieren Sie das Kontrollkästchen **Access Azure Service Management (Vorschau)** .
8. Drücken Sie **Auswählen**.
9. Klicken Sie auf **Berechtigungen erteilen**.

### <a name="configuring-your-app"></a>Konfigurieren der App

Vor dem Aufrufen der APIs, die die Sicherung erstellen und wiederherstellen, muss ein Token abgerufen werden. Im folgenden Beispiel wird das Token mit dem NuGet-Paket [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) abgerufen.

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System;

namespace GetTokenResourceManagerRequests
{
    class Program
    {
        static void Main(string[] args)
        {
            var authenticationContext = new AuthenticationContext("https://login.microsoftonline.com/{tenant id}");
            var result = authenticationContext.AcquireTokenAsync("https://management.azure.com/", "{application id}", new Uri("{redirect uri}"), new PlatformParameters(PromptBehavior.Auto)).Result;

            if (result == null) {
                throw new InvalidOperationException("Failed to obtain the JWT token");
            }

            Console.WriteLine(result.AccessToken);

            Console.ReadLine();
        }
    }
}
```

Ersetzen Sie `{tenant id}`, `{application id}` und `{redirect uri}` entsprechend den folgenden Anweisungen:

1. Ersetzen Sie `{tenant id}` durch die Mandanten-ID der Azure Active Directory-Anwendung, die Sie erstellt haben. Auf die ID können Sie durch Klicken auf **App-Registrierungen** -> **Endpunkte** zugreifen.

    ![Endpunkte][api-management-endpoint]

2. Ersetzen Sie `{application id}` durch den Wert, den Sie durch Navigieren zur Seite **Einstellungen** erhalten.
3. Ersetzen Sie `{redirect uri}` durch den Wert der Registerkarte **Weiterleitungs-URIs** Ihrer Azure Active Directory-Anwendung.

    Wenn die Werte angegeben wurden, sollte im Codebeispiel ein Token ähnlich dem folgenden Beispiel zurückgegeben werden:

    ![Tokenverschlüsselung][api-management-arm-token]

    > [!NOTE]
    > Das Token kann nach einer bestimmten Zeit ablaufen. Führen Sie das Codebeispiel erneut aus, um ein neues Token zu generieren.

## <a name="calling-the-backup-and-restore-operations"></a>Aufrufen der Sicherungs- und Wiederherstellungsvorgänge

Die REST-APIs sind [API Management-Dienst – Backup](/rest/api/apimanagement/2019-01-01/apimanagementservice/backup) und [API Management-Dienst – Restore](/rest/api/apimanagement/2019-01-01/apimanagementservice/restore).

Legen Sie vor dem Aufrufen der in den folgenden Abschnitten beschriebenen Vorgänge zur Sicherung und Wiederherstellung den Autorisierungsanforderungsheader für den REST-Aufruf fest.

```csharp
request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);
```

### <a name="step1"></a>Sichern eines API Management-Diensts

Zum Sichern eines API Management-Diensts führen Sie die folgende HTTP-Anforderung aus:

```http
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}
```

Hierbei gilt:

-   `subscriptionId` – ID des Abonnements, das den API Management-Dienst enthält, den Sie sichern möchten
-   `resourceGroupName` – der Name der Ressourcengruppe Ihres Azure API Management-Diensts
-   `serviceName` – der Name des zu sichernden API Management-Diensts zum Zeitpunkt seiner Erstellung
-   `api-version` – durch `2018-06-01-preview` ersetzen

Geben Sie im Hauptteil der Anforderung das Azure-Zielspeicherkonto, den Zugriffsschlüssel, den Blobcontainernamen und den Sicherungsnamen an:

```json
{
    "storageAccount": "{storage account name for the backup}",
    "accessKey": "{access key for the account}",
    "containerName": "{backup container name}",
    "backupName": "{backup blob name}"
}
```

Legen Sie für den `Content-Type`-Anforderungsheader den Wert `application/json` fest.

Die Sicherung ist ein länger anhaltender Vorgang, der bis zum Abschluss mehrere Minuten dauern kann. Falls die Anforderung erfolgreich war und der Sicherungsvorgang eingeleitet wurde, erhalten Sie den `202 Accepted` Antwortstatuscode mit einem `Location`-Header. Senden Sie GET-Anforderungen der URL im `Location` -Header, um den Status des Vorgangs zu ermitteln. Während der Sicherung erhalten Sie weiterhin den Statuscode „202 Accepted“. Mit dem Antwortcode `200 OK` wird der erfolgreiche Abschluss des Sicherungsvorgangs angezeigt.

Beachten Sie die folgenden Einschränkungen für Sicherungsanforderungen:

-   Der im Hauptteil der Anforderung angegebene **Container** **muss vorhanden sein**.
-   Vermeiden Sie während der Sicherung **Änderungen in der Dienstverwaltung**, wie z.B. SKU-Upgrades oder Herabstufungen, Änderungen im Domänennamen usw.
-   Die Wiederherstellung einer Sicherung nach ihrer Erstellung **wird nur 30 Tage lange garantiert**.
-   **Nutzungsdaten** zum Erstellen von Analyseberichten sind in der Sicherung **nicht enthalten**. Verwenden Sie [Azure API Management REST API][azure api management rest api] , um regelmäßig Analyseberichte zur Aufbewahrung abzurufen.
-   Darüber hinaus sind die folgenden Elemente nicht Bestandteil der Sicherungsdaten: benutzerdefinierte SSL-Zertifikate für Domänen und alle vom Kunden hochgeladenen Zwischen- oder Stammzertifikate, Inhalte des Entwicklerportals und Integrationseinstellungen für virtuelle Netzwerke.
-   Die Häufigkeit, mit der Sie Dienstsicherungen durchführen, wirkt sich auf das Ziel Ihres Wiederherstellungspunkts aus. Um die Auswirkungen zu minimieren, empfehlen wir, regelmäßige Sicherungen zu implementieren und bei Bedarf Sicherungen durchzuführen, wenn Sie Änderungen an Ihrem API Management-Dienst vorgenommen haben.
-   **Änderungen** an der Dienstkonfiguration (z.B. APIs, Richtlinien, Erscheinungsbild des Entwicklerportals), die während des Sicherungsvorgangs vorgenommen werden, sind ggf. **nicht in der Sicherung enthalten und gehen verloren**.
-   Sie müssen den Zugriff von der Steuerungsebene auf das Azure Storage-Konto **zulassen**. Kunden müssen die folgenden eingehenden IP-Adressen in ihrem Storage-Konto für das Backup öffnen. 
    > 13.84.189.17/32, 13.85.22.63/32, 23.96.224.175/32, 23.101.166.38/32, 52.162.110.80/32, 104.214.19.224/32, 13.64.39.16/32, 40.81.47.216/32, 51.145.179.78/32, 52.142.95.35/32, 40.90.185.46/32, 20.40.125.155/32
### <a name="step2"></a>Wiederherstellen eines API Management-Diensts

Zum Wiederherstellen eines API Management-Diensts aus einer zuvor erstellten Sicherung führen Sie die folgende HTTP-Anforderung aus:

```http
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}
```

Hierbei gilt:

-   `subscriptionId` – ID des Abonnements, das den API Management-Dienst enthält, in den Sie eine Sicherung erstellen
-   `resourceGroupName` – Name der Ressourcengruppe mit dem Azure-API Management-Dienst, in dem Sie eine Sicherung wiederherstellen
-   `serviceName` – der Name des wiederherzustellenden API Management-Diensts zum Zeitpunkt seiner Erstellung
-   `api-version` – durch `2018-06-01-preview` ersetzen

Geben Sie im Anforderungstext den Speicherort der Sicherungsdatei an. Fügen Sie das Azure-Zielspeicherkonto, den Zugriffsschlüssel, den Blobcontainernamen und den Sicherungsnamen an:

```json
{
    "storageAccount": "{storage account name for the backup}",
    "accessKey": "{access key for the account}",
    "containerName": "{backup container name}",
    "backupName": "{backup blob name}"
}
```

Legen Sie für den `Content-Type`-Anforderungsheader den Wert `application/json` fest.

Die Wiederherstellung ist ein länger anhaltender Vorgang, der bis zum Abschluss bis zu 30 Minuten dauern kann. Falls die Anforderung erfolgreich war und der Wiederherstellungsvorgang eingeleitet wurde, erhalten Sie den `202 Accepted` Antwortstatuscode mit einem `Location`-Header. Senden Sie GET-Anforderungen der URL im `Location` -Header, um den Status des Vorgangs zu ermitteln. Während der Wiederherstellung erhalten Sie weiterhin den Statuscode „202 Accepted“. Mit dem Antwortcode `200 OK` wird der erfolgreiche Abschluss des Wiederherstellungsvorgangs angezeigt.

> [!IMPORTANT]
> **Die SKU** des wiederherzustellenden Diensts **muss** mit der SKU des gesicherten Diensts übereinstimmen.
>
> **Änderungen** an der Dienstkonfiguration (z.B. APIs, Richtlinien, Erscheinungsbild des Entwicklerportals), die während des Wiederherstellungsvorgangs vorgenommen werden, **könnten überschrieben werden**.

<!-- Dummy comment added to suppress markdown lint warning -->

> [!NOTE]
> Sicherungs- und Wiederherstellungsvorgänge können außerdem mithilfe der PowerShell-Befehle [_Backup-AzApiManagement_](/powershell/module/az.apimanagement/backup-azapimanagement) bzw. [_Restore-AzApiManagement_](/powershell/module/az.apimanagement/restore-azapimanagement) ausgeführt werden.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die folgenden Ressourcen für verschiedene Vorgehensweisen für den Sicherungs- und Wiederherstellungsvorgang an.

-   [Replicate Azure API Management Accounts (in englischer Sprache)](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/)
-   [Automating API Management Backup and Restore with Logic Apps](https://github.com/Azure/api-management-samples/tree/master/tutorials/automating-apim-backup-restore-with-logic-apps) (Automatisieren der Sicherung und Wiederherstellung von API Management mit Logic Apps)
-   [Azure API Management: Sichern und Wiederherstellen der Konfiguration](https://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
     _–der Ansatz von Stuart entspricht nicht der offiziellen Anleitung, ist aber sehr interessant._

[backup an api management service]: #step1
[restore an api management service]: #step2
[azure api management rest api]: https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest
[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png
[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png

---
title: Verwenden von verwalteten Identitäten zum Authentifizieren von Azure Stream Analytics-Aufträgen für die Power BI-Ausgabe (Vorschauversion)
description: In diesem Artikel wird die Verwendung von verwalteten Identitäten zum Authentifizieren von Azure Stream Analytics-Aufträgen für die Power BI-Ausgabe beschrieben.
author: cedarbaum
ms.author: sacedarb
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 10/31/2019
ms.openlocfilehash: 0c5f64e08446698bbd8d1ee4af5454e3aa1dd5ff
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73693791"
---
# <a name="use-managed-identity-to-authenticate-your-azure-stream-analytics-job-to-power-bi-preview"></a>Verwenden von verwalteten Identitäten zum Authentifizieren von Azure Stream Analytics-Aufträgen in Power BI (Vorschauversion)

Die [Authentifizierung von verwalteten Identitäten](../active-directory/managed-identities-azure-resources/overview.md) für die Ausgabe in Power BI ist für Azure Stream Analytics in der Vorschauphase verfügbar. Dies ermöglicht Stream Analytics Aufträgen direkten Zugriff auf einen Arbeitsbereich in Ihrem Power BI-Konto. Mit diesem Feature können Bereitstellungen von Stream Analytics-Aufträgen vollständig automatisiert werden, da es nicht mehr erforderlich ist, dass sich ein Benutzer über das Azure-Portal interaktiv bei Power BI anmeldet. Darüber hinaus werden Aufträge mit langer Ausführungszeit, die in Power BI schreiben, jetzt besser unterstützt, da der Auftrag nicht mehr regelmäßig erneut autorisiert werden muss.

Dieser Artikel zeigt Ihnen, wie Sie verwaltete Identitäten für die Power BI-Ausgaben eines Stream Analytics-Auftrags über das Azure-Portal und mit einer Azure Resource Manager-Bereitstellung aktivieren.

## <a name="prerequisites"></a>Voraussetzungen

Folgendes ist für die Verwendung dieses Features erforderlich:

- Ein Power BI-Konto mit einer [Pro-Lizenz](https://docs.microsoft.com/power-bi/service-admin-purchasing-power-bi-pro)

- Ein aktualisierter Arbeitsbereich in Ihrem Power BI-Konto Weitere Informationen finden Sie in der [Power BI-Ankündigung](https://powerbi.microsoft.com/blog/announcing-new-workspace-experience-general-availability-ga/) dieses Features.

## <a name="create-a-stream-analytics-job-using-the-azure-portal"></a>Erstellen eines Stream Analytics-Auftrags über das Azure-Portal

1. Erstellen Sie einen neuen Stream Analytics-Auftrag, oder öffnen Sie einen vorhandenen im Azure-Portal. Wählen Sie in der Menüleiste auf der linken Bildschirmseite unter **Konfigurieren** die Option **Verwaltete Identität**. Vergewissern Sie sich, dass „Systemseitig zugewiesene verwaltete Identität verwenden“ ausgewählt ist, und wählen Sie dann am unteren Bildschirmrand die Schaltfläche **Speichern** aus.

   ![Konfigurieren einer verwalteten Identität für Stream Analytics](./media/common/stream-analytics-enable-managed-identity.png)

2. Geben Sie vor dem Konfigurieren der Ausgabe dem Stream Analytics-Auftrag Zugriff auf Ihren Power BI Arbeitsbereich, indem Sie die Anweisungen im Abschnitt [Erteilen des Zugriffs auf Ihren Power BI-Arbeitsbereich für den Stream Analytics-Auftrag](#give-the-stream-analytics-job-access-to-your-power-bi-workspace) in diesem Artikel befolgen.

3. Navigieren Sie zum Abschnitt **Ausgaben** Ihres Stream Analytics-Auftrags, und wählen Sie **+ Hinzufügen** und dann **Power BI** aus. Wählen Sie als Nächstes die Schaltfläche **Autorisieren** aus, und melden Sie sich mit Ihrem Power BI-Konto an.

   ![Autorisieren mit einem Power BI-Konto](./media/stream-analytics-powerbi-output-managed-identity/stream-analytics-authorize-powerbi.png)

4. Nach der Autorisierung wird eine Dropdownliste mit allen Arbeitsbereichen aufgefüllt, auf die Sie Zugriff haben. Wählen Sie den Arbeitsbereich aus, den Sie im vorherigen Schritt autorisiert haben. Wählen Sie dann für „Authentifizierungsmodus“ den Eintrag **Verwaltete Identität** aus. Wählen Sie schließlich die Schaltfläche **Speichern** aus.

   ![Konfigurieren der Power BI-Ausgabe mit der verwalteten Identität](./media/stream-analytics-powerbi-output-managed-identity/stream-analytics-configure-powerbi-with-managed-id.png)

## <a name="azure-resource-manager-deployment"></a>Azure Resource Manager-Bereitstellung

Mit Azure Resource Manager können Sie die Bereitstellung Ihres Stream Analytics-Auftrags vollständig automatisieren. Sie können Resource Manager-Vorlagen mit Azure PowerShell oder der [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) bereitstellen. In den folgenden Beispielen wird die Azure-Befehlszeilenschnittstelle verwendet.


1. Sie können eine **Microsoft.StreamAnalytics/streamingjobs**-Ressource mit einer verwalteten Identität erstellen, indem Sie die folgende Eigenschaft in den Ressourcenabschnitt Ihrer Resource Manager-Vorlage einfügen:

    ```json
    "identity": {
        "type": "SystemAssigned",
    }
    ```

   Diese Eigenschaft weist Azure Resource Manager an, die Identität für Ihren Stream Analytics-Auftrag zu erstellen und zu verwalten. Nachfolgend finden Sie ein Beispiel für eine Resource Manager-Vorlage, die einen Stream Analytics-Auftrag mit aktivierter verwalteter Identität und eine Power BI-Ausgabesenke mit verwalteter Identität bereitstellt:

    ```json
    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "resources": [
            {
                "apiVersion": "2017-04-01-preview",
                "name": "pbi_managed_id",
                "location": "[resourceGroup().location]",
                "type": "Microsoft.StreamAnalytics/StreamingJobs",
                "identity": {
                    "type": "systemAssigned"
                },
                "properties": {
                    "sku": {
                        "name": "standard"
                    },
                    "outputs":[
                        {
                            "name":"output",
                            "properties":{
                                "datasource":{
                                    "type":"PowerBI",
                                    "properties":{
                                        "dataset": "dataset_name",
                                        "table": "table_name",
                                        "groupId": "01234567-89ab-cdef-0123-456789abcdef",
                                        "authenticationMode": "Msi"
                                    }
                                }
                            }
                        }
                    ]
                }
            }
        ]
    }
    ```

    Stellen Sie den oben angegebenen Auftrag mit dem folgenden Azure CLI-Befehl in der Ressourcengruppe **ExampleGroup** bereit:

    ```azurecli
    az group deployment create --resource-group ExampleGroup -template-file StreamingJob.json
    ```

2. Nachdem der Auftrag erstellt wurde, rufen Sie mit Azure Resource Manager die vollständige Definition des Auftrags ab.

    ```azurecli
    az resource show --ids /subscriptions/<subsription-id>/resourceGroups/<resource-group>/providers/Microsoft.StreamAnalytics/StreamingJobs/<resource-name>
    ```

    Der obige Befehl gibt eine Antwort ähnlich der nachstehenden zurück:

    ```json
    {
        "id": "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.StreamAnalytics/streamingjobs/<resource-name>",
        "identity": {
            "principalId": "<principal-id>",
            "tenantId": "<tenant-id>",
            "type": "SystemAssigned",
            "userAssignedIdentities": null
        },
        "kind": null,
        "location": "West US",
        "managedBy": null,
        "name": "<resource-name>",
        "plan": null,
        "properties": {
            "compatibilityLevel": "1.0",
            "createdDate": "2019-07-12T03:11:30.39Z",
            "dataLocale": "en-US",
            "eventsLateArrivalMaxDelayInSeconds": 5,
            "jobId": "<job-id>",
            "jobState": "Created",
            "jobStorageAccount": null,
            "jobType": "Cloud",
            "outputErrorPolicy": "Stop",
            "package": null,
            "provisioningState": "Succeeded",
            "sku": {
                "name": "Standard"
            }
        },
        "resourceGroup": "<resource-group>",
        "sku": null,
        "tags": null,
        "type": "Microsoft.StreamAnalytics/streamingjobs"
    }
    ```

    Wenn Sie die REST-API von Power BI zum Hinzufügen des Stream Analytics Auftrags zu Ihrem Power BI-Arbeitsbereich verwenden möchten, notieren Sie sich die zurückgegebene principalId.

3. Nachdem der Auftrag nun erstellt wurde, fahren Sie mit dem Abschnitt [Erteilen des Zugriffs auf Ihren Power BI-Arbeitsbereich für den Stream Analytics-Auftrag](#give-the-stream-analytics-job-access-to-your-power-bi-workspace) in diesem Artikel fort.


## <a name="give-the-stream-analytics-job-access-to-your-power-bi-workspace"></a>Erteilen des Zugriffs auf Ihren Power BI-Arbeitsbereich für den Stream Analytics-Auftrag

Nachdem der Stream Analytics Auftrag nun erstellt wurde, kann ihm Zugriff auf einen Power BI-Arbeitsbereich gewährt werden.

### <a name="use-the-power-bi-ui"></a>Verwenden der Power BI-Benutzeroberfläche

   > [!Note]
   > Wenn Sie den Stream Analytics Auftrag Ihrem Power BI-Arbeitsbereich mithilfe der Benutzeroberfläche hinzufügen möchten, müssen Sie auch in den **Entwicklereinstellungen** im Power BI-Verwaltungsportal den Dienstprinzipalzugriff aktivieren. Weitere Informationen finden Sie unter [Erste Schritte mit einem Dienstprinzipal](https://docs.microsoft.com/power-bi/developer/embed-service-principal#get-started-with-a-service-principal).

1. Navigieren Sie zu den Zugriffseinstellungen des Arbeitsbereichs. Weitere Informationen finden Sie in diesem Artikel: [Erteilen des Zugriffs auf Ihren Arbeitsbereich](https://docs.microsoft.com/power-bi/service-create-the-new-workspaces#give-access-to-your-workspace).

2. Geben Sie den Namen Ihres Stream Analytics-Auftrags in das Textfeld ein, und wählen Sie als Zugriffsebene **Mitwirkende** aus.

3. Wählen Sie **Hinzufügen** aus, und schließen Sie den Bereich.

   ![Hinzufügen eines Stream Analytics-Auftrags zum Power BI-Arbeitsbereich](./media/stream-analytics-powerbi-output-managed-identity/stream-analytics-add-job-to-powerbi-workspace.png)

### <a name="use-the-power-bi-rest-api"></a>Verwenden der Power BI-REST-API

Der Stream Analytics-Auftrag kann dem Arbeitsbereich auch als Mitwirkender hinzugefügt werden, indem die REST-API „Gruppenbenutzer hinzufügen“ direkt verwendet wird. Die vollständige Dokumentation für diese API finden Sie hier: [Gruppen – Gruppenbenutzer hinzufügen](https://docs.microsoft.com/rest/api/power-bi/groups/addgroupuser).

**Beispiel für eine Anforderung**
```http
POST https://api.powerbi.com/v1.0/myorg/groups/{groupId}/users
```
Anforderungstext
```json
{
    "groupUserAccessRight": "Contributor",
    "identifier": "<principal-id>",
    "principalType": "App"
}
```

## <a name="limitations"></a>Einschränkungen
Im Folgenden finden Sie Einschränkungen für dieses Feature:

- Klassische Power BI-Arbeitsbereiche werden nicht unterstützt.

- Azure-Konten ohne Azure Active Directory.

- Mehrinstanzenfähiger Zugriff wird nicht unterstützt. Der für einen bestimmten Stream Analytics-Auftrag erstellte Dienstprinzipal muss in dem Azure Active Directory-Mandanten ausgeführt werden, in dem der Auftrag erstellt wurde, und er kann nicht für eine Ressource in einem anderen Azure Active Directory-Mandanten verwendet werden.

- [Vom Benutzer zugewiesene Identitäten](../active-directory/managed-identities-azure-resources/overview.md) werden nicht unterstützt. Das bedeutet, dass Sie nicht Ihren eigenen Dienstprinzipal für Ihren Stream Analytics-Auftrag eingeben können. Der Dienstprinzipal muss von Azure Stream Analytics generiert werden.

## <a name="next-steps"></a>Nächste Schritte

* [Integration eines Power BI-Dashboards mit Azure Stream Analytics](./stream-analytics-power-bi-dashboard.md)
* [Grundlegendes zu den Ausgaben von Azure Stream Analytics](./stream-analytics-define-outputs.md)

---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/14/2019
ms.author: glenga
ms.openlocfilehash: e319356d555f26354ea29dc7be068bf6168abb17
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67455150"
---
## <a name="local-settings-file"></a>Datei für lokale Einstellungen

Die Datei „local.settings.json“ speichert App-Einstellungen, Verbindungszeichenfolgen und Einstellungen, die von lokalen Entwicklungstools verwendet werden. Einstellungen in der Datei „local.settings.json“ werden nur bei der lokalen Ausführung verwendet. Die Datei mit den lokalen Einstellungen hat folgende Struktur:

```json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "<language worker>",
    "AzureWebJobsStorage": "<connection-string>",
    "AzureWebJobsDashboard": "<connection-string>",
    "MyBindingConnection": "<binding-connection-string>"
  },
  "Host": {
    "LocalHttpPort": 7071,
    "CORS": "*",
    "CORSCredentials": false
  },
  "ConnectionStrings": {
    "SQLConnectionString": "<sqlclient-connection-string>"
  }
}
```

Die folgenden Einstellungen werden bei lokaler Ausführung unterstützt:

| Einstellung      | BESCHREIBUNG                            |
| ------------ | -------------------------------------- |
| **`IsEncrypted`** | Wenn diese Einstellung auf `true` festgelegt wird, werden alle Werte mithilfe eines Schlüssels des lokalen Computers verschlüsselt. Wird mit `func settings`-Befehlen verwendet. Der Standardwert ist `false`. |
| **`Values`** | Ein Array von Anwendungseinstellungen und Verbindungszeichenfolgen, die bei der lokalen Ausführung verwendet werden. Diese Schlüssel-Wert-Paare (Zeichenfolge-Zeichenfolge) entsprechen den Anwendungseinstellungen in Ihrer Funktions-App in Azure, wie z.B. [`AzureWebJobsStorage`]. Viele Trigger und Bindungen verfügen über eine Eigenschaft, die auf eine App-Einstellung für die Verbindungszeichenfolge verweist, z. B. `Connection` für den [Blob Storage-Trigger](../articles/azure-functions/functions-bindings-storage-blob.md#trigger---configuration). Für solche Eigenschaften muss eine Anwendungseinstellung im Array `Values` definiert sein. <br/>[`AzureWebJobsStorage`] ist eine erforderliche App-Einstellung für andere Trigger als HTTP. <br/>Für Version 2.x der Functions-Runtime ist die Einstellung [`FUNCTIONS_WORKER_RUNTIME`] erforderlich, die mit Core Tools für Ihr Projekt generiert wird. <br/> Wenn der [Azure-Speicheremulator](../articles/storage/common/storage-use-emulator.md) lokal installiert ist, können Sie [`AzureWebJobsStorage`] auf `UseDevelopmentStorage=true` festlegen, und Core Tools verwendet den Emulator. Dies ist während der Entwicklung hilfreich, doch sollten Sie vor der Bereitstellung einen Test mit einer tatsächlichen Speicherverbindung durchführen.<br/> Werte müssen Zeichenfolgen und dürfen nicht JSON-Objekte oder Arrays sein. Einstellungsnamen dürfen weder einen Doppelpunkt (`:`) noch einen doppelten Unterstrich (`__`) enthalten; diese sind für die Runtime reserviert.  |
| **`Host`** | Die Einstellungen in diesem Abschnitt passen den Hostprozess von Functions bei der lokalen Ausführung an. Diese sind getrennt von den host.json-Einstellungen, die auch bei der Ausführung in Azure angewendet werden. |
| **`LocalHttpPort`** | Legt den Standardport fest, der bei der Ausführung des lokalen Functions-Host verwendet wird (`func host start` und `func run`). Die Befehlszeilenoption `--port` hat Vorrang vor diesem Wert. |
| **`CORS`** | Definiert die für die [Ressourcenfreigabe zwischen verschiedenen Ursprüngen (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) zulässigen Ursprünge. Ursprünge werden als durch Trennzeichen getrennte Liste ohne Leerzeichen bereitgestellt. Den Platzhalterwert (\*) wird unterstützt, wodurch Anforderungen von einem beliebigen Ursprung zulässig sind. |
| **`CORSCredentials`** |  Legen Sie für diese Einstellung „true“ fest, um `withCredentials`-Anforderungen zuzulassen. |
| **`ConnectionStrings`** | Verwenden Sie diese Sammlung nicht für die Verbindungszeichenfolgen, die von Ihren Funktionsbindungen verwendet werden. Diese Sammlung wird nur von Frameworks verwendet, die Verbindungszeichenfolgen üblicherweise aus dem Abschnitt `ConnectionStrings` einer Konfigurationsdatei abrufen, z. B. [Entity Framework](https://msdn.microsoft.com/library/aa937723(v=vs.113).aspx). Verbindungszeichenfolgen in diesem Objekt werden der Umgebung mit dem Anbietertyp [System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx) hinzugefügt. Elemente in dieser Sammlung werden nicht mit anderen App-Einstellungen in Azure veröffentlicht. Sie müssen diese Werte explizit zur Sammlung `Connection strings` in den Einstellungen Ihrer Funktions-App hinzufügen. Bei der Erstellung einer [`SqlConnection`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(v=vs.110).aspx)-Klasse in Ihrem Funktionscode sollten Sie den Verbindungszeichenfolgenwert zusammen mit den anderen Verbindungen in den **Anwendungseinstellungen** im Portal speichern. |

[AzureWebJobsStorage]: ../articles/azure-functions/functions-app-settings.md#azurewebjobsstorage

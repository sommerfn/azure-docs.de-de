---
title: Konfigurieren der Einstellungen einer Funktions-App in Azure
description: Hier erhalten Sie Informationen zum Konfigurieren von Azure Funktionen-App-Einstellungen.
author: ggailey777
manager: gwallace
ms.assetid: 81eb04f8-9a27-45bb-bf24-9ab6c30d205c
ms.service: azure-functions
ms.topic: conceptual
ms.date: 08/14/2019
ms.author: glenga
ms.custom: cc996988-fb4f-47
ms.openlocfilehash: 7ad7f6156bbd8ea86e3e71bda4b23dac9722a0ef
ms.sourcegitcommit: ee61ec9b09c8c87e7dfc72ef47175d934e6019cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2019
ms.locfileid: "70170734"
---
# <a name="manage-your-function-app"></a>Verwalten Ihrer Funktions-App 

In Azure Functions wird mit einer Funktionen-App der Ausführungskontext für die einzelnen Funktionen angegeben. Funktionen-App-Verhalten gelten für alle von einer bestimmten Funktionen-App gehosteten Funktionen. Alle Funktionen in einer Funktions-App müssen in der gleichen [Sprache](supported-languages.md) geschrieben sein. 

Einzelne Funktionen in einer Funktions-App werden gemeinsam bereitgestellt und skaliert. Beim Skalieren der Funktions-App nutzen alle Funktionen in der gleichen Funktions-App die Ressourcen gemeinsam (pro Instanz). 

Verbindungszeichenfolgen, Umgebungsvariablen und andere Anwendungseinstellungen werden für jede Funktions-App separat definiert. Daten, die von Funktions-Apps gemeinsam genutzt werden sollen, müssen extern in einem persistenten Speicher gespeichert werden.

In diesem Thema erfahren Sie, wie Sie Ihre Funktions-Apps konfigurieren und verwalten. 

> [!TIP]  
> Viele Konfigurationsoptionen können auch mithilfe der [Azure-Befehlszeilenschnittstelle] verwaltet werden. 

## <a name="get-started-in-the-azure-portal"></a>Erste Schritte im Azure-Portal

Wechseln Sie zunächst zum [Azure-Portal], und melden Sie sich bei Ihrem Azure-Konto an. Geben Sie auf der Suchleiste oben im Portal den Namen der Funktionen-App ein, und wählen Sie sie in der Liste aus. Nach dem Auswählen der Funktionen-App wird die folgende Seite angezeigt:

![„Übersicht“ für Funktionen-Apps im Azure-Portal](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-main.png)

Auf der Übersichtsseite können Sie zu allen Punkten navigieren, die Sie zur Verwaltung Ihrer Funktions-App benötigen, insbesondere zu den **[Anwendungseinstellungen](#settings)** und **[Plattformfeatures](#platform-features)** .

## <a name="settings"></a>Anwendungseinstellungen

Die Registerkarte **Anwendungseinstellungen** verwaltet Einstellungen, die von Ihrer Funktions-App verwendet werden. Diese Einstellungen werden verschlüsselt gespeichert, und Sie müssen **Werte anzeigen** auswählen, um die Werte im Portal anzuzeigen. Auf Anwendungseinstellungen kann auch über die Azure CLI zugegriffen werden.

### <a name="portal"></a>Portal

Wählen Sie zum Hinzufügen einer Einstellung im Portal **Neue Anwendungseinstellung** aus, und fügen Sie das neue Schlüssel-Wert-Paar hinzu.

![Einstellungen der Funktions-App im Azure-Portal.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Der Befehl [`az functionapp config appsettings list`](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-list) gibt die vorhandenen Anwendungseinstellungen zurück, wie im folgenden Beispiel zu sehen:

```azurecli-interactive
az functionapp config appsettings list --name <FUNCTION_APP_NAME> \
--resource-group <RESOURCE_GROUP_NAME>
```

Der Befehl [`az functionapp config appsettings set`](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set) dient zum Hinzufügen oder Aktualisieren einer Anwendungseinstellung. Im folgenden Beispiel wird eine Einstellung mit einem Schlüssel namens `CUSTOM_FUNCTION_APP_SETTING` und dem Wert `12345` erstellt:


```azurecli-interactive
az functionapp config appsettings set --name <FUNCTION_APP_NAME> \
--resource-group <RESOURCE_GROUP_NAME> \
--settings CUSTOM_FUNCTION_APP_SETTING=12345
```

### <a name="use-application-settings"></a>Verwenden von Anwendungseinstellungen

[!INCLUDE [functions-environment-variables](../../includes/functions-environment-variables.md)]

Wenn Sie eine Funktions-App lokal entwickeln, müssen Sie lokale Kopien dieser Werte in der Projektdatei „local.settings.json“ speichern. Weitere Informationen finden Sie unter [Datei für lokale Einstellungen](functions-run-local.md#local-settings-file).

## <a name="platform-features"></a>Plattformfeatures

![Registerkarte „Plattformfeatures“ für Funktionen-Apps.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-features-tab.png)

Funktionen-Apps werden auf der Azure App Service-Plattform ausgeführt und verwaltet. So haben die Funktionen-Apps Zugriff auf die meisten Features der Azure-Kernplattform für das Webhosting. Auf der Registerkarte **Plattformfeatures** greifen Sie auf die vielen Features der App Service-Plattform zu, die Sie in Ihren Funktionen-Apps verwenden können. 

> [!NOTE]
> Nicht alle App Service-Features sind verfügbar, wenn eine Funktionen-App nach dem verbrauchsbasierten Hostingplan ausgeführt wird.

Im weiteren Verlauf dieses Artikels werden schwerpunktmäßig folgende App Service-Features im Azure-Portal behandelt, die für Functions nützlich sind:

+ [App Service-Editor](#editor)
+ [Console](#console)
+ [Erweiterte Tools (Kudu)](#kudu)
+ [Bereitstellungsoptionen](#deployment)
+ [CORS](#cors)
+ [Authentifizierung](#auth)

Weitere Informationen zum Verwenden von App Service-Einstellungen finden Sie unter [Konfigurieren von Web-Apps in Azure App Service](../app-service/configure-common.md).

### <a name="editor"></a>App Service-Editor

![App Service-Editor](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-appservice-editor.png)

Der App Service-Editor ist ein leistungsstarker Editor im Portal, mit dem Sie JSON-Konfigurationsdateien und Codedateien gleichermaßen bearbeiten können. Bei Auswahl dieser Option wird eine separate Browserregisterkarte mit einem einfachen Editor gestartet. Sie können damit im Git-Repository arbeiten, Code ausführen und debuggen und Einstellungen von Funktionen-Apps ändern. Im Vergleich zum integrierten Funktionen-Editor bietet dieser Editor eine erweiterte Entwicklungsumgebung für Ihre Funktionen.  

Es empfiehlt sich, Funktionen auf dem lokalen Computer zu entwickeln. Wenn Sie Funktionen lokal entwickeln und in Azure veröffentlichen, sind Ihre Projektdateien im Portal schreibgeschützt. Weitere Informationen finden Sie unter [Lokales Codieren und Testen von Azure Functions](functions-develop-local.md).

### <a name="console"></a>Konsole

![Konsole für Funktionen-Apps](./media/functions-how-to-use-azure-function-app-settings/configure-function-console.png)

Die Konsole im Portal ist das ideale Entwicklungstool, wenn Sie Funktionen-Apps über die Befehlszeile konfigurieren möchten. Häufig verwendete Befehle sind z.B. Erstellen von Verzeichnissen und Dateien, Navigation sowie das Ausführen von Batchdateien und -skripts. 

Bei der lokalen Entwicklung wird die Verwendung von [Azure Functions Core Tools](functions-run-local.md) und [Azure-Befehlszeilenschnittstelle] empfohlen.

### <a name="kudu"></a>Erweiterte Tools (Kudu)

![Konfigurieren von Kudu](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-kudu.png)

Die erweiterten Tools für App Service (auch bekannt als Kudu) ermöglichen den Zugriff auf erweiterte Verwaltungsfunktionen der Funktionen-App. Über Kudu können Sie Systeminformationen, App-Einstellungen, Umgebungsvariablen, Websiteerweiterungen, HTTP-Header und Servervariablen verwalten. Sie können **Kudu** auch starten, indem Sie zum SCM-Endpunkt für die Funktionen-App navigieren, z.B. zu `https://<myfunctionapp>.scm.azurewebsites.net/`. 


### <a name="deployment"></a>Bereitstellungscenter

Wenn Sie Ihren Funktionencode mithilfe einer Quellcodeverwaltungslösung entwickeln und verwalten, können Sie im Bereitstellungscenter beim Erstellen und Bereitstellen auf die Quellcodeverwaltung zurückgreifen. Ihr Projekt wird in Azure erstellt und bereitgestellt, wenn Sie Aktualisierungen vornehmen. Weitere Informationen finden Sie unter [Bereitstellungstechnologien in Azure Functions](functions-deployment-technologies.md).

### <a name="cors"></a>Cross-Origin Resource Sharing

Um die Ausführung von schädlichem Code auf dem Client zu verhindern, blockieren moderne Browser Anforderungen, die von Webanwendungen an Ressourcen in einer separaten Domäne gerichtet werden. Mit [Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS) kann ein Header vom Typ `Access-Control-Allow-Origin` die Ursprünge deklarieren, von denen Endpunkte in Ihrer Funktions-App aufgerufen werden dürfen.

#### <a name="portal"></a>Portal

Wenn Sie die Liste **Zulässige Ursprünge** für Ihre Funktions-App konfigurieren, wird der Header `Access-Control-Allow-Origin` automatisch allen Antworten von HTTP-Endpunkten in Ihrer Funktions-App hinzugefügt. 

![Konfigurieren der CORS-Liste für die Funktions-App](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-cors.png)

Bei Verwendung des Platzhalterzeichens (`*`) werden alle anderen Domänen ignoriert. 

Verwenden Sie den Befehl [`az functionapp cors add`](/cli/azure/functionapp/cors#az-functionapp-cors-add),um der Liste mit den zulässigen Ursprüngen eine Domäne hinzuzufügen. Im folgenden Beispiel wird die Domäne „contoso.com“ hinzugefügt:

```azurecli-interactive
az functionapp cors add --name <FUNCTION_APP_NAME> \
--resource-group <RESOURCE_GROUP_NAME> \
--allowed-origins https://contoso.com
```

Verwenden Sie den Befehl [`az functionapp cors show`](/cli/azure/functionapp/cors#az-functionapp-cors-show), um die derzeit zulässigen Ursprünge aufzulisten.

### <a name="auth"></a>Authentifizierung

![Konfigurieren der Authentifizierung für eine Funktionen-App](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-authentication.png)

Wenn Funktionen einen HTTP-Trigger verwenden, können Sie festlegen, dass Aufrufe zunächst authentifiziert werden müssen. App Service unterstützt die Azure Active Directory-Authentifizierung sowie die Anmeldung über Anbieter von sozialen Netzwerken wie Facebook, Microsoft und Twitter. Weitere Informationen zum Konfigurieren von bestimmten Authentifizierungsanbietern finden Sie unter [Authentifizierung und Autorisierung in Azure App Service](../app-service/overview-authentication-authorization.md). 


## <a name="next-steps"></a>Nächste Schritte

+ [Konfigurieren von Web-Apps in Azure App Service](../app-service/configure-common.md)
+ [Kontinuierliche Bereitstellung für Azure Functions](functions-continuous-deployment.md)

[Azure-Befehlszeilenschnittstelle]: /cli/azure/
[Azure-Portal]: https://portal.azure.com

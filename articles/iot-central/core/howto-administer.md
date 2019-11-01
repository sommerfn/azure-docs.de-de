---
title: Verwalten Ihrer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie als Administrator Ihre Azure IoT Central-Anwendung verwalten, indem Sie den Anwendungsnamen und die URL ändern, Bilder hochladen, eine Anwendung kopieren und löschen.
author: viv-liu
ms.author: viviali
ms.date: 08/26/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 4c729d2502464df021df4d03e989d489b2d1ecea
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2019
ms.locfileid: "72942839"
---
# <a name="manage-your-iot-central-application"></a>Verwalten einer IoT Central-Anwendung

[!INCLUDE [iot-central-original-pnp](../../../includes/iot-central-original-pnp-note.md)]

In diesem Artikel wird beschrieben, wie Sie als Administrator Anwendungen verwalten können, indem Sie den Anwendungsnamen und die URL ändern und Bilder hochladen. Außerdem werden Sie sehen, wie Sie eine Anwendung in ihrer Azure IoT Central-Anwendung kopieren und löschen.

Um auf den Abschnitt **Verwaltung** zugreifen und diesen verwenden zu können, muss Ihnen die Rolle **Administrator** für eine Azure IoT Central-Anwendung zugewiesen sein. Wenn Sie eine Azure IoT Central-Anwendung erstellen, wird Ihnen automatisch die Rolle **Administrator** für die jeweilige Anwendung zugewiesen. 

## <a name="change-application-name-and-url"></a>Ändern von Anwendungsnamen und -URLs

Auf der Seite **Anwendungseinstellungen** können Sie den Namen und die URL Ihrer Anwendung ändern. Wählen Sie im Anschluss **Speichern** aus.

![Seite „Anwendungseinstellungen“](media/howto-administer/image0-a.png)

Wenn Ihr Administrator ein benutzerdefiniertes Design für Ihre Anwendung erstellt, enthält diese Seite eine Option zum Ausblenden des **Anwendungsnamens** in der Benutzeroberfläche. Das ist hilfreich, wenn das Anwendungslogo im benutzerdefinierten Design den Anwendungsnamen enthält. Weitere Informationen finden Sie unter [Anpassen der Azure IoT Central-Benutzeroberfläche](./howto-customize-ui.md).

> [!Note]
> Wenn Sie Ihre URL ändern, kann Ihre alte URL von einem anderen Azure IoT Central-Kunden verwendet werden. In diesem Fall kann diese nicht mehr von Ihnen verwendet werden. Wenn Sie Ihre URL ändern, funktioniert die alte URL nicht mehr, und Sie müssen Ihre Benutzer über die neue zu verwendende URL benachrichtigen.

## <a name="prepare-and-upload-image"></a>Vorbereiten und Hochladen eines Bilds

Wie Sie das Anwendungsbilds ändern, erfahren Sie unter [Vorbereiten und Hochladen von Bildern in eine Azure IoT Central-Anwendung](howto-prepare-images.md).

## <a name="copy-an-application"></a>Kopieren einer Anwendung

Sie können eine Kopie jeder Anwendung erstellen, mit Ausnahme von Geräteinstanzen, Gerätedatenverlauf und Benutzerdaten. Die Kopie ist eine Anwendung, für die Gebühren in Form von nutzungsbasierter Zahlung anfallen. Auf diese Weise können Sie keine Anwendung mit einer Testversion erstellen.

Wählen Sie **Kopieren** aus. Geben Sie im Dialogfenster die Details für die neue Anwendung mit nutzungsbasierter Zahlung ein. Wählen Sie dann **Kopieren** aus, um zu bestätigen, dass Sie den Vorgang fortsetzen möchten. Weitere Informationen zu den Feldern in diesem Formular finden Sie im Schnellstart [Erstellen einer Anwendung](quick-deploy-iot-central.md).

![Seite „Anwendungseinstellungen“](media/howto-administer/appcopy2.png)

Nach erfolgreichem Abschluss des App-Kopiervorgangs können Sie über den Link zur neuen Anwendung navigieren.

![Seite „Anwendungseinstellungen“](media/howto-administer/appcopy3a.png)

Beim Kopieren einer Anwendung wird auch die Definition von Regeln und E-Mail-Aktionen kopiert. Einige Aktionen wie Flow, Logic Apps usw. sind über die Regel-ID an bestimmte Regeln gebunden. Wenn eine Regel in eine andere Anwendung kopiert wird, erhält sie eine eigene Regel-ID. In diesem Fall müssen Benutzer eine neue Aktion erstellen und diese dann der neuen Regel zuordnen. Generell ist es eine gute Idee, die Regeln und Aktionen zu überprüfen, damit sie in der neuen App auf dem neuesten Stand sind.

> [!WARNING]
> Wenn ein Dashboard Kacheln enthält, in denen Informationen zu bestimmten Geräten angezeigt werden, erscheint in der neuen Anwendung die Meldung **Die angeforderte Ressource wurde nicht gefunden** in diesen Kacheln. Sie müssen diese Kacheln erneut konfigurieren, damit in der neuen Anwendung Informationen zu Geräten angezeigt werden.

## <a name="delete-an-application"></a>Löschen einer Anwendung

Verwenden Sie die Schaltfläche **Löschen**, um Ihre IoT Central-Anwendung dauerhaft zu löschen. Durch diese Aktion werden alle Daten im Zusammenhang mit der Anwendung endgültig gelöscht.

> [!Note]
> Zum Löschen einer Anwendung benötigen Sie auch Berechtigungen zum Löschen von Ressourcen im Azure-Abonnement, das Sie beim Erstellen der Anwendung gewählt haben. Weitere Informationen finden Sie unter [Verwalten des Zugriffs mithilfe der RBAC und des Azure-Portals](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).


## <a name="manage-programatically"></a>Programmgesteuertes Verwalten

Azure Resource Manager SDK-Pakete für IoT Central sind für Node, Python, C#, Ruby, Java und Go verfügbar. Mit diesen Paketen können Sie IoT Central-Anwendungen erstellen, auflisten, aktualisieren oder löschen. Die Pakete enthalten Hilfsprogramme zum Verwalten von Authentifizierung und Fehlerbehandlung.

Beispiele zur Verwendung der Azure Resource Manager SDKs finden Sie unter [https://github.com/emgarten/iotcentral-arm-sdk-examples](https://github.com/emgarten/iotcentral-arm-sdk-examples).

Weitere Informationen finden Sie in den folgenden GitHub-Repositorys und -Paketen:

| Sprache | Repository | Paket |
| ---------| ---------- | ------- |
| Knoten | [https://github.com/Azure/azure-sdk-for-node](https://github.com/Azure/azure-sdk-for-node) | [https://www.npmjs.com/package/azure-arm-iotcentral](https://www.npmjs.com/package/azure-arm-iotcentral)
| Python |[https://github.com/Azure/azure-sdk-for-python](https://github.com/Azure/azure-sdk-for-python) | [https://pypi.org/project/azure-mgmt-iotcentral](https://pypi.org/project/azure-mgmt-iotcentral)
| C# | [https://github.com/Azure/azure-sdk-for-net](https://github.com/Azure/azure-sdk-for-net) | [https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral](https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral)
| Ruby | [https://github.com/Azure/azure-sdk-for-ruby](https://github.com/Azure/azure-sdk-for-ruby) | [https://rubygems.org/gems/azure_mgmt_iot_central](https://rubygems.org/gems/azure_mgmt_iot_central)
| Java | [https://github.com/Azure/azure-sdk-for-java](https://github.com/Azure/azure-sdk-for-java) | [https://search.maven.org/search?q=a:azure-mgmt-iotcentral](https://search.maven.org/search?q=a:azure-mgmt-iotcentral)
| Go | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go) | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go)

## <a name="next-steps"></a>Nächste Schritte
 
Nachdem Sie nun mehr über die Verwaltung Ihrer Azure IoT Central-Anwendung erfahren haben, wird im nächsten Schritt empfohlen, sich mit dem [Verwalten von Benutzern und Rollen](howto-manage-users-roles.md) in Azure IoT Central vertraut zu machen.
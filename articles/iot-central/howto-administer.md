---
title: Verwalten einer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie als Administrator eine Azure IoT Central-Anwendung verwalten.
author: viv-liu
ms.author: viviali
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 87ed31836fcda922b085ec951eb6d9d14542db6a
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2019
ms.locfileid: "65467556"
---
# <a name="administer-your-iot-central-application"></a>Verwalten einer IoT Central-Anwendung

Wenn Sie eine IoT Central-Anwendung erstellt haben, können Sie im Abschnitt **Verwaltung** die folgenden Aktionen ausführen:

- Verwalten von Anwendungseinstellungen
- Verwalten von Benutzern
- Verwalten von Rollen
- Anzeigen Ihrer Rechnung
- Umstellen der kostenlosen Testversion auf nutzungsbasierte Zahlung
- Exportieren von Daten
- Verwalten von Geräteverbindungen
- Verwenden von Zugriffstoken für Entwicklertools
- Anpassen der Benutzeroberfläche Ihrer Anwendung
- Anpassen von Hilfelinks in der Anwendung
- Programmgesteuertes Verwalten von IoT Central

Um auf den Abschnitt **Verwaltung** zugreifen und diesen verwenden zu können, muss Ihnen die Rolle **Administrator** für eine Azure IoT Central-Anwendung zugewiesen sein. Wenn Sie eine Azure IoT Central-Anwendung erstellen, wird Ihnen automatisch die Rolle **Administrator** für die jeweilige Anwendung zugewiesen. Im Abschnitt [Verwalten von Benutzern](#manage-users) in diesem Artikel wird erläutert, wie Sie anderen Benutzern die Rolle **Administrator** zuweisen.

## <a name="manage-application-settings"></a>Verwalten von Anwendungseinstellungen

### <a name="change-application-name-and-url"></a>Ändern von Anwendungsnamen und -URLs
Auf der Seite **Anwendungseinstellungen** können Sie den Namen und die URL Ihrer Anwendung ändern. Wählen Sie im Anschluss **Speichern** aus.

![Seite „Anwendungseinstellungen“](media/howto-administer/image0-a.png)

Wenn Ihr Administrator ein benutzerdefiniertes Design für Ihre Anwendung erstellt, enthält diese Seite eine Option zum Ausblenden des **Anwendungsnamens** in der Benutzeroberfläche. Das ist hilfreich, wenn das Anwendungslogo im benutzerdefinierten Design den Anwendungsnamen enthält. Weitere Informationen finden Sie unter [Anpassen der Azure IoT Central-Benutzeroberfläche](./howto-customize-ui.md).

> [!Note]
> Wenn Sie Ihre URL ändern, kann Ihre alte URL von einem anderen Azure IoT Central-Kunden verwendet werden. In diesem Fall kann diese nicht mehr von Ihnen verwendet werden. Wenn Sie Ihre URL ändern, funktioniert die alte URL nicht mehr, und Sie müssen Ihre Benutzer über die neue zu verwendende URL benachrichtigen.

### <a name="prepare-and-upload-image"></a>Vorbereiten und Hochladen eines Bilds
Wie Sie das Anwendungsbilds ändern, erfahren Sie unter [Vorbereiten und Hochladen von Bildern in eine Azure IoT Central-Anwendung](howto-prepare-images.md).

### <a name="copy-an-application"></a>Kopieren einer Anwendung
Sie können eine Kopie jeder Anwendung erstellen, mit Ausnahme von Geräteinstanzen, Gerätedatenverlauf und Benutzerdaten. Die Kopie ist eine Anwendung, für die Gebühren in Form von nutzungsbasierter Zahlung anfallen. Auf diese Weise können Sie keine Anwendung mit einer Testversion erstellen.

Wählen Sie **Kopieren** aus. Geben Sie im Dialogfenster die Details für die neue Anwendung mit nutzungsbasierter Zahlung ein. Wählen Sie dann **Kopieren** aus, um zu bestätigen, dass Sie den Vorgang fortsetzen möchten. Weitere Informationen zu den Feldern in diesem Formular finden Sie im Schnellstart [Erstellen einer Anwendung](quick-deploy-iot-central.md).

![Seite „Anwendungseinstellungen“](media/howto-administer/appcopy2.png)

Nach erfolgreichem Abschluss des App-Kopiervorgangs können Sie über den Link zur neuen Anwendung navigieren.

![Seite „Anwendungseinstellungen“](media/howto-administer/appcopy3a.png)

> [!Note]
> Beim Kopieren einer Anwendung wird auch die Definition von Regeln und Aktionen kopiert. Da Benutzer mit Zugriff auf Ihre ursprüngliche App jedoch nicht in die kopierte App übernommen werden, müssen Sie Benutzer manuell zu Aktionen wie E-Mail hinzufügen, für die Benutzer eine erforderliche Komponente sind. Generell ist es eine gute Idee, die Regeln und Aktionen zu überprüfen, damit sie in der neuen App auf dem neuesten Stand sind.

### <a name="delete-an-application"></a>Löschen einer Anwendung

> [!Note]
> Zum Löschen einer Anwendung benötigen Sie auch Berechtigungen zum Löschen von Ressourcen im Azure-Abonnement, das Sie beim Erstellen der Anwendung gewählt haben. Weitere Informationen finden Sie unter [Verwalten des Zugriffs mithilfe der RBAC und des Azure-Portals](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).

Verwenden Sie die Schaltfläche **Löschen**, um Ihre IoT Central-Anwendung dauerhaft zu löschen. Durch diese Aktion werden alle Daten im Zusammenhang mit der Anwendung endgültig gelöscht.

## <a name="manage-users"></a>Verwalten von Benutzern

### <a name="add-users"></a>Hinzufügen von Benutzern

Jeder Benutzer muss ein Benutzerkonto besitzen, um sich bei einer Azure IoT Central-Anwendung anmelden und auf diese zugreifen zu können. Azure IoT Central unterstützt Microsoft-Konten (MSAs) und Azure Active Directory-Konten (Azure AD). Azure Active Directory-Gruppen werden in Azure IoT Central derzeit nicht unterstützt.

Weitere Informationen finden Sie in der [Hilfe zum Microsoft-Konto](https://support.microsoft.com/products/microsoft-account?category=manage-account) und unter [Schnellstart: Hinzufügen neuer Benutzer zu Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory).

1. Um einen Benutzer einer IoT Central-Anwendung hinzuzufügen, rufen Sie im Abschnitt **Verwaltung** die Seite **Benutzer** auf.

    ![Liste der Benutzer](media/howto-administer/image1.png)

1. Klicken Sie auf der Seite **Benutzer** auf **+ Benutzer hinzufügen**, um einen Benutzer hinzuzufügen.

1. Wählen Sie aus der Dropdownliste **Rolle** eine Rolle für den Benutzer aus. Weitere Informationen zu Rollen finden Sie in diesem Artikel im Abschnitt [Verwalten von Rollen](#manage-roles).

    ![Rollenauswahl](media/howto-administer/image3.png)

    > [!NOTE]
    >  Geben Sie zum Massenhinzufügen von Benutzern die Benutzer-IDs aller hinzuzufügenden Benutzer durch Semikolons getrennt ein. Wählen Sie eine Rolle aus der Dropdownliste **Rolle** aus. Klicken Sie dann auf **Speichern**.

### <a name="edit-the-roles-that-are-assigned-to-users"></a>Bearbeiten der den Benutzern zugewiesenen Rollen

Rollen können nach ihrer Zuweisung nicht geändert werden. Um die einem Benutzer zugewiesene Rolle zu ändern, löschen Sie den Benutzer und fügen den Benutzer dann erneut mit einer anderen Rolle hinzu.

### <a name="delete-users"></a>Löschen von Benutzern

Aktivieren Sie zum Löschen von Benutzern auf der Seite **Benutzer** die entsprechenden Kontrollkästchen. Wählen Sie anschließend die Option **Löschen**.

## <a name="manage-roles"></a>Verwalten von Rollen

Mit Rollen können Sie steuern, wer in Ihrer Organisation verschiedene Aufgaben in IoT Central ausführen kann. Sie können Benutzern Ihrer Anwendung drei unterschiedliche Rollen zuweisen.

### <a name="administrator"></a>Administrator

Benutzer mit der Rolle **Administrator** haben Zugriff auf alle Funktionen in einer Anwendung.

Dem Benutzer, der eine Anwendung erstellt, wird automatisch die Rolle **Administrator** zugewiesen. Es muss immer mindestens ein Benutzer mit der Rolle **Administrator** vorhanden sein.

### <a name="application-builder"></a>Anwendungsersteller

Benutzer mit der Rolle **Anwendungsersteller** können sämtliche Aktionen in einer Anwendung ausführen, ausgenommen der Verwaltung der Anwendung. Anwendungsersteller können Gerätevorlagen und Geräte erstellen, bearbeiten und löschen, Gerätesätze verwalten sowie Analysen und Aufträge ausführen. Anwendungsersteller haben keinen Zugriff auf den Bereich **Verwaltung** der Anwendung.

### <a name="application-operator"></a>Anwendungsoperator

Benutzer mit der Rolle **Anwendungsoperator** können Gerätevorlagen nicht ändern und die Anwendung nicht verwalten. Anwendungsoperatoren können Geräte hinzufügen und löschen, Gerätesätze verwalten sowie Analysen und Aufträge ausführen. Anwendungsoperatoren haben keinen Zugriff auf die Seiten **Anwendungsersteller** und **Verwaltung**.

## <a name="view-your-bill"></a>Anzeigen Ihrer Rechnung

Um Ihre Rechnung anzuzeigen, navigieren Sie im Abschnitt **Verwaltung** zur Seite **Abrechnung**. Die Seite mit der Azure-Abrechnung wird auf einer neuen Registerkarte geöffnet. Auf dieser wird für jede Ihrer Azure IoT Central-Anwendung die jeweilige Rechnung angezeigt.

### <a name="convert-your-trial-to-pay-as-you-go"></a>Umstellen der kostenlosen Testversion auf nutzungsbasierte Zahlung

Sie können Ihre Anwendung mit der Testversion in eine Anwendung mit nutzungsbasierter Zahlung umwandeln. Das sind die Unterschiede zwischen diesen beiden Versionen von Anwendungen:

- Anwendungen mit einer **Testversion** können sieben Tage kostenlos genutzt werden, bevor die Version abläuft. Bevor das geschieht, können sie jederzeit auf die nutzungsbasierte Zahlung umgestellt werden.
- Anwendungen mit **nutzungsbasierter Zahlung** werden pro Gerät berechnet, wobei die ersten fünf Geräte kostenlos sind.

Weitere Informationen zu den Preisen finden Sie unter [Azure IoT Central – Preise](https://azure.microsoft.com/pricing/details/iot-central/).

Um diesen Self-Service-Prozess durchzuführen, gehen Sie folgendermaßen vor:

1. Navigieren Sie im Abschnitt **Verwaltung** zur Seite **Abrechnung**.

    ![Status der Testversion](media/howto-administer/freetrialbilling.png)

1. Wählen Sie **Auf nutzungsbasierte Zahlung umstellen** aus.

    ![Umwandeln der Testversion](media/howto-administer/convert.png)

1. Wählen Sie die entsprechende Azure Active Directory-Instanz und dann das Azure-Abonnement aus, das für Ihre Anwendung mit nutzungsbasierter Zahlung verwendet werden soll.

1. Wenn Sie **Umwandeln** auswählen, wird Ihre Anwendung auf die nutzungsbasierte Zahlung umgestellt. Von diesem Zeitpunkt an fallen Gebühren an.

## <a name="export-data"></a>Exportieren von Daten

Sie können **Kontinuierlicher Datenexport** aktivieren, um Daten von Messungen, Geräten und Gerätevorlagen in Ihr Azure Blob Storage-Konto zu exportieren. Erfahren Sie mehr dazu, wie Sie [Ihre Daten exportieren](howto-export-data.md).

## <a name="manage-device-connection"></a>Verwalten von Geräteverbindungen

Verbinden Sie Geräte in Ihrer Anwendung im benötigten Umfang mit den hier vorgestellten Schlüsseln und Zertifikaten. Lernen Sie mehr über das [Verbinden von Geräten](concepts-connectivity.md).

## <a name="use-access-tokens"></a>Verwenden von Zugriffstoken

Generieren Sie Zugriffstoken, um sie in Entwicklertools zu verwenden. Derzeit ist das einzige verfügbare Entwicklertool der IoT Central-Explorer zum Überwachen von Gerätemeldungen sowie von Änderungen an Eigenschaften und Einstellungen. Lernen Sie mehr über den [IoT Central-Explorer](howto-use-iotc-explorer.md).

## <a name="customize-your-application"></a>Anpassen Ihrer Anwendung

Weitere Informationen zum Ändern der Farben und Symbole in Ihrer Anwendung finden Sie unter [Anpassen der Azure IoT Central-Benutzeroberfläche](./howto-customize-ui.md).

## <a name="customize-help"></a>Anpassen der Hilfe

Weitere Informationen zum Hinzufügen von benutzerdefinierten Hilfelinks in Ihrer Anwendung finden Sie unter [Anpassen der Azure IoT Central-Benutzeroberfläche](./howto-customize-ui.md).

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

Nachdem Sie nun erfahren haben, wie Ihre Azure IoT Central-Anwendung verwaltet wird, wird als Nächstes der folgende Schritt empfohlen:

> [!div class="nextstepaction"]
> [Einrichten der Gerätevorlage](howto-set-up-template.md)

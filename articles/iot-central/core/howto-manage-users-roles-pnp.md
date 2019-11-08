---
title: Verwalten von Benutzern und Rollen in Ihrer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie als Administrator Benutzer und Rollen in Ihrer Azure IoT Central-Anwendung verwalten.
author: lmasieri
ms.author: lmasieri
ms.date: 10/22/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: corywink
ms.openlocfilehash: c51af15d2334e873c8c7c399322fbd1279de3784
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73468191"
---
# <a name="manage-users-and-roles-in-your-iot-central-application-preview-features"></a>Verwalten von Benutzern und Rollen in Ihrer IoT Central-Anwendung (Previewfunktion)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

In diesem Artikel wird beschrieben, wie Sie als Administrator Benutzer in Ihrer Azure IoT Central-Anwendung hinzufügen, bearbeiten und löschen. Außerdem erfahren Sie, wie Sie Rollen in Ihrer Azure IoT Central-Anwendung verwalten.

Um auf den Abschnitt **Verwaltung** zugreifen und diesen verwenden zu können, muss Ihnen die Rolle **Administrator** für eine Azure IoT Central-Anwendung zugewiesen sein. Wenn Sie eine Azure IoT Central-Anwendung erstellen, wird Ihnen automatisch die Rolle **Administrator** für die diese Anwendung hinzugefügt.

## <a name="add-users"></a>Hinzufügen von Benutzern

Jeder Benutzer muss ein Benutzerkonto besitzen, um sich bei einer Azure IoT Central-Anwendung anmelden und auf diese zugreifen zu können. Azure IoT Central unterstützt Microsoft-Konten (MSAs) und Azure Active Directory-Konten (Azure AD). Azure Active Directory-Gruppen werden in Azure IoT Central derzeit nicht unterstützt.

Weitere Informationen finden Sie in der [Hilfe zum Microsoft-Konto](https://support.microsoft.com/products/microsoft-account?category=manage-account) und unter [Schnellstart: Hinzufügen neuer Benutzer zu Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory).

1. Um einen Benutzer einer IoT Central-Anwendung hinzuzufügen, rufen Sie im Abschnitt **Verwaltung** die Seite **Benutzer** auf.
    
    > [!div class="mx-imgBorder"]
    >![Verwalten von Benutzern](media/howto-manage-users-roles-pnp/manage-users-pnp.png)

1. Klicken Sie auf der Seite **Benutzer** auf **+ Benutzer hinzufügen**, um einen Benutzer hinzuzufügen.

1. Wählen Sie aus der Dropdownliste **Rolle** eine Rolle für den Benutzer aus. Weitere Informationen zu Rollen finden Sie in diesem Artikel im Abschnitt [Verwalten von Rollen](#manage-roles).

    > [!div class="mx-imgBorder"]
    >![Hinzufügen von Benutzern und Auswählen von Rollen](media/howto-manage-users-roles-pnp/add-user-pnp.png)

    > [!NOTE]
    > Ein Benutzer mit einer benutzerdefinierten Rolle, die ihm die Berechtigung zum Hinzufügen anderer Benutzer gewährt, kann nur Benutzer einer Rolle mit denselben oder niedrigeren Berechtigungen wie die eigene Rolle hinzufügen.

### <a name="edit-the-roles-that-are-assigned-to-users"></a>Bearbeiten der den Benutzern zugewiesenen Rollen

Rollen können nach ihrer Zuweisung nicht mehr geändert werden. Um die einem Benutzer zugewiesene Rolle zu ändern, löschen Sie den Benutzer und fügen den Benutzer dann erneut mit einer anderen Rolle hinzu.

> [!NOTE]
> Die zugewiesenen Rollen gelten nur für die IoT Central-Anwendung und können nicht über das Azure-Portal verwaltet werden.

## <a name="delete-users"></a>Löschen von Benutzern

Aktivieren Sie zum Löschen von Benutzern auf der Seite **Benutzer** die entsprechenden Kontrollkästchen. Wählen Sie anschließend die Option **Löschen**.

## <a name="manage-roles"></a>Verwalten von Rollen

Mit Rollen können Sie steuern, wer in Ihrer Organisation verschiedene Aufgaben in IoT Central ausführen darf. Es gibt drei integrierte Rollen, die Sie Benutzern Ihrer Anwendung zuweisen können. Sie können auch [benutzerdefinierte Rollen erstellen](#create-a-custom-role), wenn Sie eine präzisere Steuerung benötigen.

> [!div class="mx-imgBorder"]
> ![Verwalten der Rollenauswahl](media/howto-manage-users-roles-pnp/manage-roles-pnp.png)

### <a name="administrator"></a>Administrator

Benutzer mit der Rolle **Administrator** können jeden Aspekt der Anwendung verwalten und steuern, einschließlich der Abrechnung.

Dem Benutzer, der eine Anwendung erstellt, wird automatisch die Rolle **Administrator** zugewiesen. Es muss immer mindestens ein Benutzer mit der Rolle **Administrator** vorhanden sein.

### <a name="builder"></a>Generator

Benutzer mit der Rolle **Ersteller** können alle Aspekte der App verwalten, sie können jedoch keine Änderungen auf den Registerkarten „Verwaltung“ und „Kontinuierlicher Datenexport“ vornehmen.

### <a name="operator"></a>Operator

Benutzer mit der Rolle **Operator** können die Integrität und den Status des Geräts überwachen. Sie dürfen keine Änderungen an Gerätevorlagen vornehmen und die Anwendung nicht verwalten. Anwendungsoperatoren können Geräte hinzufügen und löschen, Gerätesätze verwalten sowie Analysen und Aufträge ausführen. 

## <a name="create-a-custom-role"></a>Erstellen einer benutzerdefinierten Rolle

Wenn Sie für Ihre Lösung eine präzisere Zugriffssteuerung benötigen, können Sie benutzerdefinierte Rollen mit benutzerdefinierten Berechtigungen erstellen. Um eine benutzerdefinierte Rolle zu erstellen, navigieren Sie im Abschnitt **Verwaltung** Ihrer Anwendung zur Seite **Rollen**. Wählen Sie dann **+ Neue Rolle** aus, und fügen Sie einen Namen und eine Beschreibung für die Rolle hinzu. Wählen Sie die Berechtigungen aus, die für die Rolle erforderlich sind, und wählen Sie dann **Speichern** aus.

Sie können Ihrer benutzerdefinierten Rolle Benutzer auf die gleiche Weise hinzufügen wie bei einer integrierten Rolle.

> [!div class="mx-imgBorder"]
> ![Erstellen einer benutzerdefinierten Rolle](media/howto-manage-users-roles-pnp/create-custom-role-pnp.png)

### <a name="custom-role-options"></a>Optionen für benutzerdefinierte Rollen

Wenn Sie eine benutzerdefinierte Rolle definieren, wählen Sie den Berechtigungssatz aus, der einem Benutzer gewährt wird, wenn er Mitglied der Rolle ist. Einige Berechtigungen sind von anderen abhängig. Wenn Sie z. B. einer Rolle die Berechtigung **Anwendungsdashboards aktualisieren** hinzufügen, wird automatisch auch die Berechtigung **Anwendungsdashboards anzeigen** hinzugefügt. In den folgenden Tabellen werden die verfügbaren Berechtigungen und ihre Abhängigkeiten zusammengefasst, die Sie beim Erstellen von benutzerdefinierten Rollen verwenden können.

#### <a name="managing-devices"></a>Verwalten von Geräten

**Berechtigungen für Gerätevorlagen**

| NAME | Abhängigkeiten |
| ---- | -------- |
| Sicht | Keine     |
| Verwalten | Sicht <br/> Andere Abhängigkeiten: Geräteinstanzen anzeigen  |
| Vollzugriff | Anzeigen, verwalten <br/> Andere Abhängigkeiten: Geräteinstanzen anzeigen |

**Berechtigungen für Geräteinstanzen**

| NAME | Abhängigkeiten |
| ---- | -------- |
| Sicht | Keine <br/> Andere Abhängigkeiten: Gerätevorlagen und Gerätegruppen anzeigen |
| Aktualisieren | Sicht <br/> Andere Abhängigkeiten: Gerätevorlagen und Gerätegruppen anzeigen  |
| Erstellen | Sicht <br/> Andere Abhängigkeiten:  Gerätevorlagen und Gerätegruppen anzeigen  |
| Löschen | Sicht <br/> Andere Abhängigkeiten: Gerätevorlagen und Gerätegruppen anzeigen  |
| Befehle ausführen | Aktualisieren, anzeigen <br/> Andere Abhängigkeiten: Gerätevorlagen und Gerätegruppen anzeigen  |
| Vollzugriff | Anzeigen, aktualisieren, erstellen, löschen, Befehle ausführen <br/> Andere Abhängigkeiten: Gerätevorlagen und Gerätegruppen anzeigen  |

**Berechtigungen für Gerätegruppen**

| NAME | Abhängigkeiten |
| ---- | -------- |
| Sicht | Keine <br/> Andere Abhängigkeiten: Gerätevorlagen und Geräteinstanzen anzeigen |
| Aktualisieren | Sicht <br/> Andere Abhängigkeiten: Gerätevorlagen und Geräteinstanzen anzeigen   |
| Erstellen | Anzeigen, aktualisieren <br/> Andere Abhängigkeiten:  Gerätevorlagen und Geräteinstanzen anzeigen   |
| Löschen | Sicht <br/> Andere Abhängigkeiten:  Gerätevorlagen und Geräteinstanzen anzeigen   |
| Vollzugriff | Anzeigen, aktualisieren, erstellen, löschen <br/> Andere Abhängigkeiten: Gerätevorlagen und Geräteinstanzen anzeigen |

**Berechtigungen für die Gerätekonnektivitätsverwaltung**

| NAME | Abhängigkeiten |
| ---- | -------- |
| Instanz lesen | Keine <br/> Andere Abhängigkeiten: Gerätevorlagen, Gerätegruppen und Geräteinstanzen anzeigen |
| Instanz verwalten | Keine |
| Global lesen | Keine   |
| Global verwalten | Global lesen |
| Vollzugriff | Instanz lesen, Instanz verwalten, global lesen, global verwalten <br/> Andere Abhängigkeiten: Gerätevorlagen, Gerätegruppen und Geräteinstanzen anzeigen |

**Berechtigungen für Aufträge**

| NAME | Abhängigkeiten |
| ---- | -------- |
| Sicht | Keine <br/> Andere Abhängigkeiten: Gerätevorlagen, Geräteinstanzen und Gerätegruppen anzeigen |
| Aktualisieren | Sicht <br/> Andere Abhängigkeiten: Gerätevorlagen, Geräteinstanzen und Gerätegruppen anzeigen |
| Erstellen | Anzeigen, aktualisieren <br/> Andere Abhängigkeiten:  Gerätevorlagen, Geräteinstanzen und Gerätegruppen anzeigen |
| Löschen | Sicht <br/> Andere Abhängigkeiten:  Gerätevorlagen, Geräteinstanzen und Gerätegruppen anzeigen |
| Ausführen | Sicht <br/> Andere Abhängigkeiten: Gerätevorlagen, Geräteinstanzen und Gerätegruppen anzeigen; Geräteinstanzen aktualisieren; Befehle auf Geräteinstanzen ausführen |
| Vollzugriff | Anzeigen, aktualisieren, erstellen, löschen, ausführen <br/> Andere Abhängigkeiten:  Gerätevorlagen, Geräteinstanzen und Gerätegruppen anzeigen; Geräteinstanzen aktualisieren; Befehle auf Geräteinstanzen ausführen |

**Berechtigungen für Regeln**

| NAME | Abhängigkeiten |
| ---- | -------- |
| Sicht | Keine <br/> Andere Abhängigkeiten: Gerätevorlagen anzeigen |
| Aktualisieren | Sicht <br/> Andere Abhängigkeiten: Gerätevorlagen anzeigen |
| Erstellen | Anzeigen, aktualisieren <br/> Andere Abhängigkeiten:  Gerätevorlagen anzeigen |
| Löschen | Sicht <br/> Andere Abhängigkeiten: Gerätevorlagen anzeigen |
| Vollzugriff | Anzeigen, aktualisieren, erstellen, löschen <br/> Andere Abhängigkeiten: Gerätevorlagen anzeigen |

#### <a name="managing-the-app"></a>App verwalten

**Berechtigungen für Anwendungseinstellungen**

| NAME | Abhängigkeiten |
| ---- | -------- |
| Sicht | Keine     |
| Aktualisieren | Sicht   |
| Kopieren | Sicht <br/> Andere Abhängigkeiten: Gerätevorlagen, Geräteinstanzen, Gerätegruppen, Dashboards, Datenexporte, Branding, Hilfelinks, benutzerdefinierte Rollen, Regeln anzeigen |
| Löschen | Sicht   |
| Vollzugriff | Anzeigen, aktualisieren, kopieren, löschen <br/> Andere Abhängigkeiten: Gerätevorlagen, Gerätegruppen, Anwendungsdashboards, Datenexporte, Branding, Hilfelinks, benutzerdefinierte Rollen, Regeln anzeigen |

**Berechtigungen für den Export von Anwendungsvorlagen**

| NAME | Abhängigkeiten |
| ---- | -------- |
| Sicht | Keine     |
| Export | Sicht <br/> Andere Abhängigkeiten:  Gerätevorlagen, Geräteinstanzen, Gerätegruppen, Dashboards, Datenexporte, Branding, Hilfelinks, benutzerdefinierte Rollen, Regeln anzeigen |
| Vollzugriff | Anzeigen, exportieren <br/> Andere Abhängigkeiten:  Gerätevorlagen, Gerätegruppen, Anwendungsdashboards, Datenexporte, Branding, Hilfelinks, benutzerdefinierte Rollen, Regeln anzeigen |

**Berechtigungen für die Abrechnung**

| NAME | Abhängigkeiten |
| ---- | -------- |
| Verwalten | Keine     |
| Vollzugriff | Verwalten |

#### <a name="managing-users-and-roles"></a>Benutzer und Schlüssel verwalten

**Berechtigungen für benutzerdefinierte Rollen**

| NAME | Abhängigkeiten |
| ---- | -------- |
| Sicht | Keine |
| Aktualisieren | Sicht |
| Erstellen | Anzeigen, aktualisieren |
| Löschen | Sicht |
| Vollzugriff | Anzeigen, aktualisieren, erstellen, löschen |

**Berechtigungen für die Benutzerverwaltung**

| NAME | Abhängigkeiten |
| ---- | -------- |
| Sicht | Keine <br/> Andere Abhängigkeiten: Benutzerdefinierte Rollen anzeigen |
| Hinzufügen | Sicht <br/> Andere Abhängigkeiten:  Benutzerdefinierte Rollen anzeigen |
| Löschen | Sicht <br/> Andere Abhängigkeiten:  Benutzerdefinierte Rollen anzeigen |
| Vollzugriff | Anzeigen, hinzufügen, löschen <br/> Andere Abhängigkeiten:  Benutzerdefinierte Rollen anzeigen |

> [!NOTE]
> Ein Benutzer mit einer benutzerdefinierten Rolle, die ihm die Berechtigung zum Hinzufügen anderer Benutzer gewährt, kann nur Benutzer einer Rolle mit denselben oder niedrigeren Berechtigungen wie die eigene Rolle hinzufügen.

#### <a name="customizing-the-app"></a>App anpassen

**Berechtigungen für Anwendungsdashboards**

| NAME | Abhängigkeiten |
| ---- | -------- |
| Sicht | Keine     |
| Aktualisieren | Sicht   |
| Erstellen | Anzeigen, aktualisieren |
| Löschen | Sicht   |
| Vollzugriff | Anzeigen, aktualisieren, erstellen, löschen |

**Berechtigungen für persönliche Dashboards**

| NAME | Abhängigkeiten |
| ---- | -------- |
| Sicht | Keine     |
| Aktualisieren | Sicht   |
| Erstellen | Anzeigen, aktualisieren   |
| Löschen | Sicht   |
| Vollzugriff | Anzeigen, aktualisieren, erstellen, löschen |

**Berechtigungen für Branding, Favicons und Farben**

| NAME | Abhängigkeiten |
| ---- | -------- |
| Sicht | Keine     |
| Aktualisieren | Sicht   |
| Vollzugriff | Anzeigen, aktualisieren |

**Berechtigungen für Hilfelinks**

| NAME | Abhängigkeiten |
| ---- | -------- |
| Sicht | Keine     |
| Aktualisieren | Sicht   |
| Vollzugriff | Anzeigen, aktualisieren |

#### <a name="extending-the-app"></a>App erweitern

**Berechtigungen für Datenexporte**

| NAME | Abhängigkeiten |
| ---- | -------- |
| Sicht | Keine     |
| Aktualisieren | Sicht   |
| Erstellen | Anzeigen, aktualisieren  |
| Löschen | Sicht   |
| Vollzugriff | Anzeigen, aktualisieren, erstellen, löschen |

**Berechtigungen für API-Token**

| NAME | Abhängigkeiten |
| ---- | -------- |
| Sicht | Keine     |
| Erstellen | Sicht   |
| Löschen | Sicht   |
| Vollzugriff | Anzeigen, erstellen, löschen |

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun mehr über die Verwaltung von Benutzern und Rollen in Ihrer Azure IoT Central-Anwendung erfahren haben, wird im nächsten Schritt empfohlen, sich mit dem [Verwalten Ihrer Rechnung](howto-view-bill-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) vertraut zu machen.

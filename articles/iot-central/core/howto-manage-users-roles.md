---
title: Verwalten von Benutzern und Rollen in Ihrer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie als Administrator Benutzer und Rollen in Ihrer Azure IoT Central-Anwendung verwalten.
author: v-krghan
ms.author: v-krghan
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 937c4a4dbf976564cbf8dc562bdec3b328fc2bc0
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2019
ms.locfileid: "72941455"
---
# <a name="manage-users-and-roles-in-your-iot-central-application"></a>Verwalten von Benutzern und Rollen in Ihrer IoT Central-Anwendung

In diesem Artikel wird beschrieben, wie Sie als Administrator Benutzer in Ihrer Azure IoT Central-Anwendung hinzufügen, bearbeiten und löschen und wie Sie die Rollen in ihrer Azure IoT Central-Anwendung verwalten können.

Um auf den Abschnitt **Verwaltung** zugreifen und diesen verwenden zu können, muss Ihnen die Rolle **Administrator** für eine Azure IoT Central-Anwendung zugewiesen sein. Wenn Sie eine Azure IoT Central-Anwendung erstellen, wird Ihnen automatisch die Rolle **Administrator** für die jeweilige Anwendung zugewiesen.


## <a name="add-users"></a>Hinzufügen von Benutzern

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

> [!NOTE]
> Die zugewiesenen Rollen gelten nur für die IoT Central-Anwendung und können nicht über das Azure-Portal verwaltet werden.

## <a name="delete-users"></a>Löschen von Benutzern

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

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun mehr über die Verwaltung von Benutzern und Rollen in Azure IoT Central erfahren haben, wird im nächsten Schritt empfohlen, sich mit dem [Anzeigen Ihrer Rechnung](howto-view-bill.md) in Azure IoT Central vertraut zu machen.

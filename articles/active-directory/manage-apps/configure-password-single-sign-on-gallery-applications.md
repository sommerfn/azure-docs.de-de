---
title: Konfigurieren des einmaligen Anmeldens per Kennwort für eine Azure AD-Kataloganwendung | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine Anwendung für das sichere einmalige Anmelden mit Kennwort konfigurieren, wenn die Anwendung bereits im Azure AD-Anwendungskatalog aufgeführt ist.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.collection: M365-identity-device-management
ROBOTS: NOINDEX
ms.openlocfilehash: 9fb33c4110a590539c85364885da9a27853f6bd0
ms.sourcegitcommit: aaa82f3797d548c324f375b5aad5d54cb03c7288
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2019
ms.locfileid: "70146884"
---
# <a name="configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Konfigurieren des einmaligen Anmeldens per Kennwort für eine Azure AD-Kataloganwendung

Wenn Sie eine Anwendung aus dem [Azure AD-Anwendungskatalog (Azure Active Directory)](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) hinzufügen, können Sie auswählen, wie Ihre Benutzer sich an dieser Anwendung anmelden sollen. Sie können diese Auswahl jederzeit konfigurieren, indem Sie im [Azure-Portal](https://portal.azure.com/) die Option **Einmaliges Anmelden** für eine Unternehmensanwendung auswählen.

Eine der verfügbaren Optionen für einmaliges Anmelden (Single Sign-On, SSO) ist das [kennwortbasierte einmalige Anmelden](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis). Dies ist eine gute Möglichkeit, um schnell mit dem Integrieren von Anwendungen in Azure AD zu beginnen. Dies ermöglicht Folgendes:

-   Sicheres Speichern und Wiedergeben von Benutzernamen und Kennwörtern für die in Azure AD integrierte Anwendung

-   Sicherstellen der Unterstützung für Anwendungen, für die über die Felder für Benutzername und Kennwort hinaus mehrere weitere Felder für die Anmeldung erforderlich sind

-   Anpassen der Bezeichnungen der Felder für Benutzername und Kennwort, die für Ihre Benutzer beim Eingeben ihrer Anmeldeinformationen im [Anwendungszugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) angezeigt werden

-   Ermöglichen einer Vorgehensweise für Benutzer, bei der für vorhandene Anwendungskonten eigene Benutzernamen und Kennwörter festgelegt werden können, die manuell im [Anwendungszugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) eingegeben werden

-   Ermöglichen der Verwendung des Features [Self-Service-Anwendungszugriff](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) für ein Mitglied der Gruppe „Business“, um die einem Benutzer zugewiesenen Benutzernamen und Kennwörter anzugeben

-   Ermöglichen der Angabe der Benutzernamen und Kennwörter, die einem Benutzer zugewiesen sind, durch einen Administrator, wenn dieser beim Zuweisen eines Benutzers zu einer Anwendung das Feature zum [Aktualisieren von Anmeldeinformationen](#assign-a-user-to-an-application-directly) verwendet

-   Ermöglichen der Verwendung des Features zum Aktualisieren von Anmeldeinformationen durch einen Administrator, um beim [Zuweisen einer Gruppe zu einer Anwendung](#assign-an-application-to-a-group-directly) den gemeinsamen Benutzernamen und das zugehörige Kennwort anzugeben

Im folgenden Abschnitt wird beschrieben, wie Sie das [kennwortbasierte einmalige Anmelden](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) bei einer Anwendung aktivieren, die bereits im [Azure AD-Anwendungskatalog](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) vorhanden ist.

## <a name="overview-of-required-steps"></a>Übersicht über die erforderlichen Schritte
Sie müssen die folgenden Schritte ausführen, um eine Anwendung aus dem Azure AD-Katalog zu konfigurieren:

-   [Hinzufügen einer Anwendung aus dem Azure AD-Katalog](#add-an-application-from-the-azure-ad-gallery)

-   [Konfigurieren der Anwendung für das einmalige Anmelden per Kennwort](#configure-the-application-for-password-single-sign-on)

-   Zuweisen der Anwendung zu einem Benutzer oder einer Gruppe:

    -   [Direktes Zuweisen eines Benutzers zu einer Anwendung](#assign-a-user-to-an-application-directly)

    -   [Direktes Zuweisen einer Anwendung zu einer Gruppe](#assign-an-application-to-a-group-directly)

## <a name="add-an-application-from-the-azure-ad-gallery"></a>Hinzufügen einer Anwendung aus dem Azure AD-Katalog

Führen Sie die folgenden Schritte aus, um eine Anwendung aus dem Azure AD-Katalog hinzuzufügen:

1.  Öffnen Sie das [Azure-Portal](https://portal.azure.com), und melden Sie sich als **Globaler Administrator** oder **Co-Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie oben im Menü auf der linken Seite die Option **Alle Dienste** auswählen.

3.  Geben Sie im Suchfeld den Suchbegriff **Azure Active Directory** ein, und wählen Sie anschließend den Eintrag **Azure Active Directory** aus.

4.  Wählen Sie links im Azure AD-Menü die Option **Unternehmensanwendungen** aus.

5.  Wählen Sie im Bereich **Unternehmensanwendungen** oben rechts die Option **Hinzufügen** aus.

6.  Geben Sie im Abschnitt **Aus Katalog hinzufügen** im Feld **Namen eingeben** den Namen der Anwendung ein.

7.  Wählen Sie die Anwendung aus, die Sie für das einmalige Anmelden konfigurieren möchten.

8.  Bevor Sie die Anwendung hinzufügen, können Sie ihren Namen im Feld **Name** ändern.

9.  Wählen Sie **Hinzufügen** aus, um die Anwendung hinzuzufügen.

Nach kurzer Zeit wird der Konfigurationsbereich der Anwendung angezeigt.

## <a name="configure-the-application-for-password-single-sign-on"></a>Konfigurieren der Anwendung für das einmalige Anmelden per Kennwort

Um das einmalige Anmelden für eine Anwendung zu konfigurieren, führen Sie folgende Schritte aus:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als **Globaler Administrator** oder **Co-Administrator** an.

2. Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie oben im Menü auf der linken Seite die Option **Alle Dienste** auswählen.

3. Geben Sie im Suchfeld den Suchbegriff **Azure Active Directory** ein, und wählen Sie anschließend den Eintrag **Azure Active Directory** aus.

4. Wählen Sie links im Azure Active Directory-Menü die Option **Unternehmensanwendungen**.

5. Wählen Sie **Alle Anwendungen**, um eine Liste mit Ihren Anwendungen anzuzeigen.

   Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6. Wählen Sie die Anwendung aus, die Sie für das einmalige Anmelden konfigurieren möchten.

7. Wählen Sie nach dem Laden der Anwendung links im Anwendungsmenü die Option **Einmaliges Anmelden** aus.

8. Wählen Sie den Modus **Kennwortbasiertes Anmelden** aus.

9. [Weisen Sie der Anwendung Benutzer zu](#assign-a-user-to-an-application-directly).

10. Sie können auch Anmeldeinformationen im Namen von Benutzern angeben, indem Sie die Zeile eines Benutzers auswählen, die Option **Anmeldeinformationen aktualisieren** auswählen und dann den Benutzernamen und das Kennwort eingeben. Andernfalls werden Benutzer aufgefordert, beim Starten der Anwendung ihre Anmeldeinformationen einzugeben.

## <a name="assign-a-user-to-an-application-directly"></a>Direktes Zuweisen eines Benutzers zu einer Anwendung

Um einer Anwendung Benutzer direkt zuzuweisen, führen Sie folgende Schritte aus:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als **Globaler Administrator** an.

2. Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie oben im Menü auf der linken Seite die Option **Alle Dienste** auswählen.

3. Geben Sie im Suchfeld den Suchbegriff **Azure Active Directory** ein, und wählen Sie anschließend den Eintrag **Azure Active Directory** aus.

4. Wählen Sie links im Azure Active Directory-Menü die Option **Unternehmensanwendungen**.

5. Wählen Sie **Alle Anwendungen**, um eine Liste mit Ihren Anwendungen anzuzeigen.

   Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6. Wählen Sie die Anwendung aus, der Sie einen Benutzer zuweisen möchten.

7. Wählen Sie nach dem Laden der Anwendung links im Anwendungsmenü die Option **Benutzer und Gruppen** aus.

8. Wählen Sie oberhalb der Liste **Benutzer und Gruppen** die Schaltfläche **Hinzufügen** aus, um den Bereich **Zuweisung hinzufügen** zu öffnen.

9. Wählen Sie dann im Bereich **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

10. Geben Sie im Suchfeld **Nach Name oder E-Mail-Adresse suchen** den vollständigen Namen oder die E-Mail-Adresse des Benutzers ein.

11. Bewegen Sie den Mauszeiger in der Liste auf den Benutzer, und wählen Sie anschließend das Kontrollkästchen neben dem Profilfoto oder Logo des Benutzers aus, um das Element der Liste **Ausgewählt** hinzuzufügen.

12. Optional: Falls Sie mehrere Benutzer hinzufügen möchten, geben Sie im Suchfeld **Nach Name oder E-Mail-Adresse suchen** einen weiteren vollständigen Namen oder eine weitere E-Mail-Adresse ein, und aktivieren Sie dann das Kontrollkästchen für den Benutzer, um ihn der Liste **Ausgewählt** hinzuzufügen.

13. Wenn Sie alle gewünschten Benutzer ausgewählt haben, verwenden Sie die Schaltfläche **Auswählen**, um sie der Liste mit den Benutzern und Gruppen hinzuzufügen, die der Anwendung zugewiesen werden sollen.

14. Optional: Verwenden Sie im Bereich **Zuweisung hinzufügen** den Befehl **Rolle auswählen**, um eine Rolle auszuwählen, die den ausgewählten Benutzern zugewiesen werden soll.

15. Wählen Sie **Zuweisen** aus, um die Anwendung den ausgewählten Benutzern zuzuweisen.

## <a name="assign-an-application-to-a-group-directly"></a>Direktes Zuweisen einer Anwendung zu einer Gruppe

Um einer Anwendung Gruppen direkt zuzuweisen, führen Sie folgende Schritte aus:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als **Globaler Administrator** an.

2. Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie oben im Menü auf der linken Seite die Option **Alle Dienste** auswählen.

3. Geben Sie im Suchfeld den Suchbegriff **Azure Active Directory** ein, und wählen Sie anschließend den Eintrag **Azure Active Directory** aus.

4. Wählen Sie links im Azure AD-Menü die Option **Unternehmensanwendungen** aus.

5. Wählen Sie **Alle Anwendungen**, um eine Liste mit Ihren Anwendungen anzuzeigen.

   Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6. Wählen Sie die Anwendung aus, der Sie einen Benutzer zuweisen möchten.

7. Wählen Sie nach dem Laden der Anwendung links im Anwendungsmenü die Option **Benutzer und Gruppen** aus.

8. Wählen Sie oben in der Liste **Benutzer und Gruppen** die Schaltfläche **Hinzufügen** aus, um den Bereich **Zuweisung hinzufügen** zu öffnen.

9. Wählen Sie dann im Bereich **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

10. Geben Sie den vollständigen Gruppennamen der Gruppe, die Sie zuweisen möchten, im Suchfeld **Nach Name oder E-Mail-Adresse suchen** ein.

11. Bewegen Sie den Mauszeiger in der Liste auf **Gruppe**, und wählen Sie anschließend das Kontrollkästchen neben dem Profilfoto oder Logo der Gruppe aus, um sie der Liste **Ausgewählt** hinzuzufügen.

12. Optional: Gehen Sie wie folgt vor, wenn Sie mehr als eine Gruppe hinzufügen möchten: Geben Sie im Suchfeld **Nach Name oder E-Mail-Adresse suchen** einen weiteren vollständigen Gruppennamen ein, und aktivieren Sie anschließend das entsprechende Kontrollkästchen, um diese Gruppe der Liste **Ausgewählt** hinzuzufügen.

13. Wenn Sie alle gewünschten Gruppen ausgewählt haben, verwenden Sie die Schaltfläche **Auswählen**, um sie der Liste mit den Benutzern und Gruppen hinzuzufügen, die der Anwendung zugewiesen werden sollen.

14. Optional: Verwenden Sie im Bereich **Zuweisung hinzufügen** den Befehl **Rolle auswählen**, um eine Rolle auszuwählen, die den ausgewählten Gruppen zugewiesen werden soll.

15. Wählen Sie **Zuweisen** aus, um die Anwendung den ausgewählten Gruppen zuzuweisen.

Nach kurzer Zeit sollten die ausgewählten Benutzer diese Anwendungen über den Zugriffsbereich starten können.

## <a name="next-steps"></a>Nächste Schritte
[Bereitstellen von einmaligem Anmelden bei Ihren Apps mit dem Anwendungsproxy](application-proxy-configure-single-sign-on-with-kcd.md)

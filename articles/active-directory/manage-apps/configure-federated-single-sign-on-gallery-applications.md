---
title: Konfigurieren der einmaligen Verbundanmeldung für eine Azure AD-Kataloganwendung | Microsoft-Dokumentation
description: Konfigurieren der einmaligen Verbundanmeldung für eine vorhandene Azure AD-Kataloganwendung und Verwenden von Tutorials für einen schnellen Einstieg
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
ms.openlocfilehash: 8e50a495e1b0406e0c935ac31111dc6b5d0c0821
ms.sourcegitcommit: 5f67772dac6a402bbaa8eb261f653a34b8672c3a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2019
ms.locfileid: "70207117"
---
# <a name="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Konfigurieren von einmaligem Anmelden im Verbund für eine Azure AD-Kataloganwendung

Für alle im Azure Active Directory-Katalog (Azure AD-Katalog) enthaltenen Anwendungen mit Enterprise Single Sign-On ist ein Schritt-für-Schritt-Tutorial verfügbar. In der [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) finden Sie ausführliche schrittweise Anweisungen.

## <a name="overview-of-steps-required"></a>Übersicht über die erforderlichen Schritte
Um eine Anwendung aus dem Azure AD-Katalog zu konfigurieren, müssen folgende Schritte ausgeführt werden:

-   [Hinzufügen einer Anwendung aus dem Azure AD-Katalog](#add-an-application-from-the-azure-ad-gallery)

-   [Konfigurieren der Metadatenwerte für die Anwendung in Azure AD (Anmelde-URL, ID, Antwort-URL)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Auswählen der Benutzer-ID und Hinzufügen von Benutzerattributen, die an die Anwendung gesendet werden](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Abrufen der Azure AD-Metadaten und des Zertifikats](#download-the-azure-ad-metadata-or-certificate)

-   [Konfigurieren von Azure AD-Metadatenwerten in der Anwendung (Anmelde-URL, Aussteller, Abmelde-URL und Zertifikat)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Zuweisen von Benutzern zur Anwendung](#assign-users-to-the-application)

## <a name="add-an-application-from-the-azure-ad-gallery"></a>Hinzufügen einer Anwendung aus dem Azure AD-Katalog

Um eine Anwendung aus dem Azure AD-Katalog hinzuzufügen, führen Sie folgende Schritte aus:

1.  Öffnen Sie das [Azure-Portal](https://portal.azure.com), und melden Sie sich als **Globaler Administrator** oder **Co-Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie oben im Hauptnavigationsmenü auf der linken Seite die Option **Alle Dienste** auswählen.

3.  Geben Sie im Suchfeld den Namen „Azure Active Directory“ ein, und wählen Sie **Azure Active Directory** aus.

4.  Wählen Sie im linken Azure AD-Navigationsmenü die Option **Unternehmensanwendungen** aus.

5.  Wählen Sie in der oberen rechten Ecke des Bereichs **Unternehmensanwendungen** die Option **Hinzufügen** aus.

6.  Geben Sie im Abschnitt **Aus Katalog hinzufügen** im Feld **Namen eingeben** den Namen der Anwendung ein.

7.  Wählen Sie die Anwendung aus, die Sie für das einmalige Anmelden konfigurieren möchten.

8.  Bevor Sie die Anwendung hinzufügen, können Sie den Namen der Anwendung im Feld **Name** ändern.

9.  Wählen Sie **Hinzufügen** aus, um die Anwendung hinzuzufügen.

Nach kurzer Zeit wird der Konfigurationsbereich der Anwendung angezeigt.

## <a name="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery"></a>Konfigurieren von einmaligem Anmelden für eine Anwendung aus dem Azure AD-Katalog

Um das einmalige Anmelden für eine Anwendung zu konfigurieren, führen Sie folgende Schritte aus:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/), und melden Sie sich als **Globaler Administrator** oder **Co-Administrator** an.

2. Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie oben im Hauptnavigationsmenü auf der linken Seite die Option **Alle Dienste** auswählen.

3. Geben Sie im Suchfeld den Namen „Azure Active Directory“ ein, und wählen Sie **Azure Active Directory** aus.

4. Wählen Sie im linken Azure Active Directory-Navigationsmenü die Option **Unternehmensanwendungen** aus.

5. Wählen Sie **Alle Anwendungen**, um eine Liste mit Ihren Anwendungen anzuzeigen.

   * Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6. Wählen Sie die Anwendung aus, für die Sie das einmalige Anmelden konfigurieren möchten.

7. Nachdem die Anwendung geladen wurde, wählen Sie im linken Navigationsmenü der Anwendung die Option **Einmaliges Anmelden** aus.

8. Wählen Sie **SAML-basierte Anmeldung** im Dropdownmenü **Modus** aus.

9. Geben Sie unter **Domäne und URLs** die erforderlichen Werte ein. Sie sollten diese Werte vom Hersteller der Anwendung erhalten.

   1. Zum Konfigurieren der Anwendung als SP-initiierte SSO ist die Anmelde-URL ein erforderlicher Wert. Bei einigen Anwendungen ist auch der Bezeichner ein erforderlicher Wert.

   2. Zum Konfigurieren der Anwendung als IdP-initiierte SSO ist die Antwort-URL ein erforderlicher Wert. Bei einigen Anwendungen ist auch der Bezeichner ein erforderlicher Wert.

10. **Optional:** Wählen Sie **Erweiterte URL-Einstellungen anzeigen** aus, wenn die nicht erforderlichen Werte angezeigt werden sollen.

11. Wählen Sie unter **Benutzerattribute** in der Dropdownliste **Benutzer-ID** den eindeutigen Bezeichner für die Benutzer aus.

12. **Optional:** Wählen Sie **Alle weiteren Benutzerattribute anzeigen und bearbeiten** aus, um die Attribute zu bearbeiten, die bei der Anmeldung durch die Benutzer im SAML-Token an die Anwendung gesendet werden sollen.

    So fügen Sie ein Attribut hinzu
   
    1. Klicken Sie auf **Attribut hinzufügen**. Geben Sie einen **Namen** ein, und wählen Sie in der Dropdownliste einen **Wert** aus.

    1. Wählen Sie **Speichern** aus. Das neue Attribut wird in der Tabelle angezeigt.

13. Wählen Sie **&lt;Anwendungsname&gt; konfigurieren** aus, um auf die Dokumentation zum Konfigurieren des einmaligen Anmeldens in der Anwendung zuzugreifen. Außerdem verfügen Sie über die erforderlichen Metadaten-URLs und das Zertifikat zum Einrichten des einmaligen Anmeldens mit der Anwendung.

14. Wählen Sie zum Speichern der Konfiguration **Speichern** aus.

15. Weisen Sie der Anwendung Benutzer zu.

## <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Auswählen der Benutzer-ID und Hinzufügen von Benutzerattributen, die an die Anwendung gesendet werden

Führen Sie die folgenden Schritte aus, um die Benutzer-ID auszuwählen oder Benutzerattribute hinzuzufügen:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/), und melden Sie sich als **Globaler Administrator** oder **Co-Admin** an.

2. Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie oben im Hauptnavigationsmenü auf der linken Seite die Option **Alle Dienste** auswählen.

3. Geben Sie im Suchfeld den Namen „Azure Active Directory“ ein, und wählen Sie **Azure Active Directory** aus.

4. Wählen Sie im linken Azure Active Directory-Navigationsmenü die Option **Unternehmensanwendungen** aus.

5. Wählen Sie **Alle Anwendungen**, um eine Liste mit Ihren Anwendungen anzuzeigen.

   * Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6. Wählen Sie die Anwendung aus, für die Sie das einmalige Anmelden konfiguriert haben.

7. Nachdem die Anwendung geladen wurde, wählen Sie im linken Navigationsmenü der Anwendung die Option **Einmaliges Anmelden** aus.

8. Wählen Sie im Abschnitt **Benutzerattribute** in der Dropdownliste **Benutzer-ID** den eindeutigen Bezeichner für die Benutzer aus. Die ausgewählte Option muss mit dem erwarteten Wert in der Anwendung übereinstimmen, um den Benutzer zu authentifizieren.

   >[!NOTE] 
   >Azure AD wählt das Format für das „NameID“-Attribut (die Benutzer-ID) auf Grundlage des ausgewählten Werts oder des von der Anwendung in der SAML-Authentifizierungsanforderung angeforderten Formats aus. Weitere Informationen finden Sie unter [SAML-Protokoll für einmaliges Anmelden](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) im Abschnitt „NameIDPolicy“.
   >
   >

9. Wählen Sie zum Hinzufügen von Benutzerattributen die Option **Alle weiteren Benutzerattribute anzeigen und bearbeiten** aus, um die Attribute zu bearbeiten, die bei der Anmeldung durch die Benutzer im SAML-Token an die Anwendung gesendet werden sollen.

   So fügen Sie ein Attribut hinzu
  
   1. Klicken Sie auf **Attribut hinzufügen**. Geben Sie einen **Namen** ein, und wählen Sie in der Dropdownliste einen **Wert** aus.

   2. Wählen Sie **Speichern** aus. Das neue Attribut wird in der Tabelle angezeigt.

## <a name="download-the-azure-ad-metadata-or-certificate"></a>Herunterladen der Azure AD-Metadaten oder des Zertifikats

Führen Sie zum Herunterladen der Metadaten oder des Zertifikats für die Anwendung aus Azure AD die folgenden Schritte aus:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/), und melden Sie sich als **Globaler Administrator** oder **Co-Admin** an.

2. Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie oben im Hauptnavigationsmenü auf der linken Seite die Option **Alle Dienste** auswählen.

3. Geben Sie im Suchfeld den Namen „Azure Active Directory“ ein, und wählen Sie **Azure Active Directory** aus.

4. Wählen Sie im linken Azure Active Directory-Navigationsmenü die Option **Unternehmensanwendungen** aus.

5. Wählen Sie **Alle Anwendungen**, um eine Liste mit Ihren Anwendungen anzuzeigen.

   *  Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6. Wählen Sie die Anwendung aus, für die Sie das einmalige Anmelden konfiguriert haben.

7. Nachdem die Anwendung geladen wurde, wählen Sie im linken Navigationsmenü der Anwendung die Option **Einmaliges Anmelden** aus.

8. Wechseln Sie zum Abschnitt **SAML-Signaturzertifikat**, und wählen Sie dann den Spaltenwert **Herunterladen** aus. Abhängig davon, welche Werte die Anwendung für die Konfiguration des einmaligen Anmeldens benötigt, wird die Option zum Herunterladen des Metadaten-XML oder des Zertifikats angezeigt.

Azure AD stellt auch eine URL für den Zugriff auf die Metadaten bereit. Verwenden Sie das folgende Modell, um die für die Anwendung spezifische Metadaten-URL abzurufen: `https://login.microsoftonline.com/<Directory ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<Application ID>`

## <a name="assign-users-to-the-application"></a>Zuweisen von Benutzern zur Anwendung

Um einer Anwendung Benutzer direkt zuzuweisen, führen Sie folgende Schritte aus:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/), und melden Sie sich als **Globaler Administrator** an.

2. Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie oben im Hauptnavigationsmenü auf der linken Seite die Option **Alle Dienste** auswählen.

3. Geben Sie im Suchfeld den Namen „Azure Active Directory“ ein, und wählen Sie **Azure Active Directory** aus.

4. Wählen Sie im linken Azure Active Directory-Navigationsmenü die Option **Unternehmensanwendungen** aus.

5. Wählen Sie **Alle Anwendungen**, um eine Liste mit Ihren Anwendungen anzuzeigen.

   * Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6. Wählen Sie aus der Liste die Anwendung aus, der Sie einen Benutzer zuweisen möchten.

7. Nachdem die Anwendung geladen wurde, wählen Sie im linken Navigationsmenü der Anwendung die Option **Benutzer und Gruppen** aus.

8. Wählen Sie oberhalb der Liste **Benutzer und Gruppen** die Schaltfläche **Hinzufügen** aus, um den Bereich **Zuweisung hinzufügen** zu öffnen.

9. Wählen Sie im Bereich **Zuweisung hinzufügen** den Selektor **Benutzer und Gruppen** aus.

10. Geben Sie im Suchfeld **Nach Name oder E-Mail-Adresse suchen** den **vollständigen Namen** oder die **E-Mail-Adresse** des Benutzers ein, der zugewiesen werden soll.

11. Zeigen Sie mit der Maus auf den **Benutzer** in der Liste, um ein **Kontrollkästchen** anzuzeigen. Aktivieren Sie das Kontrollkästchen neben dem Profilbild oder Logo des Benutzers, um den Benutzer zur Liste **Ausgewählt** hinzuzufügen.

12. **Optional:** Wenn Sie **mehrere Benutzer hinzufügen** möchten, geben Sie im Suchfeld **Nach Name oder E-Mail-Adresse suchen** einen weiteren **vollständigen Namen** oder eine weitere **E-Mail-Adresse** ein, und aktivieren Sie dann das Kontrollkästchen, um diesen Benutzer zur Liste **Ausgewählt** hinzuzufügen.

13. Wählen Sie nach der Auswahl der gewünschten Benutzer die Schaltfläche **Auswählen** aus, um die Benutzer zur Liste der Benutzer und Gruppen hinzuzufügen, die der Anwendung zugewiesen werden sollen.

14. **Optional:** Wählen Sie im Bereich **Zuweisung hinzufügen** das Auswahlfeld **Rolle auswählen** aus, um eine Rolle auszuwählen, die den ausgewählten Benutzern zugewiesen werden soll.

15. Wählen Sie die Schaltfläche **Zuweisen** aus, um die Anwendung den ausgewählten Benutzern zuzuweisen.

Nach kurzer Zeit können die ausgewählten Benutzer diese Anwendungen mit den Verfahren starten, die im Abschnitt mit der Lösungsbeschreibung beschrieben wurden.

## <a name="customizing-the-saml-claims-sent-to-an-application"></a>Anpassen der SAML-Ansprüche, die an eine Anwendung gesendet werden

Informationen zum Anpassen der SAML-Attributsansprüche, die an Ihre Anwendung gesendet werden, finden Sie unter [Anspruchszuordnung in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping).

## <a name="next-steps"></a>Nächste Schritte
[Bereitstellen von einmaligem Anmelden bei Ihren Apps mit dem Anwendungsproxy](application-proxy-configure-single-sign-on-with-kcd.md)




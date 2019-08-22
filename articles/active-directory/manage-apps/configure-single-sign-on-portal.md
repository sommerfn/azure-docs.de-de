---
title: Konfigurieren des einmaligen Anmeldens – Azure Active Directory | Microsoft-Dokumentation
description: In diesem Tutorial wird das Azure-Portal verwendet, um SAML-basiertes einmaliges Anmelden (Single Sign-On, SSO) für eine Anwendung mit Azure Active Directory (Azure AD) zu konfigurieren.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: tutorial
ms.workload: identity
ms.date: 04/08/2019
ms.author: mimart
ms.reviewer: arvinh,luleon
ms.collection: M365-identity-device-management
ROBOTS: NOINDEX
ms.openlocfilehash: c5e8ed4a78fccce4f3a5c631a99a8729114e5722
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2019
ms.locfileid: "68422604"
---
# <a name="how-to-configure-saml-based-single-sign-on"></a>Konfigurieren des SAML-basierten einmaligen Anmeldens

Nachdem Sie Ihren Azure AD-Unternehmensanwendungen eine App hinzugefügt haben, konfigurieren Sie die Einstellungen für das einmalige Anmelden. In diesem Artikel erfahren Sie, wie Sie das SAML-basierte einmalige Anmelden für eine nicht im Katalog enthaltene App konfigurieren. 

> [!NOTE]
> Fügen Sie eine Katalog-App hinzu? Eine detaillierte Einrichtungsanleitung finden Sie in der [Liste mit den SaaS-App-Tutorials](../saas-apps/tutorial-list.md).

Wenn Sie einmaliges Anmelden für eine Anwendung konfigurieren möchten, die nicht aus dem Katalog stammt, *ohne Code zu schreiben*, müssen Sie über ein Abonnement oder Azure AD Premium verfügen, und die Anwendung muss SAML 2.0 unterstützen. Weitere Informationen zu Azure AD-Versionen finden Sie in der [Preisübersicht für Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="before-you-begin"></a>Voraussetzungen

- Falls die Anwendung Ihrem Azure AD-Mandanten noch nicht hinzugefügt wurde, lesen Sie [Hinzufügen einer Katalog-App zu Ihrer Azure AD-Organisation](add-gallery-app.md) bzw. [Add an unlisted (non-gallery) application to your Azure AD organization](add-non-gallery-app.md) (Hinzufügen einer nicht aufgeführten (nicht im Katalog enthaltenen) Anwendung zu Ihrer Azure AD-Organisation).
- Wenden Sie sich an Ihren Anwendungsanbieter, um die korrekten Informationen für die folgenden Einstellungen zu erhalten:

    | Einstellung für die grundlegende SAML-Konfiguration | Vom Dienstanbieter initiiert | Vom Identitätsanbieter initiiert | Beschreibung |
    |:--|:--|:--|:--|
    | Bezeichner (Entitäts-ID) | Für einige Apps erforderlich | Für einige Apps erforderlich | Er identifiziert eindeutig die Anwendung, für die einmaliges Anmelden konfiguriert wird. Azure AD sendet den Bezeichner als Audience-Parameter des SAML-Tokens an die Anwendung. Von der Anwendung wird erwartet, dass sie diesen Parameter überprüft. Dieser Wert ist auch als Entitäts-ID in SAML-Metadaten enthalten, die von der Anwendung bereitgestellt werden. *Sie finden diesen Wert als Element vom Typ **Issuer** (Aussteller) in der SAML-Anforderung **AuthnRequest**, die von der Anwendung gesendet wurde.* |
    | Antwort-URL | Optional | Erforderlich | Gibt an, ob die Anwendung den Empfang des SAML-Tokens erwartet. Die Antwort-URL wird auch als „Assertionsverbraucherdienst-URL“ (Assertion Consumer Service, ACS) bezeichnet. |
    | Anmelde-URL | Erforderlich | Nicht angeben | Wenn ein Benutzer diese URL öffnet, wird er vom Dienstanbieter zur Authentifizierung und Anmeldung an Azure AD umgeleitet. Azure AD verwendet die URL, um die Anwendung über Office 365 oder den Azure AD-Zugriffsbereich zu starten. Wird keine URL angegeben, verwendet Azure AD den Identitätsanbieter, um einmaliges Anmelden zu aktivieren, wenn ein Benutzer die Anwendung startet.|
    | Relayzustand | Optional | Optional | Mit dieser Option wird die Anwendung darüber informiert, wohin der Benutzer nach der Authentifizierung umgeleitet werden soll. In der Regel ist der Wert eine für die Anwendung gültige URL. Einige Anwendungen verwenden dieses Feld jedoch anders. Weitere Informationen erhalten Sie vom Anwendungshersteller.
    | Abmelde-URL | Optional | Optional | Wird verwendet, um die SAML-Abmeldeantworten an die Anwendung zurückzusenden.

## <a name="step-1-edit-the-basic-saml-configuration"></a>Schritt 1: Bearbeiten der grundlegenden SAML-Konfiguration

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Cloudanwendungsadministrator oder Anwendungsadministrator für Ihren Azure AD-Mandanten an.

1. Navigieren Sie zu **Azure Active Directory** > **Unternehmensanwendungen**, und wählen Sie in der Liste die Anwendung aus. 
   
   - Wählen Sie zum Suchen der Anwendung im Menü **Anwendungstyp** die Option **Alle Anwendungen** und anschließend **Übernehmen** aus. Geben Sie den Namen Ihrer Anwendung in das Suchfeld ein, und wählen Sie in den Ergebnissen Ihre Anwendung aus.

1. Wählen Sie im Abschnitt **Verwalten** die Option **Einmaliges Anmelden** aus. 

1. Wählen Sie **SAML** aus. Die Seite **Einmaliges Anmelden (SSO) mit SAML einrichten – Vorschau** wird angezeigt.

1. Um die grundlegenden SAML-Konfigurationsoptionen zu bearbeiten, klicken Sie rechts oben im Abschnitt **Grundlegende SAML-Konfiguration** auf das Symbol **Bearbeiten** (Stift).

     ![Konfigurieren von Zertifikaten](media/configure-single-sign-on-portal/basic-saml-configuration-edit-icon.png)

1. Geben Sie die im Abschnitt [Voraussetzungen](#before-you-begin) beschriebenen Informationen in die entsprechenden Felder ein.

1. Klicken Sie am oberen Rand der Seite auf **Speichern**.

## <a name="step-2-configure-user-attributes-and-claims"></a>Schritt 2: Konfigurieren von Benutzerattributen und Ansprüchen 

Von einer Anwendung werden möglicherweise bestimmte Benutzerattribute oder Ansprüche in dem SAML-Token erwartet, das sie bei der Benutzeranmeldung von Azure AD erhält. So können beispielsweise bestimmte Anspruchs-URIs oder Anspruchswerte erforderlich sein, oder **Name** darf nicht dem in Microsoft Identity Platform gespeicherten Benutzernamen entsprechen. Anforderungen für Katalog-Apps werden in den [anwendungsspezifischen Tutorials](../saas-apps/tutorial-list.md) beschrieben. Sie können sich aber auch an den Anwendungsanbieter wenden. Im Anschluss werden die allgemeinen Schritte zum Konfigurieren von Benutzerattributen und Ansprüchen beschrieben.

1. Wählen Sie rechts oben im Abschnitt **Benutzerattribute und Ansprüche** das Symbol **Bearbeiten** (Stiftsymbol) aus.

1. Überprüfen Sie die Angabe unter **Wert für Namensbezeichner**. Der Standardwert lautet *user.principalname*. Mit der Benutzer-ID wird jeder Benutzer in der Anwendung eindeutig identifiziert. Beispiel: Ist die E-Mail-Adresse sowohl der Benutzername als auch der eindeutige Bezeichner, legen Sie den Wert auf *user.mail* fest.

1. Um den **Wert für Namensbezeichner** zu ändern, klicken Sie für das Feld **Wert für Namensbezeichner** auf das Symbol **Bearbeiten** (Stift). Nehmen Sie bei Bedarf die entsprechenden Änderungen am Format und an der Quelle des Bezeichners vor. Ausführliche Informationen finden Sie unter [Bearbeiten der NameID](https://docs.microsoft.com/azure/active-directory//develop/active-directory-saml-claims-customization#editing-nameid). Speichern Sie die Änderungen, wenn Sie fertig sind. 
 
1. Wählen Sie zum Konfigurieren von Gruppenansprüchen das Symbol **Bearbeiten** für das Feld **Im Anspruch zurückgegebene Gruppen** aus. Ausführliche Informationen finden Sie unter [Konfigurieren von Gruppenansprüchen für Anwendungen mit Azure Active Directory (öffentliche Vorschau)](../hybrid/how-to-connect-fed-group-claims.md).

3. Klicken Sie zum Hinzufügen eines Anspruchs oben auf der Seite auf **Neuen Anspruch hinzufügen**. Geben Sie den **Namen** ein, und wählen Sie die entsprechende Quelle. Wenn Sie die Quelle **Attribut** auswählen, müssen Sie das **Quellattribut** auswählen, das Sie verwenden möchten. Wenn Sie die Quelle **Übersetzung** auswählen, müssen Sie die gewünschte **Transformation** und **Parameter 1** auswählen. Ausführliche Informationen finden Sie unter [Hinzufügen anwendungsspezifischer Ansprüche](https://docs.microsoft.com/azure/active-directory//develop/active-directory-saml-claims-customization#adding-application-specific-claims). Speichern Sie die Änderungen, wenn Sie fertig sind. 

4. Wählen Sie **Speichern** aus. Der neue Anspruch wird in der Tabelle angezeigt.

   > [!NOTE]
   > Weitere Möglichkeiten zum Anpassen des SAML-Tokens, das von Azure AD an Ihre Anwendung gesendet wird, finden Sie in den folgenden Ressourcen:
   >- Informationen zum Erstellen benutzerdefinierter Rollen über das Azure-Portal finden Sie unter [Gewusst wie: Konfigurieren von im SAML-Token ausgestellten Rollenansprüchen für Unternehmensanwendungen](../develop/active-directory-enterprise-app-role-management.md).
   >- Informationen zum Anpassen der Ansprüche über PowerShell finden Sie unter [Gewusst wie: Anpassen von in Token ausgegebenen Ansprüchen für eine bestimmte App in einem Mandanten (Vorschau)](../develop/active-directory-claims-mapping.md).
   >- Informationen zum Konfigurieren optionaler Ansprüche für Ihre Anwendung durch Anpassen des Anwendungsmanifests finden Sie unter [Gewusst wie: Bereitstellen optionaler Ansprüche für Ihre Azure AD-App](../develop/active-directory-optional-claims.md).
   >- Informationen zum Festlegen von Tokengültigkeitsdauer-Richtlinien für Aktualisierungstoken, Zugriffstoken, Sitzungstoken und ID-Token finden Sie unter [Konfigurierbare Tokengültigkeitsdauern in Azure Active Directory (Vorschau)](../develop/active-directory-configurable-token-lifetimes.md). Informationen zum Einschränken von Authentifizierungssitzungen mithilfe des bedingten Azure AD-Zugriffs finden Sie unter [Konfigurieren der Verwaltung von Authentifizierungssitzungen mit bedingtem Zugriff](https://go.microsoft.com/fwlink/?linkid=2083106).

## <a name="step-3-manage-the-saml-signing-certificate"></a>Schritt 3: Verwalten des SAML-Signaturzertifikats

Azure AD verwendet ein Zertifikat zum Signieren der SAML-Token, die an die Anwendung gesendet werden. Auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** können Sie das aktive Zertifikat anzeigen und herunterladen. Außerdem können Sie ein Zertifikat aktualisieren, erstellen oder importieren. Für Kataloganwendungen stehen Details zum Zertifikatformat in der SAML-Dokumentation der Anwendung zur Verfügung (siehe [anwendungsspezifische Tutorials](../saas-apps/tutorial-list.md)). 

1. Navigieren Sie zum Abschnitt **SAML-Signaturzertifikat**. Je nach Art der Anwendung werden Optionen zum Herunterladen des Zertifikats im Base64-Format, im RAW-Format oder als Verbundmetadaten-XML angezeigt. Azure AD bietet auch die **App-Verbundmetadaten-URL**, unter der Sie auf die anwendungsspezifischen Metadaten zugreifen können (im Format `https://login.microsoftonline.com/<Directory ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<Application ID>`).

1. Wenn Sie ein Zertifikat verwalten, erstellen oder importieren möchten, wählen Sie rechts oben im Abschnitt **SAML-Signaturzertifikat** das Symbol **Bearbeiten** (Stiftsymbol) aus, und gehen Sie anschließend wie folgt vor:

   - Wenn Sie ein neues Zertifikat erstellen möchten, wählen Sie **Neues Zertifikat**, unter **Ablaufdatum** das Ablaufdatum und anschließend **Speichern** aus. Wenn Sie das Zertifikat aktivieren möchten, wählen Sie das Kontextmenü ( **...** ) und anschließend **Zertifikat als aktiv festlegen** aus.
   - Wenn Sie ein Zertifikat mit privatem Schlüssel und PFX-Anmeldeinformationen hochladen möchten, wählen Sie **Zertifikat importieren** aus, und navigieren Sie zu dem Zertifikat. Geben Sie unter **PFX-Kennwort** das PFX-Kennwort ein, und wählen Sie anschließend **Speichern** aus.  
   - Wenn Sie erweiterte Optionen für die Zertifikatsignatur konfigurieren möchten, verwenden Sie die folgenden Optionen. Beschreibungen dieser Optionen finden Sie im Artikel unter [Erweiterte Optionen für die Zertifikatsignatur im SAML-Token für Katalog-Apps in Azure Active Directory](certificate-signing-options.md).
      - Wählen Sie in der Dropdownliste **Signaturoption** die Option **SAML-Anwort signieren**, **SAML-Assertion signieren** oder **SAML-Antwort und-Assertion signieren** aus.
      - Wählen Sie in der Dropdownliste **Signaturalgorithmus** die Option **SHA-1** oder **SHA-256** aus.
   - Wenn weitere Personen darüber informiert werden sollen, dass das Ablaufdatum des aktiven Zertifikats bald erreicht ist, geben Sie die entsprechenden E-Mail-Adressen in die Felder für die **Benachrichtigungs-E-Mail-Adressen**ein.

1. Klicken Sie oben im Abschnitt **SAML-Signaturzertifikat** auf **Speichern**. 

## <a name="step-4-set-up-the-application-to-use-azure-ad"></a>Schritt 4: Einrichten der Anwendung für die Verwendung von Azure AD

Im Abschnitt **\<Anwendungsname> einrichten** werden die Werte aufgeführt, die in der Anwendung konfiguriert werden müssen, damit sie Azure AD als SAML-Identitätsanbieter verwendet. Die erforderlichen Werte variieren je nach Anwendung. Einzelheiten finden Sie in der SAML-Dokumentation der Anwendung.

1. Scrollen Sie nach unten zum Abschnitt **\<applicationName> einrichten**. 
2. Kopieren Sie nach Bedarf den Wert aus der jeweiligen Zeile dieses Abschnitts, und fügen Sie ihn gemäß den anwendungsspezifischen Anweisungen der Anwendung hinzu. Bei Katalog-Apps können Sie die Dokumentation durch Auswählen von **Schrittanleitung anzeigen** anzeigen. 
   - Die Werte für **Anmelde-URL** und **Abmelde-URL** werden in den gleichen Endpunkt aufgelöst – hierbei handelt es sich um den Endpunkt, in dem die SAML-Anforderungen für Ihre Azure AD-Instanz verarbeitet werden. 
   - **Azure AD-Bezeichner** ist der Wert für den **Issuer** (Aussteller) in dem SAML-Token, das für die Anwendung ausgestellt wird.
1. Wenn Sie alle Werte in die entsprechenden Felder eingefügt haben, klicken Sie auf **Speichern**.

## <a name="step-5-validate-single-sign-on"></a>Schritt 5: Überprüfen des einmaligen Anmeldens

Sie können nun die Einstellungen testen, um zu ermitteln, ob das einmalige Anmelden für Sie, den Administrator, funktioniert.  

1. Öffnen Sie die SSO-Einstellungen für Ihre Anwendung. 
2. Scrollen Sie zum Abschnitt **Einmaliges Anmelden mit <applicationName> überprüfen**. In diesem Tutorial heißt dieser Abschnitt **GitHub-test einrichten**.
3. Klicken Sie auf **Test**. Die Testoptionen werden angezeigt.
4. Klicken Sie auf **Als aktueller Benutzer anmelden**. 

Ist die Anmeldung erfolgreich, können Sie Ihrer SAML-Anwendung Benutzer und Gruppen zuweisen.
Sollte eine Fehlermeldung angezeigt werden, gehen Sie wie folgt vor:

1. Kopieren Sie die Details, und fügen Sie sie ins Feld **Wie äußert sich der Fehler?** ein.

    ![Verwenden des Felds „Wie äußert sich der Fehler?“, um Hilfe zur Lösung zu erhalten](media/configure-single-sign-on-portal/error-guidance.png)

1. Klicken Sie auf **Leitfaden zur Problemlösung abrufen**. Die Grundursache und Informationen zur Problemlösung werden angezeigt.  In diesem Beispiel wurde der Benutzer nicht der Anwendung zugewiesen.
1. Lesen Sie den Leitfaden zur Problemlösung, und beheben Sie nach Möglichkeit das Problem.
1. Führen Sie den Test erneut aus, bis der Vorgang erfolgreich abgeschlossen wird.

## <a name="next-steps"></a>Nächste Schritte

- [Zuweisen von Benutzern und Gruppen zu einer Anwendung in Azure Active Directory](methods-for-assigning-users-and-groups.md)
- [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps im Azure-Portal](configure-automatic-user-provisioning-portal.md)

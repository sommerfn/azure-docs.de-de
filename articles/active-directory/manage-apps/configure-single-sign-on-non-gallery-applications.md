---
title: Einmaliges Anmelden über SAML – nicht im Katalog enthaltene Anwendungen – Microsoft Identity Platform | Microsoft-Dokumentation
description: Konfigurieren des einmaligen Anmeldens bei nicht im Katalog enthaltenen Anwendungen auf der Microsoft Identity Platform (Azure AD)
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: article
ms.workload: identity
ms.date: 07/19/2019
ms.author: celested
ms.reviewer: arvinh,luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: e993ad17a07ff741ff33073304ed774dcf30203d
ms.sourcegitcommit: 39da2d9675c3a2ac54ddc164da4568cf341ddecf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2019
ms.locfileid: "73961654"
---
# <a name="configure-saml-based-single-sign-on-to-non-gallery-applications"></a>Konfigurieren des SAML-basierten einmaligen Anmeldens bei nicht im Katalog aufgeführten Anwendungen

Wenn Sie Ihren Azure AD-Unternehmensanwendungen [eine Katalog-App](add-gallery-app.md) oder eine [nicht im Katalog enthaltene Web-App](add-non-gallery-app.md) hinzufügen, ist eine der SSO-Optionen (Single Sign-On, einmaliges Anmelden) das [SAML-basierte einmalige Anmelden](what-is-single-sign-on.md#saml-sso). Wählen Sie für Anwendungen, bei denen die Authentifizierung mit einem der SAML-Protokolle durchgeführt wird, nach Möglichkeit immer SAML aus. Mit SAML-SSO nimmt Azure AD die Authentifizierung bei der Anwendung mithilfe des Azure AD-Kontos des Benutzers vor. Azure AD gibt die Informationen für das einmalige Anmelden über ein Verbindungsprotokoll an die Anwendung weiter. Sie können Benutzer basierend auf Regeln, die Sie in Ihren SAML-Ansprüchen definieren, bestimmten Anwendungsrollen zuordnen. In diesem Artikel erfahren Sie, wie Sie das SAML-basierte einmalige Anmelden für eine nicht im Katalog enthaltene App konfigurieren. 

> [!NOTE]
> Fügen Sie eine Katalog-App hinzu? Eine detaillierte Einrichtungsanleitung finden Sie in der [Liste mit den SaaS-App-Tutorials](../saas-apps/tutorial-list.md).

Wenn Sie das einmalige Anmelden über SAML für eine nicht aus dem Katalog stammende Anwendung konfigurieren und dafür keinen Code schreiben möchten, müssen Sie über ein Abonnement oder Azure AD Premium verfügen, und die Anwendung muss SAML 2.0 unterstützen. Weitere Informationen zu Azure AD-Versionen finden Sie in der [Preisübersicht für Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="before-you-begin"></a>Voraussetzungen

Falls die Anwendung Ihrem Azure AD-Mandanten noch nicht hinzugefügt wurde, finden Sie weitere Informationen unter [Hinzufügen einer nicht im Katalog enthaltenen App](add-non-gallery-app.md).

## <a name="step-1-edit-the-basic-saml-configuration"></a>Schritt 1: Bearbeiten der grundlegenden SAML-Konfiguration

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Cloudanwendungsadministrator oder Anwendungsadministrator für Ihren Azure AD-Mandanten an.

2. Navigieren Sie zu **Azure Active Directory** > **Unternehmensanwendungen**, und wählen Sie in der Liste die Anwendung aus. 
   
   - Wählen Sie zum Suchen der Anwendung im Menü **Anwendungstyp** die Option **Alle Anwendungen** und anschließend **Übernehmen** aus. Geben Sie den Namen Ihrer Anwendung in das Suchfeld ein, und wählen Sie in den Ergebnissen Ihre Anwendung aus.

3. Wählen Sie im Abschnitt **Verwalten** die Option **Einmaliges Anmelden** aus. 

4. Wählen Sie **SAML** aus. Die Seite **Einmaliges Anmelden (SSO) mit SAML einrichten – Vorschau** wird angezeigt.

   ![Schritt 1: Bearbeiten der grundlegenden SAML-Konfiguration](media/configure-single-sign-on-non-gallery-applications/step-one-basic-saml-config.png)

5. Um die grundlegenden SAML-Konfigurationsoptionen zu bearbeiten, klicken Sie rechts oben im Abschnitt **Grundlegende SAML-Konfiguration** auf das Symbol **Bearbeiten** (Stift).

1. Geben Sie folgende Einstellungen ein. Sie sollten die Werte vom Hersteller der Anwendung erhalten. Sie können die Werte manuell eingeben oder eine Metadatendatei hochladen, um die Werte der Felder zu extrahieren.

    | Einstellung für die grundlegende SAML-Konfiguration | Vom Dienstanbieter initiiert | Vom Identitätsanbieter initiiert | BESCHREIBUNG |
    |:--|:--|:--|:--|
    | **Bezeichner (Entitäts-ID)** | Für einige Apps erforderlich | Für einige Apps erforderlich | Hiermit wird die Anwendung eindeutig identifiziert. Azure AD sendet den Bezeichner als Audience-Parameter des SAML-Tokens an die Anwendung. Von der Anwendung wird erwartet, dass sie diesen Parameter überprüft. Dieser Wert ist auch als Entitäts-ID in SAML-Metadaten enthalten, die von der Anwendung bereitgestellt werden. *Sie finden diesen Wert als Element vom Typ **Issuer** (Aussteller) in der SAML-Anforderung **AuthnRequest**, die von der Anwendung gesendet wurde.* |
    | **Antwort-URL** | Erforderlich | Erforderlich | Gibt an, ob die Anwendung den Empfang des SAML-Tokens erwartet. Die Antwort-URL wird auch als „Assertionsverbraucherdienst-URL“ (Assertion Consumer Service, ACS) bezeichnet. Sie können die zusätzlichen Antwort-URL-Felder verwenden, um mehrere Antwort-URLs anzugeben. Zusätzliche Antwort-URLs werden möglicherweise für mehrere Unterdomänen benötigt. Sie können auch zu Testzwecken mehrere Antwort-URLs gleichzeitig angeben (URL des lokalen Host und öffentliche URL). |
    | **Anmelde-URL** | Erforderlich | Nicht angeben | Wenn ein Benutzer diese URL öffnet, wird er vom Dienstanbieter zur Authentifizierung und Anmeldung an Azure AD umgeleitet. Azure AD verwendet die URL, um die Anwendung über Office 365 oder den Azure AD-Zugriffsbereich zu starten. Ist das Feld leer, führt Azure AD das vom Identitätsanbieter initiierte einmalige Anmelden aus, wenn ein Benutzer die Anwendung über Office 365, den Azure AD-Zugriffsbereich oder die Azure AD-SSO-URL startet.|
    | **Relayzustand** | Optional | Optional | Mit dieser Option wird die Anwendung darüber informiert, wohin der Benutzer nach der Authentifizierung umgeleitet werden soll. In der Regel ist der Wert eine für die Anwendung gültige URL. Einige Anwendungen verwenden dieses Feld jedoch anders. Weitere Informationen erhalten Sie vom Anwendungshersteller.
    | **Abmelde-URL** | Optional | Optional | Wird verwendet, um die SAML-Abmeldeantworten an die Anwendung zurückzusenden.

Weitere Informationen finden Sie unter [SAML-Protokoll für einmaliges Anmelden](../develop/single-sign-on-saml-protocol.md).

## <a name="step-2-configure-user-attributes-and-claims"></a>Schritt 2: Konfigurieren von Benutzerattributen und Ansprüchen 

Wenn sich ein Benutzer bei der Anwendung authentifiziert, stellt Azure AD der Anwendung ein SAML-Token aus, das Informationen (so genannte „Ansprüche“) über den Benutzer enthält, die ihn eindeutig identifizieren. Zu diesen Informationen gehören standardmäßig der Benutzername, die E-Mail-Adresse, der Vorname und der Nachname des Benutzers. Möglicherweise müssen Sie diese Ansprüche anpassen, wenn die Anwendung beispielsweise bestimmte Anspruchswerte oder für **Name** ein anderes Format als den Benutzernamen erfordert. Anforderungen für Katalog-Apps werden in den [anwendungsspezifischen Tutorials](../saas-apps/tutorial-list.md) beschrieben. Sie können sich aber auch an den Anwendungsanbieter wenden. Im Anschluss werden die allgemeinen Schritte zum Konfigurieren von Benutzerattributen und Ansprüchen beschrieben.

1. Wählen Sie rechts oben im Abschnitt **Benutzerattribute und Ansprüche** das Symbol **Bearbeiten** (Stiftsymbol) aus.

   ![Schritt 2: Konfigurieren von Benutzerattributen und Ansprüchen](media/configure-single-sign-on-non-gallery-applications/step-two-user-attributes-claims.png)

2. Überprüfen Sie die Angabe unter **Wert für Namensbezeichner**. Der Standardwert lautet *user.principalname*. Mit der Benutzer-ID wird jeder Benutzer in der Anwendung eindeutig identifiziert. Beispiel: Ist die E-Mail-Adresse sowohl der Benutzername als auch der eindeutige Bezeichner, legen Sie den Wert auf *user.mail* fest.

3. Um den **Wert für Namensbezeichner** zu ändern, klicken Sie für das Feld **Wert für Namensbezeichner** auf das Symbol **Bearbeiten** (Stift). Nehmen Sie bei Bedarf die entsprechenden Änderungen am Format und an der Quelle des Bezeichners vor. Ausführliche Informationen finden Sie unter [Bearbeiten der NameID](https://docs.microsoft.com/azure/active-directory//develop/active-directory-saml-claims-customization#editing-nameid). Speichern Sie die Änderungen, wenn Sie fertig sind. 
 
4. Wählen Sie zum Konfigurieren von Gruppenansprüchen das Symbol **Bearbeiten** für das Feld **Im Anspruch zurückgegebene Gruppen** aus. Ausführliche Informationen finden Sie unter [Konfigurieren von Gruppenansprüchen für Anwendungen mit Azure Active Directory (öffentliche Vorschau)](../hybrid/how-to-connect-fed-group-claims.md).

5. Klicken Sie zum Hinzufügen eines Anspruchs oben auf der Seite auf **Neuen Anspruch hinzufügen**. Geben Sie den **Namen** ein, und wählen Sie die entsprechende Quelle. Wenn Sie die Quelle **Attribut** auswählen, müssen Sie das **Quellattribut** auswählen, das Sie verwenden möchten. Wenn Sie die Quelle **Übersetzung** auswählen, müssen Sie die gewünschte **Transformation** und **Parameter 1** auswählen. Ausführliche Informationen finden Sie unter [Hinzufügen anwendungsspezifischer Ansprüche](https://docs.microsoft.com/azure/active-directory//develop/active-directory-saml-claims-customization#adding-application-specific-claims). Speichern Sie die Änderungen, wenn Sie fertig sind. 

6. Wählen Sie **Speichern** aus. Der neue Anspruch wird in der Tabelle angezeigt.

   > [!NOTE]
   > Weitere Möglichkeiten zum Anpassen des SAML-Tokens, das von Azure AD an Ihre Anwendung gesendet wird, finden Sie in den folgenden Ressourcen:
   >- Informationen zum Erstellen benutzerdefinierter Rollen über das Azure-Portal finden Sie unter [Gewusst wie: Konfigurieren von im SAML-Token ausgestellten Rollenansprüchen für Unternehmensanwendungen](../develop/active-directory-enterprise-app-role-management.md).
   >- Informationen zum Anpassen der Ansprüche über PowerShell finden Sie unter [Gewusst wie: Anpassen von in Token ausgegebenen Ansprüchen für eine bestimmte App in einem Mandanten (Vorschau)](../develop/active-directory-claims-mapping.md).
   >- Informationen zum Konfigurieren optionaler Ansprüche für Ihre Anwendung durch Anpassen des Anwendungsmanifests finden Sie unter [Gewusst wie: Bereitstellen optionaler Ansprüche für Ihre Azure AD-App](../develop/active-directory-optional-claims.md).
   >- Informationen zum Festlegen von Tokengültigkeitsdauer-Richtlinien für Aktualisierungstoken, Zugriffstoken, Sitzungstoken und ID-Token finden Sie unter [Konfigurierbare Tokengültigkeitsdauern in Azure Active Directory (Vorschau)](../develop/active-directory-configurable-token-lifetimes.md). Informationen zum Einschränken von Authentifizierungssitzungen mithilfe des bedingten Azure AD-Zugriffs finden Sie unter [Konfigurieren der Verwaltung von Authentifizierungssitzungen mit bedingtem Zugriff](https://go.microsoft.com/fwlink/?linkid=2083106).

## <a name="step-3-manage-the-saml-signing-certificate"></a>Schritt 3: Verwalten des SAML-Signaturzertifikats

Azure AD verwendet ein Zertifikat zum Signieren der SAML-Token, die an die Anwendung gesendet werden. Sie benötigen dieses Zertifikat, um die Vertrauensstellung zwischen Azure AD und der Anwendung einzurichten. Einzelheiten zum Zertifikatsformat finden Sie in der SAML-Dokumentation der Anwendung. Weitere Informationen finden Sie unter [Verwalten von Zertifikaten für die einmalige Verbundanmeldung in Azure Active Directory](manage-certificates-for-federated-single-sign-on.md) und [Erweiterte Optionen für die Zertifikatsignatur im SAML-Token für Katalog-Apps in Azure Active Directory](certificate-signing-options.md).

In Azure AD können Sie das aktive Zertifikat im Base64- oder Raw-Format direkt von der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** herunterladen. Alternativ dazu können Sie das aktive Zertifikat erhalten, indem Sie die XML-Datei mit den Anwendungsmetadaten herunterladen oder die App-Verbundmetadaten-URL verwenden. Führen Sie diese Schritte aus, um Ihre Zertifikate (aktiv oder inaktiv) anzuzeigen, zu erstellen oder herunterzuladen.

1. Navigieren Sie zum Abschnitt **SAML-Signaturzertifikat**. 

   ![Schritt 3: Verwalten des SAML-Signaturzertifikats](./media/configure-single-sign-on-non-gallery-applications/step-three-certificate.png)

2. Stellen Sie sicher, dass das Zertifikat über Folgendes verfügt:

   - *Das gewünschte Ablaufdatum*. Sie können ein Ablaufdatum konfigurieren, das bis zu drei Jahre in der Zukunft liegt.
   - *Aktiver Status des gewünschten Zertifikats*. Wenn der Status **Inaktiv** lautet, ändern Sie diesen zu **Aktiv**. Um den Status zu ändern, klicken Sie mit der rechten Maustaste auf die Zeile des gewünschten Zertifikats, und wählen Sie **Zertifikat als aktiv festlegen** aus.
   - *Die richtige Anmeldeoption und der entsprechende Algorithmus*.
   - *Die richtige(n) E-Mail-Adresse(n) für Benachrichtigungen*. Wenn sich das aktive Zertifikat dem Ablaufdatum nähert, sendet Azure AD eine Benachrichtigung an die in diesem Feld konfigurierte E-Mail-Adresse.

2. Um das Zertifikat herunterzuladen, wählen Sie eine der Optionen für Base64-Format, Raw-Format oder Verbundmetadaten-XML aus. Azure AD bietet auch die **App-Verbundmetadaten-URL**, unter der Sie auf die anwendungsspezifischen Metadaten zugreifen können (im Format `https://login.microsoftonline.com/<Directory ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<Application ID>`).

3. Um ein Zertifikat zu verwalten, zu erstellen oder zu importieren, wählen Sie rechts oben im Abschnitt **SAML-Signaturzertifikat** das Symbol **Bearbeiten** (Stiftsymbol) aus.

   ![SAML-Signaturzertifikat](./media/configure-single-sign-on-non-gallery-applications/saml-signing-certificate.png)


   Führen Sie eine der folgenden Aktionen aus:

   - Wenn Sie ein neues Zertifikat erstellen möchten, wählen Sie **Neues Zertifikat**, unter **Ablaufdatum** das Ablaufdatum und anschließend **Speichern** aus. Wenn Sie das Zertifikat aktivieren möchten, wählen Sie das Kontextmenü ( **...** ) und anschließend **Zertifikat als aktiv festlegen** aus.
   - Wenn Sie ein Zertifikat mit privatem Schlüssel und PFX-Anmeldeinformationen hochladen möchten, wählen Sie **Zertifikat importieren** aus, und navigieren Sie zu dem Zertifikat. Geben Sie unter **PFX-Kennwort** das PFX-Kennwort ein, und wählen Sie anschließend **Speichern** aus.  
   - Wenn Sie erweiterte Optionen für die Zertifikatsignatur konfigurieren möchten, verwenden Sie die folgenden Optionen. Beschreibungen dieser Optionen finden Sie im Artikel unter [Erweiterte Optionen für die Zertifikatsignatur im SAML-Token für Katalog-Apps in Azure Active Directory](certificate-signing-options.md).
      - Wählen Sie in der Dropdownliste **Signaturoption** die Option **SAML-Anwort signieren**, **SAML-Assertion signieren** oder **SAML-Antwort und-Assertion signieren** aus.
      - Wählen Sie in der Dropdownliste **Signaturalgorithmus** die Option **SHA-1** oder **SHA-256** aus.
   - Wenn weitere Personen darüber informiert werden sollen, dass das Ablaufdatum des aktiven Zertifikats bald erreicht ist, geben Sie die entsprechenden E-Mail-Adressen in die Felder für die **Benachrichtigungs-E-Mail-Adressen**ein.

4. Wenn Sie Änderungen vorgenommen haben, klicken Sie oben im Abschnitt **SAML-Signaturzertifikat** auf **Speichern**. 

## <a name="step-4-set-up-the-application-to-use-azure-ad"></a>Schritt 4: Einrichten der Anwendung für die Verwendung von Azure AD

Im Abschnitt **\<Anwendungsname> einrichten** werden die Werte aufgeführt, die in der Anwendung konfiguriert werden müssen, damit sie Azure AD als SAML-Identitätsanbieter verwendet. Die erforderlichen Werte variieren je nach Anwendung. Einzelheiten finden Sie in der SAML-Dokumentation der Anwendung. Wechseln Sie zur Rubrik **\<Anwendungsname> einrichten**, und wählen Sie **Schrittanleitung anzeigen** aus. Die Dokumentation wird auf der Seite **Anmeldung konfigurieren** angezeigt. Auf dieser Seite finden Sie Informationen dazu, wie Sie die Werte für **Anmelde-URL**, **Azure AD-Bezeichner** und **Abmelde-URL** in der Rubrik **\<Anwendungsname> einrichten** ausfüllen.

1. Scrollen Sie nach unten zum Abschnitt **\<applicationName> einrichten**. 
   
   ![Schritt 4: Einrichten der Anwendung](media/configure-single-sign-on-non-gallery-applications/step-four-app-config.png)

1. Kopieren Sie nach Bedarf den Wert aus der jeweiligen Zeile dieses Abschnitts, und fügen Sie ihn gemäß den anwendungsspezifischen Anweisungen der Anwendung hinzu. Bei Katalog-Apps können Sie die Dokumentation durch Auswählen von **Schrittanleitung anzeigen** anzeigen. 
   - Die Werte für **Anmelde-URL** und **Abmelde-URL** werden in den gleichen Endpunkt aufgelöst – hierbei handelt es sich um den Endpunkt, in dem die SAML-Anforderungen für Ihre Azure AD-Instanz verarbeitet werden. 
   - **Azure AD-Bezeichner** ist der Wert für den **Issuer** (Aussteller) in dem SAML-Token, das für die Anwendung ausgestellt wird.
2. Wenn Sie alle Werte in die entsprechenden Felder eingefügt haben, klicken Sie auf **Speichern**.

## <a name="step-5-validate-single-sign-on"></a>Schritt 5: Überprüfen des einmaligen Anmeldens

Sobald Sie Ihre Anwendung für die Verwendung von Azure AD als SAML-basiertem Identitätsanbieter konfiguriert haben, können Sie die Einstellungen testen, um zu überprüfen, ob das einmalige Anmelden für Konto funktioniert. 

2. Scrollen Sie zum Abschnitt **Einmaliges Anmelden mit <applicationName> überprüfen**.

   ![Schritt 5: Überprüfen des einmaligen Anmeldens](media/configure-single-sign-on-non-gallery-applications/step-five-validate.png)

3. Wählen Sie **Überprüfen**. Die Testoptionen werden angezeigt.

4. Klicken Sie auf **Als aktueller Benutzer anmelden**. 

Ist die Anmeldung erfolgreich, können Sie Ihrer SAML-Anwendung Benutzer und Gruppen zuweisen.
Sollte eine Fehlermeldung angezeigt werden, gehen Sie wie folgt vor:

1. Kopieren Sie die Details, und fügen Sie sie ins Feld **Wie äußert sich der Fehler?** ein.

    ![Leitfaden zur Problemlösung abrufen](media/configure-single-sign-on-portal/error-guidance.png)

2. Klicken Sie auf **Leitfaden zur Problemlösung abrufen**. Die Grundursache und Informationen zur Problemlösung werden angezeigt.  In diesem Beispiel wurde der Benutzer nicht der Anwendung zugewiesen.

3. Lesen Sie den Leitfaden zur Problemlösung, und beheben Sie nach Möglichkeit das Problem.

4. Führen Sie den Test erneut aus, bis der Vorgang erfolgreich abgeschlossen wird.

Weitere Informationen finden Sie unter [Debuggen des SAML-basierten einmaligen Anmeldens bei Anwendungen in Azure Active Directory](../develop/howto-v1-debug-saml-sso-issues.md).

## <a name="next-steps"></a>Nächste Schritte

- [Zuweisen von Benutzern und Gruppen zu einer Anwendung in Azure Active Directory](methods-for-assigning-users-and-groups.md)
- [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps im Azure-Portal](configure-automatic-user-provisioning-portal.md)

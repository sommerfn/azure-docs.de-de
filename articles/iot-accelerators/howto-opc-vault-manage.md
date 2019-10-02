---
title: 'Verwalten des OPC Vault-Zertifikatdiensts: Azure | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie die Zertifikate der Stammzertifizierungsstelle und die Benutzerberechtigungen für OPC Vault verwalten.
author: mregen
ms.author: mregen
ms.date: 8/16/2019
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 890a25ed2cf11d657cad930815d78dbf968cc9f9
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71203654"
---
# <a name="manage-the-opc-vault-certificate-service"></a>Verwalten des OPC Vault-Zertifikatdiensts

In diesem Artikel werden die administrativen Aufgaben für den OPC Vault-Zertifikatverwaltungsdienst in Azure erläutert. Er enthält Informationen zum Erneuern von Zertifizierungsstellenzertifikaten (ZS-Zertifikaten) des Zertifikatausstellers, zum Erneuern der Zertifikatssperrliste sowie zum Gewähren und Widerrufen des Benutzerzugriffs.

## <a name="create-or-renew-the-root-ca-certificate"></a>Erstellen oder Erneuern des Zertifikats der Stammzertifizierungsstelle

Nach der Bereitstellung von OPC Vault müssen Sie das Zertifikat der Stammzertifizierungsstelle erstellen. Ohne ein gültiges ZS-Zertifikat des Zertifikatausstellers können Sie keine Anwendungszertifikate signieren oder ausstellen. Wenn Sie Ihre Zertifikate mit angemessener, sicherer Gültigkeitsdauer verwalten möchten, lesen Sie [Zertifikate](howto-opc-vault-secure-ca.md#certificates). Erneuern Sie ein ZS-Zertifikat des Zertifikatausstellers nach seiner halben Gültigkeitsdauer. Beachten Sie bei der Erneuerung auch, dass die konfigurierte Gültigkeitsdauer eines neu signierten Anwendungszertifikats die Gültigkeitsdauer des ZS-Zertifikats des Zertifikatausstellers nicht überschreiten darf.
> [!IMPORTANT]
> Zum Erstellen oder Erneuern des ZS-Zertifikats des Zertifikatausstellers ist die Rolle „Administrator“ erforderlich.

1. Öffnen Sie Ihren Zertifikatdienst unter `https://myResourceGroup-app.azurewebsites.net`, und melden Sie sich an.
2. Wechseln Sie zu **Certificate Groups** (Zertifikatgruppen).
3. Dort wird eine einzige Standardzertifikatgruppe aufgelistet. Wählen Sie **Bearbeiten** aus.
4. Unter **Edit Certificate Group Details** (Details der Zertifikatgruppe bearbeiten) können Sie den Antragstellernamen und die Gültigkeitsdauer Ihrer ZS- und Anwendungszertifikate ändern. Der Antragsteller und die Gültigkeitsdauer sollten nur einmal vor der erstmaligen Ausstellung des ZS-Zertifikats festgelegt werden. Änderungen der Gültigkeitsdauer während des Betriebs könnten zu inkonsistenten Gültigkeitsdauern von ausgestellten Zertifikaten und Zertifikatssperrlisten führen.
5. Geben Sie einen gültigen Antragsteller ein (z.B. `CN=My CA Root, O=MyCompany, OU=MyDepartment`).<br>
   > [!IMPORTANT]
   > Wenn Sie den Antragsteller ändern, müssen Sie das Zertifikat des Zertifikatausstellers erneuern. Andernfalls kann der Dienst keine Anwendungszertifikate signieren. Der Antragsteller für die Konfiguration wird mit dem Antragsteller im aktiven Zertifikat des Zertifikatausstellers abgeglichen. Stimmen die Antragsteller nicht überein, wird die Zertifikatsignatur verweigert.
6. Wählen Sie **Speichern** aus.
7. Wenn an dieser Stelle ein Fehler vom Typ „Verboten“ auftritt, wurde Ihren Benutzeranmeldeinformationen die Administratorberechtigung zum Ändern oder Erstellen eines neuen Stammzertifikats nicht zugewiesen. Standardmäßig verfügt der Benutzer, der den Dienst bereitgestellt hat, über Administrator-und Signaturrollen bei dem Dienst. Andere Benutzer müssen den Rollen „Genehmigende Person“, „Writer“ oder „Administrator“ entsprechend der Azure Active Directory (Azure AD)-Anwendungsregistrierung hinzugefügt werden.
8. Wählen Sie **Details** aus. Daraufhin sollten die aktualisierten Informationen angezeigt werden.
9. Wählen Sie **Zertifizierungsstellenzertifikat erneuern** aus, um das erste ZS-Zertifikat des Zertifikatausstellers auszustellen oder das Ausstellerzertifikat zu erneuern. Wählen Sie dann **OK**aus.
10. Nach einigen Sekunden wird **Zertifikatdetails** angezeigt. Wählen Sie **Antragsteller** oder **Zertifikatssperrliste** aus, um das aktuelle ZS-Zertifikat und die Zertifikatssperrliste zur Verteilung an Ihre OPC UA-Anwendungen herunterzuladen.

Jetzt ist der OPC UA-Zertifikatverwaltungsdienst bereit, Zertifikate für OPC UA-Anwendungen auszustellen.

## <a name="renew-the-crl"></a>Erneuern der Zertifikatssperrliste

Die Erneuerung der Zertifikatssperrliste ist ein Update, das in regelmäßigen Abständen an die Anwendungen verteilt werden sollte. OPC UA-Geräte, die die X509-Erweiterung für den Zertifikatssperrlisten-Verteilungspunkt unterstützen, können die Zertifikatssperrliste direkt über den Endpunkt des Microservices aktualisieren. Andere OPC UA-Geräte erfordern möglicherweise manuelle Updates oder können mithilfe von GDS-Serverpusherweiterungen (*) aktualisiert werden, um die Vertrauenslisten mit den Zertifikaten und Zertifikatssperrlisten zu aktualisieren.

Im folgenden Workflow werden alle Zertifikatanforderungen im gelöschten Zustand in den Zertifikatssperrlisten widerrufen, die dem ZS-Zertifikat des Zertifikatausstellers entsprechen, für das sie ausgestellt wurden. Die Versionsnummer der Zertifikatssperrliste wird um 1 erhöht. <br>
> [!NOTE]
> Alle ausgestellten Zertifikatssperrlisten sind bis zum Ablauf des ZS-Zertifikats des Zertifikatausstellers gültig. Dies liegt daran, dass die OPC UA-Spezifikation kein verbindliches, deterministisches Verteilungsmodell für Zertifikatssperrlisten erfordert.

> [!IMPORTANT]
> Zum Erneuern der Zertifikatssperrliste des Zertifikatausstellers ist die Rolle „Administrator“ erforderlich.

1. Öffnen Sie Ihren Zertifikatdienst unter `https://myResourceGroup.azurewebsites.net`, und melden Sie sich an.
2. Wechseln Sie zur Seite **Zertifikatgruppen**.
3. Wählen Sie **Details** aus. Daraufhin sollten das aktuelle Zertifikat und Informationen zur Zertifikatssperrliste angezeigt werden.
4. Wählen Sie **Update CRL Revocation List (CRL)** (Zertifikatssperrliste aktualisieren) aus, um eine aktualisierte Zertifikatssperrliste für alle aktiven Ausstellerzertifikate im OPC Vault-Speicher auszustellen.
5. Nach einigen Sekunden wird **Zertifikatdetails** angezeigt. Wählen Sie **Antragsteller** oder **Zertifikatssperrliste** aus, um das aktuelle ZS-Zertifikat und die Zertifikatssperrliste zur Verteilung an Ihre OPC UA-Anwendungen herunterzuladen.

## <a name="manage-user-roles"></a>Verwalten von Benutzerrollen

Sie verwalten Benutzerrollen für den OPC Vault-Microservice in der Azure AD-Unternehmensanwendung. Eine ausführliche Beschreibung der Rollendefinitionen finden Sie unter [Rollen](howto-opc-vault-secure-ca.md#roles).

Ein authentifizierter Benutzer im Mandanten kann sich beim Dienst standardmäßig als „Leser“ anmelden. Rollen mit erweiterten Berechtigungen müssen manuell im Azure-Portal oder mithilfe von PowerShell verwaltet werden.

### <a name="add-user"></a>Benutzer hinzufügen

1. Öffnen Sie das Azure-Portal.
2. Wechseln Sie zu **Azure Active Directory** > **Unternehmensanwendungen**.
3. Wählen Sie die Registrierung des OPC Vault-Microservices aus (standardmäßig Ihr `resourceGroupName-service`).
4. Wechseln Sie zu **Benutzer und Gruppen**.
5. Wählen Sie **Add User** (Benutzer hinzufügen) aus.
6. Wählen Sie den Benutzer aus, den Sie einer bestimmten Rolle zuweisen möchten, oder laden Sie ihn ein.
7. Wählen Sie die Rolle für den Benutzer aus.
8. Wählen Sie **Zuweisen** aus.
9. Bei Benutzern mit der Rolle „Administrator“ oder „Genehmigende Person“ setzen Sie den Vorgang mit dem Hinzufügen von Azure Key Vault-Zugriffsrichtlinien fort.

### <a name="remove-user"></a>Entfernen von Benutzern

1. Öffnen Sie das Azure-Portal.
2. Wechseln Sie zu **Azure Active Directory** > **Unternehmensanwendungen**.
3. Wählen Sie die Registrierung des OPC Vault-Microservices aus (standardmäßig Ihr `resourceGroupName-service`).
4. Wechseln Sie zu **Benutzer und Gruppen**.
5. Wählen Sie den Benutzer mit einer Rolle aus, den Sie entfernen möchten. Wählen Sie dann **Entfernen** aus.
6. Entfernen Sie entfernte Benutzer mit der Rolle „Administrator“ oder „Genehmigende Person“ auch aus den Azure Key Vault-Richtlinien.

### <a name="add-user-access-policy-to-azure-key-vault"></a>Hinzufügen einer Benutzerzugriffsrichtlinie zu Azure Key Vault

Für die Rollen „Genehmigende Person“ und „Administrator“ sind zusätzliche Zugriffsrichtlinien erforderlich.

Um Vorgänge mit erhöhten Rechten oder Änderungen ohne Benutzeridentitätswechsel zu verhindern, verfügt die Dienstidentität standardmäßig nur über eingeschränkte Berechtigungen für den Zugriff auf Key Vault. Die grundlegenden Dienstberechtigungen für Geheimnisse und Zertifikate sind „Abrufen“ und „Auflisten“. Bei Geheimnissen gibt es nur eine Ausnahme: Der Dienst kann einen privaten Schlüssel aus dem Geheimnisspeicher löschen, nachdem er von einem Benutzer akzeptiert wurde. Für alle anderen Vorgänge sind Berechtigungen mit Benutzeridentitätswechsel erforderlich.

#### <a name="for-an-approver-role-the-following-permissions-must-be-added-to-key-vault"></a>Für die Rolle „Genehmigende Person“ müssen Key Vault folgende Berechtigungen hinzugefügt werden:

1. Öffnen Sie das Azure-Portal.
2. Wechseln Sie zu Ihrem OPC Vault `resourceGroupName`, der während der Bereitstellung verwendet wurde.
3. Wechseln Sie zum Key Vault `resourceGroupName-xxxxx`.
4. Wechseln Sie zu **Zugriffsrichtlinien**.
5. Wählen Sie **Neue hinzufügen** aus.
6. Überspringen Sie die Vorlage. Es gibt keine Vorlage, die den Anforderungen entspricht.
7. Wählen Sie **Prinzipal auswählen** und dann den Benutzer aus, den Sie hinzufügen möchten, oder laden Sie einen neuen Benutzer zum Mandanten ein.
8. Wählen Sie die folgenden **Schlüsselberechtigungen** aus: **Abrufen**, **Auflisten** und **Signieren**.
9. Wählen Sie die folgenden **Geheimnisberechtigungen** aus: **Abrufen**, **Auflisten**, **Festlegen** und **Löschen**.
10. Wählen Sie die folgenden **Zertifikatberechtigungen** aus: **Abrufen** und **Auflisten**.
11. Wählen Sie **OK** und dann **Speichern** aus.

#### <a name="for-an-administrator-role-the-following-permissions-must-be-added-to-key-vault"></a>Für die Rolle „Administrator“ müssen Key Vault folgende Berechtigungen hinzugefügt werden:

1. Öffnen Sie das Azure-Portal.
2. Wechseln Sie zu Ihrem OPC Vault `resourceGroupName`, der während der Bereitstellung verwendet wurde.
3. Wechseln Sie zum Key Vault `resourceGroupName-xxxxx`.
4. Wechseln Sie zu **Zugriffsrichtlinien**.
5. Wählen Sie **Neue hinzufügen** aus.
6. Überspringen Sie die Vorlage. Es gibt keine Vorlage, die den Anforderungen entspricht.
7. Wählen Sie **Prinzipal auswählen** und dann den Benutzer aus, den Sie hinzufügen möchten, oder laden Sie einen neuen Benutzer zum Mandanten ein.
8. Wählen Sie die folgenden **Schlüsselberechtigungen** aus: **Abrufen**, **Auflisten** und **Signieren**.
9. Wählen Sie die folgenden **Geheimnisberechtigungen** aus: **Abrufen**, **Auflisten**, **Festlegen** und **Löschen**.
10. Wählen Sie die folgenden **Zertifikatberechtigungen** aus: **Abrufen**, **Auflisten**, **Aktualisieren**, **Erstellen** und **Importieren**.
11. Wählen Sie **OK** und dann **Speichern** aus.

### <a name="remove-user-access-policy-from-azure-key-vault"></a>Entfernen einer Benutzerzugriffsrichtlinie aus Azure Key Vault

1. Öffnen Sie das Azure-Portal.
2. Wechseln Sie zu Ihrem OPC Vault `resourceGroupName`, der während der Bereitstellung verwendet wurde.
3. Wechseln Sie zum Key Vault `resourceGroupName-xxxxx`.
4. Wechseln Sie zu **Zugriffsrichtlinien**.
5. Suchen Sie den Benutzer, den Sie entfernen möchten, und wählen Sie **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie OPC Vault-Zertifikate und Benutzer verwaltet werden, können Sie sich jetzt zu Folgendem informieren:

> [!div class="nextstepaction"]
> [Schützen der Kommunikation von OPC-Geräten](howto-opc-vault-secure.md)

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
ms.openlocfilehash: 4420e5b0d895f8ea30dbd39fc50dd7480d57d086
ms.sourcegitcommit: 4b8a69b920ade815d095236c16175124a6a34996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2019
ms.locfileid: "69995996"
---
# <a name="how-to-manage-the-opc-vault-certificate-service"></a>Verwalten des OPC Vault-Zertifikatdiensts

In diesem Artikel werden die administrativen Aufgaben für den OPC Vault-Zertifikatverwaltungsdienst in Azure, das Erneuern der Zertifizierungsstellenzertifikate (ZS-Zertifikate) des Zertifikatausstellers, das Erneuern der Zertifikatssperrliste sowie das Erteilen und Widerrufen des Benutzerzugriffs erläutert.

## <a name="create-or-renew-the-root-ca-certificate"></a>Erstellen oder Erneuern des Zertifikats der Stammzertifizierungsstelle

Die Erstellung des Zertifikats der Stammzertifizierungsstelle ist ein obligatorischer Schritt nach der Bereitstellung. Ohne ein gültiges ZS-Zertifikat des Zertifikatausstellers können keine Anwendungszertifikate signiert und ausgestellt werden.<br>Informationen zum Verwalten Ihrer Zertifikate mit angemessener, sicherer Gültigkeitsdauer finden Sie im Kapitel zur [Gültigkeitsdauer von Zertifikaten](howto-opc-vault-secure-ca.md#certificates). Ein ZS-Zertifikat des Zertifikatausstellers sollte nach der Hälfte seiner Gültigkeitsdauer erneuert werden, aber spätestens, bevor diese von der konfigurierten Gültigkeitsdauer eines neu signierten Anwendungszertifikats überschritten wird.<br>
> [!IMPORTANT]
> Zum Erstellen oder Erneuern des ZS-Zertifikats des Zertifikatausstellers ist die Rolle „Administrator“ erforderlich.

1. Öffnen Sie den Zertifikatdienst unter `https://myResourceGroup-app.azurewebsites.net`, und melden Sie sich an.
2. Navigieren Sie zur Seite `Certificate Groups`.
3. Dort sehen Sie eine Zertifikatgruppe `Default`. Klicken Sie auf `Edit`.
4. Unter `Edit Certificate Group Details` können Sie den Antragstellernamen und die Gültigkeitsdauer Ihrer ZS- und Anwendungszertifikate ändern.<br>Der Antragsteller und die Gültigkeitsdauer sollten nur einmal vor der erstmaligen Ausstellung des ZS-Zertifikats festgelegt werden. Wird die Gültigkeitsdauer während des Betriebs geändert, kann dies zu einer inkonsistenten Gültigkeitsdauer von ausgestellten Zertifikaten und Zertifikatssperrlisten führen.
5. Geben Sie einen gültigen Antragsteller ein, z. B. `CN=My CA Root, O=MyCompany, OU=MyDepartment`.<br>
   > [!IMPORTANT]
   > Wenn Sie den Antragsteller ändern, muss das Zertifikat des Zertifikatausstellers erneuert werden. Andernfalls kann der Dienst keine Anwendungszertifikate signieren. Die Integrität des Antragstellers der Konfiguration wird anhand des aktiven Zertifikats des Zertifikatausstellers überprüft. Stimmen die Antragsteller nicht überein, wird die Zertifikatsignatur verweigert.
6. Klicken Sie auf die Schaltfläche `Save`.
7. Wenn an dieser Stelle ein Fehler vom Typ „Verboten“ auftritt, ist Ihren Benutzeranmeldeinformationen die Administratorberechtigung zum Ändern oder Erstellen eines neuen Stammzertifikats nicht zugewiesen. Standardmäßig verfügt der Benutzer, der den Dienst bereitgestellt hat, über Rollen mit Administrator- und Signierberechtigungen für den Dienst. Andere Benutzer müssen ggf. in der Azure AD-Anwendungsregistrierung den Rollen „Genehmigende Person“, „Schreiben“ oder „Administrator“ hinzugefügt werden.
8. Klicken Sie auf die Schaltfläche `Details`. Unter `View Certificate Group Details` sollten die aktualisierten Informationen angezeigt werden.
9. Klicken Sie auf die Schaltfläche `Renew CA Certificate`, um das erste ZS-Zertifikat des Zertifikatausstellers auszustellen oder das Zertifikat des Zertifikatausstellers zu erneuern. Klicken Sie auf `Ok`, um fortzufahren.
10. Nach wenigen Sekunden werden die `Certificate Details` angezeigt. Klicken Sie auf `Issuer` oder `Crl`, um das aktuelle ZS-Zertifikat und die Zertifikatssperrliste zur Verteilung an Ihre OPC UA-Anwendungen herunterzuladen.
11. Jetzt ist der OPC UA-Zertifikatverwaltungsdienst bereit, Zertifikate für OPC UA-Anwendungen auszustellen.

## <a name="renew-the-crl"></a>Erneuern der Zertifikatssperrliste

Die Erneuerung der Zertifikatssperrliste ist ein Update, das in regelmäßigen Abständen an die Anwendungen verteilt werden sollte. OPC UA-Geräte, die die X509-Erweiterung für den Zertifikatssperrlisten-Verteilungspunkt unterstützen, können die Zertifikatssperrliste direkt über den Endpunkt des Microservices aktualisieren. Andere OPC UA-Geräte erfordern möglicherweise manuelle Updates oder können im besten Fall mithilfe von GDS-Serverpusherweiterungen (*) aktualisiert werden, um die Vertrauenslisten mit den Zertifikaten und Zertifikatssperrlisten zu aktualisieren.

Im folgenden Workflow werden alle Zertifikatanforderungen im gelöschten Zustand in den Zertifikatssperrlisten widerrufen, die dem ZS-Zertifikat des Zertifikatausstellers entsprechen, für das sie ausgestellt wurden. Die Versionsnummer der Zertifikatssperrliste wird um 1 erhöht. <br>
> [!NOTE]
> Alle ausgestellten Zertifikatssperrlisten sind bis zum Ablauf des ZS-Zertifikats des Zertifikatausstellers gültig, da die OPC UA-Spezifikation kein obligatorisches, deterministisches Verteilungsmodell für Zertifikatssperrlisten vorschreibt.

> [!IMPORTANT]
> Zum Erneuern der Zertifikatssperrliste des Zertifikatausstellers ist die Rolle „Administrator“ erforderlich.

1. Öffnen Sie den Zertifikatdienst unter `https://myResourceGroup.azurewebsites.net`, und melden Sie sich an.
2. Navigieren Sie zur Seite `Certificate Groups`.
3. Klicken Sie auf die Schaltfläche `Details`. Unter `View Certificate Group Details` sollten die aktuellen Informationen zu den Zertifikaten und Zertifikatssperrlisten angezeigt werden.
4. Klicken Sie auf die Schaltfläche `Update CRL Revocation List(CRL)`, um eine aktualisierte Zertifikatssperrliste für alle aktiven Zertifikate des Zertifikatausstellers im OPC Vault-Speicher auszustellen.
5. Nach wenigen Sekunden werden die `Certificate Details` angezeigt. Klicken Sie auf `Issuer` oder `Crl`, um das aktuelle ZS-Zertifikat und die Zertifikatssperrliste zur Verteilung an Ihre OPC UA-Anwendungen herunterzuladen.

## <a name="manage-user-roles"></a>Verwalten von Benutzerrollen

Benutzerrollen für den OPC Vault-Microservice werden in der Azure Active Directory-Unternehmensanwendung verwaltet.

Eine ausführliche Beschreibung der Rollendefinitionen finden Sie im Abschnitt [Roles](howto-opc-vault-secure-ca.md#roles) (Rollen).

Ein authentifizierter Benutzer im Mandanten kann sich standardmäßig als „Leser“ beim Dienst anmelden. Rollen mit erweiterten Berechtigungen müssen manuell im Azure-Portal oder mit PowerShell verwaltet werden.

### <a name="add-user"></a>Benutzer hinzufügen

1. Öffnen Sie das Azure-Portal unter `portal.azure.com`.
2. Navigieren Sie zu `Azure Active Directory`/`Enterprise applications`.
3. Wählen Sie die Registrierung des OPC Vault-Microservices aus (standardmäßig Ihr `resourceGroupName-service`).
4. Navigieren Sie zu `Users and Groups`.
5. Klicken Sie auf `Add User`.
6. Wählen Sie den Benutzer aus, den Sie einer bestimmten Rolle zuweisen möchten, oder laden Sie ihn ein.
7. Wählen Sie die Rolle für den Benutzer aus.
8. Klicken Sie auf die Schaltfläche `Assign`.
9. Fügen Sie für Benutzer mit der Rolle `Administrator` oder `Approver` Azure Key Vault-Zugriffsrichtlinien hinzu.

### <a name="remove-user"></a>Entfernen von Benutzern

1. Öffnen Sie das Azure-Portal unter `portal.azure.com`.
2. Navigieren Sie zu `Azure Active Directory`/`Enterprise applications`.
3. Wählen Sie die Registrierung des OPC Vault-Microservices aus (standardmäßig Ihr `resourceGroupName-service`).
4. Navigieren Sie zu `Users and Groups`.
5. Wählen Sie einen Benutzer mit einer Rolle aus, den Sie entfernen möchten.
6. Klicken Sie auf die Schaltfläche `Remove`.
7. Entfernen Sie Administratoren und genehmigende Personen, die Sie entfernt haben, auch aus Azure Key Vault-Richtlinien.

### <a name="add-user-access-policy-to-azure-key-vault"></a>Hinzufügen einer Benutzerzugriffsrichtlinie zu Azure Key Vault

Für **Genehmigende Personen** und **Administratoren** sind zusätzliche Zugriffsrichtlinien erforderlich.

Um Vorgänge mit erhöhten Rechten oder Änderungen ohne Benutzeridentitätswechsel zu verhindern, verfügt die Dienstidentität standardmäßig nur über eingeschränkte Berechtigungen für den Zugriff auf Key Vault. Die grundlegenden Dienstberechtigungen für Geheimnisse und Zertifikate sind `Get` und `List`. Für Geheimnisse gibt es nur eine Ausnahme: Der Dienst kann mit `Delete` einen privaten Schlüssel aus dem Geheimnisspeicher löschen, sobald er von einem Benutzer akzeptiert wurde. Für alle anderen Vorgänge sind Berechtigungen mit Benutzeridentitätswechsel erforderlich.<br>

#### <a name="for-an-approver-role-the-following-permissions-must-be-added-to-key-vault"></a>Für die **Rolle „Genehmigende Person“** müssen Key Vault folgende Berechtigungen hinzugefügt werden:

1. Öffnen Sie das Azure-Portal unter `portal.azure.com`.
2. Navigieren Sie zum OPC Vault `resourceGroupName`, das während der Bereitstellung verwendet wurde.
3. Navigieren Sie zum Schlüsseltresor `resourceGroupName-xxxxx`.
4. Navigieren Sie zu den `Access Policies`.
5. Klicken Sie auf `Add new`.
6. Überspringen Sie die Vorlage (es gibt keine Vorlage, die den Anforderungen entspricht).
7. Klicken Sie auf `Select Principal`, und wählen Sie den Benutzer aus, den Sie hinzufügen möchten, oder laden Sie einen neuen Benutzer zum Mandanten ein.
8. Überprüfen Sie `Key permissions`: `Get`, `List` und vor allem `Sign`.
9. Überprüfen Sie `Secret permissions`: `Get`, `List`, `Set` und `Delete`.
10. Überprüfen Sie `Certificate permissions`: `Get` und `List`.
11. Klicken Sie auf `Ok`.
12. `Save` Sie die Änderungen.

#### <a name="for-an-administrator-role-the-following-permissions-must-be-added-to-key-vault"></a>Für eine **Administratorrolle** müssen Key Vault folgende Berechtigungen hinzugefügt werden:

1. Öffnen Sie das Azure-Portal unter `portal.azure.com`.
2. Navigieren Sie zum OPC Vault `resourceGroupName`, das während der Bereitstellung verwendet wurde.
3. Navigieren Sie zum Schlüsseltresor `resourceGroupName-xxxxx`.
4. Navigieren Sie zu den `Access Policies`.
5. Klicken Sie auf `Add new`.
6. Überspringen Sie die Vorlage (es gibt keine Vorlage, die den Anforderungen entspricht).
7. Klicken Sie auf `Select Principal`, und wählen Sie den Benutzer aus, den Sie hinzufügen möchten, oder laden Sie einen neuen Benutzer zum Mandanten ein.
8. Überprüfen Sie `Key permissions`: `Get`, `List` und vor allem `Sign`.
9. Überprüfen Sie `Secret permissions`: `Get`, `List`, `Set` und `Delete`.
10. Überprüfen Sie `Certificate permissions`: `Get`, `List`, `Update`, `Create` und `Import`.
11. Klicken Sie auf `Ok`.
12. `Save` Sie die Änderungen.

### <a name="remove-user-access-policy-from-azure-key-vault"></a>Entfernen einer Benutzerzugriffsrichtlinie aus Azure Key Vault

1. Öffnen Sie das Azure-Portal unter `portal.azure.com`.
2. Navigieren Sie zum OPC Vault `resourceGroupName`, das während der Bereitstellung verwendet wurde.
3. Navigieren Sie zum Schlüsseltresor `resourceGroupName-xxxxx`.
4. Navigieren Sie zu den `Access Policies`.
5. Suchen Sie nach dem Benutzer, den Sie entfernen möchten, und klicken Sie auf `... / Delete`, um den Benutzerzugriff zu löschen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Sie OPC Vault-Zertifikate und -Benutzer verwalten, schlagen wir als nächsten Schritt das folgende Thema vor:

> [!div class="nextstepaction"]
> [Schützen der Kommunikation von OPC-Geräten](howto-opc-vault-secure.md)

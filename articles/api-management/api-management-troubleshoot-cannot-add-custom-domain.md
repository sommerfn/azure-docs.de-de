---
title: Eine benutzerdefinierte Domäne kann nicht mithilfe des Key Vault-Zertifikats in Azure API Management hinzugefügt werden | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie das Problem beheben, dass Sie eine benutzerdefinierte Domäne in Azure API Management nicht mithilfe eines Key Vault-Zertifikats hinzufügen können.
services: api-management
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/19/2019
ms.author: tehnoonr
ms.openlocfilehash: 5d31ec21e341c46c2f2d0ab49fdb2d4302c29dc6
ms.sourcegitcommit: fad368d47a83dadc85523d86126941c1250b14e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2019
ms.locfileid: "71121519"
---
# <a name="failed-to-update-api-management-service-hostnames"></a>Fehler beim Aktualisieren von Hostnamen des API Management-Diensts

In diesem Artikel wird die Fehlermeldung „Fehler beim Aktualisieren von Hostnamen des API Management-Diensts“ beschrieben, die beim Hinzufügen einer benutzerdefinierten Domäne für den Azure API Management-Dienst auftreten kann. Dieser Artikel enthält Schritte, mit denen Sie dieses Problem beheben können.

## <a name="symptoms"></a>Symptome

Wenn Sie versuchen, mithilfe eines Zertifikats von Azure Key Vault eine benutzerdefinierte Domäne für den API Management-Dienst hinzuzufügen, erhalten Sie die folgende Fehlermeldung:

- Fehler beim Aktualisieren von Hostnamen des API Management-Diensts. Fehler bei Anforderung an Ressource „https://vaultname.vault.azure.net/secrets/secretname/?api-version=7.0“ StatusCode: Verboten für RequestID: . Ausnahmemeldung: Der Vorgang hat einen ungültigen Statuscode „Verboten“ zurückgegeben.

## <a name="cause"></a>Ursache

Der API Management-Dienst verfügt nicht über die Berechtigung für den Zugriff auf den Schlüsseltresor, den Sie für die benutzerdefinierte Domäne verwenden möchten.

## <a name="solution"></a>Lösung

Gehen Sie folgendermaßen vor, um das Problem zu beheben:

1. Wechseln Sie zum [Azure-Portal](Https://portal.azure.com), wählen Sie Ihre API Management-Instanz und dann **Verwaltete Identitäten** aus. Stellen Sie sicher, dass die Option **Registrierung bei Azure Active Directory** auf **Ja** gesetzt ist. 
    ![Registrierung bei Azure Active Directory](./media/api-management-troubleshoot-cannot-add-custom-domain/register-with-aad.png)
1. Öffnen Sie im Azure-Portal den Dienst **Schlüsseltresore**, und wählen Sie den Schlüsseltresor aus, den Sie für die benutzerdefinierte Domäne verwenden möchten.
1. Wählen Sie **Zugriffsrichtlinien** aus, und überprüfen Sie, ob ein Dienstprinzipal vorhanden ist, der mit dem Namen der API Management-Dienstinstanz übereinstimmt. Wenn dies der Fall ist, wählen Sie den Dienstprinzipal aus, und vergewissern Sie sich, dass die **Get**-Berechtigung unter **Berechtigungen für Geheimnis** aufgeführt ist.  
    ![Hinzufügen einer Zugriffsrichtlinie für den Dienstprinzipal](./media/api-management-troubleshoot-cannot-add-custom-domain/access-policy.png)
1. Wenn der API Management-Dienst nicht in der Liste enthalten ist, wählen Sie **Zugriffsrichtlinie hinzufügen** aus, und erstellen Sie dann die folgende Zugriffsrichtlinie:
    - **Anhand einer Vorlage konfigurieren**: Keine
    - **Prinzipal auswählen**: Suchen Sie den Namen des API Management-Diensts, und wählen Sie ihn in der Liste aus.
    - **Schlüsselberechtigungen**: Keine
    - **Berechtigungen für Geheimnis**: Get
    - **Zertifikatberechtigungen**: Keine
1. Wählen Sie **OK** aus, um die Zugriffsrichtlinie zu erstellen.
1. Klicken Sie zum Speichern der Änderungen auf **Speichern**.

Überprüfen Sie, ob das Problem behoben ist. Versuchen Sie hierzu, die benutzerdefinierte Domäne im API Management-Dienst zu erstellen, indem Sie das Key Vault-Zertifikat verwenden.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum API Management-Dienst:

- Hier finden Sie weitere [Videos](https://azure.microsoft.com/documentation/videos/index/?services=api-management) zu API Management.
* Weitere Methoden zum Sichern Ihres Back-End-Diensts finden Sie unter [Sichern von Back-End-Diensten über eine Clientzertifikatauthentifizierung in Azure API Management](api-management-howto-mutual-certificates.md).

* [Erstellen einer API Management-Dienstinstanz](get-started-create-service-instance.md)
* [Verwalten Ihrer ersten API](import-and-publish.md)
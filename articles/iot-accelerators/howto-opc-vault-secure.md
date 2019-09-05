---
title: 'Schützen der Kommunikation von OPC UA-Geräten mit OPC Vault: Azure | Microsoft-Dokumentation'
description: Es wird beschrieben, wie Sie OPC UA-Anwendungen registrieren und signierte Anwendungszertifikate für Ihre OPC UA-Geräte mit OPC Vault ausstellen.
author: mregen
ms.author: mregen
ms.date: 8/16/2018
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: ae10bb3488c21b6637163e333231e95a18c652bf
ms.sourcegitcommit: 4b8a69b920ade815d095236c16175124a6a34996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2019
ms.locfileid: "69996139"
---
# <a name="how-to-use-the-opc-vault-certificate-management-service"></a>Verwenden des OPC Vault-Zertifikatverwaltungsdiensts

In diesem Artikel wird beschrieben, wie Sie Anwendungen registrieren und signierte Anwendungszertifikate für Ihre OPC UA-Geräte ausstellen.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="deploy-the-certificate-management-service"></a>Bereitstellen des Zertifikatverwaltungsdiensts

Zunächst muss der Dienst in der Azure-Cloud bereitgestellt werden.
Lesen Sie den Artikel, in dem das [Bereitstellen des OPC Vault-Zertifikatverwaltungsdiensts](howto-opc-vault-deploy.md) beschrieben wird.

### <a name="create-the-issuer-ca-certificate"></a>Erstellen des ZS-Zertifikats des Zertifikatausstellers

Erstellen Sie das ZS-Zertifikat des Zertifikatausstellers, falls Sie dies noch nicht durchgeführt haben.

Lesen Sie den Artikel, in dem das [Erstellen und Verwalten des Zertifikats des Zertifikatausstellers für OPC Vault](howto-opc-vault-manage.md) beschrieben wird.

## <a name="secure-opc-ua-applications"></a>Schützen von OPC UA-Anwendungen

### <a name="step-1-register-your-opc-ua-application"></a>Schritt 1: Registrieren Ihrer OPC UA-Anwendung 

> [!IMPORTANT]
> Die Rolle „Verfasser“ ist erforderlich, um eine Anwendung zu registrieren.

1. Öffnen Sie den Zertifikatdienst unter `https://myResourceGroup-app.azurewebsites.net`, und melden Sie sich an.
2. Navigieren Sie zur Seite `Register New`.
1. Für eine Anwendungsregistrierung muss einem Benutzer mindestens die Rolle „Verfasser“ zugewiesen sein.
2. Für das Eingabeformular gelten die Namenskonventionen aus dem OPC UA-Bereich. In der Abbildung unten sind beispielsweise die Einstellungen für das Beispiel [OPC UA-Referenzserver](https://github.com/OPCFoundation/UA-.NETStandard/tree/master/SampleApplications/Workshop/Reference) im OPC UA .NET Standard-Stapel dargestellt:

   ![Registrierung des UA-Referenzservers](media/howto-opc-vault-secure/reference-server-registration.png "Registrierung des UA-Referenzservers")

5. Wählen Sie die Schaltfläche `Register` aus, um die Anwendung in der Anwendungsdatenbank des Zertifikatdiensts zu registrieren. Im Workflow wird der Benutzer direkt zum nächsten Schritt weitergeleitet, um für die Anwendung ein signiertes Zertifikat anzufordern.

### <a name="step-2-secure-your-application-with-a-ca-signed-application-certificate"></a>Schritt 2: Schützen Ihrer Anwendung mit einem von einer Zertifizierungsstelle signierten Anwendungszertifikat

Sie können eine OPC UA-Anwendung schützen, indem Sie ein signiertes Zertifikat basierend auf einer Zertifikatsignieranforderung (Certificate Signing Request, CSR) ausstellen oder ein neues Schlüsselpaar anfordern, das auch einen neuen privaten Schlüssel im PFX- oder PEM-Format enthält. Sie können der Dokumentation Ihres OPC UA-Geräts entnehmen, welche Methode für Ihre Anwendung unterstützt wird. Im Allgemeinen wird die Methode mit CSR empfohlen, weil hierfür kein privater Schlüssel über eine Leitung übertragen werden muss.

- Führen Sie [Schritt 3a](##step-3a-request-a-new-certificate-with-a-new-keypair) aus, falls für Ihr Gerät ein neues Schlüsselpaar ausgestellt werden muss.
- Wenn Ihr Gerät das Ausstellen einer Zertifikatsignieranforderung (Certificate Signing Request, CSR) unterstützt, sollten Sie [Schritt 3b](#step-3b-request-a-new-certificate-with-a-csr) ausführen.

### <a name="step-3a-request-a-new-certificate-with-a-new-keypair"></a>Schritt 3a: Anfordern eines neuen Zertifikats mit einem neuen Schlüsselpaar

1. Navigieren Sie zur Seite `Applications`.
3. Starten Sie den Workflow für die Zertifikatanforderung, indem Sie für eine aufgeführte Anwendung die Option `New Request` auswählen.

   ![Neues Zertifikat anfordern](media/howto-opc-vault-secure/request-new-certificate.png "Neues Zertifikat anfordern")

3. Wählen Sie „Request new KeyPair and Certificate“ (Neues Schlüsselpaar und Zertifikat anfordern) aus, um einen privaten Schlüssel und ein neues signiertes Zertifikat mit dem öffentlichen Schlüssel für Ihre Anwendung anzufordern.

   ![Neues Schlüsselpaar generieren](media/howto-opc-vault-secure/generate-new-key-pair.png "Neues Schlüsselpaar generieren")

4. Geben Sie in das Formular einen Antragsteller und die Domänennamen ein, und wählen Sie PEM oder PFX mit Kennwort für den privaten Schlüssel aus. Wählen Sie die Schaltfläche `Generate New Certificate` aus, um die Zertifikatanforderung zu erstellen.

   ![Zertifikat genehmigen](media/howto-opc-vault-secure/approve-reject.png "Zertifikat genehmigen")

5. Für den Genehmigungsschritt muss ein Benutzer vorhanden sein, der über die Rolle „Genehmigende Person“ und Signierberechtigungen in Azure Key Vault verfügt. In einem typischen Workflow sollten die Rolle „Genehmigende Person“ bzw. „Anfordernde Person“ unterschiedlichen Benutzern zugewiesen werden.
6. Genehmigen Sie das Zertifikat, oder lehnen Sie es ab, um die eigentliche Erstellung des Schlüsselpaars und den Signiervorgang zu starten bzw. abzubrechen. Das neue Schlüsselpaar wird erstellt und sicher in Azure Key Vault gespeichert, bis es vom Anforderer des Zertifikats heruntergeladen wird. Das sich ergebende Zertifikat mit öffentlichem Schlüssel wird von der Zertifizierungsstelle signiert. Es kann einige Sekunden dauern, bis diese Vorgänge abgeschlossen sind.

   ![Schlüsselpaar anzeigen](media/howto-opc-vault-secure/view-key-pair.png "Schlüsselpaar anzeigen")

7. Der sich ergebende private Schlüssel (PFX oder PEM) und das Zertifikat (DER) können hier in dem Format heruntergeladen werden, das für den Download von Binärdateien ausgewählt wurde. Es ist auch eine Base64-codierte Version verfügbar, z. B. zum Kopieren und Einfügen des Zertifikats in eine Befehlszeile oder einen Texteintrag. 
8. Nachdem der private Schlüssel heruntergeladen und sicher gespeichert wurde, können Sie ihn mit der Schaltfläche `Delete Private Key` aus dem Dienst löschen. Das Zertifikat mit dem öffentlichen Schlüssel ist für die spätere Verwendung weiterhin verfügbar.
9. Aufgrund der Verwendung eines von der Zertifizierungsstelle signierten Zertifikats sollten hier auch das ZS-Zertifikat und die Zertifikatsperrliste heruntergeladen werden.
10. Jetzt liegt es am OPC UA-Gerät, wie das neue Schlüsselpaar angewendet wird. Normalerweise werden das ZS-Zertifikat und die Zertifikatsperrliste in den Ordner `trusted` kopiert, während der öffentliche und private Schlüssel des Anwendungszertifikats auf den Ordner `own` im Zertifikatspeicher angewendet wird. Einige Geräte unterstützen ggf. bereits einen „Serverpush“-Vorgang für Zertifikatupdates. Informationen hierzu finden Sie in der Dokumentation Ihres OPC UA-Geräts.

### <a name="step-3b-request-a-new-certificate-with-a-csr"></a>Schritt 3b: Anfordern eines neuen Zertifikats mit einer Zertifikatsignieranforderung 

1. Navigieren Sie zur Seite `Applications`.
3. Starten Sie den Workflow für die Zertifikatanforderung, indem Sie für eine aufgeführte Anwendung die Option `New Request` auswählen.

   ![Neues Zertifikat anfordern](media/howto-opc-vault-secure/request-new-certificate.png "Neues Zertifikat anfordern")

3. Wählen Sie „Request new Certificate with signing request“ (Neues Zertifikat mit Signieranforderung anfordern) aus, um für Ihre Anwendung ein neues signiertes Zertifikat anzufordern.

   ![Neues Zertifikat generieren](media/howto-opc-vault-secure/generate-new-certificate.png "Neues Zertifikat generieren")

4. Laden Sie die Zertifikatsignieranforderung hoch, indem Sie eine lokale Datei auswählen oder eine Base64-codierte Zertifikatsignieranforderung in das Formular einfügen. Wählen Sie die Schaltfläche `Generate New Certificate` aus, um die Zertifikatanforderung zu erstellen.

   ![Zertifikatanforderung genehmigen](media/howto-opc-vault-secure/approve-reject-csr.png "Zertifikatanforderung genehmigen")

5. Für den Genehmigungsschritt muss ein Benutzer vorhanden sein, der über die Rolle „Genehmigende Person“ und Signierberechtigungen in Azure Key Vault verfügt. Genehmigen Sie die Zertifikatanforderung, oder lehnen Sie sie ab, um den eigentlichen Signiervorgang zu starten bzw. abzubrechen. Das sich ergebende Zertifikat mit öffentlichem Schlüssel wird von der Zertifizierungsstelle signiert. Es kann einige Sekunden dauern, bis dieser Vorgang abgeschlossen ist.

   ![Zertifikat anzeigen](media/howto-opc-vault-secure/view-cert-csr.png "Zertifikat anzeigen")

6. Das sich ergebende Zertifikat (DER) kann hier als Binärdatei heruntergeladen werden. Es ist auch eine Base64-codierte Version verfügbar, z. B. zum Kopieren und Einfügen des Zertifikats in eine Befehlszeile oder einen Texteintrag. 
10. Nachdem das Zertifikat heruntergeladen und sicher gespeichert wurde, können Sie es mit der Schaltfläche `Delete Certificate` aus dem Dienst löschen.
11. Aufgrund der Verwendung eines von der Zertifizierungsstelle signierten Zertifikats sollten hier auch das ZS-Zertifikat und die Zertifikatsperrliste heruntergeladen werden.
12. Jetzt liegt es am OPC UA-Gerät, wie das neue Zertifikat angewendet wird. Normalerweise werden das ZS-Zertifikat und die Zertifikatsperrliste in den Ordner `trusted` kopiert, während das Anwendungszertifikat auf den Ordner `own` im Zertifikatspeicher angewendet wird. Einige Geräte unterstützen ggf. bereits einen „Serverpush“-Vorgang für Zertifikatupdates. Informationen hierzu finden Sie in der Dokumentation Ihres OPC UA-Geräts.

### <a name="step-4-device-secured"></a>Schritt 4: Gerät geschützt

Das OPC UA-Gerät ist jetzt für die Kommunikation mit anderen OPC UA-Geräten, die mit von einer Zertifizierungsstelle signierten Zertifikaten geschützt sind, ohne weitere Konfiguration bereit. Viel Spaß!


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich darüber informiert haben, wie Sie OPC UA-Geräte schützen, empfehlen wir Ihnen als nächsten Schritt Folgendes:

> [!div class="nextstepaction"]
> [How to run the OPC Vault certificate management service securely](howto-opc-vault-secure-ca.md) (Sicheres Ausführen des OPC Vault-Zertifikatverwaltungsdiensts)
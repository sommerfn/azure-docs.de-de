---
title: Sicheres Ausführen des OPC Vault-Zertifikatverwaltungsdiensts – Azure | Microsoft-Dokumentation
description: Hier wird beschrieben, wie Sie den OPC Vault-Zertifikatverwaltungsdienst in Azure sicher ausführen und welche anderen Sicherheitsrichtlinien berücksichtigt werden müssen.
author: mregen
ms.author: mregen
ms.date: 8/16/2019
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 88f8188779c5fb6b3cd07c67e9f35a6b8f9ad97d
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71200079"
---
# <a name="run-the-opc-vault-certificate-management-service-securely"></a>Sicheres Ausführen des OPC Vault-Zertifikatverwaltungsdiensts

In diesem Artikel wird erläutert, wie Sie den OPC Vault-Zertifikatverwaltungsdienst in Azure sicher ausführen. Außerdem werden andere zu berücksichtigende Sicherheitsrichtlinien beschrieben.

## <a name="roles"></a>Rollen

### <a name="trusted-and-authorized-roles"></a>Vertrauenswürdige und autorisierte Rollen

Der OPC Vault-Microservice ermöglicht es, dass unterschiedliche Rollen auf verschiedene Teile des Diensts zugreifen können.

> [!IMPORTANT]
> Während der Bereitstellung fügt das Skript nur den Benutzer hinzu, der das Bereitstellungsskript als Benutzer für alle Rollen ausführt. Sie sollten diese Rollenzuweisung für eine Produktionsbereitstellung überprüfen und entsprechend den Richtlinien (siehe unten) neu konfigurieren. Diese Aufgabe erfordert die manuelle Zuweisung von Rollen und Diensten im Azure Active Directory-Portal (Azure AD) für Unternehmensanwendungen.

### <a name="certificate-management-service-roles"></a>Rollen des Zertifikatverwaltungsdiensts

Der OPC Vault-Microservice definiert die folgenden Rollen:

- **Leser:** Standardmäßig hat jeder authentifizierte Benutzer im Mandanten Lesezugriff. 
  - Lesezugriff auf Anwendungen und Zertifikatanforderungen. Kann Anwendungen und Zertifikatanforderungen auflisten und abfragen. Außerdem ist der Zugriff auf Geräteermittlungsinformationen und öffentliche Zertifikate mit Lesezugriff möglich.
- **Writer**: Die Rolle „Writer“ wird einem Benutzer zugewiesen, damit er Schreibberechtigungen für bestimmte Aufgaben hinzufügen kann. 
  - Lese-/Schreibzugriff auf Anwendungen und Zertifikatanforderungen. Kann Anwendungen registrieren, aktualisieren und deren Registrierung aufheben. Kann Zertifikatanforderungen erstellen sowie genehmigte private Schlüssel und Zertifikate abrufen. Kann private Schlüssel auch löschen.
- **Genehmigende Person**: Die Rolle „Genehmigende Person“ wird einem Benutzer zugewiesen, damit er Zertifikatanforderungen genehmigen oder ablehnen kann. Diese Rolle enthält keine andere Rolle.
  - Zusätzlich zur Rolle „Genehmigende Person“ für den Zugriff auf die API des OPC Vault-Microservices muss der Benutzer auch über die Schlüsselsignaturberechtigung in Azure Key Vault verfügen, um die Zertifikate signieren zu können.
  - Die Rollen „Writer“ und „Genehmigende Person“ sollten verschiedenen Benutzern zugewiesen werden.
  - Die Hauptrolle von „Genehmigende Person“ ist die Genehmigung der Generierung und Ablehnung von Zertifikatanforderungen.
- **Administrator**: Die Rolle „Administrator“ wird einem Benutzer zugewiesen, damit er die Zertifikatgruppen verwalten kann. Diese Rolle unterstützt nicht die Rolle „Genehmigende Person“, enthält aber die Rolle „Writer“.
  - Der Administrator kann die Zertifikatgruppen verwalten, die Konfiguration ändern und Anwendungszertifikate durch Ausstellung einer neuen Zertifikatsperrliste widerrufen.
  - Im Idealfall werden die Rollen „Writer“, „Genehmigende Person“ und „Administrator“ verschiedenen Benutzern zugewiesen. Um die Sicherheit zu erhöhen, benötigt ein Benutzer mit der Rolle „Genehmigende Person“ oder „Administrator“ außerdem die Schlüsselsignaturberechtigung in Key Vault, um Zertifikate ausstellen oder ein Zertifizierungsstellenzertifikat des Zertifikatausstellers erneuern zu können.
  - Zusätzlich zur Rolle „Microservice-Verwaltung“ umfasst die Rolle, ist aber nicht darauf beschränkt:
    - Zuständigkeit für die Verwaltung der Implementierung von Sicherheitspraktiken der Zertifizierungsstelle.
    - Verwaltung der Generierung, Sperrung und Aussetzung von Zertifikaten. 
    - Lebenszyklusverwaltung für Kryptografieschlüssel (z.B. die Erneuerung der Zertifizierungsstellenschlüssel des Zertifikatausstellers).
    - Installation, Konfiguration und Wartung von Diensten, die die Zertifizierungsstelle betreiben.
    - Täglicher Betrieb der Dienste. 
    - Sicherung und Wiederherstellung von Zertifizierungsstelle und Datenbank.

### <a name="other-role-assignments"></a>Andere Rollenzuweisungen

Beachten Sie bei der Ausführung des Diensts auch die folgenden Rollen:

- Geschäftlicher Besitzer des Zertifikatbeschaffungsvertrags bei der externen Stammzertifizierungsstelle (beispielsweise, wenn der Besitzer Zertifikate von einer externen Zertifizierungsstelle erwirbt oder eine Zertifizierungsstelle betreibt, die einer externen Zertifizierungsstelle untergeordnet ist).
- Entwicklung und Validierung der Zertifizierungsstelle.
- Überprüfung von Überwachungsdatensätzen.
- Mitarbeiter, die bei der Unterstützung der Zertifizierungsstelle oder der Verwaltung der physischen Funktionen und Cloudfunktionen helfen, aber nicht direkt vertrauenswürdig sind, um Zertifizierungsstellenvorgänge auszuführen, befinden sich laut Definition in der *autorisierten* Rolle. Der Satz von Aufgaben, den Personen in der autorisierten Rolle ausführen dürfen, muss ebenfalls dokumentiert werden.

### <a name="review-memberships-of-trusted-and-authorized-roles-quarterly"></a>Vierteljährliche Überprüfung der Mitgliedschaften von vertrauenswürdigen und autorisierten Rollen

Überprüfen Sie die Mitgliedschaft von vertrauenswürdigen und autorisierten Rollen mindestens vierteljährlich. Stellen Sie sicher, dass die Gruppe der Personen (für manuelle Prozesse) oder Dienstidentitäten (für automatisierte Prozesse) in jeder Rolle auf ein Minimum beschränkt wird.

### <a name="role-separation-between-certificate-requester-and-approver"></a>Rollentrennung zwischen Personen, die ein Zertifikat anfordern und genehmigen

Der Vorgang zur Zertifikatausstellung muss eine Rollentrennung zwischen den Rollen „das Zertifikat anfordernde Person“ und „das Zertifikat genehmigende Person“ (Personen oder automatisierte Systeme) erzwingen. Die Zertifikatausstellung muss von einer Rolle „das Zertifikat genehmigende Person“ autorisiert werden, die überprüft, ob die das Zertifikat anfordernde Person zum Abrufen von Zertifikaten autorisiert ist. Die Personen, die die Rolle „das Zertifikat genehmigende Person“ haben, müssen formell autorisiert sein.

### <a name="restrict-assignment-of-privileged-roles"></a>Beschränken der Zuweisung privilegierter Rollen

Sie sollten die Zuweisung privilegierter Rollen (z.B. Autorisierung der Mitgliedschaft in der Gruppe „Administratoren“ und „Genehmigende Personen“) auf eine begrenzte Menge von autorisierten Mitarbeitern beschränken. Bei Änderungen privilegierter Rollen muss der Zugriff innerhalb von 24 Stunden widerrufen werden. Schließlich: Überprüfen Sie die Zuweisungen privilegierter Rollen vierteljährlich, und entfernen Sie alle nicht benötigten oder abgelaufenen Zuweisungen.

### <a name="privileged-roles-should-use-two-factor-authentication"></a>Privilegierte Rollen sollten die zweistufige Authentifizierung (Two-Factor Authentication, TFA) verwenden

Verwenden Sie die mehrstufige Authentifizierung (auch als „zweistufige Authentifizierung“ bezeichnet) für interaktive Anmeldungen von „Genehmigende Personen“ und „Administratoren“ beim Dienst.

## <a name="certificate-service-operation-guidelines"></a>Richtlinien für den Zertifikatdienstvorgang

### <a name="operational-contacts"></a>Betriebliche Kontakte

Der Zertifikatdienst muss einen aktuellen Sicherheitsreaktionsplan gespeichert haben, der detaillierte Kontakte für die Reaktion auf betriebliche Vorfälle enthält.

### <a name="security-updates"></a>Sicherheitsupdates

Alle Systeme müssen kontinuierlich überwacht und mit den neuesten Sicherheitsupdates aktualisiert werden.

> [!IMPORTANT]
> Das GitHub-Repository des OPC Vault-Diensts wird mit Sicherheitspatches kontinuierlich aktualisiert. Überwachen Sie diese Updates, und wenden Sie sie in regelmäßigen Abständen auf den Dienst an.

### <a name="security-monitoring"></a>Sicherheitsüberwachung

Abonnieren Sie die entsprechende Sicherheitsüberwachung, oder implementieren Sie sie. Abonnieren Sie beispielsweise eine zentrale Überwachungslösung (z.B. Azure Security Center oder eine Überwachungslösung von Office 365), und konfigurieren Sie sie entsprechend, um sicherzustellen, dass sicherheitsrelevante Ereignisse an die Überwachungslösung übertragen werden.

> [!IMPORTANT]
> Der OPC Vault-Dienst wird standardmäßig mit [Azure Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/devops) als Überwachungslösung bereitgestellt. Es wird dringend empfohlen, eine Sicherheitslösung wie [Azure Security Center](https://azure.microsoft.com/services/security-center/) hinzuzufügen.

### <a name="assess-the-security-of-open-source-software-components"></a>Bewerten der Sicherheit von Open-Source-Softwarekomponenten

Keine der in einem Produkt oder Dienst verwendeten Open-Source-Komponenten darf mittlere oder größere Sicherheitsrisiken aufweisen.

> [!IMPORTANT]
> Während Continuous Integration-Builds überprüft das GitHub-Repository des OPC Vault-Diensts alle Komponenten auf Sicherheitsrisiken. Überwachen Sie diese Updates auf GitHub, und wenden Sie sie in regelmäßigen Abständen auf den Dienst an.

### <a name="maintain-an-inventory"></a>Verwalten eines Bestands

Verwalten Sie einen Assetbestand für alle Produktionshosts (einschließlich persistenter virtueller Computer), Geräte, alle internen IP-Adressbereiche, VIPs und öffentlichen DNS-Domänennamen. Wenn Sie ein System, eine Geräte-IP-Adresse, eine VIP oder eine öffentliche DNS-Domäne hinzufügen oder entfernen, müssen Sie den Bestand innerhalb von 30 Tagen aktualisieren.

#### <a name="inventory-of-the-default-azure-opc-vault-microservice-production-deployment"></a>Bestand der Standardproduktionsbereitstellung für Azure OPC Vault-Microservice 

In Azure:
- **App Service-Plan**: App Service-Plan für Diensthosts. Standard S1.
- **App Service** für Microservice: Der OPC Vault-Diensthost.
- **App Service** für die Beispielanwendung: Der OPC Vault-Host für die Beispielanwendung.
- **Key Vault Standard**: Zum Speichern von Geheimnissen und Azure Cosmos DB-Schlüsseln für die Webdienste.
- **Key Vault Premium**: Zum Hosten der Zertifizierungsstellenschlüssel des Zertifikatausstellers, für den Signaturdienst sowie für Vault-Konfiguration und Speicherung privater Anwendungsschlüssel.
- **Azure Cosmos DB**: Datenbank für Anwendungs- und Zertifikatanforderungen. 
- **Application Insights**: (optional) Überwachungslösung für den Webdienst und die Anwendung.
- **Azure AD-Anwendungsregistrierung**: Eine Registrierung für die Beispielanwendung, den Dienst und das Edge-Modul.

Für die Cloud-Dienste sollten alle Hostnamen, Ressourcengruppen, Ressourcennamen, Abonnement-IDs und Mandanten-IDs, die für die Bereitstellung des Diensts verwendet wurden, dokumentiert werden. 

In Azure IoT Edge oder auf einem lokalen IoT Edge-Server:
- **OPC Vault-IoT Edge-Modul**: Zur Unterstützung eines globalen OPC UA-Ermittlungsservers im Factory-Netzwerk. 

Bei den IoT Edge-Geräten sollten die Hostnamen und IP-Adressen dokumentiert werden. 

### <a name="document-the-certification-authorities-cas"></a>Dokumentieren der Zertifizierungsstellen (Certification Authorities, CAs)

Die Dokumentation der Zertifizierungsstellenhierarchie muss alle betriebenen Zertifizierungsstellen enthalten. Dies schließt alle zugehörigen untergeordneten Zertifizierungsstellen, übergeordneten Zertifizierungsstellen und Stammzertifizierungsstellen ein, selbst wenn sie nicht vom Dienst verwaltet werden. Statt der formalen Dokumentation können Sie einen umfassenden Satz aller nicht abgelaufenen Zertifizierungsstellenzertifikate bereitstellen.

> [!NOTE]
> Die OPC Vault-Beispielanwendung unterstützt den Download aller Zertifikate, die im Dienst für die Dokumentation verwendet und erstellt wurden.

### <a name="document-the-issued-certificates-by-all-certification-authorities-cas"></a>Dokumentieren der ausgestellten Zertifikate von allen Zertifizierungsstellen (Certification Authorities, CAs)

Stellen Sie einen umfassenden Satz aller Zertifikate bereit, die in den letzten 12 Monaten ausgestellt wurden.

> [!NOTE]
> Die OPC Vault-Beispielanwendung unterstützt den Download aller Zertifikate, die im Dienst für die Dokumentation verwendet und erstellt wurden.

### <a name="document-the-standard-operating-procedure-for-securely-deleting-cryptographic-keys"></a>Dokumentieren der Standardprozedur zum sicheren Löschen von kryptografischen Schlüsseln

Während der Lebensdauer einer Zertifizierungsstelle geschieht das Löschen von Schlüsseln möglicherweise nur selten. Aus diesem Grund wurde keinem Benutzer das Recht zum Löschen des Key Vault-Zertifikats zugewiesen, und es werden keine APIs zum Löschen eines Zertifikats der Zertifizierungsstelle verfügbar gemacht. Das manuelle Standardverfahren zum sicheren Löschen von Kryptografieschlüsseln für Zertifizierungsstellen steht nur zur Verfügung, indem auf den Key Vault im Azure-Portal direkt zugegriffen wird. Sie können die Zertifikatgruppe auch in Key Vault löschen. Wenn Sie das sofortige Löschen sicherstellen möchten, deaktivieren Sie die Funktionalität für [Vorläufiges Löschen von Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete).

## <a name="certificates"></a>Zertifikate

### <a name="certificates-must-comply-with-minimum-certificate-profile"></a>Zertifikate müssen dem minimalen Zertifikatprofil entsprechen

Der OPC Vault-Dienst ist eine Online-Zertifizierungsstelle (ZS), die Endeinheitzertifikate für Abonnenten ausstellt. Der OPC Vault-Microservice hält diese Richtlinien in der Standardimplementierung ein.

- Alle Zertifikate müssen die folgenden (unten angegebenen) X.509-Felder enthalten:
  - Der Inhalt des Felds „Version“ muss „v3“ lauten. 
  - Der Inhalt des Felds „serialNumber“ muss mindestens 8 Byte Entropie enthalten, die aus einem von FIPS (Federal Information Processing Standards) 140 genehmigten Zufallszahlengenerator abgerufen wurden.<br>
    > [!IMPORTANT]
    > Die Seriennummer des OPC Vault beträgt standardmäßig 20 Byte und wird vom Generator für kryptografische Zufallszahlen des Betriebssystems abgerufen. Der Zufallszahlengenerator ist von FIPS 140 auf Windows-Geräten genehmigt, aber nicht unter Linux. Berücksichtigen Sie dies, wenn Sie eine Dienstbereitstellung auswählen, bei der Linux-VMs oder Linux-Docker-Container verwendet werden, für die die zugrunde liegende Technologie OpenSSL von FIPS 140 nicht genehmigt wurde.
  - Die Felder „IssuerUniqueID“ und „subjectUniqueID“ dürfen nicht vorhanden sein.
  - Endeinheitzertifikate müssen in Übereinstimmung mit IETF RFC 5280 anhand der Erweiterung für Basiseinschränkungen identifiziert werden.
  - Das Feld „pathLenConstraint“ muss für das Zertifikat der ausstellenden Zertifizierungsstelle auf „0“ festgelegt werden. 
  - Die Erweiterung „Erweiterte Schlüsselverwendung“ (Extended Key Usage, EKU) muss vorhanden sein und den minimalen Satz an Objektbezeichnern (Object Identifiers, OIDs) für erweiterte Schlüsselverwendung enthalten. Die „anyExtendedKeyUsage“-OID (2.5.29.37.0) darf nicht angegeben werden. 
  - Die Zertifikatsperrlisten-Verteilungspunkterweiterung (CRL Distribution Point, CDP) muss im Zertifizierungsstellenzertifikat des Zertifikatausstellers vorhanden sein.<br>
    > [!IMPORTANT]
    > Die CDP-Erweiterung ist in den OPC Vault-Zertifizierungsstellenzertifikaten enthalten. Trotzdem verwenden OPC UA-Geräte benutzerdefinierte Methoden zur Verteilung von Zertifikatsperrlisten.
  - Die Erweiterung des Stelleninformationszugriffs muss in den Zertifikaten für Abonnenten vorhanden sein.<br>
    > [!IMPORTANT]
    > Die Erweiterung des Stelleninformationszugriffs ist in den Zertifikaten für OPC Vault-Abonnenten vorhanden. Trotzdem verwenden OPC UA-Geräte benutzerdefinierte Methoden, um Informationen zur Zertifizierungsstelle (ZS) des Zertifikatausstellers zu verteilen.
- Es müssen genehmigte asymmetrische Algorithmen, Schlüssellängen, Hashfunktionen und Auffüllmodi verwendet werden.
  - RSA und SHA-2 sind die einzigen unterstützten Algorithmen.
  - RSA kann für die Verschlüsselung, den Schlüsselaustausch und Signaturen verwendet werden.
  - Für die RSA-Verschlüsselung dürfen nur die Auffüllmodi OAEP, RSA-KEM oder RSA-PSS verwendet werden.
  - Es sind Schlüssellängen von größer oder gleich 2.048 Bit erforderlich.
  - Verwenden Sie die SHA-2-Familie von Hashalgorithmen (SHA256, SHA384 und SHA512).
  - RSA-Stamm-ZS-Schlüssel mit einer typischen Gültigkeitsdauer von mehr als oder gleich 20 Jahren müssen 4.096 Bit oder größer sein.
  - Die Zertifizierungsstellenschlüssel der RSA-Zertifikataussteller müssen eine Größe von mindestens 2.048 Bit haben. Wenn das Ablaufdatum für das Zertifizierungsstellenzertifikat nach 2030 liegt, muss die Größe des Zertifizierungsstellenschlüssels mindestens 4.096 Bit betragen.
- Zertifikatgültigkeitsdauer
  - Zertifikate der Stammzertifizierungsstelle: Der maximale Zertifikatgültigkeitszeitraum für Stammzertifizierungsstellen darf 25 Jahre nicht überschreiten.
  - Zertifikate der untergeordneten Zertifizierungsstelle oder Online-Zertifizierungsstellenzertifikate des Zertifikatausstellers: Der maximale Zertifikatgültigkeitszeitraum für Zertifizierungsstellen, die online sind und nur Zertifikate für Abonnenten ausstellen, darf 6 Jahre nicht überschreiten. Bei diesen Zertifizierungsstellen darf der zugehörige private Signaturschlüssel nicht länger als 3 Jahre zum Ausstellen neuer Zertifikate verwendet werden.<br>
    > [!IMPORTANT]
    > Das Ausstellerzertifikat wird bei seiner Generierung im standardmäßigen OPC Vault-Microservice ohne externe Stammzertifizierungsstelle wie eine untergeordnete Online-Zertifizierungsstelle mit entsprechenden Anforderungen und Gültigkeitsdauern behandelt. Die Standardgültigkeitsdauer ist bei einer Schlüssellänge von größer oder gleich 2.048 Bit auf 5 Jahre festgelegt.
  - Alle asymmetrischen Schlüssel müssen eine maximale Gültigkeitsdauer von 5 Jahren und eine empfohlene Gültigkeitsdauer von 1 Jahr haben.<br>
    > [!IMPORTANT]
    > Standardmäßig sind die Gültigkeitsdauern der mit OPC Vault ausgestellten Anwendungszertifikate 2 Jahre, und sie sollten jedes Jahr ersetzt werden. 
  - Immer wenn ein Zertifikat erneuert wird, geschieht dies mit einem neuen Schlüssel.
- OPC UA-spezifische Erweiterungen in Anwendungsinstanzzertifikaten
  - Die Erweiterung „subjectAltName“ umfasst den Anwendungs-URI und die Hostnamen. Dazu könnten auch FQDN, IPv4- und IPv6-Adressen gehören.
  - Die Erweiterung „keyUsage“ umfasst „digitalSignature“, „nonRepudiation“, „keyEncipherment“ und „dataEncipherment“.
  - Die Erweiterung „extendedKeyUsage“ umfasst „serverAuth“ und „clientAuth“.
  - Die Erweiterung „authorityKeyIdentifier“ wird in signierten Zertifikaten angegeben.

### <a name="ca-keys-and-certificates-must-meet-minimum-requirements"></a>Schlüssel und Zertifikate der Zertifizierungsstelle (ZS) müssen die Mindestanforderungen erfüllen.

- **Private Schlüssel**: Die Größe von RSA-Schlüsseln muss mindestens 2.048 Bit betragen. Wenn das Ablaufdatum für das Zertifizierungsstellenzertifikat nach 2030 liegt, muss die Größe des Zertifizierungsstellenschlüssels mindestens 4.096 Bit betragen.
- **Gültigkeitsdauer**: Der maximale Zertifikatgültigkeitszeitraum für Zertifizierungsstellen, die online sind und nur Zertifikate für Abonnenten ausstellen, darf 6 Jahre nicht überschreiten. Bei diesen Zertifizierungsstellen darf der zugehörige private Signaturschlüssel nicht länger als 3 Jahre zum Ausstellen neuer Zertifikate verwendet werden.

### <a name="ca-keys-are-protected-using-hardware-security-modules"></a>Zertifizierungsstellenschlüssel werden mithilfe von Hardwaresicherheitsmodulen geschützt.

OPC Vault verwendet Azure Key Vault Premium, und Schlüssel werden durch FIPS 140-2 Level 2-Hardwaresicherheitsmodule (HSM) geschützt. 

Die kryptografischen Module, die Key Vault verwendet – egal, ob HSM oder Software – entsprechen FIPS. Schlüssel, die als durch HSM geschützt erstellt oder importiert wurden, werden in einem HSM verarbeitet und gemäß FIPS 140-2 Level 2 überprüft. Schlüssel, die als softwaregeschützt erstellt oder importiert wurden, werden in kryptografischen Modulen verarbeitet und gemäß FIPS 140-2 Level 1 überprüft.

## <a name="operational-practices"></a>Betriebliche Methoden

### <a name="document-and-maintain-standard-operational-pki-practices-for-certificate-enrollment"></a>Dokumentieren und Verwalten der standardmäßigen betrieblichen PKI-Methoden für Zertifikatregistrierung

Dokumentieren und verwalten Sie Standardvorgehensweisen (Standard Operational Procedures, SOPs) zur Ausstellung von Zertifikaten durch Zertifizierungsstellen, einschließlich: 
- Identifizierung und Authentifizierung des Abonnenten 
- Verarbeitung und Überprüfung der Zertifikatanforderung (falls zutreffend, beziehen Sie auch die Verarbeitung von Anforderungen zur Zertifikaterneuerung und erneuten Schlüsselerstellung mit ein). 
- Verteilung ausgestellter Zertifikate an die Abonnenten 

Die SOP des OPC Vault-Microservice wird in [OPC Vault-Architektur](overview-opc-vault-architecture.md) und [Verwalten des OPC Vault-Zertifikatdiensts](howto-opc-vault-manage.md) beschrieben. Die Vorgehensweisen folgen der „OPC Unified Architecture-Spezifikation, Teil 12: Ermittlung und globale Dienste“.


### <a name="document-and-maintain-standard-operational-pki-practices-for-certificate-revocation"></a>Dokumentieren und Verwalten der standardmäßigen betrieblichen PKI-Methoden für Zertifikatsperrung

Der Zertifikatsperrvorgang wird in [OPC Vault-Architektur](overview-opc-vault-architecture.md) und [Verwalten des OPC Vault-Zertifikatdiensts](howto-opc-vault-manage.md) beschrieben.
    
### <a name="document-ca-key-generation-ceremony"></a>Dokumentieren des Vorgangs zur Generierung des Zertifizierungsstellenschlüssels 

Die Generierung des Zertifizierungsstellenschlüssels des Zertifikatausstellers im OPC Vault-Microservice wird dank der sicheren Speicherung in Azure Key Vault vereinfacht. Weitere Informationen finden Sie unter [Verwalten des OPC Vault-Zertifikatdiensts](howto-opc-vault-manage.md).

Wenn Sie aber eine externe Stammzertifizierungsstelle verwenden, muss ein Vorgang zur Generierung des Zertifizierungsstellenschlüssels die nachstehend aufgeführten Anforderungen erfüllen.

Der Vorgangs zur Generierung des Zertifizierungsstellenschlüssels muss für ein dokumentiertes Skript ausgeführt werden, das mindestens die folgenden Elemente enthält: 
- Definition von Rollen und Teilnehmeraufgaben
- Genehmigung für die Durchführung des Vorgangs zur Generierung des Zertifizierungsstellenschlüssels
- Für den Vorgang erforderliche Kryptografiehardware und Aktivierungsmaterial
- Hardwarevorbereitung (einschließlich Aktualisieren und Freigabe von Informationen zu Assets/Konfiguration)
- Installation des Betriebssystems
- Bestimmte Schritte, die während des Vorgangs zur Generierung des Zertifizierungsstellenschlüssels ausgeführt werden, beispielsweise: 
  - Installation und Konfiguration der Zertifizierungsstellenanwendung
  - Generierung des Zertifizierungsstellenschlüssels
  - Sicherung des Zertifizierungsstellenschlüssels
  - Signieren des Zertifizierungsstellenzertifikats
  - Importieren von signierten Schlüsseln in das geschützte HSM des Diensts
  - Herunterfahren des Systems für die Zertifizierungsstelle
  - Vorbereitung von Material zur Speicherung


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie Sie OPC Vault sicher verwalten, können Sie sich jetzt über Folgendes informieren:

> [!div class="nextstepaction"]
> [Sichern von OPC UA-Geräten mit OPC Vault](howto-opc-vault-secure.md)

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
ms.openlocfilehash: f35836f60fae11c0955c128e96a4cea188681942
ms.sourcegitcommit: 4b8a69b920ade815d095236c16175124a6a34996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2019
ms.locfileid: "69997663"
---
# <a name="how-to-run-the-opc-vault-certificate-management-service-securely"></a>Sicheres Ausführen des OPC Vault-Zertifikatverwaltungsdiensts

In diesem Artikel wird erläutert, wie Sie den OPC Vault-Zertifikatverwaltungsdienst in Azure sicher ausführen. Außerdem werden andere zu berücksichtigende Sicherheitsrichtlinien beschrieben.

## <a name="roles"></a>Rollen

### <a name="trusted-and-authorized-roles"></a>Vertrauenswürdige und autorisierte Rollen

Der OPC Vault-Microservice ist so konfiguriert, dass unterschiedliche Rollen auf verschiedene Teile des Diensts zugreifen können.

> [!IMPORTANT]
> Während der Bereitstellung fügt das Skript nur den Benutzer hinzu, der das Bereitstellungsskript als Benutzer für alle Rollen ausführt.
> Diese Rollenzuweisung sollte für eine Produktionsbereitstellung überprüft und entsprechend den Richtlinien (siehe unten) neu konfiguriert werden.
> Diese Aufgabe erfordert die manuelle Zuweisung von Rollen und Diensten im Azure AD-Portal für Unternehmensanwendungen.

### <a name="certificate-management-service-roles"></a>Rollen des Zertifikatverwaltungsdiensts

Der OPC Vault-Microservice definiert die folgenden Rollen:

- **Leser:** Standardmäßig hat jeder authentifizierte Benutzer im Mandanten Lesezugriff. 
  - Lesezugriff auf Anwendungen und Zertifikatanforderungen. Kann Anwendungen und Zertifikatanforderungen auflisten und abfragen. Außerdem ist der Zugriff auf Geräteermittlungsinformationen und öffentliche Zertifikate mit Lesezugriff möglich.
- **Writer**: Die Rolle „Writer“ wird einem Benutzer zugewiesen, damit er Schreibberechtigungen für bestimmte Aufgaben hinzufügen kann. 
  - Lese-/Schreibzugriff auf Anwendungen und Zertifikatanforderungen. Kann Anwendungen registrieren, aktualisieren und deren Registrierung aufheben. Kann Zertifikatanforderungen erstellen sowie genehmigte private Schlüssel und Zertifikate abrufen. Kann private Schlüssel auch löschen.
- **Genehmigende Person**: Die Rolle „Genehmigende Person“ wird einem Benutzer zugewiesen, damit er Zertifikatanforderungen genehmigen oder ablehnen kann. Diese Rolle enthält keine andere Rolle.
  - Zusätzlich zur Rolle „Genehmigende Person“ für den Zugriff auf die API des OPC Vault-Microservice muss der Benutzer auch über die Schlüsselsignaturberechtigung in Key Vault verfügen, um die Zertifikate signieren zu können.
  - Die Rollen „Writer“ und „Genehmigende Person“ sollten verschiedenen Benutzern zugewiesen werden.
  - Die Hauptrolle von „Genehmigende Person“ ist die Genehmigung der Generierung und Ablehnung von Zertifikatanforderungen.
- **Administrator**: Die Rolle „Administrator“ wird einem Benutzer zugewiesen, damit er die Zertifikatgruppen verwalten kann. Diese Rolle unterstützt nicht die Rolle „Genehmigende Person“, enthält aber die Rolle „Writer“.
  - Der Administrator kann die Zertifikatgruppen verwalten, die Konfiguration ändern und Anwendungszertifikate durch Ausstellung einer neuen Zertifikatsperrliste widerrufen.
  - Im Idealfall werden die Rollen „Writer“, „Genehmigende Person“ und „Administrator“ verschiedenen Benutzern zugewiesen. Um die Sicherheit zu erhöhen, benötigt ein Benutzer mit der Rolle „Genehmigende Person“ oder „Administrator“ außerdem die Schlüsselsignaturberechtigung in KeyVault, um Zertifikate ausstellen oder ein Zertifizierungsstellenzertifikat des Zertifikatausstellers erneuern zu können.
  - Zusätzlich zur Rolle „Microservice-Verwaltung“ umfasst die Rolle auch, ist aber nicht darauf beschränkt:
    - Verantwortlich für die Verwaltung der Implementierung von Sicherheitspraktiken der Zertifizierungsstelle.
    - Verwaltung der Generierung, Sperrung und Aussetzung von Zertifikaten. 
    - Lebenszyklusverwaltung für Kryptografieschlüssel (z.B. die Erneuerung der Zertifizierungsstellenschlüssel des Zertifikatausstellers).
    - Installation, Konfiguration und Wartung von Diensten, die die Zertifizierungsstelle betreiben.
    - Täglicher Betrieb der Dienste. 
    - Sicherung und Wiederherstellung von Zertifizierungsstelle und Datenbank.

### <a name="other-role-assignments"></a>Andere Rollenzuweisungen

Die folgenden Rollen sollten bei der Ausführung des Diensts ebenfalls berücksichtigt und zugewiesen werden:

- Geschäftlicher Besitzer des Zertifikatbeschaffungsvertrags bei der externen Stammzertifizierungsstelle (für den Fall, dass der Besitzer Zertifikate von einer externen Zertifizierungsstelle erwirbt oder eine Zertifizierungsstelle betreibt, die einer externen Zertifizierungsstelle untergeordnet ist).
- Entwicklung und Validierung der Zertifizierungsstelle.
- Überprüfung von Überwachungsdatensätzen.
- Mitarbeiter, die bei der Unterstützung der Zertifizierungsstelle oder der Verwaltung der physischen Funktionen und Cloudfunktionen helfen, aber nicht direkt vertrauenswürdig sind, um Zertifizierungsstellenvorgänge auszuführen, befinden sich laut Definition in der autorisierten Rolle. Der Satz von Aufgaben, den Personen in der autorisierten Rolle ausführen dürfen, muss ebenfalls dokumentiert werden.

### <a name="memberships-of-trusted-and-authorized-roles-must-be-reviewed-annually"></a>Mitgliedschaften von vertrauenswürdigen und autorisierten Rollen müssen jährlich überprüft werden

Die Mitgliedschaft von vertrauenswürdigen und autorisierten Rollen muss mindestens vierteljährlich überprüft werden, um sicherzustellen, dass die Gruppe von Personen (bei manuellen Prozessen) oder von Dienstidentitäten (bei automatisierten Prozessen) in jeder Rolle auf ein Minimum beschränkt wird.

### <a name="certificate-issuance-process-must-enforce-role-separation-between-certificate-requester-and-approver"></a>Vorgang zur Zertifikatausstellung muss eine Rollentrennung zwischen „das Zertifikat anfordernde Person“ und „das Zertifikat genehmigende Person“ erzwingen

Der Vorgang zur Zertifikatausstellung muss eine Rollentrennung zwischen den Rollen „das Zertifikat anfordernde Person“ und „das Zertifikat genehmigende Person“ (Personen oder automatisierte Systeme) erzwingen. Die Zertifikatausstellung muss von einer Rolle „das Zertifikat genehmigende Person“ autorisiert werden, die überprüft, ob die das Zertifikat anfordernde Person zum Abrufen von Zertifikaten autorisiert ist. Die Personen, die die Rolle „das Zertifikat genehmigende Person“ haben, müssen formell autorisiert sein.

### <a name="privileged-role-management-must-restrict-access-and-be-reviewed-quarterly"></a>Verwaltung privilegierter Rollen muss Zugriff einschränken und vierteljährlich überprüft werden

Die Zuweisung privilegierter Rollen (z.B. Autorisierung der Mitgliedschaft in der Gruppe „Administratoren“ und „Genehmigende Personen“) muss auf eine begrenzte Menge von autorisierten Mitarbeitern beschränkt werden. Bei Änderungen privilegierter Rollen muss der Zugriff innerhalb von 24 Stunden widerrufen werden. Schließlich müssen Zuweisungen privilegierter Rollen vierteljährlich überprüft, und alle nicht benötigten oder abgelaufenen Zuweisungen müssen entfernt werden.

### <a name="privileged-roles-should-use-two-factor-authentication"></a>Privilegierte Rollen sollten die zweistufige Authentifizierung (Two-Factor Authentication, TFA) verwenden

Die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) muss für interaktive Anmeldungen von „Genehmigende Personen“ und „Administratoren“ beim Dienst verwendet werden.

## <a name="certificate-service-operation-guidelines"></a>Richtlinien für den Zertifikatdienstvorgang

### <a name="operational-contacts"></a>Betriebliche Kontakte

Der Zertifikatdienst muss einen aktuellen Security Response-Plan gespeichert haben, der detaillierte Kontakte für die Reaktion auf operative Probleme enthält.

### <a name="security-updates"></a>Sicherheitsupdates

Alle Systeme müssen kontinuierlich überwacht und mit den neuesten Sicherheitsupdates/der neuesten Patchkonformität aktualisiert werden.

> [!IMPORTANT]
> Das GitHub-Repository des OPC Vault-Diensts wird mit Sicherheitspatches kontinuierlich aktualisiert. Die Updates auf GitHub sollten überwacht und in regelmäßigen Abständen auf den Dienst angewendet werden.

### <a name="security-monitoring"></a>Sicherheitsüberwachung

Abonnieren oder implementieren Sie die entsprechende Sicherheitsüberwachung – indem Sie beispielsweise eine zentrale Überwachungslösung (z.B. Azure Security Center, O365-Überwachungslösung) abonnieren und entsprechend konfigurieren, um sicherzustellen, dass sicherheitsrelevante Ereignisse an die Überwachungslösung übertragen werden.

> [!IMPORTANT]
> Der OPC Vault-Dienst wird standardmäßig mit [Azure Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/devops) als Überwachungslösung bereitgestellt. Es wird dringend empfohlen, eine Sicherheitslösung wie [Azure Security Center](https://azure.microsoft.com/services/security-center/) hinzuzufügen.

### <a name="assess-security-of-open-source-software-components"></a>Bewerten der Sicherheit von Open-Source-Softwarekomponenten

Keine der in einem Produkt oder Dienst verwendeten Open-Source-Komponenten darf mittlere oder größere Sicherheitsrisiken aufweisen.

> [!IMPORTANT]
> Das GitHub-Repository des OPC Vault-Diensts überprüft alle Komponenten während Continuous Integration-Builds auf Sicherheitsrisiken. Die Updates auf GitHub sollten überwacht und in regelmäßigen Abständen auf den Dienst angewendet werden.

### <a name="maintain-an-inventory"></a>Verwalten eines Bestands

Ein Assetbestand muss für alle Produktionshosts (einschließlich persistenter virtueller Computer), Geräte, alle internen IP-Adressbereiche, VIPs und öffentliche DNS-Domänennamen verwaltet werden. Dieser Bestand muss beim Hinzufügen oder Entfernen eines Systems, einer Geräte-IP-Adresse, einer VIP oder einer öffentlichen DNS-Domäne innerhalb von 30 Tagen aktualisiert werden.

#### <a name="inventory-of-the-default-azure-opc-vault-microservice-production-deployment"></a>Bestand der Standardproduktionsbereitstellung für Azure OPC Vault-Microservice: 

In **Azure**:
- **App Service-Plan**: App Service-Plan für Diensthosts. Standard S1.
- **App Service** für Microservice: Der OPC Vault-Diensthost.
- **App Service** für die Beispielanwendung: Der OPC Vault-Host für die Beispielanwendung.
- **KeyVault Standard**: Zum Speichern von Geheimnissen und Cosmos DB-Schlüsseln für die Webdienste.
- **KeyVault Premium**: Zum Hosten der Zertifizierungsstellenschlüssel des Zertifikatausstellers, für den Signaturdienst, für Vault-Konfiguration und Speicherung privater Anwendungsschlüssel.
- **Cosmos DB**: Datenbank für Anwendungs- und Zertifikatanforderungen. 
- **Application Insights**: (optional) Überwachungslösung für den Webdienst und die Anwendung.
- **Azure AD-Anwendungsregistrierung**: Eine Registrierung für die Beispielanwendung, den Dienst und das Edge-Modul.

Für die Cloud-Dienste sollten alle Hostnamen, Ressourcengruppen, Ressourcennamen, die Abonnement-ID und TenantId, die für die Bereitstellung des Diensts verwendet werden, dokumentiert werden. 

In **IoT Edge** oder auf einem lokalen **IoT Edge-Server**:
- **OPC Vault-IoT Edge-Modul**: Zur Unterstützung eines globalen OPC UA-Ermittlungsservers im Factory-Netzwerk. 

Bei den IoT Edge-Geräten sollten die Hostnamen und IP-Adressen dokumentiert werden. 

### <a name="document-the-certification-authorities-cas"></a>Dokumentieren der Zertifizierungsstellen (Certification Authorities, CAs)

Die Dokumentation der Zertifizierungsstellenhierarchie muss alle betriebenen Zertifizierungsstellen enthalten, einschließlich aller zugehörigen untergeordneten Zertifizierungsstellen, übergeordneten Zertifizierungsstellen und Stammzertifizierungsstellen – selbst wenn sie vom Dienst nicht verwaltet werden. Anstelle der formalen Dokumentation wird möglicherweise ein umfassender Satz aller nicht abgelaufenen Zertifizierungsstellenzertifikate bereitgestellt.

> [!NOTE]
> Die OPC Vault-Beispielanwendung unterstützt den Download aller Zertifikate, die im Dienst für die Dokumentation verwendet und erstellt wurden.

### <a name="document-the-issued-certificates-by-all-certification-authorities-cas"></a>Dokumentieren der ausgestellten Zertifikate von allen Zertifizierungsstellen (Certification Authorities, CAs)

Ein vollständiger Satz aller Zertifikate, die in den letzten 12 Monaten ausgestellt wurden, sollte für die Dokumentation bereitgestellt werden.

> [!NOTE]
> Die OPC Vault-Beispielanwendung unterstützt den Download aller Zertifikate, die im Dienst für die Dokumentation verwendet und erstellt wurden.

### <a name="document-the-sop-for-securely-deleting-cryptographic-keys"></a>Dokumentieren des SOP zum sicheren Löschen von Kryptografieschlüsseln

Eine Schlüssellöschung erfolgt während der Gültigkeitsdauer einer Zertifizierungsstelle möglicherweise nur selten. Aus diesem Grund wird keinem Benutzer das Recht „KeyVault-Zertifikat löschen“ zugewiesen, und es werden keine APIs zum Löschen eines Zertifizierungsstellenzertifikats des Zertifikatausstellers verfügbar gemacht. Das manuelle Standardverfahren zum sicheren Löschen von Kryptografieschlüsseln für Zertifizierungsstellen steht nur zur Verfügung, indem auf den KeyVault im Azure-Portal direkt zugegriffen und die Zertifikatsgruppe in KeyVault gelöscht wird. Zum Sicherstellen der sofortigen Löschung sollte [vorläufiges KeyVault-Löschen](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete) deaktiviert werden.

## <a name="certificates"></a>Zertifikate

### <a name="certificates-must-comply-with-minimum-certificate-profile"></a>Zertifikate müssen dem minimalen Zertifikatprofil entsprechen

Der OPC Vault-Dienst ist eine Online-Zertifizierungsstelle (CA), die Endeinheitzertifikate für Abonnenten ausstellt.
Der OPC Vault-Microservice hält diese Richtlinien in der Standardimplementierung ein.

- Alle Zertifikate müssen die folgenden (unten angegebenen) X.509-Felder enthalten:
  - Der Inhalt des Felds „Version“ muss „v3“ lauten. 
  - Der Inhalt des Felds „serialNumber“ muss mindestens 8 Byte Entropie enthalten, die aus einem von FIPS (Federal Information Processing Standards) 140 genehmigten Zufallszahlengenerator abgerufen wurden.<br>
    > [!IMPORTANT]
    > Die Seriennummer des OPC Vault beträgt standardmäßig 20 Byte und wird vom Generator für kryptografische Zufallszahlen des Betriebssystems abgerufen. Der Zufallszahlengenerator ist von FIPS 140 auf Windows-Geräten genehmigt, jedoch nicht auf Linux-Varianten. Diese Tatsache muss berücksichtigt werden, wenn eine Dienstbereitstellung ausgewählt wird, bei der Linux-VMs oder Linux-Docker-Container verwendet werden, für die die zugrunde liegende Technologie OpenSSL von FPS 140 nicht genehmigt wurde.
  - Die Felder „IssuerUniqueID“ und „subjectUniqueID“ dürfen nicht vorhanden sein.
  - Endeinheitzertifikate müssen in Übereinstimmung mit IETF RFC 5280 anhand der Erweiterung für Basiseinschränkungen identifiziert werden.
  - Das Feld „pathLenConstraint“ muss für das Zertifikat der ausstellenden Zertifizierungsstelle auf „0“ festgelegt werden. 
  - Die Erweiterung „Erweiterte Schlüsselverwendung“ (Extended Key Usage, EKU) muss vorhanden sein und den minimalen Satz an Objektbezeichnern (Object Identifiers, OIDs) für erweiterte Schlüsselverwendung enthalten. Die „anyExtendedKeyUsage“-OID (2.5.29.37.0) darf nicht angegeben werden. 
  - Die Zertifikatsperrlisten-Verteilungspunkterweiterung (CRL Distribution Point, CDP) muss im Zertifizierungsstellenzertifikat des Zertifikatausstellers vorhanden sein.<br>
    > [!IMPORTANT]
    > Obwohl die Zertifikatsperrlisten-Verteilungspunkterweiterung in OPC Vault-Zertifizierungsstellenzertifikaten vorhanden ist, verwenden OPC UA-Geräte benutzerdefinierte Methoden zum Verteilen von Zertifikatsperrlisten.
  - Die Erweiterung des Stelleninformationszugriffs muss in den Zertifikaten für Abonnenten vorhanden sein.<br>
    > [!IMPORTANT]
    > Obwohl die Erweiterung des Stelleninformationszugriffs in Zertifikaten für OPC Vault-Abonnenten vorhanden ist, verwenden OPC UA-Geräte benutzerdefinierte Methoden zum Verteilen von Zertifizierungsstelleninformationen des Zertifikatausstellers.
- Es müssen genehmigte asymmetrische Algorithmen, Schlüssellängen, Hashfunktionen und Auffüllmodi verwendet werden.
  - **RSA** und **SHA-2** sind die einzigen unterstützten Algorithmen (*).
  - RSA kann für die Verschlüsselung, den Schlüsselaustausch und Signaturen verwendet werden.
  - Für die RSA-Verschlüsselung dürfen nur die Auffüllmodi OAEP, RSA-KEM oder RSA-PSS verwendet werden.
  - Schlüssellängen >= 2048 Bit sind erforderlich.
  - Verwenden Sie die SHA-2-Familie von Hashalgorithmen (SHA256, SHA384 und SHA512).
  - RSA-Schlüssel der Stammzertifizierungsstelle mit einer typischen Gültigkeitsdauer >= 20 Jahre müssen eine Größe von mindestens 4096 Bit haben.
  - Die Größe der RSA-Zertifizierungsstellenschlüssel des Zertifikatausstellers muss mindestens 2048 Bit betragen. Bei einem Ablaufdatum des Zertifizierungsstellenzertifikats nach 2030 muss sie mindestens 4096 Bit betragen.
- Zertifikatgültigkeitsdauer
  - Zertifikate der Stammzertifizierungsstelle: Der maximale Zertifikatgültigkeitszeitraum für Stammzertifizierungsstellen darf 25 Jahre nicht überschreiten.
  - Zertifikate der untergeordneten Zertifizierungsstelle oder Online-Zertifizierungsstellenzertifikate des Zertifikatausstellers: Der maximale Zertifikatgültigkeitszeitraum für Zertifizierungsstellen, die online sind und nur Zertifikate für Abonnenten ausstellen, darf sechs Jahre nicht überschreiten. Bei diesen Zertifizierungsstellen darf der zugehörige private Signaturschlüssel nicht länger als drei Jahre zum Ausstellen neuer Zertifikate verwendet werden.<br>
    > [!IMPORTANT]
    > Das Ausstellerzertifikat wird bei seiner Generierung im standardmäßigen OPC Vault-Microservice ohne externe Stammzertifizierungsstelle wie eine untergeordnete Online-Zertifizierungsstelle mit entsprechenden Anforderungen und Gültigkeitsdauern behandelt. Die Standardgültigkeitsdauer ist auf fünf Jahre mit einer Schlüssellänge >= 2048 festgelegt.
  - Alle asymmetrischen Schlüssel müssen eine maximale Gültigkeitsdauer von fünf Jahren haben; die empfohlene Gültigkeitsdauer ist ein Jahr.<br>
    > [!IMPORTANT]
    > Standardmäßig sind die Gültigkeitsdauern der mit OPC Vault ausgestellten Anwendungszertifikate zwei Jahre, und sie sollten jedes Jahr ersetzt werden. 
  - Immer wenn ein Zertifikat erneuert wird, geschieht dies mit einem neuen Schlüssel.
- OPC UA-spezifische Erweiterungen in Anwendungsinstanzzertifikaten
  - Die Erweiterung „subjectAltName“ umfasst die Anwendungs-URI und Hostnamen, die außerdem FQDN, IPv4- und IPv6-Adressen enthalten können.
  - Die Erweiterung „keyUsage“ umfasst „digitalSignature“, „nonRepudiation“, „keyEncipherment“ und „dataEncipherment“.
  - Die Erweiterung „extendedKeyUsage“ umfasst „serverAuth“ und/oder „clientAuth“.
  - Die Erweiterung „authorityKeyIdentifier“ wird in signierten Zertifikaten angegeben.

### <a name="certificate-authority-ca-keys-and-certificates-must-meet-minimum-requirements"></a>Schlüssel und Zertifikate der Zertifizierungsstelle (Certificate Authority, CA) müssen die Mindestanforderungen erfüllen

- **Private Schlüssel**: Die Größe von **RSA**-Schlüsseln muss mindestens 2048 Bit betragen; wenn das Ablaufdatum für das Zertifizierungsstellenzertifikat nach 2030 liegt, muss die Größe des Zertifizierungsstellenschlüssels mindestens 4096 Bit betragen.
- **Gültigkeitsdauer**: Der maximale Zertifikatgültigkeitszeitraum für Zertifizierungsstellen, die online sind und nur Zertifikate für Abonnenten ausstellen, darf sechs Jahre nicht überschreiten. Bei diesen Zertifizierungsstellen darf der zugehörige private Signaturschlüssel nicht länger als drei Jahre zum Ausstellen neuer Zertifikate verwendet werden.

### <a name="ca-keys-are-protected-using-hardware-security-modules-hsm"></a>Zertifizierungsstellenschlüssel werden mithilfe von Hardwaresicherheitsmodulen (HSM) geschützt

- OPC Vault verwendet Azure KeyVault Premium, und Schlüssel werden durch FIPS 140-2 Level 2-Hardwaresicherheitsmodule (HSM) geschützt. 

Die kryptografischen Module, die Key Vault verwendet – sowohl HSM als auch Software – sind FIPS-geprüft (Federal Information Processing Standards).<br>
Schlüssel, die als durch HSM geschützt erstellt oder importiert wurden, werden in einem HSM verarbeitet und gemäß FIPS 140-2 Level 2 überprüft.<br>
Schlüssel, die als softwaregeschützt erstellt oder importiert wurden, werden in kryptografischen Modulen verarbeitet und gemäß FIPS 140-2 Level 1 überprüft.

## <a name="operational-practices"></a>Betriebliche Methoden

### <a name="document-and-maintain-standard-operational-pki-practices-for-certificate-enrollment"></a>Dokumentieren und Verwalten der standardmäßigen betrieblichen PKI-Methoden für Zertifikatregistrierung

Dokumentieren und verwalten Sie Standardvorgehensweisen (Standard Operational Procedures, SOPs) zur Ausstellung von Zertifikaten durch Zertifizierungsstellen, einschließlich: 
- Identifizierung und Authentifizierung des Abonnenten 
- Verarbeitung und Überprüfung der Zertifikatanforderung (falls zutreffend, beziehen Sie auch die Verarbeitung von Anforderungen zur Zertifikaterneuerung und erneuten Schlüsselerstellung mit ein) 
- Verteilung ausgestellter Zertifikate an die Abonnenten 

Die SOP des OPC Vault-Microservice wird in der [Übersicht](overview-opc-vault-architecture.md) und in den Dokumenten zur [Vorgehensweise zum Verwalten](howto-opc-vault-manage.md) beschrieben. Die Vorgehensweisen folgen der OPC Unified Architecture-Spezifikation, Teil 12: Ermittlung und globale Dienste.


### <a name="document-and-maintain-standard-operational-pki-practices-for-certificate-revocation"></a>Dokumentieren und Verwalten der standardmäßigen betrieblichen PKI-Methoden für Zertifikatsperrung

Der Zertifikatsperrvorgang wird in der [Übersicht](overview-opc-vault-architecture.md) und in den Dokumenten zur [Vorgehensweise zum Verwalten](howto-opc-vault-manage.md) beschrieben.
    
### <a name="document-certification-authority-ca-key-generation-ceremony"></a>Dokumentieren des Vorgangs zur Generierung des Zertifizierungsstellenschlüssels 

Die Generierung des Zertifizierungsstellenschlüssels des Zertifikatausstellers im OPC Vault-Microservice wird aufgrund der sicheren Speicherung in Azure KeyVault vereinfacht und ist in der Dokumentation zur [Vorgehensweise zum Verwalten](howto-opc-vault-manage.md) beschrieben.

Wenn aber eine externe Stammzertifizierungsstelle verwendet wird, muss ein Vorgang zur Generierung des Zertifizierungsstellenschlüssels die folgenden Anforderungen erfüllen:

Der Vorgangs zur Generierung des Zertifizierungsstellenschlüssels muss für ein dokumentiertes Skript ausgeführt werden, das mindestens die folgenden Elemente enthält: 
1. Definition von Rollen und Teilnehmeraufgaben
2. Genehmigung für die Durchführung des Vorgangs zur Generierung des Zertifizierungsstellenschlüssels
3. Für den Vorgang erforderliche Kryptografiehardware und Aktivierungsmaterial
4. Hardwarevorbereitung (einschließlich Aktualisieren und Freigabe von Informationen zu Assets/Konfiguration)
5. Installation des Betriebssystems
6. Bestimmte Schritte, die während des Vorgangs zur Generierung des Zertifizierungsstellenschlüssels ausgeführt werden, beispielsweise: 
7. Installation und Konfiguration der Zertifizierungsstellenanwendung
8. Generierung des Zertifizierungsstellenschlüssels
9. Sicherung des Zertifizierungsstellenschlüssels
10. Signatur des Zertifizierungsstellenzertifikats
9. Importieren von signierten Schlüsseln in das geschützte HSM des Diensts
11. Herunterfahren des Systems für die Zertifizierungsstelle
12. Vorbereitung von Material zur Speicherung


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie Sie OPC Vault sicher verwalten, lautet unser Vorschlag für den nächsten Schritt:

> [!div class="nextstepaction"]
> [Sichern von OPC UA-Geräten mit OPC Vault](howto-opc-vault-secure.md)
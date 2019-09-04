---
title: OPC Vault-Architektur – Azure | Microsoft-Dokumentation
description: OPC Vault-Architektur für Zertifikatverwaltungsdienst
author: mregen
ms.author: mregen
ms.date: 08/16/2019
ms.topic: overview
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 9331473402ddd22180df3b404824969360d48164
ms.sourcegitcommit: 4b8a69b920ade815d095236c16175124a6a34996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2019
ms.locfileid: "69995813"
---
# <a name="opc-vault-architecture"></a>OPC Vault-Architektur

Dieser Artikel enthält eine Übersicht über den **OPC Vault-Microservice** und das **IoT Edge-Modul von OPC Vault**.

## <a name="overview"></a>Übersicht

Für OPC UA-Anwendungen werden Anwendungsinstanzzertifikate verwendet, um für Sicherheit auf Anwendungsebene zu sorgen. Eine sichere Verbindung wird mit asymmetrischer Kryptografie hergestellt, für die die Anwendungszertifikate das Schlüsselpaar mit einem öffentlichem und einem privatem Schlüssel bereitstellen. Die Zertifikate können selbstsigniert oder von einer Zertifizierungsstelle (Certificate Authority, CA) signiert sein.

Eine OPC UA-Anwendung verfügt über eine Liste mit vertrauenswürdigen Zertifikaten, die für die als vertrauenswürdig angesehenen Anwendungen gelten. Diese Zertifikate können selbstsigniert oder von einer Zertifizierungsstelle signiert sein, oder es kann sich dabei selbst um Stamm- oder Unterzertifizierungsstellen handeln. Wenn ein vertrauenswürdiges Zertifikat Teil einer größeren Vertrauenskette ist, vertraut die Anwendung allen Zertifikaten, die mit dem Zertifikat in der Vertrauensliste verknüpft sind, sofern die vollständige Zertifikatskette überprüft werden kann.

Der Hauptunterschied zwischen dem Ansehen von selbstsignierten Zertifikaten und eines Zertifizierungsstellenzertifikats als vertrauenswürdig ist der Installationsaufwand, der für die Bereitstellung und Verwaltung einer Vertrauensstellung erforderlich ist, und der zusätzliche Aufwand zum Hosten einer unternehmensspezifischen Zertifizierungsstelle. 

Um eine Vertrauensstellung für selbstsignierte Zertifikate für N Server mit einer einzelnen Clientanwendung zu verteilen, müssen alle N Serveranwendungszertifikate in der Vertrauensliste der Clientanwendung installiert sein. Außerdem muss das Clientanwendungszertifikat in allen Vertrauenslisten der Serveranwendung installiert sein. Der Verwaltungsaufwand ist hierbei relativ hoch und steigt noch weiter an, wenn Zertifikatlebensdauern berücksichtigt und Zertifikate verlängert werden müssen.

Die Verwendung einer unternehmensspezifischen Zertifizierungsstelle kann die Verwaltung der Vertrauensstellung mit mehreren Servern und Clients deutlich vereinfachen. In diesem Fall generiert der Administrator einmal für jeden verwendeten Client und Server ein Anwendungsinstanzzertifikat, das von der Zertifizierungsstelle signiert ist. Darüber hinaus wird das Zertifizierungsstellenzertifikat in jeder Anwendungsvertrauensliste auf allen Servern und Clients installiert. Bei diesem Ansatz müssen nur abgelaufene Zertifikate verlängert und für die betroffenen Anwendungen ersetzt werden.

Der Azure Industrial IoT-OPC UA-Zertifikatverwaltungsdienst ist unsere Lösung für die Verwaltung einer unternehmensspezifischen Zertifizierungsstelle für OPC UA-Anwendungen, die auf dem OPC Vault-Microservice basiert.

Mit OPC Vault wird ein Microservice zum Hosten einer unternehmensspezifischen Zertifizierungsstelle in einer sicheren Cloud bereitgestellt, die auf geschützten Azure AD-Diensten mit Azure Key Vault (einschließlich Hardwaresicherheitsmodulen), Cosmos DB und optional auch IoT Hub als Anwendungsspeicher basieren.

Der OPC Vault-Microservice ist so konzipiert, dass ein rollenbasierter Workflow unterstützt wird. Hierbei fordern OT-Mitarbeiter signierte Anwendungszertifikate an, und Sicherheitsadministratoren und genehmigende Personen mit Signaturrechten in Azure Key Vault genehmigen diese Anforderungen bzw. lehnen sie ab.

Zur Sicherstellung der Kompatibilität mit vorhandenen OT-Lösungen auf OPC UA-GDS-Basis verfügen die Dienste über Unterstützung für ein Edge-Modul mit OPC Vault-Microservice. Hiermit wird die *Schnittstelle für den globalen OPC UA Discovery Server und die Zertifikatverwaltung* implementiert, um Zertifikate und Vertrauenslisten gemäß Teil 12 der Spezifikation zu verteilen. Nach unserem Kenntnisstand wird diese GDS-Serverschnittstelle noch nicht viel genutzt, und die Funktionalität ist noch eingeschränkt (Rolle „Leser“). [Bei Bedarf (Kundenanfrage) können wir die Benutzerfreundlichkeit verbessern (*)](#yet-unsupported-features).

## <a name="architecture"></a>Architecture

Die Architektur basiert auf dem OPC Vault-Microservice mit einem OPC Vault-IoT Edge-Modul für das Fabriknetzwerk und einer Beispiel-Webbenutzeroberfläche zum Steuern des Workflows:

![OPC Vault-Architektur](media/overview-opc-vault-architecture/opc-vault.png)

## <a name="opc-vault-microservice"></a>OPC Vault-Microservice

Der OPC Vault-Microservice umfasst die folgenden Schnittstellen zum Implementieren des Workflows für die Verteilung und Verwaltung einer unternehmensspezifischen Zertifizierungsstelle für OPC UA-Anwendungen:

### <a name="application"></a>Anwendung 
- Eine „OPC UA-Anwendung“ kann als Server, Client oder beides fungieren. OPC Vault dient in diesem Fall als Stelle für die Anwendungsregistrierung. 
- Neben den grundlegenden Vorgängen zum Registrieren, Aktualisieren und Aufheben der Registrierung von Anwendungen sind auch Schnittstellen vorhanden, mit denen nach Anwendungen mit Suchbegriffen gesucht werden kann und Abfragen durchgeführt werden können. 
- In den Zertifikatanforderungen muss auf eine gültige Anwendung verwiesen werden, um eine Anforderung zu verarbeiten und ein signiertes Zertifikat mit allen OPC UA-spezifischen Erweiterungen auszustellen. 
- Der Anwendungsdienst basiert entweder auf einer Cosmos DB-Datenbank oder der [OpcTwin-Geräteregistrierung (*)](#yet-unsupported-features). Dies hängt von der jeweiligen Kundenkonfiguration ab.

### <a name="certificate-group"></a>Zertifikatgruppe
- Eine Zertifikatgruppe ist eine Entität, in der ein Zertifikat einer Stamm- oder Unterzertifizierungsstelle gespeichert ist, einschließlich des privaten Schlüssels zum Signieren von Zertifikaten. 
- Die Länge des RSA-Schlüssels und des SHA-2-Hashwerts sowie die jeweiligen Lebensdauern können sowohl für Zertifikate des Zertifikatausstellers als auch für signierte Anwendungszertifikate konfiguriert werden. 
- Mehrere Gruppen können in einem einzelnen Dienst gehostet werden, um zukünftige Erweiterungen mit HTTPS-, Benutzer- oder ECC-Algorithmuszertifikatgruppen zu ermöglichen [(*)](#yet-unsupported-features). 
- Die Zertifizierungsstellenzertifikate werden in Azure Key Vault mit Verwendung von Hardwaresicherheitsmodulen (HSMs) vom Typ FIPS 140-2 Level 2 gespeichert. Der private Schlüssel verlässt den sicheren Speicher niemals, weil das Signieren über einen per Azure AD geschützten Key Vault-Vorgang erfolgt. 
- Die Zertifizierungsstellenzertifikate können im Laufe der Zeit verlängert werden und verbleiben aufgrund des Key Vault-Verlaufs weiterhin im sicheren Speicher. 
- Die Sperrliste für jedes Zertifizierungsstellenzertifikat wird in Key Vault ebenfalls als Geheimnis gespeichert. Nachdem die Registrierung einer Anwendung aufgehoben wurde, wird auch das Anwendungszertifikat in der Zertifikatsperrliste von einem Administrator widerrufen (gesperrt).
- Für das Sperren von Zertifikaten wird der Batch- und der Einzelmodus unterstützt.

### <a name="certificate-request"></a>Zertifikatanforderung
Mit einer Zertifikatanforderung wird der Workflow zum Generieren eines neuen Schlüsselpaars oder eines signierten Zertifikats per „Zertifikatsignieranforderung“ (Certificate Signing Request, CSR) für eine OPC UA-Anwendung implementiert. 
- Die Anforderung wird in einer Datenbank mit den zugehörigen Informationen gespeichert, z. B. Antragsteller oder Zertifikatsignieranforderung und Verweis auf die OPC UA-Anwendung. 
- Mit der Geschäftslogik des Diensts wird die Anforderung anhand der Informationen überprüft, die in der Anwendungsdatenbank gespeichert sind. Der Anwendungs-URI in der Datenbank muss mit dem Anwendungs-URI in der Zertifikatsignieranforderung übereinstimmen.
- Ein Sicherheitsadministrator mit Rechten zum Signieren (Rolle „Genehmigende Person“) genehmigt die Anforderung oder lehnt sie ab. Wenn die Anforderung genehmigt wird, wird ein neues Schlüsselpaar bzw. ein signiertes Zertifikat generiert. Der neue private Schlüssel wird sicher in Key Vault gespeichert, und das neue signierte öffentliche Zertifikat wird in der Datenbank für Zertifikatanforderungen abgelegt.
- Die anfordernde Person kann den Status der Anforderung abfragen, bis diese genehmigt oder abgelehnt wurde. Wenn die Anforderung genehmigt wurde, können der private Schlüssel und das Zertifikat heruntergeladen und im Zertifikatspeicher der OPC UA-Anwendung installiert werden.
- Die anfordernde Person kann die Anforderung jetzt akzeptieren, damit nicht benötigte Informationen aus der Anforderungsdatenbank gelöscht werden. 

Während der Lebensdauer eines signierten Zertifikats kann es vorkommen, dass eine Anwendung gelöscht oder ein Schlüssel kompromittiert wird. In diesem Fall hat ein Zertifizierungsstellen-Manager folgende Möglichkeiten:
- Löschen einer Anwendung, wobei gleichzeitig auch alle ausstehenden und genehmigten Zertifikatanforderungen der Anwendung gelöscht werden. 
- Eine andere Möglichkeit ist das Löschen nur einer Zertifikatanforderung, falls nur ein Schlüssel verlängert wird oder kompromittiert ist.
- Kompromittierte genehmigte und akzeptierte Zertifikatanforderungen werden jetzt als gelöscht gekennzeichnet.
- Ein Manager kann regelmäßig eine Verlängerung der Zertifikatsperrliste der ausstellenden Zertifizierungsstelle durchführen. Zum Verlängerungszeitpunkt werden alle gelöschten Zertifikatanforderungen widerrufen, und die Seriennummern der Zertifikate werden der Zertifikatsperrliste hinzugefügt. Widerrufene Zertifikatanforderungen werden als „Gesperrt“ gekennzeichnet.
- In dringenden Fällen können auch einzelne Zertifikatanforderungen widerrufen werden.
- Die aktualisierten Zertifikatsperrlisten sind dann für die Verteilung an die betreffenden OPC UA-Clients und -Server bereit.

Weitere Informationen zur OPC Vault-Microservice-API finden Sie in der Swagger-Dokumentation zum Microservice.

## <a name="opc-vault-iot-edge-module"></a>OPC Vault-IoT Edge-Modul
Zur Unterstützung des globalen Discovery Servers für Fabriknetzwerke kann das OPC Vault-Modul im Edgebereich bereitgestellt, als lokale .NET Core-Anwendung ausgeführt oder in einem Docker-Container gestartet werden. Aufgrund einer fehlenden Auth2-Authentifizierungsunterstützung im aktuellen OPC UA .NET Standard-Stapel ist die Funktionalität des OPC Vault-Edge-Moduls auf die Rolle „Leser“ beschränkt. Es ist nämlich nicht möglich, im Edge-Modul die Identität eines Benutzers für den Microservice anzunehmen, indem die GDS-Standardschnittstelle von OPC UA verwendet wird. An diesem Punkt sind nur Vorgänge zulässig, für die nicht die Rolle „Verfasser“, „Administrator“ oder „Genehmigende Person“ erforderlich ist[(*)](#yet-unsupported-features). 

## <a name="yet-unsupported-features"></a>Noch nicht unterstützte Funktionen

Mit **(*)** wird angegeben, dass noch keine Unterstützung vorhanden ist.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun über die OPC Vault-Architektur informiert haben, schlagen wir Folgendes als nächsten Schritt vor:

> [!div class="nextstepaction"]
> [Erstellen und Bereitstellen von OPC Vault](howto-opc-vault-deploy.md)

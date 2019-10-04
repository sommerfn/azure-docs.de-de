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
ms.openlocfilehash: 1e08968034134e2b9ab3b8064387d18663d5c866
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71200146"
---
# <a name="opc-vault-architecture"></a>OPC Vault-Architektur

Dieser Artikel enthält eine Übersicht über den OPC Vault-Microservice und das IoT Edge-Modul von OPC Vault.

Für OPC UA-Anwendungen werden Anwendungsinstanzzertifikate verwendet, um für Sicherheit auf Anwendungsebene zu sorgen. Per asymmetrischer Kryptografie wird eine sichere Verbindung hergestellt, für die die Anwendungszertifikate das Schlüsselpaar mit einem öffentlichen und einem privaten Schlüssel bereitstellen. Die Zertifikate können selbstsigniert oder von einer Zertifizierungsstelle (Certificate Authority, CA) signiert sein.

Eine OPC UA-Anwendung verfügt über eine Liste mit vertrauenswürdigen Zertifikaten, die für die als vertrauenswürdig angesehenen Anwendungen gelten. Diese Zertifikate können selbstsigniert oder von einer Zertifizierungsstelle signiert sein, oder es kann sich dabei selbst um Stamm- oder Unterzertifizierungsstellen handeln. Wenn ein vertrauenswürdiges Zertifikat Teil einer größeren Vertrauenskette ist, vertraut die Anwendung allen Zertifikaten, die mit dem Zertifikat in der Vertrauensliste verknüpft sind. Dies gilt, sofern die vollständige Vertrauenskette der Zertifikate überprüft werden kann.

Der Hauptunterschied zwischen dem Ansehen von selbstsignierten Zertifikaten und eines Zertifizierungsstellenzertifikats als vertrauenswürdig ist der Installationsaufwand, der für die Bereitstellung und Verwaltung einer Vertrauensstellung erforderlich ist. Es fällt auch zusätzlicher Aufwand zum Hosten einer unternehmensspezifischen Zertifizierungsstelle an. 

Um eine Vertrauensstellung für selbstsignierte Zertifikate für mehrere Server mit einer einzelnen Clientanwendung zu verteilen, müssen alle Serveranwendungszertifikate in der Vertrauensliste der Clientanwendung installiert sein. Darüber hinaus müssen Sie das Clientanwendungszertifikat in allen Vertrauenslisten der Serveranwendung installieren. Dieser Verwaltungsaufwand ist relativ hoch und nimmt sogar noch zu, wenn Sie sich auch um die Lebensdauer und Verlängerung von Zertifikaten kümmern müssen.

Die Verwendung einer unternehmensspezifischen Zertifizierungsstelle kann die Verwaltung der Vertrauensstellung mit mehreren Servern und Clients deutlich vereinfachen. In diesem Fall generiert der Administrator einmal für jeden verwendeten Client und Server ein Anwendungsinstanzzertifikat, das von der Zertifizierungsstelle signiert ist. Darüber hinaus wird das Zertifizierungsstellenzertifikat in jeder Anwendungsvertrauensliste auf allen Servern und Clients installiert. Bei diesem Ansatz müssen nur abgelaufene Zertifikate verlängert und für die betroffenen Anwendungen ersetzt werden.

Der Azure Industrial IoT-OPC UA-Zertifikatverwaltungsdienst unterstützt Sie beim Verwalten einer unternehmensspezifischen Zertifizierungsstelle für OPC UA-Anwendungen. Dieser Dienst basiert auf dem OPC Vault-Microservice. OPC Vault stellt einen Microservice bereit, um eine unternehmensspezifische Zertifizierungsstelle in einer sicheren Cloud zu hosten. Diese Lösung basiert auf Diensten, die per Azure Active Directory (Azure AD), Azure Key Vault mit Hardwaresicherheitsmodulen (HSMs), Azure Cosmos DB und optional IoT Hub als Anwendungsspeicher geschützt sind.

Der OPC Vault-Microservice ist so konzipiert, dass ein rollenbasierter Workflow unterstützt wird. Hierbei werden Anforderungen von Sicherheitsadministratoren und Personen mit Signaturrechten in Azure Key Vault genehmigt oder abgelehnt.

Zur Erzielung von Kompatibilität mit vorhandenen OPC UA-Lösungen verfügen die Dienste über Unterstützung für ein Edgemodul, das auf einem OPC Vault-Microservice basiert. Hiermit wird die **Schnittstelle für den globalen OPC UA Discovery Server und die Zertifikatverwaltung** implementiert, um Zertifikate und Vertrauenslisten gemäß Teil 12 der Spezifikation zu verteilen. 


## <a name="architecture"></a>Architecture

Die Architektur basiert auf dem OPC Vault-Microservice mit einem OPC Vault-IoT Edge-Modul für das Fabriknetzwerk und einer Beispiel-Webbenutzeroberfläche zum Steuern des Workflows:

![Diagramm: OPC Vault-Architektur](media/overview-opc-vault-architecture/opc-vault.png)

## <a name="opc-vault-microservice"></a>OPC Vault-Microservice

Der OPC Vault-Microservice umfasst die folgenden Schnittstellen zum Implementieren des Workflows für die Verteilung und Verwaltung einer unternehmensspezifischen Zertifizierungsstelle für OPC UA-Anwendungen.

### <a name="application"></a>Anwendung 
- Eine OPC UA-Anwendung kann als Server, Client oder beides fungieren. OPC Vault dient in diesem Fall als Stelle für die Anwendungsregistrierung. 
- Zusätzlich zu den grundlegenden Vorgängen zum Registrieren, Aktualisieren und Aufheben der Registrierung von Anwendungen sind auch Schnittstellen vorhanden, mit denen nach Anwendungen mit Suchbegriffen gesucht werden kann und Abfragen durchgeführt werden können. 
- In den Zertifikatanforderungen muss auf eine gültige Anwendung verwiesen werden, um eine Anforderung zu verarbeiten und ein signiertes Zertifikat mit allen OPC UA-spezifischen Erweiterungen auszustellen. 
- Der Anwendungsdienst basiert auf einer Datenbank in Azure Cosmos DB.

### <a name="certificate-group"></a>Zertifikatgruppe
- Eine Zertifikatgruppe ist eine Entität, in der ein Zertifikat einer Stamm- oder Unterzertifizierungsstelle gespeichert ist, einschließlich des privaten Schlüssels zum Signieren von Zertifikaten. 
- Die Länge des RSA-Schlüssels und des SHA-2-Hashwerts sowie die jeweiligen Lebensdauern können sowohl für Zertifikate des Zertifikatausstellers als auch für signierte Anwendungszertifikate konfiguriert werden. 
- Sie speichern die Zertifizierungsstellenzertifikate in Azure Key Vault basierend auf einem HSM vom Typ FIPS 140-2 Level 2. Der private Schlüssel verlässt den sicheren Speicher niemals, weil das Signieren über einen per Azure AD geschützten Key Vault-Vorgang erfolgt. 
- Sie können die Zertifizierungsstellenzertifikate im Laufe der Zeit verlängern und aufgrund des Key Vault-Verlaufs im sicheren Speicher belassen. 
- Die Sperrliste für jedes Zertifizierungsstellenzertifikat wird in Key Vault ebenfalls als Geheimnis gespeichert. Wenn die Registrierung einer Anwendung aufgehoben wird, wird auch das Anwendungszertifikat in der Zertifikatsperrliste von einem Administrator widerrufen (gesperrt).
- Sie können Zertifikate einzeln oder als Batch widerrufen.

### <a name="certificate-request"></a>Zertifikatanforderung
Mit einer Zertifikatanforderung wird der Workflow zum Generieren eines neuen Schlüsselpaars oder eines signierten Zertifikats per Zertifikatsignieranforderung (Certificate Signing Request, CSR) für eine OPC UA-Anwendung implementiert. 
- Die Anforderung wird in einer Datenbank mit den zugehörigen Informationen gespeichert, z. B. Antragsteller oder Zertifikatsignieranforderung und Verweis auf die OPC UA-Anwendung. 
- Mit der Geschäftslogik des Diensts wird die Anforderung anhand der Informationen überprüft, die in der Anwendungsdatenbank gespeichert sind. Der Anwendungs-URI in der Datenbank muss mit dem Anwendungs-URI in der Zertifikatsignieranforderung übereinstimmen.
- Ein Sicherheitsadministrator mit Rechten zum Signieren (Rolle „Genehmigende Person“) genehmigt die Anforderung oder lehnt sie ab. Wenn die Anforderung genehmigt wird, wird ein neues Schlüsselpaar oder ein signiertes Zertifikat generiert (oder beides). Der neue private Schlüssel wird sicher in Key Vault gespeichert, und das neue signierte öffentliche Zertifikat wird in der Datenbank für Zertifikatanforderungen abgelegt.
- Die anfordernde Person kann den Status der Anforderung abfragen, bis diese genehmigt oder abgelehnt wurde. Wenn die Anforderung genehmigt wurde, können der private Schlüssel und das Zertifikat heruntergeladen und im Zertifikatspeicher der OPC UA-Anwendung installiert werden.
- Die anfordernde Person kann die Anforderung jetzt akzeptieren, damit nicht benötigte Informationen aus der Anforderungsdatenbank gelöscht werden. 

Während der Lebensdauer eines signierten Zertifikats kann es vorkommen, dass eine Anwendung gelöscht oder ein Schlüssel kompromittiert wird. In diesem Fall hat ein Zertifizierungsstellen-Manager folgende Möglichkeiten:
- Löschen einer Anwendung, wobei gleichzeitig auch alle ausstehenden und genehmigten Zertifikatanforderungen der Anwendung gelöscht werden. 
- Löschen nur einer Zertifikatanforderung, falls nur ein Schlüssel verlängert wird oder kompromittiert ist.

Kompromittierte genehmigte und akzeptierte Zertifikatanforderungen werden jetzt als gelöscht gekennzeichnet.

Ein Manager kann die Zertifikatsperrliste der ausstellenden Zertifizierungsstelle regelmäßig verlängern. Zum Verlängerungszeitpunkt werden alle gelöschten Zertifikatanforderungen widerrufen, und die Seriennummern der Zertifikate werden der Zertifikatsperrliste hinzugefügt. Widerrufene Zertifikatanforderungen werden als „Gesperrt“ gekennzeichnet. In dringenden Fällen können auch einzelne Zertifikatanforderungen widerrufen werden.

Die aktualisierten Zertifikatsperrlisten sind dann für die Verteilung an die betreffenden OPC UA-Clients und -Server bereit.

## <a name="opc-vault-iot-edge-module"></a>OPC Vault-IoT Edge-Modul
Zur Unterstützung des globalen Ermittlungsservers eines Fabriknetzwerks können Sie das OPC Vault-Modul im Edgebereich bereitstellen. Führen Sie es als lokale .NET Core-Anwendung aus, oder starten Sie es in einem Docker-Container. Beachten Sie Folgendes: Aufgrund der fehlenden Auth2-Authentifizierungsunterstützung im aktuellen OPC UA-.NET Standard-Stapel ist die Funktionalität des OPC Vault-Edgemoduls auf die Rolle „Leser“ beschränkt. Für einen Benutzer kann über das Edgemodul kein Identitätswechsel für den Microservice erfolgen, indem die OPC UA-Standardschnittstelle des globalen Ermittlungsservers verwendet wird.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun über die OPC Vault-Architektur informiert haben, haben Sie folgende Möglichkeiten:

> [!div class="nextstepaction"]
> [Erstellen und Bereitstellen von OPC Vault](howto-opc-vault-deploy.md)

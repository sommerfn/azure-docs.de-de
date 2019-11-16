---
title: Einführung in Azure Storage – Cloudspeicher in Azure | Microsoft-Dokumentation
description: Azure Storage ist die Cloudspeicherlösung von Microsoft. Azure Storage stellt Speicher für Datenobjekte bereit, die hoch verfügbar, sicher, langlebig, stark skalierbar und redundant sind.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 06/20/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 612da322e0d248f9b0000c8f0006893b87b74e44
ms.sourcegitcommit: a22cb7e641c6187315f0c6de9eb3734895d31b9d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74084638"
---
# <a name="introduction-to-azure-storage"></a>Einführung in Azure Storage

Azure Storage ist die Cloudspeicherlösung von Microsoft für moderne Datenspeicherszenarien. Für Azure Storage werden ein stark skalierbarer Objektspeicher für Datenobjekte, ein Dateisystemdienst für die Cloud, ein Messagingspeicher für zuverlässiges Messaging und ein NoSQL-Speicher bereitgestellt. Für Azure Storage gilt Folgendes:

- **Robust und hoch verfügbar:** Mit Redundanz wird sichergestellt, dass Ihre Daten sicher sind, falls es zu vorübergehenden Hardwareausfällen kommt. Sie können sich auch für das Replizieren von Daten über Rechenzentren oder geografische Regionen hinweg entscheiden, um eine weitere Schutzebene vor lokalen Notfällen oder Naturkatastrophen zu schaffen. Daten, die auf diese Weise repliziert werden, sind bei einem unerwarteten Ausfall weiterhin hoch verfügbar.
- **Sicher:** Alle Daten, die in Azure Storage geschrieben werden, werden vom Dienst verschlüsselt. Bei Azure Storage können Sie genau steuern, wer Zugriff auf Ihre Daten hat.
- **Skalierbar:** Azure Storage ist auf hohe Skalierbarkeit ausgelegt, um die Datenspeicherungs- und Leistungsanforderungen heutiger Anwendungen zu erfüllen. 
- **Verwaltet:** Microsoft Azure übernimmt die Hardwarewartung, Updates und die Behandlung kritischer Probleme für Sie.
- **Zugänglich:** Auf Daten in Azure Storage kann weltweit per HTTP oder HTTPS zugegriffen werden. Microsoft stellt Clientbibliotheken für Azure Storage in verschiedenen Sprachen (z. B. .NET, Java, Node.js, Python, PHP, Ruby und Go) sowie eine ausgereifte REST-API bereit. Azure Storage unterstützt die Skripterstellung in Azure PowerShell oder der Azure CLI. Darüber hinaus werden im Azure-Portal und über Azure Storage-Explorer einfache visuelle Lösungen für die Arbeit mit Ihren Daten bereitgestellt.  

## <a name="azure-storage-services"></a>Azure Storage-Dienste

Azure Storage umfasst die folgenden Datendienste:

- [Azure-Blobs](../blobs/storage-blobs-introduction.md): Ein überaus skalierbarer Objektspeicher für Text- und Binärdaten.
- [Azure Files](../files/storage-files-introduction.md): Verwaltete Dateifreigaben für Bereitstellungen lokal oder in der Cloud.
- [Azure-Warteschlangen](../queues/storage-queues-introduction.md): Ein Messagingspeicher für zuverlässiges Messaging zwischen Anwendungskomponenten. 
- [Azure-Tabellen](../tables/table-storage-overview.md): Ein NoSQL-Speicher für die schemalose Speicherung von strukturierten Daten.

Auf jeden Dienst wird über ein Speicherkonto zugegriffen. Informationen zu den ersten Schritten finden Sie unter [Speicherkonto erstellen](storage-quickstart-create-account.md).

## <a name="blob-storage"></a>Blob Storage

Azure Blob Storage ist die Objektspeicherlösung von Microsoft für die Cloud. Blobspeicher ist für die Speicherung großer Mengen von unstrukturierten Daten, z.B. Text oder Binärdaten, optimiert. 

Blobspeicher ist für folgende Zwecke ideal geeignet:

- Speichern von Bildern oder Dokumenten direkt für einen Browser
- Speichern von Dateien für verteilten Zugriff
- Video- und Audio-Streaming
- Speichern von Daten für Sicherung und Wiederherstellung, Notfallwiederherstellung und Archivierung
- Speichern von Daten für Analysen durch einen lokalen oder von Azure gehosteten Dienst

Über HTTP oder HTTPS kann von allen Orten weltweit auf Objekte zugegriffen werden, die sich in Blobspeicher befinden. Benutzer oder Clientanwendungen können über URLs, die [Azure Storage-REST-API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api), [Azure PowerShell](https://docs.microsoft.com/powershell/module/azure.storage), die [Azure CLI](https://docs.microsoft.com/cli/azure/storage) oder eine Azure Storage-Clientbibliothek auf Blobs zugreifen. Die Speicherclientbibliotheken sind für mehrere Sprachen verfügbar, z.B. [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage/client), [Java](https://docs.microsoft.com/java/api/overview/azure/storage), [Node.js](https://azure.github.io/azure-storage-node), [Python](https://azure-storage.readthedocs.io/), [PHP](https://azure.github.io/azure-storage-php/) und [Ruby](https://azure.github.io/azure-storage-ruby).

Weitere Informationen zu Blob Storage finden Sie unter [Einführung in Blob Storage](../blobs/storage-blobs-introduction.md).

## <a name="azure-files"></a>Azure Files

[Azure Files](../files/storage-files-introduction.md) ermöglicht die Einrichtung hochverfügbarer Netzwerkdateifreigaben, auf die über das standardmäßige SMB-Protokoll (Server Message Block) zugegriffen werden kann. Dadurch können mehrere virtuelle Computer gemeinsam die gleichen Dateien mit Lese- und Schreibzugriff nutzen. Die Dateien können auch mithilfe der REST-Schnittstelle oder mithilfe der Speicherclientbibliotheken gelesen werden.

Azure Files unterscheidet sich in einem Punkt von Dateien auf einer Dateifreigabe eines Unternehmens: Über eine URL, die auf die gewünschte Datei verweist und ein SAS-Token (Shared Access Signature) enthält, kann von jedem Ort der Welt auf die Datei zugegriffen werden. Sie können SAS-Token generieren, um für einen bestimmten Zeitraum spezifischen Zugriff auf eine private Ressource zu ermöglichen.

Dateifreigaben können in zahlreichen Szenarien verwendet werden:

- Viele lokale Anwendungen verwenden Dateifreigaben. Dieses Feature erleichtert die Migration dieser Anwendungen mit gemeinsamen Daten zu Azure. Wenn Sie die Dateifreigabe unter dem gleichen Laufwerksbuchstaben einbinden, den auch die lokale Anwendung verwendet, müsste der Teil Ihrer Anwendung, der auf die Dateifreigabe zugreift, mit minimalen (oder sogar ganz ohne) Änderungen funktionieren.

- Konfigurationsdateien können auf einer Dateifreigabe gespeichert und von mehreren virtuellen Computern genutzt werden. Tools und Hilfsprogramme, die von mehreren Entwicklern in einer Gruppe verwendet werden, können auf einer Dateifreigabe gespeichert werden, um sicherzustellen, dass sie von allen gefunden werden und dass alle die gleiche Version verwenden.

- Diagnoseprotokolle, Metriken und Absturzabbilder sind nur drei Beispiele für Daten, die zur späteren Verarbeitung oder Analyse auf eine Dateifreigabe geschrieben werden können.

Active Directory-basierte Authentifizierung und Zugriffssteuerungslisten (Access Control Lists, ACLs) werden derzeit noch nicht unterstützt, wir arbeiten aber daran. Zur Authentifizierung für den Zugriff auf die Dateifreigabe werden die Speicherkonto-Anmeldeinformationen verwendet. Das bedeutet, dass jeder Benutzer, bei dem die Freigabe eingebunden ist, uneingeschränkten Lese-/Schreibzugriff auf die Freigabe hat.

Weitere Informationen zu Azure Files finden Sie unter [Einführung in Azure Files](../files/storage-files-introduction.md).

## <a name="queue-storage"></a>Queue Storage

Der Azure Queue-Dienst wird zum Speichern und Abrufen von Nachrichten verwendet. Warteschlangennachrichten können eine Größe von bis zu 64 KB haben, und eine Warteschlange kann Millionen von Nachrichten enthalten. Warteschlangen dienen im Allgemeinen zum Speichern von Listen mit Nachrichten, die asynchron verarbeitet werden sollen.

Beispiel: Ihre Kunden sollen Bilder hochladen können, und Sie möchten Miniaturbilder für jedes Bild erstellen. Sie können Kunden warten lassen, bis die Miniaturbilder beim Hochladen der Bilder erstellt werden. Alternativ können Sie eine Warteschlange verwenden. Wenn der Kunde seinen Upload abgeschlossen hat, schreiben Sie eine Nachricht an die Warteschlange. Legen Sie dann eine Azure-Funktion fest, die die Nachricht aus der Warteschlange abruft und die Miniaturbilder erstellt. Alle Schritte dieses Verarbeitungsvorgangs können separat skaliert werden, sodass Sie ihn für Ihre Zwecke optimieren können.

Weitere Informationen zu Azure-Warteschlangen finden Sie unter [Einführung in Warteschlangen](../queues/storage-queues-introduction.md).

## <a name="table-storage"></a>Table Storage

Azure Table Storage ist jetzt Teil von Azure Cosmos DB. Diese Azure Table Storage-Dokumentation finden Sie unter [Übersicht über Azure Table Storage](../tables/table-storage-overview.md). Zusätzlich zum vorhandenen Azure Table Storage-Dienst ist das neue Angebot „Azure Cosmos DB-Tabellen-API“ mit durchsatzoptimierten Tabellen, globaler Verteilung und automatischen sekundären Indizes verfügbar. Unter [Einführung in die Tabellen-API von Azure Cosmos DB](https://aka.ms/premiumtables) erfahren Sie mehr und können das neue Premiumangebot testen.

Weitere Informationen zu Table Storage finden Sie unter [Übersicht über Azure Table Storage](../tables/table-storage-overview.md).

## <a name="disk-storage"></a>Datenträgerspeicher

Ein verwalteter Azure-Datenträger ist eine virtuelle Festplatte (Virtual Hard Disk, VHD). Sie können sich dies wie einen physischen Datenträger in einem lokalen Server vorstellen, aber der Unterschied besteht darin, dass er virtualisiert ist. Verwaltete Azure-Datenträger werden als Seitenblobs gespeichert, bei denen es sich in Azure um ein zufälliges E/A-Speicherobjekt handelt. Wir bezeichnen einen verwalteten Datenträger als „verwaltet“, weil es eine Abstraktion ist, die sich über Seitenblobs, Blobcontainer und Azure-Speicherkonten erstreckt. Bei verwalteten Datenträgern müssen Sie nicht mehr tun, als den Datenträger bereitzustellen. Azure übernimmt dann den Rest.

Weitere Informationen zu verwalteten Datenträgern finden Sie in der [Einführung in Azure Managed Disks](../../virtual-machines/windows/managed-disks-overview.md).

## <a name="types-of-storage-accounts"></a>Speicherkontentypen

[!INCLUDE [storage-account-types-include](../../../includes/storage-account-types-include.md)]

Weitere Informationen zu Arten von Speicherkontotypen finden Sie unter [Übersicht über Azure Storage-Konten](storage-account-overview.md).

## <a name="securing-access-to-storage-accounts"></a>Schützen des Zugriffs auf Speicherkonten

Jede Anforderung an Azure Storage muss autorisiert sein. Azure Storage unterstützt die folgenden Autorisierungsmethoden:

- **Integration von Azure Active Directory (Azure AD) für Blob- und Warteschlangendaten.** Azure Storage unterstützt die Authentifizierung und Autorisierung mit Azure AD für Blob- und Warteschlangendienste über die rollenbasierte Zugriffssteuerung (RBAC). Für mehr Sicherheit und Benutzerfreundlichkeit wird die Autorisierung von Anforderungen mit Azure AD empfohlen. Weitere Informationen finden Sie unter [Authorize access to Azure blobs and queues using Azure Active Directory (Autorisieren des Zugriffs auf Azure-Blobs und -Warteschlangen mit Azure Active Directory)](storage-auth-aad.md).
- **Azure AD-Autorisierung über SMB für Azure Files (Vorschau).** Azure Files unterstützt identitätsbasierte Autorisierung über SMB (Server Message Block) über Azure Active Directory Domain Services. Ihre in die Domäne eingebundenen virtuellen Windows-Computer (VMs) können mit Azure AD-Anmeldeinformationen auf Azure-Dateifreigaben zugreifen. Weitere Informationen finden Sie unter [Übersicht zur Azure Active Directory-Autorisierung über SMB für Azure Files (Vorschau)](../files/storage-files-active-directory-overview.md).
- **Autorisierung mit gemeinsam verwendetem Schlüssel.** Die Azure Storage-Blob-, Warteschlangen- und Tabellendienste sowie Azure Files unterstützten die Autorisierung mit gemeinsam verwendetem Schlüssel. Ein Client, der die Autorisierung mit gemeinsam verwendetem Schlüssel nutzt, übergibt mit jeder Anforderung einen Header, der mit dem Speicherkonto-Zugriffsschlüssel signiert wird. Weitere Informationen finden Sie unter [Authentifizieren mit gemeinsam verwendetem Schlüssel](https://docs.microsoft.com/rest/api/storageservices/authorize-with-shared-key).
- **Autorisierung mit Shared Access Signatures (SAS).** Eine Shared Access Signature (SAS) ist eine Zeichenfolge mit einem Sicherheitstoken, das an den URI für eine Speicherressource angefügt werden kann. Im Sicherheitstoken sind Einschränkungen wie Berechtigungen und das Zugriffsintervall gekapselt. Weitere Informationen finden Sie unter [Verwenden von Shared Access Signatures (SAS)](storage-sas-overview.md).
- **Anonymer Zugriff auf Container und Blobs.** Ein Container und seine Blobs können öffentlich verfügbar sein. Wenn Sie einen Container oder ein Blob als öffentliche Ressource festlegen, kann jeder Benutzer anonym und ohne Authentifizierung darauf zugreifen. Weitere Informationen finden Sie unter [Manage anonymous read access to containers and blobs](../blobs/storage-manage-access-to-resources.md) (Verwalten des anonymen Lesezugriffs auf Container und Blobs).

## <a name="encryption"></a>Verschlüsselung

Für die Storage-Dienste stehen zwei grundlegende Verschlüsselungsarten zur Verfügung. Ausführlichere Informationen zu Sicherheit und Verschlüsselung finden Sie im [Azure Storage-Sicherheitsleitfaden](storage-security-guide.md).

### <a name="encryption-at-rest"></a>Verschlüsselung ruhender Daten

Durch die Azure Storage-Verschlüsselung werden Ihre Daten ausreichend geschützt, um den Sicherheits- und Complianceanforderungen Ihrer Organisation gerecht zu werden. Vor der dauerhaften Speicherung im Speicherkonto werden alle Daten automatisch durch Azure Storage verschlüsselt. Beim Abrufen werden die Daten entschlüsselt. Die Verschlüsselungs-, Entschlüsselungs- und Schlüsselverwaltungsprozesse sind für Benutzer vollständig transparent. Die Kunden können ihre eigenen Schlüssel mithilfe von Azure Key Vault verwalten. Weitere Informationen finden Sie unter [Azure Storage-Verschlüsselung für ruhende Daten](storage-service-encryption.md).

### <a name="client-side-encryption"></a>Clientseitige Verschlüsselung

Die Clientbibliotheken für Azure Storage umfassen verschiedene Methoden zum Verschlüsseln von Daten aus der Clientbibliothek, bevor sie gesendet werden und die Antwort entschlüsselt wird. Daten, die über die clientseitige Verschlüsselung verschlüsselt werden, werden auch im ruhenden Zustand von Azure Storage verschlüsselt. Weitere Informationen zur clientseitigen Verschlüsselung finden Sie unter [Client-side encryption with .NET for Azure Storage (Clientseitige Verschlüsselung mit .NET für Azure Storage)](storage-client-side-encryption.md).

## <a name="redundancy"></a>Redundanz

Um die Dauerhaftigkeit Ihrer Daten zu gewährleisten, repliziert Azure Storage mehrere Kopien Ihrer Daten. Beim Einrichten Ihres Speicherkontos wählen Sie eine Redundanzoption aus.

[!INCLUDE [storage-common-redundancy-options](../../../includes/storage-common-redundancy-options.md)]

Weitere Informationen zur Notfallwiederherstellung finden Sie unter [Vorgehensweise beim Ausfall von Azure Storage](storage-disaster-recovery-guidance.md).

## <a name="transferring-data-to-and-from-azure-storage"></a>Übertragen von Daten in und aus Azure Storage

Sie haben mehrere Optionen zum Verschieben von Daten in oder aus Azure Storage. Welche Option Sie auswählen, hängt von der Größe Ihres Datasets und der verfügbaren Netzwerkbandbreite ab. Weitere Informationen finden Sie unter [Auswählen einer Azure-Lösung für die Datenübertragung](storage-choose-data-transfer-solution.md).

## <a name="pricing"></a>Preise

Ausführliche Informationen zu den Preisen für Azure Storage finden Sie auf der Seite mit den [Azure Storage-Preisen](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="storage-apis-libraries-and-tools"></a>Storage-APIs, -Bibliotheken und -Tools

Auf Azure Storage-Ressourcen kann über jede Sprache zugegriffen werden, die für HTTP/HTTPS-Anforderungen geeignet ist. Zusätzlich bietet Azure Storage Programmierbibliotheken für einige beliebte Sprachen. Diese Bibliotheken vereinfachen viele Aspekte der Arbeit mit Azure Storage und behandeln bestimmte Details wie synchrone und asynchrone Aufrufe, die Zusammenfassung von Vorgängen, die Ausnahmeverwaltung, automatische Wiederholungen, das Betriebsverhalten und Ähnliches. Bibliotheken sind aktuell für die folgenden Sprachen und Plattformen erhältlich. Weitere sind in Planung.

### <a name="azure-storage-data-api-and-library-references"></a>Verweise auf die Azure Storage-Daten-API und Bibliotheken

- [Azure Storage-REST-API](https://docs.microsoft.com/rest/api/storageservices/)
- [Azure Storage-Clientbibliothek für .NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage)
- [Azure Storage-Clientbibliothek für Java/Android](https://docs.microsoft.com/java/api/overview/azure/storage)
- [Azure Storage-Clientbibliothek für Node.js](https://docs.microsoft.com/javascript/api/overview/azure/storage)
- [Azure Storage-Clientbibliothek für Python](https://github.com/Azure/azure-storage-python)
- [Azure Storage-Clientbibliothek für PHP](https://github.com/Azure/azure-storage-php)
- [Azure Storage-Clientbibliothek für Ruby](https://github.com/Azure/azure-storage-ruby)
- [Azure Storage-Clientbibliothek für C++](https://github.com/Azure/azure-storage-cpp)

### <a name="azure-storage-management-api-and-library-references"></a>Verweise auf die Azure Storage-Verwaltungs-API und Bibliotheken

- [Speicherressourcenanbieter – REST-API](https://docs.microsoft.com/rest/api/storagerp/)
- [Speicherressourcenanbieter – Clientbibliothek für .NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage/management)
- [Storage-Dienstverwaltung – REST-API (klassisch)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### <a name="azure-storage-data-movement-api-and-library-references"></a>Verweise auf die Azure Storage-Datenverschiebungs-API und Bibliotheken

- [Import/Export-Storage-Dienst – REST-API](https://docs.microsoft.com/rest/api/storageimportexport/)
- [Storage-Datenverschiebung – Clientbibliothek für .NET](/dotnet/api/microsoft.azure.storage.datamovement)

### <a name="tools-and-utilities"></a>Tools und Hilfsprogramme

- [Azure PowerShell-Cmdlets für Storage](https://docs.microsoft.com/powershell/module/az.storage)
- [Azure CLI-Cmdlets für Storage](https://docs.microsoft.com/cli/azure/storage)
- [Befehlszeilenprogramm AzCopy](https://aka.ms/downloadazcopy)
- Bei [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) handelt es sich um eine kostenlose eigenständige App von Microsoft, über die Sie ganz einfach visuell mit Azure Storage-Daten arbeiten können – unter Windows, MacOS und Linux.
- [Azure Storage-Clienttools](../storage-explorers.md)
- [Azure-Entwicklertools](https://azure.microsoft.com/tools/)

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Herstellung der Betriebsbereitschaft für Azure Storage finden Sie unter [Erstellen eines Speicherkontos](storage-quickstart-create-account.md).

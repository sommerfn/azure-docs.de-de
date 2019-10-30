---
title: Cloudspeicher zum Erstellen von äußerst sicheren, langlebigen und skalierbaren Apps mit Azure Storage
description: Erfahren Sie mehr über die Dienste, mit denen umfangreiche strukturierte und nicht strukturierte Daten einer mobilen Anwendung in der Cloud gespeichert werden können.
author: elamalani
ms.assetid: 12bbb070-9b3c-4faf-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 10/22/2019
ms.author: emalani
ms.openlocfilehash: 01407f843ef36095fc87feb3722e85dc477ad8bb
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72795134"
---
# <a name="cloud-storage-for-highly-secure-durable-scalable-apps-with-azure-storage"></a>Cloudspeicher für äußerst sichere, langlebige und skalierbare Apps mit Azure Storage
[Azure Storage](https://azure.microsoft.com/services/storage/) ist Microsofts Cloudspeicherlösung für moderne Anwendungen, die einen stark skalierbaren Objektspeicher für Datenobjekte, einen Dateisystemdienst für die Cloud, einen Messagingspeicher für zuverlässiges Messaging und einen NoSQL-Speicher bereitstellt. Für Azure Storage gilt Folgendes:
- **Dauerhaft und hochverfügbar**: Mit Redundanz wird sichergestellt, dass Ihre Daten sicher sind, falls es zu vorübergehenden Hardwareausfällen kommt. Sie können sich auch für das Replizieren von Daten über Rechenzentren oder geografische Regionen hinweg entscheiden, um eine weitere Schutzebene vor lokalen Notfällen oder Naturkatastrophen zu schaffen. Daten, die auf diese Weise repliziert werden, sind bei einem unerwarteten Ausfall weiterhin hoch verfügbar.
- **Sicher**: Alle Daten, die in Azure Storage geschrieben werden, werden vom Dienst verschlüsselt. Bei Azure Storage können Sie genau steuern, wer Zugriff auf Ihre Daten hat.
- **Skalierbar**: Dienste sind auf hohe Skalierbarkeit ausgelegt, um die Datenspeicherungs- und Leistungsanforderungen heutiger Anwendungen zu erfüllen.
- **Verwaltet**: In Azure werden Hardwarewartung, Updates und die Behandlung kritischer Probleme für Sie übernommen.
- Die Daten sind von überall auf der Welt über HTTP oder HTTPS **verfügbar**. Microsoft stellt Clientbibliotheken in verschiedenen Sprachen (z. B. .NET, Java, Node.js, Python, PHP, Ruby und Go) sowie eine ausgereifte REST-API bereit. Skripterstellung wird in Azure PowerShell oder Azure CLI unterstützt, und im Azure-Portal und über Azure Storage-Explorer werden einfache visuelle Lösungen für das Arbeiten mit Ihren Daten bereitgestellt.

Mit den folgenden Diensten können Sie Cloudspeicher in ihren mobilen Apps aktivieren.

## <a name="azure-blob-storage"></a>Azure Blob Storage
[Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/) stellt eine Objektspeicherlösung für die Cloud bereit und ist zum Speichern von großen Mengen unstrukturierter Daten optimiert, die nicht an ein bestimmtes Datenmodell oder eine bestimmte Datendefinition gebunden sind, z. B. Text oder binäre Daten. Der Dienst unterstützt Clientbibliotheken in verschiedenen Sprachen und ist für Folgendes konzipiert:
- Speichern von Bildern oder Dokumenten direkt für einen Browser
- Speichern von Dateien für verteilten Zugriff
- Video- und Audio-Streaming
- Schreiben in Protokolldateien
- Speichern von Daten für Sicherung und Wiederherstellung, Notfallwiederherstellung und Archivierung
- Speichern von Daten für Analysen durch einen lokalen oder von Azure gehosteten Dienst

**Referenzen**
- [Azure-Portal](https://portal.azure.com)
- [Documentation](/azure/storage/blobs/storage-blobs-introduction)
- [Schnellstarts](/azure/storage/blobs/storage-quickstart-blobs-portal)
- [Beispiele](/azure/storage/common/storage-samples-dotnet?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

## <a name="azure-table-storage"></a>Azure Table Storage
[Azure Table Storage](https://azure.microsoft.com/services/storage/tables/) ist ein Dienst, der strukturierte NoSQL-Daten in der Cloud speichert, wozu ein Schlüssel-/Attributspeicher mit einem schemalosen Design bereitgestellt wird. Mit Azure Table Storage können Sie große Mengen strukturierter Daten speichern. Der Dienst ist ein NoSQL-Datenspeicher, der authentifizierte Aufrufe von innerhalb und außerhalb der Azure-Cloud akzeptiert. Azure-Tabellen sind hervorragend zur Speicherung strukturierter nicht relationaler Daten geeignet. Table Storage wird hauptsächlich für folgende Zwecke verwendet:
- Speicherung strukturierter Daten in TB-Größe zur Verarbeitung webbasierter Anwendungen
- Speicherung von Datensätzen, die keine komplexen Verknüpfungen, Fremdschlüssel oder gespeicherte Prozeduren erfordern und für schnellen Zugriff denormalisiert werden können
- Schnelle Datenabfrage mithilfe eines gruppierten Index
- Datenzugriff über das OData-Protokoll und LINQ-Abfragen mit WCF Date Service .NET-Bibliotheken

Sie können Table Storage verwenden, um sehr große Mengen strukturierter nicht relationaler Daten zu speichern und abzufragen und Ihre Tabellen bei steigendem Bedarf skalieren.

**Referenzen**
- [Azure-Portal](https://portal.azure.com)
- [Documentation](/azure/storage/tables/table-storage-overview)
- [Beispiele](/azure/cosmos-db/tutorial-develop-table-dotnet?toc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fstorage%2Ftables%2FTOC.json&bc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fbread%2Ftoc.json)
- [Schnellstarts](/azure/storage/tables/table-storage-quickstart-portal)

## <a name="azure-files"></a>Azure Files
[Azure Files](https://azure.microsoft.com/services/storage/files/) ermöglicht die Einrichtung hochverfügbarer Netzwerkdateifreigaben, auf die über das standardmäßige SMB-Protokoll (Server Message Block) zugegriffen werden kann. Mehrere virtuelle Computer können dieselben Dateien mit Lese- und Schreibzugriff gemeinsam nutzen. Die Dateien können auch mithilfe der REST-Schnittstelle oder mithilfe der Speicherclientbibliotheken gelesen werden. Über eine URL, die auf die jeweils gewünschte Datei verweist und ein SAS-Token (Shared Access Signature) enthält, kann von jedem Ort der Welt auf die Dateien zugegriffen werden. Sie können SAS-Token generieren, um für einen bestimmten Zeitraum spezifischen Zugriff auf eine private Ressource zu ermöglichen.

Verwendungsmöglichkeiten für Azure-Dateifreigaben:
- Ersetzen oder Erweitern von lokalen Dateiservern: Gängige Betriebssysteme wie Windows, macOS und Linux können Azure-Dateifreigaben auf der ganzen Welt direkt einbinden. Azure-Dateifreigaben können auch per Azure-Dateisynchronisierung auf Windows-Servern repliziert werden (entweder lokal oder in der Cloud), um für die Daten am Ort ihrer Verwendung eine hohe Leistung und eine verteilte Zwischenspeicherung zu erzielen.
- **Verschieben von Anwendungen**, für die eine Dateifreigabe erwartet wird, in der Dateianwendungs- oder Benutzerdaten gespeichert werden, in die Cloud.
- **Einfachere Cloudentwicklung:** Azure Files kann auch auf verschiedene Arten genutzt werden, um neue Cloudentwicklungsprojekte zu vereinfachen. Beispiel:
    - Gemeinsame Anwendungseinstellungen: Bei verteilten Anwendungen befinden sich die Konfigurationsdateien häufig an einem zentralen Ort, sodass viele Anwendungsinstanzen darauf zugreifen können. Anwendungsinstanzen können ihre Konfiguration über die Datei-REST-API laden, und Menschen können je nach Bedarf darauf zugreifen, indem sie die SMB-Freigabe lokal bereitstellen.
    - Diagnosefreigabe: Eine Azure-Dateifreigabe ist für Cloudanwendungen ein guter Ort zum Schreiben von Protokollen, Metriken und Absturzabbildern. Protokolle können von den Anwendungsinstanzen über die Datei-REST-API geschrieben werden, und Entwickler können darauf zugreifen, indem sie die Dateifreigabe auf ihrem lokalen Computer bereitstellen. Dies führt zu einer hohen Flexibilität, da Entwickler sich mit der Cloudentwicklung beschäftigen können, ohne dafür ihre vertrauten und bevorzugten Tools aufgeben zu müssen.

**Referenzen**
- [Azure-Portal](https://portal.azure.com)
- [Documentation](/azure/storage/files/storage-files-introduction)
- [Schnellstarts](/azure/storage/files/storage-files-quick-create-use-windows)

## <a name="azure-queues"></a>Azure-Warteschlangen
[Azure Queue Storage](https://azure.microsoft.com/services/storage/queues/) ist ein Dienst für die Speicherung sehr vieler Nachrichten. Sie können überall auf der Welt über authentifizierte Aufrufe mithilfe von HTTP oder HTTPS auf Nachrichten zugreifen. Eine Warteschlangennachricht kann bis zu 64 KB groß sein, und eine Warteschlange kann Millionen von Nachrichten enthalten. Deren Anzahl ist nur durch die Kapazität des Speicherkontos begrenzt. Warteschlangen werden häufig verwendet, um ein Arbeits-Backlog zur asynchronen Verarbeitung zu erstellen.

**Referenzen**
- [Azure-Portal](https://portal.azure.com)
- [Documentation](/azure/storage/queues/)
- [Schnellstarts](/azure/storage/queues/storage-quickstart-queues-portal)
- [Beispiele](/azure/storage/common/storage-samples-dotnet?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)

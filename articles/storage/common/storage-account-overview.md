---
title: Übersicht über Azure Storage-Konten | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Optionen zum Erstellen und Verwenden eines Azure Storage-Kontos.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 06/07/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 308809a9b78de9d3c0e77ed6028e62c42ff4e1c5
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72882590"
---
# <a name="azure-storage-account-overview"></a>Übersicht über Azure Storage-Konten

Ein Azure Storage-Konto enthält all Ihre Azure Storage-Datenobjekte: Blobs, Dateien, Warteschlangen, Tabellen und Datenträger. Das Speicherkonto stellt einen eindeutigen Namespace für Ihre Azure Storage-Daten bereit, auf den von jedem Ort der Welt aus über HTTP oder HTTPS zugegriffen werden kann. Daten in Ihrem Azure Storage-Konto sind dauerhaft und hochverfügbar, sicher und extrem skalierbar.

Um zu erfahren, wie Sie ein Azure Storage-Konto erstellen, lesen Sie den Artikel [Erstellen eines Speicherkontos](storage-quickstart-create-account.md).

## <a name="types-of-storage-accounts"></a>Speicherkontentypen

[!INCLUDE [storage-account-types-include](../../../includes/storage-account-types-include.md)]

### <a name="general-purpose-v2-accounts"></a>Allgemeines Konto vom Typ „General Purpose v2“

Speicherkonten vom Typ „Allgemein v2“ unterstützen die neuesten Azure Storage-Features und umfassen die gesamte Funktionalität von Konten des Typs „Allgemein v1“ und Blob Storage-Konten. Konten vom Typ „Allgemein v2“ bieten die niedrigsten Preise pro Gigabyte für Azure Storage sowie wettbewerbsfähige Transaktionspreise. Speicherkonten vom Typ „Allgemein v2“ unterstützen die folgenden Azure Storage-Dienste:

- Blobs (alle Typen: Block-, Anfüge- und Seitenblobs)
- Dateien
- Datenträger
- Warteschlangen
- Tabellen

> [!NOTE]
> Microsoft empfiehlt für die meisten Szenarien die Verwendung von Speicherkonten vom Typ „Allgemein v2“. Sie können ganz einfach ein Upgrade von einem Allgemein v1-Konto oder einem Blob Storage-Konto auf ein Allgemein v2-Konto durchführen. Dabei treten keine Ausfallzeiten auf, und Sie müssen keine Daten kopieren.
>
> Weitere Informationen zum Upgrade auf ein Allgemein v2-Konto finden Sie unter [Upgrade auf ein Allgemein v2-Speicherkonto](storage-account-upgrade.md).

Allgemein v2-Speicherkonten bieten mehrere Zugriffsebenen zum Speichern von Daten basierend auf Ihren Nutzungsmustern. Weitere Informationen finden Sie unter [Zugriffsebenen für Blockblobdaten](#access-tiers-for-block-blob-data).

### <a name="general-purpose-v1-accounts"></a>Allgemeines Konto vom Typ „General Purpose v1“

Speicherkonten vom Typ „Universell v1“ ermöglichen Zugriff auf alle Azure Storage-Dienste, bieten aber möglicherweise nicht die neuesten Features oder die niedrigsten Preise pro Gigabyte. Speicherkonten vom Typ „Allgemein v1“ unterstützen die folgenden Azure Storage-Dienste:

- Blobs (alle Typen)
- Dateien
- Datenträger
- Warteschlangen
- Tabellen

Während Allgemein v2-Konten für die meisten Fälle empfohlen werden, eignen sich Allgemein v1-Konten am besten für folgende Szenarien:

* Ihre Anwendungen erfordern das klassische Azure-Bereitstellungsmodell. Allgemein v2- und Blob Storage-Konten unterstützen nur das Azure Resource Manager-Bereitstellungsmodell.

* Ihre Anwendungen verursachen eine hohe Transaktionslast oder nutzen eine erhebliche Bandbreite für die Georeplikation, erfordern aber keine großen Kapazitäten. In diesem Fall sind Allgemein v1-Konten möglicherweise die wirtschaftlich sinnvollste Wahl.

* Sie verwenden eine ältere Version der [REST-API für Speicherdienste](https://msdn.microsoft.com/library/azure/dd894041.aspx) (vor 2014-02-14) oder eine Clientbibliothek mit einer niedrigeren Version als 4.x und können kein Upgrade für Ihre Anwendung durchführen.

### <a name="blockblobstorage-accounts"></a>BlockBlobStorage-Konten

Ein BlockBlobStorage-Konto ist ein spezielles Speicherkonto, das zum Speichern von unstrukturierten Objektdaten als Blockblobs und Erstellen von Premium-Blockblobs verwendet wird. Diese Speicherkontoart unterstützt Block- und Anfügeblobs, nicht jedoch Seitenblobs, Tabellen oder Warteschlangen.

Gegenüber universellen V2- und BlobStorage-Konten bieten BlockBlobStorage-Konten niedrige und einheitliche Latenzzeiten und höhere Transaktionsraten.

BlockBlobStorage-Konten unterstützen derzeit kein Tiering zu den Zugriffsebenen „Heiß“, „Kalt“ oder „Archiv“.

### <a name="filestorage-accounts"></a>FileStorage-Konten

Ein FileStorage-Konto ist ein spezielles Speicherkonto, das zum Speichern und Erstellen von Premium-Dateifreigaben verwendet wird. Diese Speicherkontoart unterstützt Dateien, aber keine Block-, Anfüge- oder Seitenblobs, Tabellen oder Warteschlangen. 

FileStorage-Konten bieten einzigartige leistungsorientierte Merkmale wie IOPS-Bursting. Weitere Informationen zu diesen Merkmalen finden Sie im Abschnitt [Leistungsstufen für Dateifreigaben](../files/storage-files-planning.md#file-share-performance-tiers) des Planungshandbuchs für Azure Files.

## <a name="naming-storage-accounts"></a>Benennen von Speicherkonten

Beachten Sie bei der Benennung Ihres Speicherkontos folgende Regeln:

- Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten.
- Der Name Ihres Speicherkontos muss innerhalb von Azure eindeutig sein. Zwei Speicherkonten können nicht denselben Namen haben.

## <a name="performance-tiers"></a>Leistungsstufen

Allgemeine Speicherkonten können für jede der folgenden Leistungsstufen konfiguriert werden:

* Eine Standard-Leistungsstufe zum Speichern von Blobs, Dateien, Tabellen, Warteschlangen und Azure-VM-Datenträgern.
* Eine Premium-Leistungsstufe zum ausschließlichen Speichern von nicht verwalteten VM-Datenträgern.

BlockBlobStorage-Konten bieten die Leistungsebene „Premium“ für das Speichern von Block- und Anfügeblobs.

FileStorage-Speicherkonten bieten die Leistungsebene „Premium“ für Azure-Dateifreigaben.

## <a name="access-tiers-for-block-blob-data"></a>Zugriffsebenen für Blockblobdaten

Azure Storage bietet verschiedene Optionen für den Zugriff auf Blockblobdaten basierend auf Nutzungsmustern. Jede Zugriffsebene in Azure Storage ist für ein bestimmtes Muster der Datennutzung optimiert. Indem Sie die richtige Zugriffsebene für Ihre Anforderungen auswählen, können Sie Ihre Blockblobdaten auf möglichst kostengünstige Art und Weise speichern.

Folgende Zugriffsebenen sind verfügbar:

* Die Zugriffsebene **Heiß**, die für häufigen Zugriff auf Objekte im Speicherkonto optimiert ist. Der Zugriff auf Daten auf der Zugriffsebene „Heiß“ ist besonders kostengünstig, dafür liegen aber die Speicherkosten etwas höher. Neue Speicherkonten werden standardmäßig auf dieser Ebene erstellt.
* Die Zugriffsebene **Kalt**, die für die Speicherung von großen Datenmengen optimiert ist, auf die selten zugegriffen wird und die mindestens 30 Tage lang gespeichert werden. Das Speichern von Daten auf der Ebene „Kalt“ ist kostengünstiger, doch kann der Zugriff auf die Daten teurer sein als der Zugriff auf der Ebene „Heiß“.
* Die Ebene **Archiv**, die nur für einzelne Blockblobs verfügbar ist. Diese Ebene ist für Daten optimiert, die mehrere Stunden Abruflatenz tolerieren und mindestens 180 Tage lang auf der Ebene „Archiv“ verbleiben. Die Archivebene ist die kosteneffektivste Option für das Speichern von Daten, der Zugriff auf die Daten ist jedoch kostenintensiver als der Zugriff auf Daten auf der heißen oder kalten Ebene.

Wenn sich das Nutzungsmusters Ihrer Daten ändern, können Sie jederzeit zwischen den Zugriffsebenen wechseln. Weitere Informationen zu Zugriffsebenen finden Sie unter [Azure Blob Storage: Zugriffsebenen „Heiß“ (Hot), „Kalt“ (Cool) und „Archiv“](../blobs/storage-blob-storage-tiers.md).

> [!IMPORTANT]
> Das Ändern der Zugriffsebene für ein vorhandenes Speicherkonto oder Blob zieht möglicherweise zusätzliche Gebühren nach sich. Weitere Informationen finden Sie im Abschnitt [Speicherkontoabrechnung](#storage-account-billing).

## <a name="replication"></a>Replikation

[!INCLUDE [storage-common-redundancy-options](../../../includes/storage-common-redundancy-options.md)]

Weitere Informationen zur Speicherreplikation finden Sie unter [Azure Storage-Replikation](storage-redundancy.md).

## <a name="encryption"></a>Verschlüsselung

Alle Daten in Ihrem Speicherkonto werden auf Dienstseite verschlüsselt. Weitere Informationen zur Verschlüsselung finden Sie unter [Azure Storage Service Encryption für ruhende Daten](storage-service-encryption.md).

## <a name="storage-account-endpoints"></a>Speicherkontoendpunkte

Ein Speicherkonto bietet einen eindeutigen Namespace für Ihre Daten in Azure. Jedes Objekt, das Sie in Azure speichern, besitzt eine Adresse, die den eindeutigen Namen Ihres Speicherkontos enthält. Die Kombination aus Kontoname und Azure Storage-Dienstendpunkt bildet die Endpunkte für Ihr Speicherkonto.

Wenn Ihr allgemeines Speicherkonto beispielsweise *meinspeicherkonto*heißt, lauten die Standardendpunkte für dieses Konto wie folgt:

* Blobspeicher: http://*meinspeicherkonto*.blob.core.windows.net
* Tabellenspeicher: http://*meinspeicherkonto*.table.core.windows.net
* Warteschlangenspeicher: http://*meinspeicherkonto*.queue.core.windows.net
* Azure Files: http://*meinspeicherkonto*.file.core.windows.net

> [!NOTE]
> Blockblob- und Blockspeicherkonten machen nur den Endpunkt des Blobdiensts verfügbar.

Die URL für den Zugriff auf ein Objekt in einem Speicherkonto wird durch Anhängen des Objektstandorts im Speicherkonto an den Endpunkt generiert. Eine Blob-Adresse kann beispielsweise das folgende Format haben: http://*meinspeicherkonto*.blob.core.windows.net/*meincontainer*/*meinblob*.

Sie können Ihr Speicherkonto auch für die Verwendung einer benutzerdefinierten Domäne konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens für Ihr Azure Storage-Konto](../blobs/storage-custom-domain-name.md).  

## <a name="control-access-to-account-data"></a>Steuern des Zugriffs auf Kontodaten

Standardmäßig sind die Daten in Ihrem Konto nur für Sie als Kontobesitzer verfügbar. Sie können steuern, welche Personen auf Ihre Daten zugreifen dürfen und welche Berechtigungen diese Personen besitzen.

Jede Anforderung, die für Ihr Speicherkonto gesendet wird, muss autorisiert werden. Auf Ebene des Diensts muss die Anforderung einen gültigen *Autorisierungsheader* aufweisen. Dieser enthält alle Informationen, die der Dienst benötigt, um die Anforderung vor der Ausführung zu überprüfen.

Sie können eine der folgenden Methoden verwenden, um Zugriff auf die Daten in Ihrem Speicherkonto zu gewähren:

- **Azure Active Directory:** Verwenden Sie Anmeldeinformationen von Azure Active Directory (Azure AD), um einen Benutzer, eine Gruppe oder eine andere Identität für den Zugriff auf Blob- und Warteschlangendaten zu authentifizieren. Wenn die Authentifizierung einer Identität erfolgreich war, gibt Azure AD ein Token zurück, das zum Autorisieren der Anforderung bei Azure Blob Storage oder Azure Queue Storage verwendet wird. Weitere Informationen finden Sie unter [Authentifizieren des Zugriffs auf Azure Storage mit Azure Active Directory](storage-auth-aad.md).
- **Autorisierung mit gemeinsam verwendetem Schlüssel:** Verwenden Sie den Zugriffsschlüssel für Ihr Speicherkonto, um eine Verbindungszeichenfolge zu erstellen, die Ihre Anwendung zur Laufzeit für den Zugriff auf Azure Storage verwendet. Die Werte in der Verbindungszeichenfolge werden verwendet, um den *Autorisierungsheader* zu generieren, der an Azure Storage übergeben wird. Weitere Informationen hierzu finden Sie unter [Konfigurieren von Azure Storage-Verbindungszeichenfolgen](storage-configure-connection-string.md).
- **Shared Access Signature (SAS):** Verwenden Sie eine Shared Access Signature, um den Zugriff auf Ressourcen in Ihrem Speicherkonto zu delegieren, wenn Sie nicht die Azure AD-Autorisierung nutzen. Eine Shared Access Signature ist ein Token, das alle Informationen kapselt, die zum Autorisieren einer Zugriffsanforderung für Azure Storage in der URL erforderlich sind. In der Shared Access Signature können Sie die Speicherressource, die gewährten Berechtigungen und das Intervall angeben, in dem die Berechtigungen gültig sind. Weitere Informationen finden Sie unter [Verwenden von Shared Access Signatures (SAS)](storage-sas-overview.md).

> [!NOTE]
> Die Authentifizierung von Benutzern oder Anwendungen mithilfe von Azure AD-Anmeldeinformationen bietet mehr Sicherheit und Benutzerfreundlichkeit als andere Autorisierungsmethoden. Während Sie weiterhin die Autorisierung mit gemeinsam verwendetem Schlüssel mit Ihren Anwendungen verwenden können, macht Azure AD das Speichern Ihrer Kontozugriffsschlüssel mit Ihrem Code überflüssig. Sie können auch weiterhin Shared Access Signatures für zum Gewähren eines differenzierten Zugriffs auf Ressourcen in Ihrem Speicherkonto verwenden. Azure AD bietet jedoch ähnliche Funktionen, bei denen Sie weder SAS-Token verwalten noch sich um das Widerrufen einer gefährdeten SAS kümmern müssen. 
>
> Microsoft empfiehlt, nach Möglichkeit die Azure AD-Autorisierung für Ihre Blob- und Warteschlangenanwendungen in Azure Storage zu verwenden.

## <a name="copying-data-into-a-storage-account"></a>Kopieren von Daten in ein Speicherkonto

Microsoft bietet Hilfsprogramme und Bibliotheken für den Import Ihrer Daten von lokalen Speichergeräten oder Drittanbietern von Cloudspeichern. Welche Lösung Sie nutzen, richtet sich nach der Menge an Daten, die Sie übertragen müssen. 

Wenn Sie ein Upgrade eines Allgemein v1- oder Blob Storage-Kontos auf ein Allgemein v2-Konto durchführen, werden Ihre Daten automatisch migriert. Microsoft empfiehlt diese Vorgehensweise für das Upgrade Ihres Kontos. Wenn Sie jedoch Daten aus einem Allgemein v1-Konto in ein Blob Storage-Konto verschieben möchten, müssen Sie die Daten manuell mithilfe der unten beschriebenen Tools und Bibliotheken migrieren. 

### <a name="azcopy"></a>AzCopy

AzCopy ist ein Windows-Befehlszeilenprogramm, mit dem sehr effizient Daten in und aus Azure Storage kopiert werden können. Mit AzCopy können Sie Daten aus einem vorhandenen allgemeinen Speicherkonto in ein Blob Storage-Konto kopieren oder Daten von lokalen Speichergeräten hochladen. Weitere Informationen finden Sie unter [Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="data-movement-library"></a>Data Movement-Bibliothek

Die Azure Storage Data Movement-Bibliothek für .NET basiert auf dem Kernframework für die Datenverschiebung, das AzCopy zugrunde liegt. Die Bibliothek ist ähnlich wie AzCopy auf leistungsfähige, zuverlässige und einfache Datenübertragungsvorgänge ausgelegt. Sie ermöglicht Ihnen die native Verwendung aller Vorteile der AzCopy-Features in Ihrer Anwendung, ohne externe Instanzen von AzCopy ausführen und überwachen zu müssen. Weitere Informationen finden Sie unter [Azure Storage Data Movement-Bibliothek für .NET](https://github.com/Azure/azure-storage-net-data-movement).

### <a name="rest-api-or-client-library"></a>REST-API oder Clientbibliothek

Sie können eine benutzerdefinierte Anwendung erstellen, um Ihre Daten mithilfe einer der Azure-Clientbibliotheken oder mithilfe der REST-API der Azure Storage-Dienste in ein Blob-Speicherkonto zu migrieren. Azure Storage bietet umfassende Clientbibliotheken für mehrere Programmiersprachen und Plattformen wie .NET, Java, C++, Node.JS, PHP, Ruby und Python. Die Clientbibliotheken bieten erweiterte Funktionen, beispielsweise Wiederholungslogik, Protokollierung und parallele Uploads. Die Entwicklung kann direkt mit der REST-API erfolgen. Diese API lässt sich mithilfe jeder Sprache aufrufen, die HTTP/HTTPS-Anforderungen verarbeitet.

Weitere Informationen zur Azure Storage REST-API finden Sie unter [Azure Storage-Dienste – REST-API-Referenz](https://docs.microsoft.com/rest/api/storageservices/). 

> [!IMPORTANT]
> Mit clientseitiger Verschlüsselung verschlüsselte Blobs speichern Metadaten im Zusammenhang mit der Verschlüsselung mit dem Blob. Wenn Sie einen mit clientseitiger Verschlüsselung verschlüsselten Blob kopieren, müssen Sie sicherstellen, dass die Blobmetadaten und insbesondere die verschlüsselungsbezogenen Metadaten beim Kopiervorgang erhalten bleiben. Wenn Sie einen Blob ohne die Verschlüsselungsmetadaten kopieren, ist der Blobinhalt nicht mehr abrufbar. Weitere Informationen zu den auf die Verschlüsselung bezogenen Metadaten finden Sie unter [Clientseitige Azure Storage-Verschlüsselung](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="azure-importexport-service"></a>Azure Import/Export-Dienst

Wenn Sie eine große Menge an Daten in Ihr Speicherkonto importieren müssen, ziehen Sie den Azure Import/Export-Dienst in Betracht. Mit dem Import/Export-Dienst können Sie große Datenmengen auf sichere Weise in Azure Blob Storage und Azure Files übertragen, indem Sie Festplattenlaufwerke an ein Azure-Rechenzentrum senden. 

Sie können diesen Dienst auch zum Übertragen von Daten aus Azure Blob Storage auf Festplattenlaufwerke und zum Versand an lokale Standorte nutzen. Daten von einem oder mehreren Datenträgern können in Azure Blob Storage oder in Azure Files importiert werden. Weitere Informationen finden Sie unter [Was ist der Azure Import/Export-Dienst?](https://docs.microsoft.com/azure/storage/common/storage-import-export-service).

## <a name="storage-account-billing"></a>Speicherkontoabrechnung

[!INCLUDE [storage-account-billing-include](../../../includes/storage-account-billing-include.md)]

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Erstellen eines universellen Azure-Speicherkontos finden Sie unter [Erstellen eines Speicherkontos](storage-quickstart-create-account.md).
* Informationen zum Erstellen eines BlockBlobStorage-Kontos finden Sie unter [Erstellen eines Blockblob-Speicherkontos](../blobs/storage-blob-create-account-block-blob.md).
* Informationen zum Verwalten oder Löschen eines vorhandenen Speicherkontos finden Sie unter [Verwalten von Azure Storage-Konten](storage-account-manage.md).

---
title: Was ist Azure Data Share?
description: Es wird beschrieben, wie Sie mit Azure Data Share auf einfache und sichere Weise Daten für viele Kunden und Partner freigeben.
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: overview
ms.date: 07/10/2019
ms.openlocfilehash: 43972bc8e64e0bf88aae00126735d3e0c8751254
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73499379"
---
# <a name="what-is-azure-data-share"></a>Was ist Azure Data Share?

Heutzutage werden Daten als wichtige strategische Ressource angesehen, die von vielen Organisationen auf einfache und sichere Weise für Kunden und Partner freigegeben werden müssen. Es gibt viele Möglichkeiten, wie Kunden dies heutzutage durchführen, z. B. per FTP, E-Mail oder API, um nur einige zu nennen. Es kann leicht passieren, dass Organisationen den Überblick darüber verlieren, für wen Daten freigegeben wurden. Das Freigeben von Daten per FTP oder durch die Einrichtung einer eigenen API-Infrastruktur ist häufig mit hohen Kosten für die Bereitstellung und Verwaltung verbunden. Wenn diese Freigabemethoden in größerem Umfang angewendet werden, kommt es zu einem Mehraufwand bei der Verwaltung. 

Viele Organisationen sind für die von ihnen freigegebenen Daten verantwortlich, und sie müssen der Organisation zurechenbar sein. Zusätzlich zur Sicherstellung der Zurechenbarkeit möchten viele Organisationen ihre gesamte Datenfreigabe auch leicht steuern, verwalten und überwachen können. In der heutigen Welt, in der in Bezug auf die Datennutzung ein weiterer exponentieller Anstieg zu erwarten ist, benötigen Organisationen eine einfache Möglichkeit zum Freigeben großer Datenmengen. Kunden fordern Daten, die immer auf dem aktuellen Stand sind, um sicher zu sein, dass Erkenntnisse ohne Zeitverlust gewonnen werden können.

Mit Azure Data Share können Organisationen Daten für viele Kunden und Partner auf einfache und sichere Weise freigeben. Mit nur wenigen Klicks können Sie ein neues Konto für die Datenfreigabe bereitstellen, Datasets hinzufügen und Kunden und Partner für Ihre Datenfreigabe einladen. Datenanbieter haben jederzeit die Kontrolle über die Daten, die sie freigegeben haben. Mit Azure Data Share können Sie leicht verwalten und überwachen, welche Daten zu welchem Zeitpunkt und von wem freigegeben wurden. 

Ein Datenanbieter kann steuern, wie seine Daten verarbeitet werden, indem er Nutzungsbedingungen für die Datenfreigabe angibt. Der Datenconsumer muss diese Bedingungen akzeptieren, bevor er Daten empfangen kann. Datenanbieter können die Häufigkeit angeben, mit der Datenconsumer Updates erhalten. Der Zugriff auf neue Updates kann vom Datenanbieter jederzeit widerrufen werden. 

Mit Azure Data Share können bessere Erkenntnisse gewonnen werden, da es leicht möglich ist, Daten von Drittanbietern zu kombinieren, um Analyse- und KI-Szenarien zu erweitern. Nutzen Sie die Leistungsstärke von Azure-Analysetools, um Daten vorzubereiten, zu verarbeiten und zu analysieren, die mit Azure Data Share freigegeben werden. 

Sowohl der Datenanbieter als auch der Datenconsumer müssen über ein Azure-Abonnement verfügen, um Daten freigeben und empfangen zu können. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen.

## <a name="scenarios-for-azure-data-share"></a>Szenarien für Azure Data Share

Azure Data Share kann in unterschiedlichen Branchen eingesetzt werden. Es kann beispielsweise sein, dass ein Einzelhändler aktuelle Point of Sale-Daten für seine Lieferanten freigeben möchte. Mit Azure Data Share kann der Einzelhändler eine Datenfreigabe einrichten, die Point of Sales-Daten für alle Lieferanten enthält und über die die Verkaufsdaten stündlich oder täglich freigegeben werden. 

Azure Data Share kann auch genutzt werden, um einen Datenmarktplatz für eine bestimmte Branche einzurichten. Ein Beispiel hierfür ist eine Behörde oder ein Forschungsunternehmen, die bzw. das regelmäßig anonymisierte Daten zum Bevölkerungswachstum für Dritte freigibt. 

Ein anderer Anwendungsfall für Azure Data Share ist die Einrichtung eines Datenkonsortiums. Beispielsweise können verschiedene Forschungsunternehmen Daten für eine gemeinsame vertrauenswürdige Stelle freigeben. Die Daten werden analysiert, aggregiert oder verarbeitet, indem Azure-Analysetools genutzt werden, und anschließend werden sie für Interessenten freigegeben. 

## <a name="how-it-works"></a>So funktioniert's

Azure Data Share ermöglicht derzeit die auf Momentaufnahmen basierende Freigabe und die direkte Freigabe (eingeschränkte Vorschau). 

Bei der auf Momentaufnahmen basierenden Freigabe werden Daten aus dem Azure-Abonnement des Datenanbieters in das Azure-Abonnement des Datenconsumers verschoben. Als Datenanbieter stellen Sie eine Datenfreigabe bereit und laden Empfänger zur Nutzung der Datenfreigabe ein. Datenconsumer erhalten per E-Mail eine Einladung für Ihre Datenfreigabe. Nachdem ein Datenconsumer die Einladung angenommen hat, kann er eine vollständige Momentaufnahme der Daten auslösen, die für ihn freigegeben werden. Diese Daten werden über das Speicherkonto des Datenconsumers empfangen. Datenconsumer können regelmäßige inkrementelle Updates der freigegebenen Daten erhalten, damit sie immer über die aktuelle Version der Daten verfügen. 

Datenanbieter können ihren Datenconsumern inkrementelle Updates der freigegebenen Daten anbieten, indem sie einen Zeitplan für Momentaufnahmen verwenden. Es gibt Zeitpläne für die stündliche und tägliche Bereitstellung von Momentaufnahmen. Wenn ein Datenconsumer die Datenfreigabe akzeptiert und konfiguriert, kann er einen Zeitplan für Momentaufnahmen abonnieren. Dies eignet sich gut für Szenarien, in denen die freigegebenen Daten regelmäßig aktualisiert werden und der Datenconsumer den aktuellsten Stand der Daten benötigt. 

![Ablauf der Datenfreigabe](media/data-share-flow.png)

Wenn ein Datenconsumer eine Datenfreigabe akzeptiert, kann er die Daten in einem Datenspeicher seiner Wahl empfangen. Wenn der Datenanbieter beispielsweise Daten mit Azure Blob Storage freigibt, kann der Datenconsumer diese Daten in Azure Data Lake Store empfangen. Analog gilt Folgendes: Wenn der Datenanbieter Daten einer Azure SQL Data Warehouse-Instanz freigibt, kann der Datenconsumer wählen, ob er die Daten in einer Azure Data Lake Store-Instanz, einer Azure SQL-Datenbank oder einer Azure SQL Data Warehouse-Instanz erhalten möchte. Bei der Freigabe über SQL-basierte Quellen kann der Datenconsumer auch auswählen, ob er Daten im Parquet- oder CSV-Format empfangen möchte. 

Die direkte Freigabe befindet sich derzeit in der eingeschränkten Vorschauphase für Azure Data Explorer. Datenanbieter können Daten vom derzeitigen Speicherort über eine symbolische Verknüpfung freigeben, ohne dass Daten verschoben werden. Registrieren Sie sich [hier](https://aka.ms/azuredatasharepreviewsignup) für die eingeschränkte Vorschauversion der direkten Freigabe für Azure Data Explorer. 

## <a name="key-capabilities"></a>Wichtige Funktionen

Mit Azure Data Share haben Datenanbieter folgende Möglichkeiten:

* Freigeben von Daten über die Liste mit den [unterstützten Datenspeichern](supported-data-stores.md) mit Kunden und Partnern außerhalb Ihrer Organisation

* Nachverfolgen, für wen Sie Ihre Daten freigegeben haben

* Verfolgen, wie häufig Ihre Datenconsumer Updates Ihrer Daten erhalten

* Zulassen des Abrufens der aktuellen Version Ihrer Daten per Pullvorgang nach Bedarf oder Zulassen des automatischen Empfangs von inkrementellen Änderungen Ihrer Daten gemäß einem von Ihnen definierten Intervall

Mit Azure Data Share haben Datenconsumer folgende Möglichkeiten: 

* Anzeigen einer Beschreibung des Typs der freigegebenen Daten

* Anzeigen der Nutzungsbedingungen für die Daten

* Annehmen oder Ablehnen einer Azure Data Share-Einladung

* Auslösen einer vollständigen oder inkrementellen Momentaufnahme einer Datenfreigabe, die Sie von einer Organisation erhalten haben

* Abonnieren einer Datenfreigabe, um die aktuelle Kopie der Daten per inkrementeller Momentaufnahme zu erhalten

* Akzeptieren von Daten, die für Sie in einem [unterstützten Datenspeicher](supported-data-stores.md) freigegeben werden

Alle wichtigen Funktionen, die oben aufgeführt sind, werden über die Azure- oder REST-APIs unterstützt. Weitere Informationen zur Verwendung von Azure Data Share mit REST-APIs finden Sie in unserer Referenzdokumentation. 

## <a name="security"></a>Sicherheit

Azure Data Share nutzt die zugrunde liegende Sicherheit, die von Azure angeboten wird, um ruhende Daten und Daten während der Übertragung zu schützen. Daten werden im ruhenden Zustand verschlüsselt, wenn dies vom zugrunde liegenden Datenspeicher unterstützt wird. Daten werden auch während der Übertragung verschlüsselt. Metadaten zu einer Datenfreigabe werden im ruhenden Zustand und während der Übertragung ebenfalls verschlüsselt. 

Zugriffssteuerungen können auf der Azure Data Share-Ressourcenebene festgelegt werden, um sicherzustellen, dass nur von berechtigten Personen darauf zugegriffen wird. 

Azure Data Share nutzt verwaltete Identitäten für Azure-Ressourcen (bisher als MSIs bezeichnet), um die automatische Identitätsverwaltung in Azure Active Directory durchzuführen. Verwaltete Identitäten für Azure-Ressourcen werden für den Zugriff auf die Datenspeicher genutzt, die für die Datenfreigabe verwendet werden. Zwischen einem Datenanbieter und einem Datenconsumer findet kein Austausch von Anmeldeinformationen statt. Weitere Informationen finden Sie unter [Dienste, die verwaltete Identitäten für Azure-Ressourcen unterstützen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities). 


## <a name="supported-regions"></a>Unterstützte Regionen

Eine Liste mit Azure-Regionen, in denen Azure Data Share verfügbar ist, finden Sie auf der [Seite mit den verfügbaren Produkten nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=data-share/), wenn Sie nach Azure Data Share suchen. 

Von Azure Data Share selbst werden keine Daten gespeichert. Die Daten werden im zugrunde liegenden Datenspeicher gespeichert, der freigegeben wird. Wenn ein Datenproduzent seine Daten beispielsweise unter einem Azure Data Lake Store-Konto in der Region „USA, Westen“ speichert, werden die Daten auch dort gespeichert. Falls Daten für ein Azure Storage-Konto in der Region „Europa, Westen“ freigegeben werden, werden die Daten direkt an das Azure Storage-Konto übertragen, das sich in „Europa, Westen“ befindet. 

Der Dienst Azure Data Share muss in Ihrer Region nicht verfügbar sein, um den Dienst nutzen zu können. Wenn Sie beispielsweise Daten unter einem Azure Storage-Konto in einer Region gespeichert haben, in der Azure Data Share noch nicht verfügbar ist, können Sie den Dienst trotzdem zum Freigeben Ihrer Daten nutzen. 

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Freigeben von Daten finden Sie im Tutorial zum [Freigeben Ihrer Daten](share-your-data.md).

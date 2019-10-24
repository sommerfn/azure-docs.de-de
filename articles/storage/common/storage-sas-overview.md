---
title: Gewähren von eingeschränktem Zugriff auf Azure Storage-Ressourcen mithilfe von SAS (Shared Access Signature)
description: Erfahren Sie, wie SAS (Shared Access Signatures) verwendet wird, um Zugriff auf Azure Storage-Ressourcen (einschließlich Blobs, Warteschlangen, Tabellen und Dateien) zu delegieren.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 10/14/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 9623152bdea5cc56e6b9bcb7d9911a730fd7a4a4
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72382012"
---
# <a name="grant-limited-access-to-azure-storage-resources-using-shared-access-signatures-sas"></a>Gewähren von eingeschränktem Zugriff auf Azure Storage-Ressourcen mithilfe von SAS (Shared Access Signature)

SAS (Shared Access Signature) ermöglicht den sicheren delegierten Zugriff auf Ressourcen in Ihrem Speicherkonto, ohne die Sicherheit Ihrer Daten zu beeinträchtigen. Mit SAS können Sie genau steuern, wie ein Client auf Ihre Daten zugreifen kann. Sie können steuern, auf welche Ressourcen der Client zugreifen kann, welche Berechtigungen er für diese Ressourcen besitzt und wie lange die SAS gültig ist (neben anderen Parametern).

## <a name="types-of-shared-access-signatures"></a>Arten von Shared Access Signatures

Azure Storage unterstützt drei Arten von Shared Access Signatures:

- **SAS für die Benutzerdelegierung (Vorschau).** Eine SAS für die Benutzerdelegierung wird durch Azure Active Directory-Anmeldeinformationen (Azure AD) sowie durch die für die SAS angegebenen Berechtigungen geschützt. Eine SAS für die Benutzerdelegierung gilt nur für Blobspeicher.

    Weitere Informationen zur SAS für die Benutzerdelegierung finden Sie unter [Erstellen einer SAS für die Benutzerdelegierung (Rest-API)](/rest/api/storageservices/create-user-delegation-sas).

- **Dienst-SAS:** Eine Dienst-SAS wird mit dem Speicherkontoschlüssel geschützt. Eine Dienst-SAS delegiert den Zugriff auf eine Ressource in nur einem der Azure Storage-Dienste: Blob Storage, Queue Storage, Table Storage oder Azure Files. 

    Weitere Informationen zur Dienst-SAS finden Sie unter [Erstellen einer Dienst-SAS (Rest-API)](/rest/api/storageservices/create-service-sas).

- **Konto-SAS:** Eine Konto-SAS wird mit dem Speicherkontoschlüssel geschützt. Eine Konto-SAS delegiert den Zugriff auf Ressourcen in einem oder mehreren der Speicherdienste. Alle Vorgänge, die über eine Dienst-SAS oder eine SAS für die Benutzerdelegierung verfügbar sind, sind auch über eine Konto-SAS verfügbar. Mit der Konto-SAS können Sie außerdem den Zugriff auf Vorgänge delegieren, die für eine bestimmte Ebene des Diensts gelten, z.B. **Get/Set Service-Eigenschaften** und **Get Service Stats**-Vorgänge. Sie können auch den Zugriff auf Lese-, Schreib- und Löschvorgänge in Blob-Containern, Tabellen, Warteschlangen und Dateifreigaben delegieren, die mit einer Dienst-SAS nicht zulässig sind. 

    Weitere Informationen zur Konto-SAS finden Sie unter [Erstellen einer Konto-SAS (Rest-API)](/rest/api/storageservices/create-account-sas).

> [!NOTE]
> Microsoft empfiehlt als bewährte Methode, nach Möglichkeit Azure AD-Anmeldeinformationen anstelle des Kontoschlüssels zu verwenden, der leichter kompromittiert werden kann. Wenn ihr Anwendungsentwurf Shared Access Signatures für den Zugriff auf Blobspeicher erfordert, verwenden Sie Azure AD-Anmeldeinformationen, um nach Möglichkeit eine SAS für die Benutzerdelegierung zu erstellen und damit die Sicherheit zu erhöhen.

Shared Access Signatures können zwei unterschiedliche Formen haben:

- **Ad-hoc-SAS:** Beim Erstellen einer Ad-hoc-SAS werden Startzeit, Ablaufzeit und Berechtigungen für die SAS im SAS-URI angegeben (bzw. impliziert, falls die Startzeit ausgelassen wird). Jede Art von SAS kann eine Ad-hoc-SAS sein.
- **Dienst-SAS mit gespeicherter Zugriffsrichtlinie:** Eine gespeicherte Zugriffsrichtlinie wird für einen Ressourcencontainer definiert, z.B. einen Blobcontainer, eine Tabelle, eine Warteschlange oder eine Dateifreigabe. Die gespeicherte Zugriffsrichtlinie kann verwendet werden, um Einschränkungen für mindestens eine SAS zu verwalten. Wenn Sie eine Dienst-SAS mit einer gespeicherten Zugriffsrichtlinie verknüpfen, erbt die SAS die Einschränkungen &mdash;(Startzeit, Ablaufzeit und Berechtigungen)&mdash;, die für diese gespeicherte Zugriffsrichtlinie definiert sind.

> [!NOTE]
> Eine SAS für die Benutzerdelegierung oder eine Konto-SAS muss eine Ad-hoc-SAS sein. Gespeicherte Zugriffsrichtlinien werden für die SAS für die Benutzerdelegierung oder die Konto-SAS nicht unterstützt.

## <a name="how-a-shared-access-signature-works"></a>Funktionsweise von Shared Access Signatures

Eine Shared Access Signature ist ein signierter URI, der auf eine oder mehrere Speicherressourcen verweist und über ein Token verfügt, das einen speziellen Satz von Abfrageparametern enthält. Das Token gibt an, wie der Client auf die Ressourcen zugreifen kann. Einer der Abfrageparameter ist die Signatur. Sie besteht aus den SAS-Parametern und wird mit dem Schlüssel signiert, der zum Erstellen der SAS verwendet wurde. Diese Signatur wird von Azure Storage verwendet, um den Zugriff auf die Speicherressource zu autorisieren.

### <a name="sas-signature"></a>SAS-Signatur

Zum Signieren einer SAS stehen zwei Möglichkeiten zur Verfügung:

- Mit einem *Benutzerdelegierungsschlüssel*, der mit Azure Active Directory-Anmeldeinformationen (Azure AD) erstellt wurde. Eine SAS für die Benutzerdelegierung wird mit dem Benutzerdelegierungsschlüssel signiert.

    Um den Benutzerdelegierungsschlüssel abzurufen und die SAS zu erstellen, muss einem Azure AD-Sicherheitsprinzipal rollenbasierte Zugriffssteuerung (RBAC) zugewiesen werden, die die Aktion **Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey** beinhaltet. Ausführliche Informationen zu RBAC-Rollen mit Berechtigungen zum Abrufen des Benutzerdelegierungsschlüssels finden Sie unter [Erstellen einer SAS für die Benutzerdelegierung (Rest-API)](/rest/api/storageservices/create-user-delegation-sas).

- Mit dem Speicherkontoschlüssel. Sowohl eine Dienst-SAS als auch eine Konto-SAS werden mit dem Speicherkontoschlüssel signiert. Zum Erstellen einer SAS, die mit dem Kontoschlüssel signiert ist, muss eine Anwendung Zugriff auf den Kontoschlüssel besitzen.

### <a name="sas-token"></a>SAS-Token

Das SAS-Token ist eine Zeichenfolge, die Sie auf der Clientseite generieren, z.B. mit einer der Azure Storage-Clientbibliotheken. Das SAS-Token wird von Azure Storage in keiner Weise nachverfolgt. Sie können auf der Clientseite eine unbegrenzte Anzahl von SAS-Token erstellen. Nachdem Sie eine SAS erstellt haben, können Sie sie an Clientanwendungen verteilen, die Zugriff auf Ressourcen in Ihrem Speicherkonto benötigen.

Wenn eine Clientanwendung im Rahmen einer Anforderung einen SAS-URI für Azure Storage bereitstellt, überprüft der Dienst die SAS-Parameter und die Signatur, um sicherzustellen, dass diese Angaben für die Authentifizierung der Anforderung gültig sind. Wenn der Dienst bestätigt, dass die Signatur gültig ist, wird die Anforderung autorisiert. Andernfalls wird die Anforderung mit Fehlercode 403 (Unzulässig) abgelehnt.

Hier ist ein Beispiel für einen Dienst-SAS-URI mit dem Ressourcen-URI und dem SAS-Token:

![Komponenten eines Dienst-SAS-URIs](./media/storage-sas-overview/sas-storage-uri.png)

## <a name="when-to-use-a-shared-access-signature"></a>Wann sollte eine Shared Access Signature verwendet werden?

Verwenden Sie eine SAS, wenn Sie sicheren Zugriff auf Ressourcen in Ihrem Speicherkonto für jeden Client bereitstellen möchten, der nicht anderweitig über Berechtigungen für diese Ressourcen verfügt.

SAS sind zum Beispiel dann hilfreich, wenn Benutzer ihre eigenen Daten in Ihrem Speicherkonto ablegen und von dort abrufen. Für den Fall, dass ein Speicherkonto Benutzerdaten enthält, existieren zwei typische Designmuster:

1. Clients laden Daten über einen Front-End-Proxydienst hoch und herunter, der die Authentifizierung übernimmt. Dieser Front-End-Proxydienst hat den Vorteil, dass auch Geschäftsregeln validiert werden können. Allerdings kann die Erstellung eines skalierbaren Dienstes für große Datenmengen oder Transaktionen mit großem Volumen teuer und aufwändig sein.

   ![Szenariodiagramm: Front-End-Proxydienst](./media/storage-sas-overview/sas-storage-fe-proxy-service.png)

1. Ein einfacher Dienst authentifiziert den Client nach Bedarf und generiert anschließend die SAS. Sobald die Clientanwendung die SAS erhält, kann sie direkt gemäß den in der SAS definierten Berechtigungen für den definierten Zeitraum auf die Speicherkontoressourcen zugreifen. Dank der SAS müssen nicht mehr alle Daten durch einen Front-End-Proxydienst geleitet werden.

   ![Szenariodiagramm: SAS-Anbieterdienst](./media/storage-sas-overview/sas-storage-provider-service.png)

Für viele tatsächlich arbeitende Dienste wird möglicherweise eine Mischung dieser beiden Ansätze verwendet. Beispielsweise kann es sein, dass einige Daten über den Front-End-Proxy verarbeitet und überprüft werden, während andere Daten direkt mit SAS gespeichert und/oder gelesen werden.

Darüber hinaus ist eine SAS erforderlich, um in bestimmten Szenarien bei einem Kopiervorgang den Zugriff auf das Quellobjekt zu autorisieren:

- Wenn Sie ein Blob in ein anderes Blob kopieren, das sich in einem anderen Speicherkonto befindet, müssen Sie eine SAS verwenden, um den Zugriff auf das Quellblob zu autorisieren. Sie können optional eine SAS verwenden, um auch den Zugriff auf das Zielblob zu autorisieren.
- Wenn Sie eine Datei in eine andere Datei kopieren, das sich in einem anderen Speicherkonto befindet, müssen Sie eine SAS verwenden, um den Zugriff auf die Quelldatei zu autorisieren. Sie können optional eine SAS verwenden, um auch den Zugriff auf die Zieldatei zu autorisieren.
- Wenn Sie ein Blob in eine Datei oder eine Datei in ein Blob kopieren, müssen Sie eine SAS verwenden, um den Zugriff auf das Quellobjekt zu autorisieren. Das gilt auch, wenn sich Quell- und Zielobjekt im gleichen Speicherkonto befinden.

## <a name="best-practices-when-using-sas"></a>Bewährte Methoden bei Verwendung von SAS

Wenn Sie Shared Access Signatures in Ihren Anwendungen verwenden, müssen Sie sich der potenziellen Risiken bewusst sein:

- Weitergegebene SAS können von beliebiger Stelle verwendet werden, was die Sicherheit Ihres Speicherkontos gefährden kann.
- Wenn die SAS einer Clientanwendung abläuft und die Anwendung keine neue SAS von Ihrem Dienst abrufen kann, wird die Funktionsweise der Anwendung möglicherweise eingeschränkt.

Mit den folgenden Empfehlungen für die Verwendung von Shared Access Signatures können Sie diese Risiken verringern:

- **Verwenden Sie immer HTTPS** für die Erstellung oder Verteilung einer SAS. Wenn eine SAS über HTTP weitergegeben und abgefangen wird, kann ein Angreifer mit einem Man-in-the-Middle-Angriff diese auslesen, anschließend im Namen des Benutzers verwenden und somit möglicherweise sensible Daten gefährden oder böswillig beschädigen.
- **Verwenden Sie nach Möglichkeit eine SAS für die Benutzerdelegierung.** Eine SAS für die Benutzerdelegierung bietet überragende Sicherheit für eine Dienst-SAS oder eine Konto-SAS. Eine SAS für die Benutzerdelegierung ist durch Azure AD-Anmeldeinformationen geschützt, sodass Sie Ihren Kontoschlüssel nicht mit Ihrem Code speichern müssen.
- **Halten Sie einen Sperrungsplan für eine SAS bereit.** Stellen Sie sicher, dass Sie darauf vorbereitet sind, unverzüglich zu reagieren, wenn eine SAS gefährdet ist.
- **Definieren Sie eine gespeicherte Zugriffsrichtlinie für eine Dienst-SAS.** Gespeicherte Zugriffsrichtlinien bieten Ihnen die Möglichkeit, Berechtigungen für eine Dienst-SAS zu widerrufen, ohne dafür die Speicherkontoschlüssel erneut generieren zu müssen. Legen Sie das Ablaufdatum für diese sehr weit in die Zukunft (oder unendlich) fest, und stellen Sie sicher, dass das Datum regelmäßig aktualisiert wird, um es weiter in die Zukunft zu verschieben.
- **Verwenden Sie Ablaufdaten in naher Zukunft für eine Ad-hoc-Dienst- oder -Konto-SAS.** Auf diese Weise ist eine SAS, selbst wenn sie gefährdet ist, nur für kurze Zeit gültig. Dies ist besonders dann wichtig, wenn Sie nicht auf eine gespeicherte Zugriffsrichtlinie verweisen können. Kurzfristige Ablaufzeiten beschränken auch die Datenmenge, die in einen Blob geschrieben werden kann, indem sie die Zeit verkürzen, die ein Blob für Uploads verfügbar ist.
- **Sorgen Sie dafür, dass die Clients die SAS bei Bedarf automatisch erneuern müssen.** Die Clients sollten ihre SAS rechtzeitig vor der Ablaufzeit erneuern, um Zeit für Wiederholungsversuche zu bieten, falls der entsprechende Dienst nicht verfügbar sein sollte. Falls Ihre SAS für eine kleine Anzahl sofortiger und kurzfristiger Operationen gilt, die normalerweise innerhalb des Ablaufzeitraums abgeschlossen werden, ist dies möglicherweise nicht notwendig, da die SAS nicht erneuert werden müssen. Wenn Ihre Clients jedoch immer wieder Anfragen über die SAS stellen, müssen Sie sich mit dem Ablaufmechanismus auseinander setzen. Dabei müssen Sie einen Ausgleich zwischen der Notwendigkeit einer kurzlebigen SAS (wie zuvor beschrieben) und dem Bedarf des Clients schaffen, diese rechtzeitig zu erneuern, um zu verhindern, dass die SAS vor der erfolgreichen Erneuerung abläuft.
- **Seien Sie vorsichtig mit der SAS-Startzeit.** Wenn Sie die Startzeit einer SAS auf **jetzt** setzen, können aufgrund von Zeitunterschieden zwischen unterschiedlichen Computern in den ersten Minuten Probleme auftreten. Üblicherweise sollten Sie als Startzeit eine Uhrzeit angeben, die mindestens 15 Minuten in der Vergangenheit liegt. Alternativ legen Sie gar keine Startzeit fest, wodurch sie in allen Fällen sofort wirksam wird. Dasselbe gilt für die Ablaufzeit. Rechnen Sie immer mit Zeitunterschieden von bis zu 15 Minuten in beide Richtungen bei jeder Anforderung. Für Clients mit einer REST-Version vor 2012-02-12 ist die maximale Dauer für eine SAS, in der nicht auf eine gespeicherte Zugriffsrichtlinie verwiesen wird, eine Stunde. Alle Richtlinien, die längere Zeiträume verwenden, schlagen fehl.
- **Geben Sie die freigegebene Ressource exakt an.** Aus Sicherheitsgründen sollten Benutzer nur die minimal erforderlichen Berechtigungen erhalten. Wenn ein Benutzer nur Lesezugriff auf eine einzige Entität benötigt, dann geben Sie auch nur Lesezugriff auf diese Entität, und nicht Lese-/Schreib-/Löschzugriff auf alle Entitäten. So lässt sich auch der Schaden verringern, wenn eine SAS kompromittiert wurde, denn die SAS bietet dem Angreifer weniger Angriffsfläche.
- **Beachten Sie, dass Ihnen jegliche Nutzung Ihres Kontos berechnet wird, inklusive der Nutzung über SAS.** Wenn Sie Schreibzugriff auf ein Blob bereitstellen, können Benutzer Blobs mit bis zu 200 GB hochladen. Falls Sie außerdem noch Lesezugriff vergeben, können die Benutzer die Daten bis zu zehnmal herunterladen und Gebühren für den Datenausgang von bis zu 2 TB verursachen. Vergeben Sie also auch hierbei eingeschränkte Berechtigungen, um die möglichen Aktionen böswilliger Benutzer abzuschwächen. Verwenden Sie kurzlebige SAS, um diese Bedrohung zu mindern (beachten Sie jedoch mögliche Zeitunterschiede bei der Ablaufzeit).
- **Überprüfen Sie geschriebene Daten mit einer SAS.** Wenn Clientanwendungen Daten in Ihr Speicherkonto schreiben, müssen Sie stets beachten, dass diese Daten problembehaftet sein können. Wenn Ihre Anwendung diese Daten vor der Verwendung überprüfen oder autorisieren muss, sollten Sie diese Überprüfung durchführen, nachdem die Daten geschrieben und bevor sie von Ihrer Anwendung verwendet werden. Auf diese Weise schützen Sie Ihr Konto auch vor beschädigten oder bösartigen Daten, sowohl von tatsächlich berechtigten SAS-Benutzern als auch von Angreifern, die eine abgefangene SAS verwenden.
- **Informieren Sie sich darüber, wann keine SAS verwendet wird.** Manchmal überwiegen die Risiken eines bestimmten Vorgangs für Ihr Speicherkonto gegenüber den Vorzügen der Verwendung einer SAS. Für solche Operation sollten Sie einen Dienst auf der mittleren Ebene erstellen, der zunächst Geschäftsregeln validiert sowie Authentifizierung und Überwachung durchführt und die Daten anschließend in Ihr Speicherkonto schreibt. Manchmal gibt es auch einfachere Möglichkeiten der Zugriffsverwaltung. Wenn Sie beispielsweise alle Blobs in einem Container öffentlich lesbar machen möchten, können Sie auch den Container öffentlich machen, anstatt jedem Client für den Zugriff eine SAS zu geben.
- **Verwenden Sie Azure Monitor- und Azure Storage-Protokolle, um Ihre Anwendung zu überwachen.** Sie können mit Azure Monitor und Storage Analytics-Protokollen Häufungen von Authentifizierungsfehlern aufgrund von Ausfällen Ihres SAS-Anbieterdiensts oder einer unbeabsichtigt gelöschten gespeicherten Zugriffsrichtlinie beobachten. Weitere Informationen finden Sie unter [Azure Storage-Metriken in Azure Monitor](storage-metrics-in-azure-monitor.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) und [Azure Storage Analytics-Protokollierung](storage-analytics-logging.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="get-started-with-sas"></a>Erste Schritte mit SAS

Informationen zu den ersten Schritten mit Shared Access Signatures finden Sie in den folgenden Artikeln zu den einzelnen SAS-Typen.

### <a name="user-delegation-sas"></a>SAS für die Benutzerdelegierung

- [Erstellen einer SAS für die Benutzerdelegierung für einen Container oder ein Blob mit PowerShell (Vorschau)](../blobs/storage-blob-user-delegation-sas-create-powershell.md)
- [Erstellen einer SAS für die Benutzerdelegierung für einen Container oder ein Blob mit der Azure CLI (Vorschau)](../blobs/storage-blob-user-delegation-sas-create-cli.md)
- [Erstellen einer SAS für die Benutzerdelegierung für einen Container oder ein Blob mit .NET (Vorschau)](../blobs/storage-blob-user-delegation-sas-create-dotnet.md)

### <a name="service-sas"></a>Dienst-SAS

- [Erstellen einer Dienst-SAS für einen Container oder ein Blob mit .NET](../blobs/storage-blob-service-sas-create-dotnet.md)

### <a name="account-sas"></a>Konto-SAS

- [Erstellen einer Konto-SAS mit .NET](storage-account-sas-create-dotnet.md)

## <a name="next-steps"></a>Nächste Schritte

- [Delegieren des Zugriffs mit einer Shared Access Signature (REST API)](/rest/api/storageservices/delegate-access-with-shared-access-signature)
- [Erstellen einer SAS für die Benutzerdelegierung (REST-API)](/rest/api/storageservices/create-user-delegation-sas)
- [Erstellen einer Dienst-SAS (REST-API)](/rest/api/storageservices/create-service-sas)
- [Erstellen einer Konto-SAS (REST-API)](/rest/api/storageservices/create-account-sas)

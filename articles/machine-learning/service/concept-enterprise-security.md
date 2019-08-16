---
title: Unternehmenssicherheit
titleSuffix: Azure Machine Learning service
description: 'Verwenden Sie sicher Azure Machine Learning Service: Authentifizierung, Autorisierung, Netzwerksicherheit, Datenverschlüsselung und Überwachung.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 07/10/2019
ms.openlocfilehash: f0fb6f0d2b2579679ee8a6ec43b3241377701d48
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780899"
---
# <a name="enterprise-security-for-azure-machine-learning-service"></a>Unternehmenssicherheit für Azure Machine Learning Service

In diesem Artikel lernen Sie Sicherheitsfeatures kennen, die mit Azure Machine Learning Service zur Verfügung stehen.

Wenn Sie einen Clouddienst verwenden, sollten Sie die bewährte Methode anwenden, den Zugriff auf die Benutzer zu beschränken, die ihn benötigen. Dies beginnt damit, das vom Dienst verwendete Authentifizierungs- und Autorisierungsmodell zu verstehen. Sie sollten auch den Zugriff auf das Netzwerk einschränken oder Ressourcen in Ihrem lokalen Netzwerk sicher mit denen in der Cloud verknüpfen. Die Datenverschlüsselung ist auch entscheidend, sowohl im Ruhezustand als auch während des Verschiebens der Daten zwischen Diensten. Schließlich müssen Sie in der Lage sein, den Dienst zu überwachen und ein Überwachungsprotokoll aller Aktivitäten zu erstellen.

## <a name="authentication"></a>Authentication

Multi-Factor Authentication wird unterstützt, wenn Azure Active Directory (Azure AD) hierfür konfiguriert ist.

* Der Client meldet sich bei Azure AD an und ruft das Azure Resource Manager-Token ab.  Benutzer und Dienstprinzipale werden vollständig unterstützt.
* Der Client präsentiert das Token dem Azure Resource Manager und allen Azure Machine Learning-Diensten.
* Azure Machine Learning Service stellt ein Azure Machine Learning-Token für das Benutzercompute bereit. Beispiel: Machine Learning Compute. Dieses Azure Machine Learning-Token wird nach Abschluss der Ausführung vom Benutzercompute für einen Rückruf in Azure Machine Learning Service (beschränkt den Bereich auf den Arbeitsbereich) verwendet.

![Screenshot zeigt Funktionsweise der Authentifizierung in Azure Machine Learning Service](./media/enterprise-readiness/authentication.png)

### <a name="authentication-for-web-service-deployment"></a>Authentifizierung für die Webdienstbereitstellung

Azure Machine Learning unterstützt zwei Authentifizierungsarten für Webdienste: Schlüssel und Token. Die einzelnen Webdienste können jeweils nur eine Authentifizierungsart verwenden.

|Authentifizierungsmethode|ACI|AKS|
|---|---|---|
|Schlüssel|Standardmäßig deaktiviert| Standardmäßig aktiviert|
|Tokenverschlüsselung| Nicht verfügbar| Standardmäßig deaktiviert |

#### <a name="authentication-with-keys"></a>Authentifizierung mit Schlüsseln

Wenn Sie die Schlüsselauthentifizierung für eine Bereitstellung aktivieren, werden automatisch Authentifizierungsschlüssel erstellt.

* Die Authentifizierung ist standardmäßig aktiviert, wenn die Bereitstellung in Azure Kubernetes Service erfolgt.
* Die Authentifizierung ist standardmäßig deaktiviert, wenn die Bereitstellung in Azure Container Instances erfolgt.

Wenn Sie die Schlüsselauthentifizierung aktivieren möchten, verwenden Sie beim Erstellen oder Aktualisieren einer Bereitstellung den Parameter `auth_enabled`.

Bei aktivierter Schlüsselauthentifizierung können Sie mithilfe der Methode `get_keys` einen primären und einen sekundären Authentifizierungsschlüssel abrufen:

```python
primary, secondary = service.get_keys()
print(primary)
```

> [!IMPORTANT]
> Wenn Sie einen Schlüssel erneut generieren müssen, verwenden Sie [`service.regen_key`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py).

#### <a name="authentication-with-tokens"></a>Authentifizierung mit Tokens

Wenn Sie die Tokenauthentifizierung für einen Webdienst aktivieren, muss ein Benutzer ein Azure Machine Learning-JSON-Webtoken für den Webdienst bereitstellen, um auf den Dienst zugreifen zu können.

* Die Tokenauthentifizierung ist standardmäßig deaktiviert, wenn die Bereitstellung in Azure Kubernetes Service erfolgt.
* Die Tokenauthentifizierung wird nicht unterstützt, wenn die Bereitstellung in Azure Container Instances erfolgt.

Um die Tokenauthentifizierung zu steuern, verwenden Sie beim Erstellen oder Aktualisieren einer Bereitstellung den Parameter `token_auth_enabled`.

Bei aktivierter Tokenauthentifizierung können Sie mithilfe der Methode `get_token` ein JWT-Token und dessen Ablaufzeit abrufen:

```python
token, refresh_by = service.get_token()
print(token)
```

> [!IMPORTANT]
> Nach Ablauf der für `refresh_by` festgelegten Zeit müssen Sie ein neues Token anfordern.
>
> Es wird dringend empfohlen, den Azure Machine Learning-Arbeitsbereich in der gleichen Region zu erstellen wie den Azure Kubernetes Service-Cluster. Im Zuge der Tokenauthentifizierung richtet der Webdienst einen Aufruf an die Region, in der Ihr Azure Machine Learning-Arbeitsbereich erstellt wird. Ist die Region Ihres Arbeitsbereichs nicht verfügbar, können Sie kein Token für Ihren Webdienst abrufen (auch dann nicht, wenn sich Ihr Cluster in einer anderen Region befindet als Ihr Arbeitsbereich). Die Azure AD-Authentifizierung ist dann erst wieder verfügbar, wenn die Region Ihres Arbeitsbereichs wieder verfügbar wird. Außerdem wirkt sich die Entfernung zwischen der Region Ihres Clusters und der Region Ihres Arbeitsbereichs direkt auf die Tokenabrufdauer aus.

## <a name="authorization"></a>Authorization

Sie können mehrere Arbeitsbereiche erstellen, und jeder Arbeitsbereich kann von mehreren Benutzern gemeinsam genutzt werden. Den Zugriff auf einen freigegebenen Arbeitsbereich steuern Sie, indem Sie Benutzern die folgenden Rollen zuweisen:

* Owner (Besitzer)
* Mitwirkender
* Leser

Die folgende Tabelle enthält einige der wichtigsten Azure Machine Learning Service-Vorgänge und die Rollen, die sie ausführen können:

| Azure Machine Learning Service-Vorgang | Owner (Besitzer) | Mitwirkender | Leser |
| ---- |:----:|:----:|:----:|
| Arbeitsbereich erstellen | ✓ | ✓ | |
| Arbeitsbereich freigeben | ✓ | |  |
| Compute erstellen | ✓ | ✓ | |
| Compute anfügen | ✓ | ✓ | |
| Datenspeicher anfügen | ✓ | ✓ | |
| Experiment ausführen | ✓ | ✓ | |
| Ausführungen/Metriken anzeigen | ✓ | ✓ | ✓ |
| Registrieren des Modells | ✓ | ✓ | |
| Image erstellen | ✓ | ✓ | |
| Bereitstellen des Webdiensts | ✓ | ✓ | |
| Modelle/Images anzeigen | ✓ | ✓ | ✓ |
| Webdienst aufrufen | ✓ | ✓ | ✓ |

Wenn die integrierten Rollen für Ihre Zwecke nicht ausreichend sind, können Sie auch benutzerdefinierte Rollen erstellen. Wir unterstützen nur benutzerdefinierte Rollen für Vorgänge im Arbeitsbereich und Machine Learning Compute. Die benutzerdefinierten Rollen verfügen möglicherweise über Lese-, Schreib- oder Löschberechtigungen für den Arbeitsbereich und die Computeressource in diesem Arbeitsbereich. Sie können die Rolle auf einer bestimmten Arbeitsbereichsebene, einer bestimmten Ressourcengruppenebene oder einer bestimmten Abonnementebene verfügbar machen. Weitere Informationen finden Sie unter [Verwalten von Benutzern und Rollen in einem Azure Machine Learning-Arbeitsbereich](how-to-assign-roles.md).

### <a name="securing-compute-and-data"></a>Sichern von Compute und Daten

Besitzer und Mitwirkende können alle Computeziele und Datenspeicher verwenden, die dem Arbeitsbereich angefügt sind.  
Jeder Arbeitsbereich verfügt auch über eine zugeordnete, vom System zugewiesene verwaltete Identität (mit demselben Namen wie der Arbeitsbereich) mit den folgenden Berechtigungen für die angefügten Ressourcen, die im Arbeitsbereich verwendet werden:

Weitere allgemeine Informationen zu verwalteten Identitäten finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

| Resource | Berechtigungen |
| ----- | ----- |
| Arbeitsbereich | Mitwirkender |
| Speicherkonto | Mitwirkender an Speicherblobdaten |
| Key Vault | Zugriff auf alle Schlüssel, Geheimnisse und Zertifikate |
| Azure Container Registry | Mitwirkender |
| Ressourcengruppe, die den Arbeitsbereich enthält | Mitwirkender |
| Ressourcengruppe, die die Key Vault-Instanz enthält (wenn nicht mit der identisch, die den Arbeitsbereich enthält) | Mitwirkender |

Administratoren sollten den Zugriff der verwalteten Identität auf die oben genannten Ressourcen nicht widerrufen. Der Zugriff kann mit dem Vorgang „Schlüssel neu synchronisieren“ wiederhergestellt werden.

Azure Machine Learning Service erstellt eine weitere Anwendung (deren Name mit `aml-` beginnt) mit Zugriff auf Mitwirkendenebene in Ihrem Abonnement für jede Region des Arbeitsbereichs. Beispiel: Wenn Sie im selben Abonnement über einen Arbeitsbereich in „USA, Osten“ und einen anderen Arbeitsbereich in „Europa, Norden“ verfügen, sehen Sie zwei solcher Anwendungen. Dies ist erforderlich, damit Azure Machine Learning Service Computeressourcen verwalten kann.

## <a name="network-security"></a>Netzwerksicherheit

Azure Machine Learning Service ist in Sachen Computeressourcen auf andere Azure-Dienste angewiesen. Computeressourcen (Computeziele) dienen zum Trainieren und Bereitstellen von Modellen. Diese Computeziele können innerhalb eines virtuellen Netzwerks erstellt werden. So können Sie beispielsweise eine Microsoft Data Science VM verwenden, um ein Modell zu trainieren, und das Modell anschließend in Azure Kubernetes Service (AKS) bereitstellen.  

Weitere Informationen finden Sie unter [Sicheres Ausführen von Experimenten und Ziehen von Rückschlüssen innerhalb eines virtuellen Azure-Netzwerks](how-to-enable-virtual-network.md).

## <a name="data-encryption"></a>Datenverschlüsselung

### <a name="encryption-at-rest"></a>Verschlüsselung ruhender Daten

#### <a name="azure-blob-storage"></a>Azure Blob Storage

Azure Machine Learning Service speichert Momentaufnahmen, Ausgaben und Protokolle in dem Azure Blob Storage-Konto, das mit dem Arbeitsbereich von Azure Machine Learning Service verknüpft ist und sich im Abonnement des Benutzers befindet. Alle in Azure Blob Storage gespeicherten Daten werden im Ruhezustand mithilfe von Microsoft verwalteter Schlüssel verschlüsselt.

Weitere Informationen dazu, wie Sie Ihre eigenen Schlüssel für die in Azure Blob Storage gespeicherten Daten verwenden können, finden Sie unter [Speicherdienstverschlüsselung mit vom Kunden verwalteten Schlüsseln in Azure Key Vault](https://docs.microsoft.com/azure/storage/common/storage-service-encryption-customer-managed-keys).

Trainingsdaten werden in der Regel auch in Azure Blob Storage gespeichert, damit sie für das Trainingscompute zugänglich sind. Dieser Speicher wird nicht von Azure Machine Learning verwaltet, jedoch dem Compute als Remotedateisystem bereitgestellt.

Informationen zum Regenerieren der Zugriffsschlüssel für die in Ihrem Arbeitsbereich verwendeten Azure-Speicherkonten finden Sie im Artikel [Zugriffsschlüssel für Speicherkonten neu generieren](how-to-change-storage-access-key.md).

#### <a name="cosmos-db"></a>Cosmos DB

Azure Machine Learning Service speichert Metriken und Metadaten in Cosmos DB, die in einem Microsoft-Abonnement aktiv sind, das von Azure Machine Learning Service verwaltet wird. Alle in Cosmos DB gespeicherten Daten werden im Ruhezustand mithilfe von Microsoft verwalteter Schlüssel verschlüsselt.

#### <a name="azure-container-registry-acr"></a>Azure Container Registry (ACR)

Alle Containerimages in Ihrer Registrierung (ACR) werden im Ruhezustand verschlüsselt. Azure verschlüsselt ein Image automatisch, bevor es gespeichert wird, und entschlüsselt es dynamisch, wenn Azure Machine Learning Service das Image pullt.

#### <a name="machine-learning-compute"></a>Machine Learning Compute

Der Betriebssystem-Datenträger wird für jeden Computeknoten in Azure Storage gespeichert und mit von Microsoft verwalteten Schlüsseln in Speicherkonten von Azure Machine Learning Service verschlüsselt. Dieses Compute ist kurzlebig, und Cluster werden in der Regel zentral herunterskaliert, wenn keine Ausführungen in der Warteschlange stehen. Die Bereitstellung des zugrunde liegenden virtuellen Computers wird aufgehoben und der Betriebssystem-Datenträger gelöscht. Die Azure-Datenträgerverschlüsselung wird für den Betriebssystem-Datenträger nicht unterstützt.
Jeder virtuelle Computer verfügt auch über einen lokalen temporären Datenträger für Betriebssystem-Vorgänge. Dieser Datenträger kann auch optional zum Bereitstellen von Trainingsdaten verwendet werden. Dieser Datenträger ist nicht verschlüsselt.
Weitere Informationen zur Verschlüsselung ruhender Daten in Azure finden Sie unter [Azure-Datenverschlüsselung ruhender Daten](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest).

### <a name="encryption-in-transit"></a>Verschlüsselung während der Übertragung

Sowohl die interne Kommunikation zwischen verschiedenen Azure Machine Learning-Microservices als auch die externe Kommunikation des Aufrufens des Bewertungsendpunkts werden mit SSL unterstützt. Der gesamte Azure Storage-Zugriff erfolgt auch über einen sicheren Kanal.
Weitere Informationen finden Sie unter [Verwenden von SSL zum Schützen von Webdiensten mit Azure Machine Learning Service](https://docs.microsoft.com/azure/machine-learning/service/how-to-secure-web-service).

### <a name="using-azure-key-vault"></a>Verwenden von Azure Key Vault

Die mit dem Arbeitsbereich verknüpfte Key Vault-Instanz wird von Azure Machine Learning Service zum Speichern von Anmeldeinformationen verschiedener Art verwendet:

* Der verknüpften Speicherkonto-Verbindungszeichenfolge
* Kennwörter in Azure Container Repository-Instanzen
* Verbindungszeichenfolgen zur Verbindung mit Datenspeichern.

SSH-Kennwörter und Schlüssel für Computeziele wie z.B. HDI HDInsight und virtuelle Computer werden in einer separaten Key Vault-Instanz gespeichert, die einem Microsoft-Abonnement zugeordnet ist. Azure Machine Learning Service speichert keine vom Benutzer bereitgestellten Kennwörter oder Schlüssel, sondern generiert, autorisiert und speichert stattdessen seine eigenen SSH-Schlüssel, um die Verbindung mit der VM/HDInsight zum Ausführen von Experimenten herzustellen.
Jeder Arbeitsbereich verfügt über eine zugeordnete, vom System zugewiesene verwaltete Identität (mit demselben Namen wie der Arbeitsbereich), die Zugriff auf alle Schlüssel, Geheimnisse und Zertifikate in der Key Vault-Instanz hat.

## <a name="monitoring"></a>Überwachung

Benutzer können das Aktivitätsprotokoll unter dem Arbeitsbereich anzeigen, um die verschiedenen Vorgänge zu sehen, die für den Arbeitsbereich ausgeführt werden, und die grundlegenden Informationen wie den Namen des Vorgangs, wodurch das Ereignis initiiert wurde, den Zeitstempel usw. zu erhalten.

Der folgende Screenshot zeigt das Aktivitätsprotokoll für einen Arbeitsbereich:

![Screenshot zeigt Aktivitätsprotokoll unter einem Arbeitsbereich](./media/enterprise-readiness/workspace-activity-log.png)

Details der Bewertungsanforderung werden in AppInsights gespeichert, das während des Erstellens des Arbeitsbereichs im Abonnement des Benutzers erstellt wird. Dies beinhaltet Felder wie HTTPMethod, UserAgent, ComputeType, RequestUrl, StatusCode, RequestId, Duration usw.

## <a name="data-flow-diagram"></a>Datenflussdiagramm

### <a name="create-workspace"></a>Arbeitsbereich erstellen

Das folgende Diagramm zeigt den Workflow des Erstellens des Arbeitsbereichs.
Der Benutzer meldet sich von einem der unterstützten Azure Machine Learning Service-Clients (CLI, Python, SDK, Azure-Portal) aus bei Azure AD an und fordert das entsprechende Azure Resource Manager-Token an. Dann ruft der Benutzer den Azure Resource Manager auf, um den Arbeitsbereich zu erstellen.  Der Azure Resource Manager kontaktiert den Ressourcenanbieter von Azure Machine Learning Service, um den Arbeitsbereich bereitzustellen.  Zusätzliche Ressourcen werden bei der Erstellung des Arbeitsbereichs im Abonnement des Kunden erstellt:

* Key Vault-Instanz (zum Speichern von Geheimnissen)
* Ein Azure Storage-Konto (einschließlich Blob und FileShare)
* Azure Container Registry (zum Speichern von Docker-Images für Rückschlüsse/Bewertungen und Experimente)
* Application Insights (zum Speichern von Telemetriedaten)

Andere Computes, die einem Arbeitsbereich (Azure Kubernetes Service, VM usw.) angefügt sind, können auch nach Bedarf von Kunden bereitgestellt werden.

![Screenshot zeigt den Workflow des Erstellens des Arbeitsbereichs](./media/enterprise-readiness/create-workspace.png)

### <a name="save-source-code-training-scripts"></a>Speichern des Quellcodes (Trainingsskripts)

Das folgende Diagramm zeigt den Workflow der Codemomentaufnahme.
Mit einem Arbeitsbereich von Azure Machine Learning Service sind Verzeichnisse (Experimente) verknüpft, die den Quellcode (Trainingsskripts) enthalten.  Diese werden auf dem lokalen Computer des Kunden und in der Cloud (in Azure Blob Storage unter dem Abonnement des Kunden) gespeichert. Diese Codemomentaufnahmen werden für die Ausführung oder Überprüfung für die Verlaufsüberwachung verwendet.

![Screenshot zeigt den Workflow des Erstellens des Arbeitsbereichs](./media/enterprise-readiness/code-snapshot.png)

### <a name="training"></a>Training

Das folgende Diagramm zeigt den Trainingsworkflow.

* Azure Machine Learning Service wird mit der Momentaufnahmen-ID für die oben gespeicherte Codemomentaufnahme aufgerufen
* Der Azure Machine Learning Service erstellt eine Ausführungs-ID (optional) und ein Azure Machine Learning Service-Token, das später von Computezielen wie Machine Learning Compute/VM zur Kommunikation mit Azure Machine Learning Service verwendet wird
* Sie können entweder ein verwaltetes Compute (z.B. Machine Learning Compute) oder nicht verwaltetes Compute (z.B. VM) auswählen, um Ihre Trainingsaufträge auszuführen. Der Datenfluss wird für beide der folgenden Szenarien erläutert:
* (VM/HDInsight – Zugriff erfolgt unter Verwendung von SSH-Anmeldeinformationen in Key Vault im Microsoft-Abonnement) Azure Machine Learning Service führt Verwaltungscode auf dem Computeziel aus, der Folgendes bewirkt:

   1. Vorbereiten der Umgebung (Hinweis: Docker ist ebenfalls eine Option für „VM“ und „Lokal“. In den folgenden Schritten für Machine Learning Compute erfahren Sie, wie die Ausführung eines Experiments in einem Docker-Container funktioniert.)
   1. Herunterladen des Codes
   1. Einrichten der Umgebungsvariablen und Konfigurationen
   1. Ausführen des Benutzerskripts (oben erwähnte Codemomentaufnahme)

* (Machine Learning Compute – Zugriff erfolgt mit vom Arbeitsbereich verwalteter Identität) Beachten Sie: Machine Learning Compute ist ein verwaltetes Compute, d.h. es wird von Microsoft verwaltet – daraus resultiert, dass es im Rahmen eines Microsoft-Abonnements ausgeführt wird.

   1. Starten der Docker-Remotekonstruktion (sofern erforderlich)
   1. Schreiben von Verwaltungscode in die Azure-Dateifreigabe des Benutzers
   1. Starten des Containers mit einem ersten Befehl (Verwaltungscode, wie im vorherigen Schritt beschrieben)

#### <a name="querying-runs-and-metrics"></a>Abfragen von Ausführungen und Metriken

Dieser Schritt ist im Fluss dargestellt, wo das Trainingscompute die *Ausführungsmetriken* an Azure Machine Learning Service zurückschreibt, von wo aus sie in Cosmos DB gespeichert werden. Clients können Azure Machine Learning Service aufrufen, der wiederum die Metriken aus Cosmos DB pullt und an den Client zurückgibt.

![Screenshot zeigt den Workflow des Erstellens des Arbeitsbereichs](./media/enterprise-readiness/training-and-metrics.png)

### <a name="creating-web-services"></a>Erstellen von Webdiensten

Das folgende Diagramm zeigt den Rückschlussworkflow. Rückschlüsse oder Modellbewertungen bilden die Phase, in der das bereitgestellte Modell für die Vorhersage verwendet wird, meist für Produktionsdaten.
Beachten Sie folgende Details:

* Der Benutzer registriert ein Modell mit einem Client wie Azure ML SDK
* Der Benutzer erstellt das Image mit Modell, Bewertungsdatei und anderen Modellabhängigkeiten
* Das Docker-Image wird erstellt und in ACR gespeichert
* Der Webdienst wird auf dem Computeziel (ACI/AKS) mithilfe des oben erstellten Image bereitgestellt
* Details der Bewertung werden in AppInsights gespeichert, das im Abonnement des Benutzers enthalten ist
* Telemetriedaten werden auch an Microsoft/Azure-Abonnement übertragen

![Screenshot zeigt den Workflow des Erstellens des Arbeitsbereichs](./media/enterprise-readiness/inferencing.png)

## <a name="next-steps"></a>Nächste Schritte

* [Secure Azure Machine Learning web services with SSL (Sichere Azure Machine Learning-Webdienste mit SSL)](how-to-secure-web-service.md)
* [Consume a ML Model deployed as a web service (Nutzen eines als Webdienst bereitgestellten Azure Machine Learning-Modells)](how-to-consume-web-service.md).
* [How to run batch predictions (Ausführen von Batchvorhersagen)](how-to-run-batch-predictions.md)
* [Überwachen Ihrer Azure Machine Learning-Modelle mit Application Insights](how-to-enable-app-insights.md)
* [Sammeln von Daten für Modelle in der Produktion](how-to-enable-data-collection.md)
* [SDK für Azure Machine Learning Service](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)
* [Verwenden von Azure Machine Learning Service mit Azure Virtual Networks](how-to-enable-virtual-network.md)
* [Bewährte Methoden für das Erstellen von Empfehlungssystemen](https://github.com/Microsoft/Recommenders)
* [Erstellen einer Echtzeitempfehlungs-API in Azure](https://docs.microsoft.com/azure/architecture/reference-architectures/ai/real-time-recommendation)

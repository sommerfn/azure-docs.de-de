---
title: Unternehmenssicherheit
titleSuffix: Azure Machine Learning
description: 'Verwenden Sie Azure Machine Learning sicher: Authentifizierung, Autorisierung, Netzwerksicherheit, Datenverschlüsselung und Überwachung.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 11/04/2019
ms.openlocfilehash: e834c55ec35195ff627176603c7611abbf6adf1c
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73497510"
---
# <a name="enterprise-security-for-azure-machine-learning"></a>Unternehmenssicherheit für Azure Machine Learning

In diesem Artikel lernen Sie Sicherheitsfeatures kennen, die für Azure Machine Learning zur Verfügung stehen.

Wenn Sie einen Clouddienst verwenden, empfiehlt es sich, den Zugriff auf die Benutzer zu beschränken, die ihn benötigen. Als Erstes sollten Sie dabei das vom Dienst verwendete Authentifizierungs- und Autorisierungsmodell kennenlernen. Ggf. ist es auch erforderlich, den Zugriff auf das Netzwerk einzuschränken oder Ressourcen in Ihrem lokalen Netzwerk sicher mit der Cloud zu verknüpfen. Datenverschlüsselung ist auch von zentraler Bedeutung, sowohl im Ruhezustand als auch während des Verschiebens der Daten zwischen Diensten. Schließlich müssen Sie in der Lage sein, den Dienst zu überwachen und ein Überwachungsprotokoll aller Aktivitäten zu erstellen.

## <a name="authentication"></a>Authentication

Die mehrstufige Authentifizierung wird unterstützt, wenn Azure Active Directory (Azure AD) dafür konfiguriert ist. So verläuft der Authentifizierungsprozess:

1. Der Client meldet sich bei Azure AD an und ruft das Azure Resource Manager-Token ab.  Benutzer und Dienstprinzipale werden vollständig unterstützt.
1. Der Client übergibt das Token an Azure Resource Manager und an Azure Machine Learning.
1. Machine Learning Service stellt ein Machine Learning Service-Token für das Benutzercomputeziel bereit (z. B. Machine Learning Compute). Dieses Token wird nach Abschluss der Ausführung vom Benutzercomputeziel für einen erneuten Aufruf von Machine Learning Service verwendet. Der Bereich ist auf den Arbeitsbereich beschränkt.

[![Authentifizierung in Azure Machine Learning](./media/enterprise-readiness/authentication.png)](./media/enterprise-readiness/authentication-expanded.png)

### <a name="authentication-for-web-service-deployment"></a>Authentifizierung für die Webdienstbereitstellung

Azure Machine Learning unterstützt zwei Authentifizierungsarten für Webdienste: Schlüssel und Token. Die einzelnen Webdienste können jeweils nur eine Authentifizierungsart verwenden.

|Authentifizierungsmethode|Azure Container Instances|AKS|
|---|---|---|
|Schlüssel|Standardmäßig deaktiviert| Standardmäßig aktiviert|
|Tokenverschlüsselung| Nicht verfügbar| Standardmäßig deaktiviert |

#### <a name="authentication-with-keys"></a>Authentifizierung mit Schlüsseln

Wenn Sie die Schlüsselauthentifizierung für eine Bereitstellung aktivieren, werden automatisch Authentifizierungsschlüssel erstellt.

* Die Authentifizierung ist standardmäßig aktiviert, wenn die Bereitstellung in Azure Kubernetes Service (AKS) erfolgt.
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

Wenn Sie die Tokenauthentifizierung für einen Webdienst aktivieren, müssen die Benutzer ein Azure Machine Learning-JSON-Webtoken für den Webdienst bereitstellen, um auf den Dienst zugreifen zu können.

* Die Tokenauthentifizierung ist standardmäßig deaktiviert, wenn die Bereitstellung in Azure Kubernetes Service erfolgt.
* Die Tokenauthentifizierung wird nicht unterstützt, wenn die Bereitstellung in Azure Container Instances erfolgt.

Wenn Sie die Tokenauthentifizierung steuern möchten, verwenden Sie beim Erstellen oder Aktualisieren einer Bereitstellung den Parameter `token_auth_enabled`.

Bei aktivierter Tokenauthentifizierung können Sie mithilfe der Methode `get_token` ein JSON Web Token (JWT) und dessen Ablaufzeit abrufen:

```python
token, refresh_by = service.get_token()
print(token)
```

> [!IMPORTANT]
> Nach Ablauf der für `refresh_by` festgelegten Zeit müssen Sie ein neues Token anfordern.
>
> Wir empfehlen Ihnen dringend, den Azure Machine Learning-Arbeitsbereich in der gleichen Region zu erstellen wie den Azure Kubernetes Service-Cluster. 
>
> Im Zuge der Tokenauthentifizierung richtet der Webdienst einen Aufruf an die Region, in der Ihr Azure Machine Learning-Arbeitsbereich erstellt wird. Wenn die Region Ihres Arbeitsbereichs nicht verfügbar ist, können Sie kein Token für Ihren Webdienst abrufen, auch dann nicht, wenn sich Ihr Cluster in einer anderen Region befindet als Ihr Arbeitsbereich. Die Azure AD-Authentifizierung ist dann erst wieder verfügbar, wenn die Region Ihres Arbeitsbereichs wieder verfügbar wird. 
>
> Außerdem wirkt sich die Entfernung zwischen der Region Ihres Clusters und der Region Ihres Arbeitsbereichs direkt auf die Tokenabrufdauer aus.

## <a name="authorization"></a>Authorization

Sie können mehrere Arbeitsbereiche erstellen, und jeder Arbeitsbereich kann von mehreren Benutzern gemeinsam genutzt werden. Den Zugriff auf einen freigegebenen Arbeitsbereich können Sie steuern, indem Sie Benutzern die folgenden Rollen zuweisen:

* Owner (Besitzer)
* Mitwirkender
* Leser

Die folgende Tabelle enthält einige der wichtigsten Azure Machine Learning-Vorgänge und die Rollen, von denen sie ausgeführt werden können:

| Azure Machine Learning-Vorgang | Owner (Besitzer) | Mitwirkender | Leser |
| ---- |:----:|:----:|:----:|
| Arbeitsbereich erstellen | ✓ | ✓ | |
| Freigeben des Arbeitsbereichs | ✓ | |  |
| Upgraden des Arbeitsbereichs auf Enterprise Edition | ✓ | |
| Erstellen eines Computeziels | ✓ | ✓ | |
| Anfügen eines Computeziels | ✓ | ✓ | |
| Anfügen von Datenspeichern | ✓ | ✓ | |
| Ausführen des Experiments | ✓ | ✓ | |
| Ausführungen/Metriken anzeigen | ✓ | ✓ | ✓ |
| Registrieren des Modells | ✓ | ✓ | |
| Image erstellen | ✓ | ✓ | |
| Bereitstellen des Webdiensts | ✓ | ✓ | |
| Modelle/Images anzeigen | ✓ | ✓ | ✓ |
| Webdienst aufrufen | ✓ | ✓ | ✓ |

Wenn die integrierten Rollen Ihren Anforderungen nicht entsprechen, können Sie benutzerdefinierte Rollen erstellen. Benutzerdefinierte Rollen werden nur für Vorgänge im Arbeitsbereich und Machine Learning Compute unterstützt. Benutzerdefinierte Rollen können Lese-, Schreib- oder Löschberechtigungen für den Arbeitsbereich und die Computeressource in diesem Arbeitsbereich haben. Sie können die Rolle auf einer bestimmten Arbeitsbereichsebene, einer bestimmten Ressourcengruppenebene oder einer bestimmten Abonnementebene verfügbar machen. Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf einen Azure Machine Learning-Arbeitsbereich](how-to-assign-roles.md).

### <a name="securing-compute-targets-and-data"></a>Sichern von Computezielen und Daten

Besitzer und Mitwirkende können alle Computeziele und Datenspeicher verwenden, die dem Arbeitsbereich angefügt sind.  

Jedem Arbeitsbereich ist außerdem eine vom System zugewiesene verwaltete Identität zugeordnet, die den gleichen Namen wie der Arbeitsbereich trägt. Die verwaltete Identität verfügt über die folgenden Berechtigungen für angefügte Ressourcen, die im Arbeitsbereich verwendet werden.

Weitere Informationen zu verwalteten Identitäten finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

| Resource | Berechtigungen |
| ----- | ----- |
| Arbeitsbereich | Mitwirkender |
| Speicherkonto | Mitwirkender an Storage-Blobdaten |
| Schlüsseltresor | Zugriff auf alle Schlüssel, Geheimnisse und Zertifikate |
| Azure Container Registry | Mitwirkender |
| Ressourcengruppe, die den Arbeitsbereich enthält | Mitwirkender |
| Ressourcengruppe, die den Schlüsseltresor enthält (wenn sie nicht mit der Ressourcengruppe identisch ist, die den Arbeitsbereich enthält) | Mitwirkender |

Administratoren sollten den Zugriff der verwalteten Identität auf die in der vorhergehenden Tabelle genannten Ressourcen nicht widerrufen. Sie können den Zugriff mithilfe des Vorgangs zum erneuten Synchronisieren von Schlüsseln wiederherstellen.

Azure Machine Learning erstellt eine weitere Anwendung (deren Name mit `aml-` oder `Microsoft-AzureML-Support-App-` beginnt) mit Zugriff auf Mitwirkendenebene in Ihrem Abonnement für jede Region des Arbeitsbereichs. Wenn Sie beispielsweise im selben Abonnement über einen Arbeitsbereich in „USA, Osten“ und einen anderen Arbeitsbereich in „Europa, Norden“ verfügen, sehen Sie zwei dieser Anwendungen. Azure Machine Learning kann Sie mithilfe dieser Anwendungen beim Verwalten der Computeressourcen unterstützen.

## <a name="network-security"></a>Netzwerksicherheit

Azure Machine Learning ist hinsichtlich Computeressourcen auf andere Azure-Dienste angewiesen. Computeressourcen (Computeziele) dienen zum Trainieren und Bereitstellen von Modellen. Sie können diese Computeziele in einem virtuellen Netzwerk erstellen. So können Sie beispielsweise Azure Data Science Virtual Machine verwenden, um ein Modell zu trainieren, und das Modell anschließend in AKS bereitstellen.  

Weitere Informationen finden Sie unter [Sicheres Ausführen von Experimenten und Ziehen von Rückschlüssen innerhalb eines virtuellen Azure-Netzwerks](how-to-enable-virtual-network.md).

## <a name="data-encryption"></a>Datenverschlüsselung

### <a name="encryption-at-rest"></a>Verschlüsselung ruhender Daten

#### <a name="azure-blob-storage"></a>Azure Blob Storage

Azure Machine Learning speichert Momentaufnahmen, Ausgaben und Protokolle in dem Azure Blob Storage-Konto, das mit dem Azure Machine Learning-Arbeitsbereich und Ihrem Abonnement verknüpft ist. Alle in Azure Blob Storage gespeicherten Daten werden im Ruhezustand mit von Microsoft verwalteten Schlüsseln verschlüsselt.

Informationen dazu, wie Sie Ihre eigenen Schlüssel für die in Azure Blob Storage gespeicherten Daten verwenden können, finden Sie unter [Konfigurieren von Kunden verwalteter Schlüssel für die Azure Storage-Verschlüsselung mithilfe des Azure-Portals](https://docs.microsoft.com/azure/storage/common/storage-service-encryption-customer-managed-keys).

Trainingsdaten werden in der Regel auch in Azure Blob Storage gespeichert, damit sie zum Trainieren von Computezielen zur Verfügung stehen. Dieser Speicher wird nicht von Azure Machine Learning verwaltet, sondern er wird Computezielen als Remotedateisystem bereitgestellt.

Informationen zum erneuten Generieren der Zugriffsschlüssel für die in Ihrem Arbeitsbereich verwendeten Azure-Speicherkonten finden Sie im Artikel [Zugriffsschlüssel für Speicherkonten neu generieren](how-to-change-storage-access-key.md).

#### <a name="azure-cosmos-db"></a>Azure Cosmos DB

Azure Machine Learning speichert Metriken und Metadaten in der Azure Cosmos DB-Instanz, die einem von Azure Machine Learning verwalteten Microsoft-Abonnement zugeordnet ist. Alle in Azure Cosmos DB gespeicherten Daten werden im Ruhezustand mit von Microsoft verwalteten Schlüsseln verschlüsselt.

#### <a name="azure-container-registry"></a>Azure Container Registry

Alle Containerimages in Ihrer Registrierung (Azure Container Registry) werden im Ruhezustand verschlüsselt. Azure verschlüsselt ein Image automatisch vor dessen Speicherung, und entschlüsselt es dynamisch, wenn Azure Machine Learning das Image herunterlädt (pullt).

#### <a name="machine-learning-compute"></a>Machine Learning Compute

Der Betriebssystem-Datenträger für jeden in Azure Storage gespeicherten Computeknoten wird mit von Microsoft verwalteten Schlüsseln in Speicherkonten von Azure Machine Learning verschlüsselt. Dieses Computeziel ist kurzlebig, und Cluster werden in der Regel zentral herunterskaliert, wenn keine Ausführungen in der Warteschlange stehen. Die Bereitstellung des zugrunde liegenden virtuellen Computers wird aufgehoben, und der Betriebssystem-Datenträger wird gelöscht. Azure Disk Encryption wird für den Betriebssystem-Datenträger nicht unterstützt.

Jeder virtuelle Computer verfügt auch über einen lokalen temporären Datenträger für Betriebssystem-Vorgänge. Wenn Sie möchten, können Sie den Datenträger zum Bereitstellen von Trainingsdaten verwenden. Der Datenträger ist nicht verschlüsselt.
Weitere Informationen zur Verschlüsselung ruhender Daten in Azure finden Sie unter [Azure-Datenverschlüsselung ruhender Daten](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest).

### <a name="encryption-in-transit"></a>Verschlüsselung während der Übertragung

Sie können SSL zum Sichern der internen Kommunikation zwischen Azure Machine Learning-Microservices sowie zum Sichern externer Aufrufe des Bewertungsendpunkts verwenden. Der gesamte Azure Storage-Zugriff erfolgt ebenfalls über einen sicheren Kanal.

Weitere Informationen finden Sie unter [Verwenden von SSL zum Schützen eines Webdiensts mit Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/how-to-secure-web-service).

### <a name="using-azure-key-vault"></a>Verwenden von Azure Key Vault

Azure Machine Learning verwendet die mit dem Arbeitsbereich verknüpfte Azure Key Vault-Instanz zum Speichern von Anmeldeinformationen verschiedener Art:

* Der verknüpften Speicherkonto-Verbindungszeichenfolge
* Kennwörter in Azure Container Repository-Instanzen
* Verbindungszeichenfolgen zur Verbindung mit Datenspeichern.

SSH-Kennwörter und Schlüssel für Computeziele wie Azure HDInsight und virtuelle Computer werden in einem separaten Schlüsseltresor gespeichert, der dem Microsoft-Abonnement zugeordnet ist. Azure Machine Learning speichert weder Kennwörter noch Schlüssel, die von Benutzern bereitgestellt wurden. Stattdessen werden eigene SSH-Schlüssel generiert, autorisiert und gespeichert, um eine Verbindung mit virtuellen Computern und HDInsight herzustellen und die Experimente auszuführen.

Jedem Arbeitsbereich ist eine vom System zugewiesene verwaltete Identität zugeordnet, die den gleichen Namen hat wie der Arbeitsbereich. Diese verwaltete Identität verfügt über Zugriff auf alle Schlüssel, Geheimnisse und Zertifikate im Schlüsseltresor.

## <a name="monitoring"></a>Überwachung

### <a name="metrics"></a>metrics

Sie können Azure Monitor-Metriken verwenden, um Metriken für Ihren Azure Machine Learning-Arbeitsbereich anzuzeigen und zu überwachen. Wählen Sie im [Azure-Portal](https://portal.azure.com) Ihren Arbeitsbereich und anschließend **Metriken** aus:

[![Screenshot, der Beispielmetriken für einen Arbeitsbereich zeigt](./media/enterprise-readiness/workspace-metrics.png)](./media/enterprise-readiness/workspace-metrics-expanded.png)

Die Metriken umfassen Informationen zu Ausführungen, Bereitstellungen und Registrierungen.

Weitere Informationen finden Sie unter [Azure Monitor-Metriken](/azure/azure-monitor/platform/data-platform-metrics).

### <a name="activity-log"></a>Aktivitätsprotokoll

Im Aktivitätsprotokoll eines Arbeitsbereichs können Sie verschiedene Vorgänge anzeigen, die im Arbeitsbereich ausgeführt werden. Das Protokoll enthält grundlegende Informationen wie den Vorgangsnamen, den Ereignisinitiator und den Zeitstempel.

Der folgende Screenshot zeigt das Aktivitätsprotokoll eines Arbeitsbereichs:

[![Screenshot, der das Aktivitätsprotokoll eines Arbeitsbereichs zeigt](./media/enterprise-readiness/workspace-activity-log.png)](./media/enterprise-readiness/workspace-activity-log-expanded.png)

Details der Bewertungsanforderung werden in Application Insights gespeichert. Application Insights wird in Ihrem Abonnement erstellt, wenn Sie einen Arbeitsbereich erstellen. Protokollierte Informationen beinhalten Felder wie HTTPMethod, UserAgent, ComputeType, RequestUrl, StatusCode, RequestId und Duration.

> [!IMPORTANT]
> Einige Aktionen im Azure Machine Learning-Arbeitsbereich protokollieren keine Informationen im Aktivitätsprotokoll. Der Start einer Trainingsausführung und die Registrierung eines Modells werden beispielsweise nicht protokolliert.
>
> Einige dieser Aktionen werden im Bereich **Aktivitäten** Ihres Arbeitsbereichs angezeigt, diese Benachrichtigungen geben jedoch nicht an, wer die Aktivität initiiert hat.

## <a name="data-flow-diagrams"></a>Datenflussdiagramme

### <a name="create-workspace"></a>Arbeitsbereich erstellen

Das folgende Diagramm zeigt den Workflow des Erstellens des Arbeitsbereichs.

* Der Benutzer meldet sich von einem der unterstützten Azure Machine Learning-Clients (Azure CLI, Python, SDK, Azure-Portal) aus bei Azure AD an und fordert das entsprechende Azure Resource Manager-Token an.
* Der Benutzer ruft den Azure Resource Manager auf, um den Arbeitsbereich zu erstellen. 
* Der Azure Resource Manager fordert den Azure Machine Learning-Ressourcenanbieter auf, den Arbeitsbereich bereitzustellen.

Zusätzliche Ressourcen werden bei der Erstellung des Arbeitsbereichs im Abonnement des Benutzers erstellt:

* Key Vault (zum Speichern von Geheimnissen)
* Ein Azure Storage-Konto (einschließlich Blob und Dateifreigabe)
* Azure Container Registry (zum Speichern von Docker-Images für Rückschlüsse/Bewertungen und Experimente)
* Application Insights (zum Speichern von Telemetriedaten)

Der Benutzer kann bei Bedarf auch andere Computeziele bereitstellen, die an einen Arbeitsbereich angefügt sind (wie Azure Kubernetes Service oder VMs).

[![Erstellen eines Arbeitsbereichs-Workflows](./media/enterprise-readiness/create-workspace.png)](./media/enterprise-readiness/create-workspace-expanded.png)

### <a name="save-source-code-training-scripts"></a>Speichern des Quellcodes (Trainingsskripts)

Das folgende Diagramm zeigt den Workflow der Codemomentaufnahme.

Einem Azure Machine Learning-Arbeitsbereich sind Verzeichnisse (Experimente) zugeordnet, die den Quellcode (Trainingsskripts) enthalten. Diese Skripts werden auf Ihrem lokalen Computer und in der Cloud (in Azure Blob Storage für Ihr Abonnement) gespeichert. Die Codemomentaufnahmen werden für die Ausführung oder Überprüfung für die Verlaufsüberwachung verwendet.

[![Workflow der Codemomentaufnahme](./media/enterprise-readiness/code-snapshot.png)](./media/enterprise-readiness/code-snapshot-expanded.png)

### <a name="training"></a>Training

Das folgende Diagramm zeigt den Trainingsworkflow.

* Azure Machine Learning wird mit der Momentaufnahmen-ID für die im vorherigen Abschnitt gespeicherte Codemomentaufnahme aufgerufen.
* Azure Machine Learning erstellt eine Ausführungs-ID (optional) und ein Azure Machine Learning-Token, das später von Computezielen wie Machine Learning Compute/VMs für die Kommunikation mit Machine Learning verwendet wird.
* Sie können entweder ein verwaltetes Computeziel (wie Machine Learning Compute) oder ein nicht verwaltetes Computeziel (wie VMs) auswählen, um Ihre Trainingsaufträge auszuführen. Im Folgenden finden Sie die Datenflüsse für beide Szenarien:
   * VMs/HDInsight, Zugriff erfolgt über SSH-Anmeldeinformationen in einem Schlüsseltresor im Microsoft-Abonnement. Azure Machine Learning führt verwalteten Code auf dem Computeziel aus, der die folgenden Aktionen ausführt:

   1. Vorbereiten der Umgebung (Docker ist eine Option für VMs und lokale Computer. In den folgenden Schritten für Machine Learning Compute erfahren Sie, wie die Ausführung eines Experiments in Docker-Containern funktioniert.)
   1. Herunterladen des Codes
   1. Einrichten von Umgebungsvariablen und Konfigurationen
   1. Ausführen von Benutzerskripts (die im vorherigen Abschnitt genannte Codemomentaufnahme)

   * Machine Learning Compute, Zugriff erfolgt über eine vom Arbeitsbereich verwaltete Identität.
Da Machine Learning Compute ein verwaltetes Computeziel ist (d. h. es wird von Microsoft verwaltet), wird es unter Ihrem Microsoft-Abonnement ausgeführt.

   1. Starten der Docker-Remotekonstruktion (sofern erforderlich)
   1. Schreiben des Verwaltungscodes in die Azure Files-Freigabe des Benutzers
   1. Starten des Containers mit einem ersten Befehl. Dies ist der Verwaltungscode, der im vorherigen Schritt beschrieben wurde.

#### <a name="querying-runs-and-metrics"></a>Abfragen von Ausführungen und Metriken

Im folgenden Flussdiagramm wird dieser Schritt ausgeführt, wenn das Trainingscomputeziel die Ausführungsmetriken aus dem Speicher in der Cosmos DB-Datenbank an Azure Machine Learning zurückschreibt. Clients können Azure Machine Learning aufrufen. Machine Learning pullt wiederum die Metriken aus der Cosmos DB-Datenbank und gibt sie an den Client zurück.

[![Trainingsworkflow](./media/enterprise-readiness/training-and-metrics.png)](./media/enterprise-readiness/training-and-metrics-expanded.png)

### <a name="creating-web-services"></a>Erstellen von Webdiensten

Das folgende Diagramm zeigt den Rückschlussworkflow. Rückschlüsse oder Modellbewertungen stellen die Phase dar, in der das bereitgestellte Modell für die Vorhersage verwendet wird (meist für Produktionsdaten).

Es folgen die Details:

* Der Benutzer registriert ein Modell, indem er einen Client wie das Azure Machine Learning-SDK verwendet.
* Der Benutzer erstellt das Image mit einem Modell, einer Bewertungsdatei und anderen Modellabhängigkeiten.
* Das Docker-Image wird erstellt und in Azure Container Registry gespeichert.
* Der Webdienst wird auf dem Computeziel (Container Instances/AKS) mithilfe des Images bereitgestellt, das im vorherigen Schritt erstellt wurde.
* Details der Bewertung werden in Application Insights gespeichert (im Abonnement des Benutzers enthalten).
* Telemetriedaten werden ebenfalls per Push an das Microsoft/Azure-Abonnement übertragen.

[![Rückschlussworkflow](./media/enterprise-readiness/inferencing.png)](./media/enterprise-readiness/inferencing-expanded.png)

## <a name="next-steps"></a>Nächste Schritte

* [Secure Azure Machine Learning web services with SSL (Sichere Azure Machine Learning-Webdienste mit SSL)](how-to-secure-web-service.md)
* [Nutzen eines als Webdienst bereitgestellten Machine Learning-Modells](how-to-consume-web-service.md)
* [How to run batch predictions (Ausführen von Batchvorhersagen)](how-to-run-batch-predictions.md)
* [Überwachen Ihrer Azure Machine Learning-Modelle mit Application Insights](how-to-enable-app-insights.md)
* [Sammeln von Daten für Modelle in der Produktion](how-to-enable-data-collection.md)
* [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)
* [Verwenden von Azure Machine Learning mit einem virtuellen Azure-Netzwerk](how-to-enable-virtual-network.md)
* [Bewährte Methoden für das Erstellen von Empfehlungssystemen](https://github.com/Microsoft/Recommenders)
* [Erstellen einer Echtzeitempfehlungs-API in Azure](https://docs.microsoft.com/azure/architecture/reference-architectures/ai/real-time-recommendation)

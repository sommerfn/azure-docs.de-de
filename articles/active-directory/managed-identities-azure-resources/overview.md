---
title: Verwaltete Identitäten für Azure-Ressourcen
description: Eine Übersicht über verwaltete Identitäten für Azure-Ressourcen.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 0232041d-b8f5-4bd2-8d11-27999ad69370
ms.service: active-directory
ms.subservice: msi
ms.devlang: ''
ms.topic: overview
ms.custom: mvc
ms.date: 09/26/2019
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: f5a9f83a8f6e8d5e83f30d30da2b1c4c0f722dc3
ms.sourcegitcommit: f7f70c9bd6c2253860e346245d6e2d8a85e8a91b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73063054"
---
# <a name="what-is-managed-identities-for-azure-resources"></a>Was sind verwaltete Identitäten für Azure-Ressourcen?

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Eine gängige Herausforderung beim Erstellen von Cloudanwendungen ist die Verwaltung der Anmeldeinformationen im Code, die für die Authentifizierung bei Clouddiensten erforderlich sind. Der Schutz dieser Anmeldeinformationen ist eine wichtige Aufgabe. Im Idealfall werden die Anmeldeinformationen nie auf Entwicklerarbeitsstationen angezeigt und auch nicht in die Quellcodeverwaltung eingecheckt. Azure Key Vault bietet eine Möglichkeit zum sicheren Speichern von Anmeldeinformationen, Geheimnissen und anderen Schlüsseln. Um diese abrufen zu können, muss sich Ihr Code jedoch bei Key Vault authentifizieren. 

Die Funktion für verwaltete Identitäten für Azure-Ressourcen in Azure Active Directory (Azure AD) löst dieses Problem. Dieses Feature stellt für Azure-Dienste eine automatisch verwaltete Identität in Azure AD bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt, einschließlich Key Vault. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein.

Die Funktion für verwaltete Identitäten für Azure-Ressourcen ist bei Azure AD mit einem Azure-Abonnement kostenlos. Es fallen keine zusätzlichen Kosten an.

> [!NOTE]
> „Verwaltete Identitäten für Azure-Ressourcen“ ist der neue Name für den Dienst, der früher als „Verwaltete Dienstidentität“ (Managed Service Identity, MSI) bezeichnet wurde.

## <a name="terminology"></a>Begriff

Die folgenden Begriffe werden in den verwalteten Identitäten für den Azure-Ressourcendokumentationssatz verwendet:

- **Client-ID**: Ein eindeutiger, vom Azure AD generierter Bezeichner, der während der ersten Bereitstellung an eine Anwendung und einen Dienstprinzipal gebunden ist
- **Prinzipal-ID**: Die Objekt-ID des Dienstprinzipalobjekts für Ihre verwaltete Identität, die zum Gewähren des rollenbasierten Zugriffs auf eine Azure-Ressource verwendet wird
- **Azure Instance Metadata Service (IMDS)** ist ein REST-Endpunkt, der für alle virtuellen IaaS-Computer verfügbar ist, die mit Azure Resource Manager erstellt wurden. Der Endpunkt steht unter einer bekannten, nicht routingfähigen IP-Adresse zur Verfügung (169.254.169.254), auf die nur von innerhalb der VM zugegriffen werden kann.

## Wie funktionieren die verwalteten Identitäten für Azure-Ressourcen?<a name="how-does-it-work"></a>

Es gibt zwei Arten von verwalteten Identitäten:

- Eine **vom System zugewiesene verwaltete Identität** wird direkt für eine Azure-Dienstinstanz aktiviert. Wenn die Identität aktiviert ist, erstellt Azure eine Identität für die Instanz in dem Azure AD-Mandanten, der vom Abonnement der Instanz als vertrauenswürdig eingestuft wird. Nach dem Erstellen der Identität werden die Anmeldeinformationen in der Instanz bereitgestellt. Der Lebenszyklus einer vom System zugewiesenen Identität ist direkt an die Azure-Dienstinstanz gebunden, für die sie aktiviert wurde. Wenn die Instanz gelöscht wird, bereinigt Azure automatisch die Anmeldeinformationen und die Identität in Azure AD.
- Eine **vom Benutzer zugewiesene verwaltete Identität** wird als eigenständige Azure-Ressource erstellt. Azure erstellt eine Identität in dem Azure AD-Mandanten, der vom verwendeten Abonnement als vertrauenswürdig eingestuft wird. Nachdem die Identität erstellt wurde, kann sie einer oder mehreren Azure-Dienstinstanzen zugewiesen werden. Der Lebenszyklus einer vom Benutzer zugewiesenen Identität wird getrennt vom Lebenszyklus der Azure-Dienstinstanzen verwaltet, denen sie zugewiesen ist.

Intern sind verwaltete Identitäten eine Sonderform von Dienstprinzipalen, die nur mit Azure-Ressourcen verwendet werden können. Wenn die verwaltete Identität gelöscht wird, wird automatisch auch der entsprechende Dienstprinzipal entfernt. 

Ihr Code kann eine verwaltete Identität zum Anfordern von Zugriffstokens für Dienste verwenden, die die Azure AD-Authentifizierung unterstützen. Azure übernimmt die Weitergabe der von der Dienstinstanz verwendeten Anmeldeinformationen. 

Das folgende Diagramm zeigt, wie verwaltete Dienstidentitäten mit virtuellen Azure-Computern funktionieren:

![Verwaltete Dienstidentitäten und Azure-VMs](media/overview/msi-vm-vmextension-imds-example.png)

|  Eigenschaft    | Systemseitig zugewiesene verwaltete Identität | Benutzerseitig zugewiesene verwaltete Identität |
|------|----------------------------------|--------------------------------|
| Erstellung |  Als Teil einer Azure-Ressource (etwa eines virtuellen Azure-Computers oder einer Azure App Service-Instanz) | Als eigenständige Azure-Ressource |
| Lebenszyklus | Gemeinsamer Lebenszyklus mit der Azure-Ressource, für die die verwaltete Identität erstellt wurde. <br/> Wenn die übergeordnete Ressource gelöscht wird, wird auch die verwaltete Identität gelöscht. | Unabhängiger Lebenszyklus. <br/> Muss explizit gelöscht werden. |
| Gemeinsame Nutzung durch mehrere Azure-Ressourcen | Keine gemeinsame Nutzung möglich. <br/> Kann nur einer einzelnen Azure-Ressource zugeordnet werden. | Gemeinsame Nutzung möglich. <br/> Die gleiche benutzerseitig zugewiesene verwaltete Identität kann mehreren Azure-Ressourcen zugeordnet werden. |
| Gängige Anwendungsfälle | Workloads innerhalb einer einzelnen Azure-Ressource. <br/> Workloads, für die unabhängige Identitäten erforderlich sind. <br/> Beispielsweise eine Anwendung, die auf einem einzelnen virtuellen Computer ausgeführt wird. | Workloads, die auf mehrere Ressourcen ausgeführt werden und sich eine einzelne Identität teilen können. <br/> Workloads, die im Rahmen eines Bereitstellungsablaufs vorab für eine sichere Ressource autorisiert werden müssen. <br/> Workloads, bei denen Ressourcen häufig recycelt werden, die Berechtigungen aber konsistent bleiben sollen. <br/> Beispielsweise eine Workload, bei der mehrere virtuelle Computer auf die gleiche Ressource zugreifen müssen. | 

### <a name="how-a-system-assigned-managed-identity-works-with-an-azure-vm"></a>Funktionsweise einer vom System zugewiesenen verwalteten Identität mit einer Azure-VM

1. Azure Resource Manager empfängt eine Anforderung zur Aktivierung der vom System zugewiesenen verwalteten Identität auf einer VM.

2. Azure Resource Manager erstellt einen Dienstprinzipal in Azure AD für die Identität des virtuellen Computers. Der Dienstprinzipal wird in dem Azure AD-Mandanten erstellt, der von diesem Abonnement als vertrauenswürdig eingestuft wird.

3. Azure Resource Manager aktualisiert den Identitätsendpunkt von Azure Instance Metadata Service mit der Client-ID und dem Zertifikat des Dienstprinzipals, um die Identität auf der VM zu konfigurieren.

4. Wenn der virtuelle Computer über eine Identität verfügt, verwenden wir die Dienstprinzipalinformationen, um ihm Zugriff auf Azure-Ressourcen zu gewähren. Verwenden Sie zum Aufrufen von Azure Resource Manager die rollenbasierte Zugriffssteuerung (RBAC) in Azure AD, um dem VM-Dienstprinzipal die entsprechende Rolle zuzuweisen. Gewähren Sie Ihrem Code zum Aufrufen von Key Vault Zugriff auf das spezifische Geheimnis oder den spezifischen Schlüssel in Key Vault.

5. Der auf dem virtuellen Computer ausgeführte Code kann ein Token vom Azure IMDS-Endpunkt (Instance Metadata Service) anfordern, auf das nur von dem virtuellen Computer aus zugegriffen werden kann: `http://169.254.169.254/metadata/identity/oauth2/token`
    - Der Ressourcenparameter gibt den Dienst an, an den das Token gesendet wird. Verwenden Sie `resource=https://management.azure.com/` für die Authentifizierung bei Azure Resource Manager.
    - Der API-Versionsparameter gibt die IMDS-Version an. Verwenden Sie mindestens „api-version=2018-02-01“.

6. Mit einem an Azure AD gerichteten Aufruf wird ein Zugriffstoken angefordert (siehe Schritt 5). Dabei werden die Client-ID und das Zertifikat verwendet, die in Schritt 3 konfiguriert wurden. Azure AD gibt ein JWT-Zugriffstoken (JSON Web Token) zurück.

7. Der Code sendet das Zugriffstoken in einem Aufruf eines Diensts, der die Azure AD-Authentifizierung unterstützt.

### <a name="how-a-user-assigned-managed-identity-works-with-an-azure-vm"></a>Funktionsweise einer vom Benutzer zugewiesenen verwalteten Identität mit einer Azure-VM

1. Azure Resource Manager empfängt eine Anforderung zur Erstellung einer vom Benutzer zugewiesenen verwalteten Identität.

2. Azure Resource Manager erstellt einen Dienstprinzipal in Azure AD für die vom Benutzer zugewiesene verwaltete Identität. Der Dienstprinzipal wird in dem Azure AD-Mandanten erstellt, der von diesem Abonnement als vertrauenswürdig eingestuft wird.

3. Azure Resource Manager empfängt eine Anforderung zum Konfigurieren der vom Benutzer zugewiesenen verwalteten Identität auf einer VM und aktualisiert den Identitätsendpunkt von Azure Instance Metadata Service mit der Client-ID und dem Zertifikat des Dienstprinzipals der vom Benutzer zugewiesenen verwalteten Identität.

4. Nachdem die vom Benutzer zugewiesene verwaltete Identität erstellt wurde, werden die Dienstprinzipalinformationen verwendet, um der Identität Zugriff auf Azure-Ressourcen zu gewähren. Verwenden Sie zum Aufrufen von Azure Resource Manager die RBAC in Azure AD, um dem Dienstprinzipal der vom Benutzer zugewiesenen Identität die entsprechende Rolle zuzuweisen. Gewähren Sie Ihrem Code zum Aufrufen von Key Vault Zugriff auf das spezifische Geheimnis oder den spezifischen Schlüssel in Key Vault.

   > [!Note]
   > Sie können diesen Schritt auch vor Schritt 3 ausführen.

5. Der auf dem virtuellen Computer ausgeführte Code kann ein Token vom Azure IMDS-Identitätsendpunkt (Instance Metadata Service) anfordern, auf das nur von dem virtuellen Computer aus zugegriffen werden kann: `http://169.254.169.254/metadata/identity/oauth2/token`
    - Der Ressourcenparameter gibt den Dienst an, an den das Token gesendet wird. Verwenden Sie `resource=https://management.azure.com/` für die Authentifizierung bei Azure Resource Manager.
    - Der Client-ID-Parameter gibt die Identität an, für die das Token angefordert wird. Dieser Wert ist erforderlich, um Mehrdeutigkeiten zu vermeiden, wenn auf einer einzelnen VM mehrere vom Benutzer zugewiesene Identitäten vorhanden sind.
    - Der API-Versionsparameter gibt die Azure Instance Metadata Service-Version an. Verwenden Sie `api-version=2018-02-01` oder höher.

6. Mit einem an Azure AD gerichteten Aufruf wird ein Zugriffstoken angefordert (siehe Schritt 5). Dabei werden die Client-ID und das Zertifikat verwendet, die in Schritt 3 konfiguriert wurden. Azure AD gibt ein JWT-Zugriffstoken (JSON Web Token) zurück.
7. Der Code sendet das Zugriffstoken in einem Aufruf eines Diensts, der die Azure AD-Authentifizierung unterstützt.

## <a name="how-can-i-use-managed-identities-for-azure-resources"></a>Wie kann ich verwaltete Identitäten für Azure-Ressourcen verwenden?

In den folgenden Tutorials erfahren Sie mehr darüber, wie Sie verwaltete Identitäten für den Zugriff auf verschiedene Azure-Ressourcen verwenden können.

> [!NOTE]
> Im Kurs [Implementing Managed Identities for Microsoft Azure Resources](https://www.pluralsight.com/courses/microsoft-azure-resources-managed-identities-implementing) (Implementierung von verwalteten Identitäten für Microsoft Azure-Ressourcen) finden Sie weitere Informationen zu verwalteten Identitäten, einschließlich exemplarischer Vorgehensweise per Video zu mehreren unterstützten Szenarien.

Erfahren Sie, wie Sie eine verwaltete Identität mit einer Windows-VM verwenden:

* [Zugreifen auf Azure Data Lake Store](tutorial-windows-vm-access-datalake.md)
* [Zugreifen auf Azure Resource Manager](tutorial-windows-vm-access-arm.md)
* [Zugreifen auf Azure SQL](tutorial-windows-vm-access-sql.md)
* [Zugreifen auf Azure Storage mithilfe eines Zugriffsschlüssels](tutorial-windows-vm-access-storage.md)
* [Zugreifen auf Azure Storage mithilfe von SAS-Anmeldeinformationen](tutorial-windows-vm-access-storage-sas.md)
* [Zugreifen auf eine Nicht-Azure AD-Ressource mithilfe von Azure Key Vault](tutorial-windows-vm-access-nonaad.md)

Erfahren Sie, wie Sie eine verwaltete Identität mit einer Linux-VM verwenden:

* [Zugreifen auf Azure Container Registry](../../container-registry/container-registry-authentication-managed-identity.md)
* [Zugreifen auf Azure Data Lake Store](tutorial-linux-vm-access-datalake.md)
* [Zugreifen auf Azure Resource Manager](tutorial-linux-vm-access-arm.md)
* [Zugreifen auf Azure Storage mithilfe eines Zugriffsschlüssels](tutorial-linux-vm-access-storage.md)
* [Zugreifen auf Azure Storage mithilfe von SAS-Anmeldeinformationen](tutorial-linux-vm-access-storage-sas.md)
* [Zugreifen auf eine Nicht-Azure AD-Ressource mithilfe von Azure Key Vault](tutorial-linux-vm-access-nonaad.md)

Erfahren Sie, wie Sie eine verwaltete Identität mit anderen Azure-Diensten verwenden:

* [Azure App Service](/azure/app-service/overview-managed-identity)
* [Azure API Management](../../api-management/api-management-howto-use-managed-service-identity.md)
* [Azure Container Instances](../../container-instances/container-instances-managed-identity.md)
* [Azure Container Registry Tasks](../../container-registry/container-registry-tasks-authentication-managed-identity.md)
* [Azure Event Hubs](../../event-hubs/event-hubs-managed-service-identity.md)
* [Azure-Funktionen](/azure/app-service/overview-managed-identity)
* [Azure Kubernetes Service](/azure/aks/use-managed-identity)
* [Azure Logic Apps](/azure/logic-apps/create-managed-service-identity)
* [Azure Service Bus](../../service-bus-messaging/service-bus-managed-service-identity.md)


## Welche Azure-Dienste unterstützen das Feature?<a name="which-azure-services-support-managed-identity"></a>

Verwaltete Identitäten für Azure-Ressourcen können zur Authentifizierung bei Diensten verwendet werden, die die Azure AD-Authentifizierung unterstützen. Eine Liste der Azure-Dienste, die die Funktion für verwaltete Identitäten für Azure-Ressourcen unterstützen, finden Sie unter [Services that support managed identities for Azure resources](services-support-msi.md) (Dienste, die verwaltete Identitäten für Azure-Ressourcen unterstützen).

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den ersten Schritten mit der Funktion für verwaltete Identitäten für Azure-Ressourcen finden Sie in den folgenden Schnellstartanleitungen:

* [Use a Windows VM system-assigned managed identity to access Resource Manager](tutorial-windows-vm-access-arm.md) (Verwenden der systemseitig zugewiesenen verwalteten Identität einer Windows-VM für den Zugriff auf Resource Manager)
* [Use a Linux VM system-assigned managed identity to access Resource Manager](tutorial-linux-vm-access-arm.md) (Verwenden der systemseitig zugewiesenen verwalteten Identität einer Linux-VM für den Zugriff auf Resource Manager)

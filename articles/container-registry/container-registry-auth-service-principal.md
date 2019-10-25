---
title: Azure Container Registry-Authentifizierung mit einem Dienstprinzipal
description: Gewähren Sie Zugriff auf Images in Ihrer privaten Containerregistrierung, indem Sie einen Azure Active Directory-Dienstprinzipal verwenden.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 12/13/2018
ms.author: danlep
ms.openlocfilehash: 16ad37eaa50f0c3825d131338cc4a0abdc369978
ms.sourcegitcommit: b4665f444dcafccd74415fb6cc3d3b65746a1a31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72262873"
---
# <a name="azure-container-registry-authentication-with-service-principals"></a>Azure Container Registry-Authentifizierung mit Dienstprinzipalen

Sie können einen Dienstprinzipal von Azure Active Directory (Azure AD) verwenden, um den Containerimages `docker push` und `pull` den Zugriff auf Ihre Containerregistrierung zu gewähren. Mithilfe eines Dienstprinzipals können Sie den Zugriff für „monitorlose“ Dienste und Anwendungen ermöglichen.

## <a name="what-is-a-service-principal"></a>Was ist ein Dienstprinzipal?

*Dienstprinzipale* von Azure AD ermöglichen den Zugriff auf Azure-Ressourcen innerhalb Ihres Abonnements. Sie können sich einen Dienstprinzipal als Benutzeridentität für einen Dienst vorstellen, wobei „Dienst“ jede Anwendung, jeder Dienst oder jede Plattform sein kann, die auf die Ressourcen zugreifen muss. Sie haben die Möglichkeit, einen Dienstprinzipal mit Zugriffsrechten so zu konfigurieren, dass diese nur für die von Ihnen angegebenen Ressourcen gelten. Anschließend legen Sie für Ihre Anwendung bzw. Ihren Dienst fest, dass diese bzw. dieser die Anmeldeinformationen des Dienstprinzipals verwendet, um auf diese Ressourcen zuzugreifen.

Im Rahmen der Azure Container Registry können Sie einen Azure AD-Dienstprinzipal mit Pull-, Push- und Pull- oder anderen Berechtigungen für Ihre private Registrierung in Azure erstellen. Eine vollständige Liste finden Sie unter [Azure Container Registry – Rollen und Berechtigungen](container-registry-roles.md).

## <a name="why-use-a-service-principal"></a>Gründe für die Verwendung eines Dienstprinzipals

Mithilfe eines Azure AD-Dienstprinzipals können Sie bereichsbezogenen Zugriff auf Ihre private Containerregistrierung gewähren. Erstellen Sie für Ihre einzelnen Anwendungen oder Dienste verschiedene Dienstprinzipale, die jeweils über abgestimmte Zugriffsrechte für die Registrierung verfügen. Und da Sie die Anmeldeinformationen nicht an Dienste und Anwendungen weitergeben müssen, haben Sie die Möglichkeit, ausschließlich für den gewählten Dienstprinzipal (und somit für die Anwendung) Anmeldeinformationen weiterzugeben oder Zugriffsrechte zu widerrufen.

Konfigurieren Sie beispielsweise Ihre Webanwendung für die Verwendung eines Dienstprinzipals, der ihr nur Zugriff auf Image `pull` ermöglicht, während Ihr Buildsystem einen Dienstprinzipal verwendet, der ihm sowohl Zugriff auf `push` als auch `pull` bietet. Wenn die Entwicklung Ihrer Anwendung von einer anderen Person übernommen wird, können Sie die Anmeldeinformationen für den Dienstprinzipal weitergeben, ohne das Buildsystem zu beeinflussen.

## <a name="when-to-use-a-service-principal"></a>Einsatzbereiche eines Dienstprinzipals

Sie sollten einen Dienstprinzipal verwenden, um in **monitorlosen Szenarien** Zugriff auf die Registrierung bieten. Das heißt, dies betrifft jede Anwendung, jeden Dienst oder jedes Skript, das Containerimages in automatisierter oder anderweitig unbeaufsichtigter Weise pushen oder pullen muss. Beispiel:

  * *Pull*: Bereitstellen von Containern aus einer Registrierung für Orchestrierungssysteme, z.B. Kubernetes, DC/OS und Docker Swarm. Sie können auch Pullvorgänge aus Containerregistrierungen in verwandte Azure-Dienste durchführen, z. B. [Azure Kubernetes Service (AKS)](../aks/cluster-container-registry-integration.md), [Azure Container Instances](container-registry-auth-aci.md), [App Service](../app-service/index.yml), [Batch](../batch/index.yml), [Service Fabric](/azure/service-fabric/) und weitere Dienste.

  * *Push*: Erstellen von Containerimages und deren Übertragung per Pushvorgang in eine Registrierung mithilfe von Lösungen für Continuous Integration und Continuous Deployment wie Azure Pipelines oder Jenkins.

Für den individuellen Zugriff auf eine Registrierung, etwa, wenn Sie manuell ein Containerimage auf Ihre Entwicklungsarbeitsstation pullen, empfehlen wir, dass Sie stattdessen Ihre eigene [Azure AD-Identität](container-registry-authentication.md#individual-login-with-azure-ad) für den Registrierungszugriff verwenden (beispielsweise mit [az acr login][az-acr-login]).

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

### <a name="sample-scripts"></a>Beispielskripts

Die obigen Beispielskripts für die Azure CLI auf GitHub sowie Versionen für Azure PowerShell finden Sie hier:

* [Azure-Befehlszeilenschnittstelle][acr-scripts-cli]
* [Azure PowerShell][acr-scripts-psh]

## <a name="authenticate-with-the-service-principal"></a>Authentifizieren mit dem Dienstprinzipal

Sobald Sie einem Dienstprinzipal Zugriff auf Ihre Containerregistrierung gewährt haben, können Sie seine Anmeldeinformationen für den Zugriff auf „monitorlose“ Dienste und Anwendungen konfigurieren oder sie mit dem Befehl `docker login` eingeben. Verwenden Sie die folgenden Werte:

* **User Name (Benutzername)** : Anwendungs-ID des Dienstprinzipals (auch als *Client-ID* bezeichnet)
* **Password (Kennwort)** : Kennwort des Dienstprinzipals (auch als *geheimer Clientschlüssel* bezeichnet)

Jeder Wert ist eine GUID mit den Format `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`. 

> [!TIP]
> Sie können das Kennwort eines Dienstprinzipals neu generieren, indem Sie den Befehl [az ad sp reset-credentials](/cli/azure/ad/sp/credential#az-ad-sp-credential-reset) ausführen.
>

### <a name="use-credentials-with-azure-services"></a>Verwenden von Anmeldeinformationen mit Azure-Diensten

Sie können die Anmeldeinformationen des Dienstprinzipals in jedem Azure-Dienst verwenden, der mit einer Azure-Containerregistrierung authentifiziert werden kann.  Dienstprinzipal-Anmeldeinformationen können anstelle der Administratoranmeldeinformationen der Registrierung in einer Vielzahl von Szenarien verwendet werden.

So können Sie mithilfe der Anmeldeinformationen beispielsweise ein Image aus einer Azure-Containerregistrierung in [Azure Container Instances](container-registry-auth-aci.md) pullen.

### <a name="use-with-docker-login"></a>Verwenden mit Docker-Anmeldung

Sie können `docker login` auch unter Verwendung eines Dienstprinzipals ausführen. Im folgenden Beispiel wird die Anwendungs-ID des Dienstprinzipals in der Umgebungsvariablen `$SP_APP_ID` und das Kennwort in der Variablen `$SP_PASSWD` übergeben. Bewährte Methoden zur Verwaltung von Docker-Anmeldeinformationen finden Sie in der Befehlsreferenz zu [docker login](https://docs.docker.com/engine/reference/commandline/login/).

```bash
# Log in to Docker with service principal credentials
docker login myregistry.azurecr.io --username $SP_APP_ID --password $SP_PASSWD
```

Nach der Anmeldung speichert Docker die Anmeldeinformationen zwischen.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Szenarios für die Authentifizierung mit einer Azure-Containerregistrierung finden Sie in der [Authentifizierungsübersicht](container-registry-authentication.md).

* Ein Beispiel für die Verwendung eines Azure-Schlüsseltresors zum Speichern und Abrufen von Dienstprinzipal-Anmeldeinformationen für eine Containerregistrierung finden Sie in dem Tutorial zum [Erstellen und Bereitstellen eines Containerimages mithilfe von ACR Tasks](container-registry-tutorial-quick-task.md).

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
[az-acr-login]: /cli/azure/acr#az-acr-login

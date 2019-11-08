---
title: Schützen von Webdiensten über SSL
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie HTTPS aktivieren, um einen über Azure Machine Learning bereitgestellten Webdienst zu schützen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 08/12/2019
ms.custom: seodec18
ms.openlocfilehash: 1455ec17898e82ed0f39fea66c44d2e9b4f57280
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73489542"
---
# <a name="use-ssl-to-secure-a--through-azure-machine-learning"></a>Verwenden von SSL zum Schützen eines Webdiensts mit Azure Machine Learning
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie einen über Azure Machine Learning bereitgestellten Webdienst schützen.

Verwenden Sie [HTTPS](https://en.wikipedia.org/wiki/HTTPS), um den Zugriff auf Webdienste zu beschränken und Daten zu schützen, die Clients übermitteln. Mit HTTPS wird die Kommunikation zwischen einem Client und einem Webdienst geschützt, indem sie verschlüsselt wird. Die Verschlüsselung verwendet [Transport Layer Security (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security). In einigen Fällen wird TLS immer noch als *Secure Sockets Layer* (SSL) bezeichnet, der Vorgänger von TLS.

> [!TIP]
> Im Azure Machine Learning SDK wird der Begriff „SSL“ für Eigenschaften im Zusammenhang mit sicherer Kommunikation verwendet. Dies bedeutet nicht, dass Ihr Webdienst nicht *TLS* verwendet. SSL ist nur ein gebräuchlicherer Begriff.

Sowohl TLS als auch SSL beruhen auf *digitalen Zertifikaten*, die zur Verschlüsselung und Identitätsüberprüfung verwendet werden. Weitere Informationen zur Funktionsweise digitaler Zertifikate finden Sie im Wikipedia-Thema [Public key infrastructure](https://en.wikipedia.org/wiki/Public_key_infrastructure) (Public-Key-Infrastruktur).

> [!WARNING]
> Wenn Sie HTTPS nicht für Ihren Webdienst verwenden, sind vom Dienst an andere Benutzer und zurück gesendete Daten möglicherweise im Internet sichtbar.
>
> HTTPS ermöglicht dem Client auch, die Authentizität des Servers, mit dem eine Verbindung hergestellt wird, zu überprüfen. Dieses Feature schützt Clients vor [Man-in-the-Middle-Angriffen](https://en.wikipedia.org/wiki/Man-in-the-middle_attack).

Dies ist der allgemeine Prozess zum Schützen eines Webdiensts:

1. Rufen Sie einen Domänennamen ab.

2. Rufen Sie ein digitales Zertifikat ab.

3. Stellen Sie den Webdienst mit aktiviertem SSL bereit, oder aktualisieren Sie ihn mit aktiviertem SSL.

4. Aktualisieren Sie Ihren DNS, sodass er auf den Dienst verweist.

> [!IMPORTANT]
> Wenn Sie für Azure Kubernetes Service (AKS) bereitstellen, können Sie Ihr eigenes Zertifikat erwerben oder ein von Microsoft bereitgestelltes Zertifikat verwenden. Wenn Sie ein Zertifikat von Microsoft verwenden, müssen Sie weder einen Domänennamen noch ein SSL-Zertifikat abrufen. Weitere Informationen finden Sie im Abschnitt [Aktivieren und Bereitstellen von SSL](#enable) dieses Artikels.

Es bestehen bei den verschiedenen [Bereitstellungszielen](how-to-deploy-and-where.md) geringfügige Unterschiede beim Schützen von Webdiensten.

## <a name="get-a-domain-name"></a>Abrufen eines Domänennamens

Wenn Sie nicht bereits über einen Domänennamen verfügen, können Sie einen solchen von einer *Domänennamen-Registrierungsstelle* erwerben. Die Prozesse und Preise der Registrierungsstellen sind unterschiedlich. Die Registrierungsstelle bietet Tools zum Verwalten des Domänennamens. Sie verwenden diese Tools zum Zuordnen eines vollqualifizierten Domänennamens (Fully Qualified Domain Name, FQDN) – z. B. www\.contoso.com – zu der IP-Adresse, die Ihren Webdienst hostet.

## <a name="get-an-ssl-certificate"></a>Beziehen eines SSL-Zertifikats

Es gibt viele Möglichkeiten, ein SSL-Zertifikat (digitales Zertifikat) zu erhalten. Die verbreitetste Methode ist der Erwerb eines Zertifikats von einer *Zertifizierungsstelle* (Certificate Authority, CA). Unabhängig davon, wo Sie das Zertifikat abrufen, benötigen Sie die folgenden Dateien:

* Ein **Zertifikat**. Das Zertifikat muss die vollständige Zertifikatkette enthalten und „PEM-codiert“ sein.
* Einen **Schlüssel**. Der Schlüssel muss auch PEM-codiert sein.

Wenn Sie ein Zertifikat anfordern, müssen Sie den FQDN der Adresse angeben, die Sie für den Webdienst verwenden möchten (z. B. www\.contoso.com). Die in das Zertifikat gestempelte Adresse und die von den Clients verwendete Adresse werden verglichen, um die Identität des Webdiensts zu überprüfen. Wenn diese Adressen nicht übereinstimmen, erhält der Client eine Fehlermeldung.

> [!TIP]
> Wenn die Zertifizierungsstelle das Zertifikat und den Schlüssel nicht als PEM-codierte Dateien bereitstellen kann, können Sie ein Hilfsprogramm wie [OpenSSL](https://www.openssl.org/) zum Ändern des Formats verwenden.

> [!WARNING]
> Verwenden Sie *selbstsignierte* Zertifikate nur für die Entwicklung. Verwenden Sie sie nicht in Produktionsumgebungen. Selbstsignierte Zertifikate können Probleme in Ihren Clientanwendungen verursachen. Weitere Informationen finden Sie in der Dokumentation für die Netzwerkbibliotheken, die Ihre Clientanwendung verwendet.

## <a id="enable"></a> Aktivieren und Bereitstellen von SSL

Legen Sie zum Bereitstellen (oder erneuten Bereitstellen) des Diensts mit aktiviertem SSL den *ssl_enabled*-Parameter auf „True“ fest, wenn zutreffend. Legen Sie den *ssl_certificate*-Parameter auf den Wert der *Zertifikatdatei* fest. Legen Sie den *ssl_key* auf den Wert der *Schlüsseldatei* fest.

### <a name="deploy-on-aks-and-field-programmable-gate-array-fpga"></a>Bereitstellen auf AKS und Field-Programmable Gate Array (FPGA)

  > [!NOTE]
  > Die Informationen in diesem Abschnitt gelten auch, wenn ein sicherer Webdienst für den Designer bereitgestellt wird. Wenn Sie das Python SDK bisher noch nicht verwendet haben, sollten Sie [Was ist das Azure Machine Learning-SDK für Python?](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) lesen.

Beim Bereitstellen in AKS können Sie einen neuen AKS-Cluster erstellen oder einen vorhandenen anfügen. Weitere Informationen zum Erstellen oder Anfügen eines Clusters finden Sie unter [Bereitstellen eines Modells in einem Azure Kubernetes Service-Cluster](how-to-deploy-azure-kubernetes-service.md).
  
-  Wenn Sie einen neuen Cluster erstellen, verwenden Sie **[AksCompute.provisionining_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute#provisioning-configuration-agent-count-none--vm-size-none--ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--location-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--service-cidr-none--dns-service-ip-none--docker-bridge-cidr-none--cluster-purpose-none-)** .
- Wenn Sie einen vorhandenen Cluster anfügen, verwenden Sie **[AksCompute.attach_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-)** . Beide geben ein Konfigurationsobjekt mit einer **enable_ssl**-Methode zurück.

Die **enable_ssl**-Methode kann ein Zertifikat verwenden, das von Microsoft bereitgestellt wird, oder ein von Ihnen erworbenes Zertifikat.

  * Wenn Sie ein Zertifikat von Microsoft verwenden, müssen Sie den *leaf_domain_label*-Parameter verwenden. Dieser Parameter generiert den DNS-Namen für den Dienst. Der Wert „myservice“ erstellt z.B. den Domänennamen „myservice\<sechs zufällige Zeichen>.\<Azureregion>.cloudapp.azure.com“, wobei \<Azureregion> die Region ist, die den Dienst enthält. Optional können Sie den *overwrite_existing_domain*-Parameter zum Überschreiben des vorhandenen *leaf_domain_label* verwenden.

    Legen Sie zum Bereitstellen (oder erneuten Bereitstellen) des Diensts mit aktiviertem SSL den *ssl_enabled*-Parameter auf „True“ fest, wenn zutreffend. Legen Sie den *ssl_certificate*-Parameter auf den Wert der *Zertifikatdatei* fest. Legen Sie den *ssl_key* auf den Wert der *Schlüsseldatei* fest.

    > [!IMPORTANT]
    > Wenn Sie ein Zertifikat von Microsoft verwenden, müssen Sie nicht Ihr eigenes Zertifikat oder Ihren eigenen Domänennamen erwerben.

    Im folgenden Beispiel wird das Erstellen einer Konfiguration veranschaulicht, die ein SSL-Zertifikat von Microsoft aktiviert:

    ```python
    from azureml.core.compute import AksCompute
    # Config used to create a new AKS cluster and enable SSL
    provisioning_config = AksCompute.provisioning_configuration()
    provisioning_config.enable_ssl(leaf_domain_label = "myservice")
    # Config used to attach an existing AKS cluster to your workspace and enable SSL
    attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                          cluster_name = cluster_name)
    attach_config.enable_ssl(leaf_domain_label = "myservice")
    ```

  * Wenn Sie *ein Zertifikat verwenden, das Sie erworben haben*, verwenden Sie die Parameter *ssl_cert_pem_file*, *ssl_key_pem_file* und *ssl_cname*. Im folgenden Beispiel wird veranschaulicht, wie *PEM*-Dateien zum Erstellen einer Konfiguration verwendet werden, die ein von Ihnen erworbenes SSL-Zertifikat verwendet:

    ```python
    from azureml.core.compute import AksCompute
    # Config used to create a new AKS cluster and enable SSL
    provisioning_config = AksCompute.provisioning_configuration()
    provisioning_config.enable_ssl(ssl_cert_pem_file="cert.pem",
                                        ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    # Config used to attach an existing AKS cluster to your workspace and enable SSL
    attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                         cluster_name = cluster_name)
    attach_config.enable_ssl(ssl_cert_pem_file="cert.pem",
                                        ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

Weitere Informationen zu *enable_ssl* finden Sie unter [AksProvisioningConfiguration.enable_ssl()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.aksprovisioningconfiguration?view=azure-ml-py#enable-ssl-ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--leaf-domain-label-none--overwrite-existing-domain-false-) und [AksAttachConfiguration.enable_ssl()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.aksattachconfiguration?view=azure-ml-py#enable-ssl-ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--leaf-domain-label-none--overwrite-existing-domain-false-).

### <a name="deploy-on-azure-container-instances"></a>Bereitstellen in Azure Container Instances

Wenn Sie in Azure Container Instances bereitstellen möchten, geben Sie Werte für die SSL-Parameter an, wie im folgenden Codeausschnitt gezeigt:

```python
from azureml.core.webservice import AciWebservice

aci_config = AciWebservice.deploy_configuration(
    ssl_enabled=True, ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
```

Weitere Informationen finden Sie unter [AciWebservice.deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none-).

## <a name="update-your-dns"></a>Aktualisieren Ihres DNS

Als Nächstes müssen Sie Ihren DNS aktualisieren, sodass er auf den Webdienst verweist.

+ **Für Container Instances:**

  Verwenden Sie die Tools von Ihrer Domänennamen-Registrierungsstelle, um den DNS-Eintrag für Ihren Domänennamen zu aktualisieren. Der Eintrag muss auf die IP-Adresse des Diensts verweisen.

  Abhängig von der Registrierungsstelle und dem TTL-Wert (Time-to-Live), der für den Domänennamen konfiguriert ist, kann es einige Minuten bis mehrere Stunden dauern, bevor Clients den Domänennamen auflösen können.

+ **Für AKS**:

  > [!WARNING]
  > Wenn Sie *leaf_domain_label* zum Erstellen des Diensts mit einem Zertifikat von Microsoft verwendet haben, aktualisieren Sie den DNS-Wert für den Cluster nicht manuell. Der Wert sollte automatisch festgelegt werden.

  Aktualisieren Sie das DNS der öffentlichen IP-Adresse des AKS-Clusters im linken Bereich auf der Registerkarte **Konfiguration** unter **Einstellungen**. (Siehe folgende Abbildung.) Die öffentliche IP-Adresse ist ein Ressourcentyp, der unter der Ressourcengruppe erstellt wird, die die AKS-Agent-Knoten und weitere Netzwerkressourcen enthält.

  [![Azure Machine Learning: Schützen von Webdiensten mit SSL](./media/how-to-secure-web-service/aks-public-ip-address.png)](./media/how-to-secure-web-service/aks-public-ip-address-expanded.png)

## <a name="update-the-ssl-certificate"></a>Aktualisieren des SSL-Zertifikats

SSL-Zertifikate laufen ab und müssen erneuert werden. In der Regel erfolgt dies jedes Jahr. Verwenden Sie die Informationen in den folgenden Abschnitten, um das Zertifikat für in Azure Kubernetes Service bereitgestellte Modelle zu aktualisieren und zu erneuern:

### <a name="update-a-microsoft-generated-certificate"></a>Aktualisieren eines von Microsoft generierten Zertifikats

Wenn das Zertifikat ursprünglich von Microsoft generiert wurde (bei Verwendung von *leaf_domain_label* zum Erstellen des Diensts), verwenden Sie eines der folgenden Beispiele, um das Zertifikat zu aktualisieren:

**Verwenden des SDK**

```python
from azureml.core.compute import AksCompute
from azureml.core.compute.aks import AksUpdateConfiguration
from azureml.core.compute.aks import SslConfiguration

# Get the existing cluster
aks_target = AksCompute(ws, clustername)

# Update the existing certificate by referencing the leaf domain label
ssl_configuration = SslConfiguration(leaf_domain_label="myaks", overwrite_existing_domain=True)
update_config = AksUpdateConfiguration(ssl_configuration)
aks_target.update(update_config)
```

**Verwenden der Befehlszeilenschnittstelle**

```azurecli
az ml computetarget update aks -g "myresourcegroup" -w "myresourceworkspace" -n "myaks" --ssl-leaf-domain-label "myaks" --ssl-overwrite-domain True
```

Weitere Informationen finden Sie in den folgenden Referenzen:

* [SslConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.sslconfiguration?view=azure-ml-py)
* [AksUpdateConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.aksupdateconfiguration?view=azure-ml-py)

### <a name="update-custom-certificate"></a>Aktualisieren eines benutzerdefinierten Zertifikats

Wenn das Zertifikat ursprünglich von einer Zertifizierungsstelle generiert wurde, gehen Sie wie folgt vor:

1. Verwenden Sie die von der Zertifizierungsstelle bereitgestellte Dokumentation, um das Zertifikat zu erneuern. Bei diesem Vorgang werden neue Zertifikatsdateien erstellt.

1. Aktualisieren Sie den Dienst mithilfe des SDK oder über die Befehlszeilenschnittstelle mit dem neuen Zertifikat:

    **Verwenden des SDK**

    ```python
    from azureml.core.compute import AksCompute
    from azureml.core.compute.aks import AksUpdateConfiguration
    from azureml.core.compute.aks import SslConfiguration
    
    # Read the certificate file
    def get_content(file_name):
        with open(file_name, 'r') as f:
            return f.read()

    # Get the existing cluster
    aks_target = AksCompute(ws, clustername)
    
    # Update cluster with custom certificate
    ssl_configuration = SslConfiguration(cname="myaks", cert=get_content('cert.pem'), key=get_content('key.pem'))
    update_config = AksUpdateConfiguration(ssl_configuration)
    aks_target.update(update_config)
    ```

    **Verwenden der Befehlszeilenschnittstelle**

    ```azurecli
    az ml computetarget update aks -g "myresourcegroup" -w "myresourceworkspace" -n "myaks" --ssl-cname "myaks"--ssl-cert-file "cert.pem" --ssl-key-file "key.pem"
    ```

Weitere Informationen finden Sie in den folgenden Referenzen:

* [SslConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.sslconfiguration?view=azure-ml-py)
* [AksUpdateConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.aksupdateconfiguration?view=azure-ml-py)

## <a name="disable-ssl"></a>Deaktivieren von SSL

Erstellen Sie zum Deaktivieren von SSL für ein in Azure Kubernetes Service bereitgestelltes Modell ein `SslConfiguration`-Element mit `status="Disabled"`, und führen Sie dann die Aktualisierung aus:

```python
from azureml.core.compute import AksCompute
from azureml.core.compute.aks import AksUpdateConfiguration
from azureml.core.compute.aks import SslConfiguration

# Get the existing cluster
aks_target = AksCompute(ws, clustername)

# Disable SSL
ssl_configuration = SslConfiguration(status="Disabled")
update_config = AksUpdateConfiguration(ssl_configuration)
aks_target.update(update_config)
```

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel werden folgende Themen erläutert:
+ [Nutzen eines als Webdienst bereitgestellten Machine Learning-Modells](how-to-consume-web-service.md)
+ [Sicheres Ausführen von Experimenten und Ziehen von Rückschlüssen innerhalb eines virtuellen Azure-Netzwerks](how-to-enable-virtual-network.md)

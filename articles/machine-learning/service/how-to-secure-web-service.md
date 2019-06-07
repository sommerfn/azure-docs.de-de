---
title: Sichern von Webdiensten mit SSL
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie einen mit Azure Machine Learning Service bereitgestellten Webdienst schützen, indem Sie HTTPS aktivieren. HTTPS schützt die von Clients übertragenen Daten mithilfe von TLS (Transport Layer Security), einem Ersatz für SSL (Secure Sockets Layer). HTTPS wird auch von Clients zum Überprüfen der Identität des Webdiensts verwendet.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 04/29/2019
ms.custom: seodec18
ms.openlocfilehash: 527f16e34e0f21d435fbd166328235566687bc88
ms.sourcegitcommit: 16cb78a0766f9b3efbaf12426519ddab2774b815
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2019
ms.locfileid: "65852007"
---
# <a name="use-ssl-to-secure-web-services-with-azure-machine-learning-service"></a>Verwenden von SSL zum Schützen von Webdiensten mit Azure Machine Learning Service

In diesem Artikel erfahren Sie, wie Sie einen mit dem Azure Machine Learning-Dienst bereitgestellten Webdienst schützen. Mithilfe von [HTTPS (Hypertext Transfer Protocol Secure)](https://en.wikipedia.org/wiki/HTTPS) können Sie den Zugriff auf Webdienste einschränken und die von Clients übermittelten Daten schützen.

Mit HTTPS wird die Kommunikation zwischen einem Client und Ihrem Webdienst geschützt, indem sie verschlüsselt wird. Die Verschlüsselung erfolgt mit [TLS (Transport Layer Security)](https://en.wikipedia.org/wiki/Transport_Layer_Security). In einigen Fällen wird TLS immer noch als SSL (Secure Sockets Layer) bezeichnet, was der Vorgänger von TLS war.

> [!TIP]
> Im Azure Machine Learning SDK wird der Begriff „SSL“ für Eigenschaften im Zusammenhang mit sicherer Kommunikation verwendet. Das bedeutet nicht, dass der Webdienst nicht TLS verwendet, sondern nur, dass SSL für viele Leser der bekanntere Begriff ist.

Sowohl TLS als auch SSL beruhen auf __digitalen Zertifikaten__, die zur Verschlüsselung und Identitätsüberprüfung verwendet werden. Weitere Informationen zur Funktionsweise digitaler Zertifikate finden Sie im Wikipedia-Eintrag zur [Public Key-Infrastruktur](https://en.wikipedia.org/wiki/Public_key_infrastructure).

> [!Warning]
> Wenn Sie HTTPS nicht für Ihren Webdienst aktivieren und verwenden, sind vom Dienst an andere Benutzer und zurück gesendete Daten möglicherweise im Internet sichtbar.
>
> HTTPS ermöglicht dem Client auch, die Authentizität des Servers, mit dem eine Verbindung hergestellt wird, zu überprüfen. Dies schützt Clients vor [Man-in-the-Middle-Angriffen](https://en.wikipedia.org/wiki/Man-in-the-middle_attack).

Das allgemeine Verfahren zum Absichern eines neuen oder bereits vorhandenen Webdiensts lautet wie folgt:

1. Rufen Sie einen Domänennamen ab.

2. Rufen Sie ein digitales Zertifikat ab.

3. Stellen Sie den Webdienst mit aktivierter SSL-Einstellung bereit, oder aktualisieren Sie ihn.

4. Aktualisieren Sie Ihren DNS, so dass er auf den Dienst verweist.

> [!IMPORTANT]
> Wenn Sie für Azure Kubernetes Service (AKS) bereitstellen, können Sie Ihr eigenes Zertifikat bereitstellen oder ein von Microsoft bereitgestelltes Zertifikat verwenden. Wenn Sie das von Microsoft bereitgestellte Zertifikat verwenden, müssen Sie weder einen Domänennamen noch ein SSL-Zertifikat abrufen. Weitere Informationen finden Sie im Abschnitt [Aktivieren und Bereitstellen von SSL](#enable).

Es bestehen bei den verschiedenen [Bereitstellungszielen](how-to-deploy-and-where.md) geringfügige Unterschiede beim Schützen von Webdiensten.

## <a name="get-a-domain-name"></a>Abrufen eines Domänennamens

Wenn Sie nicht bereits über einen Domänennamen verfügen, können Sie einen solchen von einer __Domänennamen-Registrierungsstelle__ erwerben. Der Vorgang unterscheidet sich (ebenso wie die Kosten) abhängig von der Domänenregistrierungsstelle. Die Registrierungsstelle stellt Ihnen außerdem Tools zum Verwalten des Domänennamens zur Verfügung. Diese Tools werden zum Zuordnen eines vollqualifizierten Domänennamens (z. B. www\.contoso.com) zu der IP-Adresse verwendet, unter der Ihr Dienst gehostet ist.

## <a name="get-an-ssl-certificate"></a>Beziehen eines SSL-Zertifikats

Es gibt viele Möglichkeiten, ein SSL-Zertifikat (digitales Zertifikat) zu erhalten. Die verbreitetste Methode ist der Erwerb eines Zertifikats von einer __Zertifizierungsstelle__ (CA). Unabhängig davon, wo Sie das Zertifikat abrufen, benötigen Sie die folgenden Dateien:

* Ein __Zertifikat__. Das Zertifikat muss die vollständige Zertifikatkette enthalten und PEM-codiert sein.
* Einen __Schlüssel__. Der Schlüssel muss PEM-codiert sein.

Wenn Sie ein Zertifikat anfordern, müssen Sie den vollqualifizierten Domänennamen (FQDN) der Adresse angeben, die Sie für den Webdienst verwenden möchten. Beispiel: www\.contoso.com. Die in das Zertifikat gestempelte Adresse und die von den Clients verwendete Adresse werden verglichen, wenn die Identität des Webdiensts überprüft wird. Wenn die Adressen nicht übereinstimmen, erhalten die Clients einen Fehler.

> [!TIP]
> Wenn die Zertifizierungsstelle das Zertifikat und den Schlüssel nicht als PEM-codierte Dateien bereitstellen kann, können Sie ein Hilfsprogramm wie [OpenSSL](https://www.openssl.org/) zum Ändern des Formats verwenden.

> [!WARNING]
> Selbstsignierte Zertifikate sollten nur für die Entwicklung verwendet werden. Sie sollten nicht in der Produktion eingesetzt werden. Selbstsignierte Zertifikate können Probleme in Ihren Clientanwendungen verursachen. Weitere Informationen finden Sie in der Dokumentation für die in der Clientanwendung verwendeten Netzwerkbibliotheken.

## <a id="enable"></a> Aktivieren und Bereitstellen von SSL

Legen Sie zum Bereitstellen (oder erneuten Bereitstellen) des Diensts mit aktiviertem SSL den `ssl_enabled`-Parameter auf `True` fest, wenn zutreffend. Legen Sie den `ssl_certificate`-Parameter auf den Wert der __Zertifikat__datei und den `ssl_key` auf den Wert der __Schlüssel__datei fest.

+ **Bereitstellen in Azure Kubernetes Service (AKS) und FPGA**

  > [!NOTE]
  > Die Informationen in diesem Abschnitt gelten auch, wenn ein sicherer Webdienst für die grafische Benutzeroberfläche bereitgestellt wird. Wenn Sie das Python SDK bisher noch nicht verwendet haben, sollten Sie [Was ist das Azure Machine Learning-SDK für Python?](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) lesen.

  Beim Bereitstellen in AKS können Sie entweder einen neuen AKS-Cluster erstellen oder einen vorhandenen anfügen. Beim Erstellen eines neuen Clusters wird [AksCompute.provisionining_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#provisioning-configuration-agent-count-none--vm-size-none--ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--location-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--service-cidr-none--dns-service-ip-none--docker-bridge-cidr-none-) verwendet, beim Anfügen eines vorhandenen Clusters [AksCompute.attach_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none-). Beide geben ein Konfigurationsobjekt mit einer `enable_ssl`-Methode zurück.

  Die `enable_ssl`-Methode kann entweder ein von Microsoft oder von Ihnen bereitgestelltes Zertifikat verwenden.

  * Wenn Sie ein __von Microsoft bereitgestelltes__ Zertifikat verwenden, müssen Sie den `leaf_domain_label`-Parameter verwenden. Beim Verwenden dieses Parameters wird der Dienst mit einem von Microsoft bereitgestellten Zertifikat erstellt. Mit dem `leaf_domain_label` wird der DNS-Name für den Dienst generiert. Der Wert `myservice` erstellt z.B. den Domänennamen `myservice<6-random-characters>.<azureregion>.cloudapp.azure.com`, wobei `<azureregion>` die Region ist, die den Dienst enthält. Optional können Sie den `overwrite_existing_domain`-Parameter zum Überschreiben der vorhandenen Bezeichnung der untergeordneten Domäne verwenden.

    Legen Sie zum Bereitstellen (oder erneuten Bereitstellen) des Diensts mit aktiviertem SSL den `ssl_enabled`-Parameter auf `True` fest, wenn zutreffend. Legen Sie den `ssl_certificate`-Parameter auf den Wert der __Zertifikat__datei und den `ssl_key` auf den Wert der __Schlüssel__datei fest.

    > [!IMPORTANT]
    > Wenn Sie ein von Microsoft bereitgestelltes Zertifikat verwenden, müssen Sie nicht Ihr eigenes Zertifikat oder Ihren eigenen Domänennamen erwerben.

    Im folgenden Beispiel wird das Erstellen von Konfigurationen veranschaulicht, die ein von Microsoft erstelltes SSL-Zertifikat aktivieren:

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

  * Bei Verwendung __eines Zertifikats, das Sie erworben haben__, verwenden Sie die Parameter `ssl_cert_pem_file`, `ssl_key_pem_file` und `ssl_cname`. Im folgenden Beispiel wird das Erstellen von Konfigurationen veranschaulicht, die ein von Ihnen mit `.pem`-Dateien bereitgestelltes SSL-Zertifikat verwenden:

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

  Weitere Informationen zu `enable_ssl` finden Sie unter [AksProvisioningConfiguration.enable_ssl()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.aksprovisioningconfiguration?view=azure-ml-py#enable-ssl-ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--leaf-domain-label-none--overwrite-existing-domain-false-) und [AksAttachConfiguration.enable_ssl()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.aksattachconfiguration?view=azure-ml-py#enable-ssl-ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--leaf-domain-label-none--overwrite-existing-domain-false-).

+ **Bereitstellen auf Azure Container Instances (ACI)**

  Geben Sie beim Bereitstellen auf ACI Werte für die auf SSL bezogenen Parameter ein, wie in diesem Codeausschnitt gezeigt:

    ```python
    from azureml.core.webservice import AciWebservice

    aci_config = AciWebservice.deploy_configuration(ssl_enabled=True, ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

  Weitere Informationen finden Sie unter [AciWebservice.deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none-).

## <a name="update-your-dns"></a>Aktualisieren Ihres DNS

Als Nächstes müssen Sie Ihren DNS aktualisieren, so dass er auf den Dienst verweist.

+ **Für ACI**:

  Verwenden Sie die von Ihrer Domänennamen-Registrierungsstelle bereitgestellten Tools, um den DNS-Eintrag für Ihren Domänennamen zu aktualisieren. Der Eintrag muss auf die IP-Adresse des Diensts verweisen.

  Abhängig von der Registrierungsstelle und dem TTL-Wert (Time-to-Live), der für den Domänennamen konfiguriert ist, kann es einige Minuten bis mehrere Stunden dauern, bevor Clients den Domänennamen auflösen können.

+ **Für AKS**:

  > [!WARNING]
  > Wenn Sie das `leaf_domain_label` zum Erstellen des Diensts mit einem von Microsoft bereitgestellten Zertifikat verwendet haben, aktualisieren Sie den DNS-Wert für den Cluster nicht manuell. Der Wert sollte automatisch festgelegt werden.

  Aktualisieren Sie den DNS auf der Registerkarte „Konfiguration“ unter „öffentliche IP-Adresse“ des AKS-Clusters, wie in der Abbildung dargestellt. Sie finden die öffentliche IP-Adresse als einen der Ressourcentypen, die unter der Ressourcengruppe erstellt wurden, die die AKS-Agent-Knoten und weitere Netzwerkressourcen enthält.

  ![Azure Machine Learning Service: Sichern von Webdiensten mit SSL](./media/how-to-secure-web-service/aks-public-ip-address.png)



## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel werden folgende Themen erläutert:
+ [Nutzen eines als Webdienst bereitgestellten Machine Learning-Modells](how-to-consume-web-service.md)
+ [Sicheres Ausführen von Experimenten und Ziehen von Rückschlüssen innerhalb eines virtuellen Azure-Netzwerks](how-to-enable-virtual-network.md)


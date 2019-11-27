---
title: Verwenden von LetsEncrypt.org-Zertifikaten mit Application Gateway
description: Dieser Artikel enthält Informationen dazu, wie Sie ein Zertifikat von LetsEncrypt.org abrufen und in Ihrer Application Gateway-Instanz für AKS-Cluster verwenden.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 92e9747865f1a0910c8bae4001cc597ae9ea3da6
ms.sourcegitcommit: 39da2d9675c3a2ac54ddc164da4568cf341ddecf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2019
ms.locfileid: "73957981"
---
# <a name="use-certificates-with-letsencryptorg-on-application-gateway-for-aks-clusters"></a>Verwenden von Zertifikaten mit LetsEncrypt.org in Application Gateway für AKS-Cluster

In diesem Abschnitt wird der AKS-Cluster so konfiguriert, dass [LetsEncrypt.org](https://letsencrypt.org/) genutzt und automatisch ein TLS/SSL-Zertifikat für Ihre Domäne abgerufen wird. Das Zertifikat wird in Application Gateway installiert, sodass eine SSL/TLS-Terminierung für den AKS-Cluster ausgeführt wird. Bei der hier beschriebenen Konfiguration wird das Kubernetes-Add-On [cert-manager](https://github.com/jetstack/cert-manager) verwendet, mit dem die Erstellung und Verwaltung von Zertifikaten automatisiert wird.

Führen Sie die nachfolgenden Schritte aus, um [cert-manager](https://docs.cert-manager.io) in Ihrem vorhandenen AKS-Cluster zu installieren.

1. Helm-Chart

    Führen Sie das folgende Skript aus, um das Helm-Chart `cert-manager` zu installieren. Dies bewirkt Folgendes:

    - Erstellen eines neuen `cert-manager`-Namespace im AKS-Cluster
    - Erstellen der folgenden CRDs: Certificate, Challenge, ClusterIssuer, Issuer, Order
    - Installieren des „cert-manager“-Charts (aus [docs.cert-manager.io](https://docs.cert-manager.io/en/latest/getting-started/install/kubernetes.html#steps))

    ```bash
    #!/bin/bash

    # Install the CustomResourceDefinition resources separately
    kubectl apply -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.8/deploy/manifests/00-crds.yaml

    # Create the namespace for cert-manager
    kubectl create namespace cert-manager

    # Label the cert-manager namespace to disable resource validation
    kubectl label namespace cert-manager certmanager.k8s.io/disable-validation=true

    # Add the Jetstack Helm repository
    helm repo add jetstack https://charts.jetstack.io

    # Update your local Helm chart repository cache
    helm repo update

    # Install the cert-manager Helm chart
    helm install \
      --name cert-manager \
      --namespace cert-manager \
      --version v0.8.0 \
      jetstack/cert-manager
    ```

2. ClusterIssuer-Ressource

    Erstellen Sie eine `ClusterIssuer`-Ressource. Diese ist in `cert-manager` erforderlich, um die Zertifizierungsstelle `Lets Encrypt` darzustellen, von der die signierten Zertifikate abgerufen werden.

    Durch Verwendung der Ressource `ClusterIssuer` ohne Namespaces werden mit cert-manager Zertifikate ausgegeben, die in mehreren Namespaces genutzt werden können. `Let’s Encrypt` verwendet das ACME-Protokoll, um zu überprüfen, ob Sie einen bestimmten Domänennamen steuern, und gibt ein Zertifikat aus. Weitere Informationen zum Konfigurieren von `ClusterIssuer`-Eigenschaften finden Sie [hier](https://docs.cert-manager.io/en/latest/tasks/issuers/index.html). `ClusterIssuer` weist `cert-manager` an, Zertifikate mit der `Lets Encrypt`-Stagingumgebung auszustellen, die für Tests verwendet wird (das Stammzertifikat ist in den Vertrauensspeichern des Browsers oder Clients nicht vorhanden).

    Der standardmäßige Abfragetyp in der YAML-Datei unten ist `http01`. Andere Abfragetypen sind unter [letsencrypt.org – Challenge Types](https://letsencrypt.org/docs/challenge-types/) (letsencrypt.org – Abfragetypen) aufgeführt.

    > [!IMPORTANT] 
    > Aktualisieren Sie `<YOUR.EMAIL@ADDRESS>` in der YAML-Datei unten.

    ```bash
    #!/bin/bash
    kubectl apply -f - <<EOF
    apiVersion: certmanager.k8s.io/v1alpha1
    kind: ClusterIssuer
    metadata:
    name: letsencrypt-staging
    spec:
    acme:
        # You must replace this email address with your own.
        # Let's Encrypt will use this to contact you about expiring
        # certificates, and issues related to your account.
        email: <YOUR.EMAIL@ADDRESS>
        # ACME server URL for Let’s Encrypt’s staging environment.
        # The staging environment will not issue trusted certificates but is
        # used to ensure that the verification process is working properly
        # before moving to production
        server: https://acme-staging-v02.api.letsencrypt.org/directory
        privateKeySecretRef:
        # Secret resource used to store the account's private key.
        name: example-issuer-account-key
        # Enable the HTTP-01 challenge provider
        # you prove ownership of a domain by ensuring that a particular
        # file is present at the domain
        http01: {}
    EOF
    ```

3. Bereitstellen der App

    Erstellen Sie eine Eingangsressource, um die Anwendung `guestbook` mithilfe von Application Gateway mit dem LetsEncrypt-Zertifikat verfügbar zu machen.

    Stellen Sie sicher, dass Application Gateway über eine öffentliche Front-End-IP-Konfiguration mit einem DNS-Namen verfügt (verwenden Sie entweder die Standarddomäne `azure.com`, oder stellen Sie einen `Azure DNS Zone`-Dienst bereit, und weisen Sie Ihre benutzerdefinierte Domäne zu).
    Beachten Sie die Anmerkung `certmanager.k8s.io/cluster-issuer: letsencrypt-staging`, mit der cert-manager angewiesen wird, die markierte Eingangsressource zu verarbeiten.

    > [!IMPORTANT] 
    > Aktualisieren Sie `<PLACEHOLDERS.COM>` in der YAML-Datei unten mit Ihrer eigenen Domäne (oder der Application Gateway-Domäne, z. B. „kh-aks-ingress.westeurope.cloudapp.azure.com“).

    ```bash
    kubectl apply -f - <<EOF
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
    name: guestbook-letsencrypt-staging
    annotations:
        kubernetes.io/ingress.class: azure/application-gateway
        certmanager.k8s.io/cluster-issuer: letsencrypt-staging
    spec:
    tls:
    - hosts:
        - <PLACEHOLDERS.COM>
        secretName: guestbook-secret-name
    rules:
    - host: <PLACEHOLDERS.COM>
        http:
        paths:
        - backend:
            serviceName: frontend
            servicePort: 80
    EOF
    ```

    Nach wenigen Sekunden können Sie über die HTTPS-URL von Application Gateway unter Verwendung des automatisch ausgestellten **Stagingzertifikats** `Lets Encrypt` auf den `guestbook`-Dienst zugreifen.
    Im Browser werden Sie möglicherweise vor einer ungültigen Zertifizierungsstelle gewarnt. Das Stagingzertifikat wird von `CN=Fake LE Intermediate X1` ausgestellt. Dies weist darauf hin, dass das System wie erwartet funktioniert hat und das Produktionszertifikat verwendet werden kann.

4. Produktionszertifikat

    Nachdem das Stagingzertifikat erfolgreich eingerichtet wurde, können Sie zu einem ACME-Produktionsserver wechseln:
    1. Ersetzen Sie die Staginganmerkung in der Eingangsressource durch Folgendes: `certmanager.k8s.io/cluster-issuer: letsencrypt-prod`
    1. Löschen Sie die vorhandene `ClusterIssuer`-Stagingressource, die Sie im vorherigen Schritt erstellt haben, und erstellen Sie eine neue, indem Sie den ACME-Server aus der YAML-Datei oben für die ClusterIssuer-Ressource durch `https://acme-v02.api.letsencrypt.org/directory` ersetzen.

5. Ablauf und Verlängerung eines Zertifikats

    Vor dem Ablauf des `Lets Encrypt`-Zertifikats wird es mit `cert-manager` im Kubernetes-Geheimnisspeicher automatisch aktualisiert. Dabei wird das in den Eingangsressourcen referenzierte aktualisierte Geheimnis in Application Gateway Ingress Controller angewandt, da es zum Konfigurieren von Application Gateway verwendet wird.

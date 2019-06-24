---
title: Best Practices für Entwickler – Podsicherheit in Azure Kubernetes Service (AKS)
description: Hier finden Sie bewährte Methode für Entwickler zum Absichern von Pods in Azure Kubernetes Service (AKS).
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: zarhoads
ms.openlocfilehash: f9d49d143b31b0b9e73d8a147605935cd88d412b
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2019
ms.locfileid: "65073969"
---
# <a name="best-practices-for-pod-security-in-azure-kubernetes-service-aks"></a>Best Practices für Podsicherheit in Azure Kubernetes Service (AKS)

Beim Entwickeln und Ausführen von Anwendungen in Azure Kubernetes Service (AKS) ist die Sicherheit Ihrer Pods ein wichtiger Aspekt. Ihre Anwendungen sollten auf das Prinzip der geringsten Anzahl von erforderlichen Berechtigungen ausgelegt sein. Die Sicherheit privater Daten ist für Kunden oberste Priorität. Sie möchten nicht zulassen, dass Anmeldeinformationen wie Datenbankverbindungszeichenfolgen, Schlüssel oder Geheimnisse und Zertifikate offengelegt werden, die Angreifer für bösartige Zwecke nutzen könnten. Fügen Sie sie nicht Ihrem Code hinzu, und betten Sie sie nicht in Containerimages ein. Dieser Ansatz würde ein Risiko der Offenlegung darstellen und die Möglichkeit einschränken, diese Anmeldeinformationen wechseln, wenn die Containerimages neu erstellt werden müssen.

Dieser Best Practices-Artikel konzentriert sich darauf, wie Sie Pods in AKS absichern. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Verwenden des Podsicherheitskontexts, um den Zugriff auf Prozesse und Dienste oder die Eskalation von Berechtigungen einzuschränken
> * Authentifizieren bei anderen Azure-Ressourcen unter Verwendung von verwalteten Podidentitäten
> * Anfordern und Abrufen von Anmeldeinformationen aus einem digitalen Tresor wie Azure Key Vault

Weitere Informationen finden Sie in den Best Practices für [Clustersicherheit][best-practices-cluster-security] und [Containerimageverwaltung][best-practices-container-image-management].

## <a name="secure-pod-access-to-resources"></a>Absichern des Podzugriffs auf Ressourcen

**Best Practices-Anleitung**: Für die Ausführung als ein anderer Benutzer oder eine andere Gruppe und um den Zugriff auf die zugrunde liegenden Knotenprozesse und -dienste zu beschränken, definieren Sie Einstellungen für den Podsicherheitskontext. Weisen Sie die geringste Anzahl von erforderlichen Berechtigungen zu.

Damit Ihre Anwendungen korrekt ausgeführt werden, sollten Pods als definierter Benutzer oder definierte Gruppe und nicht als *root* ausgeführt werden. Mit dem `securityContext` für einen Pod oder Container können Sie Einstellungen wie *runAsUser* oder *fsGroup* definieren, um die entsprechenden Berechtigungen zu übernehmen. Vergeben Sie nur die erforderlichen Benutzer- oder Gruppenberechtigungen und verwenden Sie den Sicherheitskontext nicht als Mittel, um zusätzliche Berechtigungen zu erhalten. *RunAsUser*, die Berechtigungsausweitung und andere Linux-Funktionseinstellungen sind nur auf Linux-Knoten und -Pods verfügbar.

Wenn Sie als Nicht-Root-Benutzer ausgeführt werden, können sich Container nicht an die privilegierten Ports unter 1024 binden. In diesem Szenario können Kubernetes-Dienste verwendet werden, um die Tatsache zu verschleiern, dass eine App auf einem bestimmten Port läuft.

Ein Podsicherheitskontext kann auch zusätzliche Funktionen oder Berechtigungen für den Zugriff auf Prozesse und Dienste definieren. Die folgenden allgemeinen Sicherheitsskontextdefinitionen können festgelegt werden:

* **allowPrivilegeEscalation** definiert, ob der Pod *root*-Berechtigungen übernehmen kann. Gestalten Sie Ihre Anwendungen so, dass diese Einstellung immer auf *false* festgelegt ist.
* Mithilfe von **Linux-Funktionen** kann der Pod auf zugrunde liegende Knotenprozesse zugreifen. Gehen Sie mit der Zuweisung dieser Funktionen umsichtig um. Weisen Sie die geringste Anzahl von benötigten Berechtigungen zu. Weitere Informationen finden Sie in den [Linux-Funktionen][linux-capabilities].
* Mit dem Linux-Kernelsicherheitsmodul **SELinux-Bezeichnungen** können Sie Zugriffsrichtlinien für Dienste, Prozesse und den Dateisystemzugriff definieren. Auch hier gilt: Weisen Sie die geringste Anzahl von benötigten Berechtigungen zu. Weitere Informationen finden Sie unter [SELinux-Optionen in Kubernetes][selinux-labels].

Das folgende Beispielpod-YAML-Manifest legt die Sicherheitskontexteinstellungen folgendermaßen fest:

* Der Pod wird als Benutzer-ID *1000* und Teil der Gruppen-ID *2000* ausgeführt.
* Es können keine Berechtigungen für die Verwendung von `root` eskaliert werden.
* Erlaubt Linux-Funktionen den Zugriff auf Netzwerkschnittstellen und die Echtzeituhr (Hardwareuhr) des Hosts.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: security-context-demo
spec:
  containers:
    - name: security-context-demo
      image: nginx:1.15.5
    securityContext:
      runAsUser: 1000
      fsGroup: 2000
      allowPrivilegeEscalation: false
      capabilities:
        add: ["NET_ADMIN", "SYS_TIME"]
```

Arbeiten Sie mit Ihrem Clusteroperator zusammen, um zu ermitteln, welche Sicherheitskontexteinstellungen Sie benötigen. Versuchen Sie, Ihre Anwendungen so zu gestalten, dass die vom Pod benötigten zusätzlichen Berechtigungen und Zugriffsrechte minimiert werden. Es gibt zusätzliche Sicherheitsfeatures, um den Zugriff mit AppArmor und seccomp (sicheres Computing) zu beschränken, die von Clusteroperatoren implementiert werden können. Weitere Informationen finden Sie unter [Sicherer Containerzugriff auf Ressourcen][apparmor-seccomp].

## <a name="limit-credential-exposure"></a>Beschränken der Offenlegung von Anmeldeinformationen

**Best Practices-Anleitung**: Definieren Sie keine Anmeldeinformationen in Ihrem Anwendungscode. Verwenden Sie verwaltete Identitäten für Azure-Ressourcen, damit Ihr Pod Zugriff auf andere Ressourcen anfordern kann. Außerdem sollte ein digitaler Tresor, wie z.B. Azure Key Vault, zum Speichern und Abrufen digitaler Schlüssel und Anmeldeinformationen verwendet werden. Pod-verwaltete Identitäten sind nur für die Verwendung mit Linux-Pods und -Containerimages vorgesehen.

Um das Risiko zu begrenzen, dass Anmeldeinformationen in Ihrem Anwendungscode offengelegt werden, vermeiden Sie die Verwendung von festen oder gemeinsamen Anmeldeinformationen. Anmeldeinformationen oder Schlüssel sollten nicht direkt in Ihrem Code enthalten sein. Wenn diese Anmeldeinformationen offengelegt werden, muss die Anwendung aktualisiert und neu bereitgestellt werden. Ein besserer Ansatz ist es, den Pods eine eigene Identität und eine Möglichkeit zu geben, sich selbst zu authentifizieren oder automatisch Anmeldeinformationen aus einem digitalen Tresor abzurufen.

Mit den folgenden [ zugehörigen AKS-Open-Source-Projekten][aks-associated-projects] können Sie Pods automatisch authentifizieren oder Anmeldeinformationen und Schlüssel von einem digitalen Tresor anfordern:

* Verwaltete Identitäten für Azure-Ressourcen und
* Azure Key Vault-FlexVol-Treiber

Zugehörige AKS-Open-Source-Projekte werden vom technischen Support von Azure nicht unterstützt. Sie werden zum Sammeln von Feedback und Fehlern mithilfe unserer Community bereitgestellt. Diese Projekte werden nicht für die Produktion empfohlen.

### <a name="use-pod-managed-identities"></a>Verwenden verwalteter Podidentitäten

Eine verwaltete Identität für Azure-Ressourcen ermöglicht es einem Pod, sich bei jedem Dienst in Azure, der ihn unterstützt, zu authentifizieren, wie z. B. Storage, SQL. Dem Pod wird eine Azure-Identität zugewiesen, mit der er sich bei Azure Active Directory authentifizieren und ein digitales Token abrufen kann. Dieses digitale Token kann anderen Azure-Diensten vorgelegt werden, die überprüfen, ob der Pod berechtigt ist, auf den Dienst zuzugreifen und die erforderlichen Aktionen auszuführen. Dieser Ansatz bedeutet z. B., dass keine Geheimnisse für Datenbankverbindungszeichenketten erforderlich sind. Der vereinfachte Workflow für eine verwaltete Podidentität ist im folgenden Diagramm dargestellt:

![Vereinfachter Workflow für verwaltete Podidentität in Azure](media/developer-best-practices-pod-security/basic-pod-identity.png)

Bei einer verwalteten Identität muss Ihr Anwendungscode keine Anmeldeinformationen für den Zugriff auf einen Dienst, wie beispielsweise Azure Storage, enthalten. Da sich jeder Pod mit seiner eigenen Identität authentifiziert, können Sie den Zugriff überwachen und überprüfen. Wenn sich Ihre Anwendung mit anderen Azure-Diensten verbindet, verwenden Sie verwaltete Identitäten, um die Wiederverwendung von Berechtigungen und das Risiko einer Offenlegung zu begrenzen.

Weitere Informationen zu Podidentitäten finden Sie unter [Konfigurieren eines AKS-Clusters zur Verwendung von verwalteten Podidentitäten und mit Ihren Anwendungen][aad-pod-identity].

### <a name="use-azure-key-vault-with-flexvol"></a>Verwenden von Azure Key Vault mit FlexVol

Verwaltete Podidentitäten können hervorragend zum Authentifizieren bei unterstützten Azure-Diensten verwendet werden. Bei Ihren eigenen Diensten oder Anwendungen ohne verwaltete Identitäten für Azure-Ressourcen authentifizieren Sie sich weiterhin mit Anmeldeinformationen oder Schlüsseln. Ein digitaler Tresor kann verwendet werden, um diese Anmeldeinformationen zu speichern.

Wenn Anwendungen Anmeldeinformationen benötigen, kommunizieren sie mit dem digitalen Tresor, rufen die neuesten Anmeldeinformationen ab und stellen dann die Verbindung mit dem gewünschten Dienst her. Azure Key Vault kann dieser digitale Tresor sein. Der vereinfachte Workflow zum Abrufen von Anmeldeinformationen aus Azure Key Vault unter Verwendung von verwalteten Podidentitäten ist im folgenden Diagramm dargestellt:

![Vereinfachter Workflow zum Abrufen von Anmeldeinformationen aus dem Schlüsseltresor unter Verwendung einer verwalteten Podidentität](media/developer-best-practices-pod-security/basic-key-vault-flexvol.png)

Mit Key Vault werden Geheimnisse wie Anmeldeinformationen, Speicherkontenschlüssel oder Zertifikate gespeichert und regelmäßig rotiert. Sie können Azure Key Vault mit einem AKS-Cluster unter Verwendung eines FlexVolume integrieren. Mit dem FlexVolume-Treiber kann der AKS-Cluster nativ Anmeldeinformationen aus dem Schlüsseltresor abrufen und diese sicher nur dem anfordernden Pod zur Verfügung stellen. Arbeiten Sie mit Ihrem Clusteroperator zusammen, um den FlexVol-Treiber für den Schlüsseltresor auf den AKS-Knoten bereitzustellen. Sie können eine verwaltete Podidentität verwenden, um den Zugriff auf den Schlüsseltresor anzufordern und die erforderlichen Anmeldeinformationen über den FlexVolume-Treiber abzurufen.

Der Azure Key Vault mit FlexVol ist für die Verwendung mit Anwendungen und Diensten vorgesehen, die auf Linux-Pods und -Knoten ausgeführt werden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde erläutert, wie Pods gesichert werden. Wenn Sie einige dieser Best Practices implementieren möchten, lesen Sie folgende Artikel:

* [Verwenden von verwalteten Identitäten für Azure-Ressourcen mit AKS][aad-pod-identity]
* [Integrieren von Azure Key Vault in AKS][aks-keyvault-flexvol]

<!-- EXTERNAL LINKS -->
[aad-pod-identity]: https://github.com/Azure/aad-pod-identity#demo-pod
[aks-keyvault-flexvol]: https://github.com/Azure/kubernetes-keyvault-flexvol
[linux-capabilities]: http://man7.org/linux/man-pages/man7/capabilities.7.html
[selinux-labels]: https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.12/#selinuxoptions-v1-core
[aks-associated-projects]: https://github.com/Azure/AKS/blob/master/previews.md#associated-projects

<!-- INTERNAL LINKS -->
[best-practices-cluster-security]: operator-best-practices-cluster-security.md
[best-practices-container-image-management]: operator-best-practices-container-image-management.md
[aks-pod-identities]: operator-best-practices-identity.md#use-pod-identities
[apparmor-seccomp]: operator-best-practices-cluster-security.md#secure-container-access-to-resources

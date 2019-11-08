---
title: Einrichten der Azure Red Hat OpenShift-Entwicklungsumgebung | Microsoft-Dokumentation
description: Hier finden Sie die Voraussetzungen für die Arbeit mit Microsoft Azure Red Hat OpenShift.
services: openshift
keywords: Red Hat OpenShift, Einrichtung, einrichten
author: jimzim
ms.author: jzim
ms.date: 11/04/2019
ms.topic: conceptual
ms.service: container-service
manager: jeconnoc
ms.openlocfilehash: fa09ed90258a62d37dafeea5f4760e1fabdc210b
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73581597"
---
# <a name="set-up-your-azure-red-hat-openshift-dev-environment"></a>Einrichten Ihrer Azure Red Hat OpenShift-Entwicklungsumgebung

Für die Erstellung und Ausführung von Microsoft Azure Red Hat OpenShift-Anwendungen müssen Sie folgende Vorbereitungen treffen:

* Installieren Sie Version 2.0.65 (oder höher) der Azure CLI (oder verwenden Sie Azure Cloud Shell).
* Registrieren Sie sich für das `AROGA`-Feature und die zugehörigen Ressourcenanbieter.
* Erstellen Sie einen Azure Active Directory-Mandanten (Azure AD).
* Erstellen Sie ein Azure AD-Anwendungsobjekt.
* Erstellen Sie einen Azure AD-Benutzer.

Die folgenden Anweisungen führen Sie durch all diese Voraussetzungen.

## <a name="install-the-azure-cli"></a>Installieren der Azure CLI

Azure Red Hat OpenShift erfordert Version 2.0.65 oder höher der Azure CLI. Wenn Sie die Azure CLI bereits installiert haben, können Sie überprüfen, um welche Version es sich handelt, indem Sie folgenden Befehl ausführen:

```bash
az --version
```

Die erste Zeile der Ausgabe zeigt die Version der Befehlszeilenschnittstelle, z.B. `azure-cli (2.0.65)`.

Wenn Sie eine Neuinstallation oder ein Upgrade benötigen, sehen Sie sich die Anweisungen zum [Installieren der Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) an.

Alternativ können Sie auch die [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) verwenden. Wenn Sie die Azure Cloud Shell verwenden, stellen Sie sicher, dass die **Bash**-Umgebung ausgewählt ist, wenn Sie der Tutorialreihe [Erstellen und Verwalten eines Azure Red Hat OpenShift-Clusters](tutorial-create-cluster.md) folgen möchten.

## <a name="register-providers-and-features"></a>Registrieren von Anbietern und Features

Das `Microsoft.ContainerService AROGA`-Feature sowie die Anbieter `Microsoft.Solutions`, `Microsoft.Compute`, `Microsoft.Storage`, `Microsoft.KeyVault` und `Microsoft.Network` müssen manuell in Ihrem Abonnement registriert werden, bevor Sie Ihren ersten Azure Red Hat OpenShift-Cluster bereitstellen können.

Um diese Anbieter und Features manuell zu registrieren, führen Sie die folgenden Anweisungen in einer Bash-Shell aus, wenn Sie die CLI installiert haben. Alternativ dazu können Sie auch eine Azure Cloud Shell-Sitzung (Bash) im Azure-Portal verwenden:

1. Wenn Sie über mehrere Azure-Abonnements verfügen, geben Sie die gewünschte Abonnement-ID an:

    ```bash
    az account set --subscription <SUBSCRIPTION ID>
    ```

1. Registrieren des Microsoft.ContainerService-AROGA-Features:

    ```bash
    az feature register --namespace Microsoft.ContainerService -n AROGA
    ```

1. Registrieren Sie den Microsoft.Storage-Anbieter:

    ```bash
    az provider register -n Microsoft.Storage --wait
    ```
    
1. Registrieren Sie den Microsoft.Compute-Anbieter:

    ```bash
    az provider register -n Microsoft.Compute --wait
    ```

1. Registrieren Sie den Microsoft.Solutions-Anbieter:

    ```bash
    az provider register -n Microsoft.Solutions --wait
    ```

1. Registrieren Sie den Microsoft.Network-Anbieter:

    ```bash
    az provider register -n Microsoft.Network --wait
    ```

1. Registrieren Sie den Microsoft.KeyVault-Anbieter:

    ```bash
    az provider register -n Microsoft.KeyVault --wait
    ```

1. Aktualisieren Sie die Registrierung des Microsoft.ContainerService-Ressourcenanbieters:

    ```bash
    az provider register -n Microsoft.ContainerService --wait
    ```

## <a name="create-an-azure-active-directory-azure-ad-tenant"></a>Erstellen eines Azure Active Directory-Mandanten (Azure AD)

Der Azure Red Hat OpenShift-Dienst erfordert einen zugeordneten Azure AD-Mandanten, der Ihre Organisation und deren Beziehung zu Microsoft repräsentiert. Ihr Azure AD-Mandant ermöglicht es Ihnen, Apps zu registrieren, zu erstellen und zu verwalten sowie weitere Azure-Dienste zu nutzen.

Wenn Sie nicht über ein Azure AD-Verzeichnis verfügen, das Sie als Mandanten für Ihren Azure Red Hat OpenShift-Cluster verwenden können, oder einen Mandanten zu Testzwecken erstellen möchten, befolgen Sie die Anweisungen unter [Erstellen eines Azure AD-Mandanten für Ihren Azure Red Hat OpenShift-Cluster](howto-create-tenant.md), bevor Sie mit diesem Leitfaden fortfahren.

## <a name="create-an-azure-ad-user-security-group-and-application-object"></a>Erstellen eines Azure AD-Benutzers, einer Sicherheitsgruppe und eines Anwendungsobjekts

Azure Red Hat OpenShift erfordert Berechtigungen zum Ausführen von Aufgaben in Ihrem Cluster, z.B. zum Konfigurieren von Speicher. Diese Berechtigungen werden durch einen [Dienstprinzipal](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals#service-principal-object) dargestellt. Sie sollten auch einen neuen Active Directory-Benutzer erstellen, um Apps zu testen, die in Ihrem Azure Red Hat OpenShift-Cluster ausgeführt werden sollen.

Befolgen Sie die Anweisungen unter [Erstellen eines Azure AD-App-Objekts und -Benutzers](howto-aad-app-configuration.md), um einen Dienstprinzipal zu erstellen, ein Clientgeheimnis und eine Rückruf-URL für die Authentifizierung für Ihre App zu generieren und eine neue Azure AD-Sicherheitsgruppe und einen neuen Benutzer für den Zugriff auf den Cluster zu erstellen.

## <a name="next-steps"></a>Nächste Schritte

Sie können Azure Red Hat OpenShift jetzt verwenden!

Lesen Sie dieses Tutorial:
> [!div class="nextstepaction"]
> [Erstellen eines Azure Red Hat OpenShift-Clusters](tutorial-create-cluster.md)

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli

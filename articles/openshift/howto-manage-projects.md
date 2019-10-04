---
title: Verwalten von Ressourcen in Azure Red Hat OpenShift | Microsoft-Dokumentation
description: Verwalten von Projekten, Vorlagen, Imagestreams in einem Azure Red Hat OpenShift-Cluster
services: openshift
keywords: Eigenständige Bereitstellung von Anforderungen für Red Hat OpenShift-Projekte
author: mjudeikis
ms.author: b-majude
ms.date: 07/19/2019
ms.topic: conceptual
ms.service: container-service
ms.openlocfilehash: 5028ce3c71538e67b50a15abb6076871d5af7050
ms.sourcegitcommit: a6888fba33fc20cc6a850e436f8f1d300d03771f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69559148"
---
# <a name="manage-projects-templates-image-streams-in-an-azure-red-hat-openshift-cluster"></a>Verwalten von Projekten, Vorlagen, Imagestreams in einem Azure Red Hat OpenShift-Cluster 

In OpenShift Container Platform werden Projekte zum Gruppieren und Isolieren verwandter Objekte verwendet. Als Administrator können Sie Entwicklern den Zugriff auf bestimmte Projekte gewähren, das Erstellen eigener Projekte gestatten und Administratorrechte für einzelne Projekte erteilen.

## <a name="self-provisioning-projects"></a>Eigenständige Bereitstellung von Projekten

Sie können Entwicklern das Erstellen eigener Projekte ermöglichen. Ein API-Endpunkt ist für die Bereitstellung eines Projekts gemäß einer Vorlage namens „project-request“ verantwortlich. Die Webkonsole und der Befehl `oc new-project` verwenden diesen Endpunkt, wenn ein Entwickler ein neues Projekt erstellt.

Beim Übermitteln einer Projektanforderung ersetzt die API die folgenden Parameter in der Vorlage:

| Parameter               | BESCHREIBUNG                                    |
| ----------------------- | ---------------------------------------------- |
| PROJECT_NAME            | Der Name des Projekts. Erforderlich.             |
| PROJECT_DISPLAYNAME     | Der Anzeigename des Projekts. Kann leer sein. |
| PROJECT_DESCRIPTION     | Die Beschreibung des Projekts. Kann leer sein.  |
| PROJECT_ADMIN_USER      | Der Benutzername des verwaltenden Benutzers.       |
| PROJECT_REQUESTING_USER | Der Benutzername des anfordernden Benutzers.           |

Der Zugriff auf die API wird Entwicklern mit der Clusterrollenbindung für die eigenständige Bereitstellung erteilt. Diese Funktion ist standardmäßig für alle authentifizierten Entwickler verfügbar.

## <a name="modify-the-template-for-a-new-project"></a>Ändern der Vorlage für ein neues Projekt 

1. Melden Sie sich als Benutzer mit `customer-admin`-Berechtigungen an.

2. Bearbeiten Sie die Standardvorlage „project-request“.

   ```
   oc edit template project-request -n openshift
   ```

3. Fügen Sie die folgende Anmerkung hinzu, um die Standardprojektvorlage aus dem Updateprozess von Azure Red Hat OpenShift (ARO) zu entfernen: `openshift.io/reconcile-protect: "true"`

   ```
   ...
   metadata:
     annotations:
       openshift.io/reconcile-protect: "true"
   ...
   ```

   Die Vorlage „project-request“ wird nicht durch den ARO-Updateprozess aktualisiert. Dadurch können Kunden die Vorlage anpassen und diese Anpassungen beibehalten, wenn der Cluster aktualisiert wird.

## <a name="disable-the-self-provisioning-role"></a>Deaktivieren der Rolle für eigenständige Bereitstellung

Sie können verhindern, dass eine authentifizierte Benutzergruppe neue Projekte eigenständig bereitstellt.

1. Melden Sie sich als Benutzer mit `customer-admin`-Berechtigungen an.

2. Bearbeiten Sie die Clusterrollenbindung für eigenständige Bereitstellung.

   ```
   oc edit clusterrolebinding self-provisioners
   ```

3. Fügen Sie die folgende Anmerkung hinzu, um die Rolle aus dem ARO-Updateprozess zu entfernen: `openshift.io/reconcile-protect: "true"`

   ```
   ...
   metadata:
     annotations:
       openshift.io/reconcile-protect: "true"
   ...
   ```

4. Ändern Sie die Clusterrollenbindung, um das Erstellen von Projekten durch `system:authenticated:oauth` zu verhindern:

   ```
   apiVersion: authorization.openshift.io/v1
   groupNames:
   - osa-customer-admins
   kind: ClusterRoleBinding
   metadata:
     annotations:
       openshift.io/reconcile-protect: "true"
     labels:
       azure.openshift.io/owned-by-sync-pod: "true"
     name: self-provisioners
   roleRef:
     name: self-provisioner
   subjects:
   - kind: SystemGroup
     name: osa-customer-admins
   ```

## <a name="manage-default-templates-and-imagestreams"></a>Verwalten von Standardvorlagen und -imagestreams

In Azure Red Hat OpenShift können Sie Updates für alle Standardvorlagen und -imagestreams im Namespace `openshift` deaktivieren.
Zum Deaktivieren von Updates für ALLE `Templates` und `ImageStreams` im Namespace `openshift` führen Sie die folgenden Schritte aus:

1. Melden Sie sich als Benutzer mit `customer-admin`-Berechtigungen an.

2. Bearbeiten Sie den Namespace `openshift`:

   ```
   oc edit namespace openshift
   ```

3. Fügen Sie die folgende Anmerkung hinzu, um den Namespace `openshift` aus dem ARO-Updateprozess zu entfernen: `openshift.io/reconcile-protect: "true"`

   ```
   ...
   metadata:
     annotations:
       openshift.io/reconcile-protect: "true"
   ...
   ```

   Jedes einzelne Objekt im Namespace `openshift` kann durch Hinzufügen der Anmerkung `openshift.io/reconcile-protect: "true"` aus dem Updateprozess entfernt werden.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie dieses Tutorial:
> [!div class="nextstepaction"]
> [Erstellen eines Azure Red Hat OpenShift-Clusters](tutorial-create-cluster.md)

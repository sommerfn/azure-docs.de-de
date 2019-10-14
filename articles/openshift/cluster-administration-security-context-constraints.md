---
title: Verwalten von Einschränkungen des Sicherheitskontexts in Azure Red Hat OpenShift | Microsoft-Dokumentation
description: Einschränkungen des Sicherheitskontexts für Administratoren von Azure Red Hat OpenShift-Clustern
services: container-service
author: troy0820
ms.author: jzim
ms.service: container-service
ms.topic: article
ms.date: 09/25/2019
ms.openlocfilehash: f98f55dca8b3dbbfbe03cb8c79691cedb63335a0
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72168991"
---
# <a name="manage-security-context-constraints-in-azure-red-hat-openshift"></a>Verwalten von Einschränkungen des Sicherheitskontexts in Azure Red Hat OpenShift 

Einschränkungen des Sicherheitskontexts (SCCs) ermöglichen Clusteradministratoren, die Berechtigungen für Pods zu steuern. Weitere Informationen zu diesem API-Typ finden Sie in der [Dokumentation der Architektur zu Einschränkungen beim Sicherheitskontext (Security Context Constraints, SCCs)](https://docs.openshift.com/container-platform/3.11/architecture/additional_concepts/authorization.html). Sie können Einschränkungen beim Sicherheitskontext in Ihrer Instanz mithilfe der Befehlszeilenschnittstelle wie normale API-Objekte verwalten.

## <a name="list-security-context-constraints"></a>Auflisten von Einschränkungen beim Sicherheitskontext

Verwenden Sie zum Erstellen einer aktuellen Liste der Einschränkungen beim Sicherheitskontext diesen Befehl: 

```bash
$ oc get scc

NAME               PRIV      CAPS      SELINUX     RUNASUSER          FSGROUP     SUPGROUP    PRIORITY   READONLYROOTFS   VOLUMES
anyuid             false     []        MustRunAs   RunAsAny           RunAsAny    RunAsAny    10         false            [configMap downwardAPI emptyDir persistentVolumeClaim secret]
hostaccess         false     []        MustRunAs   MustRunAsRange     MustRunAs   RunAsAny    <none>     false            [configMap downwardAPI emptyDir hostPath persistentVolumeClaim secret]
hostmount-anyuid   false     []        MustRunAs   RunAsAny           RunAsAny    RunAsAny    <none>     false            [configMap downwardAPI emptyDir hostPath nfs persistentVolumeClaim secret]
hostnetwork        false     []        MustRunAs   MustRunAsRange     MustRunAs   MustRunAs   <none>     false            [configMap downwardAPI emptyDir persistentVolumeClaim secret]
nonroot            false     []        MustRunAs   MustRunAsNonRoot   RunAsAny    RunAsAny    <none>     false            [configMap downwardAPI emptyDir persistentVolumeClaim secret]
privileged         true      [*]       RunAsAny    RunAsAny           RunAsAny    RunAsAny    <none>     false            [*]
restricted         false     []        MustRunAs   MustRunAsRange     MustRunAs   RunAsAny    <none>     false            [configMap downwardAPI emptyDir persistentVolumeClaim secret]
```

## <a name="examine-an-object-for-security-context-constraints"></a>Untersuchen eines Einschränkungsobjekts für den Sicherheitskontext

Verwenden Sie zum Untersuchen einer bestimmten Einschränkung für den Sicherheitskontext `oc get`, `oc describe` oder `oc edit`.  Um beispielsweise die **restricted** Einschränkung für den Sicherheitskontext zu untersuchen, verwenden Sie diesen Befehl:
```bash
$ oc describe scc restricted
Name:                   restricted
Priority:               <none>
Access:
  Users:                <none>
  Groups:               system:authenticated
Settings:
  Allow Privileged:         false
  Default Add Capabilities:     <none>
  Required Drop Capabilities:       KILL,MKNOD,SYS_CHROOT,SETUID,SETGID
  Allowed Capabilities:         <none>
  Allowed Seccomp Profiles:     <none>
  Allowed Volume Types:         configMap,downwardAPI,emptyDir,persistentVolumeClaim,projected,secret
  Allow Host Network:           false
  Allow Host Ports:         false
  Allow Host PID:           false
  Allow Host IPC:           false
  Read Only Root Filesystem:        false
  Run As User Strategy: MustRunAsRange
    UID:                <none>
    UID Range Min:          <none>
    UID Range Max:          <none>
  SELinux Context Strategy: MustRunAs
    User:               <none>
    Role:               <none>
    Type:               <none>
    Level:              <none>
  FSGroup Strategy: MustRunAs
    Ranges:             <none>
  Supplemental Groups Strategy: RunAsAny
    Ranges:             <none>
```
## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Erstellen eines Azure Red Hat OpenShift-Clusters](tutorial-create-cluster.md) 

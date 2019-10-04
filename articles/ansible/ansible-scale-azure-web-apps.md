---
title: 'Tutorial: Skalieren von Apps in Azure App Service mit Ansible | Microsoft-Dokumentation'
description: Es wird beschrieben, wie Sie eine App in Azure App Service zentral hochskalieren.
keywords: Ansible, Azure, DevOps, Bash, Playbook, Azure App Service, Web-App, skalieren, Java
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: d63708cd87afa426f2712da6d0fcb11c84590798
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2019
ms.locfileid: "65230944"
---
# <a name="tutorial-scale-apps-in-azure-app-service-using-ansible"></a>Tutorial: Skalieren von Apps in Azure App Service mit Ansible

[!INCLUDE [ansible-27-note.md](../../includes/ansible-27-note.md)]

[!INCLUDE [open-source-devops-intro-app-service.md](../../includes/open-source-devops-intro-app-service.md)]

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Abrufen der Angaben eines vorhandenen App Service-Plans
> * Zentrales Hochskalieren des App Service-Plans auf S2 mit drei Workern

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]
- **Azure App Service-App**: [Konfigurieren Sie eine App in Azure App Service mit Ansible](ansible-create-configure-azure-web-apps.md), falls Sie nicht über eine Azure App Service-App verfügen.

## <a name="scale-up-an-app"></a>Zentrales Hochskalieren einer App

Es gibt zwei Workflows für die Skalierung: *zentrales Hochskalieren* und *horizontales Hochskalieren*.

**Zentrales Hochskalieren**: Beim zentralen Hochskalieren werden zusätzliche Ressourcen beschafft. Beispiele für diese Ressourcen sind CPU, Arbeitsspeicher, Speicherplatz auf dem Datenträger, virtuelle Computer usw. Sie führen für eine App das zentrale Hochskalieren durch, indem Sie den Tarif des App Service-Plans ändern, zu dem die App gehört. 
**Horizontales Hochskalieren:** Beim horizontalen Hochskalieren wird die Anzahl von VM-Instanzen erhöht, über die Ihre App ausgeführt wird. Je nach Tarif Ihres App Service-Plans können Sie auf bis zu 20 Instanzen horizontal hochskalieren. Die [automatische Skalierung](/azure/azure-monitor/platform/autoscale-get-started) ermöglicht Ihnen das automatische Skalieren der Instanzanzahl basierend auf vordefinierten Regeln und Zeitplänen.

Mit dem Playbookcode in diesem Abschnitt wird der folgende Vorgang definiert:

* Abrufen der Angaben eines vorhandenen App Service-Plans
* Aktualisieren des App Service-Plans auf S2 mit drei Workern

Speichern Sie das folgende Playbook als `webapp_scaleup.yml`:

```yml
- hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    plan_name: myAppServicePlan
    location: eastus

  tasks:
  - name: Get facts of existing App service plan
    azure_rm_appserviceplan_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
    register: facts

  - debug: 
      var: facts.appserviceplans[0].sku

  - name: Scale up the App service plan
    azure_rm_appserviceplan:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
      is_linux: true
      sku: S2
      number_of_workers: 3
      
  - name: Get facts
    azure_rm_appserviceplan_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
    register: facts

  - debug: 
      var: facts.appserviceplans[0].sku
```

Führen Sie das Playbook mit dem Befehl `ansible-playbook` aus:

```bash
ansible-playbook webapp_scaleup.yml
```

Nach dem Ausführen des Playbooks wird in etwa die folgende Ausgabe angezeigt:

```Output
PLAY [localhost] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Get facts of existing App service plan] 
 [WARNING]: Azure API profile latest does not define an entry for WebSiteManagementClient

ok: [localhost]

TASK [debug] 
ok: [localhost] => {
    "facts.appserviceplans[0].sku": {
        "capacity": 1,
        "family": "S",
        "name": "S1",
        "size": "S1",
        "tier": "Standard"
    }
}

TASK [Scale up the App service plan] 
changed: [localhost]

TASK [Get facts] 
ok: [localhost]

TASK [debug] 
ok: [localhost] => {
    "facts.appserviceplans[0].sku": {
        "capacity": 3,
        "family": "S",
        "name": "S2",
        "size": "S2",
        "tier": "Standard"
    }
}

PLAY RECAP 
localhost                  : ok=6    changed=1    unreachable=0    failed=0 
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"] 
> [Ansible unter Azure](/azure/ansible/)
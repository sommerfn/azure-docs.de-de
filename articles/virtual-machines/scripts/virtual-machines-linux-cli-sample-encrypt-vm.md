---
title: Azure CLI-Skriptbeispiel – Verschlüsseln einer Linux-VM
description: Azure CLI-Skriptbeispiel – Verschlüsseln einer Linux-VM
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/15/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 26d453adb4ded59de162fda0ae62322399fa4d9a
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74040274"
---
# <a name="encrypt-a-linux-virtual-machine-in-azure"></a>Verschlüsseln eines virtuellen Linux-Computers in Azure

Dieses Skript erstellt eine sichere Azure Key Vault-Lösung, Verschlüsselungsschlüssel, ein Azure Active Directory-Dienstprinzipal und einen virtuellen Linux-Computer (VM). Die VM wird anschließend mithilfe des Verschlüsselungsschlüssels aus Key Vault und Dienstprinzipal-Anmeldeinformationen verschlüsselt.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_vm.sh "Encrypt VM disks")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, Azure Key Vault, ein Dienstprinzipal, einen virtuellen Computer und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az keyvault create](https://docs.microsoft.com/cli/azure/keyvault) | Erstellt Azure Key Vault, um sichere Daten wie Verschlüsselungsschlüssel zu speichern. |
| [az keyvault key create](https://docs.microsoft.com/cli/azure/keyvault/key) | Erstellt einen Verschlüsselungsschlüssel in Key Vault. |
| [az ad sp create-for-rbac](https://docs.microsoft.com/cli/azure/ad/sp) | Erstellt ein Azure Active Directory-Dienstprinzipal für die sichere Authentifizierung und Steuerung des Zugriffs auf die Verschlüsselungsschlüssel. |
| [az keyvault set-policy](https://docs.microsoft.com/cli/azure/keyvault) | Legt Berechtigungen für Key Vault fest, um dem Dienstprinzipal Zugriff auf Verschlüsselungsschlüssel zu gewähren. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm) | Erstellt den virtuellen Computer und verbindet diesen mit der Netzwerkkarte, dem virtuellen Netzwerk, dem Subnetz und der NSG. Dieser Befehl legt außerdem das zu verwendende VM-Image und die Administratoranmeldeinformationen fest.  |
| [az vm encryption enable](https://docs.microsoft.com/cli/azure/vm/encryption) | Aktiviert die Verschlüsselung mithilfe der Dienstprinzipal-Anmeldeinformationen und Verschlüsselungsschlüssel auf einem virtuellen Computer. |
| [az vm encryption show](https://docs.microsoft.com/cli/azure/vm/encryption) | Zeigt den Status des Verschlüsselungsprozesses des virtuellen Computers an. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Zusätzliche VM-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Linux-VMs in Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

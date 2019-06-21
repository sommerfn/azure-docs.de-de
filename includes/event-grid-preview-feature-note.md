---
title: include file
description: include file
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 11/06/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: d32beb2d799a60cb9c5be061c39e4ec834da8dcf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66814754"
---
Dieses Feature befindet sich in der Vorschauphase. Sie müssen eine Vorschauerweiterung oder ein Vorschaumodul installieren, um es zu verwenden.

### <a name="install-extension-for-azure-cli"></a>Installieren der Erweiterung für Azure CLI

Für die Azure CLI benötigen Sie die [Event Grid-Erweiterung](/cli/azure/azure-cli-extensions-list).

In [CloudShell](/azure/cloud-shell/quickstart):

* Wenn Sie die Erweiterung bereits installiert haben, aktualisieren Sie sie mit `az extension update -n eventgrid`.
* Wenn Sie die Erweiterung noch nicht installiert haben, installieren Sie sie mit `az extension add -n eventgrid`.

Für eine lokale Installation:

1. [Installieren Sie die Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli). Vergewissern Sie sich mit `az --version`, dass Sie über die aktuelle Version verfügen.
1. Deinstallieren Sie frühere Versionen der Erweiterung `az extension remove -n eventgrid`.
1. Installieren Sie die Erweiterung `eventgrid` mit `az extension add -n eventgrid`.

### <a name="install-module-for-powershell"></a>Installieren des Moduls für PowerShell

Für PowerShell benötigen Sie das [AzureRM.EventGrid-Modul](https://www.powershellgallery.com/packages/AzureRM.EventGrid/0.4.1-preview).

In [CloudShell](/azure/cloud-shell/quickstart-powershell):

* Installieren Sie das Modul `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`.

Für eine lokale Installation:

1. Öffnen Sie die PowerShell-Konsole als Administrator.
1. Installieren Sie das Modul `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`.

Wenn der `-AllowPrerelease`-Parameter nicht verfügbar ist, führen Sie die folgenden Schritte aus:

1. Führen Sie `Install-Module PowerShellGet -Force` aus.
1. Führen Sie `Update-Module PowerShellGet` aus.
1. Schließen Sie die PowerShell-Konsole.
1. Starten Sie PowerShell als Administrator neu.
1. Installieren Sie das Modul `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`.

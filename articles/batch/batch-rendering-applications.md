---
title: Renderinganwendungen – Azure Batch
description: Vorinstallierte Batch Rendering-Anwendungen
services: batch
ms.service: batch
author: laurenhughes
ms.author: lahugh
ms.date: 09/19/2019
ms.topic: conceptual
ms.openlocfilehash: 30a365b36645dfe79f35b4bb889c0a06535a4c73
ms.sourcegitcommit: 7df70220062f1f09738f113f860fad7ab5736e88
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2019
ms.locfileid: "71212711"
---
# <a name="pre-installed-applications-on-rendering-vm-images"></a>Vorinstallierte Anwendungen in Rendering-VM-Images

Mit Azure Batch können beliebige Renderinganwendungen verwendet werden. In Azure Marketplace-VM-Images sind gängige Anwendungen jedoch bereits vorinstalliert.

Für vorinstallierte Renderinganwendungen ist ggf. Lizenzierung mit nutzungsbasierter Zahlung verfügbar. Bei der Erstellung eines Batch-Pools können die erforderlichen Anwendungen angegeben werden, und die Kosten des virtuellen Computers sowie der Anwendungen werden pro Minute abgerechnet. Anwendungspreise sind auf der [Seite mit den Preisen für Azure Batch](https://azure.microsoft.com/pricing/details/batch/#graphic-rendering) aufgeführt.

Einige Anwendungen unterstützen nur Windows, die meisten werden jedoch unter Windows und Linux unterstützt.

## <a name="applications-on-centos-7-rendering-images"></a>Anwendungen auf CentOS 7-Renderingimages

Die folgende Liste gilt für Renderingimages von CentOS 7.6, Version 1.1.6.

* Autodesk Maya I/O 2017 Update 5 (Cut 201708032230)
* Autodesk Maya I/O 2018 Update 2 (Cut 201711281015)
* Autodesk Maya I/O 2019 Update 1
* Autodesk Arnold for Maya 2017 (Arnold-Version 5.3.1.1) MtoA-3.2.1.1-2017
* Autodesk Arnold for Maya 2018 (Arnold-Version 5.3.1.1) MtoA-3.2.1.1-2018
* Autodesk Arnold for Maya 2019 (Arnold-Version 5.3.1.1) MtoA-3.2.1.1-2019
* Chaos Group V-Ray for Maya 2017 (Version 3.60.04)
* Chaos Group V-Ray for Maya 2018 (Version 3.60.04)
* Blender (2.68)
* Blender (2.8)

## <a name="applications-on-latest-windows-server-2016-rendering-images"></a>Anwendungen auf Windows Server 2016-Renderingimages

Die folgende Liste betrifft Renderingimages von Windows Server 2016, Version 1.3.8.

* Autodesk Maya I/O 2017 Update 5 (Version 17.4.5459)
* Autodesk Maya I/O 2018 Update 6 (Version 18.4.0.7622)
* Autodesk Maya I/O 2019
* Autodesk 3ds Max I/O 2018 Update 4 (Version 20.4.0.4254)
* Autodesk 3ds Max I/O 2019 Update 1 (Version 21.2.0.2219)
* Autodesk 3ds Max I/O 2020 Update 2
* Autodesk Arnold for Maya 2017 (Arnold-Version 5.3.0.2) MtoA-3.2.0.2-2017
* Autodesk Arnold for Maya 2018 (Arnold-Version 5.3.0.2) MtoA-3.2.0.2-2018
* Autodesk Arnold for Maya 2019 (Arnold-Version 5.3.0.2) MtoA-3.2.0.2-2019
* Autodesk Arnold for 3ds Max 2018 (Arnold-Version 5.3.0.2) (Version 1.2.926)
* Autodesk Arnold for 3ds Max 2019 (Arnold-Version 5.3.0.2) (Version 1.2.926)
* Autodesk Arnold for 3ds Max 2020 (Arnold-Version 5.3.0.2) (Version 1.2.926)
* Chaos Group V-Ray for Maya 2017 (Version 4.12.01)
* Chaos Group V-Ray for Maya 2018 (Version 4.12.01)
* Chaos Group V-Ray for Maya 2019 (Version 4.04.03)
* Chaos Group V-Ray for 3ds Max 2018 (Version 4.20.01)
* Chaos Group V-Ray for 3ds Max 2019 (Version 4.20.01)
* Chaos Group V-Ray for 3ds Max 2020 (Version 4.20.01)
* Blender (2.79)
* Blender (2.80)
* AZ 10

> [!IMPORTANT]
> Um V-Ray mit Maya außerhalb der [Azure Batch-Erweiterungsvorlagen](https://github.com/Azure/batch-extension-templates) auszuführen, starten Sie `vrayses.exe`, bevor Sie das Rendering ausführen. Wenn Sie „vrayses.exe“ außerhalb der Vorlagen starten möchten, können Sie den folgenden Befehl verwenden: `%MAYA_2017%\vray\bin\vrayses.exe"`.
>
> Ein Beispiel finden Sie im Starttask der [Vorlage „Maya und V-Ray“](https://github.com/Azure/batch-extension-templates/blob/master/templates/maya/render-vray-windows/pool.template.json) auf GitHub.

## <a name="applications-on-previous-windows-server-2016-rendering-images"></a>Anwendungen auf früheren Windows Server 2016-Renderingimages

Die folgende Liste betrifft Renderingimages von Windows Server 2016, Version 1.3.7.

* Autodesk Maya I/O 2017 Update 5 (Version 17.4.5459)
* Autodesk Maya I/O 2018 Update 4 (Version 18.4.0.7622)
* Autodesk 3ds Max I/O 2019 Update 1 (Version 21.2.0.2219)
* Autodesk 3ds Max I/O 2018 Update 4 (Version 20.4.0.4254)
* Autodesk Arnold for Maya 2017 (Arnold-Version 5.2.0.1) MtoA-3.1.0.1-2017
* Autodesk Arnold for Maya 2018 (Arnold-Version 5.2.0.1) MtoA-3.1.0.1-2018
* Autodesk Arnold for 3ds Max 2018 (Arnold-Version 5.0.2.4) (Version 1.2.926)
* Autodesk Arnold for 3ds Max 2019 (Arnold-Version 5.0.2.4) (Version 1.2.926)
* Chaos Group V-Ray for Maya 2018 (Version 3.52.03)
* Chaos Group V-Ray for 3ds Max 2018 (Version 3.60.02)
* Chaos Group V-Ray for Maya 2019 (Version 3.52.03)
* Chaos Group V-Ray for 3ds Max 2019 (Version 4.10.01)
* Blender (2.79)

> [!NOTE]
> Mit Chaos Group V-Ray für 3ds Max 2019 (Version 4.10.01) werden nicht abwärtskompatible Änderungen an V-Ray eingeführt. Wenn Sie die vorherige Version (Version 3.60.02) einsetzen möchten, verwenden Sie Renderingimages mit Windows Server 2016, Version 1.3.2.

## <a name="next-steps"></a>Nächste Schritte

Damit Sie die Rendering-VM-Images verwenden können, müssen Sie sie beim Erstellen eines Pools in der Poolkonfiguration angeben. Informationen finden Sie unter [Azure Batch rendering capabilities](https://docs.microsoft.com/azure/batch/batch-rendering-functionality#batch-pools) (Renderingfunktionen von Azure Batch).

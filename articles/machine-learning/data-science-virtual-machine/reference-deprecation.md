---
title: 'Referenz: Veraltete DSVM-Images'
description: Details zu veralteten Azure Data Science Virtual Machine (DSVM)
author: gvashishtha
ms.service: machine-learning
ms.subservice: data-science-vm
ms.author: gopalv
ms.date: 10/14/2019
ms.topic: reference
ms.openlocfilehash: 4968ac08b75141ed84994ca287215a34728232c4
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72333443"
---
# <a name="reference-deprecation-of-dsvm-images"></a>Referenz: Veraltete DSVM-Images

Im folgenden werden Vorschläge zum Umgang mit anstehendem Veralten bei Azure Data Science Virtual Machine erläutert.

## <a name="windows-2012-migrating-data-disks"></a>Windows 2012: Migrieren von Datenträgern

Der Support des Windows 2012 DSVM-Images wird am 5. November 2019 eingestellt. Führen Sie die folgenden Schritte aus, um einen Datenträger von Ihrer vorhandenen Windows 2012 DSVM zu einer Windows 2016 DSVM zu migrieren:

1. Erstellen Sie eine neue Windows 2016 DSVM, und befolgen Sie die [hier](./provision-vm.md#create-your-dsvm) gezeigten Anweisungen.
1. Trennen Sie unter Befolgen [dieser Anweisungen](../../virtual-machines/windows/detach-disk.md) vorhandene Datenträger von Ihrem Windows 2012-Image.
1. Fügen Sie unter Befolgen [dieser Anweisungen](../../virtual-machines/windows/attach-disk-ps.md#attach-an-existing-data-disk-to-a-vm) den Datenträger aus dem vorherigen Schritt an Ihr Windows 2016-Image an.

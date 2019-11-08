---
title: Verwenden eines Katalogs mit freigegebenen Images in Azure Lab Services | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie ein Labkonto für die Verwendung eines Katalogs mit freigegebenen Images konfigurieren, sodass ein Benutzer ein Image für andere freigeben und ein anderer Benutzer mithilfe dieses Images eine Vorlage für virtuelle Computer im Lab erstellen kann.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2019
ms.author: spelluru
ms.openlocfilehash: f438c32deb7e923f08396b0580d807d6e5b5e69a
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73585043"
---
# <a name="use-a-shared-image-gallery-in-azure-lab-services"></a>Verwenden eines Katalogs mit freigegebenen Images in Azure Lab Services
In diesem Artikel wird gezeigt, wie Lehrkräfte/Labadministratoren ein VM-Vorlagenimage speichern können, damit es von anderen wiederverwendet werden kann. Diese Images werden in einem [Katalog mit freigegebenen Images](../../virtual-machines/windows/shared-image-galleries.md) von Azure gespeichert. Als Erstes fügt der Labadministrator einen vorhandenen Katalog mit freigegebenen Images an das Labkonto an. Nachdem der Katalog mit freigegebenen Images angefügt wurde, können in Labs, die unter dem Labkonto erstellt wurden, Images in dem Katalog mit freigegebenen Images gespeichert werden. Andere Lehrkräfte können dieses Image im Katalog mit freigegebenen Images auswählen, um eine Vorlage für ihre Klassen zu erstellen. 

## <a name="prerequisites"></a>Voraussetzungen
- Erstellen Sie mithilfe von [Azure PowerShell](../../virtual-machines/windows/shared-images.md) oder über die [Azure-Befehlszeilenschnittstelle](../../virtual-machines/linux/shared-images.md) einen Katalog mit freigegebenen Images.
- Sie haben den Katalog mit freigegebenen Images an das Lab-Konto angefügt. Schrittweise Anweisungen finden Sie unter [Anfügen oder Trennen eines Katalogs mit freigegebenen Images](how-to-attach-detach-shared-image-gallery.md).


## <a name="save-an-image-to-the-shared-image-gallery"></a>Speichern eines Images im Katalog mit freigegebenen Images
Nachdem ein Katalog mit freigegebenen Images angefügt wurde, kann ein Labkontoadministrator oder eine Lehrkraft ein Image im Katalog mit freigegebenen Images speichern, das dann von anderen Lehrkräften wiederverwendet werden kann. 

1. Wählen Sie auf der Seite **Vorlage** für das Lab auf der Symbolleiste die Option **Export to Shared Image Gallery** (Nach Shared Image Gallery exportieren) aus.

    ![Schaltfläche „Image speichern“](../media/how-to-use-shared-image-gallery/export-to-shared-image-gallery-button.png)
2. Geben Sie im Dialogfeld **Export to Shared Image Gallery** (Nach Shared Image Gallery exportieren) einen **Namen für das Image** ein, und wählen Sie **Exportieren** aus. 

    ![Dialogfeld „Export to Shared Image Gallery“ (Nach Shared Image Gallery exportieren)](../media/how-to-use-shared-image-gallery/export-to-shared-image-gallery-dialog.png)
3. Der Fortschritt dieses Vorgangs wird auf der Seite **Vorlage** angezeigt. Dieser Vorgang kann einige Zeit dauern. 

    ![Aktiver Exportvorgang](../media/how-to-use-shared-image-gallery/exporting-image-in-progress.png)
4. War der Exportvorgang erfolgreich, wird die folgende Meldung angezeigt:

    ![Export erfolgreich](../media/how-to-use-shared-image-gallery/exporting-image-completed.png)

Sie können ein Image auch außerhalb des Kontexts eines Labs in den Katalog mit freigegebenen Images hochladen. Weitere Informationen finden Sie unter [Katalog mit freigegebenen Images: Übersicht](../../virtual-machines/windows/shared-images.md). 

## <a name="use-an-image-from-the-shared-image-gallery"></a>Verwenden eines Images aus dem Katalog mit freigegebenen Images
Lehrer/Dozenten können beim Erstellen eines neuen Labs ein benutzerdefiniertes Image aus dem Katalog mit freigegebenen Images als Vorlage auswählen.

![Verwenden eines VM-Images aus dem Katalog](../media/how-to-use-shared-image-gallery/use-shared-image.png)

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Katalogen mit freigegebenen Images finden Sie in der [Übersicht über den Katalog mit freigegebenen Images](../../virtual-machines/windows/shared-image-galleries.md).

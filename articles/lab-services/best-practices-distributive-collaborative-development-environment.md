---
title: Verteilte kollaborative Entwicklung von Azure DevTest Labs Ressourcen | Microsoft Docs
description: Bietet Best Practices für die Einrichtung einer verteilten und kollaborativen Entwicklungsumgebung zur Entwicklung von DevTest Labs-Ressourcen.
services: devtest-lab,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2019
ms.author: spelluru
ms.openlocfilehash: 8ffc8ed3f84284ff69e9515cba0982790b823a37
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543763"
---
# <a name="best-practices-for-distributed-and-collaborative-development-of-azure-devtest-labs-resources"></a>Best Practices für die verteilte und gemeinschaftliche Entwicklung von Azure DevTest Labs-Ressourcen
Verteilte kollaborative Entwicklung ermöglicht es verschiedenen Teams oder Personen, eine Codebasis zu entwickeln und zu pflegen. Um erfolgreich zu sein, hängt der Entwicklungsprozess von der Fähigkeit ab, Informationen zu erstellen, zu teilen und zu integrieren. Dieses wichtige Entwicklungsprinzip kann in den Azure DevTest Labs eingesetzt werden. Es gibt verschiedene Typen von Ressourcen in einem Labor, die häufig zwischen verschiedenen Labs in einem Unternehmen verteilt sind. Die verschiedenen Arten von Ressourcen sind in zwei Bereiche gegliedert:

- Ressourcen, die intern im Labor gelagert werden (laborbasiert)
- Ressourcen, die in [externen Repositorien gelagert sind, die mit dem Labor verbunden sind](devtest-lab-add-artifact-repo.md) (basierend auf Code-Repositorien). 

Dieses Dokument beschreibt einige Best Practices, die die Zusammenarbeit und Verteilung in mehreren Teams ermöglichen und gleichzeitig die Anpassung und Qualität auf allen Ebenen sicherstellen.

## <a name="lab-based-resources"></a>Lab-basierte Ressourcen

### <a name="custom-virtual-machine-images"></a>Benutzerdefinierte Images virtueller Maschinen
Sie können eine gemeinsame Quelle für benutzerdefinierte Bilder haben, die nächtlich in Labors bereitgestellt werden. Detaillierte Informationen finden Sie unter [Image factory](image-factory-create.md).    

### <a name="formulas"></a>Formeln
[Formeln](devtest-lab-manage-formulas.md) sind Lab-spezifisch und haben keinen Verteilungsmechanismus. Die Lab-Mitglieder entwickeln alle Formeln. 

## <a name="code-repository-based-resources"></a>Code-Repositorium-basierte Ressourcen
Es gibt zwei verschiedene Funktionen, die auf Code Repositorien, Artefakten und Umgebungen basieren. Dieser Artikel geht auf die Funktionen und die effektivste Einrichtung von Repositorien und Workflows ein, um die Möglichkeit zu geben, die verfügbaren Artefakte und Umgebungen auf Unternehmens- oder Teamebene anzupassen.  Dieser Workflow basiert auf der standardisierten[Quellcode Branchingstrategie](/azure/devops/repos/tfvc/branching-strategies-with-tfvc?view=azure-devops). 

### <a name="key-concepts"></a>Wichtige Begriffe
Die Quellinformationen für Artefakte beinhalten Metadaten, Skripte. Zu den Quellinformationen für Umgebungen gehören Metadaten- und Ressourcen-Manager-Vorlagen mit allen unterstützenden Dateien wie PowerShell-Skripte, DSC-Skripte, Zip-Dateien und so weiter.  

### <a name="repository-structure"></a>Struktur des Repositoriums  
Die gebräuchlichste Konfiguration für die Quellcodekontrolle (SCC) ist die Einrichtung einer mehrschichtigen Struktur zum Speichern von Code-Dateien (Resource Manager-Vorlagen, Metadaten und Skripte), die in den Labors verwendet werden. Erstellen Sie insbesondere verschiedene Repositorien, um Ressourcen zu speichern, die von den verschiedenen Ebenen des Unternehmens verwaltet werden:   

- Unternehmensweite Ressourcen.
- Geschäftseinheit/Geschäftsbereichsweite Ressourcen
- Team-spezifische Ressourcen.

Jede dieser Ebenen verweist auf ein anderes Repositorium, in dem der Hauptzweig die Produktionsqualität aufweisen muss. Die [Verzweigungen](/azure/devops/repos/git/git-branching-guidance?view=azure-devops) in jedem Repositorium dienen der Entwicklung dieser spezifischen Ressourcen (Artefakte oder Vorlagen). Diese Struktur passt gut zu DevTest Labs, da Sie mehrere Repositorien und mehrere Verzweigungen gleichzeitig mit den Labors des Unternehmens verbinden können. Der Repositoriums-Name ist in der Benutzeroberfläche (UI) enthalten, um Verwechslungen bei identischen Namen, Beschreibungen und Publishern zu vermeiden.
     
Die folgende Abbildung zeigt zwei Repositorien: ein Unternehmens-Repositorium, das von der IT-Abteilung gepflegt wird, und ein Bereichs-Repositorium, das von der F&E-Abteilung verwaltet wird.

![Eine exemplarische distributive und kollaborative Entwicklungsumgebung](./media/best-practices-distributive-collaborative-dev-env/distributive-collaborative-dev-env.png)
   
Diese mehrschichtige Struktur ermöglicht die Entwicklung unter Beibehaltung eines höheren Qualitätsniveaus im Master-Zweig, während mehrere Repositorien, die an ein Labor angeschlossen sind, eine größere Flexibilität ermöglichen.

## <a name="next-steps"></a>Nächste Schritte    
Entsprechende Informationen finden Sie in den folgenden Artikeln:

- Fügen Sie ein Repositorium zu einem Labor hinzu, das entweder das [Azure-Portal](devtest-lab-add-artifact-repo.md) oder die [Vorlage Azure Resource Management](add-artifact-repository.md) verwendet
- [DevTest Labs-Artefakte](devtest-lab-artifact-author.md)
- [DevTest Labs Umgebungen](devtest-lab-create-environment-from-arm.md).

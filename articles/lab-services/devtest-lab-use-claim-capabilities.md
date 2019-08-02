---
title: Verwalten von Anspruchsfunktionen in Azure DevTest Labs | Microsoft-Dokumentation
description: Erfahren Sie mehr über verschiedene Szenarien für die Nutzung der Funktionen für den Anspruch und die Aufhebung des Anspruchs von Azure DevTest Labs
services: devtest-lab
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2019
ms.author: spelluru
ms.openlocfilehash: a15148260bccadc59966c86031100f0e0332b0f9
ms.sourcegitcommit: 470041c681719df2d4ee9b81c9be6104befffcea
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2019
ms.locfileid: "67861617"
---
# <a name="use-claim-capabilities-in-azure-devtest-labs"></a>Verwalten von Anspruchsfunktionen in Azure DevTest Labs
Der Azure DevTest Labs-Dienst verbessert die Effektivität und Effizienz von Entwicklern und Testern. Dieser Artikel konzentriert sich auf die Möglichkeit, virtuelle Computer in den Azure DevTest Labs in Anspruch zu nehmen oder den Anspruch wieder aufzuheben. Es werden auch verschiedene Möglichkeiten aufgelistet, wie diese Funktion die Benutzerfreundlichkeit verbessert. Bevor wir uns verschiedene Szenarien ansehen, in denen diese Funktion verwendet werden kann, schauen wir uns an, was ein **Anspruch** ist und wie er funktioniert.

## <a name="claimable-machines"></a>Anforderbare Computer
Ein anforderbarer Computer ist ein virtueller Computer (VM), der in einem Lab ohne Besitzer erstellt wird. Sobald der Computer angefordert wurde, stehen dem Benutzer die gesamte Bandbreite der Optionen für den virtuellen Computer zur Verfügung. Wenn ein Benutzer einen Computer anfordert, werden einige Änderungen vorgenommen. Der virtuelle Computer wird aus der Liste der **anforderbaren virtuellen Computer** in die Liste **„Eigene virtuelle Computer“** im Azure-Portal verschoben. 

Der Benutzer kann sich mit dem virtuellen Computer verbinden, Artefakte anpassen, ihn neu starten oder stoppen oder den Anspruch auf den Computer freigeben. Es gibt zwei Verfahren zum Erstellen eines anforderbaren virtuellen Computers:

- Erstellen Sie einen Computer und heben Sie dann den Anspruch wieder auf, damit er zurück in den Pool der anforderbaren Computer verschoben wird. 
- Erstellen Sie einen virtuellen Computer und platzieren Sie ihn im gemeinsamen Pool mit den [erweiterten Einstellungen](https://azure.microsoft.com/updates/azure-devtest-labs-claim-lab-vms-from-a-shared-pool/).

Es gibt zwei Fälle, in denen die Funktionen zum Anspruch und zum Aufheben des Anspruchs effektiv genutzt werden können. Der erste Fall erfordert mehr Voraussicht und Planung, damit er richtig entworfen und ausgeführt werden kann. Der Zweite ist eher situationsbedingt. Es folgen Beispiele für diese Art von unterschiedlichen Fällen.

## <a name="designed-use-of-claimable-machines"></a>Konzipierte Verwendung von anforderbaren Computern

- **Softwareentwicklung/-tests:** Ermöglichen Sie Entwicklern oder Testern eine höhere Produktivität, indem Sie die konfigurierten Maschinen bereit und in einem nicht angeforderten Zustand bereitstellen. Eine Reihe von VMs mit unterschiedlichen Konfigurationen, notwendigen Tools und dem neuesten Code ermöglicht es Benutzern, eine VM anzufordern und sofort mit der Arbeit zu beginnen, ohne die Zeit für die Einrichtung eines Computers aufwenden zu müssen. Bevor die VMs in Anspruch genommen werden, werden die Computer bereitgestellt, aber heruntergefahren, was die Kosten für weniger häufig genutzte Computer minimiert. Wenn die VMs benötigt werden, fordert ein Benutzer einfach die VM an, die den Computer startet. Die Option zum Aufheben des Anspruchs ist in diesem Fall nicht so nützlich, da das Erstellen einer neuen VM oft einfacher und kostengünstiger ist.
- **Classroom/Labs:** Sorgen Sie dafür, dass VMs für eine Klasse oder ein Lab vorkonfiguriert sind, sodass die Kursteilnehmer sofort eine Verbindung zu einem Computer über das Azure-Portal herstellen können.  Sobald ein Kursteilnehmer eine VM beansprucht, stellt das Labor sicher, dass niemand den gleichen Computer beanspruchen kann. Die Automatisierung dieses Prozesses stellt sicher, dass die erforderliche Anzahl von Computern mit der angegebenen Umgebung zur Verfügung steht. Wenn Kursteilnehmer nicht erscheinen oder zu spät kommen, können die nicht beanspruchten Computer mit minimalen Kosten bis zum Ende der Sitzung zur Verfügung stehen bleiben. Die Option zum Aufheben des Anspruchs ist in diesem Szenario nicht so effektiv, da sich die VM in einem unbekannten Zustand befindet, wenn der vorherige Benutzer fertig ist.
- **Demonstrationen:** Verwenden Sie Computer für Demonstrationen, bei denen die Computer im Lab mit spezifischen Umgebungen ausgestattet sind. Diese Funktion ist nützlich, wenn mehrere Personen gleichzeitig oder zu beliebigen Zeiten eine Demonstration durchführen, z. B. bei einer Konferenz. Die Option zum Aufheben des Anspruchs kann in dieser Situation nützlich sein, da die Demo den Zustand des Computer nicht ändern sollte, sodass Benutzer eine VM für die nächste Demonstration wieder in den Pool der verfügbaren Computer zurückführen können. Da der nicht beanspruchte Computer nicht mehr bereitgestellt wird und minimale Kosten anfallen, können VMs für längere Zeit im Lab verbleiben.
- **Zeitarbeiter/Auftragnehmer:** Ermöglicht Benutzern, einen Computer zu verwenden. Wenn sie das Unternehmen verlassen, geben sie die VM ohne Datenverlust an den Pool zurück. Wenn die VM nicht in Anspruch genommen wird, kann ein anderer Benutzer die VM in Anspruch nehmen und an dem Computer weiterarbeiten oder ihn überprüfen, um zusätzliche Informationen zu erhalten.
- **Im Allgemeinen:** Die Möglichkeit, eine einzige Quelle zu haben, die VMs automatisch in einem bestimmten Rhythmus konfiguriert und bereitstellt, ist in vielen verschiedenen Situationen nützlich. Es gibt mehrere verschiedene Situationen, in denen die Anspruchs-/Aufhebefunktion den Benutzern hilft, effizienter zu sein, indem sie einen automatisierten Prozess zum Erstellen der VMs in einem nicht beanspruchten Zustand mit einer festgelegten Konfiguration durchführt. Die Konfiguration(en) können je nach Bedarf verschiedene Betriebssysteme, Sprachen, Datenträger oder [andere Software (Artefakte)](devtest-lab-artifact-author.md) beinhalten. Die Möglichkeit, eine VM aus dem Lab zu beziehen, ermöglicht es dem Lab-Benutzer, ein richtig konfiguriertes System zu erhalten, ohne die Zeit oder den Aufwand für die Konfiguration des Computers aufzuwenden. Der Lab-Leiter könnte den beanspruchten Zustand der VMs nutzen, um die Anzahl der erzeugten Computer zu optimieren, Computer zu bereinigen und die Priorität der Konfigurationen zu bestimmen. Die [Image Factory](image-factory-create.md) ist ein gutes Beispiel für einen automatisierten Prozess zum Erstellen von VMs und Images für mehrere Labs. Die Skripte können modifiziert werden, um eine der folgenden Situationen mit den entsprechenden Änderungen auszuführen oder als Referenz für die Erstellung eines benutzerdefinierten Systems verwendet werden.

## <a name="situational-use-of-claimable-machines"></a>Situativer Einsatz von anforderbaren Computern

- Verwenden Sie die Anspruchs-/Aufhebefunktion, um die Kontrolle über Computer von einer Person zur anderen weiterzugeben, ohne explizit zu wissen, wer den Computer als Nächstes beanspruchen wird.
- Die Entwicklung, das Testen und das Debugging eines Szenarios kann durchgeführt werden, in dem eine bestimmte Computerkonfiguration einen Fehler reproduziert. Dann kann der Anspruch auf den Computer aufgehoben werden, damit der nächste Entwickler den Computer beanspruchen und mit der Arbeit fortfahren kann. Diese Funktion ist besonders nützlich, da immer mehr Menschen in verschiedenen Regionen der Welt von zuhause aus arbeiten. 
- Die Teammitglieder können mit einer einzigen Umgebung arbeiten. Beispielsweise können Sie manuell eine komplexe Umgebung einrichten, die nicht automatisiert werden kann, oder Ressourcen erstellen, die nur Änderungen für eine einzelne Eingabe wie Images verarbeiten können. In der Vergangenheit wurde dieses Problem durch die Inbetriebnahme eines dedizierten Computers gelöst. Die abrufbare Funktion ist eine Verbesserung gegenüber dem manuellen Prozess, da sie über eine integrierte Benutzerzugriffssteuerung und eine visuelle Identifizierung verfügt, sofern verfügbar. Wenn die VM nicht in Anspruch genommen wird, wird sie nicht mehr bereitgestellt und kann so die Kosten senken.
- Stellen Sie einen Datenträger bereit, der an eine VM angehängt ist. Jede Festplatte mit bis zu 1 TB Daten ermöglicht es, ein großes Datenvolumen zu übertragen, ohne die Daten kopieren oder duplizieren zu müssen. Die VM wird zunächst mit einer angehängten Festplatte erstellt, die ein großes Datenvolumen aufweist.  Jeder Benutzer kann dann den Computer beanspruchen und auf die Daten zugreifen. Wenn Benutzer fertig sind, heben sie den Anspruch auf die VM wieder auf, um anderen Benutzern den Zugriff auf den Computer zu ermöglichen.

Es gibt einige Vorbehalte gegen die Verwendung von abrufbaren Computern, vor allem, wenn es um den Zugriff auf den Computer geht. Wenn der Computer mit einer Domäne verknüpft ist, dann muss der Benutzer, der den Anspruch auf den Computer erhebt, bereits Zugriff darauf erhalten haben. Dies erfolgt in der Regel durch die Gewährung von Zugriff für eine Gruppe, die alle Benutzer im Lab umfasst, wenn die VM erstellt wird. Wenn der Computer nicht mit der Domäne verbunden ist, muss das Artefakt **„VM-Passwort zurücksetzen“** im öffentlichen Repository ausgeführt werden, um den Benutzer als Administrator hinzuzufügen.  Artefakte können auch nach dem Start oder der Beanspruchung der Maschine angewendet werden.

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie den folgenden Artikel: [Erstellen und Verwalten anforderbarer virtueller Computer in einem Lab in Azure DevTest Labs](devtest-lab-add-claimable-vm.md)

---
title: Grundlegendes zum Rabatt für reservierte Azure-VM-Instanzen | Microsoft-Dokumentation
description: Hier erfahren Sie, wie der Rabatt für reservierte Azure-VM-Instanzen auf ausgeführte virtuelle Computer angewendet wird.
author: yashesvi
manager: yashar
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2019
ms.author: banders
ms.openlocfilehash: 191160035f516d818d5537c5c47f9604998c46f7
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "67849994"
---
# <a name="how-the-azure-reservation-discount-is-applied-to-virtual-machines"></a>Anwendung des Rabatts für Azure-Reservierungen auf virtuelle Computer

Nachdem Sie eine reservierte Azure-VM-Instanz erworben haben, wird der Reservierungsrabatt automatisch auf virtuelle Computer angewendet, die den Attributen und der Menge der Reservierung entsprechen. Eine Reservierung deckt die Computekosten Ihrer virtuellen Computer ab.

Ein Reservierungsrabatt gilt für die Basis-VMs, die Sie vom Azure Marketplace erwerben.

Informationen zu reservierten SQL-Datenbank-Kapazitäten finden Sie unter [Grundlegendes zur Anwendung des Rabatts für Azure-Reservierungen auf SQL-Datenbank-Instanzen](billing-understand-reservation-charges.md).

In der folgenden Tabelle sind die Kosten für virtuelle Computer nach dem Kauf einer reservierten VM-Instanz veranschaulicht. In jedem Fall werden Ihnen die regulären Gebühren für Speicher und Netzwerke berechnet.

| Typ des virtuellen Computers  | Gebühren bei einer reservierten VM-Instanz |
|-----------------------|--------------------------------------------|
|Virtuelle Linux-Computer ohne zusätzliche Software | Die Reservierung deckt die Kosten der VM-Infrastruktur ab.|
|Virtuelle Linux-Computer mit Softwaregebühren (z.B. Red Hat) | Die Reservierung deckt die Kosten der Infrastruktur ab. Zusätzliche Software wird in Rechnung gestellt.|
|Virtuelle Windows-Computer ohne zusätzliche Software |Die Reservierung deckt die Kosten der Infrastruktur ab. Windows-Software wird in Rechnung gestellt.|
|Virtuelle Windows-Computer mit zusätzlicher Software (z.B. SQL Server) | Die Reservierung deckt die Kosten der Infrastruktur ab. Windows-Software und zusätzliche Software werden in Rechnung gestellt.|
|Virtuelle Windows-Computer mit [Azure-Hybridvorteil](../virtual-machines/windows/hybrid-use-benefit-licensing.md) | Die Reservierung deckt die Kosten der Infrastruktur ab. Die Kosten für die Windows-Software werden durch den Azure-Hybridvorteil abgedeckt. Zusätzliche Software wird separat in Rechnung gestellt.|

## <a name="how-reservation-discount-is-applied"></a>Wie der Reservierungsrabatt angewendet wird

Reservierungsrabatte funktionieren nach dem Prinzip „*use-it-or-lose-it*“. Das heißt, wenn Sie für eine Stunde nicht über die entsprechenden Ressourcen verfügen, verlieren Sie eine Reservierungsmenge für diese Stunde. Sie können ungenutzte reservierte Stunden nicht übertragen.

Wenn Sie eine Ressource beenden, wird der Reservierungsrabatt automatisch auf eine andere entsprechende Ressource im angegebenen Reservierungsumfang angewandt. Wenn keine übereinstimmenden Ressourcen im angegebenen Reservierungsumfang gefunden werden, gehen die reservierten Stunden *verloren*.

## <a name="reservation-discount-for-non-windows-vms"></a>Reservierungsrabatte für virtuelle Nicht-Windows-Computer

 Der Rabatt für Azure-Reservierungen wird auf Stundenbasis auf ausgeführte VM-Instanzen angewendet. Die erworbenen Reservierungen werden zur Anwendung des Reservierungsrabatts auf die von den ausgeführten virtuellen Computern ausgegebene Nutzung abgestimmt. Bei virtuellen Computern, die keine volle Stunde lang ausgeführt werden, wird die Reservierung von anderen virtuellen Computern aufgefüllt, die keine Reservierung verwenden. Dazu gehören auch gleichzeitig ausgeführte virtuelle Computer. Am Ende der Stunde wird die Anwendung der Reservierung für VMs für diese Stunde gesperrt. Wenn eine VM nicht eine ganze Stunde lang ausgeführt wird oder gleichzeitige VMs diese Reservierungsstunde nicht auffüllen, ist die Reservierung für diese Stunde nicht vollständig ausgelastet. Das folgende Diagramm veranschaulicht die Anwendung einer Reservierung auf die abrechenbare VM-Nutzung. Die Abbildung basiert auf einem Reservierungskauf und zwei entsprechenden VM-Instanzen.

![Screenshot einer angewendeten Reservierung und zweier entsprechender VM-Instanzen](./media/billing-reserved-vm-instance-application/billing-reserved-vm-instance-application.png)

1. Jegliche Nutzung, die über der Grenze für die Reservierung liegt, wird zu den regulären Preisen der nutzungsbasierten Bezahlung in Rechnung gestellt. Die Nutzung unterhalb der Grenze für Reservierungen wird nicht in Rechnung gestellt, da dafür bereits im Rahmen eines Reservierungskaufs bezahlt wurde.
2. In der ersten Stunde wird Instanz 1 für 0,75 Stunden und Instanz 2 für 0,5 Stunden ausgeführt. Die Gesamtnutzung für die erste Stunde beträgt 1,25 Stunden. Ihnen wird der Preis für die nutzungsbasierte Bezahlung für die verbleibenden 0,25 Stunden in Rechnung gestellt.
3. In der zweiten und dritten Stunde wurden beide Instanzen jeweils eine Stunde lang ausgeführt. Eine Instanz wird durch die Reservierung abgedeckt, die andere wird zu den Preisen für die nutzungsbasierte Zahlung in Rechnung gestellt.
4. In der vierten Stunde wird Instanz 1 für 0,5 Stunden und Instanz 2 für 1 Stunde ausgeführt. Instanz 1 wird vollständig durch die Reservierung abgedeckt. Außerdem sind 0,5 Stunden von Instanz 2 abgedeckt. Ihnen wird der Preis für die nutzungsbasierte Zahlung für die verbleibenden 0,5 Stunden in Rechnung gestellt.

Wenn Sie grundlegende Informationen wünschen und die Anwendung Ihrer Azure-Reservierungen in Abrechnungsnutzungsberichten anzeigen möchten, lesen Sie [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](billing-understand-reserved-instance-usage-ea.md).

## <a name="reservation-discount-for-windows-vms"></a>Reservierungsrabatte für virtuelle Windows-Computer

Wenn Sie Windows-VM-Instanzen ausführen, wird die Reservierung angewendet, um die Infrastrukturkosten abzudecken. Die Anwendung der Reservierung auf die VM-Infrastrukturkosten ist bei virtuellen Windows-Computer und virtuellen Nicht-Windows-Computern identisch. Windows-Software wird separat basierend auf vCPU-Basis in Rechnung gestellt. Informationen finden Sie unter [Nicht in Azure-Reservierungen enthaltene Windows-Softwarekosten](billing-reserved-Instance-windows-software-costs.md). Sie können Ihre Windows-Lizenzierungskosten mit dem [Azure-Hybridvorteil für Windows Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md) decken.

## <a name="discount-can-apply-to-different-sizes"></a>Rabatt kann für verschiedene Größen gelten.

Wenn Sie reservierte VM-Instanzen erwerben, und Sie unter **Optimiert für** die **Instanzgrößenflexibilität** auswählen, hängt die Abdeckung des Rabatts von der ausgewählten VM-Größe ab. Die Reservierung kann für VM-Größen in derselben Größenordnung gelten. Weitere Informationen finden Sie unter [Flexibilität bei der VM-Größe mit reservierten VM-Instanzen](../virtual-machines/windows/reserved-vm-instance-size-flexibility.md).

## <a name="discount-applies-to-matching-servicetype-only"></a>Rabatt gilt nur bei Übereinstimmung von „ServiceType“.

Ein Reservierungsrabatt gilt nur für die VM-Nutzung, bei der der Wert `ServiceType` in `AdditionalInfo` mit der gekauften Reservierung übereinstimmt. Bei der Anwendung von Reservierungsrabatten wird die für virtuelle Computer verwendete Verbrauchseinheit ignoriert, nur `ServiceType` wird ausgewertet. Sie müssen wissen, für welchen Diensttyp Sie den virtuellen Computer gekauft haben. Sie können eine VM-Reservierung ohne Storage Premium gegen eine Reservierung mit Storage Premium umtauschen und umgekehrt.

## <a name="services-that-get-vm-reservation-discounts"></a>Dienste, die VM-Reservierungsrabatte erhalten

Ihre VM-Reservierungen können für die von mehreren Diensten ausgegebene VM-Nutzung gelten, nicht allein für Ihre VM-Bereitstellungen. Welche Ressourcen Reservierungsrabatte erhalten, ändert sich abhängig von der Einstellung für die Instanzgrößenflexibilität.

### <a name="instance-size-flexibility-setting"></a>Einstellung der Instanzgrößenflexibilität

Die Einstellung der Instanzgrößenflexibilität bestimmt, welche Dienste reservierte Instanzrabatte erhalten.

Unabhängig davon, ob die Einstellung ein- oder ausgeschaltet ist, gelten Reservierungsrabatte automatisch für jede übereinstimmende VM-Nutzung, wenn der *ConsumedService* gleich `Microsoft.Compute` ist. Überprüfen Sie daher ihre Nutzungsdaten auf den Wert *ConsumedService*. Beispiele hierfür sind:

- Virtuelle Computer
- VM-Skalierungsgruppen
- Containerdienst
- Azure Batch-Bereitstellungen (im Benutzerabonnementmodus)
- Azure Kubernetes Service (AKS)
- Service Fabric

Wenn die Einstellung eingeschaltet ist, gelten Reservierungsrabatte automatisch für jede übereinstimmende VM-Nutzung, wenn der *ConsumedService* einem der folgenden Elemente entspricht:

- Microsoft.Compute
- Microsoft.ClassicCompute
- Microsoft.Batch
- Microsoft.MachineLearningServices
- Microsoft.Kusto

Überprüfen Sie den Wert *ConsumedService* in ihren Nutzungsdaten, um zu ermitteln, ob die Nutzung zu Reservierungsrabatten berechtigt ist.

Weitere Informationen zur Instanzgrößenflexibilität finden Sie unter [Flexibilität bei der VM-Größe mit reservierten VM-Instanzen](../virtual-machines/windows/reserved-vm-instance-size-flexibility.md).


## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Kontakt

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:

- [Was sind Azure-Reservierungen?](billing-save-compute-costs-reservations.md)
- [Vorauszahlen für virtuelle Computer mit Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Vorauszahlen von SQL-Datenbank-Computeressourcen mit reservierter Azure SQL-Datenbank-Kapazität](../sql-database/sql-database-reserved-capacity.md)
- [Verwalten von Azure-Reservierungen](billing-manage-reserved-vm-instance.md)
- [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](billing-understand-reserved-instance-usage.md)
- [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](billing-understand-reserved-instance-usage-ea.md)
- [Grundlegendes zur Verwendung von Azure-Reservierungen für CSP-Abonnements](/partner-center/azure-reservations)
- [Nicht in Azure-Reservierungen enthaltene Windows-Softwarekosten](billing-reserved-instance-windows-software-costs.md)

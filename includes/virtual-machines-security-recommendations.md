---
title: include file
description: include file
services: virtual-machines
author: barclayn
ms.service: virtual-machines
ms.topic: include
ms.date: 09/19/2019
ms.author: barclayn
ms.custom: include file
ms.openlocfilehash: 60d4957f65c124de683c6d156561de3b52aaec94
ms.sourcegitcommit: a7a9d7f366adab2cfca13c8d9cbcf5b40d57e63a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71164081"
---
Dieser Artikel enthält Sicherheitsempfehlungen für virtuelle Azure-Computer. Die Umsetzung dieser Empfehlungen erleichtert es Ihnen, Ihre Sicherheitspflichten zu erfüllen, die im Microsoft-Modell der gemeinsamen Verantwortung beschrieben werden. Außerdem erhöhen Sie dadurch die Gesamtsicherheit Ihrer Web-App-Lösungen. Weitere Informationen dazu, wie Microsoft seiner Verantwortung als Dienstanbieter nachkommt, finden Sie im Dokument [Shared Responsibilities for Cloud Computing](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/153019/1/Shared%20responsibilities%20for%20cloud%20computing.pdf) (Gemeinsame Verantwortung für das Cloud Computing).

Einige der in diesem Artikel enthaltenen Empfehlungen können automatisch durch Azure Security Center überwacht werden. Azure Security Center ist die erste Verteidigungslinie zum Schutz Ihrer Ressourcen in Azure. Der Dienst analysiert regelmäßig den Sicherheitsstatus Ihrer Azure-Ressourcen, um potenzielle Sicherheitsrisiken zu erkennen. Anschließend erhalten Sie Empfehlungen dazu, wie damit umgegangen werden kann.

- Weitere Informationen zu Empfehlungen von Azure Security Center finden Sie unter [Sicherheitsempfehlungen in Azure Security Center](../articles/security-center/security-center-recommendations.md).
- Weitere Informationen zu Azure Security Center finden Sie unter [Was ist Azure Security Center?](../articles/security-center/security-center-intro.md)

## <a name="recommendations"></a>Empfehlungen

| Category | Empfehlung | Kommentare | Security Center |
|-|-|----|--|
| Allgemein | Beim Erstellen von benutzerdefinierten VM-Images die neuesten Updates verwenden | Bevor Sie ein Image erstellen, sollten Sie sich vergewissern, dass für das Betriebssystem und alle Anwendungen, die im Image enthalten sind, alle neuesten Updates installiert sind.  | - |
| Allgemein | VMs auf dem neuesten Stand halten | Sie können die Lösung für [Updateverwaltung](../articles/automation/automation-update-management.md) in Azure Automation verwenden, um Betriebssystemupdates für Ihre Windows- und Linux-Computer in Azure zu verwalten. | [Ja](../articles/security-center/security-center-apply-system-updates.md) |
| Allgemein | Virtuelle Computer sichern | Mit [Azure Backup](../articles/backup/backup-overview.md) können Sie Ihre Anwendungsdaten bei minimalen Betriebskosten schützen. Anwendungsfehler können Ihre Daten beschädigen, und menschliche Fehler können Bugs in Ihren Anwendungen verursachen. Mit Azure Backup sind Ihre virtuellen Windows- und Linux-Computer geschützt. | - |
| Allgemein | Mehrere VMs für größere Ausfallsicherheit und Verfügbarkeit verwenden | Werden auf Ihrer VM Anwendungen ausgeführt, für die Hochverfügbarkeit erforderlich ist, sollten Sie mehrere VMs oder [Verfügbarkeitsgruppen](../articles/virtual-machines/windows/manage-availability.md) verwenden. | - |
| Allgemein | Strategie für Geschäftskontinuität und Notfallwiederherstellung (Business Continuity & Disaster Recovery, BCDR) umsetzen | Azure Site Recovery ermöglicht es Ihnen, unter verschiedenen Optionen zu wählen, die Ihre Geschäftskontinuitätsanforderungen unterstützen. Die Unterstützung erstreckt sich über verschiedene Replikations- und Failoverszenarien. Weitere Informationen finden Sie unter [Informationen zu Azure Site Recovery](../articles/site-recovery/site-recovery-overview.md). | - |
| Identitäts- und Zugriffsverwaltung | VM-Authentifizierung zentralisieren | Sie können die Authentifizierung Ihrer virtuellen Windows- und Linux-Computer mit [Azure AD-Authentifizierung](../articles/active-directory/develop/authentication-scenarios.md) zentralisieren. | - |
| Datensicherheit | Betriebssystemdatenträger verschlüsseln | Mit [Azure Disk Encryption](../articles/security/azure-security-disk-encryption-overview.md) können Sie die Datenträger von virtuellen Windows- und Linux-IaaS-Computern verschlüsseln. Verschlüsseln von Datenträgern bewirkt, dass deren Inhalte ohne die erforderlichen Schlüssel nicht gelesen werden können. Datenträgerverschlüsselung schützt gespeicherte Daten vor nicht autorisiertem Zugriff, der andernfalls möglich wäre, wenn der jeweilige Datenträger kopiert wurde.| [Ja](../articles/security-center/security-center-apply-disk-encryption.md) |
| Datensicherheit | Datenträger verschlüsseln | Mit [Azure Disk Encryption](../articles/security/azure-security-disk-encryption-overview.md) können Sie die Datenträger von virtuellen Windows- und Linux-IaaS-Computern verschlüsseln. Verschlüsseln von Datenträgern bewirkt, dass deren Inhalte ohne die erforderlichen Schlüssel nicht gelesen werden können. Datenträgerverschlüsselung schützt gespeicherte Daten vor nicht autorisiertem Zugriff, der andernfalls möglich wäre, wenn der jeweilige Datenträger kopiert wurde.| -  |
| Datensicherheit | Installierte Software beschränken | Durch Beschränken der installierten Software auf die Bestandteile, die für eine erfolgreiche Implementierung Ihrer Lösung erforderlich sind, wird die Angriffsfläche Ihrer Lösung verringert. | - |
| Datensicherheit | Antivirus-/Antischadsoftware verwenden | In Azure können Sie Antischadsoftware von Sicherheitsanbietern wie Microsoft, Symantec, Trend Micro und Kaspersky verwenden. Diese Software dient dem Schutz Ihrer virtuellen Computer vor Dateien mit schädlichem Inhalt, Adware und anderen Bedrohungen. Sie können Schutz durch Antischadsoftware von Microsoft basierend auf Ihren Anwendungsworkloads bereitstellen, entweder mit der einfachen Konfiguration, die Schutz durch die Standardeinstellungen bietet, oder mit einer erweiterten benutzerdefinierten Konfiguration. Weitere Informationen zu Microsoft Antimalware für Azure finden Sie unter [Microsoft Antimalware für Azure Cloud Services und Virtual Machines](../articles/security/azure-security-antimalware.md). | - |
| Datensicherheit | Schlüssel und Geheimnisse sicher speichern | Dadurch, dass Sie die Verwaltung Ihrer Geheimnisse und Schlüssel vereinfachen, indem Sie für die Anwendungsbesitzer eine sichere, zentral verwaltete Option bereitstellen, können Sie das Risiko von versehentlichen Gefährdungen oder Verlusten verringern. Mit Azure Key Vault können Sie Ihre Schlüssel sicher in Hardwaresicherheitsmodulen (Hardware Security Modules, HSMs) speichern, die gemäß FIPS 140-2 Level 2 zertifiziert sind. Wenn Sie Ihre Schlüssel und Geheimnisse gemäß FIPS 140-2 Level 3 speichern müssen, können Sie [Azure Dedicated HSM](../articles/dedicated-hsm/overview.md) verwenden. | - |
| Netzwerk | Zugriff auf Verwaltungsports einschränken | Angreifer scannen öffentliche Cloud-IP-Adressbereiche nach offenen Verwaltungsports und versuchen „einfache“ Angriffe mittels häufig verwendeter Kennwörter und bekannter ungepatchter Sicherheitslücken. Mit [Just-In-Time-VM-Zugriff (JIT-VM-Zugriff)](../articles/security-center/security-center-just-in-time.md) kann eingehender Datenverkehr auf den Azure-VMs gesperrt werden, um die Gefährdung durch Angriffe zu reduzieren und bei Bedarf einfachen Zugriff auf Verbindungen mit virtuellen Computern bereitzustellen. | - |
| Netzwerk | Netzwerkzugriff einschränken | Mit Netzwerksicherheitsgruppen können Sie den Netzwerkzugriff einschränken und die Anzahl der verfügbaren Endpunkte festlegen. Weitere Informationen finden Sie unter [Erstellen, Ändern oder Löschen einer Netzwerksicherheitsgruppe](../articles/virtual-network/manage-network-security-group.md). | - |
| Überwachung | Virtuelle Computer überwachen | Sie können [Azure Monitor für VMs](../articles/azure-monitor/insights/vminsights-overview.md) verwenden, um den Zustand Ihrer virtuellen Azure-Computer und Ihrer VM-Skalierungsgruppen zu überwachen. Leistungsprobleme bei einem virtuellen Computer können zu einer Dienstunterbrechung führen und gegen das Sicherheitsprinzip der Verfügbarkeit verstoßen. | - |

## <a name="next-steps"></a>Nächste Schritte

Erkundigen Sie sich bei Ihrem Anwendungsanbieter, ob weitere Sicherheitsanforderungen bestehen. Weitere Informationen zum Entwickeln sicherer Anwendungen finden Sie unter [Dokumentation zur sicheren Entwicklung](../articles/security/fundamentals/abstract-develop-secure-apps.md).
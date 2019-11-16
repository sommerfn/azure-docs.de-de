---
title: include file
description: include file
services: virtual-machines
author: barclayn
ms.service: virtual-machines
ms.topic: include
ms.date: 11/13/2019
ms.author: barclayn
ms.custom: include file
ms.openlocfilehash: d0ede337f2ae17f28cd02df7ed35919bd9955e7a
ms.sourcegitcommit: b1a8f3ab79c605684336c6e9a45ef2334200844b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74048568"
---
Dieser Artikel enthält Sicherheitsempfehlungen für virtuelle Azure-Computer. Das Befolgen dieser Empfehlungen kann Sie beim Erfüllen der Sicherheitsverpflichtungen unterstützen, die in unserem Modell für gemeinsame Verantwortung beschrieben sind. Die Empfehlungen helfen Ihnen außerdem, die Gesamtsicherheit Ihrer Web-App-Lösungen zu verbessern. Weitere Informationen dazu, wie Microsoft seiner Verantwortung als Dienstanbieter nachkommt, finden Sie unter [Gemeinsame Verantwortung für das Cloud Computing](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91).

Einige der Empfehlungen in diesem Artikel können automatisch durch Azure Security Center berücksichtigt werden. Azure Security Center ist die erste Verteidigungslinie für Ihre Ressourcen in Azure. Der Dienst analysiert regelmäßig den Sicherheitsstatus Ihrer Azure-Ressourcen, um potenzielle Sicherheitsrisiken zu erkennen. Anschließend gibt er eine Empfehlung ab, wie Sie die Sicherheitsrisiken beheben sollten. Weitere Informationen finden Sie unter [Sicherheitsempfehlungen in Azure Security Center anzeigen](../articles/security-center/security-center-recommendations.md).

Allgemeine Informationen zu Azure Security Center finden Sie unter [Was ist Azure Security Center?](../articles/security-center/security-center-intro.md).

## <a name="general"></a>Allgemein

| Empfehlung | Kommentare | Security Center |
|-|----|--|
| Wenden Sie beim Erstellen benutzerdefinierter VM-Images die neuesten Updates an. | Installieren Sie vor dem Erstellen von Images die neuesten Updates für das Betriebssystem und alle Anwendungen, die im Image enthalten sein sollen.  | - |
| Halten Sie Ihre virtuellen Computer auf dem neuesten Stand. | Sie können die Lösung für [Updateverwaltung](../articles/automation/automation-update-management.md) in Azure Automation verwenden, um Betriebssystemupdates für Ihre Windows- und Linux-Computer in Azure zu verwalten. | [Ja](../articles/security-center/security-center-apply-system-updates.md) |
| Sichern Sie Ihre VMs. | Mit [Azure Backup](../articles/backup/backup-overview.md) können Sie Ihre Anwendungsdaten bei minimalen Betriebskosten schützen. Anwendungsfehler können Ihre Daten beschädigen, und menschliche Fehler können Bugs in Ihren Anwendungen verursachen. Azure Backup schützt Ihre VMs, auf denen Windows und Linux ausgeführt werden. | - |
| Verwenden Sie mehrere VMs, um größere Ausfallsicherheit und Verfügbarkeit zu erreichen. | Wenn Ihre VM Anwendungen ausführt, die in hohem Maß verfügbar sein müssen, verwenden Sie mehrere VMs oder [Verfügbarkeitsgruppen](../articles/virtual-machines/windows/manage-availability.md). | - |
| Führen Sie eine Strategie für Geschäftskontinuität und Notfallwiederherstellung (Business Continuity & Disaster Recovery, BCDR) ein. | Azure Site Recovery ermöglicht es Ihnen, unter verschiedenen Optionen zu wählen, die Ihre Geschäftskontinuitätsanforderungen unterstützen. Die Unterstützung erstreckt sich über verschiedene Replikations- und Failoverszenarien. Mehr dazu erfahren Sie unter [Informationen zu Azure Site Recovery](../articles/site-recovery/site-recovery-overview.md). | - |

## <a name="data-security"></a>Datensicherheit

| Empfehlung | Kommentare | Security Center |
|-|----|--|
| Verschlüsseln Sie Betriebssystemdatenträger. | Mit [Azure Disk Encryption](../articles/security/azure-security-disk-encryption-overview.md) können Sie die Datenträger Ihrer Windows- und Linux-IaaS-VMs verschlüsseln. Ohne die erforderlichen Schlüssel sind die Inhalte verschlüsselter Datenträger unlesbar. Datenträgerverschlüsselung schützt gespeicherte Daten vor nicht autorisiertem Zugriff, der andernfalls nach einem Kopieren des betreffenden Datenträgers möglich wäre.| [Ja](../articles/security-center/security-center-apply-disk-encryption.md) |
| Verschlüsseln Sie Datenträger. | Mit [Azure Disk Encryption](../articles/security/azure-security-disk-encryption-overview.md) können Sie die Datenträger Ihrer Windows- und Linux-IaaS-VMs verschlüsseln. Ohne die erforderlichen Schlüssel sind die Inhalte verschlüsselter Datenträger unlesbar. Datenträgerverschlüsselung schützt gespeicherte Daten vor nicht autorisiertem Zugriff, der andernfalls nach einem Kopieren des betreffenden Datenträgers möglich wäre.| -  |
| Beschränken Sie die installierte Software. | Schränken Sie die installierte Software auf das ein, was für die erfolgreiche Anwendung Ihrer Lösung erforderlich ist. Diese Richtlinie trägt dazu bei, die Angriffsfläche Ihrer Lösung zu reduzieren. | - |
| Verwenden Sie Antivirus-/Antischadsoftware. | In Azure können Sie Antischadsoftware von Sicherheitsanbietern wie Microsoft, Symantec, Trend Micro und Kaspersky verwenden. Diese Software dient dem Schutz Ihrer VMs vor Dateien mit schädlichem Inhalt, Adware und anderen Bedrohungen. Abhängig von Ihren Anwendungsworkloads können Sie Microsoft Antimalware bereitstellen. Verwenden Sie entweder eine standardmäßig sichere Grundkonfiguration oder eine erweiterte benutzerdefinierte Konfiguration. Weitere Informationen finden Sie unter [Microsoft Antimalware für Azure Cloud Services und Virtual Machines](../articles/security/azure-security-antimalware.md). | - |
| Speichern Sie Schlüssel und Geheimnisse sicher. | Vereinfachen Sie die Verwaltung Ihrer Geheimnisse und Schlüssel, indem Sie für die Anwendungsbesitzer eine sichere, zentral verwaltete Option bereitstellen. Diese Art der Verwaltung verringert das Risiko von versehentlichen Gefährdungen oder Verlusten. Mit Azure Key Vault können Sie Ihre Schlüssel sicher in Hardwaresicherheitsmodulen (Hardware Security Modules, HSMs) speichern, die gemäß FIPS 140-2 Level 2 zertifiziert sind. Wenn Sie FIPS 140-2 Level 3 zum Speichern Ihrer Schlüssel und Geheimnisse verwenden müssen, können Sie [Azure Dedicated HSM](../articles/dedicated-hsm/overview.md) nutzen. | - |

## <a name="identity-and-access-management"></a>Identitäts- und Zugriffsverwaltung 

| Empfehlung | Kommentare | Security Center |
|-|----|--|
| Zentralisieren Sie die VM-Authentifizierung. | Sie können die Authentifizierung Ihrer Windows- und Linux-VMs mit [Azure Active Directory-Authentifizierung](../articles/active-directory/develop/authentication-scenarios.md) zentralisieren. | - |

## <a name="monitoring"></a>Überwachung

| Empfehlung | Kommentare | Security Center |
|-|----|--|
| Überwachen Sie Ihre VMs. | Sie können [Azure Monitor für VMs](../articles/azure-monitor/insights/vminsights-overview.md) verwenden, um den Zustand Ihrer Azure-VMs und Ihrer VM-Skalierungsgruppen zu überwachen. Leistungsprobleme bei einem virtuellen Computer können zu einer Dienstunterbrechung führen und gegen das Sicherheitsprinzip der Verfügbarkeit verstoßen. | - |

## <a name="networking"></a>Netzwerk

| Empfehlung | Kommentare | Security Center |
|-|----|--|
| Schränken Sie den Zugriff auf Verwaltungsports ein. | Angreifer scannen öffentliche Cloud-IP-Adressbereiche nach offenen Verwaltungsports und versuchen „einfache“ Angriffe mittels häufig verwendeter Kennwörter und bekannter ungepatchter Sicherheitslücken. Mit einem [JIT-VM-Zugriff](../articles/security-center/security-center-just-in-time.md) (Just-In-Time-VM-Zugriff) können Sie eingehenden Datenverkehr auf den Azure-VMs sperren, um die Gefährdung durch Angriffe zu reduzieren und dennoch bei Bedarf komfortable Verbindungen zu virtuellen Computern bereitzustellen. | - |
| Schränken Sie den Netzwerkzugriff ein. | Mit Netzwerksicherheitsgruppen können Sie den Netzwerkzugriff einschränken und die Anzahl der verfügbaren Endpunkte festlegen. Weitere Informationen finden Sie unter [Erstellen, Ändern oder Löschen einer Netzwerksicherheitsgruppe](../articles/virtual-network/manage-network-security-group.md). | - |

## <a name="next-steps"></a>Nächste Schritte

Erkundigen Sie sich bei Ihrem Anwendungsanbieter, ob weitere Sicherheitsanforderungen bestehen. Weitere Informationen zum Entwickeln sicherer Anwendungen finden Sie unter [Dokumentation zur sicheren Entwicklung](../articles/security/fundamentals/abstract-develop-secure-apps.md).

---
title: Grundlegendes zur Architektur der Lösung „Azure Security Center für IoT“ | Microsoft-Dokumentation
description: Lernen Sie den Informationsfluss im Azure Security Center for IoT-Dienst kennen.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 2cf6a49b-5d35-491f-abc3-63ec24eb4bc2
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: a013d4cfcfddc709e60e91adf57bc27c98934a96
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68596552"
---
# <a name="azure-security-center-for-iot-architecture"></a>Azure Security Center für IoT-Architektur

In diesem Artikel wird die funktionale Systemarchitektur der Lösung „Azure Security Center für IoT“ erläutert. 

## <a name="azure-security-center-for-iot-components"></a>Azure Security Center für IoT-Komponenten

Azure Security Center für IoT besteht aus den folgenden Komponenten:
- IoT Hub-Integration
- Geräte-Agents (optional)
- SDK zum Senden von Sicherheitsmeldungen
- Analytics-Pipeline
 
### <a name="azure-security-center-for-iot-workflows"></a>Azure Security Center für IoT-Workflows

Azure Security Center für IoT arbeitet in einem von zwei Featureworkflows: „Integriert“ und „Erweitert“  

### <a name="built-in"></a>Integriert
Im Modus **Integriert** wird Azure Security Center für IoT aktiviert, wenn Sie in Ihrem IoT Hub die Option **Sicherheit** aktivieren. Durch Echtzeitüberwachung, Empfehlungen und Warnungen bietet der Modus „Integriert“ Gerätesichtbarkeit und beispiellose Sicherheit in einem Schritt. Der Modus „Integriert“ erfordert keine Agentinstallation auf Geräten, und er verwendet erweiterte Analysen für protokollierte Aktivitäten, um Ihr Feldgerät zu analysieren und zu schützen. 

### <a name="enhanced"></a>Verbessert 
Nach dem Aktivieren der Option **Sicherheit** in Ihrem IoT Hub und der Installation von Azure Security Center für IoT-Geräte-Agents auf Ihren Geräten erfassen, aggregieren und analysieren die Agents im Modus **Erweitert** Sicherheitsereignis-Rohdaten von ihren Geräten. Sicherheitsereignis-Rohdaten können IP-Verbindungen, die Prozesserstellung, Benutzeranmeldungen und andere sicherheitsrelevante Informationen enthalten. Azure Security Center für IoT-Geräte-Agents verarbeiten auch die Ereignisaggregation, um hohe Netzwerkdurchsätze zu vermeiden. Die Agents sind in hohem Maße anpassbar, sodass Sie sie für bestimmte Aufgaben wie das Senden ausschließlich wichtiger Informationen mit dem dringlichsten SLA oder für das Aggregieren umfangreicher Sicherheitsinformationen und Kontextdaten zu größeren Segmenten verwenden und auf diese Weise höhere Servicekosten vermeiden können.

![Azure Security Center für IoT-Architektur](./media/architecture/azure-iot-security-architecture.png)
 
Geräte-Agents und andere Anwendungen verwenden das **Azure Security Center SDK zum Senden von Sicherheitsmeldungen**, um Sicherheitsinformationen an Azure IoT Hub zu senden. IoT Hub übernimmt diese Informationen und leitet sie an den Dienst „Azure Security Center für IoT“ weiter.

Sobald der Dienst „Azure Security Center für IoT“ aktiviert wurde, versendet IoT Hub zusätzlich zu den weitergeleiteten Daten alle seine internen Daten zur Analyse durch Azure Security Center für IoT. Diese Daten umfassen Cloudvorgangsprotokolle von Geräten, Geräte-Identitäten und die Hub-Konfiguration. Alle diese Informationen tragen zum Erstellen der Azure Security Center für IoT-Analysepipeline bei.
 
Die Azure Security Center für IoT-Analysepipeline empfängt auch zusätzliche Threat Intelligence-Streams aus verschiedenen Quellen innerhalb von Microsoft und Microsoft-Partnern. Die gesamte Azure Security Center für IoT-Analysepipeline arbeitet mit sämtlichen für den Dienst erstellten Kundenkonfigurationen (z.B. benutzerdefinierten Warnungen und Verwendung des SDK zum Senden von Sicherheitsmeldungen).
 
Bei der Verwendung der Analysepipeline kombiniert Azure Security Center für IoT alle Informationsdatenströme, um direkt umsetzbare Empfehlungen und Warnungen zu generieren. Die Pipeline enthält sowohl benutzerdefinierte Regeln, die von Sicherheitsanalytikern und Experten erstellt wurden, als auch nach Abweichungen vom standardmäßigen Geräteverhalten suchende Machine Learning-Modelle und Risikoanalysen.
 
Azure Security Center für IoT-Empfehlungen und -Warnungen (Analysepipeline-Ausgabe) werden in den Log Analytics-Arbeitsbereich des einzelnen Kunden geschrieben. Durch die Einbeziehung der Ereignisrohdaten im Arbeitsbereich sowie der Warnungen und Empfehlungen werden eingehende Untersuchungen und Abfragen möglich, die die genauen Details der festgestellten verdächtigen Aktivitäten verwenden.  

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie die grundlegende Architektur und den Workflow der Azure Security Center für IoT-Lösung kennengelernt. Weitere Informationen zu den Voraussetzungen sowie zum Einstieg in Ihre Sicherheitslösung in IoT Hub und zu deren Aktivierung finden Sie in den folgenden Artikeln:

- [ASC für IoT-Voraussetzungen](service-prerequisites.md)
- [Erste Schritte](getting-started.md)
- [Konfigurieren Ihrer IoT-Lösung](quickstart-configure-your-solution.md)
- [Enable security in IoT Hub (Aktivieren der Sicherheit in IoT Hub)](quickstart-onboard-iot-hub.md)
- [Azure Security Center für IoT – Häufig gestellte Fragen (FAQ)](resources-frequently-asked-questions.md)
- [Azure Security Center für IoT – Sicherheitswarnungen](concept-security-alerts.md)

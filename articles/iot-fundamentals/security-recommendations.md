---
title: Sicherheitsempfehlungen für Azure IoT | Microsoft-Dokumentation
description: In diesem Artikel werden zusätzliche Schritte zur Gewährleistung der Sicherheit Ihrer Azure IoT Hub-Lösung zusammengefasst.
author: dsk-2015
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: article
ms.date: 09/25/2019
ms.author: dkshir
ms.custom: security-recommendations
ms.openlocfilehash: 9c8891eca87444c2be43294f208ad5257ffce889
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73177181"
---
# <a name="security-recommendations-for-azure-internet-of-things-iot-deployment"></a>Sicherheitsempfehlungen für die Bereitstellung von Azure Internet of Things (IoT)

Dieser Artikel enthält Sicherheitsempfehlungen für das Internet der Dinge. Die Umsetzung dieser Empfehlungen erleichtert es Ihnen, Ihre Sicherheitspflichten zu erfüllen, die in unserem Modell der gemeinsamen Verantwortung beschrieben werden. Weitere Informationen dazu, wie Microsoft seiner Verantwortung als Dienstanbieter nachkommt, finden Sie unter [Gemeinsame Verantwortung für das Cloud Computing](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91).

Einige der in diesem Artikel enthaltenen Empfehlungen können automatisch durch Azure Security Center überwacht werden. Azure Security Center ist die erste Verteidigungslinie zum Schutz Ihrer Ressourcen in Azure. Der Dienst analysiert regelmäßig den Sicherheitsstatus Ihrer Azure-Ressourcen, um potenzielle Sicherheitsrisiken zu erkennen. Anschließend erhalten Sie Empfehlungen dazu, wie damit umgegangen werden kann.

- Weitere Informationen zu Empfehlungen von Azure Security Center finden Sie unter [Sicherheitsempfehlungen in Azure Security Center](../security-center/security-center-recommendations.md).
- Weitere Informationen zu Azure Security Center finden Sie unter [Was ist Azure Security Center?](../security-center/security-center-intro.md)

## <a name="recommendations"></a>Empfehlungen

| Category | Empfehlung | Kommentare | Von ASC unterstützt |
|-|-|----|--|
| Allgemein | Bleiben Sie auf dem neusten Stand | Verwenden Sie aktuelle Versionen der unterstützten Plattformen, Programmiersprachen, Protokolle und Frameworks. | - |
| Allgemein | Schützen von Authentifizierungsschlüsseln | Bewahren Sie die Geräte-IDs und ihre Authentifizierungsschlüssel nach der Bereitstellung physisch geschützt auf. Dadurch wird vermieden, dass sich ein böswilliges Gerät als registriertes Gerät ausgibt. | - |
| Allgemein | Verwenden von Geräte-SDKs nach Möglichkeit | Geräte-SDKs implementieren eine Vielzahl von Sicherheitsmerkmalen, wie z.B. Verschlüsselung, Authentifizierung usw., um Sie bei der Entwicklung einer zuverlässigen und sicheren Geräteanwendung zu unterstützen. Weitere Informationen finden Sie unter [Verstehen und Verwenden von Azure IoT Hub SDKs](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks). | - |
| Identitäts- und Zugriffsverwaltung | Definieren der Zugriffssteuerung für den Hub | [Verstehen und definieren Sie die Art des Zugriffs](iot-security-deployment.md#securing-the-cloud), die jede Komponente in Ihrer IoT Hub-Lösung basierend auf der Funktionalität haben wird. Die zulässigen Berechtigungen sind *Registry Read*, *RegistryReadWrite*, *ServiceConnect* und *DeviceConnect*. Standardmäßige [SAS-Richtlinien in Ihrem IoT-Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security#access-control-and-permissions) können auch dazu beitragen, die Berechtigungen jeder Komponente basierend auf ihrer Rolle zu definieren. | - |
| Identitäts- und Zugriffsverwaltung | Definieren der Zugriffssteuerung für Back-End-Dienste | Daten, die von Ihrer IoT-Hub-Lösung erfasst werden, können von anderen Azure-Diensten wie [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/), [Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/), [App Service](https://docs.microsoft.com/azure/app-service/), [Logik-Apps](https://docs.microsoft.com/azure/logic-apps/) und [Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) genutzt werden. Stellen Sie sicher, dass Sie die für diese Dienste dokumentierten Zugriffsberechtigungen verstehen und erteilen. | - |
| Datenschutz | Sichere Geräteauthentifizierung | Stellen Sie eine sichere Kommunikation zwischen Ihren Geräten und Ihrem IoT-Hub sicher, indem Sie entweder [einen eindeutigen Identitätsschlüssel oder ein Sicherheitstoken](iot-security-deployment.md#iot-hub-security-tokens) oder für jedes Gerät [ein auf dem Gerät befindliches X.509-Zertifikat](iot-security-deployment.md#x509-certificate-based-device-authentication) verwenden. Wählen Sie die geeignete Methode, um [Sicherheitstoken basierend auf dem gewählten Protokoll (MQTT, AMQP oder HTTPS)](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security) einzusetzen. | - |
| Datenschutz | Sichere Kommunikation zwischen Geräten | IoT Hub schützt die Verbindung mit den Geräten gemäß dem TLS-Standard (Transport Layer Security) und unterstützt die Versionen 1.2 und 1.0. Verwenden Sie [TLS 1.2](https://tools.ietf.org/html/rfc5246), um maximale Sicherheit zu gewährleisten. | - |
| Datenschutz | Sichern der Dienstkommunikation | IoT Hub bietet Endpunkte für die Verbindung mit Back-End-Diensten wie [Azure Storage](/azure/storage/) oder [Event Hubs](/azure/event-hubs), wobei nur das TLS-Protokoll verwendet und kein Endpunkt auf einem unverschlüsselten Kanal offengelegt wird. Sobald diese Daten diese Back-End-Dienste zur Speicherung oder Analyse erreichen, stellen Sie sicher, dass Sie geeignete Sicherheits- und Verschlüsselungsmethoden für den jeweiligen Dienst verwenden und sensible Informationen im Back-End schützen. | - |
| Netzwerk | Schützen des Zugriffs auf Ihre Geräte | Beschränken Sie Hardwareanschlüsse in Ihren Geräten auf ein absolutes Minimum, um unerwünschten Zugriff zu vermeiden. Entwickeln Sie darüber hinaus Mechanismen, um physische Manipulationen am Gerät zu verhindern oder zu erkennen. Weitere Informationen finden Sie unter [Bewährte Methoden für die IoT-Sicherheit](iot-security-best-practices.md). | - |
| Netzwerk | Entwickeln sicherer Hardware | Integrieren Sie Sicherheitsmerkmale wie verschlüsselte Speicherung oder Trusted Platform Module (TPM), um Geräte und Infrastruktur sicherer zu gestalten. Halten Sie das Gerätebetriebssystem und Treiber auf dem neuesten Stand, und installieren Sie, sofern der Platz es zulässt, Antiviren- und Antischadsoftware. Unter [IoT-Sicherheitsarchitektur](iot-security-architecture.md) erfahren Sie, wie dies dazu beitragen kann, mehrere Sicherheitsbedrohungen zu entschärfen. | - |
| Überwachung | Überwachen des nicht autorisierten Zugriffs auf Ihre Geräte |  Überwachen Sie mithilfe der Protokollierungsfunktion Ihres Gerätebetriebssystems etwaige Sicherheitsverletzungen oder physische Manipulationen am Gerät oder an seinen Anschlüssen. | - |
| Überwachung | Überwachen Ihrer IoT-Lösung in der Cloud | Überwachen Sie die allgemeine Integrität Ihrer IoT Hub-Lösung mithilfe der [Metriken in Azure Monitor](https://docs.microsoft.com/azure/iot-hub/iot-hub-metrics). | - |
| Überwachung | Einrichten der Diagnose | Beobachten Sie Ihren Betrieb genau, indem Sie Ereignisse in Ihrer Lösung protokollieren und dann die Diagnoseprotokolle an Azure Monitor senden, um einen Überblick über die Leistung zu erhalten. Weitere Informationen finden Sie unter [Überwachen und Diagnostizieren von Problemen in Ihrem IoT-Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitor-resource-health). | - |

## <a name="next-steps"></a>Nächste Schritte

Für weitergehende Szenarien mit Azure IoT müssen Sie möglicherweise zusätzliche Sicherheitsanforderungen berücksichtigen. Weitere Anleitungen finden Sie unter [IoT-Sicherheitsarchitektur](iot-security-architecture.md).


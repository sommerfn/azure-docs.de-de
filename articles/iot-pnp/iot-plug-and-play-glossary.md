---
title: 'Glossar: IoT Plug & Play (Vorschau) | Microsoft-Dokumentation'
description: 'Konzepte: Ein Glossar mit häufig verwendeten Begriffen in Zusammenhang mit IoT Plug & Play (Vorschau).'
author: ChrisGMsft
ms.author: chrisgre
ms.date: 10/10/2019
ms.topic: conceptual
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
manager: philmea
ms.openlocfilehash: a5c9d2d54fcce1179e43c38027db23c3d7e557e8
ms.sourcegitcommit: 961468fa0cfe650dc1bec87e032e648486f67651
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72249300"
---
# <a name="glossary-of-terms-for-iot-plug-and-play-preview"></a>Glossar für IoT Plug & Play (Vorschau)

Definitionen häufig verwendeter Begriffe, die in den Artikeln zu IoT Plug & Play verwendet werden.

## <a name="azure-certified-for-iot-portal"></a>Azure Certified for IoT-Portal

Sie können die Website [Azure Certified for IoT-Portal](https://aka.ms/ACFI) für folgende Aufgaben verwenden:

- Abschließen des [Zertifizierungsprozesses](#device-certification) für Ihr [IoT Plug & Play-Gerät](#iot-plug-and-play-device).
- Suchen von [Gerätefunktionsmodellen](#device-capability-model).
- Veröffentlichen eines Gerätefunktionsmodell im [öffentlichen Modellrepository](#public-model-repository).

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Die Azure-Befehlszeilenschnittstelle (Command Line Interface, CLI) ist ein plattformübergreifendes Befehlszeilentool zum Verwalten von Azure-Ressourcen. Die Azure IoT-Erweiterung für die Azure-Befehlszeilenschnittstelle ist ein Befehlszeilentool zum Interagieren mit und Testen von [IoT Plug & Play-Geräten](#iot-plug-and-play-device). Sie können die Erweiterung für folgende Aufgaben verwenden:

- Herstellen einer Verbindung mit einem IoT Plug & Play-Gerät.
- Anzeigen der vom Gerät gesendeten [Telemetriedaten](#telemetry).
- Arbeiten mit [Geräteeigenschaften](#properties).
- Aufrufen von [Gerätebefehlen](#commands).
- Verwalten von [Modellrepositorys](#model-repository), [Schnittstellen](#interface) und [Gerätefunktionsmodellen](#device-capability-model).

## <a name="azure-iot-central"></a>Azure IoT Central

Azure IoT Central ist eine vollständig verwaltete SaaS-Lösung (Software-as-a-Service), mit der Sie Ihre [IoT Plug & Play-Geräte](#iot-plug-and-play-device) auf einfache Weise verbinden, überwachen und verwalten können. Sie können mithilfe von [Gerätefunktionsmodellen](#device-capability-model) eine IoT Central-Anwendung für die Überwachung und Verwaltung Ihrer Geräte automatisch konfigurieren.

## <a name="azure-iot-certification-service"></a>Azure IoT-Zertifizierungsdienst

Der Azure IoT-Zertifizierungsdienst führt eine Reihe von Zertifizierungstests aus, wenn Sie ein [IoT Plug & Play-Gerät](#iot-plug-and-play-device) zur Zertifizierung über das [Azure Certified for IoT-Portal](#azure-certified-for-iot-portal) senden. Bevor Sie ein Gerät zum [Certified for IoT-Gerätekatalog](#certified-for-iot-device-catalog) hinzufügen können, muss es zertifiziert werden.

## <a name="azure-iot-tools-extension"></a>Azure IoT Tools-Erweiterung

Azure IoT Tools ist eine Sammlung von Erweiterungen in [Visual Studio-Code](#visual-studio-code), die Ihnen beim Interagieren mit IoT Hub und beim Entwickeln von IoT-Geräten helfen. Bei der Entwicklung von IoT Plug & Play-Geräten hilft es Ihnen bei folgenden Aufgaben:

- Erstellen von [Gerätefunktionsmodellen](#device-capability-model) und [Schnittstellen](#interface).
- Veröffentlichen in [Modellrepositorys](#model-repository).
- Generieren eines Codegerüsts zum Implementieren der Geräteanwendung.

## <a name="azure-iot-explorer-tool"></a>Azure IoT-Explorer-Tool

Der Azure IoT-Explorer ist ein grafisches Tool, das Sie verwenden können, um mit Ihren [IoT Plug & Play-Geräten](#iot-plug-and-play-device) zu interagieren und sie zu testen. Nachdem Sie das Tool auf Ihrem lokalen Computer installiert haben, können Sie es für folgende Aufgaben verwenden:

- Anzeigen der mit Ihrem [IoT-Hub](#azure-iot-hub) verbundenen Geräte.
- Herstellen einer Verbindung mit einem IoT Plug & Play-Gerät.
- Anzeigen der vom Gerät gesendeten [Telemetriedaten](#telemetry).
- Arbeiten mit [Geräteeigenschaften](#properties).
- Aufrufen von [Gerätebefehlen](#commands).

## <a name="azure-iot-hub"></a>Azure IoT Hub

IoT Hub ist ein in der Cloud gehosteter, verwalteter Dienst, der als zentraler Nachrichtenhub für die bidirektionale Kommunikation zwischen Ihrer IoT-Anwendung und den Geräten dient, die von der Anwendung verwaltet werden. [IoT Plug & Play-Geräte](#iot-plug-and-play-device) können eine Verbindung mit einer IoT Hub-Instanz herstellen. Eine IoT-Lösung verwendet einen IoT Hub, um Folgendes zu ermöglichen:

- Geräten das Senden von Telemetriedaten an eine cloudbasierte Lösung.
- Einer cloudbasierten Lösung das Verwalten von verbundenen Geräten.

## <a name="azure-iot-device-sdk"></a>Azure IoT-Geräte-SDK

Es gibt Geräte-SDKs für mehrere Sprachen, mit denen Sie IoT Plug & Play-Geräteclientanwendungen erstellen können. Eine der Anforderungen für [Gerätezertifizierung](#device-certification) lautet: Für den Geräteclientcode muss eines der Azure IoT-Geräte-SDKs verwendet werden.

## <a name="certified-for-iot-device-catalog"></a>Certified for IoT-Gerätekatalog

Im [Certified for IoT-Gerätekatalog](https://catalog.azureiotsolutions.com/) werden [IoT Plug & Play-Geräte](#iot-plug-and-play-device) aufgelistet, die die [Gerätezertifizierungstests](#device-certification) bestanden haben. Die [Gerätefunktionsmodelle](#device-capability-model) für die IoT Plug & Play-Geräte im Katalog werden im öffentlichen Modellrepository veröffentlicht.

## <a name="commands"></a>Befehle

Die in einer [Schnittstelle](#interface) definierten Befehle stellen Methoden dar, die auf dem [digitalen Zwilling](#digital-twin) ausgeführt werden können. Dies kann beispielsweise ein Befehl zum Neustart eines Geräts sein.

## <a name="common-interface"></a>Allgemeine Schnittstelle

Alle [IoT Plug & Play-Geräte](#iot-plug-and-play-device) sollten einige allgemeine [Schnittstellen](#interface) implementieren. Die Geräteinformationsschnittstelle beispielsweise definiert Hardware- und Betriebssysteminformationen zum Gerät. Die [Gerätezertifizierung](#device-certification) erfordert, dass Ihr Gerät mehrere allgemeine Schnittstellen implementiert. Sie können die allgemeinen Schnittstellendefinitionen aus dem öffentlichen Modellrepository abrufen.

## <a name="company-model-repository"></a>Modellrepository des Unternehmens

Eine Organisation kann ein [Modellrepository](#model-repository) des Unternehmens als privaten Speicher für [Gerätefunktionsmodelle](#device-capability-model) und [Schnittstellen](#interface) verwenden.

## <a name="connection-string"></a>Verbindungszeichenfolge

Eine Verbindungszeichenfolge kapselt die Informationen, die zum Herstellen einer Verbindung mit einem Endpunkt erforderlich sind. Eine Verbindungszeichenfolge enthält üblicherweise die Adresse des Endpunkts und Sicherheitsinformationen, aber das Format der Verbindungszeichenfolge variiert für die verschiedenen Dienste. Im Zusammenhang mit dem IoT Hub-Dienst gibt es zwei Arten von Verbindungszeichenfolgen:

- Geräteverbindungszeichenfolgen ermöglichen es [IoT Plug & Play-Geräten](#iot-plug-and-play-device), eine Verbindung mit den geräteseitigen Endpunkten einer IoT Hub-Instanz herzustellen. Der Clientcode auf einem Gerät verwendet die Verbindungszeichenfolge zum Herstellen einer sicheren Verbindung mit einer IoT Hub-Instanz.
- IoT Hub-Verbindungszeichenfolgen ermöglichen es Back-End-Lösungen und -Tools, eine sichere Verbindung mit den dienstseitigen Endpunkten einer IoT Hub-Instanz herzustellen. Diese Lösungen und Tools verwalten die IoT Hub-Instanz und die damit verbundenen Geräte.
- Verbindungszeichenfolgen für das Modellrepository des Unternehmens ermöglichen es Back-End-Lösungen und -Tools, eine sichere Verbindung mit einem [Modellrepository des Unternehmens](#company-model-repository) herzustellen. Diese Lösungen und Tools nutzen oder verwalten die [Gerätefunktionsmodelle](#device-capability-model) und [Schnittstellen](#interface) im Repository.

## <a name="device-capability-model"></a>Gerätefunktionsmodell

Ein Gerätefunktionsmodell beschreibt ein [IoT Plug & Play-Gerät](#iot-plug-and-play-device) und definiert die von diesem Gerät implementierten [Schnittstellen](#interface). Ein Gerätefunktionsmodell entspricht normalerweise einem physischen Gerät, einem Produkt oder einer SKU. Zum Definieren eines Gerätefunktionsmodells verwenden Sie die [Digital Twin Definition Language](#digital-twin-definition-language).

## <a name="device-certification"></a>Gerätezertifizierung

Gerätezertifizierung ist der Vorgang, bei dem ein [IoT Plug & Play-Gerät](#iot-plug-and-play-device) zertifiziert wird, damit es dem [Certified for IoT-Gerätekatalog](#certified-for-iot-device-catalog) sowie dem zugehörigen [Gerätefunktionsmodell](#device-capability-model) und [Schnittstellen](#interface) hinzugefügt werden kann, die dem [öffentlichen Modellrepository](#public-model-repository) hinzugefügt wurden.

## <a name="device-developer"></a>Geräteentwickler

Ein Geräteentwickler verwendet ein [Gerätefunktionsmodell](#device-capability-model), [Schnittstellen](#interface) und ein [Azure IoT-Geräte-SDK](#azure-iot-device-sdk) zum Implementieren von Code, der auf einem [IoT Plug & Play-Gerät](#iot-plug-and-play-device) ausgeführt werden soll.

## <a name="device-modeling"></a>Gerätemodellierung

Ein [Geräteentwickler](#device-developer) verwendet die [Digital Twin Definition Language](#digital-twin-definition-language) zum Modellieren der Funktionen eines [IoT Plug & Play-Geräts](#iot-plug-and-play-device). Das Modell kann mithilfe eines Modellrepositorys freigegeben werden. Ein Geräteentwickler kann ein Gerätecodegerüst aus dem Modell generieren. Ein [Lösungsentwickler](#solution-developer) kann eine IoT-Lösung aus dem Modell konfigurieren.

## <a name="device-provisioning-service"></a>Device Provisioning-Dienst

[Azure IoT Central](#azure-iot-central) verwendet den Device Provisioning-Dienst zum Verwalten aller Geräteregistrierungen und -verbindungen. Weitere Informationen finden Sie unter [Gerätekonnektivität in Azure IoT Central](../iot-central/concepts-connectivity-pnp.md). Mithilfe des Device Provisioning-Diensts können Sie auch die Geräteregistrierung und die Verbindung mit Ihrer IoT Hub-basierten IoT-Lösung verwalten. Weitere Informationen finden Sie unter [Bereitstellen von Geräten mit dem Azure IoT Hub Device Provisioning-Dienst](../iot-dps/about-iot-dps.md).

## <a name="device-registration"></a>Geräteregistrierung

Bevor ein [IoT Plug & Play-Gerät](#iot-plug-and-play-device) eine Verbindung mit einer IoT-Lösung herstellen kann, muss es bei der Lösung registriert werden. [Azure IoT Central](#azure-iot-central) verwendet den [Device Provisioning-Dienst](#device-provisioning-service) zum Verwalten der Geräteregistrierung. In einer benutzerdefinierten IoT-Lösung können Sie Geräte bei Ihrem IoT-Hub im Azure-Portal oder programmgesteuert registrieren.

## <a name="device-first"></a>Gerätepriorisierung

[Azure IoT Central](#azure-iot-central) unterstützt eine gerätepriorisierende Registrierung und ein gerätepriorisierendes Verbindungsszenario. In diesem Szenario kann ein [IoT Plug & Play-Gerät](#iot-plug-and-play-device) eine Verbindung mit einer IoT Central-Anwendung herstellen, ohne dass es im Voraus registriert wurde. Die Registrierung erfolgt automatisch, wenn sich das Gerät zum ersten Mal mit der Anwendung verbindet.

## <a name="digital-twin"></a>Digitaler Zwilling

Ein digitaler Zwilling ist ein Modell eines [IoT Plug & Play-Geräts](#iot-plug-and-play-device). Er wird mithilfe der [Digital Twin Definition Language](#digital-twin-definition-language) modelliert. Sie können zur Laufzeit mithilfe der [Azure IoT-Geräte-SDKs](#azure-iot-device-sdk) mit digitalen Zwillingen interagieren. So können Sie beispielsweise einen Eigenschaftswert in einem digitalen Zwilling auf einem Gerät festlegen; dann kommuniziert das SDK diese Änderung an Ihrer IoT-Lösung in der Cloud.

## <a name="digital-twin-change-events"></a>Änderungsereignisse bei digitalen Zwillingen

Wenn ein [IoT Plug & Play-Gerät](#iot-plug-and-play-device) mit einem [IoT-Hub](#azure-iot-hub) verbunden wird, kann der Hub dessen Routingfunktionen nutzen, um Benachrichtigungen über Änderungen bei digitalen Zwillingen zu senden. Beispiel: Immer wenn sich ein [Eigenschaftswert](#properties) auf einem Gerät ändert, kann IoT Hub eine Benachrichtigung an einen Endpunkt, z.B. eine Service Bus-Warteschlange, senden.

## <a name="digital-twin-definition-language"></a>Digital Twin Definition Language

Eine Sprache zum Beschreiben von Modellen und Schnittstellen für [IoT Plug & Play-Geräte](#iot-plug-and-play-device). Verwenden Sie die [Digital Twin Definition Language](https://aka.ms/DTDL), um die Funktionen eines [digitalen Zwillings](#digital-twin) zu beschreiben und es der IoT-Plattform und den IoT-Lösungen zu ermöglichen, die Semantik der Entität zu nutzen.

## <a name="digital-twin-route"></a>Route für digitale Zwillinge

Eine Route, die in einem [IoT-Hub](#azure-iot-hub) eingerichtet wurde, um [Änderungsereignisse bei digitalen Zwillingen](#digital-twin-change-events) an einen Endpunkt, z.B. eine Service Bus-Warteschlange, zu übermitteln.

## <a name="interface"></a>Schnittstelle

Eine Schnittstelle beschreibt verwandte Funktionen, die durch ein [IoT Plug & Play-Gerät](#iot-plug-and-play-device) oder einen [digitalen Zwilling](#digital-twin) implementiert werden. Sie können Schnittstellen in verschiedenen [Gerätefunktionsmodellen](#device-capability-model) wieder verwenden.

## <a name="iot-hub-query-language"></a>IoT Hub-Abfragesprache

Die IoT Hub-Abfragesprache wird für mehrere Zwecke verwendet. Beispielsweise können Sie mithilfe dieser Sprache nach bei Ihrer IoT Hub-Instanz [registrierten Geräten](#device-registration) suchen oder das Verhalten des [Routings bei digitalen Zwillingen](#digital-twin-route) verfeinern.

## <a name="iot-plug-and-play-device"></a>IoT Plug & Play-Gerät

Ein IoT Plug & Play-Gerät ist normalerweise ein kleines, eigenständiges Computergerät, das Daten sammelt oder andere Geräte steuert. Auf einem solchen Gerät wird Software oder Firmware ausgeführt, die ein [Gerätefunktionsmodell](#device-capability-model) implementiert.  Ein IoT Plug & Play-Gerät könnte z.B. ein Gerät zur Überwachung von Umweltdaten oder ein Regler für ein intelligentes Bewässerungssystem in der Landwirtschaft sein. Sie können eine in der Cloud gehostete IoT-Lösung zum Steuern, Überwachen und Empfangen von Daten von IoT Plug & Play-Geräten schreiben. Im [Azure Certified for IoT-Gerätekatalog](#certified-for-iot-device-catalog) werden die verfügbaren IoT Plug & Play-Geräte aufgelistet. Jedes IoT Plug & Play-Gerät im Katalog wurde überprüft und verfügt über ein [Gerätefunktionsmodell](#device-capability-model).

## <a name="microsoft-partner-center"></a>Microsoft Partner Center

Im [Microsoft Partner Center](https://docs.microsoft.com/partner-center/) verwaltet Ihre Organisation ihre End-to-End-Beziehung zu Microsoft. Sie benötigen ein Microsoft Partner Center-Konto, bevor Sie Ihr [IoT Plug & Play-Gerät](#iot-plug-and-play-device) im [Azure Certified for IoT-Portal](#azure-certified-for-iot-portal) zertifizieren können.

## <a name="model-discovery"></a>Modellermittlung

Wenn ein [IoT Plug & Play-Gerät](#iot-plug-and-play-device) eine Verbindung mit einer IoT-Lösung herstellt, kann die Lösung die Funktionen des Geräts ermitteln, indem sie das [Gerätefunktionsmodell](#device-capability-model) sucht. Ein Gerät kann sein Funktionsmodell an die Lösung senden, oder die Lösung kann ein Gerätefunktionsmodell in einem [Modellrepository](#model-repository) suchen.

## <a name="model-repository"></a>Modellrepository

In einem Modellrepository werden [Gerätefunktionsmodelle](#device-capability-model) und [Schnittstellen](#interface) gespeichert. Es gibt ein einziges [öffentliches Modellrepository](#public-model-repository). Organisationen können ihre eigenen organisationsweiten Modellrepositorys erstellen.

## <a name="model-repository-rest-api"></a>Modellrepository-REST-API

Eine API zum Verwalten von und Interagieren mit Modellrepositorys. So können Sie beispielsweise mithilfe der API [Gerätefunktionsmodelle](#device-capability-model) hinzufügen und nach Funktionsmodellen suchen.

## <a name="properties"></a>Properties

Eigenschaften sind Datenfelder, die in einer [Schnittstelle](#interface) definiert sind und einen bestimmten Zustand eines digitalen Zwillings repräsentieren. Sie können Eigenschaften als schreibgeschützt oder beschreibbar deklarieren. Schreibgeschützte Eigenschaften, z.B. eine Seriennummer, werden durch Code festgelegt, der auf dem [IoT Plug & Play-Gerät](#iot-plug-and-play-device) selbst ausgeführt wird.  Schreibbare Eigenschaften, z.B. ein Alarmschwellenwert, werden normalerweise aus der cloudbasierten IoT-Lösung festgelegt.

## <a name="public-model-repository"></a>Öffentliches Modellrepository

Es gibt ein einziges öffentliches Modellrepository, in dem [Gerätefunktionsmodelle](#device-capability-model) und [Schnittstellen](#interface) für [zertifizierte Geräte](#device-certification) gespeichert werden. Im öffentlichen Modellrepository werden außerdem Definitionen von [allgemeinen Schnittstellen](#common-interface) gespeichert.

## <a name="registration-id"></a>Registrierungs-ID

Eine Registrierungs-ID identifiziert ein Gerät im [Device Provisioning-Dienst](#device-provisioning-service) eindeutig. Diese ID ist nicht identisch mit der Geräte-ID, bei der es sich um einen eindeutigen Bezeichner für ein Gerät in einem [IoT-Hub](#azure-iot-hub) handelt.

## <a name="scope-id"></a>Bereichs-ID

Die Bereichs-ID identifiziert eine [Device Provisioning-Dienst](#device-provisioning-service)-Instanz eindeutig.

## <a name="shared-access-signature"></a>Shared Access Signature (SAS)

Shared Access Signatures (SAS) sind ein Authentifizierungsmechanismus, der auf mit SHA-256 gesicherten Hashes oder URIs basiert. Die SAS-Authentifizierung besteht aus zwei Komponenten: einer SAS-Richtlinie und einer Shared Access Signature (oft als ein „Token“ bezeichnet). Ein [IoT Plug & Play-Gerät](#iot-plug-and-play-device) verwendet eine Shared Access Signature zum Authentifizieren bei einem [IoT-Hub](#azure-iot-hub).

## <a name="solution-developer"></a>Lösungsentwickler

Ein Lösungsentwickler erstellt das Lösungs-Back-End. Ein Lösungsentwickler arbeitet normalerweise mit Azure-Ressourcen wie [IoT Hub](#azure-iot-hub) und [Modellrepositorys](#model-repository), oder er arbeitet mit [IoT Central](#azure-iot-central).

## <a name="telemetry"></a>Telemetrie

In einer [Schnittstelle](#interface) definierte Telemetriefelder stellen Messungen dar. Diese Messungen sind normalerweise Werte wie Sensormesswerte, die von dem [IoT Plug & Play-Gerät](#iot-plug-and-play-device) als Datenstrom gesendet werden.

## <a name="visual-studio-code"></a>Visual Studio-Code

Visual Studio-Code ist ein moderner Code-Editor, der für mehrere Plattformen zur Verfügung steht. Erweiterungen, z.B. diejenigen im [Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)-Paket, ermöglichen es Ihnen, den Editor zur Unterstützung eines breiten Bereichs von Entwicklungsszenarien anzupassen.

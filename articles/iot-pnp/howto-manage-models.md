---
title: Verwalten von IoT Plug & Play Vorschaumodellen im Repository | Microsoft-Dokumentation
description: Erfahren Sie mehr über das Verwalten von Gerätefunktionsmodellen im Repository mithilfe des Azure Certified for IoT-Portals, der Azure CLI und Visual Studio Code.
author: YasinMSFT
manager: philmea
ms.service: iot-pnp
services: iot-pnp
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: yahajiza
ms.openlocfilehash: ec36bef9c2b65a5c88a52621bbf3a1db708c235c
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69879214"
---
# <a name="manage-models-in-the-repository"></a>Verwalten von Modellen im Repository

Das IoT Plug & Play-Vorschaumodellrepository speichert Gerätefunktionsmodelle und Schnittstellen. Durch das Repository können Lösungsentwickler die Modelle und Schnittstellen ermitteln und verwenden.

Es gibt drei Tools, die Sie zum Verwalten des Repositorys verwenden können:

- Das Azure Certified for IoT-Portal
- Die Azure-CLI
- Visual Studio Code

## <a name="model-repositories"></a>Modellrepositorys

Es gibt zwei Arten von Modellrepositorys zum Speichern von Gerätefunktionsmodellen und Schnittstellen:

- Es gibt ein einzelnes _öffentliches Repository_, in dem die Gerätefunktionsmodelle und Schnittstellen für Geräte im [Certified for IoT-Gerätekatalog](https://aka.ms/iotdevcat) gespeichert werden. Dieses Repository speichert auch [gängigen Schnittstellen](./concepts-common-interfaces.md) und [DCMs und von Microsoft-Partnern veröffentlichte Schnittstellen](./howto-onboard-portal.md). Um zu erfahren, wie Sie ein Gerät zertifizieren und sein Gerätefunktionsmodell zum öffentlichen Repository hinzufügen können, lesen Sie das Tutorial [Zertifizierung Ihres IoT Plug & Play-Geräts](./tutorial-certification-test.md).
- Es gibt mehrere _Unternehmensrepositorys_. Ein Unternehmensrepository wird automatisch beim [Onboarding zum Azure Certified for IoT-Portal](./howto-onboard-portal.md) für Ihre Organisation erstellt. Sie können das Unternehmensrepository verwenden, um Ihre Gerätefunktionsmodelle und Schnittstellen während der Entwicklung und der Tests zu speichern.

## <a name="azure-certified-for-iot-portal"></a>Azure Certified for IoT-Portal

Im [Azure Certified for IoT-Portal](https://preview.catalog.azureiotsolutions.com) können Sie die folgenden Aufgaben ausführen:

- [Schließen Sie den Zertifizierungsprozess für Ihr IoT-Gerät ab](./tutorial-certification-test.md).
- Suchen Sie nach IoT Plug & Play-Gerätefunktionsmodellen. Mithilfe dieser Modelle können Sie [im Handumdrehen IoT-fähige Geräte erstellen und in Lösungen integrieren](./quickstart-connect-pnp-device-solution.md).

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Die Azure CLI stellt Befehle zum Verwalten von Gerätefunktionsmodellen und Schnittstellen in den öffentlichen Modellrepositorys und den unternehmenseigenen Modellrepositorys in IoT Plug & Play bereit.

## <a name="visual-studio-code"></a>Visual Studio Code

So öffnen Sie die Ansicht **Modellrepository** in Visual Studio Code.

1. Öffnen Sie Visual Studio Code, verwenden Sie **STRG+UMSCHALT+P**, und wählen Sie **IoT Plug & Play: Modellrepository öffnen** aus.

1. Sie können zwischen zwei Optionen wählen: **Öffentliches Modellrepository öffnen** oder **Unternehmenseigenes Modellrepository öffnen**. Für das unternehmenseigene Modellrepository müssen Sie Ihre Verbindungszeichenfolge für das Modellrepository eingeben. Diese Verbindungszeichenfolge finden Sie im [Azure Certified for IoT-Portal](https://preview.catalog.azureiotsolutions.com) auf der Registerkarte **Verbindungszeichenfolgen** für Ihr **Unternehmensrepository**.

1. Die Ansicht **Modellrepository** wird auf einer neuen Registerkarte geöffnet.

    Über diese Ansicht können Sie Gerätefunktionsmodelle und Schnittstellen hinzufügen, herunterladen und löschen. Über Filter können Sie bestimmte Elemente in der Liste suchen.

1. Um zwischen dem unternehmenseigene Modellrepository und dem öffentlichen Modellrepository zu wechseln, drücken Sie **STRG+UMSCHALT+P**, und wählen Sie **IoT Plug & Play: Modellrepository abmelden** aus. Verwenden Sie dann erneut den Befehl **IoT Plug & Play: Modellrepository öffnen**.

> [!NOTE]
> In VS Code ist das öffentliche Modellrepository schreibgeschützt. Microsoft-Partner können das öffentliche Repository im [Azure Certified for IOT-Portal](https://preview.catalog.azureiotsolutions.com) aktualisieren.

## <a name="next-steps"></a>Nächste Schritte

Als Nächstes empfehlen wir, sich darüber zu informieren, wie Sie ein [IoT Plug & Play-Gerät zur Zertifizierung übermitteln](tutorial-certification-test.md).

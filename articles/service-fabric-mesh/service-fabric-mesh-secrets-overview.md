---
title: Speichern und Verwenden von Azure Service Fabric Mesh-Anwendungsgeheimnissen | Microsoft-Dokumentation
description: Speichern und Verwenden von Service Fabric Mesh-Geheimnissen
services: service-fabric-mesh
keywords: secrets
author: v-steg
ms.author: jeconnoc
ms.date: 10/25/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: jeconnoc
ms.openlocfilehash: b5cfa93298222d914069b6ab63deb8ba8a9b59c3
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2019
ms.locfileid: "67875349"
---
# <a name="service-fabric-mesh-application-secrets"></a>Service Fabric Mesh-Anwendungsgeheimnisse
Service Fabric Mesh unterstützt Geheimnisse als Azure-Ressourcen. Bei einem Service Fabric Mesh-Geheimnis kann es sich um beliebige sensible Informationen in Textform handeln, z.B. Speicherverbindungszeichenfolgen, Kennwörter oder andere Werte, die sicher gespeichert und übertragen werden sollten.

![Übersicht über Mesh-Geheimnisse][sf-mesh-secrets-overview]

## <a name="mesh-secrets-resources"></a>Mesh-Geheimnisressourcen
Ein Mesh-Anwendungsgeheimnis umfasst Folgendes:
* Eine Ressource **Geheimnisse**, bei der es sich um einen Container zum Speichern von Textgeheimnissen handelt. In der Ressource **Geheimnisse** enthaltene Geheimnisse werden auf sichere Weise gespeichert und übertragen.
* Mindestens eine Ressource vom Typ **Geheimnisse/Werte**, die im Ressourcencontainer **Geheimnisse** gespeichert ist. Jede Ressource **Geheimnisse/Werte** verfügt über eine Versionsnummer.

## <a name="next-steps"></a>Nächste Schritte 
Weitere Informationen zu Service Fabric Mesh-Geheimnissen finden Sie in den folgenden Artikeln:
- [Verwalten von Service Fabric Mesh-Anwendungsgeheimnissen](service-fabric-mesh-howto-manage-secrets.md)
- [Einführung in das Service Fabric-Ressourcenmodell](service-fabric-mesh-service-fabric-resources.md)

<!-- pics -->
[sf-mesh-secrets-overview]: ./media/service-fabric-mesh-secrets-overview/MeshAppSecretsOverview.png
